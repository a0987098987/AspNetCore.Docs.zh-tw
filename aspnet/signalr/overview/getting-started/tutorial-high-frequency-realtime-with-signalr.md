---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 教學課程： 高頻率即時與 SignalR 2 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 提供高頻率的傳訊功能。 高頻率訊息處理中...
ms.author: aspnetcontent
ms.date: 06/10/2014
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 14ec1c8b4c034332afd8d3102a310fd3fb399d32
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37829459"
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>教學課程： 高頻率即時與 SignalR 2
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[下載已完成的專案](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> 本教學課程會示範如何建立 web 應用程式，使用 ASP.NET SignalR 2 提供高頻率的傳訊功能。 在此情況下頻繁訊息表示會以固定費率; 的更新在此應用程式，最多 10 個訊息，第二個。
> 
> 您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。 圖形的位置，所有其他已連線的瀏覽器中就會更新以符合使用定時的更新拖曳圖形的位置。
> 
> 本教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - [Visual Studio 2013](https://www.microsoft.com/visualstudio/eng/2013-downloads)
> - .NET 4.5
> - SignalR 第 2 版
>   
> 
> 
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>本教學課程中使用 Visual Studio 2012
> 
> 
> 若要使用 Visual Studio 2012，本教學課程中，執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)為最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中，搜尋並安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 類別，例如**中樞**。
> - 有些範本 (例如**OWIN 啟動類別**) 將無法使用，這些項目，請改用類別檔案。
> 
> 
> ## <a name="tutorial-versions"></a>教學課程的版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>提出問題或意見
> 
> 您喜歡本教學課程中的方式，和我們可以改善在頁面底部的註解中，歡迎留下意見反應。 如果您有不直接相關的教學課程中的問題，您可以張貼他們[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或是[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

本教學課程會示範如何建立與其他瀏覽器即時分享物件狀態的應用程式。 我們將建立的應用程式稱為 MoveShape。 MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，則其新位置將傳送到伺服器，然後就可以知道所有其他連線的用戶端更新以符合圖形的位置。

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

在本教學課程所建立的應用程式根據示範中，由 Damian Edwards 主講。 可以看到包含這段示範影片的影片[此處](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。

本教學課程會示範如何從每個引發拖曳圖形的事件傳送 SignalR 訊息啟動。 每個連線的用戶端會更新圖形的本機版本的位置收到訊息的每一次。

儘管應用程式將使用此方法來運作，這不是建議的程式設計模型，因為會開始傳送，因此用戶端和伺服器無法取得應付訊息，並會降低效能的訊息數目沒有上限. 脫離，圖形會立即移動每個方法，由，而不是移動順暢到每個新的位置，也會是在用戶端上顯示的動畫。 本教學課程後面幾節將示範如何建立計時器函式，將由用戶端或伺服器，傳送的訊息最大速率限制以及如何將圖案移動位置之間的順暢。 在本教學課程所建立的應用程式的最終版本可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

本教學課程包含下列各節：

- [必要條件](#prerequisites)
- [建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝](#createtheproject2013)
- [建立基底的應用程式](#baseapp)
- [應用程式啟動時啟動的中樞](#startup2013)
- [新增用戶端迴圈](#clientloop)
- [新增伺服器迴圈](#serverloop)
- [用戶端上新增動畫更為順暢](#animation)
- [後續步驟](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

本教學課程需要 Visual Studio 2013。

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝

在本節中，我們將建立 Visual Studio 2013 中的專案。

下列步驟使用 Visual Studio 2013 建立 ASP.NET 空白 Web 應用程式並將新增之 SignalR 和 jQuery.UI 程式庫：

1. 在 Visual Studio 建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. 在 **新增 ASP.NET 專案** 視窗中，保持**空白**選取，然後按一下 **建立專案**。

    ![建立空白網站](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |SignalR Hub 類別 (v2)**。 將類別命名為**MoveShapeHub.cs**並將它新增至專案。 這個步驟會建立**MoveShapeHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。

    > [!NOTE]
    > 您也可以新增至專案 SignalR，依序按一下**工具 |程式庫套件管理員 |套件管理員主控台**並執行命令：

    `install-package Microsoft.AspNet.SignalR`. 

    如果您可以使用主控台來新增 SignalR，建立 SignalR hub 類別做為個別的步驟之後新增 SignalR。
4. 按一下 **工具 |程式庫套件管理員 |套件管理員主控台**。 在 [套件管理員] 視窗中，執行下列命令：

    `Install-Package jQuery.UI.Combined`

    這會安裝 jQuery UI 程式庫，您將用來製作圖案動畫。
5. 在 **方案總管 中**展開指令碼 節點。 適用於 jQuery、 jQueryUI 及 SignalR 的指令碼程式庫會顯示在專案中。

    ![指令碼程式庫參考](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>建立基底的應用程式

在本節中，我們將建立每個滑鼠移動事件期間，將形狀的位置傳送至伺服器的瀏覽器應用程式。 伺服器然後廣播給所有其他連接的用戶端的這項資訊是在接收。 我們將探討在稍後的章節中的此應用程式。

1. 如果您尚未建立 MoveShapeHub.cs 類別中**方案總管**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別命名為**MoveShapeHub**然後按一下**新增**。
2. 在新程式碼取代**MoveShapeHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub`上述類別會實作 SignalR hub。 依照[開始使用 SignalR](tutorial-getting-started-with-signalr.md)教學課程中，中樞都有在用戶端會直接呼叫的方法。 在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。 SignalR 將自動序列化此物件，使用 JSON。

    物件，將會傳送至用戶端 (`ShapeModel`) 包含要儲存圖形的位置的成員。 在伺服器上物件的版本也包含成員，才能追蹤用戶端的資料儲存，以便指定用戶端將不會傳送自己的資料。 這個成員會使用`JsonIgnore`以防止它被序列化並傳送至用戶端的屬性。

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>應用程式啟動時啟動的中樞

1. 接下來，我們會設定對應至中樞應用程式啟動時。 在 SignalR 2 中，這是藉由新增 OWIN 啟動類別，稱之為`MapSignalR`時的啟動類別`Configuration`OWIN 啟動時，執行方法。 OWIN 的啟動類別加入使用`OwinStartup`組件屬性。

    在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |OWIN 啟動類別**。 將類別命名為*啟始*，按一下 **確定**。
2. 變更 Startup.cs 的內容所示：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>新增用戶端

1. 接下來，我們將新增用戶端。 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。 在 **加入新項目**對話方塊中，選取**Html 網頁**。 將頁面命名**Default.html**然後按一下**新增**。
2. 在 **方案總管**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。
3. HTML 網頁中的預設程式碼取代為下列程式碼片段。

    > [!NOTE]
    > 確認指令碼參照以下相符項目新增至您的專案，在指令碼 資料夾中的封裝。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    上述的 HTML 和 JavaScript 程式碼會建立紅色的 Div 稱為圖形、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`圖形的位置傳送至伺服器的事件。
4. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>新增用戶端迴圈

由於傳送每一個滑鼠移動事件形狀的位置，會建立網路流量不需要數量，將訊息從用戶端就必須進行節流處理。 我們將使用 javascript`setInterval`函式來設定迴圈，將新的位置資訊傳送到伺服器以固定費率。 這個迴圈是功能的 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他模擬非常基本表示法。

1. 更新用戶端中的程式碼的 HTML 網頁，以符合下列程式碼片段。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    上述的更新新增了`updateServerModel`函式，都會呼叫固定的頻率。 此函式會將位置資料傳送到伺服器時`moved`旗標指出是要傳送的新位置資料。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 取得傳送到伺服器的訊息數目會受到節流控制，因為動畫不會顯示為 smooth 上一節。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>新增伺服器迴圈

在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。 這會呈現類似的問題，因為出現在用戶端中;訊息可以傳送頻率比所需之連接如此一來可能會淹沒。 本節說明如何更新伺服器實作的計時器，針對外寄訊息的速率進行節流。

1. 內容取代`MoveShapeHub.cs`取代為下列程式碼片段。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    上述程式碼會展開要新增的用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。

    由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。 延遲初始設定 （.NET 4 中引入） 用來延遲其建立，直到有需要確保計時器啟動之前，完全建立第一個 「 中樞 」 執行個體。

    用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。 相反地，您會將用戶端訊息傳送速率為每秒 25 呼叫受`_broadcastLoop`從計時器`Broadcaster`類別。

    最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 不會從上一節中，瀏覽器中的可見差異，但會進行節流處理至用戶端傳送的訊息數目。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>用戶端上新增動畫更為順暢

應用程式幾乎已完成，但我們無法讓一個更多改進，圖形用戶端上的移動中移動以回應伺服器的訊息時。 而不是給定伺服器的新位置來設定圖案的位置，我們會使用 JQuery UI 程式庫的`animate`順暢移動圖形，其目前的和新的位置之間的函式。

1. 更新用戶端`updateShape`方法看起來像下列反白顯示的程式碼：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    上述程式碼會將圖形從舊位置移到新伺服器所指定的期間 （在本例中為 100 毫秒） 的動畫間隔。 新動畫開始之前，會清除任何在圖形上執行的上一個動畫。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一個瀏覽器視窗中：其他瀏覽器視窗內圖案應該一併移動。 移動的另一個視窗中的圖形應該會出現較不為移轉的時間，而不是一次設定每個內送訊息會以內插值取代其移動不穩定。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>後續步驟

在本教學課程中，您已了解如何以程式設計的 SignalR 應用程式，將用戶端與伺服器之間的頻率高的訊息傳送方式。 此通訊架構可用於開發線上遊戲，以及其他的模擬，例如[ShootR 遊戲建立與 SignalR](http://shootr.signalr.net)。

在本教學課程中建立完整的應用程式可以從下載[程式碼庫](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

若要深入了解 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

如需如何部署至 Azure 的 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 使用 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。 如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。
