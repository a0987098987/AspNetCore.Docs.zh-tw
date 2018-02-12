---
title: "使用 ASP.NET Core 和 Visual Studio for Mac 建立 Web API"
author: rick-anderson
description: "使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API"
helpviewer_heywords: ASP.NET Core, WebAPI, Web API, REST, mac, macOS, HTTP, Service, HTTP Service
manager: wpickett
ms.author: riande
ms.date: 09/15/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: tutorials/first-web-api-mac
ms.openlocfilehash: b0e1a331fe3229119f4669fa336b6af4822785bf
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="create-a-web-api-with-aspnet-core-mvc-and-visual-studio-for-mac"></a><span data-ttu-id="5a751-103">使用 ASP.NET Core MVC 和 Visual Studio for Mac 建立 Web API</span><span class="sxs-lookup"><span data-stu-id="5a751-103">Create a Web API with ASP.NET Core MVC and Visual Studio for Mac</span></span>

<span data-ttu-id="5a751-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Mike Wasson](https://github.com/mikewasson) 提供</span><span class="sxs-lookup"><span data-stu-id="5a751-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Mike Wasson](https://github.com/mikewasson)</span></span>

<span data-ttu-id="5a751-105">在本教學課程中，請建置 Web API 來管理「待辦事項」項目清單。</span><span class="sxs-lookup"><span data-stu-id="5a751-105">In this tutorial, build a web API for managing a list of "to-do" items.</span></span> <span data-ttu-id="5a751-106">不會建構 UI。</span><span class="sxs-lookup"><span data-stu-id="5a751-106">The UI isn't constructed.</span></span>

<span data-ttu-id="5a751-107">本教學課程有 3 個版本：</span><span class="sxs-lookup"><span data-stu-id="5a751-107">There are 3 versions of this tutorial:</span></span>

* <span data-ttu-id="5a751-108">macOS：使用 Visual Studio for Mac 建立 Web API (本教學課程)</span><span class="sxs-lookup"><span data-stu-id="5a751-108">macOS: Web API with Visual Studio for Mac (This tutorial)</span></span>
* <span data-ttu-id="5a751-109">Windows：[使用 Visual Studio for Windows 建立 Web API](xref:tutorials/first-web-api)</span><span class="sxs-lookup"><span data-stu-id="5a751-109">Windows: [Web API with Visual Studio for Windows](xref:tutorials/first-web-api)</span></span>
* <span data-ttu-id="5a751-110">macOS、Linux、Windows：[使用 Visual Studio Code 建立 Web API](xref:tutorials/web-api-vsc)</span><span class="sxs-lookup"><span data-stu-id="5a751-110">macOS, Linux, Windows: [Web API with Visual Studio Code](xref:tutorials/web-api-vsc)</span></span>

<!-- WARNING: The code AND images in this doc are used by uid: tutorials/web-api-vsc, tutorials/first-web-api-mac and tutorials/first-web-api. If you change any code/images in this tutorial, update uid: tutorials/web-api-vsc -->

[!INCLUDE[template files](../includes/webApi/intro.md)]

* <span data-ttu-id="5a751-111">如需使用持續性資料庫的範例，請參閱 [Mac 或 Linux 上的 ASP.NET Core MVC 簡介](xref:tutorials/first-mvc-app-xplat/index)。</span><span class="sxs-lookup"><span data-stu-id="5a751-111">See [Introduction to ASP.NET Core MVC on Mac or Linux](xref:tutorials/first-mvc-app-xplat/index) for an example that uses a persistent database.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="5a751-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="5a751-112">Prerequisites</span></span>

<span data-ttu-id="5a751-113">安裝下列項目：</span><span class="sxs-lookup"><span data-stu-id="5a751-113">Install the following:</span></span>

- <span data-ttu-id="5a751-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) (含) 以上版本</span><span class="sxs-lookup"><span data-stu-id="5a751-114">[.NET Core 2.0.0 SDK](https://www.microsoft.com/net/core) or later</span></span>
- [<span data-ttu-id="5a751-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="5a751-115">Visual Studio for Mac</span></span>](https://www.visualstudio.com/vs/visual-studio-mac/)

## <a name="create-the-project"></a><span data-ttu-id="5a751-116">建立專案</span><span class="sxs-lookup"><span data-stu-id="5a751-116">Create the project</span></span>

<span data-ttu-id="5a751-117">從 Visual Studio 中，選取 [檔案] > [新增方案]。</span><span class="sxs-lookup"><span data-stu-id="5a751-117">From Visual Studio, select **File > New Solution**.</span></span>

![macOS 新增方案](first-web-api-mac/_static/sln.png)

<span data-ttu-id="5a751-119">選取 [.NET Core 應用程式] > [ASP.NET Core Web API] > [下一步]。</span><span class="sxs-lookup"><span data-stu-id="5a751-119">Select **.NET Core App >  ASP.NET Core Web API > Next**.</span></span>

![macOS [新增專案] 對話方塊](first-web-api-mac/_static/1.png)

<span data-ttu-id="5a751-121">為 [專案名稱] 輸入 **TodoApi**，然後選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="5a751-121">Enter **TodoApi** for the **Project Name**, and then select Create.</span></span>

![設定對話方塊](first-web-api-mac/_static/2.png)

### <a name="launch-the-app"></a><span data-ttu-id="5a751-123">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="5a751-123">Launch the app</span></span>

<span data-ttu-id="5a751-124">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a751-124">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5a751-125">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:5000`。</span><span class="sxs-lookup"><span data-stu-id="5a751-125">Visual Studio launches a browser and navigates to `http://localhost:5000`.</span></span> <span data-ttu-id="5a751-126">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="5a751-126">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5a751-127">將 URL 變更為 `http://localhost:port/api/values`。</span><span class="sxs-lookup"><span data-stu-id="5a751-127">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5a751-128">將會顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="5a751-128">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

### <a name="add-support-for-entity-framework-core"></a><span data-ttu-id="5a751-129">新增 Entity Framework Core 的支援</span><span class="sxs-lookup"><span data-stu-id="5a751-129">Add support for Entity Framework Core</span></span>

<span data-ttu-id="5a751-130">安裝 [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) 資料庫提供者。</span><span class="sxs-lookup"><span data-stu-id="5a751-130">Install the [Entity Framework Core InMemory](https://docs.microsoft.com/ef/core/providers/in-memory/) database provider.</span></span> <span data-ttu-id="5a751-131">此資料庫提供者可讓 Entity Framework Core 搭配使用記憶體內部資料庫。</span><span class="sxs-lookup"><span data-stu-id="5a751-131">This database provider allows Entity Framework Core to be used with an in-memory database.</span></span>

* <span data-ttu-id="5a751-132">從 [專案] 功能表中，選取 [新增 NuGet 套件]。</span><span class="sxs-lookup"><span data-stu-id="5a751-132">From the **Project** menu, select **Add NuGet Packages**.</span></span> 

  *  <span data-ttu-id="5a751-133">或者，您可以用滑鼠右鍵按一下 [相依性]，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="5a751-133">Alternately, you can right-click **Dependencies**, and then select **Add Packages**.</span></span>

* <span data-ttu-id="5a751-134">在搜尋方塊中輸入 `EntityFrameworkCore.InMemory`。</span><span class="sxs-lookup"><span data-stu-id="5a751-134">Enter `EntityFrameworkCore.InMemory` in the search box.</span></span>
* <span data-ttu-id="5a751-135">選取 `Microsoft.EntityFrameworkCore.InMemory`，然後選取 [新增套件]。</span><span class="sxs-lookup"><span data-stu-id="5a751-135">Select `Microsoft.EntityFrameworkCore.InMemory`, and then select **Add Package**.</span></span>

### <a name="add-a-model-class"></a><span data-ttu-id="5a751-136">新增模型類別</span><span class="sxs-lookup"><span data-stu-id="5a751-136">Add a model class</span></span>

<span data-ttu-id="5a751-137">模型是代表應用程式中資料的物件。</span><span class="sxs-lookup"><span data-stu-id="5a751-137">A model is an object that represents the data in your application.</span></span> <span data-ttu-id="5a751-138">在此情況下，唯一的模型是待辦事項。</span><span class="sxs-lookup"><span data-stu-id="5a751-138">In this case, the only model is a to-do item.</span></span>

<span data-ttu-id="5a751-139">新增名為 *Models* 的資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a751-139">Add a folder named *Models*.</span></span> <span data-ttu-id="5a751-140">在方案總管中，以滑鼠右鍵按一下專案。</span><span class="sxs-lookup"><span data-stu-id="5a751-140">In Solution Explorer, right-click the project.</span></span> <span data-ttu-id="5a751-141">選取 [新增] > [新增資料夾]。</span><span class="sxs-lookup"><span data-stu-id="5a751-141">Select **Add** > **New Folder**.</span></span> <span data-ttu-id="5a751-142">將資料夾命名為 *Models*。</span><span class="sxs-lookup"><span data-stu-id="5a751-142">Name the folder *Models*.</span></span>

![新增資料夾](first-web-api-mac/_static/folder.png)

<span data-ttu-id="5a751-144">注意：您可以將模型類別放在專案中的任何位置，但依照慣例會使用 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a751-144">Note: You can put model classes anywhere in your project, but the *Models* folder is used by convention.</span></span>

<span data-ttu-id="5a751-145">新增 `TodoItem` 類別。</span><span class="sxs-lookup"><span data-stu-id="5a751-145">Add a `TodoItem` class.</span></span> <span data-ttu-id="5a751-146">以滑鼠右鍵按一下 *Models* 資料夾，然後選取 [新增] > [新增檔案] > [一般] > [空類別]。</span><span class="sxs-lookup"><span data-stu-id="5a751-146">Right-click the *Models* folder and select **Add > New File > General > Empty Class**.</span></span> <span data-ttu-id="5a751-147">將類別命名為 `TodoItem`，然後選取 [新增]。</span><span class="sxs-lookup"><span data-stu-id="5a751-147">Name the class `TodoItem`, and then select **New**.</span></span>

<span data-ttu-id="5a751-148">使用下列程式碼取代產生的程式碼：</span><span class="sxs-lookup"><span data-stu-id="5a751-148">Replace the generated code with:</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoItem.cs)]

<span data-ttu-id="5a751-149">此資料庫會在建立 `TodoItem` 時產生 `Id`。</span><span class="sxs-lookup"><span data-stu-id="5a751-149">The database generates the `Id` when a `TodoItem` is created.</span></span>

### <a name="create-the-database-context"></a><span data-ttu-id="5a751-150">建立資料庫內容</span><span class="sxs-lookup"><span data-stu-id="5a751-150">Create the database context</span></span>

<span data-ttu-id="5a751-151">「資料庫內容」是為指定的資料模型協調 Entity Framework 功能的主要類別。</span><span class="sxs-lookup"><span data-stu-id="5a751-151">The *database context* is the main class that coordinates Entity Framework functionality for a given data model.</span></span> <span data-ttu-id="5a751-152">您可以透過衍生自 `Microsoft.EntityFrameworkCore.DbContext` 類別來建立此類別。</span><span class="sxs-lookup"><span data-stu-id="5a751-152">You create this class by deriving from the `Microsoft.EntityFrameworkCore.DbContext` class.</span></span>

<span data-ttu-id="5a751-153">將 `TodoContext` 類別新增至 *Models* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="5a751-153">Add a `TodoContext` class to the *Models* folder.</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Models/TodoContext.cs)]

[!INCLUDE[Register the database context](../includes/webApi/register_dbContext.md)]

## <a name="add-a-controller"></a><span data-ttu-id="5a751-154">新增控制器</span><span class="sxs-lookup"><span data-stu-id="5a751-154">Add a controller</span></span>

<span data-ttu-id="5a751-155">在方案總管的 *Controllers* 資料夾中新增 `TodoController` 類別。</span><span class="sxs-lookup"><span data-stu-id="5a751-155">In Solution Explorer, in the *Controllers* folder, add the class `TodoController`.</span></span>

<span data-ttu-id="5a751-156">使用下列程式碼取代產生的程式碼 (並新增右大括弧)：</span><span class="sxs-lookup"><span data-stu-id="5a751-156">Replace the generated code with the following (and add closing braces):</span></span>

[!INCLUDE[code and get todo items](../includes/webApi/getTodoItems.md)]

### <a name="launch-the-app"></a><span data-ttu-id="5a751-157">啟動應用程式</span><span class="sxs-lookup"><span data-stu-id="5a751-157">Launch the app</span></span>

<span data-ttu-id="5a751-158">在 Visual Studio 中，選取 [執行] > [啟動並偵錯] 來啟動應用程式。</span><span class="sxs-lookup"><span data-stu-id="5a751-158">In Visual Studio, select **Run > Start With Debugging** to launch the app.</span></span> <span data-ttu-id="5a751-159">Visual Studio 會啟動瀏覽器並巡覽至 `http://localhost:port`，其中 *port* 是隨機選擇的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="5a751-159">Visual Studio launches a browser and navigates to `http://localhost:port`, where *port* is a randomly chosen port number.</span></span> <span data-ttu-id="5a751-160">您收到 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="5a751-160">You get an HTTP 404 (Not Found) error.</span></span>  <span data-ttu-id="5a751-161">將 URL 變更為 `http://localhost:port/api/values`。</span><span class="sxs-lookup"><span data-stu-id="5a751-161">Change the URL to `http://localhost:port/api/values`.</span></span> <span data-ttu-id="5a751-162">將會顯示 `ValuesController` 資料：</span><span class="sxs-lookup"><span data-stu-id="5a751-162">The `ValuesController` data will be displayed:</span></span>

```
["value1","value2"]
```

<span data-ttu-id="5a751-163">巡覽至位於 `http://localhost:port/api/todo` 的 `Todo` 控制器：</span><span class="sxs-lookup"><span data-stu-id="5a751-163">Navigate to the `Todo` controller at`http://localhost:port/api/todo`:</span></span>

```
[{"key":1,"name":"Item1","isComplete":false}]
```

## <a name="implement-the-other-crud-operations"></a><span data-ttu-id="5a751-164">實作其他的 CRUD 作業</span><span class="sxs-lookup"><span data-stu-id="5a751-164">Implement the other CRUD operations</span></span>

<span data-ttu-id="5a751-165">我們會將 `Create`、`Update` 和 `Delete` 方法新增至控制器。</span><span class="sxs-lookup"><span data-stu-id="5a751-165">We'll add `Create`, `Update`, and `Delete` methods to the controller.</span></span> <span data-ttu-id="5a751-166">這些是佈景主題的變化，因此我只會顯示程式碼，並強調顯示主要的差異。</span><span class="sxs-lookup"><span data-stu-id="5a751-166">These are variations on a theme, so I'll just show the code and highlight the main differences.</span></span> <span data-ttu-id="5a751-167">請在新增或變更程式碼之後建置專案。</span><span class="sxs-lookup"><span data-stu-id="5a751-167">Build the project after adding or changing code.</span></span>

### <a name="create"></a><span data-ttu-id="5a751-168">建立</span><span class="sxs-lookup"><span data-stu-id="5a751-168">Create</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Create)]

<span data-ttu-id="5a751-169">這是 HTTP POST 方法，以 [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) 屬性表示。</span><span class="sxs-lookup"><span data-stu-id="5a751-169">This is an HTTP POST method, indicated by the [`[HttpPost]`](/aspnet/core/api/microsoft.aspnetcore.mvc.httppostattribute) attribute.</span></span> <span data-ttu-id="5a751-170">[`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) 屬性會告知 MVC 從 HTTP 要求的主體取得待辦事項的值。</span><span class="sxs-lookup"><span data-stu-id="5a751-170">The [`[FromBody]`](/aspnet/core/api/microsoft.aspnetcore.mvc.frombodyattribute) attribute tells MVC to get the value of the to-do item from the body of the HTTP request.</span></span>

<span data-ttu-id="5a751-171">`CreatedAtRoute` 方法會傳回 201 回應，這是 HTTP POST 方法的標準回應，可在伺服器上建立新的資源。</span><span class="sxs-lookup"><span data-stu-id="5a751-171">The `CreatedAtRoute` method returns a 201 response, which is the standard response for an HTTP POST method that creates a new resource on the server.</span></span> <span data-ttu-id="5a751-172">`CreatedAtRoute` 也會將位置標頭新增至回應。</span><span class="sxs-lookup"><span data-stu-id="5a751-172">`CreatedAtRoute` also adds a Location header to the response.</span></span> <span data-ttu-id="5a751-173">位置標頭指定新建立之待辦事項的 URI。</span><span class="sxs-lookup"><span data-stu-id="5a751-173">The Location header specifies the URI of the newly created to-do item.</span></span> <span data-ttu-id="5a751-174">請參閱 [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html) (10.2.2 201 已建立)。</span><span class="sxs-lookup"><span data-stu-id="5a751-174">See [10.2.2 201 Created](http://www.w3.org/Protocols/rfc2616/rfc2616-sec10.html).</span></span>

### <a name="use-postman-to-send-a-create-request"></a><span data-ttu-id="5a751-175">使用 Postman 來傳送建立要求</span><span class="sxs-lookup"><span data-stu-id="5a751-175">Use Postman to send a Create request</span></span>

* <span data-ttu-id="5a751-176">啟動應用程式 ([執行] > [啟動並偵錯])。</span><span class="sxs-lookup"><span data-stu-id="5a751-176">Start the app (**Run > Start With Debugging**).</span></span>
* <span data-ttu-id="5a751-177">啟動 Postman。</span><span class="sxs-lookup"><span data-stu-id="5a751-177">Start Postman.</span></span>

![Postman 主控台](first-web-api/_static/pmc.png)

* <span data-ttu-id="5a751-179">將 HTTP 方法設定為 `POST`</span><span class="sxs-lookup"><span data-stu-id="5a751-179">Set the HTTP method to `POST`</span></span>
* <span data-ttu-id="5a751-180">選取 [主體] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="5a751-180">Select the **Body** radio button</span></span>
* <span data-ttu-id="5a751-181">選取 [原始] 選項按鈕</span><span class="sxs-lookup"><span data-stu-id="5a751-181">Select the **raw** radio button</span></span>
* <span data-ttu-id="5a751-182">將類型設定為 JSON</span><span class="sxs-lookup"><span data-stu-id="5a751-182">Set the type to JSON</span></span>
* <span data-ttu-id="5a751-183">在索引鍵/值編輯器中，輸入待辦事項，例如</span><span class="sxs-lookup"><span data-stu-id="5a751-183">In the key-value editor, enter a Todo item such as</span></span>

```json
{
    "name":"walk dog",
    "isComplete":true
}
```

* <span data-ttu-id="5a751-184">選取 [傳送]</span><span class="sxs-lookup"><span data-stu-id="5a751-184">Select **Send**</span></span>

* <span data-ttu-id="5a751-185">選取下方窗格中的 [標頭] 索引標籤，並複製 [位置] 標頭：</span><span class="sxs-lookup"><span data-stu-id="5a751-185">Select the Headers tab in the lower pane and copy the **Location** header:</span></span>

![Postman 主控台的 [標頭] 索引標籤](first-web-api/_static/pmget.png)

<span data-ttu-id="5a751-187">您可以使用 [位置] 標頭 URI 來存取您剛才建立的資源。</span><span class="sxs-lookup"><span data-stu-id="5a751-187">You can use the Location header URI to access the resource you just created.</span></span> <span data-ttu-id="5a751-188">請重新叫用建立 `"GetTodo"` 具名路由的 `GetById` 方法：</span><span class="sxs-lookup"><span data-stu-id="5a751-188">Recall the `GetById` method created the `"GetTodo"` named route:</span></span>

```csharp
[HttpGet("{id}", Name = "GetTodo")]
public IActionResult GetById(string id)
```

### <a name="update"></a><span data-ttu-id="5a751-189">更新</span><span class="sxs-lookup"><span data-stu-id="5a751-189">Update</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Update)]

<span data-ttu-id="5a751-190">`Update` 類似於 `Create`，但是會使用 HTTP PUT。</span><span class="sxs-lookup"><span data-stu-id="5a751-190">`Update` is similar to `Create`, but uses HTTP PUT.</span></span> <span data-ttu-id="5a751-191">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="5a751-191">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span> <span data-ttu-id="5a751-192">根據 HTTP 規格，PUT 要求需要用戶端傳送整個更新的實體，而不只是差異。</span><span class="sxs-lookup"><span data-stu-id="5a751-192">According to the HTTP spec, a PUT request requires the client to send the entire updated entity, not just the deltas.</span></span> <span data-ttu-id="5a751-193">若要支援部分更新，請使用 HTTP PATCH。</span><span class="sxs-lookup"><span data-stu-id="5a751-193">To support partial updates, use HTTP PATCH.</span></span>

```json
{
  "key": 1,
  "name": "walk dog",
  "isComplete": true
}
```

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmcput.png)

### <a name="delete"></a><span data-ttu-id="5a751-195">刪除</span><span class="sxs-lookup"><span data-stu-id="5a751-195">Delete</span></span>

[!code-csharp[Main](first-web-api/sample/TodoApi/Controllers/TodoController.cs?name=snippet_Delete)]

<span data-ttu-id="5a751-196">回應是 [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html) (204 (沒有內容))。</span><span class="sxs-lookup"><span data-stu-id="5a751-196">The response is [204 (No Content)](http://www.w3.org/Protocols/rfc2616/rfc2616-sec9.html).</span></span>

![顯示「204 (沒有內容) 回應」的 Postman 主控台](first-web-api/_static/pmd.png)

## <a name="next-steps"></a><span data-ttu-id="5a751-198">後續步驟</span><span class="sxs-lookup"><span data-stu-id="5a751-198">Next steps</span></span>

* [<span data-ttu-id="5a751-199">傳送至控制器動作</span><span class="sxs-lookup"><span data-stu-id="5a751-199">Routing to Controller Actions</span></span>](xref:mvc/controllers/routing)
* <span data-ttu-id="5a751-200">如需部署 API 的資訊，請參閱[裝載與部署](xref:host-and-deploy/index)。</span><span class="sxs-lookup"><span data-stu-id="5a751-200">For information about deploying your API, see [Host and deploy](xref:host-and-deploy/index).</span></span>
* <span data-ttu-id="5a751-201">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="5a751-201">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/tutorials/first-web-api/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>
* [<span data-ttu-id="5a751-202">Postman</span><span class="sxs-lookup"><span data-stu-id="5a751-202">Postman</span></span>](https://www.getpostman.com/)
* [<span data-ttu-id="5a751-203">Fiddler</span><span class="sxs-lookup"><span data-stu-id="5a751-203">Fiddler</span></span>](https://www.telerik.com/download/fiddler)
