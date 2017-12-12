---
uid: signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
title: "教學課程： 開始使用 SignalR 1.x 和 MVC 4 |Microsoft 文件"
author: pfletcher
description: "使用 ASP.NET SignalR 和 ASP.NET MVC 4 建置即時聊天的應用程式。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/29/2013
ms.topic: article
ms.assetid: eeef9f73-6de3-49f9-b50b-9af22108f2ce
ms.technology: dotnet-signalr
ms.prod: .net-framework
msc.legacyurl: /signalr/overview/older-versions/tutorial-getting-started-with-signalr-and-mvc-4
msc.type: authoredcontent
ms.openlocfilehash: e678c85520613fea2a8d00de60aca04d895d6307
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="tutorial-getting-started-with-signalr-1x-and-mvc-4"></a>教學課程： 開始使用 SignalR 1.x 和 MVC 4
====================
由[Patrick Fletcher](https://github.com/pfletcher)， [Tim Teebken](https://github.com/timlt)

> 本教學課程會示範如何使用 ASP.NET SignalR 建立即時聊天的應用程式。 將 SignalR 加入至 MVC 4 應用程式，並建立聊天檢視來傳送，並顯示訊息。


## <a name="overview"></a>概觀

本教學課程為您介紹 ASP.NET SignalR 和 ASP.NET MVC 4 的即時 web 應用程式開發。 教學課程使用相同的交談應用程式程式碼做為[SignalR 快速入門教學課程](tutorial-getting-started-with-signalr.md)，但示範如何將它加入至 MVC 4 應用程式以網際網路為範本。

本主題中，您將學習下列 SignalR 開發工作：

- 將 SignalR 程式庫加入至 MVC 4 應用程式。
- 建立內容推播至用戶端中樞類別。
- 使用 SignalR jQuery 程式庫，在網頁中的傳送訊息，並顯示從中樞的更新。

下列螢幕擷取畫面顯示瀏覽器中執行之已完成的交談應用程式。

![交談的執行個體](tutorial-getting-started-with-signalr-and-mvc-4/_static/image2.png)

章節：

- [設定專案](#setup)
- [執行範例](#run)
- [檢查程式碼](#code)
- [接下來的步驟](#next)

<a id="setup"></a>

## <a name="set-up-the-project"></a>設定專案

必要條件：

- Visual Studio 2010 SP1、 Visual Studio 2012 或 Visual Studio 2012 Express。 如果您沒有 Visual Studio，請參閱[ASP.NET 下載](https://www.asp.net/downloads)取得免費 Visual Studio 2012 Express 開發的工具。
- 針對 Visual Studio 2010、 安裝[ASP.NET MVC 4](https://www.microsoft.com/en-us/download/details.aspx?id=30683)。

本節說明如何建立 ASP.NET MVC 4 應用程式、 新增 SignalR 程式庫和建立交談應用程式。

1. 1. Visual Studio 中建立 ASP.NET MVC 4 應用程式、 其命名為 SignalRChat，並按一下 [確定]。

        > [!NOTE]
        > 在 VS 2010 中，選取**.NET Framework 4** Framework 版本下拉式控制項中。 在.NET Framework 4 和 4.5 版上執行 SignalR 程式碼。

        ![建立 mvc web](tutorial-getting-started-with-signalr-and-mvc-4/_static/image3.png)
    2. 選取 網際網路應用程式範本，請清除選項以**建立單元測試專案**，按一下 確定。

        ![建立 mvc 網際網路站台](tutorial-getting-started-with-signalr-and-mvc-4/_static/image4.png)
    3. 開啟**工具 |程式庫套件管理員 |Package Manager Console** ，然後執行下列命令。 這個步驟可將一組指令碼檔案和啟用 SignalR 功能的組件參考加入至專案。

        `install-package Microsoft.AspNet.SignalR -Version 1.1.3`
    4. 在**方案總管 中**展開指令碼 資料夾。 請注意 SignalR 的指令碼程式庫已加入至專案。

        ![程式庫參考](tutorial-getting-started-with-signalr-and-mvc-4/_static/image6.png)
    5. 在**方案總管 中**，以滑鼠右鍵按一下專案，選取**新增 |新的資料夾**，並加入新的資料夾，名為**集線器**。
    6. 以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |類別**，並建立新的 C# 類別名稱為**ChatHub.cs**。 您將使用此類別將訊息傳送至所有用戶端的 SignalR 伺服器集線器。

> [!NOTE]
> 如果您使用 Visual Studio 2012，而且已安裝[ASP.NET 和 Web 工具 2012.2 更新](../../../visual-studio/overview/2012/aspnet-and-web-tools-20122-release-notes-rtw.md#_Installation)，您可以使用新的 SignalR 項目範本建立中樞類別。 若要這樣做，請以滑鼠右鍵按一下**集線器**資料夾中，按一下 **新增 |新項目**，選取**SignalR 中樞類別 (v1)**，並加以**ChatHub.cs**。


1. 中的程式碼取代**ChatHub**為下列程式碼的類別。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample1.cs)]
2. 開啟**Global.asax**檔的專案，並加入至方法的呼叫`RouteTable.Routes.MapHubs();`做為第一行中的程式碼`Application_Start`方法。 此程式碼會註冊 SignalR 中樞的預設路由，並必須在呼叫之前註冊的任何其他路由。 已完成`Application_Start`方法看起來類似下列的範例。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample2.cs)]
3. 編輯`HomeController`類別中找到**Controllers/HomeController.cs**並將下列方法加入至類別。 這個方法會傳回**聊天**您將在稍後步驟中建立的檢視。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample3.cs)]
4. 以滑鼠右鍵按一下內`Chat`方法剛剛建立，並按一下**加入檢視**建立新的檢視檔案。
5. 在**加入檢視**] 對話方塊中，請確定已選取核取方塊**使用版面配置頁或主版頁面**（清除其他核取方塊），然後按一下 [**新增**。

    ![新增檢視](tutorial-getting-started-with-signalr-and-mvc-4/_static/image8.png)
6. 編輯名為新的檢視檔案**Chat.cshtml**。 之後&lt;h2&gt;標記中，貼上下列&lt;div&gt;區段和`@section scripts`加入頁面中的程式碼區塊。 此指令碼可讓網頁來傳送訊息，並顯示從伺服器的訊息。 [聊天室] 檢視的完整程式碼會出現在下列程式碼區塊。

    > [!IMPORTANT]
    > 當您將 SignalR 和其他指令碼程式庫加入您的 Visual Studio 專案時，封裝管理員可能會安裝版本比本主題所顯示的版本還新的指令碼。 請確定您的程式碼中的指令碼參考符合安裝在您的專案中的指令碼程式庫的版本。

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample4.cshtml)]
7. **儲存所有**專案。

<a id="run"></a>

## <a name="run-the-sample"></a>執行範例

1. 按 F5 以偵錯模式執行專案。
2. 在瀏覽器位址列中，附加**/首頁/聊天**專案的預設網頁的 url。 聊天室頁面載入瀏覽器執行個體，並提示輸入使用者名稱。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc-4/_static/image9.png)
3. 輸入使用者名稱。
4. 從瀏覽器位址列複製 URL，並使用它來開啟兩個的多個瀏覽器執行個體。 在每個瀏覽器執行個體中，輸入唯一的使用者名稱。
5. 每個瀏覽器執行個體中，加入註解，然後按一下**傳送**。 這些註解應該顯示在所有瀏覽器執行個體。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維持在伺服器上的討論內容。 集線器會廣播到所有目前使用者的註解。 稍後加入交談的使用者會看到訊息的時間加入它們加入。
6. 下列螢幕擷取畫面顯示瀏覽器中執行之交談應用程式。

    ![對談瀏覽器](tutorial-getting-started-with-signalr-and-mvc-4/_static/image11.png)
7. 在**方案總管 中**，檢查**指令碼文件**節點執行的應用程式。 如果您使用 Internet Explorer 瀏覽器，此節點會顯示在偵錯模式。 沒有名為指令碼檔案**集線器**SignalR library 動態產生在執行階段。 這個檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。 如果您使用非 Internet Explorer 的瀏覽器，您也可以存取動態**集線器**瀏覽至它直接管理，例如 http://mywebsite/signalr/hubs 檔案。

    ![產生的中樞指令碼](tutorial-getting-started-with-signalr-and-mvc-4/_static/image13.png)

<a id="code"></a>

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式將示範兩個基本的 SignalR 開發工作： 協調主要伺服器上的物件，以建立中樞和使用 SignalR jQuery 程式庫來傳送和接收訊息。

### <a name="signalr-hubs"></a>SignalR 中樞

下列程式碼範例**ChatHub**類別衍生自**Microsoft.AspNet.SignalR.Hub**類別。 衍生自**中樞**類別是有用的方式來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，然後呼叫在網頁中的 jQuery 指令碼的方式來存取這些方法。

在對談程式碼中，用戶端呼叫**ChatHub.Send**傳送新訊息的方法。 中樞接著將訊息傳送至所有用戶端藉由呼叫**Clients.All.addNewMessageToPage**。

**傳送**方法將示範中樞的幾個概念：

- 中樞上宣告的公用方法，可讓用戶端呼叫。
- 使用**Microsoft.AspNet.SignalR.Hub.Clients**屬性來存取所有用戶端連線到此中樞。
- 在用戶端上呼叫 jQuery 函式 (例如`addNewMessageToPage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample5.cs)]

### <a name="signalr-and-jquery"></a>SignalR 和 jQuery

**Chat.cshtml**檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞通訊。 在程式碼中的重要工作建立的自動產生 proxy 集線器，宣告的函式推入內容給用戶端，可以呼叫的伺服器和啟動連線將訊息傳送至中樞的參考。

下列程式碼會宣告中樞 proxy。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample6.js)]

> [!NOTE]
> 在 jQuery 伺服器類別和其成員的參考是 camel 命名法的大小寫。 程式碼範例會參考 C# **ChatHub**類別中當做 jQuery **chatHub**。 如果您想要參考`ChatHub`類別在 jQuery 與傳統 Pascal 大小寫在 C# 中一樣編輯 ChatHub.cs 類別檔案。 新增`using`陳述式來參考`Microsoft.AspNet.SignalR.Hubs`命名空間。 然後加入`HubName`屬性`ChatHub`類別，例如`[HubName("ChatHub")]`。 最後，更新您 jQuery 參考`ChatHub`類別。


下列程式碼會示範如何在指令碼中建立的回呼函式。 在伺服器上的中樞類別會呼叫此函式可將內容更新推送到每個用戶端。 選擇性呼叫`htmlEncode`函式便會顯示為 HTML 的方式之後，顯示在頁面中，以避免指令碼資料隱碼的方式編碼的訊息內容。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample7.html)]

下列程式碼會示範如何開啟與中樞的連線。 此程式碼啟動連線，並接著將該函式來處理 click 事件上**傳送**聊天室網頁中的按鈕。

> [!NOTE]
> 這個方法可確保此事件處理常式執行之前，已建立連線。


[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc-4/samples/sample8.js)]

<a id="next"></a>

## <a name="next-steps"></a>後續步驟

您已學習 SignalR 是建置即時 web 應用程式的架構。 您也學到幾個 SignalR 開發工作： 如何將 ASP.NET 應用程式中的 SignalR、 如何建立中樞類別，以及如何傳送和接收來自中樞的訊息。

深入了解更多進階的 SignalR 發展概念，請造訪下列網站 SignalR 原始碼和資源：

- [SignalR 專案](http://signalr.net)
- [SignalR Github 和範例](https://github.com/SignalR/SignalR)
- [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)
