---
title: "開始使用 ASP.NET Core 的 SignalR"
author: rachelappel
description: "了解建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。"
manager: wpickett
ms.author: rachelap
ms.custom: mvc
ms.date: 03/06/2018
ms.prod: aspnet-core
ms.technology: dotnet-signalr
ms.topic: tutorial
uid: signalr/get-started-signalr-core
ms.openlocfilehash: 79af59fc8c2ada71d764ada95a431e10f4f00f27
ms.sourcegitcommit: 493a215355576cfa481773365de021bcf04bb9c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/15/2018
---
# <a name="tutorial-get-started-with-signalr-for-aspnet-core"></a><span data-ttu-id="d7eae-103">教學課程： 開始使用 SignalR for ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="d7eae-103">Tutorial: Get started with SignalR for ASP.NET Core</span></span>

<span data-ttu-id="d7eae-104">作者：[Rachel Appel](https://twitter.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="d7eae-104">By [Rachel Appel](https://twitter.com/rachelappel)</span></span>

<span data-ttu-id="d7eae-105">本教學課程將教導您建置適用於 ASP.NET Core 使用 SignalR 的即時應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="d7eae-105">This tutorial teaches the basics of building a real-time app using SignalR for ASP.NET Core.</span></span>

   ![方案](get-started-signalr-core/_static/signalr-get-started-finished.png)

<span data-ttu-id="d7eae-107">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="d7eae-107">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/signalr/get-started-signalr-core/sample/) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

<span data-ttu-id="d7eae-108">本教學課程將示範下列 SignalR 開發工作：</span><span class="sxs-lookup"><span data-stu-id="d7eae-108">This tutorial demonstrates the following SignalR development tasks:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="d7eae-109">建立 ASP.NET Core web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d7eae-109">Create an ASP.NET Core web app.</span></span>
> * <span data-ttu-id="d7eae-110">建立 SignalR 中樞內容推播至用戶端。</span><span class="sxs-lookup"><span data-stu-id="d7eae-110">Create a SignalR hub to push content to clients.</span></span>
> * <span data-ttu-id="d7eae-111">您可以使用 SignalR JavaScript 程式庫來傳送訊息，並顯示從中樞的更新。</span><span class="sxs-lookup"><span data-stu-id="d7eae-111">Use the SignalR JavaScript library to send messages and display updates from the hub.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="d7eae-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="d7eae-112">Prerequisites</span></span>

<span data-ttu-id="d7eae-113">安裝下列軟體：</span><span class="sxs-lookup"><span data-stu-id="d7eae-113">Install the following software:</span></span>

* <span data-ttu-id="d7eae-114">[.NET core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1)或更新版本</span><span class="sxs-lookup"><span data-stu-id="d7eae-114">[.NET Core 2.1.0 Preview 1 SDK](https://www.microsoft.com/net/download/dotnet-core/sdk-2.1.300-preview1) or later</span></span>
* <span data-ttu-id="d7eae-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.6 或更新與 ASP.NET 及 web 程式開發工作負載版本</span><span class="sxs-lookup"><span data-stu-id="d7eae-115">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.6 or later with the ASP.NET and web development workload</span></span>
* [<span data-ttu-id="d7eae-116">npm</span><span class="sxs-lookup"><span data-stu-id="d7eae-116">npm</span></span>](https://www.npmjs.com/get-npm)

## <a name="create-an-aspnet-core-project-that-hosts-signalr-client-and-server"></a><span data-ttu-id="d7eae-117">建立 SignalR 用戶端和伺服器所裝載的 ASP.NET Core 專案</span><span class="sxs-lookup"><span data-stu-id="d7eae-117">Create an ASP.NET Core project that hosts SignalR client and server</span></span>

1. <span data-ttu-id="d7eae-118">使用**檔案** > **新專案**功能表選項，然後選擇**ASP.NET Core Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="d7eae-118">Use the **File** > **New Project** menu option and choose **ASP.NET Core Web Application**.</span></span> <span data-ttu-id="d7eae-119">將專案命名為 `SignalRChat`。</span><span class="sxs-lookup"><span data-stu-id="d7eae-119">Name the project `SignalRChat`.</span></span>

  ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started-signalr-core/_static/signalr-new-project-dialog.png)

2. <span data-ttu-id="d7eae-121">選取**Web 應用程式**建立使用 Razor 頁面專案。</span><span class="sxs-lookup"><span data-stu-id="d7eae-121">Select **Web Application** to create a project using Razor Pages.</span></span> <span data-ttu-id="d7eae-122">然後選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="d7eae-122">Then select **Ok**.</span></span> <span data-ttu-id="d7eae-123">確定**ASP.NET Core 2.1**雖然 SignalR 執行較舊版本的.NET framework 選取器中選取。</span><span class="sxs-lookup"><span data-stu-id="d7eae-123">Be sure that **ASP.NET Core 2.1** is selected from the framework selector, though SignalR runs on older versions of .NET.</span></span>

  ![在 Visual Studio 中的 [新增專案] 對話方塊](get-started-signalr-core/_static/signalr-new-project-choose-type.png)

  <span data-ttu-id="d7eae-125">裝載 SignalR 的伺服器端程式碼的程式庫會包含在專案範本。</span><span class="sxs-lookup"><span data-stu-id="d7eae-125">The libraries that host SignalR server-side code are included in the project template.</span></span> <span data-ttu-id="d7eae-126">安裝個別的用戶端 JavaScript [npm](https://www.npmjs.com/)。</span><span class="sxs-lookup"><span data-stu-id="d7eae-126">Install the client-side JavaScript separately with [npm](https://www.npmjs.com/).</span></span>

  ```console
   npm install @aspnet/signalr
  ```

3. <span data-ttu-id="d7eae-127">複製*signalr.js*從*node_modules\\ @aspnet\signalr\dist\browser* 至*wwwroot\lib*專案資料夾中的。</span><span class="sxs-lookup"><span data-stu-id="d7eae-127">Copy the *signalr.js* from *node_modules\\@aspnet\signalr\dist\browser* to the *wwwroot\lib* folder in your project.</span></span>

## <a name="create-the-signalr-hub"></a><span data-ttu-id="d7eae-128">建立 SignalR 中樞</span><span class="sxs-lookup"><span data-stu-id="d7eae-128">Create the SignalR Hub</span></span>

<span data-ttu-id="d7eae-129">集線器是做為概要管線可讓用戶端與伺服器彼此呼叫方法的類別。</span><span class="sxs-lookup"><span data-stu-id="d7eae-129">A hub is a class that serves as a high-level pipeline that allows the client and server to call methods on each other.</span></span>

1. <span data-ttu-id="d7eae-130">將類別加入專案中，選擇**檔案** > **新增** > **檔案**，然後選取**Visual C# 類別**。</span><span class="sxs-lookup"><span data-stu-id="d7eae-130">Add a class to the project by choosing **File** > **New** > **File** and selecting **Visual C# Class**.</span></span> 

1. <span data-ttu-id="d7eae-131">繼承自`Microsoft.AspNetCore.SignalR.Hub`。</span><span class="sxs-lookup"><span data-stu-id="d7eae-131">Inherit from `Microsoft.AspNetCore.SignalR.Hub`.</span></span> <span data-ttu-id="d7eae-132">`Hub`類別包含屬性和事件管理連接和群組，以及傳送和接收資料。</span><span class="sxs-lookup"><span data-stu-id="d7eae-132">The `Hub` class contains properties and events for managing connections and groups, as well as sending and receiving data.</span></span>

1. <span data-ttu-id="d7eae-133">建立`Send`連接的交談的所有用戶端傳送訊息的方法。</span><span class="sxs-lookup"><span data-stu-id="d7eae-133">Create the `Send` method that sends a message to all connected chat clients.</span></span> <span data-ttu-id="d7eae-134">請注意，它會傳回`Task`，因為 SignalR 是非同步。</span><span class="sxs-lookup"><span data-stu-id="d7eae-134">Notice it returns a `Task`, because SignalR is asynchronous.</span></span> <span data-ttu-id="d7eae-135">非同步程式碼較易調整大小。</span><span class="sxs-lookup"><span data-stu-id="d7eae-135">Asynchronous code scales better.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Hubs/ChatHub.cs?range=7-14)]

## <a name="configure-the-project-to-use-signalr"></a><span data-ttu-id="d7eae-136">設定專案以使用 SignalR</span><span class="sxs-lookup"><span data-stu-id="d7eae-136">Configure the project to use SignalR</span></span>

<span data-ttu-id="d7eae-137">SignalR 伺服器必須設定，讓它知道要傳遞到 SignalR 要求。</span><span class="sxs-lookup"><span data-stu-id="d7eae-137">The SignalR server must be configured so that it knows to pass requests to SignalR.</span></span>

1. <span data-ttu-id="d7eae-138">若要設定 SignalR 專案，請修改`ConfigureServices`應用程式的方法`Startup`類別藉由將呼叫`services.AddSignalR`。</span><span class="sxs-lookup"><span data-stu-id="d7eae-138">To configure a SignalR project, modify the `ConfigureServices` method of the application's `Startup` class by inserting a call to `services.AddSignalR`.</span></span>

  <span data-ttu-id="d7eae-139">`services.AddSignalR` 將 SignalR 的一部分[ASP.NET Core 中介軟體](xref:fundamentals/middleware/index)管線。</span><span class="sxs-lookup"><span data-stu-id="d7eae-139">`services.AddSignalR` adds SignalR as part of the [ASP.NET Core middleware](xref:fundamentals/middleware/index) pipeline.</span></span>

1. <span data-ttu-id="d7eae-140">設定路由，以便您使用的中樞`UseSignalR`。</span><span class="sxs-lookup"><span data-stu-id="d7eae-140">Configure routes to your hubs using `UseSignalR`.</span></span>

  [!code-csharp[Startup](get-started-signalr-core/sample/Startup.cs?highlight=22,40-43)]

## <a name="create-the-signalr-client-code"></a><span data-ttu-id="d7eae-141">建立 SignalR 用戶端程式碼</span><span class="sxs-lookup"><span data-stu-id="d7eae-141">Create the SignalR client code</span></span>

1. <span data-ttu-id="d7eae-142">取代中的內容*Pages\Index.cshtml*為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7eae-142">Replace the content in *Pages\Index.cshtml* with the following code:</span></span>

  [!code-cshtml[Index](get-started-signalr-core/sample/Pages/Index.cshtml)]

  <span data-ttu-id="d7eae-143">前面的 HTML 顯示名稱和訊息欄位及提交按鈕。</span><span class="sxs-lookup"><span data-stu-id="d7eae-143">The preceding HTML displays name and message fields, and a submit button.</span></span> <span data-ttu-id="d7eae-144">請注意底部的指令碼參考： SignalR 的參考和*chat.js*。</span><span class="sxs-lookup"><span data-stu-id="d7eae-144">Notice the script references at the bottom: a reference to SignalR and *chat.js*.</span></span>

1. <span data-ttu-id="d7eae-145">加入 JavaScript 檔案*wwwroot\js*資料夾名為*chat.js*並加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="d7eae-145">Add a JavaScript file to the *wwwroot\js* folder named *chat.js* and add the following code to it:</span></span>

  [!code-javascript[Index](get-started-signalr-core/sample/wwwroot/js/chat.js)]

## <a name="run-the-app"></a><span data-ttu-id="d7eae-146">執行應用程式</span><span class="sxs-lookup"><span data-stu-id="d7eae-146">Run the app</span></span>

1. <span data-ttu-id="d7eae-147">選取**偵錯** > **啟動但不偵錯**啟動瀏覽器，並載入本機網站。</span><span class="sxs-lookup"><span data-stu-id="d7eae-147">Select **Debug** > **Start without debugging** to launch a browser and load the website locally.</span></span> <span data-ttu-id="d7eae-148">從網址列複製 URL。</span><span class="sxs-lookup"><span data-stu-id="d7eae-148">Copy the URL from the address bar.</span></span>

1. <span data-ttu-id="d7eae-149">開啟另一個瀏覽器執行個體 （任何瀏覽器），並在網址列中貼上 URL。</span><span class="sxs-lookup"><span data-stu-id="d7eae-149">Open another browser instance (any browser) and paste the URL in the address bar.</span></span>

1. <span data-ttu-id="d7eae-150">選擇任一個瀏覽器，輸入名稱和訊息，然後按一下**傳送** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="d7eae-150">Choose either browser, enter a name and message, and click the **Send** button.</span></span> <span data-ttu-id="d7eae-151">名稱和訊息會顯示兩個頁面上立即。</span><span class="sxs-lookup"><span data-stu-id="d7eae-151">The name and message are displayed on both pages instantly.</span></span>

  ![方案](get-started-signalr-core/_static/signalr-get-started-finished.png)
