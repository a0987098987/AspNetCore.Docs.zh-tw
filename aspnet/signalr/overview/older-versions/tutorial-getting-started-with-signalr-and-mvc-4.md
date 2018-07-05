---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: 教學課程： 開始使用 SignalR 1.x 及 MVC 4 |Microsoft Docs
author: pfletcher
description: 使用 ASP.NET SignalR 及 ASP.NET MVC 4 建置即時聊天應用程式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: cced40f65700eff073aa4cff4560a137a50bbc5b
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37399953"
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>教學課程： 開始使用 SignalR 1.x 及 MVC 4
====================
藉由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

> 本教學課程會示範如何使用 ASP.NET SignalR 建立即時聊天應用程式。 您會將 SignalR 加入至 MVC 4 應用程式，並建立交談檢視，以傳送和顯示訊息。


## <a name="overview"></a>總覽

本教學課程會向您介紹使用 ASP.NET SignalR 及 ASP.NET MVC 4 的即時 web 應用程式開發。 本教學課程會使用相同的交談應用程式程式碼做為[SignalR 開始使用教學課程](tutorial-getting-started-with-signalr.md)，但會顯示如何將它新增至以網際網路範本為基礎的 MVC 4 應用程式。

在本主題中，您將學習下列 SignalR 開發工作：

- 將 SignalR 程式庫新增至 MVC 4 應用程式。
- 建立中樞類別以將內容推送至用戶端。
- 使用 SignalR jQuery 程式庫在網頁上傳送訊息，並顯示從中樞的更新。

下列螢幕擷取畫面顯示在瀏覽器中執行之已完成的交談應用程式。

![交談執行個體](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

章節：

- [設定專案](#setup)
- [執行範例](#run)
- [檢查程式碼](#code)
- [後續步驟](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>設定專案

必要條件：

- Visual Studio 2010 SP1、 Visual Studio 2012 或 Visual Studio 2012 Express。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發工具。
- 針對 Visual Studio 2010 中，安裝[ASP.NET MVC 4](https://www.microsoft.com/download/details.aspx?id=30683)。

本節說明如何建立 ASP.NET MVC 4 應用程式、 新增 SignalR 程式庫和建立交談應用程式。

1. 1. 在 Visual Studio 建立 ASP.NET MVC 4 應用程式、 SignalRChat，將它命名，然後按一下 [確定]。

        > [!NOTE]
        > 在 VS 2010 中，選取 **.NET Framework 4** Framework 版本下拉式清單控制項中。 SignalR 的程式碼會執行在.NET Framework 4 和 4.5 版。

        ![建立 mvc web](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
      2. 選取 [網際網路應用程式] 範本，請清除的選項**建立單元測試專案**，按一下 [確定]。

         ![建立 mvc 網際網路網站](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
      3. 開啟**工具 |程式庫套件管理員 |套件管理員主控台**並執行下列命令。 此步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。

         `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
      4. 在 **方案總管 中**展開指令碼 資料夾。 請注意 SignalR 的指令碼程式庫已加入至專案。

         ![程式庫參考](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
      5. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增 |新的資料夾**，並新增名為的新資料夾**中樞**。
      6. 以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |類別**，並建立新 C# 類別名為**ChatHub.cs**。 您將使用這個類別做為 SignalR 伺服器中樞將訊息傳送至所有用戶端。

> [!NOTE]
> 如果您使用 Visual Studio 2012，並且已安裝[ASP.NET 和 Web 工具 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)，您可以使用新的 SignalR 項目範本建立中樞類別。 若要這樣做，請以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新的項目**，選取**SignalR Hub 類別 (v1)**，並加以命名**ChatHub.cs**。


1. 中的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. 開啟**Global.asax**專案的檔案，並新增至方法的呼叫`RouteTable.Routes.MapHubs();`中的程式碼的第一行`Application_Start`方法。 此程式碼會註冊 SignalR 中樞的預設路由，並註冊任何其他路由之前，必須呼叫。 已完成`Application_Start`方法如以下範例所示。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. 編輯`HomeController`類別中找到**controllers/Homecontroller.cs**並將下列方法新增至類別。 這個方法會傳回**聊天**您將在稍後的步驟建立的檢視。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. 以滑鼠右鍵按一下`Chat`方法剛剛建立，並按一下 **加入檢視**以建立新的檢視檔案。
5. 在**加入檢視** 對話方塊中，請確定已選取核取方塊**使用版面配置頁或主版頁面**（清除其他核取方塊），然後按一下**新增**。

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. 編輯新的檢視檔案命名為**Chat.cshtml**。 在後&lt;h2&gt;標記中，貼上下列&lt;div&gt;一節和`@section scripts`到頁面的程式碼區塊。 此指令碼可讓頁面，即可將交談訊息傳送，並顯示從伺服器的訊息。 [聊天室] 檢視的完整程式碼會出現在下列程式碼區塊。

    > [!IMPORTANT]
    > 當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，套件管理員可能會安裝比本主題中顯示的版本還新的指令碼的版本。 請確定您的程式碼中的指令碼參考符合安裝在您專案的指令碼程式庫的版本。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **全部儲存**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式中執行專案。
2. 在瀏覽器網址列中，附加 **/home/聊天**專案的預設頁面的 url。 聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. 輸入使用者名稱。
4. 從瀏覽器的網址列複製 URL，並使用它來開啟兩個更多的瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
5. 在每個瀏覽器執行個體中，新增註解，然後按一下**傳送**。 註解應該會顯示在瀏覽器的所有執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維護伺服器上的討論內容。 中樞會廣播到所有目前使用者的註解。 稍後加入聊天室使用者會看到訊息的時間加入它們加入。
6. 下列螢幕擷取畫面顯示在瀏覽器中執行的聊天應用程式。

    ![對談的瀏覽器](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. 在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。 如果您使用 Internet Explorer 作為您的瀏覽器，此節點會顯示在 偵錯模式。 沒有名為的指令碼檔案**中樞**SignalR 程式庫以動態方式產生在執行階段。 此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。 如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接，例如檔案http://mywebsite/signalr/hubs。

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 建立中樞做為主要協調物件在伺服器上，並使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

在程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是實用的方式，來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，並在網頁上的 jQuery 指令碼的方式呼叫它們，以存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**方法以傳送新訊息。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。

**傳送**方法將示範數個中樞概念：

- 可讓用戶端呼叫，則請在中樞中宣告的公用方法。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此集線器。
- 在用戶端上呼叫 jQuery 函式 (例如`addNewMessageToPage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。 在程式碼中的重要工作建立自動產生之 proxy 的中樞中，宣告的函式推播內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。

下列程式碼會宣告中樞 proxy。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> 在 jQuery 中的伺服器類別和其成員的參考會處於駝峰式大小寫。 程式碼範例會參考 C# **ChatHub**在 jQuery 中做為類別**chatHub**。 如果您想要參考`ChatHub`類別在 jQuery 中使用傳統的 pascal 命名法大小寫，如同在 C# 中，編輯 ChatHub.cs 類別檔案。 新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。 然後新增`HubName`屬性設定為`ChatHub`類別，例如`[HubName("ChatHub")]`。 最後，更新您的 jQuery 參考`ChatHub`類別。


下列程式碼示範如何建立指令碼中的回呼函式。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。 選擇性呼叫`htmlEncode`函式會顯示為 HTML 的辦法之前先顯示在頁面中，做為防止指令碼資料隱碼攻擊的方式編碼的訊息內容。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

下列程式碼示範如何使用中樞開啟的連接。 程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**[聊天室] 頁面中的按鈕。

> [!NOTE]
> 這個方法可確保事件處理常式執行之前，會建立連接。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已了解 SignalR 是用來建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

若要深入了更進階的 SignalR 開發概念，請瀏覽下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
