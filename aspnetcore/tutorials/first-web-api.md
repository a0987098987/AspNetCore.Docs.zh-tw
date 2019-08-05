---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 07/11/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 60235af56077127093ac1d77338bc228a6edf073
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602508"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="2210a-103">教學課程：使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="2210a-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="2210a-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="2210a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="2210a-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="2210a-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2210a-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="2210a-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2210a-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-107">Create a web API project.</span></span>
> * <span data-ttu-id="2210a-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2210a-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="2210a-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="2210a-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="2210a-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="2210a-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="2210a-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-111">Call the web API with Postman.</span></span>

<span data-ttu-id="2210a-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="2210a-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="2210a-113">總覽</span><span class="sxs-lookup"><span data-stu-id="2210a-113">Overview</span></span>

<span data-ttu-id="2210a-114">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="2210a-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="2210a-115">API</span><span class="sxs-lookup"><span data-stu-id="2210a-115">API</span></span> | <span data-ttu-id="2210a-116">說明</span><span class="sxs-lookup"><span data-stu-id="2210a-116">Description</span></span> | <span data-ttu-id="2210a-117">要求本文</span><span class="sxs-lookup"><span data-stu-id="2210a-117">Request body</span></span> | <span data-ttu-id="2210a-118">回應本文</span><span class="sxs-lookup"><span data-stu-id="2210a-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="2210a-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="2210a-119">GET /api/TodoItems</span></span> | <span data-ttu-id="2210a-120">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-120">Get all to-do items</span></span> | <span data-ttu-id="2210a-121">None</span><span class="sxs-lookup"><span data-stu-id="2210a-121">None</span></span> | <span data-ttu-id="2210a-122">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="2210a-122">Array of to-do items</span></span>|
|<span data-ttu-id="2210a-123">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="2210a-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="2210a-124">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="2210a-124">Get an item by ID</span></span> | <span data-ttu-id="2210a-125">None</span><span class="sxs-lookup"><span data-stu-id="2210a-125">None</span></span> | <span data-ttu-id="2210a-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-126">To-do item</span></span>|
|<span data-ttu-id="2210a-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="2210a-127">POST /api/TodoItems</span></span> | <span data-ttu-id="2210a-128">新增記錄</span><span class="sxs-lookup"><span data-stu-id="2210a-128">Add a new item</span></span> | <span data-ttu-id="2210a-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-129">To-do item</span></span> | <span data-ttu-id="2210a-130">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-130">To-do item</span></span> |
|<span data-ttu-id="2210a-131">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="2210a-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="2210a-132">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="2210a-133">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-133">To-do item</span></span> | <span data-ttu-id="2210a-134">None</span><span class="sxs-lookup"><span data-stu-id="2210a-134">None</span></span> |
|<span data-ttu-id="2210a-135">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="2210a-136">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="2210a-137">None</span><span class="sxs-lookup"><span data-stu-id="2210a-137">None</span></span> | <span data-ttu-id="2210a-138">None</span><span class="sxs-lookup"><span data-stu-id="2210a-138">None</span></span>|

<span data-ttu-id="2210a-139">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="2210a-139">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="2210a-145">必要條件</span><span class="sxs-lookup"><span data-stu-id="2210a-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="2210a-149">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="2210a-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-151">從 [檔案]  功能表選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2210a-152">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="2210a-153">將專案命名為 *TodoApi*，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="2210a-154">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中，確認選取 [.NET Core]  和 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="2210a-155">選取 **API** 範本，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="2210a-156">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-156">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2210a-159">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="2210a-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2210a-160">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="2210a-161">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2210a-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="2210a-162">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="2210a-163">上述命令：</span><span class="sxs-lookup"><span data-stu-id="2210a-163">The preceding commands:</span></span>

  * <span data-ttu-id="2210a-164">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="2210a-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="2210a-165">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2210a-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2210a-167">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-167">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="2210a-169">選取[.Net Core]  > [應用程式]  > [API]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="2210a-171">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，選取 \* *.NET Core 3.0* 的 [目標 Framework]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="2210a-172">針對 [專案名稱]  輸入 *TodoApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="2210a-174">測試 API</span><span class="sxs-lookup"><span data-stu-id="2210a-174">Test the API</span></span>

<span data-ttu-id="2210a-175">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="2210a-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="2210a-176">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2210a-178">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2210a-179">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="2210a-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="2210a-180">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="2210a-181">在接著出現的 [安全性警告]  對話方塊中，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2210a-183">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2210a-184">在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。</span><span class="sxs-lookup"><span data-stu-id="2210a-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2210a-186">選取 [執行]   > [開始偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="2210a-187">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="2210a-188">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2210a-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="2210a-189">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="2210a-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="2210a-190">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="2210a-190">JSON similar to the following is returned:</span></span>

```json
[
    {
        "date": "2019-07-16T19:04:05.7257911-06:00",
        "temperatureC": 52,
        "temperatureF": 125,
        "summary": "Mild"
    },
    {
        "date": "2019-07-17T19:04:05.7258461-06:00",
        "temperatureC": 36,
        "temperatureF": 96,
        "summary": "Warm"
    },
    {
        "date": "2019-07-18T19:04:05.7258467-06:00",
        "temperatureC": 39,
        "temperatureF": 102,
        "summary": "Cool"
    },
    {
        "date": "2019-07-19T19:04:05.7258471-06:00",
        "temperatureC": 10,
        "temperatureF": 49,
        "summary": "Bracing"
    },
    {
        "date": "2019-07-20T19:04:05.7258474-06:00",
        "temperatureC": -1,
        "temperatureF": 31,
        "summary": "Chilly"
    }
]
```

## <a name="add-a-model-class"></a><span data-ttu-id="2210a-191">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="2210a-191">Add a model class</span></span>

<span data-ttu-id="2210a-192">「模型」  是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="2210a-193">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-195">在 [方案總管]  中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="2210a-196">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2210a-197">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="2210a-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="2210a-198">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2210a-199">將類別命名為 *TodoItem*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="2210a-200">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2210a-202">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="2210a-203">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="2210a-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2210a-205">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-205">Right-click the project.</span></span> <span data-ttu-id="2210a-206">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2210a-207">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="2210a-207">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="2210a-209">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]  > [新增檔案]  > [一般]  > [空類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="2210a-210">將類別命名為 *TodoItem*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="2210a-211">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2210a-212">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2210a-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="2210a-213">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="2210a-214">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="2210a-214">Add a database context</span></span>

<span data-ttu-id="2210a-215">「資料庫內容」  是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="2210a-216">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="2210a-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="2210a-218">新增 Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="2210a-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="2210a-219">在 [工具]  功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="2210a-220">選取 [包括發行前版本]  核取方塊。</span><span class="sxs-lookup"><span data-stu-id="2210a-220">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="2210a-221">選取 [瀏覽]  索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="2210a-221">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="2210a-222">選取左窗格中的 [Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-222">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="2210a-223">選取右窗格中的 [專案]  核取方塊，然後選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-223">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="2210a-224">使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2210a-224">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 封裝管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="2210a-226">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="2210a-226">Add the TodoContext database context</span></span>

* <span data-ttu-id="2210a-227">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-227">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2210a-228">將類別命名為 *TodoContext*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-228">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2210a-229">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-229">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2210a-230">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-230">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="2210a-231">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-231">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="2210a-232">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="2210a-232">Register the database context</span></span>

<span data-ttu-id="2210a-233">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="2210a-233">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2210a-234">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="2210a-234">The container provides the service to controllers.</span></span>

<span data-ttu-id="2210a-235">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="2210a-235">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="2210a-236">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-236">The preceding code:</span></span>

* <span data-ttu-id="2210a-237">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="2210a-237">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="2210a-238">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="2210a-238">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="2210a-239">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-239">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="2210a-240">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="2210a-240">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-241">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-241">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-242">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-242">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="2210a-243">選取 [新增]  > [新增 Scaffold 項目]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-243">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="2210a-244">選取 [使用 Entity Framework 執行動作的 API 控制器]  ，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-244">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="2210a-245">在 [使用 Entity Framework 執行動作的 API 控制器]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="2210a-245">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="2210a-246">在 [模型類別]  中選取 [TodoItem (TodoAPI.Models)]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-246">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="2210a-247">在 [資料內容類別]  中選取 [TodoContext (TodoAPI.Models)]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-247">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="2210a-248">選取 [新增] </span><span class="sxs-lookup"><span data-stu-id="2210a-248">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2210a-249">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-249">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="2210a-250">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2210a-250">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="2210a-251">上述命令：</span><span class="sxs-lookup"><span data-stu-id="2210a-251">The preceding commands:</span></span>

* <span data-ttu-id="2210a-252">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="2210a-252">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="2210a-253">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="2210a-253">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="2210a-254">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="2210a-254">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="2210a-255">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-255">The generated code:</span></span>

* <span data-ttu-id="2210a-256">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-256">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="2210a-257">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-257">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="2210a-258">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-258">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="2210a-259">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="2210a-259">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="2210a-260">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="2210a-260">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="2210a-261">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2210a-261">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="2210a-262">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="2210a-262">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="2210a-263">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="2210a-263">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="2210a-264">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="2210a-264">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="2210a-265">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="2210a-265">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="2210a-266"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-266">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="2210a-267">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-267">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="2210a-268">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="2210a-268">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="2210a-269">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。</span><span class="sxs-lookup"><span data-stu-id="2210a-269">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="2210a-270">`Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。</span><span class="sxs-lookup"><span data-stu-id="2210a-270">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="2210a-271">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="2210a-271">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="2210a-272">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="2210a-272">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="2210a-273">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="2210a-273">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="2210a-274">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="2210a-274">Install Postman</span></span>

<span data-ttu-id="2210a-275">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-275">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="2210a-276">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="2210a-276">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="2210a-277">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-277">Start the web app.</span></span>
* <span data-ttu-id="2210a-278">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="2210a-278">Start Postman.</span></span>
* <span data-ttu-id="2210a-279">停用 [SSL certificate verification] \(SSL 憑證驗證\) </span><span class="sxs-lookup"><span data-stu-id="2210a-279">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="2210a-280">從 [檔案] > [設定]  (\* *[一般]* 索引標籤)，停用 [SSL certificate verification] \(SSL 憑證驗證\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-280">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="2210a-281">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="2210a-281">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="2210a-282">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="2210a-282">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="2210a-283">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-283">Create a new request.</span></span>
* <span data-ttu-id="2210a-284">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="2210a-284">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="2210a-285">選取 [本文]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2210a-285">Select the **Body** tab.</span></span>
* <span data-ttu-id="2210a-286">選取 [原始]  選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="2210a-286">Select the **raw** radio button.</span></span>
* <span data-ttu-id="2210a-287">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="2210a-287">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="2210a-288">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="2210a-288">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="2210a-289">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-289">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="2210a-291">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="2210a-291">Test the location header URI</span></span>

* <span data-ttu-id="2210a-292">在 [回應]  窗格中選取 [標頭]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2210a-292">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="2210a-293">複製 [位置]  標頭值：</span><span class="sxs-lookup"><span data-stu-id="2210a-293">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="2210a-295">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="2210a-295">Set the method to GET.</span></span>
* <span data-ttu-id="2210a-296">貼上 URI (例如 `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="2210a-296">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="2210a-297">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-297">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="2210a-298">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-298">Examine the GET methods</span></span>

<span data-ttu-id="2210a-299">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="2210a-299">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="2210a-300">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-300">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="2210a-301">例如：</span><span class="sxs-lookup"><span data-stu-id="2210a-301">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="2210a-302">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="2210a-302">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="2210a-303">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="2210a-303">Test Get with Postman</span></span>

* <span data-ttu-id="2210a-304">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-304">Create a new request.</span></span>
* <span data-ttu-id="2210a-305">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="2210a-305">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="2210a-306">將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="2210a-306">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="2210a-307">例如，`https://localhost:5001/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="2210a-307">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="2210a-308">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-308">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="2210a-309">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-309">Select **Send**.</span></span>

<span data-ttu-id="2210a-310">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-310">This app uses an in-memory database.</span></span> <span data-ttu-id="2210a-311">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="2210a-311">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="2210a-312">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-312">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="2210a-313">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="2210a-313">Routing and URL paths</span></span>

<span data-ttu-id="2210a-314">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="2210a-314">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="2210a-315">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="2210a-315">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="2210a-316">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="2210a-316">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="2210a-317">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="2210a-317">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="2210a-318">在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="2210a-318">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="2210a-319">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2210a-319">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="2210a-320">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="2210a-320">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="2210a-321">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="2210a-321">This sample doesn't use a template.</span></span> <span data-ttu-id="2210a-322">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="2210a-322">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="2210a-323">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="2210a-323">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="2210a-324">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="2210a-324">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="2210a-325">傳回值</span><span class="sxs-lookup"><span data-stu-id="2210a-325">Return values</span></span>

<span data-ttu-id="2210a-326">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="2210a-326">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="2210a-327">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="2210a-327">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="2210a-328">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2210a-328">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="2210a-329">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2210a-329">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="2210a-330">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-330">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="2210a-331">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="2210a-331">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="2210a-332">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-332">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="2210a-333">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="2210a-333">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="2210a-334">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="2210a-334">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="2210a-335">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-335">The PutTodoItem method</span></span>

<span data-ttu-id="2210a-336">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-336">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="2210a-337">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="2210a-337">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="2210a-338">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="2210a-338">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="2210a-339">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="2210a-339">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="2210a-340">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="2210a-340">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="2210a-341">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-341">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="2210a-342">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-342">Test the PutTodoItem method</span></span>

<span data-ttu-id="2210a-343">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-343">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="2210a-344">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2210a-344">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="2210a-345">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-345">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="2210a-346">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="2210a-346">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="2210a-347">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="2210a-347">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="2210a-349">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="2210a-350">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="2210a-351">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="2210a-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="2210a-352">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="2210a-353">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="2210a-353">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="2210a-354">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="2210a-354">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="2210a-355">設定要刪除的物件 URI，例如 `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="2210a-355">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="2210a-356">選取 [傳送] </span><span class="sxs-lookup"><span data-stu-id="2210a-356">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="2210a-357">從 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="2210a-357">Call the API from jQuery</span></span>

<span data-ttu-id="2210a-358">請參閱[教學課程：使用 jQuery 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-jquery)。</span><span class="sxs-lookup"><span data-stu-id="2210a-358">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2210a-359">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="2210a-359">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="2210a-360">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-360">Create a web API project.</span></span>
> * <span data-ttu-id="2210a-361">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2210a-361">Add a model class and a database context.</span></span>
> * <span data-ttu-id="2210a-362">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="2210a-362">Add a controller.</span></span>
> * <span data-ttu-id="2210a-363">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="2210a-363">Add CRUD methods.</span></span>
> * <span data-ttu-id="2210a-364">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="2210a-364">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="2210a-365">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="2210a-365">Specify return values.</span></span>
> * <span data-ttu-id="2210a-366">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-366">Call the web API with Postman.</span></span>
> * <span data-ttu-id="2210a-367">使用 jQuery 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-367">Call the web API with jQuery.</span></span>

<span data-ttu-id="2210a-368">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-368">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="2210a-369">總覽</span><span class="sxs-lookup"><span data-stu-id="2210a-369">Overview</span></span>

<span data-ttu-id="2210a-370">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="2210a-370">This tutorial creates the following API:</span></span>

|<span data-ttu-id="2210a-371">API</span><span class="sxs-lookup"><span data-stu-id="2210a-371">API</span></span> | <span data-ttu-id="2210a-372">說明</span><span class="sxs-lookup"><span data-stu-id="2210a-372">Description</span></span> | <span data-ttu-id="2210a-373">要求本文</span><span class="sxs-lookup"><span data-stu-id="2210a-373">Request body</span></span> | <span data-ttu-id="2210a-374">回應本文</span><span class="sxs-lookup"><span data-stu-id="2210a-374">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="2210a-375">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="2210a-375">GET /api/TodoItems</span></span> | <span data-ttu-id="2210a-376">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-376">Get all to-do items</span></span> | <span data-ttu-id="2210a-377">None</span><span class="sxs-lookup"><span data-stu-id="2210a-377">None</span></span> | <span data-ttu-id="2210a-378">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="2210a-378">Array of to-do items</span></span>|
|<span data-ttu-id="2210a-379">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="2210a-379">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="2210a-380">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="2210a-380">Get an item by ID</span></span> | <span data-ttu-id="2210a-381">None</span><span class="sxs-lookup"><span data-stu-id="2210a-381">None</span></span> | <span data-ttu-id="2210a-382">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-382">To-do item</span></span>|
|<span data-ttu-id="2210a-383">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="2210a-383">POST /api/TodoItems</span></span> | <span data-ttu-id="2210a-384">新增記錄</span><span class="sxs-lookup"><span data-stu-id="2210a-384">Add a new item</span></span> | <span data-ttu-id="2210a-385">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-385">To-do item</span></span> | <span data-ttu-id="2210a-386">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-386">To-do item</span></span> |
|<span data-ttu-id="2210a-387">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="2210a-387">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="2210a-388">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-388">Update an existing item &nbsp;</span></span> | <span data-ttu-id="2210a-389">待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-389">To-do item</span></span> | <span data-ttu-id="2210a-390">None</span><span class="sxs-lookup"><span data-stu-id="2210a-390">None</span></span> |
|<span data-ttu-id="2210a-391">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-391">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="2210a-392">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="2210a-392">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="2210a-393">None</span><span class="sxs-lookup"><span data-stu-id="2210a-393">None</span></span> | <span data-ttu-id="2210a-394">None</span><span class="sxs-lookup"><span data-stu-id="2210a-394">None</span></span>|

<span data-ttu-id="2210a-395">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="2210a-395">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="2210a-401">必要條件</span><span class="sxs-lookup"><span data-stu-id="2210a-401">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-402">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-402">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-403">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-403">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-404">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-404">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="2210a-405">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="2210a-405">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-406">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-406">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-407">從 [檔案]  功能表選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-407">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="2210a-408">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-408">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="2210a-409">將專案命名為 *TodoApi*，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-409">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="2210a-410">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中，確認選取 [.NET Core]  和 [ASP.NET Core 2.2]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-410">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="2210a-411">選取 **API** 範本，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-411">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="2210a-412">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-412">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-414">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-414">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2210a-415">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="2210a-415">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="2210a-416">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-416">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="2210a-417">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="2210a-417">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="2210a-418">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="2210a-418">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="2210a-419">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-419">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-420">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2210a-421">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-421">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="2210a-423">選取[.Net Core]  > [應用程式]  > [API]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-423">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="2210a-425">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，接受 [目標 Framework]  的預設 \* *.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="2210a-425">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="2210a-426">針對 [專案名稱]  輸入 *TodoApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-426">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="2210a-428">測試 API</span><span class="sxs-lookup"><span data-stu-id="2210a-428">Test the API</span></span>

<span data-ttu-id="2210a-429">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="2210a-429">The project template creates a `values` API.</span></span> <span data-ttu-id="2210a-430">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-430">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-431">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-431">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2210a-432">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-432">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2210a-433">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="2210a-433">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="2210a-434">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-434">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="2210a-435">在接著出現的 [安全性警告]  對話方塊中，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-435">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-436">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-436">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="2210a-437">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-437">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="2210a-438">在瀏覽器中，前往下列 URL：[https://localhost:5001/api/values](https://localhost:5001/api/values)。</span><span class="sxs-lookup"><span data-stu-id="2210a-438">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-439">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="2210a-440">選取 [執行]   > [開始偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-440">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="2210a-441">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-441">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="2210a-442">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2210a-442">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="2210a-443">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="2210a-443">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="2210a-444">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="2210a-444">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="2210a-445">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="2210a-445">Add a model class</span></span>

<span data-ttu-id="2210a-446">「模型」  是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-446">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="2210a-447">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-447">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-448">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-448">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-449">在 [方案總管]  中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-449">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="2210a-450">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-450">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2210a-451">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="2210a-451">Name the folder *Models*.</span></span>

* <span data-ttu-id="2210a-452">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-452">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2210a-453">將類別命名為 *TodoItem*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-453">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="2210a-454">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-454">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="2210a-455">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="2210a-455">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="2210a-456">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-456">Add a folder named *Models*.</span></span>

* <span data-ttu-id="2210a-457">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="2210a-457">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="2210a-458">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-458">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="2210a-459">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-459">Right-click the project.</span></span> <span data-ttu-id="2210a-460">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-460">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="2210a-461">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="2210a-461">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="2210a-463">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]  > [新增檔案]  > [一般]  > [空類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-463">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="2210a-464">將類別命名為 *TodoItem*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-464">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="2210a-465">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-465">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="2210a-466">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="2210a-466">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="2210a-467">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-467">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="2210a-468">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="2210a-468">Add a database context</span></span>

<span data-ttu-id="2210a-469">「資料庫內容」  是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-469">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="2210a-470">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="2210a-470">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-471">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-471">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-472">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-472">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="2210a-473">將類別命名為 *TodoContext*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-473">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2210a-474">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-474">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2210a-475">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-475">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="2210a-476">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-476">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="2210a-477">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="2210a-477">Register the database context</span></span>

<span data-ttu-id="2210a-478">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="2210a-478">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="2210a-479">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="2210a-479">The container provides the service to controllers.</span></span>

<span data-ttu-id="2210a-480">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="2210a-480">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="2210a-481">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-481">The preceding code:</span></span>

* <span data-ttu-id="2210a-482">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="2210a-482">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="2210a-483">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="2210a-483">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="2210a-484">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-484">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="2210a-485">新增控制器</span><span class="sxs-lookup"><span data-stu-id="2210a-485">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="2210a-486">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2210a-486">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="2210a-487">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-487">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="2210a-488">選取 [新增]  > [新增項目]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-488">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="2210a-489">在 [新增項目]  對話方塊中，選取 [API 控制器類別]  範本。</span><span class="sxs-lookup"><span data-stu-id="2210a-489">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="2210a-490">將類別命名為 *TodoController*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-490">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="2210a-492">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="2210a-492">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="2210a-493">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-493">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="2210a-494">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-494">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="2210a-495">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-495">The preceding code:</span></span>

* <span data-ttu-id="2210a-496">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-496">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="2210a-497">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="2210a-497">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="2210a-498">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-498">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="2210a-499">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="2210a-499">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="2210a-500">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="2210a-500">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="2210a-501">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="2210a-501">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="2210a-502">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-502">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="2210a-503">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-503">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="2210a-504">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="2210a-504">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="2210a-505">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="2210a-505">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="2210a-506">新增 Get 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-506">Add Get methods</span></span>

<span data-ttu-id="2210a-507">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-507">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="2210a-508">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="2210a-508">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="2210a-509">如果應用程式仍在執行，請將其停止。</span><span class="sxs-lookup"><span data-stu-id="2210a-509">Stop the app if it's still running.</span></span> <span data-ttu-id="2210a-510">然後重新予以執行以包含最新的變更。</span><span class="sxs-lookup"><span data-stu-id="2210a-510">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="2210a-511">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-511">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="2210a-512">例如：</span><span class="sxs-lookup"><span data-stu-id="2210a-512">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="2210a-513">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="2210a-513">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="2210a-514">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="2210a-514">Routing and URL paths</span></span>

<span data-ttu-id="2210a-515">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="2210a-515">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="2210a-516">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="2210a-516">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="2210a-517">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="2210a-517">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="2210a-518">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="2210a-518">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="2210a-519">在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="2210a-519">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="2210a-520">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="2210a-520">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="2210a-521">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="2210a-521">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="2210a-522">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="2210a-522">This sample doesn't use a template.</span></span> <span data-ttu-id="2210a-523">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="2210a-523">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="2210a-524">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="2210a-524">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="2210a-525">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="2210a-525">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="2210a-526">傳回值</span><span class="sxs-lookup"><span data-stu-id="2210a-526">Return values</span></span>

<span data-ttu-id="2210a-527">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="2210a-527">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="2210a-528">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="2210a-528">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="2210a-529">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="2210a-529">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="2210a-530">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="2210a-530">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="2210a-531">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-531">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="2210a-532">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="2210a-532">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="2210a-533">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-533">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="2210a-534">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="2210a-534">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="2210a-535">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="2210a-535">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="2210a-536">測試 GetTodoItems 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-536">Test the GetTodoItems method</span></span>

<span data-ttu-id="2210a-537">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-537">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="2210a-538">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="2210a-538">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="2210a-539">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="2210a-539">Start the web app.</span></span>
* <span data-ttu-id="2210a-540">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="2210a-540">Start Postman.</span></span>
* <span data-ttu-id="2210a-541">停用 [SSL certificate verification] \(SSL 憑證驗證\) </span><span class="sxs-lookup"><span data-stu-id="2210a-541">Disable **SSL certificate verification**</span></span>
  
  * <span data-ttu-id="2210a-542">從 [檔案] > [設定]  (\* *[一般]* 索引標籤)，停用 [SSL certificate verification] \(SSL 憑證驗證\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-542">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="2210a-543">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="2210a-543">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="2210a-544">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="2210a-544">Create a new request.</span></span>
  * <span data-ttu-id="2210a-545">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="2210a-545">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="2210a-546">將要求 URL 設定為 `https://localhost:<port>/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="2210a-546">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="2210a-547">例如，`https://localhost:5001/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="2210a-547">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="2210a-548">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)  。</span><span class="sxs-lookup"><span data-stu-id="2210a-548">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="2210a-549">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-549">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="2210a-551">新增 Create 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-551">Add a Create method</span></span>

<span data-ttu-id="2210a-552">新增以下 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-552">Add the following `PostTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="2210a-553">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="2210a-553">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="2210a-554">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="2210a-554">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="2210a-555">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-555">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="2210a-556">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-556">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="2210a-557">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="2210a-557">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="2210a-558">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="2210a-558">Adds a `Location` header to the response.</span></span> <span data-ttu-id="2210a-559">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="2210a-559">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="2210a-560">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="2210a-560">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="2210a-561">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="2210a-561">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="2210a-562">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="2210a-562">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="2210a-563">測試 PostTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-563">Test the PostTodoItem method</span></span>

* <span data-ttu-id="2210a-564">建置專案。</span><span class="sxs-lookup"><span data-stu-id="2210a-564">Build the project.</span></span>
* <span data-ttu-id="2210a-565">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="2210a-565">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="2210a-566">選取 [本文]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2210a-566">Select the **Body** tab.</span></span>
* <span data-ttu-id="2210a-567">選取 [原始]  選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="2210a-567">Select the **raw** radio button.</span></span>
* <span data-ttu-id="2210a-568">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="2210a-568">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="2210a-569">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="2210a-569">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="2210a-570">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-570">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="2210a-572">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="2210a-572">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="2210a-573">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="2210a-573">Test the location header URI</span></span>

* <span data-ttu-id="2210a-574">在 [回應]  窗格中選取 [標頭]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="2210a-574">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="2210a-575">複製 [位置]  標頭值：</span><span class="sxs-lookup"><span data-stu-id="2210a-575">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="2210a-577">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="2210a-577">Set the method to GET.</span></span>
* <span data-ttu-id="2210a-578">貼上 URI (例如 `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="2210a-578">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="2210a-579">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="2210a-579">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="2210a-580">新增 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-580">Add a PutTodoItem method</span></span>

<span data-ttu-id="2210a-581">新增以下 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-581">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="2210a-582">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="2210a-582">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="2210a-583">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="2210a-583">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="2210a-584">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="2210a-584">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="2210a-585">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="2210a-585">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="2210a-586">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-586">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="2210a-587">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-587">Test the PutTodoItem method</span></span>

<span data-ttu-id="2210a-588">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-588">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="2210a-589">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="2210a-589">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="2210a-590">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-590">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="2210a-591">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="2210a-591">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="2210a-592">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="2210a-592">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="2210a-594">新增 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-594">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="2210a-595">新增以下 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="2210a-595">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="2210a-596">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="2210a-596">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="2210a-597">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="2210a-597">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="2210a-598">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="2210a-598">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="2210a-599">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="2210a-599">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="2210a-600">設定要刪除的物件 URI，例如 `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="2210a-600">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="2210a-601">選取 [傳送] </span><span class="sxs-lookup"><span data-stu-id="2210a-601">Select **Send**</span></span>

<span data-ttu-id="2210a-602">範例應用程式可讓您刪除所有項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-602">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="2210a-603">但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="2210a-603">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="2210a-604">使用 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="2210a-604">Call the API with jQuery</span></span>

<span data-ttu-id="2210a-605">在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="2210a-605">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="2210a-606">jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。</span><span class="sxs-lookup"><span data-stu-id="2210a-606">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="2210a-607">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="2210a-607">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="2210a-608">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="2210a-608">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="2210a-609">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2210a-609">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="2210a-610">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="2210a-610">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="2210a-611">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="2210a-611">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="2210a-612">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2210a-612">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="2210a-613">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="2210a-613">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="2210a-614">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="2210a-614">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="2210a-615">移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="2210a-615">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="2210a-616">您可以使用幾種方式來取得 jQuery。</span><span class="sxs-lookup"><span data-stu-id="2210a-616">There are several ways to get jQuery.</span></span> <span data-ttu-id="2210a-617">在上述的程式碼片段中，從 CDN 載入程式庫。</span><span class="sxs-lookup"><span data-stu-id="2210a-617">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="2210a-618">此範例會呼叫 API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="2210a-618">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="2210a-619">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="2210a-619">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="2210a-620">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="2210a-620">Get a list of to-do items</span></span>

<span data-ttu-id="2210a-621">JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 `GET` 要求傳送至 API，API 則會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="2210a-621">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="2210a-622">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="2210a-622">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="2210a-623">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="2210a-623">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="2210a-624">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-624">Add a to-do item</span></span>

<span data-ttu-id="2210a-625">[ajax](https://api.jquery.com/jquery.ajax/) 函式會傳送 `POST` 要求，並在要求本文中包含待辦事項。</span><span class="sxs-lookup"><span data-stu-id="2210a-625">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="2210a-626">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="2210a-626">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="2210a-627">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="2210a-627">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="2210a-628">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="2210a-628">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="2210a-629">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-629">Update a to-do item</span></span>

<span data-ttu-id="2210a-630">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="2210a-630">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="2210a-631">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="2210a-631">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="2210a-632">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="2210a-632">Delete a to-do item</span></span>

<span data-ttu-id="2210a-633">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="2210a-633">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="2210a-634">其他資源</span><span class="sxs-lookup"><span data-stu-id="2210a-634">Additional resources</span></span>

<span data-ttu-id="2210a-635">[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="2210a-635">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="2210a-636">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="2210a-636">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="2210a-637">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="2210a-637">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/index>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="2210a-638">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="2210a-638">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
