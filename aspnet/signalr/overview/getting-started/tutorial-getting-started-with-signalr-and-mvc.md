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
# <a name="tutorial-real-time-chat-with-signalr-2-and-mvc-5"></a><span data-ttu-id="d4d99-104">教學課程：即時聊天與 SignalR 2 和 MVC 5</span><span class="sxs-lookup"><span data-stu-id="d4d99-104">Tutorial: Real-time chat with SignalR 2 and MVC 5</span></span>

<span data-ttu-id="d4d99-105">本教學課程會示範如何使用 ASP.NET SignalR 2 建立即時聊天應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-105">This tutorial shows how to use ASP.NET SignalR 2 to create a real-time chat application.</span></span> <span data-ttu-id="d4d99-106">您加入 MVC 5 應用程式中的 SignalR 和建立交談檢視，以傳送，並顯示訊息。</span><span class="sxs-lookup"><span data-stu-id="d4d99-106">You add SignalR to an MVC 5 application and create a chat view to send and display messages.</span></span>

<span data-ttu-id="d4d99-107">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="d4d99-107">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d4d99-108">設定專案</span><span class="sxs-lookup"><span data-stu-id="d4d99-108">Set up the project</span></span>
> * <span data-ttu-id="d4d99-109">執行範例</span><span class="sxs-lookup"><span data-stu-id="d4d99-109">Run the sample</span></span>
> * <span data-ttu-id="d4d99-110">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="d4d99-110">Examine the code</span></span>

[!INCLUDE [Consider ASP.NET Core SignalR](~/includes/signalr/signalr-version-disambiguation.md)]

## <a name="prerequisites"></a><span data-ttu-id="d4d99-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="d4d99-111">Prerequisites</span></span>

* <span data-ttu-id="d4d99-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/)具有**ASP.NET 和 web 開發**工作負載。</span><span class="sxs-lookup"><span data-stu-id="d4d99-112">[Visual Studio 2017](https://visualstudio.microsoft.com/downloads/) with the **ASP.NET and web development** workload.</span></span>

## <a name="set-up-the-project"></a><span data-ttu-id="d4d99-113">設定專案</span><span class="sxs-lookup"><span data-stu-id="d4d99-113">Set up the Project</span></span>

<span data-ttu-id="d4d99-114">本節說明如何使用 Visual Studio 2017 與 SignalR 2 來建立空的 ASP.NET MVC 5 應用程式、 新增 SignalR 程式庫，並建立交談應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-114">This section shows how to use Visual Studio 2017 and SignalR 2 to create an empty ASP.NET MVC 5 application, add the SignalR library, and create the chat application.</span></span>

1. <span data-ttu-id="d4d99-115">在 Visual Studio 中建立.NET Framework 4.5 為目標的 C# ASP.NET 應用程式、 SignalRChat，將它命名，並按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="d4d99-115">In Visual Studio, create a C# ASP.NET application that targets .NET Framework 4.5, name it SignalRChat, and click OK.</span></span>

    ![建立 web](tutorial-getting-started-with-signalr-and-mvc/_static/image1.png)

1. <span data-ttu-id="d4d99-117">在 **新增 ASP.NET Web 應用程式-SignalRMvcChat**，選取**MVC** ，然後選取**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-117">In **New ASP.NET Web Application - SignalRMvcChat**, select **MVC** and then select **Change Authentication**.</span></span>

1. <span data-ttu-id="d4d99-118">在 **變更驗證**，選取**不需要驗證**，按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-118">In **Change Authentication**, select **No Authentication** and click **OK**.</span></span>

    ![選取 不驗證](tutorial-getting-started-with-signalr-and-mvc/_static/image2.png)

1. <span data-ttu-id="d4d99-120">在 **新增 ASP.NET Web 應用程式-SignalRMvcChat**，選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-120">In **New ASP.NET Web Application - SignalRMvcChat**, select **OK**.</span></span>

1. <span data-ttu-id="d4d99-121">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **新項目**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-121">In **Solution Explorer**, right-click the project and select **Add** > **New Item**.</span></span>

1. <span data-ttu-id="d4d99-122">在 **加入新項目-SignalRChat**，選取**已安裝** > **Visual C#**   >  **Web**  > **SignalR** ，然後選取**SignalR Hub 類別 (v2)**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-122">In **Add New Item - SignalRChat**, select **Installed** > **Visual C#** > **Web** > **SignalR**  and then select **SignalR Hub Class (v2)**.</span></span>

1. <span data-ttu-id="d4d99-123">將類別命名為*ChatHub*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="d4d99-123">Name the class *ChatHub* and add it to the project.</span></span>

    <span data-ttu-id="d4d99-124">這個步驟會建立*ChatHub.cs*類別檔案，以及新增一組指令碼檔案和專案支援 SignalR 的組件參考。</span><span class="sxs-lookup"><span data-stu-id="d4d99-124">This step creates the *ChatHub.cs* class file and adds a set of script files and assembly references that support SignalR to the project.</span></span>

1. <span data-ttu-id="d4d99-125">在新程式碼取代*ChatHub.cs*類別檔案，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d4d99-125">Replace the code in the new *ChatHub.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample1.cs)]

1. <span data-ttu-id="d4d99-126">在 **方案總管**，以滑鼠右鍵按一下專案，然後選取**新增** > **類別**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-126">In **Solution Explorer**, right-click the project and select **Add** > **Class**.</span></span>

1. <span data-ttu-id="d4d99-127">新類別命名*啟動*並將它新增至專案。</span><span class="sxs-lookup"><span data-stu-id="d4d99-127">Name the new class *Startup* and add it to the project.</span></span>

1. <span data-ttu-id="d4d99-128">中的程式碼取代*Startup.cs*類別檔案，以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d4d99-128">Replace the code in the *Startup.cs* class file with this code:</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample2.cs)]

1. <span data-ttu-id="d4d99-129">在 **方案總管**，選取**控制站** > **HomeController.cs**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-129">In **Solution Explorer**, select **Controllers** > **HomeController.cs**.</span></span>

1. <span data-ttu-id="d4d99-130">將下列方法來新增*HomeController.cs*。</span><span class="sxs-lookup"><span data-stu-id="d4d99-130">Add this method to the *HomeController.cs*.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample3.cs)]

    <span data-ttu-id="d4d99-131">這個方法會傳回**聊天**您在稍後步驟中建立的檢視。</span><span class="sxs-lookup"><span data-stu-id="d4d99-131">This method returns the **Chat** view that you create in a later step.</span></span>

1. <span data-ttu-id="d4d99-132">中**方案總管**，以滑鼠右鍵按一下**檢視** > **首頁**，然後選取**新增** >   **檢視**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-132">In **Solution Explorer**, right-click **Views** > **Home**, and select **Add** >  **View**.</span></span>

1. <span data-ttu-id="d4d99-133">在**加入檢視**，將新檢視**聊天**，然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-133">In **Add View**, name the new view **Chat** and select **Add**.</span></span>

1. <span data-ttu-id="d4d99-134">內容取代**Chat.cshtml**以下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d4d99-134">Replace the contents of **Chat.cshtml** with this code:</span></span>

    [!code-cshtml[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample4.cshtml)]

1. <span data-ttu-id="d4d99-135">在 **方案總管**，展開**指令碼**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-135">In **Solution Explorer**, expand **Scripts**.</span></span>

    <span data-ttu-id="d4d99-136">JQuery 和 SignalR 的指令碼程式庫會顯示在專案中。</span><span class="sxs-lookup"><span data-stu-id="d4d99-136">Script libraries for jQuery and SignalR are visible in the project.</span></span>

    > [!IMPORTANT]
    > <span data-ttu-id="d4d99-137">封裝管理員可能已安裝較新版 SignalR 指令碼。</span><span class="sxs-lookup"><span data-stu-id="d4d99-137">The package manager may have installed a later version of the SignalR scripts.</span></span>

1. <span data-ttu-id="d4d99-138">請檢查程式碼區塊中的指令碼參考對應至專案中的指令碼檔案的版本。</span><span class="sxs-lookup"><span data-stu-id="d4d99-138">Check that the script references in the code block correspond to the versions of the script files in the project.</span></span>

    <span data-ttu-id="d4d99-139">從原始的程式碼區塊的指令碼參考：</span><span class="sxs-lookup"><span data-stu-id="d4d99-139">Script references from the original code block:</span></span>

    ```cshtml
    <!--Script references. -->
    <!--The jQuery library is required and is referenced by default in _Layout.cshtml. -->
    <!--Reference the SignalR library. -->
    <script src="~/Scripts/jquery.signalR-2.1.0.min.js"></script>
    ```

1. <span data-ttu-id="d4d99-140">如果兩者不符，更新 *.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="d4d99-140">If they don't match, update the *.cshtml* file.</span></span>

1. <span data-ttu-id="d4d99-141">從功能表列中，選取**檔案** > **全部儲存**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-141">From the menu bar, select **File** > **Save All**.</span></span>

## <a name="run-the-sample"></a><span data-ttu-id="d4d99-142">執行範例</span><span class="sxs-lookup"><span data-stu-id="d4d99-142">Run the Sample</span></span>

1. <span data-ttu-id="d4d99-143">在工具列中，開啟**指令碼偵錯**，然後選取 偵錯模式中執行範例的 播放 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d4d99-143">In the toolbar, turn on **Script Debugging** and then select the play button to run the sample in Debug mode.</span></span>

    ![輸入使用者名稱](tutorial-getting-started-with-signalr-and-mvc/_static/image3.png)

1. <span data-ttu-id="d4d99-145">瀏覽器開啟時，輸入您的對談身分識別的名稱。</span><span class="sxs-lookup"><span data-stu-id="d4d99-145">When the browser opens, enter a name for your chat identity.</span></span>

1. <span data-ttu-id="d4d99-146">從瀏覽器複製 URL，開啟兩個其他瀏覽器，並將 Url 貼到 位址列。</span><span class="sxs-lookup"><span data-stu-id="d4d99-146">Copy the URL from the browser, open two other browsers, and paste the URLs into the address bars.</span></span>

1. <span data-ttu-id="d4d99-147">在每個瀏覽器中，輸入唯一的名稱。</span><span class="sxs-lookup"><span data-stu-id="d4d99-147">In each browser, enter a unique name.</span></span>

1. <span data-ttu-id="d4d99-148">現在，加入 註解，然後選取**傳送**。</span><span class="sxs-lookup"><span data-stu-id="d4d99-148">Now, add a comment and select **Send**.</span></span> <span data-ttu-id="d4d99-149">您可以重複，在其他瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d4d99-149">Repeat that in the other browsers.</span></span> <span data-ttu-id="d4d99-150">註解會出現在即時。</span><span class="sxs-lookup"><span data-stu-id="d4d99-150">The comments appear in real time.</span></span>

    > [!NOTE]
    > <span data-ttu-id="d4d99-151">這個簡單的聊天應用程式不會維護伺服器上的討論內容。</span><span class="sxs-lookup"><span data-stu-id="d4d99-151">This simple chat application does not maintain the discussion context on the server.</span></span> <span data-ttu-id="d4d99-152">中樞會廣播到所有目前使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="d4d99-152">The hub broadcasts comments to all current users.</span></span> <span data-ttu-id="d4d99-153">稍後加入聊天室使用者會看到訊息的時間加入它們加入。</span><span class="sxs-lookup"><span data-stu-id="d4d99-153">Users who join the chat later will see messages added from the time they join.</span></span>

    <span data-ttu-id="d4d99-154">了解交談應用程式在三個不同的瀏覽器中執行時。</span><span class="sxs-lookup"><span data-stu-id="d4d99-154">See how the chat application runs in three different browsers.</span></span> <span data-ttu-id="d4d99-155">當 Tom、 Anand 和 Susan 傳送訊息時，即時更新所有瀏覽器：</span><span class="sxs-lookup"><span data-stu-id="d4d99-155">When Tom, Anand, and Susan send messages, all browsers update in real time:</span></span>

    ![所有的三個瀏覽器會顯示相同的交談記錄](tutorial-getting-started-with-signalr-and-mvc/_static/image4.png)

1. <span data-ttu-id="d4d99-157">在 **方案總管**，檢查**指令碼文件**節點執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-157">In **Solution Explorer**, inspect the **Script Documents** node for the running application.</span></span> <span data-ttu-id="d4d99-158">沒有名為的指令碼檔案*中樞*SignalR 程式庫產生在執行階段。</span><span class="sxs-lookup"><span data-stu-id="d4d99-158">There's a script file named *hubs* that the SignalR library generates at runtime.</span></span> <span data-ttu-id="d4d99-159">此檔案會管理 jQuery 指令碼和伺服器端程式碼之間的通訊。</span><span class="sxs-lookup"><span data-stu-id="d4d99-159">This file manages the communication between jQuery script and server-side code.</span></span>

    ![在 [指令碼文件] 節點中的自動產生中樞指令碼](tutorial-getting-started-with-signalr-and-mvc/_static/image5.png)

## <a name="examine-the-code"></a><span data-ttu-id="d4d99-161">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="d4d99-161">Examine the Code</span></span>

<span data-ttu-id="d4d99-162">SignalR 交談應用程式會示範兩個基本的 SignalR 開發工作。</span><span class="sxs-lookup"><span data-stu-id="d4d99-162">The SignalR chat application demonstrates two basic SignalR development tasks.</span></span> <span data-ttu-id="d4d99-163">它說明如何建立中樞。</span><span class="sxs-lookup"><span data-stu-id="d4d99-163">It shows you how to create a hub.</span></span> <span data-ttu-id="d4d99-164">伺服器會使用該中樞做為主要協調物件。</span><span class="sxs-lookup"><span data-stu-id="d4d99-164">The server uses that hub as the main coordination object.</span></span> <span data-ttu-id="d4d99-165">中樞會使用 SignalR jQuery 程式庫，來傳送和接收訊息。</span><span class="sxs-lookup"><span data-stu-id="d4d99-165">The hub uses the SignalR jQuery library to send and receive messages.</span></span>

### <a name="signalr-hubs-in-the-chathubcs"></a><span data-ttu-id="d4d99-166">在 ChatHub.cs SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d4d99-166">SignalR Hubs in the ChatHub.cs</span></span>

<span data-ttu-id="d4d99-167">在程式碼範例中，`ChatHub`類別衍生自`Microsoft.AspNet.SignalR.Hub`類別。</span><span class="sxs-lookup"><span data-stu-id="d4d99-167">In the code sample, the `ChatHub` class derives from the `Microsoft.AspNet.SignalR.Hub` class.</span></span> <span data-ttu-id="d4d99-168">衍生自`Hub`類別是實用的方式，來建置 SignalR 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-168">Deriving from the `Hub` class is a useful way to build a SignalR application.</span></span> <span data-ttu-id="d4d99-169">您可以建立中樞類別上的公用方法，並從網頁中的指令碼的方式呼叫它們，以存取這些方法。</span><span class="sxs-lookup"><span data-stu-id="d4d99-169">You can create public methods on your hub class and then access those methods by calling them from scripts in a web page.</span></span>

<span data-ttu-id="d4d99-170">在對談程式碼中，用戶端呼叫`ChatHub.Send`方法以傳送新訊息。</span><span class="sxs-lookup"><span data-stu-id="d4d99-170">In the chat code, clients call the `ChatHub.Send` method to send a new message.</span></span> <span data-ttu-id="d4d99-171">中樞接著將訊息傳送至所有用戶端藉由呼叫`Clients.All.addNewMessageToPage`。</span><span class="sxs-lookup"><span data-stu-id="d4d99-171">The hub in turn sends the message to all clients by calling `Clients.All.addNewMessageToPage`.</span></span>

<span data-ttu-id="d4d99-172">`Send`方法將示範數個中樞概念：</span><span class="sxs-lookup"><span data-stu-id="d4d99-172">The `Send` method demonstrates several hub concepts:</span></span>

* <span data-ttu-id="d4d99-173">可讓用戶端呼叫，則請在中樞中宣告的公用方法。</span><span class="sxs-lookup"><span data-stu-id="d4d99-173">Declare public methods on a hub so that clients can call them.</span></span>

* <span data-ttu-id="d4d99-174">使用`Microsoft.AspNet.SignalR.Hub.Clients`此中樞的連線與所有的用戶端通訊的動態屬性。</span><span class="sxs-lookup"><span data-stu-id="d4d99-174">Use the `Microsoft.AspNet.SignalR.Hub.Clients` dynamic property to communicate with all clients connected to this hub.</span></span>

* <span data-ttu-id="d4d99-175">在用戶端上呼叫的函式 (例如`addNewMessageToPage`函式) 來更新用戶端。</span><span class="sxs-lookup"><span data-stu-id="d4d99-175">Call a function on the client (like the `addNewMessageToPage` function) to update clients.</span></span>

    [!code-csharp[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample5.cs)]

### <a name="signalr-and-jquery-chatcshtml"></a><span data-ttu-id="d4d99-176">SignalR 和 jQuery Chat.cshtml</span><span class="sxs-lookup"><span data-stu-id="d4d99-176">SignalR and jQuery Chat.cshtml</span></span>

<span data-ttu-id="d4d99-177">*Chat.cshtml*檢視檔案中的程式碼範例示範如何使用 SignalR jQuery 程式庫與 SignalR 中樞進行通訊。</span><span class="sxs-lookup"><span data-stu-id="d4d99-177">The *Chat.cshtml* view file in the code sample shows how to use the SignalR jQuery library to communicate with a SignalR hub.</span></span>  <span data-ttu-id="d4d99-178">程式碼會執行許多重要的工作。</span><span class="sxs-lookup"><span data-stu-id="d4d99-178">The code carries out many important tasks.</span></span> <span data-ttu-id="d4d99-179">它會建立中樞自動產生 proxy 的參考、 宣告的函式，伺服器可以呼叫以將內容推送至用戶端，同時也會將訊息傳送至中樞的連接。</span><span class="sxs-lookup"><span data-stu-id="d4d99-179">It creates a reference to the autogenerated proxy for the hub, declares a function that the server can call to push content to clients, and it starts a connection to send messages to the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample6.js)]

> [!NOTE]
> <span data-ttu-id="d4d99-180">在 JavaScript 中，伺服器類別和其成員的參考會處於 camelCase。</span><span class="sxs-lookup"><span data-stu-id="d4d99-180">In JavaScript, the reference to the server class and its members is in camelCase.</span></span> <span data-ttu-id="d4d99-181">程式碼範例參考C#`ChatHub`在 JavaScript 中的類別`chatHub`。</span><span class="sxs-lookup"><span data-stu-id="d4d99-181">The code sample references the C# `ChatHub` class in JavaScript as `chatHub`.</span></span>

<span data-ttu-id="d4d99-182">在此程式碼區塊中，您可以建立指令碼中的回呼函式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-182">In this code block, you create a callback function in the script.</span></span>

[!code-html[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample7.html)]

<span data-ttu-id="d4d99-183">在伺服器上的中樞類別會呼叫此函式可將內容更新推送至每個用戶端。</span><span class="sxs-lookup"><span data-stu-id="d4d99-183">The hub class on the server calls this function to push content updates to each client.</span></span> <span data-ttu-id="d4d99-184">選擇性呼叫`htmlEncode`函式會顯示為 HTML 的辦法之前先顯示在頁面中編碼的訊息內容。</span><span class="sxs-lookup"><span data-stu-id="d4d99-184">The optional call to the `htmlEncode` function shows a way to HTML encode the message content before displaying it in the page.</span></span> <span data-ttu-id="d4d99-185">這是防止指令碼資料隱碼攻擊的方式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-185">It's a way to prevent script injection.</span></span>

<span data-ttu-id="d4d99-186">此程式碼與中樞開啟的連接。</span><span class="sxs-lookup"><span data-stu-id="d4d99-186">This code opens a connection with the hub.</span></span>

[!code-javascript[Main](tutorial-getting-started-with-signalr-and-mvc/samples/sample8.js)]

> [!NOTE]
> <span data-ttu-id="d4d99-187">這個方法可確保您的事件處理常式執行之前時，才建立連線。</span><span class="sxs-lookup"><span data-stu-id="d4d99-187">This approach ensures that you establish a connection before the event handler executes.</span></span>

<span data-ttu-id="d4d99-188">程式碼啟動連線並再將它傳遞至 click 事件處理函式**傳送**[聊天室] 頁面中的按鈕。</span><span class="sxs-lookup"><span data-stu-id="d4d99-188">The code starts the connection and then passes it a function to handle the click event on the **Send** button in the Chat page.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="d4d99-189">其他資源</span><span class="sxs-lookup"><span data-stu-id="d4d99-189">Additional resources</span></span>

<span data-ttu-id="d4d99-190">如需 SignalR 相關資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="d4d99-190">For more about SignalR, see the following resources:</span></span>

* [<span data-ttu-id="d4d99-191">SignalR 專案</span><span class="sxs-lookup"><span data-stu-id="d4d99-191">SignalR Project</span></span>](http://signalr.net)

* [<span data-ttu-id="d4d99-192">SignalR GitHub 和範例</span><span class="sxs-lookup"><span data-stu-id="d4d99-192">SignalR GitHub and Samples</span></span>](https://github.com/SignalR/SignalR)

* [<span data-ttu-id="d4d99-193">SignalR Wiki</span><span class="sxs-lookup"><span data-stu-id="d4d99-193">SignalR Wiki</span></span>](https://github.com/SignalR/SignalR/wiki)

## <a name="next-steps"></a><span data-ttu-id="d4d99-194">後續步驟</span><span class="sxs-lookup"><span data-stu-id="d4d99-194">Next steps</span></span>

<span data-ttu-id="d4d99-195">在本教學課程中，您：</span><span class="sxs-lookup"><span data-stu-id="d4d99-195">In this tutorial, you:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d4d99-196">設定專案</span><span class="sxs-lookup"><span data-stu-id="d4d99-196">Set up the project</span></span>
> * <span data-ttu-id="d4d99-197">執行範例</span><span class="sxs-lookup"><span data-stu-id="d4d99-197">Ran the sample</span></span>
> * <span data-ttu-id="d4d99-198">檢查程式碼</span><span class="sxs-lookup"><span data-stu-id="d4d99-198">Examined the code</span></span>

<span data-ttu-id="d4d99-199">請前往下一篇文章，以了解如何建立使用 ASP.NET SignalR 2 以提供高頻率的傳訊功能的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4d99-199">Advance to the next article to learn how to create a web application that uses ASP.NET SignalR 2 to provide high-frequency messaging functionality.</span></span>
> [!div class="nextstepaction"]
> [<span data-ttu-id="d4d99-200">Web 應用程式具有高頻率的訊息</span><span class="sxs-lookup"><span data-stu-id="d4d99-200">Web app with high-frequency messaging</span></span>](tutorial-high-frequency-realtime-with-signalr.md)