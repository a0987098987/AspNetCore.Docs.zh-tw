---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 08/14/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 99985e9fb1134c2ba808434f8d24c4a768773268
ms.sourcegitcommit: 476ea5ad86a680b7b017c6f32098acd3414c0f6c
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/14/2019
ms.locfileid: "69022599"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="9f0a3-103">教學課程：使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="9f0a3-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="9f0a3-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="9f0a3-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9f0a3-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f0a3-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-107">Create a web API project.</span></span>
> * <span data-ttu-id="9f0a3-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="9f0a3-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="9f0a3-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="9f0a3-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-111">Call the web API with Postman.</span></span>

<span data-ttu-id="9f0a3-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="9f0a3-113">總覽</span><span class="sxs-lookup"><span data-stu-id="9f0a3-113">Overview</span></span>

<span data-ttu-id="9f0a3-114">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="9f0a3-115">API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-115">API</span></span> | <span data-ttu-id="9f0a3-116">說明</span><span class="sxs-lookup"><span data-stu-id="9f0a3-116">Description</span></span> | <span data-ttu-id="9f0a3-117">要求本文</span><span class="sxs-lookup"><span data-stu-id="9f0a3-117">Request body</span></span> | <span data-ttu-id="9f0a3-118">回應本文</span><span class="sxs-lookup"><span data-stu-id="9f0a3-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="9f0a3-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="9f0a3-119">GET /api/TodoItems</span></span> | <span data-ttu-id="9f0a3-120">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-120">Get all to-do items</span></span> | <span data-ttu-id="9f0a3-121">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-121">None</span></span> | <span data-ttu-id="9f0a3-122">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="9f0a3-122">Array of to-do items</span></span>|
|<span data-ttu-id="9f0a3-123">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="9f0a3-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="9f0a3-124">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="9f0a3-124">Get an item by ID</span></span> | <span data-ttu-id="9f0a3-125">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-125">None</span></span> | <span data-ttu-id="9f0a3-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-126">To-do item</span></span>|
|<span data-ttu-id="9f0a3-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="9f0a3-127">POST /api/TodoItems</span></span> | <span data-ttu-id="9f0a3-128">新增記錄</span><span class="sxs-lookup"><span data-stu-id="9f0a3-128">Add a new item</span></span> | <span data-ttu-id="9f0a3-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-129">To-do item</span></span> | <span data-ttu-id="9f0a3-130">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-130">To-do item</span></span> |
|<span data-ttu-id="9f0a3-131">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="9f0a3-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="9f0a3-132">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="9f0a3-133">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-133">To-do item</span></span> | <span data-ttu-id="9f0a3-134">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-134">None</span></span> |
|<span data-ttu-id="9f0a3-135">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="9f0a3-136">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="9f0a3-137">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-137">None</span></span> | <span data-ttu-id="9f0a3-138">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-138">None</span></span>|

<span data-ttu-id="9f0a3-139">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-139">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="9f0a3-145">必要條件</span><span class="sxs-lookup"><span data-stu-id="9f0a3-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="9f0a3-149">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="9f0a3-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-151">從 [檔案]  功能表選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9f0a3-152">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="9f0a3-153">將專案命名為 *TodoApi*，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="9f0a3-154">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中，確認選取 [.NET Core]  和 [ASP.NET Core 3.0]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="9f0a3-155">選取 **API** 範本，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-155">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="9f0a3-156">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-156">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-158">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-158">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f0a3-159">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-159">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9f0a3-160">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-160">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="9f0a3-161">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-161">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   cd TodoAPI
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   code -r ../TodoApi
   ```

* <span data-ttu-id="9f0a3-162">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-162">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="9f0a3-163">上述命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-163">The preceding commands:</span></span>

  * <span data-ttu-id="9f0a3-164">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-164">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="9f0a3-165">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-165">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-166">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-166">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9f0a3-167">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-167">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="9f0a3-169">選取[.Net Core]  > [應用程式]  > [API]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-169">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="9f0a3-171">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，選取 \* *.NET Core 3.0* 的 [目標 Framework]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-171">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="9f0a3-172">針對 [專案名稱]  輸入 *TodoApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-172">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="9f0a3-174">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-174">Open a command terminal in the project folder and run the following commands:</span></span>

   ```console
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer --version 3.0.0-*
   dotnet add package Microsoft.EntityFrameworkCore.InMemory --version 3.0.0-*
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="9f0a3-175">測試 API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-175">Test the API</span></span>

<span data-ttu-id="9f0a3-176">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-176">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="9f0a3-177">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-177">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-178">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-178">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f0a3-179">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="9f0a3-180">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-180">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="9f0a3-181">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-181">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="9f0a3-182">在接著出現的 [安全性警告]  對話方塊中，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-182">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-183">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-183">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9f0a3-184">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-184">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="9f0a3-185">在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-185">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-186">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-186">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9f0a3-187">選取 [執行]   > [開始偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-187">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="9f0a3-188">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-188">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="9f0a3-189">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-189">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="9f0a3-190">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-190">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="9f0a3-191">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-191">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="9f0a3-192">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="9f0a3-192">Add a model class</span></span>

<span data-ttu-id="9f0a3-193">「模型」  是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-193">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="9f0a3-194">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-194">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-195">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-195">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-196">在 [方案總管]  中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-196">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="9f0a3-197">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-197">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="9f0a3-198">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-198">Name the folder *Models*.</span></span>

* <span data-ttu-id="9f0a3-199">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-199">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="9f0a3-200">將類別命名為 *TodoItem*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-200">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="9f0a3-201">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-201">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-202">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-202">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f0a3-203">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-203">Add a folder named *Models*.</span></span>

* <span data-ttu-id="9f0a3-204">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-204">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-205">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-205">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9f0a3-206">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-206">Right-click the project.</span></span> <span data-ttu-id="9f0a3-207">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-207">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="9f0a3-208">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-208">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="9f0a3-210">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]  > [新增檔案]  > [一般]  > [空類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-210">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="9f0a3-211">將類別命名為 *TodoItem*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-211">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="9f0a3-212">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-212">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="9f0a3-213">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-213">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="9f0a3-214">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-214">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="9f0a3-215">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9f0a3-215">Add a database context</span></span>

<span data-ttu-id="9f0a3-216">「資料庫內容」  是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-216">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="9f0a3-217">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-217">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-218">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-218">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="9f0a3-219">新增 Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="9f0a3-219">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="9f0a3-220">在 [工具]  功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-220">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="9f0a3-221">選取 [包括發行前版本]  核取方塊。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-221">Select the **Include prerelease** checkbox.</span></span>
* <span data-ttu-id="9f0a3-222">選取 [瀏覽]  索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-222">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="9f0a3-223">選取左窗格中的 [Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-223">Select  **Microsoft.EntityFrameworkCore.SqlServer V3.0.0-preview** in the left pane.</span></span>
* <span data-ttu-id="9f0a3-224">選取右窗格中的 [專案]  核取方塊，然後選取 [安裝]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-224">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="9f0a3-225">使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-225">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 封裝管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="9f0a3-227">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9f0a3-227">Add the TodoContext database context</span></span>

* <span data-ttu-id="9f0a3-228">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-228">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="9f0a3-229">將類別命名為 *TodoContext*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-229">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9f0a3-230">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-230">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9f0a3-231">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-231">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="9f0a3-232">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-232">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="9f0a3-233">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9f0a3-233">Register the database context</span></span>

<span data-ttu-id="9f0a3-234">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-234">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9f0a3-235">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-235">The container provides the service to controllers.</span></span>

<span data-ttu-id="9f0a3-236">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-236">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="9f0a3-237">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-237">The preceding code:</span></span>

* <span data-ttu-id="9f0a3-238">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-238">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="9f0a3-239">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-239">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="9f0a3-240">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-240">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="9f0a3-241">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="9f0a3-241">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-242">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-242">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-243">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-243">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="9f0a3-244">選取 [新增]  > [新增 Scaffold 項目]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-244">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="9f0a3-245">選取 [使用 Entity Framework 執行動作的 API 控制器]  ，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-245">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="9f0a3-246">在 [使用 Entity Framework 執行動作的 API 控制器]  對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-246">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="9f0a3-247">在 [模型類別]  中選取 [TodoItem (TodoAPI.Models)]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-247">Select **TodoItem (TodoAPI.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="9f0a3-248">在 [資料內容類別]  中選取 [TodoContext (TodoAPI.Models)]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-248">Select **TodoContext (TodoAPI.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="9f0a3-249">選取 [新增] </span><span class="sxs-lookup"><span data-stu-id="9f0a3-249">Select **Add**</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9f0a3-250">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-250">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="9f0a3-251">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-251">Run the following commands:</span></span>

```console
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design --version 3.0.0-*
dotnet add package Microsoft.EntityFrameworkCore.Design --version 3.0.0-*
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext  -outDir Controllers
```

<span data-ttu-id="9f0a3-252">上述命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-252">The preceding commands:</span></span>

* <span data-ttu-id="9f0a3-253">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-253">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="9f0a3-254">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-254">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="9f0a3-255">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-255">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="9f0a3-256">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-256">The generated code:</span></span>

* <span data-ttu-id="9f0a3-257">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-257">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="9f0a3-258">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-258">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="9f0a3-259">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-259">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="9f0a3-260">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-260">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="9f0a3-261">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-261">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="9f0a3-262">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-262">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="9f0a3-263">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-263">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="9f0a3-264">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-264">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="9f0a3-265">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-265">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="9f0a3-266">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-266">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="9f0a3-267"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-267">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="9f0a3-268">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-268">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="9f0a3-269">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-269">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="9f0a3-270">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-270">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="9f0a3-271">`Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-271">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="9f0a3-272">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-272">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="9f0a3-273">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-273">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="9f0a3-274">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-274">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="9f0a3-275">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="9f0a3-275">Install Postman</span></span>

<span data-ttu-id="9f0a3-276">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-276">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="9f0a3-277">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="9f0a3-277">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="9f0a3-278">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-278">Start the web app.</span></span>
* <span data-ttu-id="9f0a3-279">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-279">Start Postman.</span></span>
* <span data-ttu-id="9f0a3-280">停用 [SSL certificate verification] \(SSL 憑證驗證\) </span><span class="sxs-lookup"><span data-stu-id="9f0a3-280">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="9f0a3-281">從 [檔案] > [設定]  (\* *[一般]* 索引標籤)，停用 [SSL certificate verification] \(SSL 憑證驗證\)  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-281">From  **File > Settings** (\**General* tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="9f0a3-282">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-282">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="9f0a3-283">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="9f0a3-283">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="9f0a3-284">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-284">Create a new request.</span></span>
* <span data-ttu-id="9f0a3-285">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-285">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="9f0a3-286">選取 [本文]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-286">Select the **Body** tab.</span></span>
* <span data-ttu-id="9f0a3-287">選取 [原始]  選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-287">Select the **raw** radio button.</span></span>
* <span data-ttu-id="9f0a3-288">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-288">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="9f0a3-289">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-289">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="9f0a3-290">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-290">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="9f0a3-292">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="9f0a3-292">Test the location header URI</span></span>

* <span data-ttu-id="9f0a3-293">在 [回應]  窗格中選取 [標頭]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-293">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="9f0a3-294">複製 [位置]  標頭值：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-294">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="9f0a3-296">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-296">Set the method to GET.</span></span>
* <span data-ttu-id="9f0a3-297">貼上 URI (例如 `https://localhost:5001/api/TodoItems/1`)</span><span class="sxs-lookup"><span data-stu-id="9f0a3-297">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`)</span></span>
* <span data-ttu-id="9f0a3-298">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-298">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="9f0a3-299">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-299">Examine the GET methods</span></span>

<span data-ttu-id="9f0a3-300">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-300">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="9f0a3-301">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-301">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="9f0a3-302">例如：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-302">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="9f0a3-303">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-303">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="9f0a3-304">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="9f0a3-304">Test Get with Postman</span></span>

* <span data-ttu-id="9f0a3-305">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-305">Create a new request.</span></span>
* <span data-ttu-id="9f0a3-306">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-306">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="9f0a3-307">將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-307">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="9f0a3-308">例如，`https://localhost:5001/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-308">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="9f0a3-309">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-309">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="9f0a3-310">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-310">Select **Send**.</span></span>

<span data-ttu-id="9f0a3-311">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-311">This app uses an in-memory database.</span></span> <span data-ttu-id="9f0a3-312">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-312">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="9f0a3-313">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-313">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="9f0a3-314">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="9f0a3-314">Routing and URL paths</span></span>

<span data-ttu-id="9f0a3-315">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-315">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="9f0a3-316">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-316">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="9f0a3-317">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-317">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="9f0a3-318">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-318">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="9f0a3-319">在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-319">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="9f0a3-320">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-320">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="9f0a3-321">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-321">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="9f0a3-322">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-322">This sample doesn't use a template.</span></span> <span data-ttu-id="9f0a3-323">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-323">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="9f0a3-324">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-324">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="9f0a3-325">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-325">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="9f0a3-326">傳回值</span><span class="sxs-lookup"><span data-stu-id="9f0a3-326">Return values</span></span>

<span data-ttu-id="9f0a3-327">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-327">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="9f0a3-328">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-328">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="9f0a3-329">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-329">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="9f0a3-330">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-330">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="9f0a3-331">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-331">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="9f0a3-332">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-332">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="9f0a3-333">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-333">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="9f0a3-334">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-334">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="9f0a3-335">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-335">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="9f0a3-336">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-336">The PutTodoItem method</span></span>

<span data-ttu-id="9f0a3-337">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-337">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="9f0a3-338">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-338">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="9f0a3-339">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-339">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="9f0a3-340">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-340">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="9f0a3-341">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-341">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="9f0a3-342">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-342">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="9f0a3-343">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-343">Test the PutTodoItem method</span></span>

<span data-ttu-id="9f0a3-344">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-344">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="9f0a3-345">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-345">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="9f0a3-346">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-346">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="9f0a3-347">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-347">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="9f0a3-348">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-348">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="9f0a3-350">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-350">The DeleteTodoItem method</span></span>

<span data-ttu-id="9f0a3-351">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-351">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="9f0a3-352">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-352">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="9f0a3-353">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-353">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="9f0a3-354">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-354">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="9f0a3-355">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-355">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="9f0a3-356">設定要刪除的物件 URI，例如 `https://localhost:5001/api/TodoItems/1`</span><span class="sxs-lookup"><span data-stu-id="9f0a3-356">Set the URI of the object to delete, for example `https://localhost:5001/api/TodoItems/1`</span></span>
* <span data-ttu-id="9f0a3-357">選取 [傳送] </span><span class="sxs-lookup"><span data-stu-id="9f0a3-357">Select **Send**</span></span>

## <a name="call-the-api-from-jquery"></a><span data-ttu-id="9f0a3-358">從 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-358">Call the API from jQuery</span></span>

<span data-ttu-id="9f0a3-359">請參閱[教學課程：使用 jQuery 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-jquery)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-359">See [Tutorial: Call an ASP.NET Core web API with jQuery](xref:tutorials/web-api-jquery).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9f0a3-360">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-360">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="9f0a3-361">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-361">Create a web API project.</span></span>
> * <span data-ttu-id="9f0a3-362">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-362">Add a model class and a database context.</span></span>
> * <span data-ttu-id="9f0a3-363">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-363">Add a controller.</span></span>
> * <span data-ttu-id="9f0a3-364">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-364">Add CRUD methods.</span></span>
> * <span data-ttu-id="9f0a3-365">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-365">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="9f0a3-366">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-366">Specify return values.</span></span>
> * <span data-ttu-id="9f0a3-367">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-367">Call the web API with Postman.</span></span>
> * <span data-ttu-id="9f0a3-368">使用 jQuery 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-368">Call the web API with jQuery.</span></span>

<span data-ttu-id="9f0a3-369">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-369">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>
## <a name="overview"></a><span data-ttu-id="9f0a3-370">總覽</span><span class="sxs-lookup"><span data-stu-id="9f0a3-370">Overview</span></span>

<span data-ttu-id="9f0a3-371">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-371">This tutorial creates the following API:</span></span>

|<span data-ttu-id="9f0a3-372">API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-372">API</span></span> | <span data-ttu-id="9f0a3-373">說明</span><span class="sxs-lookup"><span data-stu-id="9f0a3-373">Description</span></span> | <span data-ttu-id="9f0a3-374">要求本文</span><span class="sxs-lookup"><span data-stu-id="9f0a3-374">Request body</span></span> | <span data-ttu-id="9f0a3-375">回應本文</span><span class="sxs-lookup"><span data-stu-id="9f0a3-375">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="9f0a3-376">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="9f0a3-376">GET /api/TodoItems</span></span> | <span data-ttu-id="9f0a3-377">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-377">Get all to-do items</span></span> | <span data-ttu-id="9f0a3-378">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-378">None</span></span> | <span data-ttu-id="9f0a3-379">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="9f0a3-379">Array of to-do items</span></span>|
|<span data-ttu-id="9f0a3-380">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="9f0a3-380">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="9f0a3-381">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="9f0a3-381">Get an item by ID</span></span> | <span data-ttu-id="9f0a3-382">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-382">None</span></span> | <span data-ttu-id="9f0a3-383">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-383">To-do item</span></span>|
|<span data-ttu-id="9f0a3-384">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="9f0a3-384">POST /api/TodoItems</span></span> | <span data-ttu-id="9f0a3-385">新增記錄</span><span class="sxs-lookup"><span data-stu-id="9f0a3-385">Add a new item</span></span> | <span data-ttu-id="9f0a3-386">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-386">To-do item</span></span> | <span data-ttu-id="9f0a3-387">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-387">To-do item</span></span> |
|<span data-ttu-id="9f0a3-388">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="9f0a3-388">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="9f0a3-389">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-389">Update an existing item &nbsp;</span></span> | <span data-ttu-id="9f0a3-390">待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-390">To-do item</span></span> | <span data-ttu-id="9f0a3-391">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-391">None</span></span> |
|<span data-ttu-id="9f0a3-392">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-392">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="9f0a3-393">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="9f0a3-393">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="9f0a3-394">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-394">None</span></span> | <span data-ttu-id="9f0a3-395">None</span><span class="sxs-lookup"><span data-stu-id="9f0a3-395">None</span></span>|

<span data-ttu-id="9f0a3-396">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-396">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="9f0a3-402">必要條件</span><span class="sxs-lookup"><span data-stu-id="9f0a3-402">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-403">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-403">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-404">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-404">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-405">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-405">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="9f0a3-406">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="9f0a3-406">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-407">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-407">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-408">從 [檔案]  功能表選取 [新增]  > [專案]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-408">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="9f0a3-409">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-409">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="9f0a3-410">將專案命名為 *TodoApi*，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-410">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="9f0a3-411">在 [建立新的 ASP.NET Core Web 應用程式]  對話方塊中，確認選取 [.NET Core]  和 [ASP.NET Core 2.2]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-411">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="9f0a3-412">選取 **API** 範本，然後按一下 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-412">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="9f0a3-413">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-413">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-415">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-415">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f0a3-416">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-416">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="9f0a3-417">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-417">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="9f0a3-418">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-418">Run the following commands:</span></span>

   ```console
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="9f0a3-419">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-419">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="9f0a3-420">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-420">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-421">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-421">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9f0a3-422">選取 [檔案]  > [新增解決方案]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-422">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="9f0a3-424">選取[.Net Core]  > [應用程式]  > [API]  > [下一步]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-424">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="9f0a3-426">在 [設定您的新 ASP.NET Core Web API]  對話方塊中，接受 [目標 Framework]  的預設 \* *.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-426">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="9f0a3-427">針對 [專案名稱]  輸入 *TodoApi*，然後選取 [建立]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-427">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="9f0a3-429">測試 API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-429">Test the API</span></span>

<span data-ttu-id="9f0a3-430">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-430">The project template creates a `values` API.</span></span> <span data-ttu-id="9f0a3-431">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-431">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-432">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-432">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="9f0a3-433">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-433">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="9f0a3-434">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-434">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="9f0a3-435">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-435">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="9f0a3-436">在接著出現的 [安全性警告]  對話方塊中，選取 [是]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-436">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-437">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-437">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="9f0a3-438">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-438">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="9f0a3-439">在瀏覽器中，前往下列 URL：[https://localhost:5001/api/values](https://localhost:5001/api/values)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-439">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-440">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="9f0a3-441">選取 [執行]   > [開始偵錯]  來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-441">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="9f0a3-442">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-442">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="9f0a3-443">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-443">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="9f0a3-444">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-444">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="9f0a3-445">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-445">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="9f0a3-446">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="9f0a3-446">Add a model class</span></span>

<span data-ttu-id="9f0a3-447">「模型」  是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-447">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="9f0a3-448">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-448">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-449">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-449">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-450">在 [方案總管]  中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-450">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="9f0a3-451">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-451">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="9f0a3-452">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-452">Name the folder *Models*.</span></span>

* <span data-ttu-id="9f0a3-453">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-453">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="9f0a3-454">將類別命名為 *TodoItem*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-454">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="9f0a3-455">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-455">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="9f0a3-456">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="9f0a3-456">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="9f0a3-457">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-457">Add a folder named *Models*.</span></span>

* <span data-ttu-id="9f0a3-458">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-458">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="9f0a3-459">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-459">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="9f0a3-460">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-460">Right-click the project.</span></span> <span data-ttu-id="9f0a3-461">選取 [新增]   > [新增資料夾]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-461">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="9f0a3-462">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-462">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="9f0a3-464">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]  > [新增檔案]  > [一般]  > [空類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-464">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="9f0a3-465">將類別命名為 *TodoItem*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-465">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="9f0a3-466">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-466">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="9f0a3-467">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-467">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="9f0a3-468">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-468">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="9f0a3-469">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9f0a3-469">Add a database context</span></span>

<span data-ttu-id="9f0a3-470">「資料庫內容」  是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-470">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="9f0a3-471">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-471">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-472">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-472">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-473">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]   > [類別]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-473">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="9f0a3-474">將類別命名為 *TodoContext*，然後按一下 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-474">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9f0a3-475">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-475">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9f0a3-476">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-476">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="9f0a3-477">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-477">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="9f0a3-478">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="9f0a3-478">Register the database context</span></span>

<span data-ttu-id="9f0a3-479">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-479">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="9f0a3-480">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-480">The container provides the service to controllers.</span></span>

<span data-ttu-id="9f0a3-481">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-481">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="9f0a3-482">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-482">The preceding code:</span></span>

* <span data-ttu-id="9f0a3-483">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-483">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="9f0a3-484">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-484">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="9f0a3-485">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-485">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="9f0a3-486">新增控制器</span><span class="sxs-lookup"><span data-stu-id="9f0a3-486">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-488">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-488">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="9f0a3-489">選取 [新增]  > [新增項目]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-489">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="9f0a3-490">在 [新增項目]  對話方塊中，選取 [API 控制器類別]  範本。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-490">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="9f0a3-491">將類別命名為 *TodoController*，然後選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-491">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9f0a3-493">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-493">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9f0a3-494">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-494">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="9f0a3-495">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-495">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="9f0a3-496">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-496">The preceding code:</span></span>

* <span data-ttu-id="9f0a3-497">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-497">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="9f0a3-498">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-498">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="9f0a3-499">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-499">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="9f0a3-500">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-500">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="9f0a3-501">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-501">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="9f0a3-502">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-502">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="9f0a3-503">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-503">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="9f0a3-504">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-504">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="9f0a3-505">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-505">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="9f0a3-506">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-506">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="9f0a3-507">新增 Get 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-507">Add Get methods</span></span>

<span data-ttu-id="9f0a3-508">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-508">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="9f0a3-509">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-509">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="9f0a3-510">如果應用程式仍在執行，請將其停止。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-510">Stop the app if it's still running.</span></span> <span data-ttu-id="9f0a3-511">然後重新予以執行以包含最新的變更。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-511">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="9f0a3-512">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-512">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="9f0a3-513">例如：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-513">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="9f0a3-514">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-514">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="9f0a3-515">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="9f0a3-515">Routing and URL paths</span></span>

<span data-ttu-id="9f0a3-516">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-516">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="9f0a3-517">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-517">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="9f0a3-518">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-518">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="9f0a3-519">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-519">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="9f0a3-520">在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-520">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="9f0a3-521">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-521">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="9f0a3-522">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-522">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="9f0a3-523">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-523">This sample doesn't use a template.</span></span> <span data-ttu-id="9f0a3-524">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-524">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="9f0a3-525">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-525">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="9f0a3-526">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-526">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="9f0a3-527">傳回值</span><span class="sxs-lookup"><span data-stu-id="9f0a3-527">Return values</span></span>

<span data-ttu-id="9f0a3-528">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-528">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="9f0a3-529">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-529">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="9f0a3-530">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-530">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="9f0a3-531">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-531">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="9f0a3-532">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-532">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="9f0a3-533">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-533">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="9f0a3-534">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-534">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="9f0a3-535">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-535">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="9f0a3-536">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-536">Returning `item` results in an HTTP 200 response.</span></span>


## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="9f0a3-537">測試 GetTodoItems 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-537">Test the GetTodoItems method</span></span>

<span data-ttu-id="9f0a3-538">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-538">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="9f0a3-539">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="9f0a3-539">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="9f0a3-540">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-540">Start the web app.</span></span>
* <span data-ttu-id="9f0a3-541">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-541">Start Postman.</span></span>
* <span data-ttu-id="9f0a3-542">停用 [SSL certificate verification] \(SSL 憑證驗證\) </span><span class="sxs-lookup"><span data-stu-id="9f0a3-542">Disable **SSL certificate verification**</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="9f0a3-543">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="9f0a3-543">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="9f0a3-544">從 [檔案]  > [設定]  ([一般]  索引標籤)，停用 [SSL 憑證驗證]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-544">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="9f0a3-545">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="9f0a3-545">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="9f0a3-546">從 [Postman]   >  [喜好設定]  ([一般]  索引標籤)，停用 [SSL 憑證驗證]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-546">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="9f0a3-547">或者，選取扳手並選取 [設定]  ，然後停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-547">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="9f0a3-548">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-548">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="9f0a3-549">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-549">Create a new request.</span></span>
  * <span data-ttu-id="9f0a3-550">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-550">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="9f0a3-551">將要求 URL 設定為 `https://localhost:<port>/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-551">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="9f0a3-552">例如，`https://localhost:5001/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-552">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="9f0a3-553">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-553">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="9f0a3-554">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-554">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="9f0a3-556">新增 Create 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-556">Add a Create method</span></span>

<span data-ttu-id="9f0a3-557">在 *Controllers/TodoController.cs* 內部新增下列 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-557">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="9f0a3-558">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-558">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="9f0a3-559">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-559">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="9f0a3-560">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-560">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="9f0a3-561">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-561">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="9f0a3-562">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-562">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="9f0a3-563">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-563">Adds a `Location` header to the response.</span></span> <span data-ttu-id="9f0a3-564">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-564">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="9f0a3-565">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-565">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="9f0a3-566">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-566">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="9f0a3-567">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-567">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="9f0a3-568">測試 PostTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-568">Test the PostTodoItem method</span></span>

* <span data-ttu-id="9f0a3-569">建置專案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-569">Build the project.</span></span>
* <span data-ttu-id="9f0a3-570">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-570">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="9f0a3-571">選取 [本文]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-571">Select the **Body** tab.</span></span>
* <span data-ttu-id="9f0a3-572">選取 [原始]  選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-572">Select the **raw** radio button.</span></span>
* <span data-ttu-id="9f0a3-573">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-573">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="9f0a3-574">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-574">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="9f0a3-575">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-575">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="9f0a3-577">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-577">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="9f0a3-578">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="9f0a3-578">Test the location header URI</span></span>

* <span data-ttu-id="9f0a3-579">在 [回應]  窗格中選取 [標頭]  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-579">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="9f0a3-580">複製 [位置]  標頭值：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-580">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="9f0a3-582">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-582">Set the method to GET.</span></span>
* <span data-ttu-id="9f0a3-583">貼上 URI (例如 `https://localhost:5001/api/Todo/2`)</span><span class="sxs-lookup"><span data-stu-id="9f0a3-583">Paste the URI (for example, `https://localhost:5001/api/Todo/2`)</span></span>
* <span data-ttu-id="9f0a3-584">選取 [傳送]  。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-584">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="9f0a3-585">新增 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-585">Add a PutTodoItem method</span></span>

<span data-ttu-id="9f0a3-586">新增以下 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-586">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="9f0a3-587">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-587">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="9f0a3-588">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-588">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="9f0a3-589">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-589">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="9f0a3-590">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-590">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="9f0a3-591">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-591">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="9f0a3-592">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-592">Test the PutTodoItem method</span></span>

<span data-ttu-id="9f0a3-593">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-593">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="9f0a3-594">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-594">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="9f0a3-595">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-595">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="9f0a3-596">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-596">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="9f0a3-597">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-597">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="9f0a3-599">新增 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-599">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="9f0a3-600">新增以下 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-600">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="9f0a3-601">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-601">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="9f0a3-602">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="9f0a3-602">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="9f0a3-603">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-603">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="9f0a3-604">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-604">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="9f0a3-605">設定要刪除的物件 URI，例如 `https://localhost:5001/api/todo/1`</span><span class="sxs-lookup"><span data-stu-id="9f0a3-605">Set the URI of the object to delete, for example `https://localhost:5001/api/todo/1`</span></span>
* <span data-ttu-id="9f0a3-606">選取 [傳送] </span><span class="sxs-lookup"><span data-stu-id="9f0a3-606">Select **Send**</span></span>

<span data-ttu-id="9f0a3-607">範例應用程式可讓您刪除所有項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-607">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="9f0a3-608">但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-608">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-api-with-jquery"></a><span data-ttu-id="9f0a3-609">使用 jQuery 呼叫 API</span><span class="sxs-lookup"><span data-stu-id="9f0a3-609">Call the API with jQuery</span></span>

<span data-ttu-id="9f0a3-610">在本節中，將會新增 HTML 網頁，以使用 jQuery 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-610">In this section, an HTML page is added that uses jQuery to call the web api.</span></span> <span data-ttu-id="9f0a3-611">jQuery 會起始要求，並使用來自 API 回應的詳細資料更新頁面。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-611">jQuery initiates the request and updates the page with the details from the API's response.</span></span>

<span data-ttu-id="9f0a3-612">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-612">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="9f0a3-613">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-613">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="9f0a3-614">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-614">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="9f0a3-615">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-615">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="9f0a3-616">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-616">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="9f0a3-617">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-617">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="9f0a3-618">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-618">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="9f0a3-619">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-619">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="9f0a3-620">移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-620">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="9f0a3-621">您可以使用幾種方式來取得 jQuery。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-621">There are several ways to get jQuery.</span></span> <span data-ttu-id="9f0a3-622">在上述的程式碼片段中，從 CDN 載入程式庫。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-622">In the preceding snippet, the library is loaded from a CDN.</span></span>

<span data-ttu-id="9f0a3-623">此範例會呼叫 API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-623">This sample calls all of the CRUD methods of the API.</span></span> <span data-ttu-id="9f0a3-624">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-624">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="9f0a3-625">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="9f0a3-625">Get a list of to-do items</span></span>

<span data-ttu-id="9f0a3-626">JQuery [ajax](https://api.jquery.com/jquery.ajax/) 函式會將 `GET` 要求傳送至 API，API 則會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-626">The jQuery [ajax](https://api.jquery.com/jquery.ajax/) function sends a `GET` request to the API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="9f0a3-627">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-627">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="9f0a3-628">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-628">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="9f0a3-629">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-629">Add a to-do item</span></span>

<span data-ttu-id="9f0a3-630">[ajax](https://api.jquery.com/jquery.ajax/) 函式會傳送 `POST` 要求，並在要求本文中包含待辦事項。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-630">The [ajax](https://api.jquery.com/jquery.ajax/) function sends a `POST` request with the to-do item in the request body.</span></span> <span data-ttu-id="9f0a3-631">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-631">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="9f0a3-632">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-632">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="9f0a3-633">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-633">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="9f0a3-634">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-634">Update a to-do item</span></span>

<span data-ttu-id="9f0a3-635">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-635">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="9f0a3-636">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-636">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="9f0a3-637">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="9f0a3-637">Delete a to-do item</span></span>

<span data-ttu-id="9f0a3-638">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-638">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="9f0a3-639">其他資源</span><span class="sxs-lookup"><span data-stu-id="9f0a3-639">Additional resources</span></span>

<span data-ttu-id="9f0a3-640">[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-640">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="9f0a3-641">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="9f0a3-641">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="9f0a3-642">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="9f0a3-642">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="9f0a3-643">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="9f0a3-643">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
