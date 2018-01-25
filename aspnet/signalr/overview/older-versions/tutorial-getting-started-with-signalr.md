---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr
title: "教學課程： 開始使用 SignalR 1.x |Microsoft 文件"
author: pfletcher
description: "使用 ASP.NET SignalR 建立 HTML 網頁中的即時聊天應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/18/2013
ms.topic: article
ms.assetid: fdc3599a-5217-44c1-951f-0eec9812dce7
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.openlocfilehash: ce4953a0abf64af28ef4dbc5a62bb2d989343d99
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="tutorial-getting-started-with-signalr-1x"></a>教學課程： 開始使用 SignalR 1.x
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

> 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 將 SignalR 加入空的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。


## <a name="overview"></a>總覽

此教學課程介紹 SignalR 開發，以顯示如何建立簡單的瀏覽器為基礎的交談應用程式。 您會加入空的 ASP.NET web 應用程式中的 SignalR 程式庫、 建立中樞類別將訊息傳送至用戶端，並建立 HTML 網頁，可讓使用者傳送及接收交談的訊息。 示範如何在 MVC 4 中建立交談應用程式使用 MVC 檢視的類似教學課程，請參閱[SignalR 和 MVC 4 入門](index.md)。

> [!NOTE]
> 本教學課程使用 SignalR 的發行 (1.x) 版本。 SignalR 之間變更的詳細資訊的 1.x 和 2.0，請參閱[升級 SignalR 1.x 專案](../releases/upgrading-signalr-1x-projects-to-20.md)。

SignalR 是開放原始碼.NET 程式庫建置 web 應用程式需要即時使用者互動或即時資料更新。 範例包括社交應用程式、 多使用者遊戲、 企業共同作業及新聞、 天氣或財務更新應用程式。 這些通常稱為即時應用程式。

SignalR 簡化建立即時應用程式的程序。 它包含 ASP.NET server 程式庫和 JavaScript 用戶端程式庫，讓您更輕鬆地管理用戶端伺服器連線，並將內容更新推送到用戶端。 您可以將 SignalR 程式庫加入現有的 ASP.NET 應用程式即可取得即時的功能。

本教學課程將示範下列 SignalR 開發工作：

- 將 SignalR 程式庫加入至 ASP.NET web 應用程式。
- 建立內容推播至用戶端中樞類別。
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

本節顯示如何建立空的 ASP.NET web 應用程式，新增 SignalR，和建立交談應用程式。

必要條件：

- Visual Studio 2010 SP1 或 2012年。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發的工具。
- [Microsoft ASP.NET 及 Web 工具 2012.2](https://go.microsoft.com/fwlink/?LinkId=279941)。 針對 Visual Studio 2012 中，此安裝程式會加入新的 ASP.NET 功能包括 SignalR 範本加入 Visual Studio。 Visual Studio 2010 SP1 中，安裝程式不提供，但您可以完成本教學課程中的設定步驟所述安裝 SignalR NuGet 封裝。

下列步驟使用 Visual Studio 2012 建立空白 ASP.NET Web 應用程式，並新增 SignalR library:

1. 在 Visual Studio 中建立空白 ASP.NET Web 應用程式。

    ![建立空的網站](tutorial-getting-started-with-signalr/_static/image2.png)
2. 開啟**Package Manager Console**選取**工具 |程式庫套件管理員 |Package Manager Console**。 到主控台視窗輸入下列命令：

    `Install-Package Microsoft.AspNet.SignalR -Version 1.1.3`

    此命令會安裝最新版的 SignalR 1.x。
3. 在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |類別**。 將新類別**ChatHub**。
4. 在**方案總管 中**展開指令碼 節點。 JQuery 和 SignalR 的指令碼程式庫會顯示專案中。

    ![程式庫參考](tutorial-getting-started-with-signalr/_static/image3.png)
5. 中的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]
6. 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。 在**加入新項目**對話方塊中，選取**全域應用程式類別**按一下**新增**。

    ![加入全域](tutorial-getting-started-with-signalr/_static/image4.png)
7. 加入下列`using`陳述式之後提供`using`Global.asax.cs 類別中的陳述式。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample2.cs)]
8. 加入下列行中的程式碼`Application_Start`全域註冊 SignalR 中樞的預設路由類別的方法。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample3.cs)]
9. 在**方案總管] 中**，以滑鼠右鍵按一下專案，然後按一下 [**新增 |新項目**。 在**加入新項目**對話方塊中，選取 Html 網頁並按一下**新增**。
10. 在**方案總管 中**，以滑鼠右鍵按一下您剛建立的 HTML 網頁，然後按一下**設定為起始頁**。
11. HTML 網頁中的預設程式碼取代為下列程式碼。

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample4.html)]
12. **儲存所有**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式執行專案。 HTML 頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image5.png)
2. 輸入使用者名稱。
3. 從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
4. 每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。 這些註解應該顯示在所有瀏覽器執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維持在伺服器上的討論內容。 集線器會廣播到所有目前使用者的註解。 稍後加入交談的使用者會看到訊息的時間加入它們加入。

    下列螢幕擷取畫面顯示三個瀏覽器執行個體，其中一個執行個體傳送訊息時就會更新中執行之交談應用程式：

    ![對談瀏覽器](tutorial-getting-started-with-signalr/_static/image6.png)
5. 在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。 沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。 這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr/_static/image7.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，然後呼叫在網頁中的 jQuery 指令碼的方式來存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.broadcastMessage**。

**傳送**方法將示範中樞的幾個概念：

- 中樞上宣告的公用方法，可讓用戶端呼叫。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**動態屬性來存取所有用戶端連線到此中樞。
- 在用戶端上呼叫 jQuery 函式 (例如`broadcastMessage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

HTML 網頁中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。 在程式碼中的重要工作宣告參考宣告的函式，伺服器可以呼叫用戶端推入內容和啟動將訊息傳送至中樞連線的中樞 proxy。

下列程式碼會宣告中樞 proxy。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample6.js)]

> [!NOTE]
> 在 jQuery 伺服器類別和其成員的參考是 camel 命名法的大小寫。 程式碼範例會參考 C# **ChatHub**類別中當做 jQuery **chatHub**。


下列程式碼是如何在指令碼中建立的回呼函式。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。 HTML 編碼內容，再顯示它兩行是選擇性的並顯示簡單的方式，以避免指令碼資料隱碼。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample7.html)]

下列程式碼會示範如何開啟與中樞的連線。 此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**的 HTML 網頁上的按鈕。

> [!NOTE]
> 這個方法會確保此事件處理常式執行之前，已建立連線。


[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已學習 SignalR 是建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

您可以提供範例應用程式在本教學課程或其他的 SignalR 應用程式在網際網路上透過將它們部署到主機服務提供者。 Microsoft 提供的免費 web 裝載中可用的最多 10 個 web sites [Windows Azure 試用帳戶](https://www.windowsazure.com/pricing/free-trial/?WT.mc_id=A443DD604)。 如需如何部署範例 SignalR 應用程式的逐步解說，請參閱[發行 SignalR 快速入門範例做為 Windows Azure 網站](https://blogs.msdn.com/b/timlee/archive/2013/02/27/deploy-the-signalr-getting-started-sample-as-a-windows-azure-web-site.aspx)。 如需如何部署 Visual Studio web 專案至 Windows Azure 網站的詳細資訊，請參閱[部署 ASP.NET 應用程式至 Windows Azure 網站](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。 (注意： WebSocket 傳輸目前不支援的 Windows Azure 網站。 當 WebSocket 傳輸無法使用，SignalR 的 [傳輸] 區段中所述，使用其他可用的傳輸[SignalR 主題的介紹](index.md)。)

深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
