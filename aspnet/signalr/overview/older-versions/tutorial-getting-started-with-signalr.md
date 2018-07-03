---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: 教學課程： 開始使用 SignalR 1.x |Microsoft Docs
author: pfletcher
description: 您可以使用 ASP.NET SignalR 來建立 HTML 網頁中的即時聊天應用程式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: 3cfeb95bcaa984de5cff246173ad03e2a774fc0c
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37402018"
---
<a name="tutorial-getting-started-with-signalr-1x"></a>教學課程： 開始使用 SignalR 1.x
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

> 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 將 SignalR 加入空白的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。


## <a name="overview"></a>總覽

本教學課程將示範如何建置簡單的瀏覽器為基礎的聊天應用程式來介紹 SignalR 開發。 您會加入空白的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收聊天訊息。 說明如何在 MVC 4 中建立的交談應用程式使用 MVC 檢視的類似教學課程，請參閱[開始使用 SignalR 和 MVC 4](index.md)。

> [!NOTE]
> 本教學課程使用 SignalR 的版本 (1.x) 版本。 如需有關變更之間 SignalR 1.x 和 2.0，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)。

SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。 範例包括社交應用程式、 多使用者的遊戲、 商務共同作業及新聞、 天氣或財務更新應用程式。 這些通常稱為即時應用程式。

SignalR 簡化建置即時應用程式的程序。 其中包含 ASP.NET server 程式庫和 JavaScript 的用戶端程式庫，讓您更輕鬆地管理用戶端-伺服器連線，並將內容更新推送至用戶端。 您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式取得即時的功能。

本教學課程會示範下列 SignalR 開發工作：

- 加入 ASP.NET web 應用程式中的 SignalR 程式庫。
- 建立中樞類別以將內容推送至用戶端。
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

本節說明如何建立空的 ASP.NET web 應用程式，SignalR，加上建立交談應用程式。

必要條件：

- Visual Studio 2010 SP1 或 2012年。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發工具。
- [Microsoft ASP.NET 和 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)。 For Visual Studio 2012，此安裝程式會新增新的 ASP.NET 功能，包括 Visual studio 的 SignalR 範本。 Visual Studio 2010 sp1，安裝程式不提供，但您可以透過設定步驟中所述安裝 SignalR NuGet 套件來完成本教學課程。

下列步驟使用 Visual Studio 2012 建立 ASP.NET 空白 Web 應用程式並新增 SignalR 程式庫：

1. 在 Visual Studio 建立 ASP.NET 空白 Web 應用程式。

    ![建立空白網站](tutorial-getting-started-with-signalr/_static/image2.png)
2. 開啟**Package Manager Console**藉由選取**工具 |程式庫套件管理員 |套件管理員主控台**。 主控台視窗中輸入下列命令：

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    此命令會安裝最新版的 SignalR 1.x。
3. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |類別**。 新類別命名**ChatHub**。
4. 在 **方案總管 中**展開指令碼 節點。 JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。

    ![程式庫參考](tutorial-getting-started-with-signalr/_static/image3.png)
5. 中的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。 在 **加入新項目**對話方塊中，選取**全域應用程式類別**然後按一下**新增**。

    ![新增全域](tutorial-getting-started-with-signalr/_static/image4.png)
7. 新增下列`using`之後所提供的陳述式`using`Global.asax.cs 類別中的陳述式。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 加入下列行中的程式碼`Application_Start`要註冊 SignalR 中樞的預設路由的全域類別的方法。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. 在 **方案總管**，以滑鼠右鍵按一下專案，然後按一下 **新增 |新的項目**。 在 [**加入新項目**] 對話方塊中，選取 Html 頁面，然後按一下**新增**。
10. 在 **方案總管**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。
11. 取代為下列程式碼中的 HTML 網頁中的預設程式碼。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **全部儲存**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式中執行專案。 HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image5.png)
2. 輸入使用者名稱。
3. 從瀏覽器的網址列複製 URL，並使用它來開啟兩個更多的瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
4. 在每個瀏覽器執行個體中，新增註解，然後按一下**傳送**。 註解應該會顯示在瀏覽器的所有執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維護伺服器上的討論內容。 中樞會廣播到所有目前使用者的註解。 稍後加入聊天室使用者會看到訊息的時間加入它們加入。

    下列螢幕擷取畫面顯示三個瀏覽器執行個體，一個執行個體傳送訊息時，當然也會更新在執行的聊天應用程式：

    ![對談的瀏覽器](tutorial-getting-started-with-signalr/_static/image6.png)
5. 在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。 沒有名為的指令碼檔案**中樞**SignalR 程式庫以動態方式產生在執行階段。 此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 建立中樞做為主要協調物件在伺服器上，並使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

在程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是實用的方式，來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，並在網頁上的 jQuery 指令碼的方式呼叫它們，以存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**方法以傳送新訊息。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。

**傳送**方法將示範數個中樞概念：

- 可讓用戶端呼叫，則請在中樞中宣告的公用方法。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性，來存取所有用戶端連線到此集線器。
- 在用戶端上呼叫 jQuery 函式 (例如`broadcastMessage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。 在程式碼中的重要工作宣告參考宣告的函式推播內容給用戶端，可以呼叫的伺服器和啟動將訊息傳送至中樞連線的中樞 proxy。

下列程式碼會宣告中樞 proxy。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> 在 jQuery 中的伺服器類別和其成員的參考會處於駝峰式大小寫。 程式碼範例會參考 C# **ChatHub**在 jQuery 中做為類別**chatHub**。


下列程式碼是您在指令碼中建立回呼函式的方法。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。 HTML 編碼內容，顯示它之前的兩行是選擇性的而且顯示簡單的方式，以避免指令碼資料隱碼攻擊。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

下列程式碼示範如何使用中樞開啟的連接。 程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**HTML 網頁中的按鈕。

> [!NOTE]
> 這種方法可確保事件處理常式執行之前，會建立連接。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已了解 SignalR 是用來建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

您可以提供的範例應用程式在本教學課程或其他的 SignalR 應用程式在網際網路上透過將它們部署到主機服務提供者。 Microsoft 提供免費的 web 裝載中可用的最多 10 個 web sites [Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 如需如何部署範例的 SignalR 應用程式的逐步解說，請參閱 <<c0> [ 發佈 SignalR 快速入門範例 Windows Azure 網站形式](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)。 如需如何將 Visual Studio web 專案部署至 Windows Azure 網站的詳細資訊，請參閱[部署 ASP.NET 應用程式至 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。 (注意： WebSocket 傳輸目前不支援的 Windows Azure 網站。 當 WebSocket 傳輸無法使用，SignalR 使用的其他可用的傳輸的 [傳輸] 區段中所述[SignalR 主題的介紹](index.md)。)

若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
