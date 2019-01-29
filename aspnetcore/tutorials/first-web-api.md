---
title: 教學課程：使用 ASP.NET Core MVC 建立 Web API
author: rick-anderson
description: 使用 ASP.NET Core MVC 建立 Web API
ms.author: riande
ms.custom: mvc
ms.date: 01/24/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 65af70be2cec68d30dd712b80312ebcd40ea0624
ms.sourcegitcommit: c6db8b14521814f1f7e528d7aa06e474e4c04a1f
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/25/2019
ms.locfileid: "55065044"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core-mvc"></a><span data-ttu-id="eb1a5-103">教學課程：使用 ASP.NET Core MVC 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="eb1a5-103">Tutorial: Create a web API with ASP.NET Core MVC</span></span>

<span data-ttu-id="eb1a5-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="eb1a5-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="eb1a5-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="eb1a5-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb1a5-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-107">Create a web API project.</span></span>
> * <span data-ttu-id="eb1a5-108">新增模型類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-108">Add a model class.</span></span>
> * <span data-ttu-id="eb1a5-109">建立資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-109">Create the database context.</span></span>
> * <span data-ttu-id="eb1a5-110">註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-110">Register the database context.</span></span>
> * <span data-ttu-id="eb1a5-111">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-111">Add a controller.</span></span>
> * <span data-ttu-id="eb1a5-112">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-112">Add CRUD methods.</span></span>
> * <span data-ttu-id="eb1a5-113">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-113">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="eb1a5-114">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-114">Specify return values.</span></span>
> * <span data-ttu-id="eb1a5-115">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-115">Call the web API with Postman.</span></span>
> * <span data-ttu-id="eb1a5-116">使用 jQuery 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-116">Call the web API with jQuery.</span></span>

<span data-ttu-id="eb1a5-117">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-117">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="eb1a5-118">總覽</span><span class="sxs-lookup"><span data-stu-id="eb1a5-118">Overview</span></span>

<span data-ttu-id="eb1a5-119">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-119">This tutorial creates the following API:</span></span>

|<span data-ttu-id="eb1a5-120">API</span><span class="sxs-lookup"><span data-stu-id="eb1a5-120">API</span></span> | <span data-ttu-id="eb1a5-121">說明</span><span class="sxs-lookup"><span data-stu-id="eb1a5-121">Description</span></span> | <span data-ttu-id="eb1a5-122">要求本文</span><span class="sxs-lookup"><span data-stu-id="eb1a5-122">Request body</span></span> | <span data-ttu-id="eb1a5-123">回應本文</span><span class="sxs-lookup"><span data-stu-id="eb1a5-123">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="eb1a5-124">GET /api/todo</span><span class="sxs-lookup"><span data-stu-id="eb1a5-124">GET /api/todo</span></span> | <span data-ttu-id="eb1a5-125">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-125">Get all to-do items</span></span> | <span data-ttu-id="eb1a5-126">無</span><span class="sxs-lookup"><span data-stu-id="eb1a5-126">None</span></span> | <span data-ttu-id="eb1a5-127">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="eb1a5-127">Array of to-do items</span></span>|
|<span data-ttu-id="eb1a5-128">GET /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="eb1a5-128">GET /api/todo/{id}</span></span> | <span data-ttu-id="eb1a5-129">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="eb1a5-129">Get an item by ID</span></span> | <span data-ttu-id="eb1a5-130">無</span><span class="sxs-lookup"><span data-stu-id="eb1a5-130">None</span></span> | <span data-ttu-id="eb1a5-131">待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-131">To-do item</span></span>|
|<span data-ttu-id="eb1a5-132">POST /api/todo</span><span class="sxs-lookup"><span data-stu-id="eb1a5-132">POST /api/todo</span></span> | <span data-ttu-id="eb1a5-133">新增記錄</span><span class="sxs-lookup"><span data-stu-id="eb1a5-133">Add a new item</span></span> | <span data-ttu-id="eb1a5-134">待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-134">To-do item</span></span> | <span data-ttu-id="eb1a5-135">待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-135">To-do item</span></span> |
|<span data-ttu-id="eb1a5-136">PUT /api/todo/{id}</span><span class="sxs-lookup"><span data-stu-id="eb1a5-136">PUT /api/todo/{id}</span></span> | <span data-ttu-id="eb1a5-137">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="eb1a5-137">Update an existing item &nbsp;</span></span> | <span data-ttu-id="eb1a5-138">待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-138">To-do item</span></span> | <span data-ttu-id="eb1a5-139">無</span><span class="sxs-lookup"><span data-stu-id="eb1a5-139">None</span></span> |
|<span data-ttu-id="eb1a5-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="eb1a5-140">DELETE /api/todo/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="eb1a5-141">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="eb1a5-141">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="eb1a5-142">無</span><span class="sxs-lookup"><span data-stu-id="eb1a5-142">None</span></span> | <span data-ttu-id="eb1a5-143">無</span><span class="sxs-lookup"><span data-stu-id="eb1a5-143">None</span></span>|

<span data-ttu-id="eb1a5-144">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-144">The following diagram shows the design of the app.</span></span>

![左側方塊所代表的用戶端會送出要求並接收來自應用程式 (右側繪製的方塊) 的回應。](first-web-api/_static/architecture.png)

[!INCLUDE[](~/includes/net-core-prereqs-all-2.2.md)]

## <a name="create-a-web-project"></a><span data-ttu-id="eb1a5-149">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="eb1a5-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eb1a5-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb1a5-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eb1a5-151">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="eb1a5-152">選取 [ASP.NET Core Web 應用程式] 範本。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-152">Select the **ASP.NET Core Web Application** template.</span></span> <span data-ttu-id="eb1a5-153">將專案命名為 *TodoApi*，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-153">Name the project *TodoApi* and click **OK**.</span></span>
* <span data-ttu-id="eb1a5-154">在 [新增 ASP.NET Core Web 應用程式 - TodoApi] 對話方塊中，選擇 ASP.NET Core 版本。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-154">In the **New ASP.NET Core Web Application - TodoApi** dialog, choose the ASP.NET Core version.</span></span> <span data-ttu-id="eb1a5-155">選取 [API] 範本，然後按一下 [確定]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-155">Select the **API** template and click **OK**.</span></span> <span data-ttu-id="eb1a5-156">請**勿**選取 [Enable Docker Support] (啟用 Docker 支援)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-156">Do **not** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eb1a5-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eb1a5-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="eb1a5-159">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="eb1a5-160">將目錄 (`cd`) 變更為其中包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-160">Change directories (`cd`) to the folder which will contain the project folder.</span></span>
* <span data-ttu-id="eb1a5-161">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="eb1a5-162">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-162">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="eb1a5-163">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-163">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eb1a5-164">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eb1a5-164">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eb1a5-165">選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-165">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="eb1a5-167">選取 [.NET Core 應用程式] > [ASP.NET Core Web API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-167">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="eb1a5-169">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，接受 [目標 Framework] 的預設 \**.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-169">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="eb1a5-170">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-170">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="eb1a5-172">測試 API</span><span class="sxs-lookup"><span data-stu-id="eb1a5-172">Test the API</span></span>

<span data-ttu-id="eb1a5-173">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-173">The project template creates a `values` API.</span></span> <span data-ttu-id="eb1a5-174">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-174">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eb1a5-175">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb1a5-175">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="eb1a5-176">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-176">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="eb1a5-177">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-177">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="eb1a5-178">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-178">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="eb1a5-179">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-179">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eb1a5-180">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eb1a5-180">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="eb1a5-181">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-181">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="eb1a5-182">在瀏覽器中，前往下列 URL：[https://localhost:5001/api/values](https://localhost:5001/api/values)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-182">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eb1a5-183">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eb1a5-183">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="eb1a5-184">選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-184">Select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="eb1a5-185">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-185">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="eb1a5-186">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-186">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="eb1a5-187">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-187">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="eb1a5-188">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-188">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="eb1a5-189">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="eb1a5-189">Add a model class</span></span>

<span data-ttu-id="eb1a5-190">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-190">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="eb1a5-191">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-191">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eb1a5-192">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb1a5-192">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eb1a5-193">在 [方案總管] 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-193">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="eb1a5-194">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-194">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="eb1a5-195">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-195">Name the folder *Models*.</span></span>

* <span data-ttu-id="eb1a5-196">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-196">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="eb1a5-197">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-197">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="eb1a5-198">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-198">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="eb1a5-199">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="eb1a5-199">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="eb1a5-200">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-200">Add a folder named *Models*.</span></span>

* <span data-ttu-id="eb1a5-201">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-201">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="eb1a5-202">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eb1a5-202">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="eb1a5-203">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-203">Right-click the project.</span></span> <span data-ttu-id="eb1a5-204">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-204">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="eb1a5-205">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-205">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="eb1a5-207">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-207">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="eb1a5-208">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-208">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="eb1a5-209">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-209">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="eb1a5-210">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-210">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="eb1a5-211">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-211">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="eb1a5-212">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="eb1a5-212">Add a database context</span></span>

<span data-ttu-id="eb1a5-213">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-213">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="eb1a5-214">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-214">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eb1a5-215">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb1a5-215">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eb1a5-216">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-216">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="eb1a5-217">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-217">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="eb1a5-218">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eb1a5-218">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="eb1a5-219">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-219">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="eb1a5-220">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-220">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="eb1a5-221">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="eb1a5-221">Register the database context</span></span>

<span data-ttu-id="eb1a5-222">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-222">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="eb1a5-223">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-223">The container provides the service to controllers.</span></span>

<span data-ttu-id="eb1a5-224">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-224">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="eb1a5-225">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-225">The preceding code:</span></span>

* <span data-ttu-id="eb1a5-226">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-226">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="eb1a5-227">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-227">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="eb1a5-228">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-228">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="eb1a5-229">新增控制器</span><span class="sxs-lookup"><span data-stu-id="eb1a5-229">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="eb1a5-230">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="eb1a5-230">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="eb1a5-231">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-231">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="eb1a5-232">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-232">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="eb1a5-233">在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-233">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="eb1a5-234">將類別命名為 *TodoController*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-234">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="eb1a5-236">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="eb1a5-236">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="eb1a5-237">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-237">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="eb1a5-238">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-238">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="eb1a5-239">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-239">The preceding code:</span></span>

* <span data-ttu-id="eb1a5-240">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-240">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="eb1a5-241">使用 [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-241">Decorates the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="eb1a5-242">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-242">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="eb1a5-243">如需屬性所啟用特定行為的資訊，請參閱[以 ApiController 屬性標註](xref:web-api/index#annotation-with-apicontroller-attribute)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-243">For information about specific behaviors that the attribute enables, see [Annotation with ApiController attribute](xref:web-api/index#annotation-with-apicontroller-attribute).</span></span>
* <span data-ttu-id="eb1a5-244">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-244">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="eb1a5-245">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-245">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="eb1a5-246">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-246">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="eb1a5-247">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-247">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="eb1a5-248">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-248">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="eb1a5-249">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-249">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="eb1a5-250">新增 Get 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-250">Add Get methods</span></span>

<span data-ttu-id="eb1a5-251">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-251">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="eb1a5-252">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-252">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="eb1a5-253">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-253">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="eb1a5-254">例如：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-254">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="eb1a5-255">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-255">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="eb1a5-256">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="eb1a5-256">Routing and URL paths</span></span>

<span data-ttu-id="eb1a5-257">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-257">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="eb1a5-258">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-258">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="eb1a5-259">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-259">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="eb1a5-260">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-260">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="eb1a5-261">在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-261">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="eb1a5-262">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-262">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="eb1a5-263">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-263">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="eb1a5-264">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-264">This sample doesn't use a template.</span></span> <span data-ttu-id="eb1a5-265">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-265">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="eb1a5-266">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-266">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="eb1a5-267">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-267">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="eb1a5-268">傳回值</span><span class="sxs-lookup"><span data-stu-id="eb1a5-268">Return values</span></span>

<span data-ttu-id="eb1a5-269">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-269">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="eb1a5-270">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-270">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="eb1a5-271">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-271">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="eb1a5-272">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-272">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="eb1a5-273">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-273">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="eb1a5-274">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-274">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="eb1a5-275">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-275">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="eb1a5-276">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-276">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="eb1a5-277">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-277">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="eb1a5-278">測試 GetTodoItems 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-278">Test the GetTodoItems method</span></span>

<span data-ttu-id="eb1a5-279">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-279">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="eb1a5-280">安裝 [Postman](https://www.getpostman.com/apps)</span><span class="sxs-lookup"><span data-stu-id="eb1a5-280">Install [Postman](https://www.getpostman.com/apps)</span></span>
* <span data-ttu-id="eb1a5-281">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-281">Start the web app.</span></span>
* <span data-ttu-id="eb1a5-282">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-282">Start Postman.</span></span>
* <span data-ttu-id="eb1a5-283">停用 [SSL certificate verification] \(SSL 憑證驗證\)</span><span class="sxs-lookup"><span data-stu-id="eb1a5-283">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="eb1a5-284">從 [檔案] > [設定] (\**[一般]* 索引標籤)，停用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-284">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="eb1a5-285">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-285">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="eb1a5-286">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-286">Create a new request.</span></span>
  * <span data-ttu-id="eb1a5-287">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-287">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="eb1a5-288">將要求 URL 設定為 `https://localhost:<port>/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-288">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="eb1a5-289">例如，`https://localhost:5001/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-289">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="eb1a5-290">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-290">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="eb1a5-291">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-291">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="eb1a5-293">新增 Create 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-293">Add a Create method</span></span>

<span data-ttu-id="eb1a5-294">新增以下 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-294">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="eb1a5-295">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-295">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="eb1a5-296">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-296">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="eb1a5-297">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-297">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="eb1a5-298">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-298">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="eb1a5-299">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-299">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="eb1a5-300">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-300">Adds a `Location` header to the response.</span></span> <span data-ttu-id="eb1a5-301">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-301">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="eb1a5-302">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-302">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="eb1a5-303">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-303">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="eb1a5-304">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-304">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="eb1a5-305">測試 PostTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-305">Test the PostTodoItem method</span></span>

* <span data-ttu-id="eb1a5-306">建置專案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-306">Build the project.</span></span>
* <span data-ttu-id="eb1a5-307">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-307">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="eb1a5-308">選取 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-308">Select the **Body** tab.</span></span>
* <span data-ttu-id="eb1a5-309">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-309">Select the **raw** radio button.</span></span>
* <span data-ttu-id="eb1a5-310">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-310">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="eb1a5-311">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-311">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="eb1a5-312">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-312">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="eb1a5-314">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-314">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="eb1a5-315">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="eb1a5-315">Test the location header URI</span></span>

* <span data-ttu-id="eb1a5-316">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-316">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="eb1a5-317">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-317">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="eb1a5-319">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-319">Set the method to GET.</span></span>
* <span data-ttu-id="eb1a5-320">貼上 URI (例如 `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="eb1a5-320">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="eb1a5-321">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-321">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="eb1a5-322">新增 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-322">Add a PutTodoItem method</span></span>

<span data-ttu-id="eb1a5-323">新增以下 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-323">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="eb1a5-324">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-324">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="eb1a5-325">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-325">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="eb1a5-326">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-326">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="eb1a5-327">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-327">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="eb1a5-328">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-328">Test the PutTodoItem method</span></span>

<span data-ttu-id="eb1a5-329">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-329">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="eb1a5-330">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-330">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="eb1a5-332">新增 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-332">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="eb1a5-333">新增以下 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-333">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="eb1a5-334">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-334">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="eb1a5-335">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="eb1a5-335">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="eb1a5-336">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-336">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="eb1a5-337">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-337">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="eb1a5-338">設定要刪除的物件 URI，例如 `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="eb1a5-338">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="eb1a5-339">選取 [傳送]</span><span class="sxs-lookup"><span data-stu-id="eb1a5-339">Select **Send**</span></span>

<span data-ttu-id="eb1a5-340">範例應用程式可讓您刪除所有項目，但刪除最後一個項目之後，模型類別建構函式就會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-340">The sample app allows you to delete all the items, but when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="eb1a5-341">使用 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="eb1a5-341">Call the API with jQuery</span></span>

<span data-ttu-id="eb1a5-342">在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-342">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="eb1a5-343">jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-343">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="eb1a5-344">請設定應用程式來[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)，並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-344">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_):</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

::: moniker range=">= aspnetcore-2.2"
<span data-ttu-id="eb1a5-345">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-345">Create a *wwwroot* folder in the project directory.</span></span>
::: moniker-end

<span data-ttu-id="eb1a5-346">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-346">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="eb1a5-347">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-347">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="eb1a5-348">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-348">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="eb1a5-349">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-349">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="eb1a5-350">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-350">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="eb1a5-351">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-351">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="eb1a5-352">移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-352">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="eb1a5-353">您可以使用幾種方式來取得 jQuery。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-353">There are several ways to get jQuery.</span></span> <span data-ttu-id="eb1a5-354">在上述的程式碼片段中，從 CDN 載入程式庫。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-354">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="eb1a5-355">此範例會呼叫 API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-355">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="eb1a5-356">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-356">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="eb1a5-357">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="eb1a5-357">Get a list of to-do items</span></span>

<span data-ttu-id="eb1a5-358">JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 `GET` 要求傳送至 API，API 則會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-358">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="eb1a5-359">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-359">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="eb1a5-360">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-360">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="eb1a5-361">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-361">Add a to-do item</span></span>

<span data-ttu-id="eb1a5-362">[ajax](https://api.jquery.com/jquery.ajax/) 函式會傳送 `POST` 要求，並在要求本文中包含待辦事項。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-362">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="eb1a5-363">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-363">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="eb1a5-364">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-364">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="eb1a5-365">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-365">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="eb1a5-366">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-366">Update a to-do item</span></span>

<span data-ttu-id="eb1a5-367">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-367">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="eb1a5-368">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-368">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="eb1a5-369">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="eb1a5-369">Delete a to-do item</span></span>

<span data-ttu-id="eb1a5-370">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-370">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

## <a name="additional-resources"></a><span data-ttu-id="eb1a5-371">其他資源</span><span class="sxs-lookup"><span data-stu-id="eb1a5-371">Additional resources</span></span>

<span data-ttu-id="eb1a5-372">[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-372">[View or download sample code for this tutorial](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="eb1a5-373">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-373">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="eb1a5-374">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-374">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>

## <a name="next-steps"></a><span data-ttu-id="eb1a5-375">後續步驟</span><span class="sxs-lookup"><span data-stu-id="eb1a5-375">Next steps</span></span>

<span data-ttu-id="eb1a5-376">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-376">In this tutorial, you learned how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="eb1a5-377">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-377">Create a web api project.</span></span>
> * <span data-ttu-id="eb1a5-378">新增模型類別。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-378">Add a model class.</span></span>
> * <span data-ttu-id="eb1a5-379">建立資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-379">Create the database context.</span></span>
> * <span data-ttu-id="eb1a5-380">註冊資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-380">Register the database context.</span></span>
> * <span data-ttu-id="eb1a5-381">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-381">Add a controller.</span></span>
> * <span data-ttu-id="eb1a5-382">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-382">Add CRUD methods.</span></span>
> * <span data-ttu-id="eb1a5-383">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-383">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="eb1a5-384">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-384">Specify return values.</span></span>
> * <span data-ttu-id="eb1a5-385">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-385">Call the web API with Postman.</span></span>
> * <span data-ttu-id="eb1a5-386">使用 jQuery 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="eb1a5-386">Call the web api with jQuery.</span></span>

<span data-ttu-id="eb1a5-387">前進到下一個教學課程來了解如何產生 API 說明頁面：</span><span class="sxs-lookup"><span data-stu-id="eb1a5-387">Advance to the next tutorial to learn how to generate API help pages:</span></span>

> [!div class="nextstepaction"]
> <xref:tutorials/get-started-with-swashbuckle>
