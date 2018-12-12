---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 教學課程：開始使用 SignalR 2 |Microsoft Docs
author: pfletcher
description: 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 您會加入空白的 ASP.NET web 應用程式中的 SignalR，並建立 HTML pa...
ms.author: riande
ms.date: 06/10/2014
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3b06e7d0a602e061112adbceba92276f836f6311
ms.sourcegitcommit: 74e3be25ea37b5fc8b4b433b0b872547b4b99186
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/12/2018
ms.locfileid: "53287334"
---
<a name="tutorial-getting-started-with-signalr-2"></a>教學課程：開始使用 SignalR 2
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

[下載已完成的專案](http://code.msdn.microsoft.com/SignalR-Getting-Started-b9d18aa9)

> 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 將 SignalR 加入空白的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。 
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

本教學課程將示範如何建置簡單的瀏覽器為基礎的聊天應用程式來介紹 SignalR 開發。 您會加入空白的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收聊天訊息。 說明如何在 MVC 5 中建立的交談應用程式使用 MVC 檢視的類似教學課程，請參閱[開始使用 SignalR 2 和 MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)。

> [!NOTE]
> 本教學課程會示範如何建立第 2 版中的 SignalR 應用程式。 如需有關變更之間 SignalR 1.x 和 2，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)和[Visual Studio 2013 版本資訊](../../../visual-studio/overview/2013/release-notes.md#TOC13)。

SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。 範例包括社交應用程式、 多使用者的遊戲、 商務共同作業及新聞、 天氣或財務更新應用程式。 這些通常稱為即時應用程式。

SignalR 簡化建置即時應用程式的程序。 其中包含 ASP.NET server 程式庫和 JavaScript 的用戶端程式庫，讓您更輕鬆地管理用戶端-伺服器連線，並將內容更新推送至用戶端。 您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式取得即時的功能。

本教學課程會示範下列 SignalR 開發工作：

- 加入 ASP.NET web 應用程式中的 SignalR 程式庫。
- 建立中樞類別以將內容推送至用戶端。
- 建立應用程式設定為 OWIN 啟動類別。
- 使用 SignalR jQuery 程式庫在網頁上傳送訊息，並顯示從中樞的更新。

下列螢幕擷取畫面顯示在瀏覽器中執行的聊天應用程式。 每個新的使用者可以張貼評論，並查看新增的使用者加入這個聊天室之後的註解。

![交談執行個體](tutorial-getting-started-with-signalr/_static/image1.png)

章節：

- [設定專案](#setup)
- [執行範例](#run)
- [檢查程式碼](#code)
- [後續步驟](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>設定專案

本節說明如何使用 Visual Studio 2013 與 SignalR 第 2 版來建立空白的 ASP.NET web 應用程式，SignalR，加上建立交談應用程式。

必要條件：

- Visual Studio 2013。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2013 Express 開發工具。

下列步驟使用 Visual Studio 2013 建立 ASP.NET 空白 Web 應用程式並新增 SignalR 程式庫：

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-getting-started-with-signalr/_static/image2.png)
2. 在 **新增 ASP.NET 專案** 視窗中，保持**空白**選取，然後按一下 **建立專案**。

    ![建立空白網站](tutorial-getting-started-with-signalr/_static/image3.png)
3. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |SignalR Hub 類別 (v2)**。 將類別命名為**ChatHub.cs**並將它新增至專案。 這個步驟會建立**ChatHub**類別，並將一組指令碼檔案和支援 SignalR 的組件參考加入至專案。

    > [!NOTE]
    > 您也可以新增至專案 SignalR，開啟**工具 > NuGet 套件管理員 > Package Manager Console**並執行命令：

    `install-package Microsoft.AspNet.SignalR`

    如果您可以使用主控台來新增 SignalR，建立 SignalR hub 類別做為個別的步驟之後新增 SignalR。

    > [!NOTE]
    > 如果您使用 Visual Studio 2012、 **SignalR Hub 類別 (v2)** 範本將無法使用。 您可以加入純**類別**稱為`ChatHub`改。
4. 在 [**方案總管] 中**，展開 [指令碼] 節點。 JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。
5. 在新程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |OWIN 啟動類別**。 新類別命名`Startup`按一下 [確定]。

    > [!NOTE]
    > 如果您使用 Visual Studio 2012、 **OWIN 啟動類別**範本將無法使用。 您可以加入純**類別**稱為`Startup`改。
7. 將新的啟動類別的內容變更為下列。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |HTML 網頁**。 將新頁面命名`index.html`。
    >[!NOTE]
    >您可能需要變更的 JQuery 和 SignalR 的程式庫參考的版本號碼
9. 在 **方案總管**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。
10. 取代為下列程式碼中的 HTML 網頁中的預設程式碼。

    > [!NOTE]
    > 封裝管理員可能會安裝新版 SignalR 指令碼。 確認下方的指令碼參考對應至版本的指令碼專案中的檔案 （它們會不同，如果您已新增使用 NuGet，而不是新增集線器的 SignalR。）

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]
11. **全部儲存**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式中執行專案。 HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image4.png)
2. 輸入使用者名稱。
3. 從瀏覽器的網址列複製 URL，並使用它來開啟兩個更多的瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
4. 在每個瀏覽器執行個體中，新增註解，然後按一下**傳送**。 註解應該會顯示在瀏覽器的所有執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維護伺服器上的討論內容。 中樞會廣播到所有目前使用者的註解。 稍後加入聊天室使用者會看到訊息的時間加入它們加入。

    下列螢幕擷取畫面顯示三個瀏覽器執行個體，一個執行個體傳送訊息時，當然也會更新在執行的聊天應用程式：

    ![對談的瀏覽器](tutorial-getting-started-with-signalr/_static/image5.png)
5. 在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。 沒有名為的指令碼檔案**中樞**SignalR 程式庫以動態方式產生在執行階段。 此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![](tutorial-getting-started-with-signalr/_static/image6.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 建立中樞做為主要協調物件在伺服器上，並使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

在程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是實用的方式，來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，並從網頁中的指令碼的方式呼叫它們，以存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**方法以傳送新訊息。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。

**傳送**方法將示範數個中樞概念：

- 可讓用戶端呼叫，則請在中樞中宣告的公用方法。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性，來存取所有用戶端連線到此集線器。
- 在用戶端上呼叫的函式 (例如`broadcastMessage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。 在程式碼中的重要工作宣告參考宣告的函式推播內容給用戶端，可以呼叫的伺服器和啟動將訊息傳送至中樞連線的中樞 proxy。

下列程式碼會宣告將中樞 proxy 的參考。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> 在 JavaScript 伺服器類別和其成員的參考是駝峰式大小寫。 程式碼範例會參考 C# **ChatHub**類別，以做為 JavaScript **chatHub**。


下列程式碼是您在指令碼中建立回呼函式的方法。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。 HTML 編碼內容，顯示它之前的兩行是選擇性的而且顯示簡單的方式，以避免指令碼資料隱碼攻擊。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

下列程式碼示範如何使用中樞開啟的連接。 程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**HTML 網頁中的按鈕。

> [!NOTE]
> 這個方法可確保事件處理常式執行之前，會建立連接。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已了解 SignalR 是用來建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

如需如何部署 Azure 範例 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 使用 Azure App Service 中的 Web 應用程式的使用 SignalR](../deployment/using-signalr-with-azure-web-sites.md)。 如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[Azure App Service 中建立 ASP.NET web 應用程式](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-get-started/)。

若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
