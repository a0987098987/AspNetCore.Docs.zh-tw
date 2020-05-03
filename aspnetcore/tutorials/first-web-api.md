---
title: 教學課程：使用 ASP.NET Core 建立 Web API
author: rick-anderson
description: 了解如何使用 ASP.NET Core 建置 Web API。
ms.author: riande
ms.custom: mvc
ms.date: 2/25/2020
uid: tutorials/first-web-api
ms.openlocfilehash: 4e205c737f606579590854b679e669cbdd0cd5ab
ms.sourcegitcommit: c19e388c83c981232e6f128d97440262adfe06e2
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/02/2020
ms.locfileid: "82727800"
---
# <a name="tutorial-create-a-web-api-with-aspnet-core"></a><span data-ttu-id="77fd0-103">教學課程：使用 ASP.NET Core 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="77fd0-103">Tutorial: Create a web API with ASP.NET Core</span></span>

<span data-ttu-id="77fd0-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)、 [Kirk Larkin](https://twitter.com/serpent5)和[Mike Wasson](https://github.com/mikewasson)</span><span class="sxs-lookup"><span data-stu-id="77fd0-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Kirk Larkin](https://twitter.com/serpent5), and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="77fd0-105">本教學課程將教導您使用 ASP.NET Core 建立 Web API 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="77fd0-105">This tutorial teaches the basics of building a web API with ASP.NET Core.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="77fd0-106">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="77fd0-106">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77fd0-107">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-107">Create a web API project.</span></span>
> * <span data-ttu-id="77fd0-108">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="77fd0-108">Add a model class and a database context.</span></span>
> * <span data-ttu-id="77fd0-109">使用 CRUD 方法 Scaffold 控制器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-109">Scaffold a controller with CRUD methods.</span></span>
> * <span data-ttu-id="77fd0-110">設定路由、URL 路徑和傳回值。</span><span class="sxs-lookup"><span data-stu-id="77fd0-110">Configure routing, URL paths, and return values.</span></span>
> * <span data-ttu-id="77fd0-111">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-111">Call the web API with Postman.</span></span>

<span data-ttu-id="77fd0-112">結束時，您會有一個 Web API，可以管理儲存在資料庫中的「待辦事項」。</span><span class="sxs-lookup"><span data-stu-id="77fd0-112">At the end, you have a web API that can manage "to-do" items stored in a database.</span></span>

## <a name="overview"></a><span data-ttu-id="77fd0-113">概觀</span><span class="sxs-lookup"><span data-stu-id="77fd0-113">Overview</span></span>

<span data-ttu-id="77fd0-114">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="77fd0-114">This tutorial creates the following API:</span></span>

|<span data-ttu-id="77fd0-115">API</span><span class="sxs-lookup"><span data-stu-id="77fd0-115">API</span></span> | <span data-ttu-id="77fd0-116">描述</span><span class="sxs-lookup"><span data-stu-id="77fd0-116">Description</span></span> | <span data-ttu-id="77fd0-117">Request body</span><span class="sxs-lookup"><span data-stu-id="77fd0-117">Request body</span></span> | <span data-ttu-id="77fd0-118">Response body</span><span class="sxs-lookup"><span data-stu-id="77fd0-118">Response body</span></span> |
|--- | ---- | ---- | ---- |
|`GET /api/TodoItems` | <span data-ttu-id="77fd0-119">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-119">Get all to-do items</span></span> | <span data-ttu-id="77fd0-120">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-120">None</span></span> | <span data-ttu-id="77fd0-121">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="77fd0-121">Array of to-do items</span></span>|
|`GET /api/TodoItems/{id}` | <span data-ttu-id="77fd0-122">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="77fd0-122">Get an item by ID</span></span> | <span data-ttu-id="77fd0-123">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-123">None</span></span> | <span data-ttu-id="77fd0-124">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-124">To-do item</span></span>|
|`POST /api/TodoItems` | <span data-ttu-id="77fd0-125">新增記錄</span><span class="sxs-lookup"><span data-stu-id="77fd0-125">Add a new item</span></span> | <span data-ttu-id="77fd0-126">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-126">To-do item</span></span> | <span data-ttu-id="77fd0-127">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-127">To-do item</span></span> |
|`PUT /api/TodoItems/{id}` | <span data-ttu-id="77fd0-128">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-128">Update an existing item &nbsp;</span></span> | <span data-ttu-id="77fd0-129">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-129">To-do item</span></span> | <span data-ttu-id="77fd0-130">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-130">None</span></span> |
|<span data-ttu-id="77fd0-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-131">`DELETE /api/TodoItems/{id}` &nbsp; &nbsp;</span></span> | <span data-ttu-id="77fd0-132">刪除專案&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-132">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="77fd0-133">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-133">None</span></span> | <span data-ttu-id="77fd0-134">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-134">None</span></span>|

<span data-ttu-id="77fd0-135">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="77fd0-135">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="77fd0-141">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="77fd0-141">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-142">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-142">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.1.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-143">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-143">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.1.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-144">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-144">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.1.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="77fd0-145">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="77fd0-145">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-146">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-146">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-147">從 [檔案]\*\*\*\* 功能表選取 [新增]**[專案]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-147">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="77fd0-148">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-148">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="77fd0-149">將專案命名為 *TodoApi*，然後按一下 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-149">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="77fd0-150">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.1** ]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-150">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="77fd0-151">選取 **API** 範本，然後按一下 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-151">Select the **API** template and click **Create**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs3.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-153">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-153">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77fd0-154">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-154">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="77fd0-155">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-155">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="77fd0-156">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-156">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   cd TodoApi
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   code -r ../TodoApi
   ```

* <span data-ttu-id="77fd0-157">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-157">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

  <span data-ttu-id="77fd0-158">上述命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-158">The preceding commands:</span></span>

  * <span data-ttu-id="77fd0-159">建立新的 Web API 專案，然後在 Visual Studio Code 中予以開啟。</span><span class="sxs-lookup"><span data-stu-id="77fd0-159">Creates a new web API project and opens it in Visual Studio Code.</span></span>
  * <span data-ttu-id="77fd0-160">新增下一節需要的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="77fd0-160">Adds the NuGet packages which are required in the next section.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-161">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-161">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77fd0-162">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-162">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="77fd0-164">選取[.Net Core]\*\* [應用程式]\*\* > \*\* [API]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-164">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="77fd0-166">在 [**設定您的新 ASP.NET Core WEB API** ] 對話方塊中，選取 [**目標 Framework** ] \* [*.net Core 3.1*]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-166">In the **Configure your new ASP.NET Core Web API** dialog, select **Target Framework** of \**.NET Core 3.1*.</span></span>

* <span data-ttu-id="77fd0-167">針對 [專案名稱]\*\*\*\* 輸入 *TodoApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-167">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

[!INCLUDE[](~/includes/mac-terminal-access.md)]

<span data-ttu-id="77fd0-169">在專案資料夾中開啟命令終端機，然後執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-169">Open a command terminal in the project folder and run the following commands:</span></span>

   ```dotnetcli
   dotnet add package Microsoft.EntityFrameworkCore.SqlServer
   dotnet add package Microsoft.EntityFrameworkCore.InMemory
   ```

---

### <a name="test-the-api"></a><span data-ttu-id="77fd0-170">測試 API</span><span class="sxs-lookup"><span data-stu-id="77fd0-170">Test the API</span></span>

<span data-ttu-id="77fd0-171">專案範本會建立 `WeatherForecast` API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-171">The project template creates a `WeatherForecast` API.</span></span> <span data-ttu-id="77fd0-172">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-172">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-173">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-173">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77fd0-174">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-174">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="77fd0-175">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/WeatherForecast`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="77fd0-175">Visual Studio launches a browser and navigates to `https://localhost:<port>/WeatherForecast`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="77fd0-176">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-176">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="77fd0-177">在接著出現的 [安全性警告]\*\*\*\* 對話方塊中，選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-177">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-178">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-178">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="77fd0-179">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-179">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="77fd0-180">在瀏覽器中，前往下列 URL：`https://localhost:5001/WeatherForecast`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-180">In a browser, go to following URL: `https://localhost:5001/WeatherForecast`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-181">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-181">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="77fd0-182">選取 [**執行** > ] [**開始調試**程式] 以啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-182">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="77fd0-183">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-183">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="77fd0-184">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-184">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="77fd0-185">將 `/WeatherForecast` 附加至 URL (將 URL 變更為 `https://localhost:<port>/WeatherForecast`)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-185">Append `/WeatherForecast` to the URL (change the URL to `https://localhost:<port>/WeatherForecast`).</span></span>

---

<span data-ttu-id="77fd0-186">系統會傳回與下列類似的 JSON：</span><span class="sxs-lookup"><span data-stu-id="77fd0-186">JSON similar to the following is returned:</span></span>

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

## <a name="add-a-model-class"></a><span data-ttu-id="77fd0-187">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="77fd0-187">Add a model class</span></span>

<span data-ttu-id="77fd0-188">「模型」\*\* 是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-188">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="77fd0-189">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-189">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-190">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-190">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-191">在 [方案總管]\*\*\*\* 中，於專案上按一下滑鼠右鍵。</span><span class="sxs-lookup"><span data-stu-id="77fd0-191">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="77fd0-192"> > 選取 **\[\*\*\**新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-192">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="77fd0-193">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-193">Name the folder *Models*.</span></span>

* <span data-ttu-id="77fd0-194">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-194">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="77fd0-195">將類別命名為 *TodoItem*，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-195">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="77fd0-196">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-196">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-197">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-197">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77fd0-198">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-198">Add a folder named *Models*.</span></span>

* <span data-ttu-id="77fd0-199">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="77fd0-199">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-200">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-200">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77fd0-201">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-201">Right-click the project.</span></span> <span data-ttu-id="77fd0-202"> > 選取 **\[\*\*\**新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-202">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="77fd0-203">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-203">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="77fd0-205">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]**[新增檔案]** > **[一般]** > **[空類別]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-205">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="77fd0-206">將類別命名為 *TodoItem*，然後按一下 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-206">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="77fd0-207">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-207">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoItem.cs?name=snippet)]

<span data-ttu-id="77fd0-208">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="77fd0-208">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="77fd0-209">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-209">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="77fd0-210">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="77fd0-210">Add a database context</span></span>

<span data-ttu-id="77fd0-211">「資料庫內容」\*\* 是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-211">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="77fd0-212">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="77fd0-212">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-213">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-213">Visual Studio</span></span>](#tab/visual-studio)

### <a name="add-microsoftentityframeworkcoresqlserver"></a><span data-ttu-id="77fd0-214">新增 Microsoft.EntityFrameworkCore.SqlServer</span><span class="sxs-lookup"><span data-stu-id="77fd0-214">Add Microsoft.EntityFrameworkCore.SqlServer</span></span>

* <span data-ttu-id="77fd0-215">在 [工具]\*\*\*\* 功能表上，選取 [NuGet 套件管理員] > [管理解決方案的 NuGet 套件]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-215">From the **Tools** menu, select **NuGet Package Manager > Manage NuGet Packages for Solution**.</span></span>
* <span data-ttu-id="77fd0-216">選取 [瀏覽]\*\*\*\* 索引標籤，然後在搜尋方塊中輸入 **Microsoft.EntityFrameworkCore.SqlServer**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-216">Select the **Browse** tab, and then enter **Microsoft.EntityFrameworkCore.SqlServer** in the search box.</span></span>
* <span data-ttu-id="77fd0-217">在左窗格中選取 [ **microsoft.entityframeworkcore** ]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-217">Select **Microsoft.EntityFrameworkCore.SqlServer** in the left pane.</span></span>
* <span data-ttu-id="77fd0-218">選取右窗格中的 [專案]\*\*\*\* 核取方塊，然後選取 [安裝]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-218">Select the **Project** check box in the right pane and then select **Install**.</span></span>
* <span data-ttu-id="77fd0-219">使用上述指示來新增 `Microsoft.EntityFrameworkCore.InMemory` NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="77fd0-219">Use the preceding instructions to add the `Microsoft.EntityFrameworkCore.InMemory` NuGet package.</span></span>

![NuGet 套件管理員](first-web-api/_static/vs3NuGet.png)

## <a name="add-the-todocontext-database-context"></a><span data-ttu-id="77fd0-221">新增 TodoCoNtext 資料庫內容</span><span class="sxs-lookup"><span data-stu-id="77fd0-221">Add the TodoContext database context</span></span>

* <span data-ttu-id="77fd0-222">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-222">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="77fd0-223">將類別命名為 *TodoContext*，然後按一下 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-223">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-224">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-224">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="77fd0-225">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-225">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="77fd0-226">輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-226">Enter the following code:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="77fd0-227">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="77fd0-227">Register the database context</span></span>

<span data-ttu-id="77fd0-228">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="77fd0-228">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="77fd0-229">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-229">The container provides the service to controllers.</span></span>

<span data-ttu-id="77fd0-230">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="77fd0-230">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Startup.cs?highlight=7-8,23-24&name=snippet_all)]

<span data-ttu-id="77fd0-231">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-231">The preceding code:</span></span>

* <span data-ttu-id="77fd0-232">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="77fd0-232">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="77fd0-233">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-233">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="77fd0-234">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-234">Specifies that the database context will use an in-memory database.</span></span>

## <a name="scaffold-a-controller"></a><span data-ttu-id="77fd0-235">Scaffold 控制器</span><span class="sxs-lookup"><span data-stu-id="77fd0-235">Scaffold a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-236">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-236">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-237">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-237">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="77fd0-238">選取 [新增]\*\* [新增 Scaffold 項目]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-238">Select **Add** > **New Scaffolded Item**.</span></span>
* <span data-ttu-id="77fd0-239">選取 [使用 Entity Framework 執行動作的 API 控制器]\*\*\*\*，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-239">Select **API Controller with actions, using Entity Framework**, and then select **Add**.</span></span>
* <span data-ttu-id="77fd0-240">在 [使用 Entity Framework 執行動作的 API 控制器]\*\*\*\* 對話方塊中：</span><span class="sxs-lookup"><span data-stu-id="77fd0-240">In the **Add API Controller with actions, using Entity Framework** dialog:</span></span>

  * <span data-ttu-id="77fd0-241">在**模型類別**中選取 [ **TodoItem （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-241">Select **TodoItem (TodoApi.Models)** in the **Model class**.</span></span>
  * <span data-ttu-id="77fd0-242">選取**資料內容類別**中的 [ **TodoCoNtext （TodoApi）** ]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-242">Select **TodoContext (TodoApi.Models)** in the **Data context class**.</span></span>
  * <span data-ttu-id="77fd0-243">選取 [新增]  。</span><span class="sxs-lookup"><span data-stu-id="77fd0-243">Select **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-244">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-244">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="77fd0-245">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-245">Run the following commands:</span></span>

```dotnetcli
dotnet add package Microsoft.VisualStudio.Web.CodeGeneration.Design
dotnet add package Microsoft.EntityFrameworkCore.Design
dotnet tool install --global dotnet-aspnet-codegenerator
dotnet aspnet-codegenerator controller -name TodoItemsController -async -api -m TodoItem -dc TodoContext -outDir Controllers
```

<span data-ttu-id="77fd0-246">上述命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-246">The preceding commands:</span></span>

* <span data-ttu-id="77fd0-247">新增 Scaffolding 所需的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="77fd0-247">Add NuGet packages required for scaffolding.</span></span>
* <span data-ttu-id="77fd0-248">安裝 Scaffolding 引擎 (`dotnet-aspnet-codegenerator`)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-248">Installs the scaffolding engine (`dotnet-aspnet-codegenerator`).</span></span>
* <span data-ttu-id="77fd0-249">Scaffold `TodoItemsController`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-249">Scaffolds the `TodoItemsController`.</span></span>

---

<span data-ttu-id="77fd0-250">產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-250">The generated code:</span></span>

* <span data-ttu-id="77fd0-251">使用[`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute)屬性來標示類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-251">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="77fd0-252">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-252">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="77fd0-253">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="77fd0-253">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="77fd0-254">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="77fd0-254">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="77fd0-255">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="77fd0-255">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>

<span data-ttu-id="77fd0-256">的 ASP.NET Core 範本：</span><span class="sxs-lookup"><span data-stu-id="77fd0-256">The ASP.NET Core templates for:</span></span>

* <span data-ttu-id="77fd0-257">具有 views 的控制器`[action]`會包含在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="77fd0-257">Controllers with views include `[action]` in the route template.</span></span>
* <span data-ttu-id="77fd0-258">API 控制器不會`[action]`包含在路由範本中。</span><span class="sxs-lookup"><span data-stu-id="77fd0-258">API controllers don't include `[action]` in the route template.</span></span>

<span data-ttu-id="77fd0-259">當`[action]`權杖不在路由範本中時，[動作](xref:mvc/controllers/routing#action)名稱會從路由中排除。</span><span class="sxs-lookup"><span data-stu-id="77fd0-259">When the `[action]` token isn't in the route template, the [action](xref:mvc/controllers/routing#action) name is excluded from the route.</span></span> <span data-ttu-id="77fd0-260">也就是說，不會在相符的路由中使用動作的相關聯方法名稱。</span><span class="sxs-lookup"><span data-stu-id="77fd0-260">That is, the action's associated method name isn't used in the matching route.</span></span>

## <a name="examine-the-posttodoitem-create-method"></a><span data-ttu-id="77fd0-261">檢查 PostTodoItem 建立方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-261">Examine the PostTodoItem create method</span></span>

<span data-ttu-id="77fd0-262">取代 `PostTodoItem` 中的 return 陳述式，以使用 [nameof](/dotnet/csharp/language-reference/operators/nameof) 運算子：</span><span class="sxs-lookup"><span data-stu-id="77fd0-262">Replace the return statement in the `PostTodoItem` to use the [nameof](/dotnet/csharp/language-reference/operators/nameof) operator:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Create)]

<span data-ttu-id="77fd0-263">上述程式碼是 HTTP POST 方法，如[`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute)屬性所示。</span><span class="sxs-lookup"><span data-stu-id="77fd0-263">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="77fd0-264">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="77fd0-264">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="77fd0-265"><xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-265">The <xref:Microsoft.AspNetCore.Mvc.ControllerBase.CreatedAtAction*> method:</span></span>

* <span data-ttu-id="77fd0-266">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-266">Returns an HTTP 201 status code if successful.</span></span> <span data-ttu-id="77fd0-267">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="77fd0-267">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="77fd0-268">將[Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location)標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="77fd0-268">Adds a [Location](https://developer.mozilla.org/docs/Web/HTTP/Headers/Location) header to the response.</span></span> <span data-ttu-id="77fd0-269">`Location`標頭會指定新建立之待辦事項專案的[URI](https://developer.mozilla.org/docs/Glossary/URI) 。</span><span class="sxs-lookup"><span data-stu-id="77fd0-269">The `Location` header specifies the [URI](https://developer.mozilla.org/docs/Glossary/URI) of the newly created to-do item.</span></span> <span data-ttu-id="77fd0-270">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-270">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="77fd0-271">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="77fd0-271">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="77fd0-272">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="77fd0-272">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

### <a name="install-postman"></a><span data-ttu-id="77fd0-273">安裝 Postman</span><span class="sxs-lookup"><span data-stu-id="77fd0-273">Install Postman</span></span>

<span data-ttu-id="77fd0-274">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-274">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="77fd0-275">安裝[Postman](https://www.getpostman.com/downloads/)</span><span class="sxs-lookup"><span data-stu-id="77fd0-275">Install [Postman](https://www.getpostman.com/downloads/)</span></span>
* <span data-ttu-id="77fd0-276">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-276">Start the web app.</span></span>
* <span data-ttu-id="77fd0-277">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="77fd0-277">Start Postman.</span></span>
* <span data-ttu-id="77fd0-278">停用 [SSL certificate verification] \(SSL 憑證驗證\)\*\*\*\*</span><span class="sxs-lookup"><span data-stu-id="77fd0-278">Disable **SSL certificate verification**</span></span>
  * <span data-ttu-id="77fd0-279">從 [檔案]\*\* [設定]\*\* > \*\*\*\* ([一般]\*\*\*\* 索引標籤)，停用 [SSL 憑證驗證]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-279">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>
    > [!WARNING]
    > <span data-ttu-id="77fd0-280">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-280">Re-enable SSL certificate verification after testing the controller.</span></span>

<a name="post"></a>

### <a name="test-posttodoitem-with-postman"></a><span data-ttu-id="77fd0-281">使用 Postman 測試 PostTodoItem</span><span class="sxs-lookup"><span data-stu-id="77fd0-281">Test PostTodoItem with Postman</span></span>

* <span data-ttu-id="77fd0-282">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-282">Create a new request.</span></span>
* <span data-ttu-id="77fd0-283">將 HTTP 方法設為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-283">Set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="77fd0-284">選取 [Body]\*\*\*\* \(本文\) 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-284">Select the **Body** tab.</span></span>
* <span data-ttu-id="77fd0-285">選取 [原始]\*\*\*\* 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="77fd0-285">Select the **raw** radio button.</span></span>
* <span data-ttu-id="77fd0-286">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-286">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="77fd0-287">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="77fd0-287">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="77fd0-288">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-288">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/3/create.png)

### <a name="test-the-location-header-uri"></a><span data-ttu-id="77fd0-290">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="77fd0-290">Test the location header URI</span></span>

* <span data-ttu-id="77fd0-291">在 [回應]\*\*\*\* 窗格中選取 [標頭]\*\*\*\* 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-291">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="77fd0-292">複製 [位置]\*\*\*\* 標頭值：</span><span class="sxs-lookup"><span data-stu-id="77fd0-292">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/3/create.png)

* <span data-ttu-id="77fd0-294">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="77fd0-294">Set the method to GET.</span></span>
* <span data-ttu-id="77fd0-295">貼上`https://localhost:5001/api/TodoItems/1`URI （例如）。</span><span class="sxs-lookup"><span data-stu-id="77fd0-295">Paste the URI (for example, `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="77fd0-296">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-296">Select **Send**.</span></span>

## <a name="examine-the-get-methods"></a><span data-ttu-id="77fd0-297">檢查 GET 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-297">Examine the GET methods</span></span>

<span data-ttu-id="77fd0-298">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="77fd0-298">These methods implement two GET endpoints:</span></span>

* `GET /api/TodoItems`
* `GET /api/TodoItems/{id}`

<span data-ttu-id="77fd0-299">從瀏覽器或 Postman 呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-299">Test the app by calling the two endpoints from a browser or Postman.</span></span> <span data-ttu-id="77fd0-300">例如：</span><span class="sxs-lookup"><span data-stu-id="77fd0-300">For example:</span></span>

* `https://localhost:5001/api/TodoItems`
* `https://localhost:5001/api/TodoItems/1`

<span data-ttu-id="77fd0-301">`GetTodoItems` 的呼叫會產生類似下列的回應：</span><span class="sxs-lookup"><span data-stu-id="77fd0-301">A response similar to the following is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

### <a name="test-get-with-postman"></a><span data-ttu-id="77fd0-302">使用 Postman 測試 Get</span><span class="sxs-lookup"><span data-stu-id="77fd0-302">Test Get with Postman</span></span>

* <span data-ttu-id="77fd0-303">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-303">Create a new request.</span></span>
* <span data-ttu-id="77fd0-304">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-304">Set the HTTP method to **GET**.</span></span>
* <span data-ttu-id="77fd0-305">將要求 URL 設定為 `https://localhost:<port>/api/TodoItems`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-305">Set the request URL to `https://localhost:<port>/api/TodoItems`.</span></span> <span data-ttu-id="77fd0-306">例如： `https://localhost:5001/api/TodoItems` 。</span><span class="sxs-lookup"><span data-stu-id="77fd0-306">For example, `https://localhost:5001/api/TodoItems`.</span></span>
* <span data-ttu-id="77fd0-307">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-307">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="77fd0-308">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-308">Select **Send**.</span></span>

<span data-ttu-id="77fd0-309">這個應用程式會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-309">This app uses an in-memory database.</span></span> <span data-ttu-id="77fd0-310">如果應用程式在停止後再啟動，上述 GET 要求將不會傳回任何資料。</span><span class="sxs-lookup"><span data-stu-id="77fd0-310">If the app is stopped and started, the preceding GET request will not return any data.</span></span> <span data-ttu-id="77fd0-311">如果沒有傳回任何資料，請將資料 [POST](#post) 到應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-311">If no data is returned, [POST](#post) data to the app.</span></span>

## <a name="routing-and-url-paths"></a><span data-ttu-id="77fd0-312">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="77fd0-312">Routing and URL paths</span></span>

<span data-ttu-id="77fd0-313">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="77fd0-313">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="77fd0-314">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="77fd0-314">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="77fd0-315">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="77fd0-315">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=TodoController&highlight=1)]

* <span data-ttu-id="77fd0-316">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-316">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="77fd0-317">在此範例中，控制器類別名稱是 **TodoItems**Controller，因此控制器名稱是 "TodoItems"。</span><span class="sxs-lookup"><span data-stu-id="77fd0-317">For this sample, the controller class name is **TodoItems**Controller, so the controller name is "TodoItems".</span></span> <span data-ttu-id="77fd0-318">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-318">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="77fd0-319">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="77fd0-319">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="77fd0-320">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="77fd0-320">This sample doesn't use a template.</span></span> <span data-ttu-id="77fd0-321">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-321">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="77fd0-322">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="77fd0-322">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="77fd0-323">叫`GetTodoItem` `"{id}"`用時，URL 中的值會在其`id`參數中提供給方法。</span><span class="sxs-lookup"><span data-stu-id="77fd0-323">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its `id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="77fd0-324">傳回值</span><span class="sxs-lookup"><span data-stu-id="77fd0-324">Return values</span></span>

<span data-ttu-id="77fd0-325">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-325">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="77fd0-326">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="77fd0-326">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="77fd0-327">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="77fd0-327">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="77fd0-328">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-328">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="77fd0-329">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-329">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="77fd0-330">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="77fd0-330">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="77fd0-331">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-331">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="77fd0-332">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="77fd0-332">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="77fd0-333">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="77fd0-333">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="the-puttodoitem-method"></a><span data-ttu-id="77fd0-334">PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-334">The PutTodoItem method</span></span>

<span data-ttu-id="77fd0-335">檢查 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-335">Examine the `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Update)]

<span data-ttu-id="77fd0-336">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="77fd0-336">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="77fd0-337">回應為[204 （沒有內容）](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-337">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="77fd0-338">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="77fd0-338">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="77fd0-339">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-339">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="77fd0-340">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-340">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="77fd0-341">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-341">Test the PutTodoItem method</span></span>

<span data-ttu-id="77fd0-342">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-342">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="77fd0-343">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-343">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="77fd0-344">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-344">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="77fd0-345">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="77fd0-345">Update the to-do item that has ID = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="77fd0-346">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="77fd0-346">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/3/pmcput.png)

## <a name="the-deletetodoitem-method"></a><span data-ttu-id="77fd0-348">DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-348">The DeleteTodoItem method</span></span>

<span data-ttu-id="77fd0-349">檢查 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-349">Examine the `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApi/Controllers/TodoItemsController.cs?name=snippet_Delete)]

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="77fd0-350">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-350">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="77fd0-351">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="77fd0-351">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="77fd0-352">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-352">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="77fd0-353">設定要刪除之物件的 URI （例如`https://localhost:5001/api/TodoItems/1`）。</span><span class="sxs-lookup"><span data-stu-id="77fd0-353">Set the URI of the object to delete (for example `https://localhost:5001/api/TodoItems/1`).</span></span>
* <span data-ttu-id="77fd0-354">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-354">Select **Send**.</span></span>

<a name="over-post"></a>

## <a name="prevent-over-posting"></a><span data-ttu-id="77fd0-355">防止過度張貼</span><span class="sxs-lookup"><span data-stu-id="77fd0-355">Prevent over-posting</span></span>

<span data-ttu-id="77fd0-356">範例應用程式目前會公開整個`TodoItem`物件。</span><span class="sxs-lookup"><span data-stu-id="77fd0-356">Currently the sample app exposes the entire `TodoItem` object.</span></span> <span data-ttu-id="77fd0-357">生產應用程式通常會使用模型的子集來限制輸入和傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="77fd0-357">Productions apps typically limit the data that's input and returned using a subset of the model.</span></span> <span data-ttu-id="77fd0-358">這種情況的原因有很多，而且安全性是主要的。</span><span class="sxs-lookup"><span data-stu-id="77fd0-358">There are multiple reasons behind this and security is a major one.</span></span> <span data-ttu-id="77fd0-359">模型的子集通常稱為「資料傳輸物件（DTO）」、「輸入模型」或「視圖模型」。</span><span class="sxs-lookup"><span data-stu-id="77fd0-359">The subset of a model is usually referred to as a Data Transfer Object (DTO), input model, or view model.</span></span> <span data-ttu-id="77fd0-360">這篇文章中使用了**DTO** 。</span><span class="sxs-lookup"><span data-stu-id="77fd0-360">**DTO** is used in this article.</span></span>

<span data-ttu-id="77fd0-361">DTO 可用來：</span><span class="sxs-lookup"><span data-stu-id="77fd0-361">A DTO may be used to:</span></span>

* <span data-ttu-id="77fd0-362">避免過度張貼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-362">Prevent over-posting.</span></span>
* <span data-ttu-id="77fd0-363">隱藏用戶端不應查看的屬性。</span><span class="sxs-lookup"><span data-stu-id="77fd0-363">Hide properties that clients are not supposed to view.</span></span>
* <span data-ttu-id="77fd0-364">省略某些屬性，以減少承載大小。</span><span class="sxs-lookup"><span data-stu-id="77fd0-364">Omit some properties in order to reduce payload size.</span></span>
* <span data-ttu-id="77fd0-365">壓平合併包含嵌套物件的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="77fd0-365">Flatten object graphs that contain nested objects.</span></span> <span data-ttu-id="77fd0-366">針對用戶端，簡維物件圖形可能更方便。</span><span class="sxs-lookup"><span data-stu-id="77fd0-366">Flattened object graphs can be more convenient for clients.</span></span>

<span data-ttu-id="77fd0-367">若要示範 DTO 方法，請更新`TodoItem`類別以包含秘密欄位：</span><span class="sxs-lookup"><span data-stu-id="77fd0-367">To demonstrate the DTO approach, update the `TodoItem` class to include a secret field:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItem.cs?name=snippet&highlight=6)]

<span data-ttu-id="77fd0-368">這個應用程式必須隱藏秘密欄位，但是系統管理應用程式可以選擇公開它。</span><span class="sxs-lookup"><span data-stu-id="77fd0-368">The secret field needs to be hidden from this app, but an administrative app could choose to expose it.</span></span>

<span data-ttu-id="77fd0-369">確認您可以張貼並取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="77fd0-369">Verify you can post and get the secret field.</span></span>

<span data-ttu-id="77fd0-370">建立 DTO 模型：</span><span class="sxs-lookup"><span data-stu-id="77fd0-370">Create a DTO model:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Models/TodoItemDTO.cs?name=snippet)]

<span data-ttu-id="77fd0-371">更新`TodoItemsController`以使用`TodoItemDTO`：</span><span class="sxs-lookup"><span data-stu-id="77fd0-371">Update the `TodoItemsController` to use `TodoItemDTO`:</span></span>

[!code-csharp[](first-web-api/samples/3.0/TodoApiDTO/Controllers/TodoItemsController.cs?name=snippet)]

<span data-ttu-id="77fd0-372">確認您無法張貼或取得秘密欄位。</span><span class="sxs-lookup"><span data-stu-id="77fd0-372">Verify you can't post or get the secret field.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="77fd0-373">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="77fd0-373">Call the web API with JavaScript</span></span>

<span data-ttu-id="77fd0-374">請參閱[教學課程：使用 JavaScript 呼叫 ASP.NET Core Web API](xref:tutorials/web-api-javascript)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-374">See [Tutorial: Call an ASP.NET Core web API with JavaScript](xref:tutorials/web-api-javascript).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="77fd0-375">在本教學課程中，您會了解如何：</span><span class="sxs-lookup"><span data-stu-id="77fd0-375">In this tutorial, you learn how to:</span></span>

> [!div class="checklist"]
> * <span data-ttu-id="77fd0-376">建立 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-376">Create a web API project.</span></span>
> * <span data-ttu-id="77fd0-377">新增模型類別和資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="77fd0-377">Add a model class and a database context.</span></span>
> * <span data-ttu-id="77fd0-378">新增控制器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-378">Add a controller.</span></span>
> * <span data-ttu-id="77fd0-379">新增 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="77fd0-379">Add CRUD methods.</span></span>
> * <span data-ttu-id="77fd0-380">設定路由和 URL 路徑。</span><span class="sxs-lookup"><span data-stu-id="77fd0-380">Configure routing and URL paths.</span></span>
> * <span data-ttu-id="77fd0-381">指定傳回值。</span><span class="sxs-lookup"><span data-stu-id="77fd0-381">Specify return values.</span></span>
> * <span data-ttu-id="77fd0-382">使用 Postman 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-382">Call the web API with Postman.</span></span>
> * <span data-ttu-id="77fd0-383">使用 JavaScript 呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-383">Call the web API with JavaScript.</span></span>

<span data-ttu-id="77fd0-384">結束時，您將會有一個可管理關聯式資料庫中所儲存「待辦事項」的 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-384">At the end, you have a web API that can manage "to-do" items stored in a relational database.</span></span>

## <a name="overview"></a><span data-ttu-id="77fd0-385">概觀</span><span class="sxs-lookup"><span data-stu-id="77fd0-385">Overview</span></span>

<span data-ttu-id="77fd0-386">本教學課程會建立以下 API：</span><span class="sxs-lookup"><span data-stu-id="77fd0-386">This tutorial creates the following API:</span></span>

|<span data-ttu-id="77fd0-387">API</span><span class="sxs-lookup"><span data-stu-id="77fd0-387">API</span></span> | <span data-ttu-id="77fd0-388">描述</span><span class="sxs-lookup"><span data-stu-id="77fd0-388">Description</span></span> | <span data-ttu-id="77fd0-389">Request body</span><span class="sxs-lookup"><span data-stu-id="77fd0-389">Request body</span></span> | <span data-ttu-id="77fd0-390">Response body</span><span class="sxs-lookup"><span data-stu-id="77fd0-390">Response body</span></span> |
|--- | ---- | ---- | ---- |
|<span data-ttu-id="77fd0-391">GET /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="77fd0-391">GET /api/TodoItems</span></span> | <span data-ttu-id="77fd0-392">取得所有待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-392">Get all to-do items</span></span> | <span data-ttu-id="77fd0-393">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-393">None</span></span> | <span data-ttu-id="77fd0-394">待辦事項的陣列</span><span class="sxs-lookup"><span data-stu-id="77fd0-394">Array of to-do items</span></span>|
|<span data-ttu-id="77fd0-395">GET /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="77fd0-395">GET /api/TodoItems/{id}</span></span> | <span data-ttu-id="77fd0-396">依識別碼取得項目</span><span class="sxs-lookup"><span data-stu-id="77fd0-396">Get an item by ID</span></span> | <span data-ttu-id="77fd0-397">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-397">None</span></span> | <span data-ttu-id="77fd0-398">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-398">To-do item</span></span>|
|<span data-ttu-id="77fd0-399">POST /api/TodoItems</span><span class="sxs-lookup"><span data-stu-id="77fd0-399">POST /api/TodoItems</span></span> | <span data-ttu-id="77fd0-400">新增記錄</span><span class="sxs-lookup"><span data-stu-id="77fd0-400">Add a new item</span></span> | <span data-ttu-id="77fd0-401">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-401">To-do item</span></span> | <span data-ttu-id="77fd0-402">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-402">To-do item</span></span> |
|<span data-ttu-id="77fd0-403">PUT /api/TodoItems/{識別碼}</span><span class="sxs-lookup"><span data-stu-id="77fd0-403">PUT /api/TodoItems/{id}</span></span> | <span data-ttu-id="77fd0-404">更新現有的項目 &nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-404">Update an existing item &nbsp;</span></span> | <span data-ttu-id="77fd0-405">待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-405">To-do item</span></span> | <span data-ttu-id="77fd0-406">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-406">None</span></span> |
|<span data-ttu-id="77fd0-407">刪除/api/TodoItems/{id} &nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-407">DELETE /api/TodoItems/{id} &nbsp; &nbsp;</span></span> | <span data-ttu-id="77fd0-408">刪除專案&nbsp;&nbsp;</span><span class="sxs-lookup"><span data-stu-id="77fd0-408">Delete an item &nbsp; &nbsp;</span></span> | <span data-ttu-id="77fd0-409">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-409">None</span></span> | <span data-ttu-id="77fd0-410">None</span><span class="sxs-lookup"><span data-stu-id="77fd0-410">None</span></span>|

<span data-ttu-id="77fd0-411">下圖顯示應用程式的設計。</span><span class="sxs-lookup"><span data-stu-id="77fd0-411">The following diagram shows the design of the app.</span></span>

![左側方塊代表用戶端。](first-web-api/_static/architecture.png)

## <a name="prerequisites"></a><span data-ttu-id="77fd0-417">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="77fd0-417">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-418">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-418">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-419">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-419">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-420">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-420">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

## <a name="create-a-web-project"></a><span data-ttu-id="77fd0-421">建立 Web 專案</span><span class="sxs-lookup"><span data-stu-id="77fd0-421">Create a web project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-422">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-422">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-423">從 [檔案]\*\*\*\* 功能表選取 [新增]**[專案]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-423">From the **File** menu, select **New** > **Project**.</span></span>
* <span data-ttu-id="77fd0-424">選取 **ASP.NET Core Web 應用程式**範本，然後按一下 [下一步]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-424">Select the **ASP.NET Core Web Application** template and click **Next**.</span></span>
* <span data-ttu-id="77fd0-425">將專案命名為 *TodoApi*，然後按一下 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-425">Name the project *TodoApi* and click **Create**.</span></span>
* <span data-ttu-id="77fd0-426">在 [建立新的 ASP.NET Core Web 應用程式]\*\*\*\* 對話方塊中，確認選取 [.NET Core]\*\*\*\* 和 [ASP.NET Core 2.2]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-426">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 2.2** are selected.</span></span> <span data-ttu-id="77fd0-427">選取 **API** 範本，然後按一下 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-427">Select the **API** template and click **Create**.</span></span> <span data-ttu-id="77fd0-428">請**勿**選取 [Enable Docker Support] \(啟用 Docker 支援\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-428">**Don't** select **Enable Docker Support**.</span></span>

![VS 新增專案對話方塊](first-web-api/_static/vs.png)

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-430">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-430">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77fd0-431">開啟[整合式終端機](https://code.visualstudio.com/docs/editor/integrated-terminal)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-431">Open the [integrated terminal](https://code.visualstudio.com/docs/editor/integrated-terminal).</span></span>
* <span data-ttu-id="77fd0-432">將目錄 (`cd`) 變更為包含專案資料夾的資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-432">Change directories (`cd`) to the folder that will contain the project folder.</span></span>
* <span data-ttu-id="77fd0-433">執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="77fd0-433">Run the following commands:</span></span>

   ```dotnetcli
   dotnet new webapi -o TodoApi
   code -r TodoApi
   ```

  <span data-ttu-id="77fd0-434">這些命令會建立新的 Web API 專案，並開啟新專案資料夾中的新 Visual Studio Code 執行個體。</span><span class="sxs-lookup"><span data-stu-id="77fd0-434">These commands create a new web API project and open a new instance of Visual Studio Code in the new project folder.</span></span>

* <span data-ttu-id="77fd0-435">當出現對話方塊詢問您是否要將所需的資產新增至專案時，選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-435">When a dialog box asks if you want to add required assets to the project, select **Yes**.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-436">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-436">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77fd0-437">選取 [檔案]\*\* [新增解決方案]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-437">Select **File** > **New Solution**.</span></span>

  ![macOS 新增方案](first-web-api-mac/_static/sln.png)

* <span data-ttu-id="77fd0-439">選取[.Net Core]\*\* [應用程式]\*\* > \*\* [API]\*\* > \*\* [下一步]\*\* > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-439">Select **.NET Core** > **App** > **API** > **Next**.</span></span>

  ![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)
  
* <span data-ttu-id="77fd0-441">在 [設定您的新 ASP.NET Core Web API]\*\*\*\* 對話方塊中，接受 [目標 Framework]\*\*\*\* 的預設 \**.NET Core 2.2*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-441">In the **Configure your new ASP.NET Core Web API** dialog, accept the default **Target Framework** of \**.NET Core 2.2*.</span></span>

* <span data-ttu-id="77fd0-442">針對 [專案名稱]\*\*\*\* 輸入 *TodoApi*，然後選取 [建立]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-442">Enter *TodoApi* for the **Project Name** and then select **Create**.</span></span>

  ![設定對話方塊](first-web-api-mac/_static/2.png)

---

### <a name="test-the-api"></a><span data-ttu-id="77fd0-444">測試 API</span><span class="sxs-lookup"><span data-stu-id="77fd0-444">Test the API</span></span>

<span data-ttu-id="77fd0-445">專案範本會建立 `values` API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-445">The project template creates a `values` API.</span></span> <span data-ttu-id="77fd0-446">從瀏覽器呼叫 `Get` 方法來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-446">Call the `Get` method from a browser to test the app.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-447">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-447">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="77fd0-448">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-448">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="77fd0-449">Visual Studio 會啟動瀏覽器並巡覽至 `https://localhost:<port>/api/values`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="77fd0-449">Visual Studio launches a browser and navigates to `https://localhost:<port>/api/values`, where `<port>` is a randomly chosen port number.</span></span>

<span data-ttu-id="77fd0-450">如果出現對話方塊詢問您是否應該信任 IIS Express 憑證，請選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-450">If you get a dialog box that asks if you should trust the IIS Express certificate, select **Yes**.</span></span> <span data-ttu-id="77fd0-451">在接著出現的 [安全性警告]\*\*\*\* 對話方塊中，選取 [是]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-451">In the **Security Warning** dialog that appears next, select **Yes**.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-452">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-452">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="77fd0-453">按 Ctrl+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-453">Press Ctrl+F5 to run the app.</span></span> <span data-ttu-id="77fd0-454">在瀏覽器中，前往下列 URL：`https://localhost:5001/api/values`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-454">In a browser, go to following URL: `https://localhost:5001/api/values`.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-455">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-455">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="77fd0-456">選取 [**執行** > ] [**開始調試**程式] 以啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-456">Select **Run** > **Start Debugging** to launch the app.</span></span> <span data-ttu-id="77fd0-457">Visual Studio for Mac 會啟動瀏覽器並巡覽至 `https://localhost:<port>`，其中 `<port>` 是隨機選擇的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-457">Visual Studio for Mac launches a browser and navigates to `https://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="77fd0-458">傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-458">An HTTP 404 (Not Found) error is returned.</span></span> <span data-ttu-id="77fd0-459">將 `/api/values` 附加至 URL (將 URL 變更為 `https://localhost:<port>/api/values`)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-459">Append `/api/values` to the URL (change the URL to `https://localhost:<port>/api/values`).</span></span>

---

<span data-ttu-id="77fd0-460">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="77fd0-460">The following JSON is returned:</span></span>

```json
["value1","value2"]
```

## <a name="add-a-model-class"></a><span data-ttu-id="77fd0-461">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="77fd0-461">Add a model class</span></span>

<span data-ttu-id="77fd0-462">「模型」\*\* 是代表應用程式所管理資料的一組類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-462">A *model* is a set of classes that represent the data that the app manages.</span></span> <span data-ttu-id="77fd0-463">此應用程式的模型是單一 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-463">The model for this app is a single `TodoItem` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-464">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-464">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-465">在 [方案總管]\*\*\*\* 中，於專案上按一下滑鼠右鍵。</span><span class="sxs-lookup"><span data-stu-id="77fd0-465">In **Solution Explorer**, right-click the project.</span></span> <span data-ttu-id="77fd0-466"> > 選取 **\[\*\*\**新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-466">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="77fd0-467">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-467">Name the folder *Models*.</span></span>

* <span data-ttu-id="77fd0-468">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-468">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="77fd0-469">將類別命名為 *TodoItem*，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-469">Name the class *TodoItem* and select **Add**.</span></span>

* <span data-ttu-id="77fd0-470">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-470">Replace the template code with the following code:</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="77fd0-471">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="77fd0-471">Visual Studio Code</span></span>](#tab/visual-studio-code)

* <span data-ttu-id="77fd0-472">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-472">Add a folder named *Models*.</span></span>

* <span data-ttu-id="77fd0-473">將 `TodoItem` 類別新增至具有下列程式碼的 *Models* 資料夾：</span><span class="sxs-lookup"><span data-stu-id="77fd0-473">Add a `TodoItem` class to the *Models* folder with the following code:</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-474">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-474">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

* <span data-ttu-id="77fd0-475">以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-475">Right-click the project.</span></span> <span data-ttu-id="77fd0-476"> > 選取 **\[\*\*\**新增資料夾**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-476">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="77fd0-477">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-477">Name the folder *Models*.</span></span>

  ![新增資料夾](first-web-api-mac/_static/folder.png)

* <span data-ttu-id="77fd0-479">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]**[新增檔案]** > **[一般]** > **[空類別]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-479">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span>

* <span data-ttu-id="77fd0-480">將類別命名為 *TodoItem*，然後按一下 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-480">Name the class *TodoItem*, and then click **New**.</span></span>

* <span data-ttu-id="77fd0-481">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-481">Replace the template code with the following code:</span></span>

---

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="77fd0-482">`Id` 屬性的功能相當於關聯式資料庫中的唯一索引鍵。</span><span class="sxs-lookup"><span data-stu-id="77fd0-482">The `Id` property functions as the unique key in a relational database.</span></span>

<span data-ttu-id="77fd0-483">模型類別可位於專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-483">Model classes can go anywhere in the project, but the *Models* folder is used by convention.</span></span>

## <a name="add-a-database-context"></a><span data-ttu-id="77fd0-484">新增資料庫內容</span><span class="sxs-lookup"><span data-stu-id="77fd0-484">Add a database context</span></span>

<span data-ttu-id="77fd0-485">「資料庫內容」\*\* 是為資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-485">The *database context* is the main class that coordinates Entity Framework functionality for a data model.</span></span> <span data-ttu-id="77fd0-486">此類別是透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立。</span><span class="sxs-lookup"><span data-stu-id="77fd0-486">This class is created by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-487">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-487">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-488">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增]\*\*\*\* > [類別]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-488">Right-click the *Models* folder and select **Add** > **Class**.</span></span> <span data-ttu-id="77fd0-489">將類別命名為 *TodoContext*，然後按一下 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-489">Name the class *TodoContext* and click **Add**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-490">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-490">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="77fd0-491">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-491">Add a `TodoContext` class to the *Models* folder.</span></span>

---

* <span data-ttu-id="77fd0-492">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-492">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Models/TodoContext.cs)]

## <a name="register-the-database-context"></a><span data-ttu-id="77fd0-493">登錄資料庫內容</span><span class="sxs-lookup"><span data-stu-id="77fd0-493">Register the database context</span></span>

<span data-ttu-id="77fd0-494">在 ASP.NET Core 中，資料庫內容等服務必須向[相依性插入 (DI)](xref:fundamentals/dependency-injection) 容器註冊。</span><span class="sxs-lookup"><span data-stu-id="77fd0-494">In ASP.NET Core, services such as the DB context must be registered with the [dependency injection (DI)](xref:fundamentals/dependency-injection) container.</span></span> <span data-ttu-id="77fd0-495">此容器會將服務提供給控制器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-495">The container provides the service to controllers.</span></span>

<span data-ttu-id="77fd0-496">使用下列醒目提示的程式碼更新 *Startup.cs*：</span><span class="sxs-lookup"><span data-stu-id="77fd0-496">Update *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup1.cs?highlight=5,8,25-26&name=snippet_all)]

<span data-ttu-id="77fd0-497">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-497">The preceding code:</span></span>

* <span data-ttu-id="77fd0-498">移除未使用的 `using` 宣告。</span><span class="sxs-lookup"><span data-stu-id="77fd0-498">Removes unused `using` declarations.</span></span>
* <span data-ttu-id="77fd0-499">將資料庫內容新增至 DI 容器。</span><span class="sxs-lookup"><span data-stu-id="77fd0-499">Adds the database context to the DI container.</span></span>
* <span data-ttu-id="77fd0-500">指定資料庫內容將會使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-500">Specifies that the database context will use an in-memory database.</span></span>

## <a name="add-a-controller"></a><span data-ttu-id="77fd0-501">新增控制器</span><span class="sxs-lookup"><span data-stu-id="77fd0-501">Add a controller</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-502">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-502">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-503">以滑鼠右鍵按一下 *Controllers* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-503">Right-click the *Controllers* folder.</span></span>
* <span data-ttu-id="77fd0-504">選取 [新增]**[新項目]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-504">Select **Add** > **New Item**.</span></span>
* <span data-ttu-id="77fd0-505">在 [新增項目]\*\*\*\* 對話方塊中，選取 [API 控制器類別]\*\*\*\* 範本。</span><span class="sxs-lookup"><span data-stu-id="77fd0-505">In the **Add New Item** dialog, select the **API Controller Class** template.</span></span>
* <span data-ttu-id="77fd0-506">將類別命名為 *TodoController*，然後選取 [新增]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-506">Name the class *TodoController*, and select **Add**.</span></span>

  ![在搜尋方塊中輸入 controller 且已選取 Web API 控制器的 [新增項目] 對話方塊](first-web-api/_static/new_controller.png)

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-508">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-508">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="77fd0-509">在 *Controllers* 資料夾中，建立名為 `TodoController` 的類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-509">In the *Controllers* folder, create a class named `TodoController`.</span></span>

---

* <span data-ttu-id="77fd0-510">使用下列程式碼取代範本程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-510">Replace the template code with the following code:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController2.cs?name=snippet_todo1)]

<span data-ttu-id="77fd0-511">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="77fd0-511">The preceding code:</span></span>

* <span data-ttu-id="77fd0-512">定義不含方法的 API 控制器類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-512">Defines an API controller class without methods.</span></span>
* <span data-ttu-id="77fd0-513">使用[`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute)屬性來標示類別。</span><span class="sxs-lookup"><span data-stu-id="77fd0-513">Marks the class with the [`[ApiController]`](/dotnet/api/microsoft.aspnetcore.mvc.apicontrollerattribute) attribute.</span></span> <span data-ttu-id="77fd0-514">這個屬性表示控制器會回應 Web API 要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-514">This attribute indicates that the controller responds to web API requests.</span></span> <span data-ttu-id="77fd0-515">如需屬性所啟用之特定行為的相關資訊，請參閱 <xref:web-api/index>。</span><span class="sxs-lookup"><span data-stu-id="77fd0-515">For information about specific behaviors that the attribute enables, see <xref:web-api/index>.</span></span>
* <span data-ttu-id="77fd0-516">使用 DI 將資料庫內容 (`TodoContext`) 插入到控制器中。</span><span class="sxs-lookup"><span data-stu-id="77fd0-516">Uses DI to inject the database context (`TodoContext`) into the controller.</span></span> <span data-ttu-id="77fd0-517">控制器中的每一個 [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) 方法都會使用資料庫內容。</span><span class="sxs-lookup"><span data-stu-id="77fd0-517">The database context is used in each of the [CRUD](https://wikipedia.org/wiki/Create,_read,_update_and_delete) methods in the controller.</span></span>
* <span data-ttu-id="77fd0-518">如果資料庫是空的，請將名為 `Item1` 的項目新增至資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-518">Adds an item named `Item1` to the database if the database is empty.</span></span> <span data-ttu-id="77fd0-519">此程式碼是在建構函式中，因此每次執行都會有新的 HTTP 要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-519">This code is in the constructor, so it runs every time there's a new HTTP request.</span></span> <span data-ttu-id="77fd0-520">如果您刪除所有項目，則建構函式會在下次呼叫 API 方法時重新建立 `Item1`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-520">If you delete all items, the constructor creates `Item1` again the next time an API method is called.</span></span> <span data-ttu-id="77fd0-521">因此看起來雖然像是刪除失敗，但實際為成功。</span><span class="sxs-lookup"><span data-stu-id="77fd0-521">So it may look like the deletion didn't work when it actually did work.</span></span>

## <a name="add-get-methods"></a><span data-ttu-id="77fd0-522">新增 Get 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-522">Add Get methods</span></span>

<span data-ttu-id="77fd0-523">若要提供擷取待辦事項的 API，請在 `TodoController` 類別中新增下列方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-523">To provide an API that retrieves to-do items, add the following methods to the `TodoController` class:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetAll)]

<span data-ttu-id="77fd0-524">這些方法會實作兩個 GET 端點：</span><span class="sxs-lookup"><span data-stu-id="77fd0-524">These methods implement two GET endpoints:</span></span>

* `GET /api/todo`
* `GET /api/todo/{id}`

<span data-ttu-id="77fd0-525">如果應用程式仍在執行，請將其停止。</span><span class="sxs-lookup"><span data-stu-id="77fd0-525">Stop the app if it's still running.</span></span> <span data-ttu-id="77fd0-526">然後重新予以執行以包含最新的變更。</span><span class="sxs-lookup"><span data-stu-id="77fd0-526">Then run it again to include the latest changes.</span></span>

<span data-ttu-id="77fd0-527">從瀏覽器呼叫這兩個端點來測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-527">Test the app by calling the two endpoints from a browser.</span></span> <span data-ttu-id="77fd0-528">例如：</span><span class="sxs-lookup"><span data-stu-id="77fd0-528">For example:</span></span>

* `https://localhost:<port>/api/todo`
* `https://localhost:<port>/api/todo/1`

<span data-ttu-id="77fd0-529">以下是呼叫 `GetTodoItems` 所產生的 HTTP 回應：</span><span class="sxs-lookup"><span data-stu-id="77fd0-529">The following HTTP response is produced by the call to `GetTodoItems`:</span></span>

```json
[
  {
    "id": 1,
    "name": "Item1",
    "isComplete": false
  }
]
```

## <a name="routing-and-url-paths"></a><span data-ttu-id="77fd0-530">傳送和 URL 路徑</span><span class="sxs-lookup"><span data-stu-id="77fd0-530">Routing and URL paths</span></span>

<span data-ttu-id="77fd0-531">[`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute)屬性代表回應 HTTP GET 要求的方法。</span><span class="sxs-lookup"><span data-stu-id="77fd0-531">The [`[HttpGet]`](/dotnet/api/microsoft.aspnetcore.mvc.httpgetattribute) attribute denotes a method that responds to an HTTP GET request.</span></span> <span data-ttu-id="77fd0-532">每個方法的 URL 路徑的建構方式如下：</span><span class="sxs-lookup"><span data-stu-id="77fd0-532">The URL path for each method is constructed as follows:</span></span>

* <span data-ttu-id="77fd0-533">一開始在控制器的 `Route` 屬性中使用範本字串：</span><span class="sxs-lookup"><span data-stu-id="77fd0-533">Start with the template string in the controller's `Route` attribute:</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=TodoController&highlight=3)]

* <span data-ttu-id="77fd0-534">以控制器的名稱取代 `[controller]`，也就是將控制器類別名稱減去 "Controller" 字尾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-534">Replace `[controller]` with the name of the controller, which by convention is the controller class name minus the "Controller" suffix.</span></span> <span data-ttu-id="77fd0-535">在此範例中，控制器類別名稱是 **Todo**Controller，因此容器名稱是 "todo"。</span><span class="sxs-lookup"><span data-stu-id="77fd0-535">For this sample, the controller class name is **Todo**Controller, so the controller name is "todo".</span></span> <span data-ttu-id="77fd0-536">ASP.NET Core [路由](xref:mvc/controllers/routing)不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-536">ASP.NET Core [routing](xref:mvc/controllers/routing) is case insensitive.</span></span>
* <span data-ttu-id="77fd0-537">如果 `[HttpGet]` 屬性具有路由範本 (例如 `[HttpGet("products")]`)，請將其附加到路徑。</span><span class="sxs-lookup"><span data-stu-id="77fd0-537">If the `[HttpGet]` attribute has a route template (for example, `[HttpGet("products")]`), append that to the path.</span></span> <span data-ttu-id="77fd0-538">此範例不使用範本。</span><span class="sxs-lookup"><span data-stu-id="77fd0-538">This sample doesn't use a template.</span></span> <span data-ttu-id="77fd0-539">如需詳細資訊，請參閱[使用 Http[Verb] 屬性的屬性路由](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-539">For more information, see [Attribute routing with Http[Verb] attributes](xref:mvc/controllers/routing#attribute-routing-with-httpverb-attributes).</span></span>

<span data-ttu-id="77fd0-540">在下列 `GetTodoItem` 方法中，`"{id}"` 是待辦事項唯一識別碼的預留位置變數。</span><span class="sxs-lookup"><span data-stu-id="77fd0-540">In the following `GetTodoItem` method, `"{id}"` is a placeholder variable for the unique identifier of the to-do item.</span></span> <span data-ttu-id="77fd0-541">在叫用 `GetTodoItem` 時，會將 URL 中的 `"{id}"` 值提供給方法的 `id` 參數。</span><span class="sxs-lookup"><span data-stu-id="77fd0-541">When `GetTodoItem` is invoked, the value of `"{id}"` in the URL is provided to the method in its`id` parameter.</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

## <a name="return-values"></a><span data-ttu-id="77fd0-542">傳回值</span><span class="sxs-lookup"><span data-stu-id="77fd0-542">Return values</span></span>

<span data-ttu-id="77fd0-543">`GetTodoItems` 和 `GetTodoItem` 方法的傳回型別為 [ActionResult\<T> 類型](xref:web-api/action-return-types#actionresultt-type)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-543">The return type of the `GetTodoItems` and `GetTodoItem` methods is [ActionResult\<T> type](xref:web-api/action-return-types#actionresultt-type).</span></span> <span data-ttu-id="77fd0-544">ASP.NET Core 會自動將物件序列化為 [JSON](https://www.json.org/)，並將 JSON 寫入至回應訊息的本文。</span><span class="sxs-lookup"><span data-stu-id="77fd0-544">ASP.NET Core automatically serializes the object to [JSON](https://www.json.org/) and writes the JSON into the body of the response message.</span></span> <span data-ttu-id="77fd0-545">此傳回型別的回應碼為 200，假設沒有任何未處理的例外狀況。</span><span class="sxs-lookup"><span data-stu-id="77fd0-545">The response code for this return type is 200, assuming there are no unhandled exceptions.</span></span> <span data-ttu-id="77fd0-546">未處理的例外狀況會轉譯成 5xx 錯誤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-546">Unhandled exceptions are translated into 5xx errors.</span></span>

<span data-ttu-id="77fd0-547">`ActionResult` 傳回型別可代表各種 HTTP 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-547">`ActionResult` return types can represent a wide range of HTTP status codes.</span></span> <span data-ttu-id="77fd0-548">例如，`GetTodoItem` 可傳回兩個不同的狀態值：</span><span class="sxs-lookup"><span data-stu-id="77fd0-548">For example, `GetTodoItem` can return two different status values:</span></span>

* <span data-ttu-id="77fd0-549">如果沒有項目符合所要求的識別碼，方法會傳回 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) 錯誤碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-549">If no item matches the requested ID, the method returns a 404 [NotFound](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.notfound) error code.</span></span>
* <span data-ttu-id="77fd0-550">否則，方法會傳回 200 與 JSON 回應本文。</span><span class="sxs-lookup"><span data-stu-id="77fd0-550">Otherwise, the method returns 200 with a JSON response body.</span></span> <span data-ttu-id="77fd0-551">傳回 `item` 會導致 HTTP 200 回應。</span><span class="sxs-lookup"><span data-stu-id="77fd0-551">Returning `item` results in an HTTP 200 response.</span></span>

## <a name="test-the-gettodoitems-method"></a><span data-ttu-id="77fd0-552">測試 GetTodoItems 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-552">Test the GetTodoItems method</span></span>

<span data-ttu-id="77fd0-553">本教學課程使用 Postman 來測試 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-553">This tutorial uses Postman to test the web API.</span></span>

* <span data-ttu-id="77fd0-554">安裝[Postman](https://www.getpostman.com/downloads/)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-554">Install [Postman](https://www.getpostman.com/downloads/).</span></span>
* <span data-ttu-id="77fd0-555">啟動 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-555">Start the web app.</span></span>
* <span data-ttu-id="77fd0-556">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="77fd0-556">Start Postman.</span></span>
* <span data-ttu-id="77fd0-557">停用**SSL 憑證驗證**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-557">Disable **SSL certificate verification**.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="77fd0-558">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="77fd0-558">Visual Studio</span></span>](#tab/visual-studio)

* <span data-ttu-id="77fd0-559">從 [檔案]\*\* [設定]\*\* > \*\*\*\* ([一般]\*\*\*\* 索引標籤)，停用 [SSL 憑證驗證]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-559">From **File** > **Settings** (**General** tab), disable **SSL certificate verification**.</span></span>

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="77fd0-560">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="77fd0-560">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

* <span data-ttu-id="77fd0-561">從 [ **Postman** > **喜好**設定] （**[一般**] 索引標籤），停用**SSL 憑證驗證**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-561">From **Postman** > **Preferences** (**General** tab), disable **SSL certificate verification**.</span></span> <span data-ttu-id="77fd0-562">或者，選取扳手並選取 [設定]\*\*\*\*，然後停用 [SSL 憑證驗證]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-562">Alternatively, select the wrench and select **Settings**, then disable the SSL certificate verification.</span></span>

---
  
> [!WARNING]
> <span data-ttu-id="77fd0-563">在測試控制器之後，請重新啟用 [SSL certificate verification] \(SSL 憑證驗證\)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-563">Re-enable SSL certificate verification after testing the controller.</span></span>

* <span data-ttu-id="77fd0-564">建立新的要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-564">Create a new request.</span></span>
  * <span data-ttu-id="77fd0-565">將 HTTP 方法設定為 **GET**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-565">Set the HTTP method to **GET**.</span></span>
  * <span data-ttu-id="77fd0-566">將要求 URL 設定為 `https://localhost:<port>/api/todo`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-566">Set the request URL to `https://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="77fd0-567">例如： `https://localhost:5001/api/todo` 。</span><span class="sxs-lookup"><span data-stu-id="77fd0-567">For example, `https://localhost:5001/api/todo`.</span></span>
* <span data-ttu-id="77fd0-568">在 Postman 中，設定 [Two pane view] \(雙窗格檢視\)\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-568">Set **Two pane view** in Postman.</span></span>
* <span data-ttu-id="77fd0-569">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-569">Select **Send**.</span></span>

![Postman 與 GET 要求](first-web-api/_static/2pv.png)

## <a name="add-a-create-method"></a><span data-ttu-id="77fd0-571">新增 Create 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-571">Add a Create method</span></span>

<span data-ttu-id="77fd0-572">在 *Controllers/TodoController.cs* 內部新增下列 `PostTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-572">Add the following `PostTodoItem` method inside of *Controllers/TodoController.cs*:</span></span> 

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="77fd0-573">上述程式碼是 HTTP POST 方法，如[`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute)屬性所示。</span><span class="sxs-lookup"><span data-stu-id="77fd0-573">The preceding code is an HTTP POST method, as indicated by the [`[HttpPost]`](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="77fd0-574">該方法會從 HTTP 要求本文取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="77fd0-574">The method gets the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="77fd0-575">`CreatedAtAction` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-575">The `CreatedAtAction` method:</span></span>

* <span data-ttu-id="77fd0-576">成功時會傳回 HTTP 201 狀態碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-576">Returns an HTTP 201 status code, if successful.</span></span> <span data-ttu-id="77fd0-577">對於可在伺服器上建立新資源的 HTTP POST 方法，其標準回應是 HTTP 201。</span><span class="sxs-lookup"><span data-stu-id="77fd0-577">HTTP 201 is the standard response for an HTTP POST method that creates a new resource on the server.</span></span>
* <span data-ttu-id="77fd0-578">將 `Location` 標頭加到回應中。</span><span class="sxs-lookup"><span data-stu-id="77fd0-578">Adds a `Location` header to the response.</span></span> <span data-ttu-id="77fd0-579">`Location` 標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="77fd0-579">The `Location` header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="77fd0-580">如需詳細資訊，請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (已建立 10.2.2 201)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-580">For more information, see [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>
* <span data-ttu-id="77fd0-581">參考 `GetTodoItem` 動作以建立 `Location` 標頭的 URI。</span><span class="sxs-lookup"><span data-stu-id="77fd0-581">References the `GetTodoItem` action to create the `Location` header's URI.</span></span> <span data-ttu-id="77fd0-582">C# `nameof` 關鍵字是用來避免在 `CreatedAtAction` 呼叫中以硬式編碼方式寫入動作名稱。</span><span class="sxs-lookup"><span data-stu-id="77fd0-582">The C# `nameof` keyword is used to avoid hard-coding the action name in the `CreatedAtAction` call.</span></span>

  [!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_GetByID&highlight=1-2)]

### <a name="test-the-posttodoitem-method"></a><span data-ttu-id="77fd0-583">測試 PostTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-583">Test the PostTodoItem method</span></span>

* <span data-ttu-id="77fd0-584">建置專案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-584">Build the project.</span></span>
* <span data-ttu-id="77fd0-585">在 Postman 中，將 HTTP 方法設定為 `POST`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-585">In Postman, set the HTTP method to `POST`.</span></span>
* <span data-ttu-id="77fd0-586">選取 [Body]\*\*\*\* \(本文\) 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-586">Select the **Body** tab.</span></span>
* <span data-ttu-id="77fd0-587">選取 [原始]\*\*\*\* 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="77fd0-587">Select the **raw** radio button.</span></span>
* <span data-ttu-id="77fd0-588">將類型設定為 **JSON (application/json)**。</span><span class="sxs-lookup"><span data-stu-id="77fd0-588">Set the type to **JSON (application/json)**.</span></span>
* <span data-ttu-id="77fd0-589">在要求本文中，針對待辦項目輸入 JSON：</span><span class="sxs-lookup"><span data-stu-id="77fd0-589">In the request body enter JSON for a to-do item:</span></span>

    ```json
    {
      "name":"walk dog",
      "isComplete":true
    }
    ```

* <span data-ttu-id="77fd0-590">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-590">Select **Send**.</span></span>

  ![Postman 與建立要求](first-web-api/_static/create.png)

  <span data-ttu-id="77fd0-592">如果您收到 405「不允許的方法」錯誤，可能是由於新增 `PostTodoItem` 方法之後未編譯專案所導致。</span><span class="sxs-lookup"><span data-stu-id="77fd0-592">If you get a 405 Method Not Allowed error, it's probably the result of not compiling the project after adding the `PostTodoItem` method.</span></span>

### <a name="test-the-location-header-uri"></a><span data-ttu-id="77fd0-593">測試位置標頭 URI</span><span class="sxs-lookup"><span data-stu-id="77fd0-593">Test the location header URI</span></span>

* <span data-ttu-id="77fd0-594">在 [回應]\*\*\*\* 窗格中選取 [標頭]\*\*\*\* 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="77fd0-594">Select the **Headers** tab in the **Response** pane.</span></span>
* <span data-ttu-id="77fd0-595">複製 [位置]\*\*\*\* 標頭值：</span><span class="sxs-lookup"><span data-stu-id="77fd0-595">Copy the **Location** header value:</span></span>

  ![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

* <span data-ttu-id="77fd0-597">將方法設定為 GET。</span><span class="sxs-lookup"><span data-stu-id="77fd0-597">Set the method to GET.</span></span>
* <span data-ttu-id="77fd0-598">貼上`https://localhost:5001/api/Todo/2`URI （例如）。</span><span class="sxs-lookup"><span data-stu-id="77fd0-598">Paste the URI (for example, `https://localhost:5001/api/Todo/2`).</span></span>
* <span data-ttu-id="77fd0-599">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-599">Select **Send**.</span></span>

## <a name="add-a-puttodoitem-method"></a><span data-ttu-id="77fd0-600">新增 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-600">Add a PutTodoItem method</span></span>

<span data-ttu-id="77fd0-601">請新增下列 `PutTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-601">Add the following `PutTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="77fd0-602">`PutTodoItem` 類似於 `PostTodoItem`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="77fd0-602">`PutTodoItem` is similar to `PostTodoItem`, except it uses HTTP PUT.</span></span> <span data-ttu-id="77fd0-603">回應為[204 （沒有內容）](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-603">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="77fd0-604">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是變更。</span><span class="sxs-lookup"><span data-stu-id="77fd0-604">According to the HTTP specification, a PUT request requires the client to send the entire updated entity, not just the changes.</span></span> <span data-ttu-id="77fd0-605">若要支援部分更新，請使用 [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-605">To support partial updates, use [HTTP PATCH](xref:Microsoft.AspNetCore.Mvc.HttpPatchAttribute).</span></span>

<span data-ttu-id="77fd0-606">如果在呼叫 `PutTodoItem` 時發生錯誤，請呼叫 `GET` 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-606">If you get an error calling `PutTodoItem`, call `GET` to ensure there's an item in the database.</span></span>

### <a name="test-the-puttodoitem-method"></a><span data-ttu-id="77fd0-607">測試 PutTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-607">Test the PutTodoItem method</span></span>

<span data-ttu-id="77fd0-608">這個範例會使用必須在每次啟動應用程式時初始化的記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-608">This sample uses an in-memory database that must be initialized each time the app is started.</span></span> <span data-ttu-id="77fd0-609">資料庫中必須有項目，您才能進行 PUT 呼叫。</span><span class="sxs-lookup"><span data-stu-id="77fd0-609">There must be an item in the database before you make a PUT call.</span></span> <span data-ttu-id="77fd0-610">在發出 PUT 呼叫之前，請先呼叫 GET 以確保資料庫中有項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-610">Call GET to insure there's an item in the database before making a PUT call.</span></span>

<span data-ttu-id="77fd0-611">更新識別碼為 1 的待辦事項，並將其名稱設定為 "feed fish"：</span><span class="sxs-lookup"><span data-stu-id="77fd0-611">Update the to-do item that has id = 1 and set its name to "feed fish":</span></span>

```json
  {
    "ID":1,
    "name":"feed fish",
    "isComplete":true
  }
```

<span data-ttu-id="77fd0-612">下圖顯示 Postman 更新：</span><span class="sxs-lookup"><span data-stu-id="77fd0-612">The following image shows the Postman update:</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

## <a name="add-a-deletetodoitem-method"></a><span data-ttu-id="77fd0-614">新增 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-614">Add a DeleteTodoItem method</span></span>

<span data-ttu-id="77fd0-615">請新增下列 `DeleteTodoItem` 方法：</span><span class="sxs-lookup"><span data-stu-id="77fd0-615">Add the following `DeleteTodoItem` method:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="77fd0-616">`DeleteTodoItem` 回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) \(204 (沒有內容)\)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-616">The `DeleteTodoItem` response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

### <a name="test-the-deletetodoitem-method"></a><span data-ttu-id="77fd0-617">測試 DeleteTodoItem 方法</span><span class="sxs-lookup"><span data-stu-id="77fd0-617">Test the DeleteTodoItem method</span></span>

<span data-ttu-id="77fd0-618">使用 Postman 刪除待辦事項：</span><span class="sxs-lookup"><span data-stu-id="77fd0-618">Use Postman to delete a to-do item:</span></span>

* <span data-ttu-id="77fd0-619">將方法設定為 `DELETE`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-619">Set the method to `DELETE`.</span></span>
* <span data-ttu-id="77fd0-620">設定要刪除之物件的 URI （例如， `https://localhost:5001/api/todo/1`）。</span><span class="sxs-lookup"><span data-stu-id="77fd0-620">Set the URI of the object to delete (for example, `https://localhost:5001/api/todo/1`).</span></span>
* <span data-ttu-id="77fd0-621">選取 [**傳送**]。</span><span class="sxs-lookup"><span data-stu-id="77fd0-621">Select **Send**.</span></span>

<span data-ttu-id="77fd0-622">範例應用程式可讓您刪除所有項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-622">The sample app allows you to delete all the items.</span></span> <span data-ttu-id="77fd0-623">但刪除最後一個項目之後，模型類別建構函式會在下次呼叫 API 時建立新的項目。</span><span class="sxs-lookup"><span data-stu-id="77fd0-623">However, when the last item is deleted, a new one is created by the model class constructor the next time the API is called.</span></span>

## <a name="call-the-web-api-with-javascript"></a><span data-ttu-id="77fd0-624">使用 JavaScript 呼叫 Web API</span><span class="sxs-lookup"><span data-stu-id="77fd0-624">Call the web API with JavaScript</span></span>

<span data-ttu-id="77fd0-625">在此節中，將會新增 HTML 網頁，以使用 JavaScript 來呼叫 Web API。</span><span class="sxs-lookup"><span data-stu-id="77fd0-625">In this section, an HTML page is added that uses JavaScript to call the web API.</span></span> <span data-ttu-id="77fd0-626">jQuery 會起始要求。</span><span class="sxs-lookup"><span data-stu-id="77fd0-626">jQuery initiates the request.</span></span> <span data-ttu-id="77fd0-627">JavaScript 會使用來自 Web API 回應的詳細資料來更新頁面。</span><span class="sxs-lookup"><span data-stu-id="77fd0-627">JavaScript updates the page with the details from the web API's response.</span></span>

<span data-ttu-id="77fd0-628">藉由使用下列反白顯示的程式碼更新 *Startup.cs*，來設定應用程式[提供靜態檔案](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)並[啟用預設檔案對應](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_)：</span><span class="sxs-lookup"><span data-stu-id="77fd0-628">Configure the app to [serve static files](/dotnet/api/microsoft.aspnetcore.builder.staticfileextensions.usestaticfiles#Microsoft_AspNetCore_Builder_StaticFileExtensions_UseStaticFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) and [enable default file mapping](/dotnet/api/microsoft.aspnetcore.builder.defaultfilesextensions.usedefaultfiles#Microsoft_AspNetCore_Builder_DefaultFilesExtensions_UseDefaultFiles_Microsoft_AspNetCore_Builder_IApplicationBuilder_) by updating *Startup.cs* with the following highlighted code:</span></span>

[!code-csharp[](first-web-api/samples/2.2/TodoApi/Startup.cs?highlight=14-15&name=snippet_configure)]

<span data-ttu-id="77fd0-629">在專案目錄中建立 *wwwroot* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="77fd0-629">Create a *wwwroot* folder in the project directory.</span></span>

<span data-ttu-id="77fd0-630">將名為 *index.html* 的 HTML 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="77fd0-630">Add an HTML file named *index.html* to the *wwwroot* directory.</span></span> <span data-ttu-id="77fd0-631">將其內容取代為下列標記：</span><span class="sxs-lookup"><span data-stu-id="77fd0-631">Replace its contents with the following markup:</span></span>

[!code-html[](first-web-api/samples/2.2/TodoApi/wwwroot/index.html)]

<span data-ttu-id="77fd0-632">將名為 *site.js* 的 JavaScript 檔案新增至 *wwwroot* 目錄。</span><span class="sxs-lookup"><span data-stu-id="77fd0-632">Add a JavaScript file named *site.js* to the *wwwroot* directory.</span></span> <span data-ttu-id="77fd0-633">以下列程式碼取代其內容：</span><span class="sxs-lookup"><span data-stu-id="77fd0-633">Replace its contents with the following code:</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_SiteJs)]

<span data-ttu-id="77fd0-634">若要在本機測試 HTML 網頁，可能需要變更 ASP.NET Core 專案的啟動設定：</span><span class="sxs-lookup"><span data-stu-id="77fd0-634">A change to the ASP.NET Core project's launch settings may be required to test the HTML page locally:</span></span>

* <span data-ttu-id="77fd0-635">開啟 *Properties\launchSettings.json*。</span><span class="sxs-lookup"><span data-stu-id="77fd0-635">Open *Properties\launchSettings.json*.</span></span>
* <span data-ttu-id="77fd0-636">請移除`launchUrl`屬性，以強制應用程式在*index. html*&mdash;開啟專案的預設檔案。</span><span class="sxs-lookup"><span data-stu-id="77fd0-636">Remove the `launchUrl` property to force the app to open at *index.html*&mdash;the project's default file.</span></span>

<span data-ttu-id="77fd0-637">此範例會呼叫 Web API 的所有 CRUD 方法。</span><span class="sxs-lookup"><span data-stu-id="77fd0-637">This sample calls all of the CRUD methods of the web API.</span></span> <span data-ttu-id="77fd0-638">以下是關於呼叫 API 的說明。</span><span class="sxs-lookup"><span data-stu-id="77fd0-638">Following are explanations of the calls to the API.</span></span>

### <a name="get-a-list-of-to-do-items"></a><span data-ttu-id="77fd0-639">取得待辦事項的清單</span><span class="sxs-lookup"><span data-stu-id="77fd0-639">Get a list of to-do items</span></span>

<span data-ttu-id="77fd0-640">jQuery 會將 HTTP GET 要求傳送至 Web API，這會傳回代表待辦事項陣列的 JSON。</span><span class="sxs-lookup"><span data-stu-id="77fd0-640">jQuery sends an HTTP GET request to the web API, which returns JSON representing an array of to-do items.</span></span> <span data-ttu-id="77fd0-641">如果要求成功，則會叫用 `success` 回呼函式。</span><span class="sxs-lookup"><span data-stu-id="77fd0-641">The `success` callback function is invoked if the request succeeds.</span></span> <span data-ttu-id="77fd0-642">在回呼中，DOM 已使用待辦事項資訊進行更新。</span><span class="sxs-lookup"><span data-stu-id="77fd0-642">In the callback, the DOM is updated with the to-do information.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_GetData)]

### <a name="add-a-to-do-item"></a><span data-ttu-id="77fd0-643">新增待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-643">Add a to-do item</span></span>

<span data-ttu-id="77fd0-644">jQuery 會傳送 HTTP POST 要求和要求主體中的待辦事項。</span><span class="sxs-lookup"><span data-stu-id="77fd0-644">jQuery sends an HTTP POST request with the to-do item in the request body.</span></span> <span data-ttu-id="77fd0-645">`accepts` 和 `contentType` 選項都設定為 `application/json`，以指定接收和傳送的媒體類型。</span><span class="sxs-lookup"><span data-stu-id="77fd0-645">The `accepts` and `contentType` options are set to `application/json` to specify the media type being received and sent.</span></span> <span data-ttu-id="77fd0-646">待辦事項會使用 [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify) 轉換成 JSON。</span><span class="sxs-lookup"><span data-stu-id="77fd0-646">The to-do item is converted to JSON by using [JSON.stringify](https://developer.mozilla.org/docs/Web/JavaScript/Reference/Global_Objects/JSON/stringify).</span></span> <span data-ttu-id="77fd0-647">當 API 傳回成功狀態碼時，會叫用 `getData` 函式來更新 HTML 資料表。</span><span class="sxs-lookup"><span data-stu-id="77fd0-647">When the API returns a successful status code, the `getData` function is invoked to update the HTML table.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AddItem)]

### <a name="update-a-to-do-item"></a><span data-ttu-id="77fd0-648">更新待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-648">Update a to-do item</span></span>

<span data-ttu-id="77fd0-649">更新待辦事項類似於新增待辦事項。</span><span class="sxs-lookup"><span data-stu-id="77fd0-649">Updating a to-do item is similar to adding one.</span></span> <span data-ttu-id="77fd0-650">`url` 會變更為新增項目的唯一識別碼，而 `type` 是 `PUT`。</span><span class="sxs-lookup"><span data-stu-id="77fd0-650">The `url` changes to add the unique identifier of the item, and the `type` is `PUT`.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxPut)]

### <a name="delete-a-to-do-item"></a><span data-ttu-id="77fd0-651">刪除待辦事項</span><span class="sxs-lookup"><span data-stu-id="77fd0-651">Delete a to-do item</span></span>

<span data-ttu-id="77fd0-652">刪除待辦事項的達成方法是將 AJAX 呼叫的 `type` 設定為 `DELETE`，並在 URL 中指定項目的唯一識別碼。</span><span class="sxs-lookup"><span data-stu-id="77fd0-652">Deleting a to-do item is accomplished by setting the `type` on the AJAX call to `DELETE` and specifying the item's unique identifier in the URL.</span></span>

[!code-javascript[](first-web-api/samples/2.2/TodoApi/wwwroot/site.js?name=snippet_AjaxDelete)]

::: moniker-end

<a name="auth"></a>

## <a name="add-authentication-support-to-a-web-api"></a><span data-ttu-id="77fd0-653">將驗證支援新增至 Web API</span><span class="sxs-lookup"><span data-stu-id="77fd0-653">Add authentication support to a web API</span></span>

[!INCLUDE[](~/includes/IdentityServer4.md)]

## <a name="additional-resources"></a><span data-ttu-id="77fd0-654">其他資源</span><span class="sxs-lookup"><span data-stu-id="77fd0-654">Additional resources</span></span>

<span data-ttu-id="77fd0-655">[檢視或下載本教學課程的範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-655">[View or download sample code for this tutorial](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/tutorials/first-web-api/samples).</span></span> <span data-ttu-id="77fd0-656">請參閱[如何下載](xref:index#how-to-download-a-sample)。</span><span class="sxs-lookup"><span data-stu-id="77fd0-656">See [how to download](xref:index#how-to-download-a-sample).</span></span>

<span data-ttu-id="77fd0-657">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="77fd0-657">For more information, see the following resources:</span></span>

* <xref:web-api/index>
* <xref:tutorials/web-api-help-pages-using-swagger>
* <xref:data/ef-rp/intro>
* <xref:mvc/controllers/routing>
* <xref:web-api/action-return-types>
* <xref:host-and-deploy/azure-apps/index>
* <xref:host-and-deploy/index>
* [<span data-ttu-id="77fd0-658">本教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="77fd0-658">YouTube version of this tutorial</span></span>](https://www.youtube.com/watch?v=TTkhEyGBfAk)
