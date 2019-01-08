---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr
title: 教學課程：使用 SignalR 2 的即時聊天 |Microsoft Docs
author: pfletcher
description: 此教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 SignalR 加入空白的 ASP.NET web 應用程式上。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: a8b3b778-f009-4369-85c7-e90f9878d8b4
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: aa015abc47bb2450e04e167c0404aaa1d119ba2c
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098620"
---
# <a name="tutorial-real-time-chat-with-signalr-2"></a>教學課程：即時聊天與 SignalR 2

本教學課程會示範如何使用 SignalR 建立即時聊天應用程式。 SignalR 加入空白的 ASP.NET web 應用程式，並建立 HTML 網頁來傳送，並顯示訊息。

在本教學課程中，您：

> [!div class="checklist"]
> * 設定專案
> * 執行範例
> * 檢查程式碼

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必要條件

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。

## <a name="set-up-the-project"></a>設定專案

本節說明如何使用 Visual Studio 2017 與 SignalR 2 來建立空白的 ASP.NET web 應用程式，SignalR，加上建立交談應用程式。

1. 在 Visual Studio 中建立 ASP.NET Web 應用程式。

    ![建立 web](tutorial-getting-started-with-signalr/_static/image2.png)

1. 在 [**新的 ASP.NET 專案-SignalRChat** ] 視窗中，保留**空白**，並選取 **[確定]**。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-SignalRChat**，選取**已安裝** > **Visual C#**   >  **Web**  > **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。

1. 將類別命名為*ChatHub*並將它新增至專案。

    這個步驟會建立*ChatHub.cs*類別檔案，以及新增一組指令碼檔案和專案支援 SignalR 的組件參考。

1. 在新程式碼取代*ChatHub.cs*類別檔案，以下列程式碼：

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample1.cs)]

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-SignalRChat**選取**已安裝** > **Visual C#**   >  **Web** ，然後選取  **OWIN 啟動類別**。

1. 將類別命名為*啟動*並將它新增至專案。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **HTML 網頁**。

1. 將新頁面命名*index* ，然後選取**確定**。

1. 在 **方案總管**，以滑鼠右鍵按一下您建立的 HTML 頁面，然後選取**設定為起始頁**。

1. HTML 網頁中的預設程式碼取代此程式碼：

    [!code-html[Main](tutorial-getting-started-with-signalr/samples/sample3.html)]

1. 在 **方案總管**，展開**指令碼**。

    JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。

    > [!IMPORTANT]
    > 封裝管理員可能已安裝較新版 SignalR 指令碼。

1. 請檢查程式碼區塊中的指令碼參考對應至專案中的指令碼檔案的版本。

    從原始的程式碼區塊的指令碼參考：
    ```html
    <!--Script references. -->
    <!--Reference the jQuery library. -->
    <script src="Scripts/jquery-3.1.1.min.js" ></script>
    <!--Reference the SignalR library. -->
    <script src="Scripts/jquery.signalR-2.2.1.min.js"></script>
    ```

1. 如果兩者不符，更新 *.html*檔案。

1. 從功能表列中，選取**檔案** > **全部儲存**。

## <a name="run-the-sample"></a>執行範例

1. 在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行範例的 播放 按鈕。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr/_static/image3.png)

1. 瀏覽器開啟時，輸入您的對談身分識別的名稱。

1. 從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。

1. 在每個瀏覽器中，輸入唯一的名稱。

1. 現在，加入 註解，然後選取**傳送**。 您可以重複，在其他瀏覽器中。 註解會出現在 即時。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維護伺服器上的討論內容。 中樞會廣播到所有目前使用者的註解。 稍後加入聊天室使用者會看到訊息的時間加入它們加入。

    了解交談應用程式在三個不同的瀏覽器中執行時。 當 Tom、 Anand 和 Susan 傳送訊息時，即時更新所有瀏覽器：

    ![所有的三個瀏覽器會顯示相同的交談記錄](tutorial-getting-started-with-signalr/_static/image4.png)

1. 在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。 沒有名為的指令碼檔案*中樞*SignalR 程式庫產生在執行階段。 此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![在 [指令碼文件] 節點中的自動產生中樞指令碼](tutorial-getting-started-with-signalr/_static/image5.png)

## <a name="examine-the-code"></a>檢查程式碼

SignalRChat 應用程式會示範兩個基本的 SignalR 開發工作。 它說明如何建立中樞。 伺服器會使用該中樞做為主要協調物件。 中樞會使用 SignalR jQuery 程式庫，來傳送和接收訊息。

### <a name="signalr-hubs-in-the-chathubcs"></a>在 ChatHub.cs SignalR 中樞

在上述程式碼範例`ChatHub`類別衍生自`Microsoft.AspNet.SignalR.Hub`類別。 衍生自`Hub`類別是實用的方式，來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法和呼叫它們從網頁中的指令碼，以使用這些方法。

在對談程式碼中，用戶端呼叫`ChatHub.Send`方法以傳送新訊息。 中樞然後將訊息傳送至所有用戶端藉由呼叫`Clients.All.broadcastMessage`。

`Send`方法將示範數個中樞概念：

* 可讓用戶端呼叫，則請在中樞中宣告的公用方法。

* 使用`Microsoft.AspNet.SignalR.Hub.Clients`此中樞的連線與所有的用戶端通訊的動態屬性。

* 在用戶端上呼叫的函式 (例如`broadcastMessage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr/samples/sample4.cs)]

### <a name="signalr-and-jquery-in-the-indexhtml"></a>SignalR 和 jQuery 在 index.html 中

*Index.html*頁面中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。 程式碼會執行許多重要的工作。 它會宣告來參考中樞 proxy、 宣告函式推播內容給用戶端，可以呼叫的伺服器，並開始將訊息傳送至中樞的連接。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample5.js)]

> [!NOTE]
> 在 JavaScript 中必須是 camelCase 伺服器類別和其成員的參考。 程式碼範例參考C# *ChatHub*類別，以做為 JavaScript `chatHub`。

在此程式碼區塊中，您可以建立指令碼中的回呼函式。

[!code-html[Main](tutorial-getting-started-with-signalr/samples/sample6.html)]

在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。 兩個線條的 HTML 編碼內容，然後再顯示它是選擇性的顯示阻止指令碼資料隱碼的好方法。

此程式碼與中樞開啟的連接。

[!code-javascript[Main](tutorial-getting-started-with-signalr/samples/sample7.js)]

> [!NOTE]
> 這個方法可確保程式碼在事件處理常式執行之前，會建立連接。

程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**HTML 網頁中的按鈕。

## <a name="additional-resources"></a>其他資源

如需 SignalR 相關資訊，請參閱下列資源：

* [SignalR 專案](http://signalr.net)

* [SignalR Github 和範例](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>後續步驟

在本教學課程中您：

> [!div class="checklist"]
> * 設定專案
> * 執行範例
> * 檢查程式碼

請前往下一篇文章，以了解如何使用 SignalR 和 MVC 5。
> [!div class="nextstepaction"]
> [SignalR 2 及 MVC 5](tutorial-getting-started-with-signalr-and-mvc.md)