---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: tutorials/first-web-api
ms.openlocfilehash: b4c88f5dc7853396448a2a6122f3652f92079e68
ms.sourcegitcommit: dd026eceee79e943bd6b4a37b144803b50617583
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72541781"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="e293d-103">教學課程：使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="e293d-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="e293d-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="e293d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="e293d-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="e293d-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

<span data-ttu-id="e293d-106">在本教學課程中，您將了解如何：</span><span class="sxs-lookup"><span data-stu-id="e293d-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="e293d-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="e293d-107">Create a web API project.</span></span>
> * <span data-ttu-id="e293d-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e293d-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="e293d-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="e293d-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="e293d-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="e293d-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="e293d-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="e293d-111">Call the web API with Postman.</span></span>

<span data-ttu-id="e293d-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="e293d-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="e293d-113">總覽</span><span class="sxs-lookup"><span data-stu-id="e293d-113">Overview</span></span>

<span data-ttu-id="e293d-114">本教學課程會建立包含下列端點的 Web API：</span><span class="sxs-lookup"><span data-stu-id="e293d-114">This tutorial creates a web API containing the following endpoints:</span></span>

|<span data-ttu-id="e293d-115">端點</span><span class="sxs-lookup"><span data-stu-id="e293d-115">Endpoint</span></span>                  |<span data-ttu-id="e293d-116">描述</span><span class="sxs-lookup"><span data-stu-id="e293d-116">Description</span></span>            |<span data-ttu-id="e293d-117">要求本文</span><span class="sxs-lookup"><span data-stu-id="e293d-117">Request body</span></span>|<span data-ttu-id="e293d-118">回應本文</span><span class="sxs-lookup"><span data-stu-id="e293d-118">Response body</span></span>       |
|--------------------------|-----------------------|------------|--------------------|
|<span data-ttu-id="e293d-119">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e293d-119">GET /api/TodoItems</span></span>        |<span data-ttu-id="e293d-120">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="e293d-120">Get all to-do items</span></span>    |<span data-ttu-id="e293d-121">None</span><span class="sxs-lookup"><span data-stu-id="e293d-121">None</span></span>        |<span data-ttu-id="e293d-122">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="e293d-122">Array of to-do items</span></span>|
|<span data-ttu-id="e293d-123">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="e293d-123">GET /api/TodoItems/{id}</span></span>   |<span data-ttu-id="e293d-124">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="e293d-124">Get an item by ID</span></span>      |<span data-ttu-id="e293d-125">None</span><span class="sxs-lookup"><span data-stu-id="e293d-125">None</span></span>        |<span data-ttu-id="e293d-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e293d-126">To-do item</span></span>          |
|<span data-ttu-id="e293d-127">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="e293d-127">POST /api/TodoItems</span></span>       |<span data-ttu-id="e293d-128">新增記錄</span><span class="sxs-lookup"><span data-stu-id="e293d-128">Add a new item</span></span>         |<span data-ttu-id="e293d-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e293d-129">To-do item</span></span>  |<span data-ttu-id="e293d-130">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e293d-130">To-do item</span></span>          |
|<span data-ttu-id="e293d-131">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="e293d-131">PUT /api/TodoItems/{id}</span></span>   |<span data-ttu-id="e293d-132">更新現有的專案</span><span class="sxs-lookup"><span data-stu-id="e293d-132">Update an existing item</span></span>|<span data-ttu-id="e293d-133">待辦事項</span><span class="sxs-lookup"><span data-stu-id="e293d-133">To-do item</span></span>  |<span data-ttu-id="e293d-134">None</span><span class="sxs-lookup"><span data-stu-id="e293d-134">None</span></span>                |
|<span data-ttu-id="e293d-135">刪除/api/TodoItems/{id}</span><span class="sxs-lookup"><span data-stu-id="e293d-135">DELETE /api/TodoItems/{id}</span></span>|<span data-ttu-id="e293d-136">刪除項目</span><span class="sxs-lookup"><span data-stu-id="e293d-136">Delete an item</span></span>         |<span data-ttu-id="e293d-137">None</span><span class="sxs-lookup"><span data-stu-id="e293d-137">None</span></span>        |<span data-ttu-id="e293d-138">None</span><span class="sxs-lookup"><span data-stu-id="e293d-138">None</span></span>                |

<span data-ttu-id="e293d-139">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="e293d-139">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="e293d-145">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="e293d-145">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-146">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e293d-147">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e293d-147">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e293d-148">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-148">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="e293d-149">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="e293d-149">Create a web project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-150">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-150">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e293d-151">從 [檔案] 功能表選取 [新增] > [專案]。</span><span class="sxs-lookup"><span data-stu-id="e293d-151">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="e293d-152">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]。</span><span class="sxs-lookup"><span data-stu-id="e293d-152">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
1. <span data-ttu-id="e293d-153">將專案命名為 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e293d-153">Name the project *TodoApi* and click **Create**.</span></span>
1. <span data-ttu-id="e293d-154">在 [建立新的 ASP.NET Core Web 應用程式] 對話方塊中，確認選取 [.NET Core] 和 [ASP.NET Core 3.0]。</span><span class="sxs-lookup"><span data-stu-id="e293d-154">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.0** are selected.</span></span> <span data-ttu-id="e293d-155">選取 **API** 範本，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e293d-155">Select the **API** template and click **Create**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e293d-157">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e293d-157">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e293d-158">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="e293d-158">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
1. <span data-ttu-id="e293d-159">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e293d-159">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
1. <span data-ttu-id="e293d-160">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e293d-160">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

1. <span data-ttu-id="e293d-161">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e293d-161">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="e293d-162">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e293d-162">The preceding commands:</span></span>

  * <span data-ttu-id="e293d-163">建立新的 Web API 專案，並在 Visual Studio Code 中開啟它。</span><span class="sxs-lookup"><span data-stu-id="e293d-163">Create a new web API project and open it in Visual Studio Code.</span></span>
  * <span data-ttu-id="e293d-164">新增下一節中所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e293d-164">Add the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e293d-165">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-165">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e293d-166">選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="e293d-166">Select **File** > **New Solution**.</span></span>

    ![macOS 新增方案](first-web-api-mac/_static/sln.png)

1. <span data-ttu-id="e293d-168">選取 .NET Core > 應用程式 > API > 下一步.</span><span class="sxs-lookup"><span data-stu-id="e293d-168">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

    ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
1. <span data-ttu-id="e293d-170">在 [設定您的新 ASP.NET Core Web API] 對話方塊中，選取 \* *.NET Core 3.0* 的 [目標 Framework]。</span><span class="sxs-lookup"><span data-stu-id="e293d-170">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.0*.</span></span>
1. <span data-ttu-id="e293d-171">針對 [專案名稱] 輸入 *TodoApi*，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="e293d-171">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="e293d-173">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e293d-173">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="e293d-174">測試 API</span><span class="sxs-lookup"><span data-stu-id="e293d-174">Test the API</span></span>

<span data-ttu-id="e293d-175">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="e293d-175">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="e293d-176">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-176">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-177">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-177">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e293d-178">按<kbd>Ctrl + F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-178">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="e293d-179">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="e293d-179">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="e293d-180">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e293d-180">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="e293d-181">在接著出現的 [安全性警告] 對話方塊中，選取 [是]。</span><span class="sxs-lookup"><span data-stu-id="e293d-181">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e293d-182">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e293d-182">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e293d-183">按<kbd>Ctrl + F5</kbd>執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-183">Press <kbd>Ctrl+F5</kbd> to run the app.</span></span> <span data-ttu-id="e293d-184">在瀏覽器中，前往下列 URL：[https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast)。</span><span class="sxs-lookup"><span data-stu-id="e293d-184">In a browser, go to following URL: [https://localhost:5001/WeatherForecast](https://localhost:5001/WeatherForecast).</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e293d-185">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-185">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e293d-186">選取 [執行] > [開始偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-186">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="e293d-187">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="e293d-187">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="e293d-188">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e293d-188">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="e293d-189">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="e293d-189">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="e293d-190">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="e293d-190">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="e293d-191">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="e293d-191">Add a model class</span></span>

<span data-ttu-id="e293d-192">「模型」是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="e293d-192">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="e293d-193">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="e293d-193">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-194">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-194">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e293d-195">在 [方案總管] 中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e293d-195">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="e293d-196">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="e293d-196">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e293d-197">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e293d-197">Name the folder *Models*.</span></span>
1. <span data-ttu-id="e293d-198">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="e293d-198">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e293d-199">將類別命名為 *TodoItem*，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e293d-199">Name the class *TodoItem* and select **Add**.</span></span>
1. <span data-ttu-id="e293d-200">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e293d-200">Replace the template code with the following code:</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="e293d-201">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e293d-201">Visual Studio Code</span></span>](#tab/visual-studio-code)

1. <span data-ttu-id="e293d-202">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="e293d-202">Add a folder named *Models*.</span></span>
1. <span data-ttu-id="e293d-203">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="e293d-203">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="e293d-204">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-204">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

1. <span data-ttu-id="e293d-205">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="e293d-205">Right-click the project.</span></span> <span data-ttu-id="e293d-206">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="e293d-206">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="e293d-207">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="e293d-207">Name the folder *Models*.</span></span>

    ![新增資料夾](first-web-api-mac/_static/folder.png)

1. <span data-ttu-id="e293d-209">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="e293d-209">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>
1. <span data-ttu-id="e293d-210">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e293d-210">Name the class *TodoItem*, and then click **New**.</span></span>
1. <span data-ttu-id="e293d-211">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="e293d-211">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="e293d-212">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="e293d-212">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="e293d-213">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e293d-213">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="e293d-214">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e293d-214">Add a database context</span></span>

<span data-ttu-id="e293d-215">「資料庫內容」是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="e293d-215">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="e293d-216">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="e293d-216">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-217">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-217">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="e293d-218">新增 Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="e293d-218">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

1. <span data-ttu-id="e293d-219">在 [工具] 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="e293d-219">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
1. <span data-ttu-id="e293d-220">選取 [瀏覽] 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="e293d-220">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
1. <span data-ttu-id="e293d-221">在左窗格中選取 [ **microsoft.entityframeworkcore** ]。</span><span class="sxs-lookup"><span data-stu-id="e293d-221">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
1. <span data-ttu-id="e293d-222">選取右窗格中的 [專案] 核取方塊，然後選取 [安裝]。</span><span class="sxs-lookup"><span data-stu-id="e293d-222">Select the **Project** check box in the right pane and then select **Install**.</span></span>
1. <span data-ttu-id="e293d-223">使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e293d-223">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 封裝管理員](first-web-api/_static/vs3nuget.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="e293d-225">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e293d-225">Add the TodoContext database context</span></span>

* <span data-ttu-id="e293d-226">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [類別]。</span><span class="sxs-lookup"><span data-stu-id="e293d-226">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="e293d-227">將類別命名為 *TodoContext*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e293d-227">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e293d-228">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-228">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="e293d-229">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e293d-229">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="e293d-230">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="e293d-230">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="e293d-231">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="e293d-231">Register the database context</span></span>

<span data-ttu-id="e293d-232">在 ASP.NET Core 中，服務（例如資料庫內容）必須向相依性[插入（DI）](xref:fundamentals/dependency-injection)容器註冊。</span><span class="sxs-lookup"><span data-stu-id="e293d-232">In ASP.NET Core, services such as the database context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="e293d-233">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="e293d-233">The container provides the service to controllers.</span></span>

<span data-ttu-id="e293d-234">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="e293d-234">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="e293d-235">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e293d-235">The preceding code:</span></span>

* <span data-ttu-id="e293d-236">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="e293d-236">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="e293d-237">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="e293d-237">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="e293d-238">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e293d-238">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="e293d-239">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="e293d-239">Scaffold a controller</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="e293d-240">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e293d-240">Visual Studio</span></span>](#tab/visual-studio)

1. <span data-ttu-id="e293d-241">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e293d-241">Right-click the *Controllers* folder.</span></span>
1. <span data-ttu-id="e293d-242">選取 [**加入** > **新增 scaffold 專案**]。</span><span class="sxs-lookup"><span data-stu-id="e293d-242">Select **Add** > **New Scaffolded Item**.</span></span>
1. <span data-ttu-id="e293d-243">選取 [使用 Entity Framework 執行動作的 API 控制器]，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e293d-243">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
1. <span data-ttu-id="e293d-244">在 [使用 Entity Framework 執行動作的 API 控制器] 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="e293d-244">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>
    * <span data-ttu-id="e293d-245">在**模型類別**中選取 [ **TodoItem （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="e293d-245">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
    * <span data-ttu-id="e293d-246">選取**資料內容類別**中的 [ **TodoCoNtext （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="e293d-246">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
    * <span data-ttu-id="e293d-247">選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="e293d-247">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mactabvisual-studio-codevisual-studio-mac"></a>[<span data-ttu-id="e293d-248">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e293d-248">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="e293d-249">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="e293d-249">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="e293d-250">上述命令：</span><span class="sxs-lookup"><span data-stu-id="e293d-250">The preceding commands:</span></span>

* <span data-ttu-id="e293d-251">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e293d-251">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="e293d-252">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="e293d-252">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="e293d-253">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="e293d-253">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="e293d-254">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e293d-254">The generated code:</span></span>

* <span data-ttu-id="e293d-255">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="e293d-255">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="e293d-256">使用 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) 屬性來裝飾類別。</span><span class="sxs-lookup"><span data-stu-id="e293d-256">Decorates the class with the [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute.</span></span> <span data-ttu-id="e293d-257">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="e293d-257">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="e293d-258">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="e293d-258">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="e293d-259">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="e293d-259">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="e293d-260">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="e293d-260">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="e293d-261">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="e293d-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="e293d-262">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="e293d-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="e293d-263">上述程式碼是 HTTP POST 方法，如 [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="e293d-263">The preceding code is an HTTP POST method, as indicated by the [[HttpPost]](xref:Microsoft.AspNetCore.Mvc.HttpPostAttribute) attribute.</span></span> <span data-ttu-id="e293d-264">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="e293d-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="e293d-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="e293d-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="e293d-266">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e293d-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="e293d-267">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="e293d-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="e293d-268">將 [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增到回應。</span><span class="sxs-lookup"><span data-stu-id="e293d-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="e293d-269">`Location` 標頭會指定新建待辦事項的 [URI](https://developer.mozilla.org/docs/Glossary/URI)。</span><span class="sxs-lookup"><span data-stu-id="e293d-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="e293d-270">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="e293d-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="e293d-271">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="e293d-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="e293d-272">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="e293d-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="e293d-273">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="e293d-273">Install Postman</span></span>

<span data-ttu-id="e293d-274">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="e293d-274">This tutorial uses Postman to test the web API.</span></span>

1. <span data-ttu-id="e293d-275">安裝 [Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="e293d-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
1. <span data-ttu-id="e293d-276">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-276">Start the web app.</span></span>
1. <span data-ttu-id="e293d-277">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="e293d-277">Start Postman.</span></span>
1. <span data-ttu-id="e293d-278">停用 [SSL certificate verification] \(SSL 憑證驗證\)</span><span class="sxs-lookup"><span data-stu-id="e293d-278">Disable **SSL certificate verification**</span></span>
1. <span data-ttu-id="e293d-279">從 [檔案 \*\* >  設定**] （ **[一般**] 索引標籤），停用**SSL 憑證驗證\*\*。</span><span class="sxs-lookup"><span data-stu-id="e293d-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

    > [!WARNING]
    > <span data-ttu-id="e293d-280">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="e293d-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="e293d-281">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="e293d-281">Test PostTodoItem with Postman</span></span>

1. <span data-ttu-id="e293d-282">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e293d-282">Create a new request.</span></span>
1. <span data-ttu-id="e293d-283">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="e293d-283">Set the HTTP method to `POST`.</span></span>
1. <span data-ttu-id="e293d-284">選取 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e293d-284">Select the **Body** tab.</span></span>
1. <span data-ttu-id="e293d-285">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="e293d-285">Select the **raw** radio button.</span></span>
1. <span data-ttu-id="e293d-286">將類型設定為 **JSON (application/json)** 。</span><span class="sxs-lookup"><span data-stu-id="e293d-286">Set the type to **JSON (application/json)**.</span></span>
1. <span data-ttu-id="e293d-287">在 [要求本文] 中，輸入 JSON 做為待辦事項：</span><span class="sxs-lookup"><span data-stu-id="e293d-287">In the request body, enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

1. <span data-ttu-id="e293d-288">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="e293d-288">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="e293d-290">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="e293d-290">Test the location header URI</span></span>

1. <span data-ttu-id="e293d-291">在 [回應] 窗格中選取 [標頭] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="e293d-291">Select the **Headers** tab in the **Response** pane.</span></span>
1. <span data-ttu-id="e293d-292">複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="e293d-292">Copy the **Location** header value:</span></span>

    ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/create.png)

1. <span data-ttu-id="e293d-294">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="e293d-294">Set the method to GET.</span></span>
1. <span data-ttu-id="e293d-295">貼上 URI （例如 `https://localhost:5001/api/TodoItems/1`）。</span><span class="sxs-lookup"><span data-stu-id="e293d-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="e293d-296">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="e293d-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="e293d-297">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="e293d-297">Examine the GET methods</span></span>

<span data-ttu-id="e293d-298">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="e293d-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="e293d-299">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="e293d-300">例如:</span><span class="sxs-lookup"><span data-stu-id="e293d-300">For example:</span></span>

* [https://localhost:5001/api/TodoItems](https://localhost:5001/api/TodoItems)
* [https://localhost:5001/api/TodoItems/1](https://localhost:5001/api/TodoItems/1)

<span data-ttu-id="e293d-301">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="e293d-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="e293d-302">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="e293d-302">Test Get with Postman</span></span>

1. <span data-ttu-id="e293d-303">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="e293d-303">Create a new request.</span></span>
1. <span data-ttu-id="e293d-304">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="e293d-304">Set the HTTP method to **GET**.</span></span>
1. <span data-ttu-id="e293d-305">將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="e293d-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="e293d-306">例如，`https://localhost:5001/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="e293d-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
1. <span data-ttu-id="e293d-307">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)。</span><span class="sxs-lookup"><span data-stu-id="e293d-307">Set **Two pane view** in Postman.</span></span>
1. <span data-ttu-id="e293d-308">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="e293d-308">Select **Send**.</span></span>

<span data-ttu-id="e293d-309">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e293d-309">This app uses an in-memory database.</span></span> <span data-ttu-id="e293d-310">如果應用程式已停止並啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="e293d-310">If the app is stopped and started, the preceding GET request won't return any data.</span></span> <span data-ttu-id="e293d-311">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="e293d-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="e293d-312">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="e293d-312">Routing and URL paths</span></span>

<span data-ttu-id="e293d-313">[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="e293d-313">The [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="e293d-314">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="e293d-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="e293d-315">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="e293d-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="e293d-316">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="e293d-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="e293d-317">在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="e293d-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="e293d-318">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="e293d-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="e293d-319">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="e293d-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="e293d-320">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="e293d-320">This sample doesn't use a template.</span></span> <span data-ttu-id="e293d-321">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e293d-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="e293d-322">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="e293d-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="e293d-323">叫用 `GetTodoItem` 時，URL 中 `"{id}"` 的值會在其 `id` 參數中提供給方法。</span><span class="sxs-lookup"><span data-stu-id="e293d-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="e293d-324">傳回值</span><span class="sxs-lookup"><span data-stu-id="e293d-324">Return values</span></span>

<span data-ttu-id="e293d-325">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="e293d-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="e293d-326">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="e293d-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="e293d-327">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="e293d-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="e293d-328">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e293d-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="e293d-329">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="e293d-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="e293d-330">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="e293d-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="e293d-331">如果沒有專案符合所要求的識別碼，方法會傳回 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="e293d-331">If no item matches the requested ID, the method returns a 404 <xref:Microsoft.AspNetCore.Mvc.ControllerBase.NotFound> error code.</span></span>
* <span data-ttu-id="e293d-332">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="e293d-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="e293d-333">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="e293d-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="e293d-334">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e293d-334">The PutTodoItem method</span></span>

<span data-ttu-id="e293d-335">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e293d-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="e293d-336">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="e293d-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="e293d-337">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="e293d-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="e293d-338">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="e293d-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="e293d-339">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="e293d-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="e293d-340">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="e293d-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="e293d-341">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e293d-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="e293d-342">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="e293d-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="e293d-343">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e293d-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="e293d-344">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="e293d-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="e293d-345">更新識別碼為1的待辦事項。</span><span class="sxs-lookup"><span data-stu-id="e293d-345">Update the to-do item that has an ID of 1.</span></span> <span data-ttu-id="e293d-346">將其名稱設為 "feed 魚"：</span><span class="sxs-lookup"><span data-stu-id="e293d-346">Set its name to "feed fish":</span></span>

```json
{
  "ID":1,
  "name":"feed fish",
  "isComplete":true
}
```

<span data-ttu-id="e293d-347">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="e293d-347">The following image shows the Postman update:</span></span>

![顯示 204 (沒有內容) 回應的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="e293d-349">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e293d-349">The DeleteTodoItem method</span></span>

<span data-ttu-id="e293d-350">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="e293d-350">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

<span data-ttu-id="e293d-351">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="e293d-351">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="e293d-352">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="e293d-352">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="e293d-353">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="e293d-353">Use Postman to delete a to-do item:</span></span>

1. <span data-ttu-id="e293d-354">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="e293d-354">Set the method to `DELETE`.</span></span>
1. <span data-ttu-id="e293d-355">設定要刪除之物件的 URI （例如，`https://localhost:5001/api/TodoItems/1`）。</span><span class="sxs-lookup"><span data-stu-id="e293d-355">Set the URI of the object to delete (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
1. <span data-ttu-id="e293d-356">選取 [傳送]。</span><span class="sxs-lookup"><span data-stu-id="e293d-356">Select **Send**.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="e293d-357">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="e293d-357">Call the web API with JavaScript</span></span>

<span data-ttu-id="e293d-358">請參閱[教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-javascript)。</span><span class="sxs-lookup"><span data-stu-id="e293d-358">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="e293d-359">將驗證支援新增至 Web API</span><span class="sxs-lookup"><span data-stu-id="e293d-359">Add authentication support to a web API</span></span>

<span data-ttu-id="e293d-360">請參閱[IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html)教學課程。</span><span class="sxs-lookup"><span data-stu-id="e293d-360">See the [IdentityServer4](https://identityserver4.readthedocs.io/en/latest/quickstarts/0_overview.html) tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e293d-361">其他資源</span><span class="sxs-lookup"><span data-stu-id="e293d-361">Additional resources</span></span>

<span data-ttu-id="e293d-362">[檢視或下載本教學課程的範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="e293d-362">[View or download sample code for this tutorial](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="e293d-363">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="e293d-363">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="e293d-364">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="e293d-364">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="e293d-365">這個教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="e293d-365">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
