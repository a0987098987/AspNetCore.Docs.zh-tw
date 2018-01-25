---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: "教學課程： 開始使用 SignalR 2 |Microsoft 文件"
author: pfletcher
description: "此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 您會加入空白的 ASP.NET web 應用程式中的 SignalR 並建立 HTML pa..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/10/2014
ms.topic: article
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 8be851f5a2b1cca39f5f8f284ff1c002c486d7e8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-2"></a>教學課程： 開始使用 SignalR 2
====================
由[Patrick Fletcher](https://github.com/pfletcher)

[下載完成的專案](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 將 SignalR 加入空的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。 
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

此教學課程介紹 SignalR 開發，以顯示如何建立簡單的瀏覽器為基礎的交談應用程式。 您會加入空的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收交談的訊息。 示範如何在 MVC 5 中建立交談應用程式使用 MVC 檢視的類似教學課程，請參閱[SignalR 2 和 MVC 5 入門](tutorial-getting-started-with-signalr-and-mvc.md)。

> [!NOTE]
> 本教學課程會示範如何建立在第 2 版中的 SignalR 應用程式。 SignalR 之間變更的詳細資訊的 1.x 和 2，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)和[Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md#TOC13)。

SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。 範例包括社交應用程式、 多使用者遊戲、 企業共同作業及新聞、 天氣或財務更新應用程式。 這些通常稱為即時應用程式。

SignalR 簡化建立即時應用程式的程序。 它包含 ASP.NET server 程式庫和 JavaScript 用戶端程式庫，讓您更輕鬆地管理用戶端伺服器連線，並將內容更新推送到用戶端。 您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式即可取得即時的功能。

本教學課程將示範下列 SignalR 開發工作：

- 將 SignalR 程式庫加入至 ASP.NET web 應用程式。
- 建立內容推播至用戶端中樞類別。
- 建立 OWIN 啟動類別將應用程式設定。
- 使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。

下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。 每個新的使用者可以張貼評論，並查看使用者加入這個聊天室之後新增的註解。

![交談的執行個體](tutorial-getting-started-with-signalr/_static/image1.png)

章節：

- [設定專案](#setup)
- [執行範例](#run)
- [檢查程式碼](#code)
- [後續步驟](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>設定專案

本節顯示如何建立空的 ASP.NET web 應用程式，使用 Visual Studio 2013 和 SignalR 第 2 版新增 SignalR，和建立交談應用程式。

必要條件：

- Visual Studio 2013. 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2013 Express 開發的工具。

下列步驟使用 Visual Studio 2013 建立的 ASP.NET 空 Web 應用程式，並新增 SignalR library:

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-getting-started-with-signalr/_static/image2.png)
2. 在**新增 ASP.NET 專案** 視窗中，保留**空**選取並按**建立專案**。

    ![建立空的網站](tutorial-getting-started-with-signalr/_static/image3.png)
3. 在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |SignalR 中樞類別 (v2)**。 將類別**ChatHub.cs**並將它加入至專案。 這個步驟會建立**ChatHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。

    > [!NOTE]
    > 您也可以加入至專案 SignalR，藉由開啟**工具 |程式庫套件管理員 |Package Manager Console**並執行命令：

    `install-package Microsoft.AspNet.SignalR`

    如果您要加入 SignalR 使用主控台，建立 SignalR 中樞類別當做個別的步驟之後新增 SignalR。

    > [!NOTE]
    > 如果您使用 Visual Studio 2012 **SignalR 中樞類別 (v2)**範本將無法使用。 您可以加入純**類別**呼叫`ChatHub`改為。
4. 在**方案總管 中**，展開 指令碼 節點。 JQuery 和 SignalR 的指令碼程式庫會顯示專案中。
5. 在新的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |OWIN 啟動類別**。 將新類別`Startup`按一下 [確定]。

    > [!NOTE]
    > 如果您使用 Visual Studio 2012 **OWIN 啟動類別**範本將無法使用。 您可以加入純**類別**呼叫`Startup`改為。
7. 下列變更新啟動類別的內容。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |HTML 網頁**。 將新頁面`index.html`。
    >[!NOTE]
    >您可能需要變更版本號碼的 JQuery 和 SignalR 的程式庫參考
9. 在**方案總管 中**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。
10. HTML 網頁中的預設程式碼取代為下列程式碼。

    > [!NOTE]
    > 封裝管理員可能會安裝較新版的 SignalR 指令碼。 請確認下面指令碼參考對應至版本的指令碼專案中的檔案 （它們將位於不同，如果您加入 SignalR 使用 NuGet 而非新增集線器。）

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **儲存所有**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式執行專案。 HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image4.png)
2. 輸入使用者名稱。
3. 從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
4. 每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。 這些註解應該顯示在所有瀏覽器執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維持在伺服器上的討論內容。 集線器會廣播到所有目前使用者的註解。 稍後加入交談的使用者會看到訊息的時間加入它們加入。

    下列螢幕擷取畫面顯示三個瀏覽器執行個體，其中一個執行個體傳送訊息時就會更新中執行之交談應用程式：

    ![對談瀏覽器](tutorial-getting-started-with-signalr/_static/image5.png)
5. 在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。 沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。 這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，然後呼叫在網頁中的指令碼的方式來存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。

**傳送**方法將示範中樞的幾個概念：

- 中樞上宣告的公用方法，可讓用戶端呼叫。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性來存取所有用戶端連線到此中樞。
- 在用戶端上呼叫的函式 (例如`broadcastMessage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。 在程式碼中的重要工作宣告參考宣告的函式，伺服器可以呼叫用戶端推入內容和啟動將訊息傳送至中樞連線的中樞 proxy。

下列程式碼會宣告中樞 proxy 的參考。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> 在 JavaScript 中在 camel 命名法的情況下會伺服器類別和其成員的參考。 程式碼範例會參考 C# **ChatHub**類別在 JavaScript 中為**chatHub**。


下列程式碼是如何在指令碼中建立的回呼函式。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。 HTML 編碼內容，再顯示它兩行是選擇性的並顯示簡單的方式，以避免指令碼資料隱碼。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

下列程式碼會示範如何開啟與中樞的連線。 此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**的 HTML 網頁上的按鈕。

> [!NOTE]
> 這個方法可確保此事件處理常式執行之前，已建立連線。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已學習 SignalR 是建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

如需如何部署範例 SignalR 應用程式至 Azure 的逐步解說，請參閱[與 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。 如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
