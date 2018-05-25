---
uid: signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
title: 教學課程： 與 SignalR 2 高頻率即時 |Microsoft 文件
author: pfletcher
description: 本教學課程會示範如何建立 web 應用程式使用 ASP.NET SignalR 提供高頻率的傳訊功能。 頻率高的內送訊息...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: 9f969dda-78ea-4329-b1e3-e51c02210a2b
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-high-frequency-realtime-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ab051b2ab85d1aac1e7179f342f22f470b1d1cc7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-high-frequency-realtime-with-signalr-2"></a>教學課程： 高頻率即時與 SignalR 2
====================
由[Patrick Fletcher](https://github.com/pfletcher)

[下載完成的專案](http://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)

> 本教學課程會示範如何建立使用 ASP.NET SignalR 2 以提供高頻率的傳訊功能的 web 應用程式。 在此情況下高頻率訊息表示傳送以固定費率; 的更新如果這個應用程式，最多 10 個第二個訊息。
> 
> 您會在本教學課程中建立應用程式會顯示使用者可以拖曳圖形。 所有其他連接的瀏覽器中圖形的位置就會更新以符合使用定時的更新 「 拖曳 」 圖形的位置。
> 
> 此教學課程中介紹的概念有即時遊戲中的應用程式和其他模擬應用程式。
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
> ## <a name="using-visual-studio-2012-with-this-tutorial"></a>使用 Visual Studio 2012 進行這個教學課程
> 
> 
> 透過本教學課程中使用 Visual Studio 2012，請執行下列作業：
> 
> - 更新您[封裝管理員](http://docs.nuget.org/docs/start-here/installing-nuget)的最新版本。
> - 安裝[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)。
> - 在 Web Platform Installer 中搜尋及安裝**ASP.NET 和 Web 工具 2013.1 for Visual Studio 2012**。 這會安裝 Visual Studio 範本 SignalR 的類別，例如**中樞**。
> - 某些範本 (例如**OWIN 啟動類別**) 將無法使用; 這些項目，請改用類別檔案。
> 
> 
> ## <a name="tutorial-versions"></a>教學課程版本
> 
> 如需舊版 SignalR 的資訊，請參閱[SignalR 舊版](../older-versions/index.md)。
> 
> ## <a name="questions-and-comments"></a>問題和註解
> 
> 請留下上如何您所喜歡的本教學課程，我們可以改進中將註解放在頁面底部的意見反應。 如果您有與本教學課程不直接相關的問題，您可以將它們來公佈[ASP.NET SignalR 論壇](https://forums.asp.net/1254.aspx/1?ASP+NET+SignalR)或[StackOverflow.com](http://stackoverflow.com/)。


## <a name="overview"></a>總覽

本教學課程會示範如何建立共用物件的狀態與即時其他瀏覽器的應用程式。 我們將建立的應用程式會呼叫 MoveShape。 MoveShape 頁面會顯示使用者可以拖曳; HTML Div 項目當使用者拖曳 Div，新的位置將傳送到伺服器，就會通知所有其他連線的用戶端更新以符合圖形的位置。

![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image1.png)

在本教學課程所建立的應用程式根據由 Damian Edwards 示範。 包含此示範影片可以看到[這裡](https://channel9.msdn.com/Series/Building-Web-Apps-with-ASP-NET-Jump-Start/Building-Web-Apps-with-ASPNET-Jump-Start-08-Real-time-Communication-with-SignalR)。

開始本教學課程會示範如何從每個引發之事件的圖形拖曳時傳送 SignalR 訊息。 每個連線的用戶端會更新位置的本機版本 」 圖形的每次收到一則訊息。

應用程式將使用此方法來運作，而這不建議使用的程式設計模型，因為有可能取得傳送，因此用戶端與伺服器無法取得爆訊息，並會降低效能的訊息數目沒有上限. 脫離，圖形會立即移動的每個方法，而非移動到每個新的位置順暢地也是在用戶端上顯示的動畫。 本教學課程稍後的章節將示範如何建立會限制用戶端或伺服器所傳送訊息的最大速率計時器函式，以及如何順暢移動位置之間的圖形。 在本教學課程所建立的應用程式的最終版本可以從下載[Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

本教學課程包含下列各節：

- [必要條件](#prerequisites)
- [建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝](#createtheproject2013)
- [建立基底應用程式](#baseapp)
- [應用程式啟動時啟動中樞](#startup2013)
- [新增用戶端迴圈](#clientloop)
- [新增伺服器迴圈](#serverloop)
- [用戶端上新增 smooth 動畫](#animation)
- [進一步的步驟](#furthersteps)

<a id="prerequisites"></a>

## <a name="prerequisites"></a>必要條件

本教學課程需要 Visual Studio 2013。

<a id="createtheproject2013"></a>

## <a name="create-the-project-and-add-the-signalr-and-jqueryui-nuget-package"></a>建立專案並加入 SignalR 和 JQuery.UI NuGet 封裝

在本節中，我們將建立 Visual Studio 2013 中的專案。

下列步驟使用 Visual Studio 2013 建立的 ASP.NET 空 Web 應用程式，並新增 SignalR 和 jQuery.UI 文件庫：

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-high-frequency-realtime-with-signalr/_static/image2.png)
2. 在**新增 ASP.NET 專案** 視窗中，保留**空**選取並按**建立專案**。

    ![建立空的網站](tutorial-high-frequency-realtime-with-signalr/_static/image3.png)
3. 在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |SignalR 中樞類別 (v2)**。 將類別**MoveShapeHub.cs**並將它加入至專案。 這個步驟會建立**MoveShapeHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。

    > [!NOTE]
    > 您也可以加入至專案 SignalR，依序按一下**工具 |程式庫套件管理員 |Package Manager Console**並執行命令：

    `install-package Microsoft.AspNet.SignalR`. 

    如果您要加入 SignalR 使用主控台，建立 SignalR 中樞類別當做個別的步驟之後新增 SignalR。
4. 按一下**工具 |程式庫套件管理員 |Package Manager Console**。 在 [封裝管理員] 視窗中，執行下列命令：

    `Install-Package jQuery.UI.Combined`

    這會安裝 jQuery UI 程式庫，您將使用動畫顯示圖案。
5. 在**方案總管 中**展開指令碼 節點。 JQuery、 jQueryUI，和 SignalR 的指令碼程式庫會顯示專案中。

    ![指令碼程式庫參考](tutorial-high-frequency-realtime-with-signalr/_static/image4.png)

<a id="baseapp"></a>

## <a name="create-the-base-application"></a>建立基底應用程式

在本節中，我們將建立每個滑鼠移動事件期間，將圖形的位置傳送到伺服器的瀏覽器應用程式。 伺服器再廣播給所有其他連接的用戶端的這項資訊在接收時。 我們將在稍後的章節中的這個應用程式上展開。

1. 如果您尚未建立 MoveShapeHub.cs 類別中**方案總管 中**，以滑鼠右鍵按一下專案，然後選取**新增**，**類別...**.將類別**MoveShapeHub**按一下**新增**。
2. 在新的程式碼取代**MoveShapeHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample1.cs)]

    `MoveShapeHub`上述類別是 SignalR 中樞的實作。 在[入門 SignalR](tutorial-getting-started-with-signalr.md)教學課程中，中樞包含用戶端會直接呼叫的方法。 在此情況下，用戶端會傳送物件，其中包含新的伺服器，然後取得傳播到所有其他連線的用戶端 圖形的 X 和 Y 座標。 SignalR 將自動序列化這個物件使用 JSON。

    將傳送至用戶端的物件 (`ShapeModel`) 包含要儲存的位置圖形的成員。 在伺服器上之物件的版本也包含成員，才能追蹤用戶端的資料儲存，，以便他們自己的資料將不會傳送指定的用戶端。 這個成員會使用`JsonIgnore`屬性，以防止被序列化並傳送至用戶端。

<a id="startup2013"></a>
## <a name="starting-the-hub-when-the-application-starts"></a>應用程式啟動時啟動中樞

1. 接下來，我們會設定對應至中樞應用程式啟動時。 在 SignalR 2 中，這是藉由新增 OWIN 啟動類別，會呼叫`MapSignalR`時啟動類別`Configuration`OWIN 啟動時執行方法。 類別會加入至 OWIN 的啟動處理使用`OwinStartup`組件屬性。

    在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |OWIN 啟動類別**。 將類別*啟動*按一下**確定**。
2. 將 Startup.cs 中的內容變更為下列：

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample2.cs)]

<a id="client"></a>
## <a name="adding-the-client"></a>新增用戶端

1. 接下來，我們要加入用戶端。 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。 在**加入新項目**對話方塊中，選取**Html 網頁**。 將頁面命名**Default.html**按一下**新增**。
2. 在**方案總管 中**，以滑鼠右鍵按一下您剛建立的網頁，然後按一下**設定為起始頁**。
3. HTML 網頁中的預設程式碼取代為下列程式碼片段。

    > [!NOTE]
    > 確認指令碼參考以下相符項目新增至指令碼 資料夾中的專案的套件。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample3.html)]

    上述的 HTML 和 JavaScript 程式碼會建立稱為圖形紅色 Div、 啟用圖形的拖曳行為使用 jQuery 程式庫，並使用圖形的`drag`事件傳送至伺服器的圖形的位置。
4. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image5.png)

<a id="clientloop"></a>

## <a name="add-the-client-loop"></a>新增用戶端迴圈

由於傳送上每個滑鼠移動事件圖形的位置將會建立不需要網路傳輸量，從用戶端的訊息必須進行節流處理。 我們會使用 javascript`setInterval`函式來設定新的位置資訊傳送到伺服器時以固定費率的迴圈。 這個迴圈是 「 遊戲迴圈 」，重複呼叫的函式的磁碟機的所有遊戲或其他的模擬功能非常基本表示法。

1. 更新以符合下列程式碼片段的 HTML 網頁上的用戶端程式碼。

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample4.html)]

    上述的更新將新增`updateServerModel`函式，以取得在固定頻率上呼叫。 此函式會將位置資料傳送到伺服器時`moved`旗標，表示沒有要傳送的新位置資料。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。 取得傳送到伺服器的訊息數目進行節流處理，因為動畫不會出現能順利，如前一節所示。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image6.png)

<a id="serverloop"></a>

## <a name="add-the-server-loop"></a>新增伺服器迴圈

在目前的應用程式，從伺服器傳送至用戶端的訊息移通常會將收到。 這顯現出驗證類似的問題已在用戶端所示超過所需，而且連接可能會變得湧入因此通常可以傳送訊息。 本節說明如何更新伺服器實作的計時器，節流處理外寄訊息的速率。

1. 取代內容`MoveShapeHub.cs`藉由下列程式碼片段。

    [!code-csharp[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample5.cs)]

    上述程式碼會展開，以新增用戶端`Broadcaster`類別，節流處理外寄訊息使用`Timer`從.NET framework 類別。

    由於中樞本身是暫時性 （它會建立每次需要它），`Broadcaster`會建立為單一值。 延遲初始設定 （.NET 4 中引進） 用來延遲其建立之前，確保在計時器啟動之前，完全建立第一個中樞執行個體。

    用戶端的呼叫`UpdateShape`函式接著會移出的中樞`UpdateModel`方法，所以它不再接收內送訊息時，立即呼叫。 相反地，將傳送給用戶端的訊息速率為 25 秒的呼叫受`_broadcastLoop`計時器從`Broadcaster`類別。

    最後，而不是直接將用戶端方法呼叫從中樞`Broadcaster`類別需要取得目前作業的中樞的參考 (`_hubContext`) 使用`GlobalHost`。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。 不會從上一節中，瀏覽器中看到的差別，但是會被調整到用戶端傳送的訊息數目。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image7.png)

<a id="animation"></a>

## <a name="add-smooth-animation-on-the-client"></a>用戶端上新增 smooth 動畫

應用程式幾乎完成，但我們無法讓一個詳細改進，圖形用戶端上的影片中移動以回應伺服器訊息。 而不是將圖形的位置設定為給定伺服器的新位置中，我們將使用 JQuery UI 程式庫的`animate`順暢移動圖形，它目前和新的位置之間的函式。

1. 更新用戶端`updateShape`方法，以便尋找類似以下的反白顯示程式碼：

    [!code-html[Main](tutorial-high-frequency-realtime-with-signalr/samples/sample6.html?highlight=33-40)]

    上述程式碼會將圖形從舊位置移到新伺服器所提供的動畫間隔 （此案例中是 100 毫秒） 的過程。 新的動畫開始之前，會清除在圖形上執行任何先前動畫。
2. 按 f5 鍵啟動應用程式。 複製頁面的 URL，並將它貼到第二個瀏覽器視窗。 將圖形拖曳其中一種瀏覽器視窗中：其他瀏覽器視窗中的圖形應該一併移動。 其他視窗中的圖形的移動應該會出現較不為一段時間，而非一次設定每個內送訊息會以內插值取代其移動不穩定。

    ![應用程式視窗](tutorial-high-frequency-realtime-with-signalr/_static/image8.png)

<a id="furthersteps"></a>

## <a name="further-steps"></a>進一步的步驟

在本教學課程中，您學到如何進行用戶端與伺服器之間的頻率高的訊息將傳送的 SignalR 應用程式。 此通訊範例可用於開發線上遊戲和其他模擬，例如[ShootR 遊戲建立與 SignalR](http://shootr.signalr.net)。

完成本教學課程建立的應用程式可以從下載[Code Gallery](https://code.msdn.microsoft.com/SignalR-20-MoveShape-Demo-6285b83a)。

若要了解有關 SignalR 開發概念的詳細資訊，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

如需如何部署至 Azure 的 SignalR 應用程式的逐步解說，請參閱[與 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。 如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。
