---
uid: signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
title: 教學課程：使用 SignalR 2 和 MVC 5 的即時聊天 |Microsoft Docs
author: pfletcher
description: 本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天應用程式。 您可以新增 SignalR 的 MVC 5 應用程式。
ms.author: riande
ms.date: 01/02/2019
ms.assetid: 80bfe5fb-bdfc-41fe-ac43-2132e5d69fac
msc.legacyurl: /signalr/overview/getting-started/tutorial-getting-started-with-signalr-and-mvc
msc.type: authoredcontent
ms.topic: tutorial
ms.openlocfilehash: eb4b7e1403f4070d65702b756bf98c5294c7fb17
ms.sourcegitcommit: 97d7a00bd39c83a8f6bccb9daa44130a509f75ce
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/08/2019
ms.locfileid: "54098600"
---
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a>教學課程：即時聊天與 SignalR 2 和 MVC 5

本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天應用程式。 您加入 MVC 5 應用程式中的 SignalR 和建立交談檢視，以傳送，並顯示訊息。

在本教學課程中，您：

> [!div class="checklist"]
> * 設定專案
> * 執行範例
> * 檢查程式碼

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a>必要條件

* [Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。

## <a name="set-up-the-project"></a>設定專案

本節說明如何使用 Visual Studio 2017 與 SignalR 2 來建立空的 ASP.NET MVC 5 應用程式、 新增 SignalR 程式庫，並建立交談應用程式。

1. 在 Visual Studio 中建立.NET Framework 4.5 為目標的 C# ASP.NET 應用程式、 SignalRChat，將它命名，並按一下 [確定]。

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. 在 **新增 ASP.NET Web 應用程式-SignalRMvcChat**，選取**MVC** ，然後選取**變更驗證**。

1. 在 **變更驗證**，選取**不需要驗證**，按一下 **確定**。

    ![選取 不驗證](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. 在 **新增 ASP.NET Web 應用程式-SignalRMvcChat**，選取**確定**。

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。

1. 在 **加入新項目-SignalRChat**，選取**已安裝** > **Visual C#**   >  **Web**  > **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。

1. 將類別命名為*ChatHub*並將它新增至專案。

    這個步驟會建立*ChatHub.cs*類別檔案，以及新增一組指令碼檔案和專案支援 SignalR 的組件參考。

1. 在新程式碼取代*ChatHub.cs*類別檔案，以下列程式碼：

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. 在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。

1. 新類別命名*啟動*並將它新增至專案。

1. 中的程式碼取代*Startup.cs*類別檔案，以下列程式碼：

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. 在 **方案總管**，選取**控制站** > **HomeController.cs**。

1. 將下列方法來新增*HomeController.cs*。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    這個方法會傳回**聊天**您在稍後步驟中建立的檢視。

1. 中**方案總管**，以滑鼠右鍵按一下**檢視** > **首頁**，然後選取**新增** >   **檢視**。

1. 在**加入檢視**，將新檢視**聊天**，然後選取**新增**。

1. 內容取代**Chat.cshtml**以下列程式碼：

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. 在 **方案總管**，展開**指令碼**。

    JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。

    > [!IMPORTANT]
    > 封裝管理員可能已安裝較新版 SignalR 指令碼。

1. 請檢查程式碼區塊中的指令碼參考對應至專案中的指令碼檔案的版本。

    從原始的程式碼區塊的指令碼參考：

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. 如果兩者不符，更新 *.cshtml*檔案。

1. 從功能表列中，選取**檔案** > **全部儲存**。

## <a name="run-the-sample"></a>執行範例

1. 在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行範例的 播放 按鈕。

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. 瀏覽器開啟時，輸入您的對談身分識別的名稱。

1. 從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。

1. 在每個瀏覽器中，輸入唯一的名稱。

1. 現在，加入 註解，然後選取**傳送**。 您可以重複，在其他瀏覽器中。 註解會出現在即時。

    > [!NOTE]
    > 這個簡單的聊天應用程式不會維護伺服器上的討論內容。 中樞會廣播到所有目前使用者的註解。 稍後加入聊天室使用者會看到訊息的時間加入它們加入。

    了解交談應用程式在三個不同的瀏覽器中執行時。 當 Tom、 Anand 和 Susan 傳送訊息時，即時更新所有瀏覽器：

    ![所有的三個瀏覽器會顯示相同的交談記錄](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. 在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。 沒有名為的指令碼檔案*中樞*SignalR 程式庫產生在執行階段。 此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。

    ![在 [指令碼文件] 節點中的自動產生中樞指令碼](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a>檢查程式碼

SignalR 交談應用程式會示範兩個基本的 SignalR 開發工作。 它說明如何建立中樞。 伺服器會使用該中樞做為主要協調物件。 中樞會使用 SignalR jQuery 程式庫，來傳送和接收訊息。

### <a name="signalr-hubs-in-the-chathubcs"></a>在 ChatHub.cs SignalR 中樞

在程式碼範例中，`ChatHub`類別衍生自`Microsoft.AspNet.SignalR.Hub`類別。 衍生自`Hub`類別是實用的方式，來建置 SignalR 應用程式。 您可以建立中樞類別上的公用方法，並從網頁中的指令碼的方式呼叫它們，以存取這些方法。

在對談程式碼中，用戶端呼叫`ChatHub.Send`方法以傳送新訊息。 中樞接著將訊息傳送至所有用戶端藉由呼叫`Clients.All.addNewMessageToPage`。

`Send`方法將示範數個中樞概念：

* 可讓用戶端呼叫，則請在中樞中宣告的公用方法。

* 使用`Microsoft.AspNet.SignalR.Hub.Clients`此中樞的連線與所有的用戶端通訊的動態屬性。

* 在用戶端上呼叫的函式 (例如`addNewMessageToPage`函式) 來更新用戶端。

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a>SignalR 和 jQuery Chat.cshtml

*Chat.cshtml*檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。  程式碼會執行許多重要的工作。 它會建立中樞自動產生 proxy 的參考、 宣告的函式，伺服器可以呼叫以將內容推送至用戶端，同時也會將訊息傳送至中樞的連接。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> 在 JavaScript 中，伺服器類別和其成員的參考會處於 camelCase。 程式碼範例參考C#`ChatHub`在 JavaScript 中的類別`chatHub`。

在此程式碼區塊中，您可以建立指令碼中的回呼函式。

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。 選擇性呼叫`htmlEncode`函式會顯示為 HTML 的辦法之前先顯示在頁面中編碼的訊息內容。 這是防止指令碼資料隱碼攻擊的方式。

此程式碼與中樞開啟的連接。

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> 這個方法可確保您的事件處理常式執行之前時，才建立連線。

程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**[聊天室] 頁面中的按鈕。

## <a name="additional-resources"></a>其他資源

如需 SignalR 相關資訊，請參閱下列資源：

* [SignalR 專案](http://signalr.net)

* [SignalR GitHub 和範例](https://github.com/SignalR/SignalR)

* [SignalR Wiki](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a>後續步驟

在本教學課程中，您：

> [!div class="checklist"]
> * 設定專案
> * 執行範例
> * 檢查程式碼

請前往下一篇文章，以了解如何建立使用 ASP.NET SignalR 2 以提供高頻率的傳訊功能的 web 應用程式。
> [!div class="nextstepaction"]
> [Web 應用程式具有高頻率的訊息](tutorial-high-frequency-realtime-with-signalr.md)