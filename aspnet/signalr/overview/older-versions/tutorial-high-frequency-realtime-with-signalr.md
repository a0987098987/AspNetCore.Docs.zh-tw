---
uid: signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
title: 高頻率即時與 SignalR 1.x |Microsoft Docs
author: bradygaster
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。 高頻率訊息處理中...
ms.author: bradyg
ms.date: 04/16/2013
ms.assetid: ad2a5da5-2e79-40ea-bc84-028d327f5982
msc.legacyurl: /signalr/overview/older-versions/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 6df35a420a0733003808a12d065b03f08ef56dd9
ms.sourcegitcommit: ebf4e5a7ca301af8494edf64f85d4a8deb61d641
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54837919"
---
<a name="high-frequency-realtime-with-signalr-1x"></a>高頻率即時與 SignalR 1.x
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

> 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。 在此情況下頻繁訊息表示會以固定費率; 的更新在此應用程式，最多 10 個訊息，第二個。
> 
> 您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。 圖形的位置，所有其他已連線的瀏覽器中就會更新以符合使用定時的更新拖曳圖形的位置。
> 
> 本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。
> 
> 在本教學課程的註解是歡迎畫面。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com)。


## <a name="overview"></a>總覽

本教學課程會示範如何建立與其他瀏覽器即時分享物件狀態的應用程式。 我們將建立的應用程式稱為 MoveShape。 MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，則其新位置將傳送到伺服器，然後就可以知道所有其他連線的用戶端更新以符合圖形的位置。

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

在本教學課程所建立的應用程式根據示範中，由 Damian Edwards 主講。 可以看到包含這段示範影片的影片[此處](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。

本教學課程會示範如何從每個引發拖曳圖形的事件傳送 SignalR 訊息啟動。 每個連線的用戶端會更新圖形的本機版本的位置收到訊息的每一次。

儘管應用程式將使用此方法來運作，這不是建議的程式設計模型，因為會開始傳送，因此用戶端和伺服器無法取得應付訊息，並會降低效能的訊息數目沒有上限. 脫離，圖形會立即移動每個方法，由，而不是移動順暢到每個新的位置，也會是在用戶端上顯示的動畫。 本教學課程後面幾節將示範如何建立計時器函式，將由用戶端或伺服器，傳送的訊息最大速率限制以及如何將圖案移動位置之間的順暢。 在本教學課程所建立的應用程式的最終版本可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。

本教學課程包含下列各節：

- [必要條件](#prerequisites)
- [建立專案](#createtheproject)
- [新增 ASP.NET SignalR 和 JQuery.UI NuGet 套件](#nugetpackages)
- [建立基底的應用程式](#baseapp)
- [新增用戶端迴圈](#clientloop)
- [新增伺服器迴圈](#serverloop)
- [用戶端上新增動畫更為順暢](#animation)
- [後續步驟](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

本教學課程需要 Visual Studio 2012 或 Visual Studio 2010。 如果使用 Visual Studio 2010，則專案會使用.NET Framework 4，而不是.NET Framework 4.5。

如果您使用 Visual Studio 2012，建議您安裝[ASP.NET 和 Web 工具 2012.2 更新](https://go.microsoft.com/fwlink/?LinkId=282650)。 此更新包含新功能，例如發行、 新功能的增強功能和新的範本。

如果您有 Visual Studio 2010，請確定[NuGet](https://visualstudiogallery.msdn.microsoft.com/27077b70-9dad-4c64-adcf-c7cf6bc9970c)安裝。

<a id="createtheproject"></a>

## <a name="create-the-project"></a>建立專案

在本節中，我們將建立 Visual Studio 中的專案。

1. 從**檔案**功能表上，按一下**新的專案**。
2. 在**新的專案**對話方塊方塊中，展開**C#** 之下**範本**，然後選取**Web**。
3. 選取  **ASP.NET 空白 Web 應用程式**範本，將專案命名為*MoveShapeDemo*，然後按一下**確定**。

    ![建立新的專案](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)

<a id="nugetpackages"></a>

## <a name="add-the-signalr-and-jqueryui-nuget-packages"></a>將 SignalR 和 JQuery.UI NuGet 套件

您可以加入至專案的 SignalR 功能，藉由安裝 NuGet 套件。 本教學課程也會使用 JQuery.UI 套件，可允許為了能夠拖放以動畫顯示圖形。

1. 按一下 **工具 |NuGet 套件管理員 |套件管理員主控台**。
2. 在 套件管理員中，輸入下列命令。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.ps1)]

    SignalR 套件會安裝一些其他 NuGet 套件作為相依項目。 安裝完成時可能會有所有的伺服器和用戶端所需的元件使用 SignalR 的 ASP.NET 應用程式中。
3. 套件管理員主控台安裝 JQuery 和 JQuery.UI 套件中輸入下列命令。

    [!code-powershell[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.ps1)]

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>建立基底的應用程式

在本節中，我們將建立每個滑鼠移動事件期間，將形狀的位置傳送至伺服器的瀏覽器應用程式。 伺服器然後廣播給所有其他連接的用戶端的這項資訊是在接收。 我們將探討在稍後的章節中的此應用程式。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別命名為**MoveShapeHub**然後按一下**新增**。
2. 在新程式碼取代**MoveShapeHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.cs)]

    `MoveShapeHub`上述類別會實作 SignalR hub。 依照[開始使用 SignalR](index.md)教學課程中，中樞都有在用戶端會直接呼叫的方法。 在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。 SignalR 將自動序列化此物件，使用 JSON。

    物件，將會傳送至用戶端 (`ShapeModel`) 包含要儲存圖形的位置的成員。 在伺服器上物件的版本也包含成員，才能追蹤用戶端的資料儲存，以便指定用戶端將不會傳送自己的資料。 這個成員會使用`JsonIgnore`以防止它被序列化並傳送至用戶端的屬性。
3. 接下來，我們會設定中樞的應用程式啟動時。 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |全域應用程式類別**。 接受預設名稱*Global*然後按一下**確定**。

    ![新增全域應用程式類別](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
4. 新增下列`using`之後所提供的陳述式**使用**Global.asax.cs 類別中的陳述式。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.cs)]
5. 加入下列行中的程式碼`Application_Start`要報名 SignalR 的預設路由的全域類別的方法。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    Global.asax 檔案看起來應該如下所示：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.cs)]
6. 接下來，我們將新增用戶端。 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。 在 **加入新項目**對話方塊中，選取**Html 網頁**。 指定頁面的適當名稱 (例如**Default.html**)，按一下 **新增**。
7. 在 **方案總管**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。
8. HTML 網頁中的預設程式碼取代為下列程式碼片段。

    > [!NOTE]
    > 確認指令碼參照以下相符項目新增至您的專案，在指令碼 資料夾中的封裝。 在 Visual Studio 2010 中，JQuery 和 SignalR 加入至專案的版本可能不符合以下的版本號碼。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample7.html)]

    上述的 HTML 和 JavaScript 程式碼會建立紅色的 Div 稱為圖形、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`圖形的位置傳送至伺服器的事件。
9. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>新增用戶端迴圈

由於傳送每一個滑鼠移動事件形狀的位置，會建立網路流量不需要數量，將訊息從用戶端就必須進行節流處理。 我們將使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。 這個迴圈是功能的 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他模擬非常基本表示法。

1. 更新用戶端中的程式碼的 HTML 網頁，以符合下列程式碼片段。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample8.html)]

    上述的更新新增了`updateServerModel`函式，都會呼叫固定的頻率。 此函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 取得傳送到伺服器的訊息數目會受到節流控制，因為動畫不會顯示為 smooth 上一節。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>新增伺服器迴圈

在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。 這會呈現類似的問題，因為出現在用戶端中;訊息可以傳送頻率比所需之連接如此一來可能會淹沒。 本節說明如何更新伺服器實作的計時器，針對外寄訊息的速率進行節流。

1. 內容取代`MoveShapeHub.cs`取代為下列程式碼片段。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample9.cs)]

    上述程式碼會展開要新增的用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。

    由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。 延遲初始設定 （.NET 4 中引入） 用來延遲其建立，直到有需要確保計時器啟動之前，完全建立第一個 「 中樞 」 執行個體。

    用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。 相反地，您會將用戶端訊息傳送速率為每秒 25 呼叫受`_broadcastLoop`從計時器`Broadcaster`類別。

    最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 不會從上一節中，瀏覽器中的可見差異，但會進行節流處理至用戶端傳送的訊息數目。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>用戶端上新增動畫更為順暢

應用程式幾乎已完成，但我們無法讓一個更多改進，圖形用戶端上的移動中移動以回應伺服器的訊息時。 而不是給定伺服器的新位置來設定圖案的位置，我們會使用 JQuery UI 程式庫的`animate`順暢移動圖形，其目前的和新的位置之間的函式。

1. 更新用戶端`updateShape`方法看起來像下列反白顯示的程式碼：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample10.html?highlight=35-42)]

    上述程式碼會將圖形從舊位置移到新伺服器所指定的期間 （在本例中為 100 毫秒） 的動畫間隔。 新動畫開始之前，會清除任何在圖形上執行的上一個動畫。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 移動的另一個視窗中的圖形應該會出現較不為移轉的時間，而不是一次設定每個內送訊息會以內插值取代其移動不穩定。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>後續步驟

在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將用戶端與伺服器之間的頻率高的訊息傳送方式。 此通訊架構可用於開發線上遊戲，以及其他的模擬，例如[ShootR 遊戲建立與 SignalR](http://shootr.signalr.net)。

在本教學課程中建立完整的應用程式可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-MoveShape-demo-3366dac6)。

若要深入了解 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
