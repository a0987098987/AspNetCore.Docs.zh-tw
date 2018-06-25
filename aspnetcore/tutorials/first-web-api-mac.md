---
title: 使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API
author: rick-anderson
description: 使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API
ms.author: riande
ms.custom: mvc
ms.date: 05/08/2018
uid: tutorials/first-web-api-mac
ms.openlocfilehash: 4caa6d9057de8d0e821c4abefe22985f43ff95ad
ms.sourcegitcommit: a1afd04758e663d7062a5bfa8a0d4dca38f42afc
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/20/2018
ms.locfileid: "36279606"
---
# <a name="create-a-web-api-with-aspnet-core-and-visual-studio-for-mac"></a><span data-ttu-id="36270-103">使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="36270-103">Create a Web API with ASP.NET Core and Visual Studio for Mac</span></span>

<span data-ttu-id="36270-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="36270-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="36270-105">在本教學課程中，請建置 Web API 來管理「待辦事項」項目清單。</span><span class="sxs-lookup"><span data-stu-id="36270-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="36270-106">不會建構 UI。</span><span class="sxs-lookup"><span data-stu-id="36270-106">The UI isn't constructed.</span></span>

<span data-ttu-id="36270-107">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="36270-107">There are three versions of this tutorial:</span></span>

* <span data-ttu-id="36270-108">macOS：使用 Visual Studio for Mac 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="36270-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="36270-109">Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="36270-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="36270-110">macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="36270-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

<span data-ttu-id="36270-111">如需使用持續性資料庫的範例，請參閱 [ macOS 或 Linux 上的 ASP.NET Core MVC 簡介](xref:tutorials/first-mvc-app-xplat/index)。</span><span class="sxs-lookup"><span data-stu-id="36270-111">See [Introduction to ASP.NET Core MVC on macOS or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="36270-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="36270-112">Prerequisites</span></span>

[!INCLUDE[](~/includes/net-core-prereqs-macos.md)]

## <a name="create-the-project"></a><span data-ttu-id="36270-113">建立專案</span><span class="sxs-lookup"><span data-stu-id="36270-113">Create the project</span></span>

<span data-ttu-id="36270-114">從 Visual Studio 中，選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="36270-114">From Visual Studio, select **File** > **New Solution**.</span></span>

![macOS 新增方案](first-web-api-mac/_static/sln.png)

<span data-ttu-id="36270-116">選取 [.NET Core 應用程式] > [ASP.NET Core Web API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="36270-116">Select **.NET Core App** > **ASP.NET Core Web API** > **Next**.</span></span>

![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)

<span data-ttu-id="36270-118">為 [專案名稱] 輸入 *TodoApi*，然後按一下 [建立]。</span><span class="sxs-lookup"><span data-stu-id="36270-118">Enter *TodoApi* for the **Project Name**, and then click **Create**.</span></span>

![設定對話方塊](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="36270-120">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="36270-120">Launch the app</span></span>

<span data-ttu-id="36270-121">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="36270-121">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="36270-122">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="36270-122">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="36270-123">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="36270-123">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="36270-124">將 URL 變更為 `http://localhost:<port>/api/values`。</span><span class="sxs-lookup"><span data-stu-id="36270-124">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="36270-125">隨即顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="36270-125">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="36270-126">新增 Entity Framework Core 的支援</span><span class="sxs-lookup"><span data-stu-id="36270-126">Add support for Entity Framework Core</span></span>

<span data-ttu-id="36270-127">安裝 [Entity Framework Core InMemory](/ef/core/providers/in-memory/) 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="36270-127">Install the [Entity Framework Core InMemory](/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="36270-128">此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="36270-128">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="36270-129">從 [專案] 功能表中，選取 [新增 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="36270-129">From the **Project** menu, select **Add NuGet Packages**.</span></span>

  * <span data-ttu-id="36270-130">或者，您可以用滑鼠右鍵按一下 [相依性]，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="36270-130">Alternatively, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="36270-131">在搜尋方塊中輸入 `EntityFrameworkCore.InMemory`。</span><span class="sxs-lookup"><span data-stu-id="36270-131">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="36270-132">選取 `Microsoft.EntityFrameworkCore.InMemory`，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="36270-132">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="36270-133">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="36270-133">Add a model class</span></span>

<span data-ttu-id="36270-134">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="36270-134">A model is an object representing the data in your app.</span></span> <span data-ttu-id="36270-135">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="36270-135">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="36270-136">在方案總管中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="36270-136">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="36270-137">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="36270-137">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="36270-138">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="36270-138">Name the folder *Models*.</span></span>

![新增資料夾](first-web-api-mac/_static/folder.png)

> [!NOTE]
> <span data-ttu-id="36270-140">您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="36270-140">You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="36270-141">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="36270-141">Right-click the *Models* folder, and select **Add** > **New File** > **General** > **Empty Class**.</span></span> <span data-ttu-id="36270-142">將類別命名為 *TodoItem*，然後按一下 [新增]。</span><span class="sxs-lookup"><span data-stu-id="36270-142">Name the class *TodoItem*, and then click **New**.</span></span>

<span data-ttu-id="36270-143">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="36270-143">Replace the generated code with:</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="36270-144">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="36270-144">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="36270-145">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="36270-145">Create the database context</span></span>

<span data-ttu-id="36270-146">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="36270-146">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="36270-147">您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。</span><span class="sxs-lookup"><span data-stu-id="36270-147">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="36270-148">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="36270-148">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="36270-149">新增控制器</span><span class="sxs-lookup"><span data-stu-id="36270-149">Add a controller</span></span>

<span data-ttu-id="36270-150">在方案總管的 *Controllers* 資料夾中新增 `TodoController` 類別。</span><span class="sxs-lookup"><span data-stu-id="36270-150">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="36270-151">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="36270-151">Replace the generated code with the following:</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="36270-152">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="36270-152">Launch the app</span></span>

<span data-ttu-id="36270-153">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="36270-153">In Visual Studio, select **Run** > **Start With Debugging** to launch the app.</span></span> <span data-ttu-id="36270-154">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:<port>`，其中 `<port>` 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="36270-154">Visual Studio launches a browser and navigates to `http://localhost:<port>`, where `<port>` is a randomly chosen port number.</span></span> <span data-ttu-id="36270-155">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="36270-155">You get an HTTP 404 (Not Found) error.</span></span> <span data-ttu-id="36270-156">將 URL 變更為 `http://localhost:<port>/api/values`。</span><span class="sxs-lookup"><span data-stu-id="36270-156">Change the URL to `http://localhost:<port>/api/values`.</span></span> <span data-ttu-id="36270-157">隨即顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="36270-157">The `ValuesController` data is displayed:</span></span>

```json
["value1","value2"]
```

<span data-ttu-id="36270-158">瀏覽至位於 `http://localhost:<port>/api/todo` 的 `Todo` 控制器。</span><span class="sxs-lookup"><span data-stu-id="36270-158">Navigate to the `Todo` controller at `http://localhost:<port>/api/todo`.</span></span> <span data-ttu-id="36270-159">即會傳回下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="36270-159">The following JSON is returned:</span></span>

```json
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="36270-160">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="36270-160">Implement the other CRUD operations</span></span>

<span data-ttu-id="36270-161">我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。</span><span class="sxs-lookup"><span data-stu-id="36270-161">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="36270-162">這些方法是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。</span><span class="sxs-lookup"><span data-stu-id="36270-162">These methods are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="36270-163">請在新增或變更程式碼之後建置專案。</span><span class="sxs-lookup"><span data-stu-id="36270-163">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="36270-164">建立</span><span class="sxs-lookup"><span data-stu-id="36270-164">Create</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="36270-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="36270-165">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="36270-166">上述方法會回應 HTTP POST，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="36270-166">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="36270-167">[[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="36270-167">The [[FromBody]](/dotnet/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="36270-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span><span class="sxs-lookup"><span data-stu-id="36270-168">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]</span></span>

<span data-ttu-id="36270-169">上述方法會回應 HTTP POST，如 [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性所示。</span><span class="sxs-lookup"><span data-stu-id="36270-169">The preceding method responds to an HTTP POST, as indicated by the [[HttpPost]](/dotnet/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="36270-170">MVC 會從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="36270-170">MVC gets the value of the to-do item from the body of the HTTP request.</span></span>
::: moniker-end

<span data-ttu-id="36270-171">`CreatedAtRoute` 方法會傳回 201 回應。</span><span class="sxs-lookup"><span data-stu-id="36270-171">The `CreatedAtRoute` method returns a 201 response.</span></span> <span data-ttu-id="36270-172">對於可在伺服器上建立新資源的 HTTP POST 方法，這是標準回應。</span><span class="sxs-lookup"><span data-stu-id="36270-172">It's the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="36270-173">`CreatedAtRoute` 也會將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="36270-173">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="36270-174">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="36270-174">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="36270-175">請參閱 [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="36270-175">See [10.2.2 201 Created](https://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="36270-176">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="36270-176">Use Postman to send a Create request</span></span>

* <span data-ttu-id="36270-177">啟動應用程式 ([執行] > [啟動並偵錯])。</span><span class="sxs-lookup"><span data-stu-id="36270-177">Start the app (**Run** > **Start With Debugging**).</span></span>
* <span data-ttu-id="36270-178">開啟 Postman。</span><span class="sxs-lookup"><span data-stu-id="36270-178">Open Postman.</span></span>

![Postman 主控台](first-web-api/_static/pmc.png)

* <span data-ttu-id="36270-180">更新 localhost URL 中的通訊埠號碼。</span><span class="sxs-lookup"><span data-stu-id="36270-180">Update the port number in the localhost URL.</span></span>
* <span data-ttu-id="36270-181">將 HTTP 方法設定為 *POST*。</span><span class="sxs-lookup"><span data-stu-id="36270-181">Set the HTTP method to *POST*.</span></span>
* <span data-ttu-id="36270-182">按一下 [本文] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="36270-182">Click the **Body** tab.</span></span>
* <span data-ttu-id="36270-183">選取 [原始] 選項按鈕。</span><span class="sxs-lookup"><span data-stu-id="36270-183">Select the **raw** radio button.</span></span>
* <span data-ttu-id="36270-184">將類型設定為 *JSON (application/json)*。</span><span class="sxs-lookup"><span data-stu-id="36270-184">Set the type to *JSON (application/json)*.</span></span>
* <span data-ttu-id="36270-185">輸入要求本文與待辦項目，類似於下列 JSON：</span><span class="sxs-lookup"><span data-stu-id="36270-185">Enter a request body with a to-do item resembling the following JSON:</span></span>

```json
{
  "name":"walk dog",
  "isComplete":true
}
```

* <span data-ttu-id="36270-186">按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="36270-186">Click the **Send** button.</span></span>

::: moniker range=">= aspnetcore-2.1"
> [!TIP]
> <span data-ttu-id="36270-187">如果按一下 [傳送] 之後沒有顯示任何回應，請停用 [SSL 憑證驗證] 選項。</span><span class="sxs-lookup"><span data-stu-id="36270-187">If no response displays after clicking **Send**, disable the **SSL certification verification** option.</span></span> <span data-ttu-id="36270-188">這位於 [檔案] > [設定] 下。</span><span class="sxs-lookup"><span data-stu-id="36270-188">This is found under **File** > **Settings**.</span></span> <span data-ttu-id="36270-189">停用設定之後，再按一下 [傳送] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="36270-189">Click the **Send** button again after disabling the setting.</span></span>
::: moniker-end

<span data-ttu-id="36270-190">按一下 [回應] 窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭值：</span><span class="sxs-lookup"><span data-stu-id="36270-190">Click the **Headers** tab in the **Response** pane and copy the **Location** header value:</span></span>

![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmc2.png)

<span data-ttu-id="36270-192">您可以使用 [位置] 標頭 URI 來存取您建立的資源。</span><span class="sxs-lookup"><span data-stu-id="36270-192">You can use the Location header URI to access the resource you created.</span></span> <span data-ttu-id="36270-193">`Create` 方法會傳回 [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_)。</span><span class="sxs-lookup"><span data-stu-id="36270-193">The `Create` method returns [CreatedAtRoute](/dotnet/api/microsoft.aspnetcore.mvc.controllerbase.createdatroute#Microsoft_AspNetCore_Mvc_ControllerBase_CreatedAtRoute_System_String_System_Object_System_Object_).</span></span> <span data-ttu-id="36270-194">第一個傳遞至 `CreatedAtRoute` 的參數代表用來產生 URL 的具名路由。</span><span class="sxs-lookup"><span data-stu-id="36270-194">The first parameter passed to `CreatedAtRoute` represents the named route to use for generating the URL.</span></span> <span data-ttu-id="36270-195">請記住，`GetById` 方法建立了 `"GetTodo"` 具名路由：</span><span class="sxs-lookup"><span data-stu-id="36270-195">Recall that the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
```

### <a name="update"></a><span data-ttu-id="36270-196">更新</span><span class="sxs-lookup"><span data-stu-id="36270-196">Update</span></span>

::: moniker range="<= aspnetcore-2.0"
<span data-ttu-id="36270-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="36270-197">[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end
::: moniker range=">= aspnetcore-2.1"
<span data-ttu-id="36270-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span><span class="sxs-lookup"><span data-stu-id="36270-198">[!code-csharp[](first-web-api/samples/2.1/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]</span></span>
::: moniker-end

<span data-ttu-id="36270-199">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="36270-199">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="36270-200">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="36270-200">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="36270-201">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="36270-201">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="36270-202">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="36270-202">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="36270-204">刪除</span><span class="sxs-lookup"><span data-stu-id="36270-204">Delete</span></span>

[!code-csharp[](first-web-api/samples/2.0/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="36270-205">回應是 [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="36270-205">The response is [204 (No Content)](https://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![顯示 204 (沒有內容) 回應的 Postman 主控台](first-web-api/_static/pmd.png)

[!INCLUDE[jQuery](../includes/webApi/add-jquery.md)]

[!INCLUDE[next steps](../includes/webApi/next.md)]
