---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 09/29/2019
uid: tutorials/first-web-api
ms.openlocfilehash: 7bb98fe5befa8eea80885d246da31ad87d5cfc2d
ms.sourcegitcommit: fe88748b762525cb490f7e39089a4760f6a73a24
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/30/2019
ms.locfileid: "71691206"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="f0134-103">教學課程：使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="f0134-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="f0134-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="f0134-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="f0134-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="f0134-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="f0134-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="f0134-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0134-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-107">Create a web API project.</span></span>
> * <span data-ttu-id="f0134-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="f0134-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f0134-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="f0134-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="f0134-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="f0134-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="f0134-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-111">Call the web API with Postman.</span></span>

<span data-ttu-id="f0134-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="f0134-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="f0134-113">總覽</span><span class="sxs-lookup"><span data-stu-id="f0134-113">Overview</span></span>

<span data-ttu-id="f0134-114">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="f0134-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f0134-115">API</span><span class="sxs-lookup"><span data-stu-id="f0134-115">API</span></span> | <span data-ttu-id="f0134-116">描述</span><span class="sxs-lookup"><span data-stu-id="f0134-116">Description</span></span> | <span data-ttu-id="f0134-117">要求本文</span><span class="sxs-lookup"><span data-stu-id="f0134-117">Request body</span></span> | <span data-ttu-id="f0134-118">回應本文</span><span class="sxs-lookup"><span data-stu-id="f0134-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f0134-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f0134-119">GET /api/TodoItems</span></span> | <span data-ttu-id="f0134-120">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-120">Get all to-do items</span></span> | <span data-ttu-id="f0134-121">None</span><span class="sxs-lookup"><span data-stu-id="f0134-121">None</span></span> | <span data-ttu-id="f0134-122">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="f0134-122">Array of to-do items</span></span>|
|<span data-ttu-id="f0134-123">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="f0134-123">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f0134-124">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="f0134-124">Get an item by ID</span></span> | <span data-ttu-id="f0134-125">None</span><span class="sxs-lookup"><span data-stu-id="f0134-125">None</span></span> | <span data-ttu-id="f0134-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-126">To-do item</span></span>|
|<span data-ttu-id="f0134-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f0134-127">POST /api/TodoItems</span></span> | <span data-ttu-id="f0134-128">新增項目</span><span class="sxs-lookup"><span data-stu-id="f0134-128">Add a new item</span></span> | <span data-ttu-id="f0134-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-129">To-do item</span></span> | <span data-ttu-id="f0134-130">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-130">To-do item</span></span> |
|<span data-ttu-id="f0134-131">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="f0134-131">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f0134-132">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-132">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f0134-133">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-133">To-do item</span></span> | <span data-ttu-id="f0134-134">None</span><span class="sxs-lookup"><span data-stu-id="f0134-134">None</span></span> |
|<span data-ttu-id="f0134-135">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-135">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f0134-136">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-136">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f0134-137">None</span><span class="sxs-lookup"><span data-stu-id="f0134-137">None</span></span> | <span data-ttu-id="f0134-138">None</span><span class="sxs-lookup"><span data-stu-id="f0134-138">None</span></span>|

<span data-ttu-id="f0134-139">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="f0134-139">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f0134-145">必要條件</span><span class="sxs-lookup"><span data-stu-id="f0134-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f0134-149">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="f0134-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-150">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-151">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f0134-151">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f0134-152">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0134-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f0134-153">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-153">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f0134-154">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="f0134-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="f0134-155">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-155">Select the **API** template and click **Create**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0134-158">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f0134-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f0134-159">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f0134-160">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="f0134-161">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="f0134-162">上述命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-162">The preceding commands:</span></span>

  * <span data-ttu-id="f0134-163">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="f0134-163">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="f0134-164">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f0134-164">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f0134-166">選取 [檔案] > [新增解決方案]。</span><span class="sxs-lookup"><span data-stu-id="f0134-166">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f0134-168">選取[.Net Core] > [應用程式] > [API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0134-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f0134-170">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，選取 \* *.NET Core 3.0* 的 [目標 Framework]。</span><span class="sxs-lookup"><span data-stu-id="f0134-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>

* <span data-ttu-id="f0134-171">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="f0134-173">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="f0134-174">測試 API</span><span class="sxs-lookup"><span data-stu-id="f0134-174">Test the API</span></span>

<span data-ttu-id="f0134-175">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="f0134-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="f0134-176">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f0134-178">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-178">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f0134-179">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="f0134-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f0134-180">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f0134-181">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f0134-183">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-183">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f0134-184">在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。</span><span class="sxs-lookup"><span data-stu-id="f0134-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f0134-186">選取 [執行] > [開始偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f0134-187">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f0134-188">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f0134-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f0134-189">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="f0134-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="f0134-190">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="f0134-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="f0134-191">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="f0134-191">Add a model class</span></span>

<span data-ttu-id="f0134-192">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f0134-193">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-194">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-195">在 [方案總管] 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f0134-196">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f0134-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f0134-197">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f0134-197">Name the folder *Models*.</span></span>

* <span data-ttu-id="f0134-198">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f0134-199">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-199">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f0134-200">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0134-202">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-202">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f0134-203">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f0134-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f0134-205">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-205">Right-click the project.</span></span> <span data-ttu-id="f0134-206">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f0134-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f0134-207">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f0134-207">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f0134-209">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f0134-210">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-210">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f0134-211">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f0134-212">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f0134-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f0134-213">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f0134-214">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f0134-214">Add a database context</span></span>

<span data-ttu-id="f0134-215">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f0134-216">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="f0134-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="f0134-218">新增 Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="f0134-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="f0134-219">在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="f0134-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="f0134-220">選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="f0134-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="f0134-221">在左窗格中選取 [ **microsoft.entityframeworkcore** ]。</span><span class="sxs-lookup"><span data-stu-id="f0134-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="f0134-222">選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="f0134-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="f0134-223">使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f0134-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 封裝管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="f0134-225">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f0134-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="f0134-226">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f0134-227">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f0134-228">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f0134-229">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f0134-230">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f0134-231">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f0134-231">Register the database context</span></span>

<span data-ttu-id="f0134-232">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f0134-232">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f0134-233">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="f0134-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="f0134-234">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="f0134-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="f0134-235">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-235">The preceding code:</span></span>

* <span data-ttu-id="f0134-236">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="f0134-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f0134-237">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="f0134-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f0134-238">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="f0134-239">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="f0134-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-240">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-241">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-241">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f0134-242">選取 [新增] > [新增 Scaffold 項目]。</span><span class="sxs-lookup"><span data-stu-id="f0134-242">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="f0134-243">選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="f0134-244">在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="f0134-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="f0134-245">在**模型類別**中選取 [ **TodoItem （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="f0134-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="f0134-246">選取**資料內容類別**中的 [ **TodoCoNtext （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="f0134-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="f0134-247">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f0134-248">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="f0134-249">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="f0134-250">上述命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-250">The preceding commands:</span></span>

* <span data-ttu-id="f0134-251">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="f0134-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="f0134-252">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="f0134-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="f0134-253">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="f0134-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="f0134-254">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-254">The generated code:</span></span>

* <span data-ttu-id="f0134-255">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f0134-256">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-256">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f0134-257">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f0134-258">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="f0134-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f0134-259">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="f0134-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f0134-260">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="f0134-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="f0134-261">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="f0134-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="f0134-262">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="f0134-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="f0134-263">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="f0134-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f0134-264">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="f0134-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f0134-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="f0134-266">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="f0134-267">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="f0134-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f0134-268">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。</span><span class="sxs-lookup"><span data-stu-id="f0134-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="f0134-269">`Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。</span><span class="sxs-lookup"><span data-stu-id="f0134-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="f0134-270">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="f0134-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f0134-271">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="f0134-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f0134-272">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="f0134-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="f0134-273">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="f0134-273">Install Postman</span></span>

<span data-ttu-id="f0134-274">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f0134-275">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="f0134-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="f0134-276">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-276">Start the web app.</span></span>
* <span data-ttu-id="f0134-277">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="f0134-277">Start Postman.</span></span>
* <span data-ttu-id="f0134-278">停用 [SSL certificate verification] \(SSL 憑證驗證\)</span><span class="sxs-lookup"><span data-stu-id="f0134-278">Disable **SSL certificate verification**</span></span>
* <span data-ttu-id="f0134-279">從 [檔案] > [設定] ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="f0134-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="f0134-280">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="f0134-281">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="f0134-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="f0134-282">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-282">Create a new request.</span></span>
* <span data-ttu-id="f0134-283">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="f0134-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f0134-284">選取 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f0134-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="f0134-285">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="f0134-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f0134-286">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="f0134-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f0134-287">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="f0134-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f0134-288">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-288">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f0134-290">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="f0134-290">Test the location header URI</span></span>

* <span data-ttu-id="f0134-291">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f0134-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f0134-292">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="f0134-292">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="f0134-294">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="f0134-294">Set the method to GET.</span></span>
* <span data-ttu-id="f0134-295">貼上 URI （例如，`https://localhost:5001/api/TodoItems/1`）。</span><span class="sxs-lookup"><span data-stu-id="f0134-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="f0134-296">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="f0134-297">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-297">Examine the GET methods</span></span>

<span data-ttu-id="f0134-298">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="f0134-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="f0134-299">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="f0134-300">例如:</span><span class="sxs-lookup"><span data-stu-id="f0134-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="f0134-301">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="f0134-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="f0134-302">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="f0134-302">Test Get with Postman</span></span>

* <span data-ttu-id="f0134-303">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-303">Create a new request.</span></span>
* <span data-ttu-id="f0134-304">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="f0134-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="f0134-305">將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="f0134-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="f0134-306">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="f0134-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="f0134-307">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f0134-308">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-308">Select **Send**.</span></span>

<span data-ttu-id="f0134-309">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-309">This app uses an in-memory database.</span></span> <span data-ttu-id="f0134-310">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="f0134-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="f0134-311">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="f0134-312">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="f0134-312">Routing and URL paths</span></span>

<span data-ttu-id="f0134-313">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="f0134-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f0134-314">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="f0134-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f0134-315">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="f0134-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="f0134-316">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="f0134-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f0134-317">在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="f0134-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="f0134-318">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="f0134-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f0134-319">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="f0134-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f0134-320">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="f0134-320">This sample doesn't use a template.</span></span> <span data-ttu-id="f0134-321">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="f0134-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f0134-322">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="f0134-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f0134-323">叫用 `GetTodoItem` 時，URL 中 `"{id}"` 的值會提供給其 @no__t 2 參數中的方法。</span><span class="sxs-lookup"><span data-stu-id="f0134-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f0134-324">傳回值</span><span class="sxs-lookup"><span data-stu-id="f0134-324">Return values</span></span>

<span data-ttu-id="f0134-325">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="f0134-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f0134-326">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="f0134-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f0134-327">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f0134-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f0134-328">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f0134-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f0134-329">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f0134-330">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="f0134-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f0134-331">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f0134-332">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="f0134-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f0134-333">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="f0134-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="f0134-334">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-334">The PutTodoItem method</span></span>

<span data-ttu-id="f0134-335">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="f0134-336">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="f0134-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f0134-337">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="f0134-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f0134-338">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="f0134-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f0134-339">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="f0134-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f0134-340">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f0134-341">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="f0134-342">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-342">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="f0134-343">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f0134-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f0134-344">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f0134-345">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="f0134-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f0134-346">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="f0134-346">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="f0134-348">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="f0134-349">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="f0134-350">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-350">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f0134-351">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-351">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f0134-352">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="f0134-352">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f0134-353">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="f0134-353">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f0134-354">設定要刪除之物件的 URI （例如 `https://localhost:5001/api/TodoItems/1`）。</span><span class="sxs-lookup"><span data-stu-id="f0134-354">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="f0134-355">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-355">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f0134-356">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="f0134-356">Call the web API with JavaScript</span></span>

<span data-ttu-id="f0134-357">請[參閱教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-javascript)。</span><span class="sxs-lookup"><span data-stu-id="f0134-357">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="f0134-358">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="f0134-358">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="f0134-359">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-359">Create a web API project.</span></span>
> * <span data-ttu-id="f0134-360">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="f0134-360">Add a model class and a database context.</span></span>
> * <span data-ttu-id="f0134-361">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="f0134-361">Add a controller.</span></span>
> * <span data-ttu-id="f0134-362">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="f0134-362">Add CRUD methods.</span></span>
> * <span data-ttu-id="f0134-363">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="f0134-363">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="f0134-364">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="f0134-364">Specify return values.</span></span>
> * <span data-ttu-id="f0134-365">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-365">Call the web API with Postman.</span></span>
> * <span data-ttu-id="f0134-366">使用 JavaScript 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-366">Call the web API with JavaScript.</span></span>

<span data-ttu-id="f0134-367">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-367">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="f0134-368">總覽</span><span class="sxs-lookup"><span data-stu-id="f0134-368">Overview</span></span>

<span data-ttu-id="f0134-369">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="f0134-369">This tutorial creates the following API:</span></span>

|<span data-ttu-id="f0134-370">API</span><span class="sxs-lookup"><span data-stu-id="f0134-370">API</span></span> | <span data-ttu-id="f0134-371">描述</span><span class="sxs-lookup"><span data-stu-id="f0134-371">Description</span></span> | <span data-ttu-id="f0134-372">要求本文</span><span class="sxs-lookup"><span data-stu-id="f0134-372">Request body</span></span> | <span data-ttu-id="f0134-373">回應本文</span><span class="sxs-lookup"><span data-stu-id="f0134-373">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="f0134-374">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f0134-374">GET /api/TodoItems</span></span> | <span data-ttu-id="f0134-375">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-375">Get all to-do items</span></span> | <span data-ttu-id="f0134-376">None</span><span class="sxs-lookup"><span data-stu-id="f0134-376">None</span></span> | <span data-ttu-id="f0134-377">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="f0134-377">Array of to-do items</span></span>|
|<span data-ttu-id="f0134-378">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="f0134-378">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="f0134-379">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="f0134-379">Get an item by ID</span></span> | <span data-ttu-id="f0134-380">None</span><span class="sxs-lookup"><span data-stu-id="f0134-380">None</span></span> | <span data-ttu-id="f0134-381">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-381">To-do item</span></span>|
|<span data-ttu-id="f0134-382">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="f0134-382">POST /api/TodoItems</span></span> | <span data-ttu-id="f0134-383">新增項目</span><span class="sxs-lookup"><span data-stu-id="f0134-383">Add a new item</span></span> | <span data-ttu-id="f0134-384">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-384">To-do item</span></span> | <span data-ttu-id="f0134-385">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-385">To-do item</span></span> |
|<span data-ttu-id="f0134-386">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="f0134-386">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="f0134-387">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-387">Update an existing item &nbsp;</span></span> | <span data-ttu-id="f0134-388">待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-388">To-do item</span></span> | <span data-ttu-id="f0134-389">None</span><span class="sxs-lookup"><span data-stu-id="f0134-389">None</span></span> |
|<span data-ttu-id="f0134-390">DELETE /api/TodoItems/{識別碼} &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-390">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="f0134-391">刪除項目 &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="f0134-391">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="f0134-392">None</span><span class="sxs-lookup"><span data-stu-id="f0134-392">None</span></span> | <span data-ttu-id="f0134-393">None</span><span class="sxs-lookup"><span data-stu-id="f0134-393">None</span></span>|

<span data-ttu-id="f0134-394">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="f0134-394">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="f0134-400">必要條件</span><span class="sxs-lookup"><span data-stu-id="f0134-400">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-401">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-401">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-402">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-402">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-403">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-403">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="f0134-404">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="f0134-404">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-405">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-405">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-406">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="f0134-406">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="f0134-407">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0134-407">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="f0134-408">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-408">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="f0134-409">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 2.2]。</span><span class="sxs-lookup"><span data-stu-id="f0134-409">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="f0134-410">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-410">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="f0134-411">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-411">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-413">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-413">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0134-414">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="f0134-414">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="f0134-415">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-415">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="f0134-416">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="f0134-416">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="f0134-417">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="f0134-417">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="f0134-418">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-418">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-419">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-419">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f0134-420">選取 [檔案] > [新增解決方案]。</span><span class="sxs-lookup"><span data-stu-id="f0134-420">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="f0134-422">選取[.Net Core] > [應用程式] > [API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="f0134-422">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="f0134-424">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，接受 [目標 Framework] 的預設 \* *.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="f0134-424">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="f0134-425">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="f0134-425">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="f0134-427">測試 API</span><span class="sxs-lookup"><span data-stu-id="f0134-427">Test the API</span></span>

<span data-ttu-id="f0134-428">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="f0134-428">The project template creates a `values` API.</span></span> <span data-ttu-id="f0134-429">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-429">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-430">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-430">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="f0134-431">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-431">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f0134-432">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="f0134-432">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="f0134-433">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-433">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="f0134-434">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="f0134-434">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-435">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-435">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="f0134-436">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-436">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="f0134-437">在瀏覽器中，前往下列 URL：[https://localhost:5001/api/values](https://localhost:5001/api/values)。</span><span class="sxs-lookup"><span data-stu-id="f0134-437">In a browser, go to following URL: [https://localhost:5001/api/values](https://localhost:5001/api/values).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-438">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-438">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="f0134-439">選取 [執行] > [開始偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-439">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="f0134-440">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-440">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="f0134-441">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f0134-441">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="f0134-442">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="f0134-442">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="f0134-443">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="f0134-443">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="f0134-444">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="f0134-444">Add a model class</span></span>

<span data-ttu-id="f0134-445">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-445">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="f0134-446">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-446">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-447">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-448">在 [方案總管] 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-448">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="f0134-449">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f0134-449">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f0134-450">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f0134-450">Name the folder *Models*.</span></span>

* <span data-ttu-id="f0134-451">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-451">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f0134-452">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-452">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="f0134-453">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-453">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="f0134-454">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="f0134-454">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="f0134-455">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-455">Add a folder named *Models*.</span></span>

* <span data-ttu-id="f0134-456">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="f0134-456">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="f0134-457">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-457">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="f0134-458">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-458">Right-click the project.</span></span> <span data-ttu-id="f0134-459">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="f0134-459">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="f0134-460">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="f0134-460">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="f0134-462">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-462">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="f0134-463">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-463">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="f0134-464">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-464">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="f0134-465">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="f0134-465">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="f0134-466">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-466">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="f0134-467">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f0134-467">Add a database context</span></span>

<span data-ttu-id="f0134-468">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-468">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="f0134-469">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="f0134-469">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-470">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-470">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-471">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="f0134-471">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="f0134-472">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-472">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f0134-473">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-473">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f0134-474">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-474">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="f0134-475">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-475">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="f0134-476">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="f0134-476">Register the database context</span></span>

<span data-ttu-id="f0134-477">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="f0134-477">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="f0134-478">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="f0134-478">The container provides the service to controllers.</span></span>

<span data-ttu-id="f0134-479">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="f0134-479">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="f0134-480">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-480">The preceding code:</span></span>

* <span data-ttu-id="f0134-481">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="f0134-481">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="f0134-482">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="f0134-482">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="f0134-483">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-483">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="f0134-484">新增控制器</span><span class="sxs-lookup"><span data-stu-id="f0134-484">Add a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-485">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-485">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-486">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-486">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="f0134-487">選取 [新增] > [新增項目]。</span><span class="sxs-lookup"><span data-stu-id="f0134-487">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="f0134-488">在 [新增項目] 對話方塊中，選取 [API 控制器類別] 範本。</span><span class="sxs-lookup"><span data-stu-id="f0134-488">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="f0134-489">將類別命名為 *TodoController*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="f0134-489">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f0134-491">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-491">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f0134-492">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-492">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="f0134-493">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-493">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="f0134-494">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-494">The preceding code:</span></span>

* <span data-ttu-id="f0134-495">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-495">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="f0134-496">使用 [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="f0134-496">Decorates the class with the [[ApiController]](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="f0134-497">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-497">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="f0134-498">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="f0134-498">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="f0134-499">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="f0134-499">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="f0134-500">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="f0134-500">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="f0134-501">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-501">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="f0134-502">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-502">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="f0134-503">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="f0134-503">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="f0134-504">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="f0134-504">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="f0134-505">新增 Get 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-505">Add Get methods</span></span>

<span data-ttu-id="f0134-506">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-506">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="f0134-507">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="f0134-507">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="f0134-508">如果應用程式仍在執行，請將其停止。</span><span class="sxs-lookup"><span data-stu-id="f0134-508">Stop the app if it's still running.</span></span> <span data-ttu-id="f0134-509">然後重新予以執行以包含最新的變更。</span><span class="sxs-lookup"><span data-stu-id="f0134-509">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="f0134-510">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-510">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="f0134-511">例如:</span><span class="sxs-lookup"><span data-stu-id="f0134-511">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="f0134-512">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="f0134-512">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="f0134-513">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="f0134-513">Routing and URL paths</span></span>

<span data-ttu-id="f0134-514">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) 屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="f0134-514">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="f0134-515">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="f0134-515">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="f0134-516">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="f0134-516">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="f0134-517">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="f0134-517">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="f0134-518">在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="f0134-518">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="f0134-519">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="f0134-519">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="f0134-520">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="f0134-520">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="f0134-521">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="f0134-521">This sample doesn't use a template.</span></span> <span data-ttu-id="f0134-522">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="f0134-522">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="f0134-523">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="f0134-523">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="f0134-524">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="f0134-524">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="f0134-525">傳回值</span><span class="sxs-lookup"><span data-stu-id="f0134-525">Return values</span></span>

<span data-ttu-id="f0134-526">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="f0134-526">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="f0134-527">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="f0134-527">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="f0134-528">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="f0134-528">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="f0134-529">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="f0134-529">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="f0134-530">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-530">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="f0134-531">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="f0134-531">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="f0134-532">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-532">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="f0134-533">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="f0134-533">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="f0134-534">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="f0134-534">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="f0134-535">測試 GetTodoItems 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-535">Test the GetTodoItems method</span></span>

<span data-ttu-id="f0134-536">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-536">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="f0134-537">安裝[Postman](https://www.getpostman.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="f0134-537">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="f0134-538">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="f0134-538">Start the web app.</span></span>
* <span data-ttu-id="f0134-539">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="f0134-539">Start Postman.</span></span>
* <span data-ttu-id="f0134-540">停用**SSL 憑證驗證**。</span><span class="sxs-lookup"><span data-stu-id="f0134-540">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="f0134-541">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="f0134-541">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="f0134-542">從 [檔案] > [設定] ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="f0134-542">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="f0134-543">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="f0134-543">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="f0134-544">從 [Postman]  >  [喜好設定] ([一般] 索引標籤)，停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="f0134-544">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="f0134-545">或者，選取扳手並選取 [設定]，然後停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="f0134-545">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="f0134-546">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-546">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="f0134-547">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-547">Create a new request.</span></span>
  * <span data-ttu-id="f0134-548">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="f0134-548">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="f0134-549">將要求 URL 設定為 `https://localhost:<port>/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="f0134-549">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="f0134-550">例如： `https://localhost:5001/api/todo` 。</span><span class="sxs-lookup"><span data-stu-id="f0134-550">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="f0134-551">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-551">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="f0134-552">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-552">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="f0134-554">新增 Create 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-554">Add a Create method</span></span>

<span data-ttu-id="f0134-555">在 *Controllers/TodoController.cs* 內部新增下列 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-555">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="f0134-556">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="f0134-556">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="f0134-557">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="f0134-557">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="f0134-558">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-558">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="f0134-559">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-559">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="f0134-560">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="f0134-560">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="f0134-561">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="f0134-561">Adds a `Location` header to the response.</span></span> <span data-ttu-id="f0134-562">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="f0134-562">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="f0134-563">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="f0134-563">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="f0134-564">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="f0134-564">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="f0134-565">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="f0134-565">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="f0134-566">測試 PostTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-566">Test the PostTodoItem method</span></span>

* <span data-ttu-id="f0134-567">建置專案。</span><span class="sxs-lookup"><span data-stu-id="f0134-567">Build the project.</span></span>
* <span data-ttu-id="f0134-568">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="f0134-568">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="f0134-569">選取 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f0134-569">Select the **Body** tab.</span></span>
* <span data-ttu-id="f0134-570">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="f0134-570">Select the **raw** radio button.</span></span>
* <span data-ttu-id="f0134-571">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="f0134-571">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="f0134-572">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="f0134-572">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="f0134-573">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-573">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="f0134-575">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="f0134-575">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="f0134-576">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="f0134-576">Test the location header URI</span></span>

* <span data-ttu-id="f0134-577">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f0134-577">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="f0134-578">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="f0134-578">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="f0134-580">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="f0134-580">Set the method to GET.</span></span>
* <span data-ttu-id="f0134-581">貼上 URI （例如，`https://localhost:5001/api/Todo/2`）。</span><span class="sxs-lookup"><span data-stu-id="f0134-581">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="f0134-582">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-582">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="f0134-583">新增 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-583">Add a PutTodoItem method</span></span>

<span data-ttu-id="f0134-584">新增以下 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-584">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="f0134-585">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="f0134-585">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="f0134-586">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="f0134-586">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="f0134-587">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="f0134-587">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="f0134-588">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="f0134-588">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="f0134-589">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-589">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="f0134-590">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-590">Test the PutTodoItem method</span></span>

<span data-ttu-id="f0134-591">此範例使用在每次應用程式啟動都必須起始的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="f0134-591">This sample uses an in-memory database that must be initialed each time the app is started.</span></span> <span data-ttu-id="f0134-592">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="f0134-592">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="f0134-593">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-593">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="f0134-594">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="f0134-594">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="f0134-595">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="f0134-595">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="f0134-597">新增 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-597">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="f0134-598">新增以下 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="f0134-598">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="f0134-599">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="f0134-599">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="f0134-600">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="f0134-600">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="f0134-601">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="f0134-601">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="f0134-602">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="f0134-602">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="f0134-603">設定要刪除之物件的 URI （例如 `https://localhost:5001/api/todo/1`）。</span><span class="sxs-lookup"><span data-stu-id="f0134-603">Set the URI of the object to delete (for example `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="f0134-604">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="f0134-604">Select **Send**.</span></span>

<span data-ttu-id="f0134-605">範例應用程式可讓您刪除所有項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-605">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="f0134-606">但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="f0134-606">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="f0134-607">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="f0134-607">Call the web API with JavaScript</span></span>

<span data-ttu-id="f0134-608">在此節中，將會新增 HTML 網頁，以使用 JavaScript 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="f0134-608">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="f0134-609">Fetch API 會起始要求。</span><span class="sxs-lookup"><span data-stu-id="f0134-609">The Fetch API initiates the request.</span></span> <span data-ttu-id="f0134-610">JavaScript 會使用來自 Web API 回應的詳細資料來更新頁面。</span><span class="sxs-lookup"><span data-stu-id="f0134-610">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="f0134-611">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="f0134-611">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="f0134-612">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="f0134-612">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="f0134-613">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="f0134-613">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="f0134-614">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="f0134-614">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="f0134-615">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="f0134-615">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="f0134-616">將其內容取代為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="f0134-616">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="f0134-617">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="f0134-617">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="f0134-618">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="f0134-618">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="f0134-619">移除 `launchUrl` 屬性，以強制應用程式於 *index.html* 處開啟 &mdash; 專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="f0134-619">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="f0134-620">此範例會呼叫 Web API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="f0134-620">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="f0134-621">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="f0134-621">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="f0134-622">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="f0134-622">Get a list of to-do items</span></span>

<span data-ttu-id="f0134-623">Fetch 會將 HTTP GET 要求傳送至 Web API，API 則會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="f0134-623">Fetch sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="f0134-624">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="f0134-624">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="f0134-625">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="f0134-625">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="f0134-626">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-626">Add a to-do item</span></span>

<span data-ttu-id="f0134-627">Fetch 會傳送 HTTP POST 要求，並在要求本文中包含待辦事項。</span><span class="sxs-lookup"><span data-stu-id="f0134-627">Fetch sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="f0134-628">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="f0134-628">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="f0134-629">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="f0134-629">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="f0134-630">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="f0134-630">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="f0134-631">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-631">Update a to-do item</span></span>

<span data-ttu-id="f0134-632">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="f0134-632">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="f0134-633">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="f0134-633">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="f0134-634">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="f0134-634">Delete a to-do item</span></span>

<span data-ttu-id="f0134-635">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="f0134-635">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="f0134-636">將驗證支援新增至 Web API</span><span class="sxs-lookup"><span data-stu-id="f0134-636">Add authentication support to a web API</span></span>

<span data-ttu-id="f0134-637">請參閱[IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)教學課程。</span><span class="sxs-lookup"><span data-stu-id="f0134-637">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f0134-638">其他資源</span><span class="sxs-lookup"><span data-stu-id="f0134-638">Additional resources</span></span>

<span data-ttu-id="f0134-639">[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="f0134-639">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="f0134-640">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="f0134-640">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="f0134-641">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="f0134-641">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="f0134-642">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="f0134-642">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
