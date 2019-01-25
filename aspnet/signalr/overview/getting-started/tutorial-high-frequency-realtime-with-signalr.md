---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 教學課程：使用 SignalR 2 建立高頻率即時應用程式 |Microsoft Docs
author: bradygaster
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。
ms.author: bradyg
ms.date: 01/22/2019
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: 44aaa2b0c059de310e963f642fa56c2f00a7e443
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54836723"
---
# <a name="tutorial-create-high-frequency-real-time-app-with-signalr-2"></a>教學課程：使用 SignalR 2 建立高頻率即時應用程式

本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 2 提供高頻率的傳訊功能。 在此情況下，「 高頻率傳訊 」，表示伺服器會以固定費率來傳送更新。 您傳送第二個最多 10 個訊息。

您所建立的應用程式會顯示使用者可以拖曳圖形。 伺服器會更新所有連線的瀏覽器，以符合使用定時的更新拖曳圖形的位置中圖形的位置。

本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。

在本教學課程中，您已：

> [!div class="checklist"]
> * 設定專案
> * 建立基底的應用程式
> * 應用程式啟動時，對應到中樞
> * 新增用戶端
> * 執行應用程式
> * 新增用戶端迴圈
> * 新增伺服器迴圈
> * 新增動畫更為順暢

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必要條件

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。

## <a name="set-up-the-project"></a>設定專案

在本節中，您可以建立 Visual Studio 2017 中的專案。

本節說明如何使用 Visual Studio 2017 來建立空白的 ASP.NET Web 應用程式並新增之 SignalR 和 jQuery.UI 程式庫。

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

1. 在 [**新增 ASP.NET Web 應用程式-MoveShapeDemo** ] 視窗中，保持**空白**，並選取 **[確定]**。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-MoveShapeDemo**，選取**已安裝** > **Visual C#**   >  **Web**  > **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。

1. 將類別命名為*MoveShapeHub*並將它新增至專案。

    這個步驟會建立*MoveShapeHub.cs*類別檔案。 同時，它會新增一組指令碼檔案和專案支援 SignalR 的組件參考。

1. 選取 **工具** > **NuGet 套件管理員** > **Package Manager Console**。

1. 在  **Package Manager Console**，執行下列命令：

    ```console
    Install-Package jQuery.UI.Combined
    ```

    此命令會安裝 jQuery UI 程式庫。 您可以使用它來製作圖案動畫。

1. 在 [**方案總管] 中**，展開 [指令碼] 節點。

    ![指令碼程式庫參考](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

    適用於 jQuery、 jQueryUI 及 SignalR 的指令碼程式庫會顯示在專案中。

## <a name="create-the-base-application"></a>建立基底的應用程式

在本節中，您可以建立瀏覽器應用程式。 應用程式會每個滑鼠移動事件期間，將形狀的位置傳送至伺服器。 伺服器會廣播到所有其他連線的用戶端即時的這項資訊。 您進一步了解此應用程式在稍後的章節。

1. 開啟*MoveShapeHub.cs*檔案。

1. 中的程式碼取代*MoveShapeHub.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

1. 儲存檔案。

`MoveShapeHub`類別是實作 SignalR hub。 依照[開始使用 SignalR](tutorial-getting-started-with-signalr.md)教學課程中，中樞都有在用戶端直接呼叫的方法。 在此情況下，於物件，使用新的 X 和 Y 座標的圖形，以伺服器的用戶端傳送。 這些座標取得傳播到所有其他連線的用戶端。 SignalR 會自動將序列化此物件，使用 JSON。

應用程式傳送`ShapeModel`給用戶端的物件。 它有成員，以儲存圖形的位置。 在伺服器上物件的版本也有成員，才能追蹤用戶端的資料儲存。 此物件會防止伺服器將用戶端的資料傳送至本身。 這個成員會使用`JsonIgnore`屬性將序列化資料，將它傳送回用戶端應用程式。

## <a name="map-to-the-hub-when-app-starts"></a>應用程式啟動時，對應到中樞

接下來，您對應至中樞時設定應用程式啟動。 SignalR 2 中新增 OWIN 啟動類別，將會建立對應。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-MoveShapeDemo**選取**已安裝** > **Visual C#**   >  **Web** ，然後選取  **OWIN 啟動類別**。

1. 將類別命名為*啟始*，然後選取**確定**。

1. 取代預設的程式碼中*Startup.cs*這段程式碼檔案：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

OWIN 啟動類別呼叫`MapSignalR`應用程式在執行時`Configuration`方法。 應用程式新增至 OWIN 的啟動類別處理使用`OwinStartup`組件屬性。

## <a name="add-the-client"></a>新增用戶端

加入 HTML 網頁用戶端。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **HTML 網頁**。

1. 將頁面命名**預設**，然後選取**確定**。

1. 在 [**方案總管] 中**，以滑鼠右鍵按一下*Default.html* ，然後選取**設定為起始頁**。

1. 取代預設的程式碼中*Default.html*這段程式碼檔案：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html?highlight=14-16)]

1. 在 **方案總管**，展開**指令碼**。

    JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。

    > [!IMPORTANT]
    > 套件管理員安裝新版 SignalR 指令碼。

1. 更新程式碼區塊，以對應至專案中的指令碼檔案的版本中的指令碼參考。

此 HTML 和 JavaScript 程式碼會建立紅色`div`稱為`shape`。 它可讓使用 jQuery 程式庫的圖形的拖曳行為，並使用`drag`圖形的位置傳送至伺服器的事件。

## <a name="run-the-app"></a>執行應用程式

您可以執行應用程式到 se'e 它運作。 當您瀏覽器視窗周圍拖曳圖形時，圖案會移動其他瀏覽器中太。

1. 在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行應用程式的 播放 按鈕。

    ![使用者開啟偵錯模式，然後選取 [播放] 的螢幕擷取畫面。](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)

    瀏覽器視窗隨即開啟的右上角的紅色圖形。

1. 複製頁面的 URL。

1. 開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。

1. 其中一種瀏覽器視窗中拖曳圖形。 遵循其他瀏覽器視窗中的圖形。

雖然應用程式使用這個方法的函式，它不是建議的程式設計模型。 取得傳送的訊息數目沒有上限。 如此一來，用戶端和伺服器無法取得應付訊息，並降低效能。 此外，應用程式會顯示在用戶端上脫離的動畫。 因為，圖形會在每個方法所立即移，則會發生此不穩定的動畫。 最好是如果，圖形會順暢地移至每個新的位置。 接下來，您會了解如何修正這些問題。

## <a name="add-the-client-loop"></a>新增用戶端迴圈

傳送每一個滑鼠移動事件形狀的位置，會建立不必要的網路傳輸量。 應用程式必須進行節流處理來自用戶端的訊息。

使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。 這個迴圈是基本的表示法的 「 遊戲迴圈 」。 它會重複呼叫的函式的磁碟機的遊戲的所有功能。

1. 中的用戶端程式碼取代*Default.html*這段程式碼檔案：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html?highlight=14-16)]

    > [!IMPORTANT]
    > 您必須取代指令碼參考一次。 它們必須符合專案中的指令碼的版本。

    這個新的程式碼加入`updateServerModel`函式。 它會呼叫在固定的頻率。 函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。

1. 選取 [播放] 按鈕，啟動應用程式

1. 複製頁面的 URL。

1. 開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。

1. 其中一種瀏覽器視窗中拖曳圖形。 遵循其他瀏覽器視窗中的圖形。

因為應用程式節流處理的訊息傳送到伺服器時，動畫不會顯示為 smooth 數目未在第一個。

## <a name="add-the-server-loop"></a>新增伺服器迴圈

在目前的應用程式，從伺服器傳送至用戶端的訊息移通常在收到。 如我們在用戶端上所見，此網路流量會提供類似的問題。

應用程式可以比在需要更常傳送訊息。 如此一來連接可能會淹沒。 本節說明如何更新伺服器，以加入計時器，針對外寄訊息的速率進行節流。

1. 內容取代`MoveShapeHub.cs`以下列程式碼：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

1. 選取 啟動應用程式的 播放 按鈕。

1. 複製頁面的 URL。

1. 開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。

1. 其中一種瀏覽器視窗中拖曳圖形。

此程式碼會展開要新增的用戶端`Broadcaster`類別。 新的類別節流處理外寄訊息使用`Timer`從.NET framework 類別。

您最好了解中樞本身是暫時性。 它會建立每次需要它。 因此，應用程式會建立`Broadcaster`為 singleton。 延遲的情況下，它在使用延遲初始設定`Broadcaster`的建立，直到需要為止。 如此一來，可保證應用程式完全建立第一個 「 中樞 」 執行個體，再開始計時器。

用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法。 它不會再呼叫應用程式接收內送訊息時，立即。 相反地，應用程式會將訊息傳送速率為每秒 25 呼叫用戶端。 此程序由管理`_broadcastLoop`從計時器`Broadcaster`類別。

最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的參考`_hubContext`中樞。 它會取得與參考`GlobalHost`。

## <a name="add-smooth-animation"></a>新增動畫更為順暢

應用程式幾乎已完成，但我們無法讓多個的其中一項改進。 應用程式移動以回應伺服器訊息用戶端上的圖案。 而不是給定伺服器的新位置來設定圖案的位置，使用 JQuery UI 程式庫的`animate`函式。 它可以將圖案移動其目前的和新的位置之間的順暢。

1. 更新用戶端`updateShape`方法中的*Default.html*檔案看起來像是醒目提示的程式碼：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

1. 選取 啟動應用程式的 播放 按鈕。

1. 複製頁面的 URL。

1. 開啟另一個瀏覽器，並將 URL 貼到 [網址] 列。

1. 其中一種瀏覽器視窗中拖曳圖形。

移動的另一個視窗中的圖形會顯示較不穩定。 應用程式進行插補其移動移轉時間，而不是每個內送訊息一次設定。

此程式碼會將圖形從舊位置移至新的。 伺服器會提供形狀的位置的動畫時間間隔期間。 在此情況下，這會是 100 毫秒。 應用程式會清除任何新的動畫開始之前，在圖形上執行的上一個動畫。

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

## <a name="additional-resources"></a>其他資源

只是您已了解的通訊架構可用於開發線上遊戲，以及其他的模擬，像是[ShootR 遊戲建立與 SignalR](https://shootr.azurewebsites.net/)。

如需 SignalR 相關資訊，請參閱下列資源：

* [SignalR 專案](http://signalr.net)

* [SignalR GitHub 和範例](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 設定專案
> * 建立基本的應用程式
> * 應用程式啟動時，對應到中樞
> * 新增用戶端
> * 執行應用程式
> * 新增用戶端迴圈
> * 新增伺服器迴圈
> * 已新增的動畫更為順暢

請前往下一篇文章，以了解如何建立會使用 ASP.NET SignalR 2 提供伺服器廣播的功能的 web 應用程式。
> [!div class="nextstepaction"]
> [SignalR 2 及伺服器廣播](tutorial-server-broadcast-with-signalr.md)