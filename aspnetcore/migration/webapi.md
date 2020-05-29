---
<span data-ttu-id="a2cd7-101">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-101">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="a2cd7-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="a2cd7-102">'Blazor'</span></span>
- <span data-ttu-id="a2cd7-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="a2cd7-103">'Identity'</span></span>
- <span data-ttu-id="a2cd7-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="a2cd7-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="a2cd7-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="a2cd7-105">'Razor'</span></span>
- <span data-ttu-id="a2cd7-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-106">'SignalR' uid:</span></span> 

---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="a2cd7-107">從 ASP.NET Web API 遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="a2cd7-107">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="a2cd7-108">作者： [Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="a2cd7-108">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="a2cd7-109">ASP.NET 4.x Web API 是一種 HTTP 服務，可觸及各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-109">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="a2cd7-110">ASP.NET Core 將 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型結合成單一程式設計模型，稱為 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-110">ASP.NET Core combines ASP.NET 4.x's MVC and Web API app models into a single programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="a2cd7-111">本文示範從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-111">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="a2cd7-112">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="a2cd7-112">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a><span data-ttu-id="a2cd7-113">必要條件</span><span class="sxs-lookup"><span data-stu-id="a2cd7-113">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs-3.1.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="a2cd7-114">審查 ASP.NET 4.x Web API 專案</span><span class="sxs-lookup"><span data-stu-id="a2cd7-114">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="a2cd7-115">本文使用[ASP.NET Web API 2 消費者入門](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)中建立的*ProductsApp*專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-115">This article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="a2cd7-116">在該專案中，會依照下列方式設定基本的 ASP.NET 4.x Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-116">In that project, a basic ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="a2cd7-117">在*Global.asax.cs*中，會對進行呼叫 `WebApiConfig.Register` ：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-117">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/3.x/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="a2cd7-118">在 `WebApiConfig` *App_Start*資料夾中找到類別，並具有靜態 `Register` 方法：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-118">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/3.x/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="a2cd7-119">上述類別：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-119">The preceding class:</span></span>

* <span data-ttu-id="a2cd7-120">會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不會使用它。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-120">Configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used.</span></span>
* <span data-ttu-id="a2cd7-121">設定路由表。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-121">Configures the routing table.</span></span>
<span data-ttu-id="a2cd7-122">範例程式碼需要 Url 以符合格式 `/api/{controller}/{id}` ，而且 `{id}` 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-122">The sample code expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="a2cd7-123">下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-123">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="a2cd7-124">建立目的地專案</span><span class="sxs-lookup"><span data-stu-id="a2cd7-124">Create the destination project</span></span>

<span data-ttu-id="a2cd7-125">在 Visual Studio 中建立新的空白方案，並新增 ASP.NET 4.x Web API 專案以進行遷移：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-125">Create a new blank solution in Visual Studio and add the ASP.NET 4.x Web API project to migrate:</span></span>

1. <span data-ttu-id="a2cd7-126">從 [檔案]\*\*\*\* 功能表選取 [新增]**[專案]** > \*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-126">From the **File** menu, select **New** > **Project**.</span></span>
1. <span data-ttu-id="a2cd7-127">選取 [**空白解決方案**] 範本，然後選取 **[下一步]**。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-127">Select the **Blank Solution** template and select **Next**.</span></span>
1. <span data-ttu-id="a2cd7-128">將方案命名為*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-128">Name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="a2cd7-129">選取 [建立]。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-129">Select **Create**.</span></span>
1. <span data-ttu-id="a2cd7-130">將現有的*ProductsApp*專案新增至方案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-130">Add the existing *ProductsApp* project to the solution.</span></span>

<span data-ttu-id="a2cd7-131">新增要遷移至的新 API 專案：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-131">Add a new API project to migrate to:</span></span>

1. <span data-ttu-id="a2cd7-132">將新的**ASP.NET Core Web 應用程式**專案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-132">Add a new **ASP.NET Core Web Application** project to the solution.</span></span>
1. <span data-ttu-id="a2cd7-133">在 [**設定您的新專案**] 對話方塊中，將專案命名為*ProductsCore*，然後選取 [**建立**]。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-133">In the **Configure your new project** dialog, Name the project *ProductsCore*, and select **Create**.</span></span>
1. <span data-ttu-id="a2cd7-134">在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.1** ]。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-134">In the **Create a new ASP.NET Core Web Application** dialog, confirm that **.NET Core** and **ASP.NET Core 3.1** are selected.</span></span> <span data-ttu-id="a2cd7-135">選取 [API]\*\*\*\* 專案範本，然後選取 [確定]\*\*\*\*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-135">Select the **API** project template, and select **Create**.</span></span>
1. <span data-ttu-id="a2cd7-136">從新的*ProductsCore*專案中移除*WeatherForecast.cs*和 controller */WeatherForecastController*範例檔案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-136">Remove the *WeatherForecast.cs* and *Controllers/WeatherForecastController.cs* example files from the new *ProductsCore* project.</span></span>

<span data-ttu-id="a2cd7-137">方案現在包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-137">The solution now contains two projects.</span></span> <span data-ttu-id="a2cd7-138">下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-138">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="a2cd7-139">移轉組態</span><span class="sxs-lookup"><span data-stu-id="a2cd7-139">Migrate configuration</span></span>

<span data-ttu-id="a2cd7-140">ASP.NET Core 不會使用*App_Start*資料夾或*global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-140">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file.</span></span> <span data-ttu-id="a2cd7-141">此外，也會在發行時新增*web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-141">Additionally, the *web.config* file is added at publish time.</span></span>

<span data-ttu-id="a2cd7-142">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-142">The `Startup` class:</span></span>

* <span data-ttu-id="a2cd7-143">取代*global.asax*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-143">Replaces *Global.asax*.</span></span>
* <span data-ttu-id="a2cd7-144">處理所有應用程式啟動工作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-144">Handles all app startup tasks.</span></span>

<span data-ttu-id="a2cd7-145">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-145">For more information, see <xref:fundamentals/startup>.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="a2cd7-146">遷移模型和控制器</span><span class="sxs-lookup"><span data-stu-id="a2cd7-146">Migrate models and controllers</span></span>

<span data-ttu-id="a2cd7-147">下列程式碼顯示 `ProductsController` 要更新 ASP.NET Core 的：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-147">The following code shows the `ProductsController` to be updated for ASP.NET Core:</span></span>

[!code-csharp[](webapi/sample/3.x/ProductsApp/Controllers/ProductsController.cs)]

<span data-ttu-id="a2cd7-148">更新 `ProductsController` ASP.NET Core 的：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-148">Update the `ProductsController` for ASP.NET Core:</span></span>

1. <span data-ttu-id="a2cd7-149">將*控制器/productscontroller.cs*和 [*模型*] 資料夾從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-149">Copy *Controllers/ProductsController.cs* and the *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="a2cd7-150">將複製的檔案根命名空間變更為 `ProductsCore` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-150">Change the copied files' root namespace to `ProductsCore`.</span></span>
1. <span data-ttu-id="a2cd7-151">將 `using ProductsApp.Models;` 語句更新為 `using ProductsCore.Models;` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-151">Update the `using ProductsApp.Models;` statement to `using ProductsCore.Models;`.</span></span>

<span data-ttu-id="a2cd7-152">ASP.NET Core 中不存在下列元件：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-152">The following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="a2cd7-153">`ApiController` 類別</span><span class="sxs-lookup"><span data-stu-id="a2cd7-153">`ApiController` class</span></span>
* <span data-ttu-id="a2cd7-154">`System.Web.Http` 命名空間</span><span class="sxs-lookup"><span data-stu-id="a2cd7-154">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="a2cd7-155">`IHttpActionResult` 介面</span><span class="sxs-lookup"><span data-stu-id="a2cd7-155">`IHttpActionResult` interface</span></span>

<span data-ttu-id="a2cd7-156">進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-156">Make the following changes:</span></span>

1. <span data-ttu-id="a2cd7-157">將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-157">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="a2cd7-158">新增 `using Microsoft.AspNetCore.Mvc;` 以解析 `ControllerBase` 參考。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-158">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="a2cd7-159">刪除 `using System.Web.Http;`。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-159">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="a2cd7-160">將 `GetProduct` 動作的傳回類型從變更 `IHttpActionResult` 為 `ActionResult<Product>` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-160">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>
1. <span data-ttu-id="a2cd7-161">將 `GetProduct` 動作的 `return` 語句簡化為下列內容：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-161">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

    ```csharp
    return product;
    ```

## <a name="configure-routing"></a><span data-ttu-id="a2cd7-162">設定路由</span><span class="sxs-lookup"><span data-stu-id="a2cd7-162">Configure routing</span></span>

<span data-ttu-id="a2cd7-163">ASP.NET Core *API*專案範本會在產生的程式碼中包含端點路由設定。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-163">The ASP.NET Core *API* project template includes endpoint routing configuration in the generated code.</span></span>

<span data-ttu-id="a2cd7-164">下列 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> 和會 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> 呼叫：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-164">The following <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> and <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> calls:</span></span>

* <span data-ttu-id="a2cd7-165">在[中介軟體](xref:fundamentals/middleware/index)管線中註冊路由對應和端點執行。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-165">Register route matching and endpoint execution in the [middleware](xref:fundamentals/middleware/index) pipeline.</span></span>
* <span data-ttu-id="a2cd7-166">取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-166">Replace the *ProductsApp* project's *App_Start/WebApiConfig.cs* file.</span></span>

[!code-csharp[](webapi/sample/3.x/ProductsCore/Startup.cs?name=snippet_Configure&highlight=10,14)]

<span data-ttu-id="a2cd7-167">設定路由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-167">Configure routing as follows:</span></span>

1. <span data-ttu-id="a2cd7-168">標記 `ProductsController` 具有下列屬性的類別：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-168">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="a2cd7-169">上述屬性會設定 [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) 控制器的屬性路由模式。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-169">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="a2cd7-170">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-170">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="a2cd7-171">屬性路由支援權杖，例如 `[controller]` 和 `[action]` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-171">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="a2cd7-172">在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-172">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="a2cd7-173">權杖：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-173">The tokens:</span></span>
    * <span data-ttu-id="a2cd7-174">減少專案中的魔術字串數目。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-174">Reduce the number of magic strings in the project.</span></span>
    * <span data-ttu-id="a2cd7-175">當套用自動重新命名重構時，請確定路由與對應的控制器和動作保持同步。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-175">Ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="a2cd7-176">對動作啟用 HTTP Get 要求 `ProductController` ：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-176">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="a2cd7-177">將 [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) 屬性套用至 `GetAllProducts` 動作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-177">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="a2cd7-178">將 `[HttpGet("{id}")]` 屬性套用至 `GetProduct` 動作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-178">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="a2cd7-179">執行已遷移的專案，並流覽至 `/api/products` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-179">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="a2cd7-180">隨即會顯示三個產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-180">A full list of three products appears.</span></span> <span data-ttu-id="a2cd7-181">瀏覽至 `/api/products/1`。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-181">Browse to `/api/products/1`.</span></span> <span data-ttu-id="a2cd7-182">第一個產品隨即出現。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-182">The first product appears.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2cd7-183">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2cd7-183">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
## <a name="prerequisites"></a><span data-ttu-id="a2cd7-184">必要條件</span><span class="sxs-lookup"><span data-stu-id="a2cd7-184">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="a2cd7-185">審查 ASP.NET 4.x Web API 專案</span><span class="sxs-lookup"><span data-stu-id="a2cd7-185">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="a2cd7-186">本文使用[ASP.NET Web API 2 消費者入門](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)中建立的*ProductsApp*專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-186">This article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="a2cd7-187">在該專案中，會依照下列方式設定基本的 ASP.NET 4.x Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-187">In that project, a basic ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="a2cd7-188">在*Global.asax.cs*中，會對進行呼叫 `WebApiConfig.Register` ：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-188">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/2.x/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="a2cd7-189">在 `WebApiConfig` *App_Start*資料夾中找到類別，並具有靜態 `Register` 方法：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-189">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/2.x/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="a2cd7-190">這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，雖然它實際上並不會在專案中使用。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-190">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="a2cd7-191">它也會設定 ASP.NET Web API 使用的路由表。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-191">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="a2cd7-192">在此情況下，ASP.NET 4.x Web API 會預期 Url 符合格式 `/api/{controller}/{id}` ，而且 `{id}` 是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-192">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="a2cd7-193">下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-193">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="a2cd7-194">建立目的地專案</span><span class="sxs-lookup"><span data-stu-id="a2cd7-194">Create the destination project</span></span>

<span data-ttu-id="a2cd7-195">在 Visual Studio 中，完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-195">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="a2cd7-196">移至 **[** 檔案] [  >  **新增**  >  **專案**] [  >  **其他專案類型**]  >  **Visual Studio 方案**]。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-196">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="a2cd7-197">選取 [**空白方案**]，並將方案命名為*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-197">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="a2cd7-198">按一下 [確定]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-198">Click the **OK** button.</span></span>
* <span data-ttu-id="a2cd7-199">將現有的*ProductsApp*專案新增至方案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-199">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="a2cd7-200">將新的**ASP.NET Core Web 應用程式**專案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-200">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="a2cd7-201">從下拉式選單選取 [ **.Net Core**目標 framework]，然後選取 [ **API** ] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-201">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="a2cd7-202">將專案命名為*ProductsCore*，然後按一下 [**確定]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-202">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="a2cd7-203">方案現在包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-203">The solution now contains two projects.</span></span> <span data-ttu-id="a2cd7-204">下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-204">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="a2cd7-205">移轉組態</span><span class="sxs-lookup"><span data-stu-id="a2cd7-205">Migrate configuration</span></span>

<span data-ttu-id="a2cd7-206">ASP.NET Core 不會使用：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-206">ASP.NET Core doesn't use:</span></span>

* <span data-ttu-id="a2cd7-207">*App_Start*資料夾或*global.asax*檔案</span><span class="sxs-lookup"><span data-stu-id="a2cd7-207">*App_Start* folder or the *Global.asax* file</span></span>
* <span data-ttu-id="a2cd7-208">*web.config 檔案*會在發行時加入。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-208">*web.config* file is added at publish time.</span></span>

<span data-ttu-id="a2cd7-209">`Startup` 類別：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-209">The `Startup` class:</span></span>

* <span data-ttu-id="a2cd7-210">取代*global.asax*。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-210">Replaces *Global.asax*.</span></span>
* <span data-ttu-id="a2cd7-211">處理所有應用程式啟動工作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-211">Handles all app startup tasks.</span></span>

<span data-ttu-id="a2cd7-212">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-212">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="a2cd7-213">在 ASP.NET Core MVC 中，在中呼叫時，預設會包含屬性路由 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> `Startup.Configure` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-213">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="a2cd7-214">下列 `UseMvc` 呼叫會取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-214">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/2.x/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13)]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="a2cd7-215">遷移模型和控制器</span><span class="sxs-lookup"><span data-stu-id="a2cd7-215">Migrate models and controllers</span></span>

<span data-ttu-id="a2cd7-216">下列程式碼顯示 `ProductsController` ASP.NET Core 的更新：[!code-csharp[](webapi/sample/2.x/ProductsApp/Controllers/ProductsController.cs)]</span><span class="sxs-lookup"><span data-stu-id="a2cd7-216">The following code shows the `ProductsController` update for ASP.NET Core: [!code-csharp[](webapi/sample/2.x/ProductsApp/Controllers/ProductsController.cs)]</span></span>

<span data-ttu-id="a2cd7-217">更新 `ProductsController` ASP.NET Core 的：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-217">Update the `ProductsController` for ASP.NET Core:</span></span>

1. <span data-ttu-id="a2cd7-218">將*控制器/productscontroller.cs*從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-218">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="a2cd7-219">將 [*模型*] 資料夾從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-219">Copy the *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="a2cd7-220">將複製的檔案根命名空間變更為 `ProductsCore` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-220">Change the copied files' root namespace to `ProductsCore`.</span></span>
1. <span data-ttu-id="a2cd7-221">將 `using ProductsApp.Models;` 語句更新為 `using ProductsCore.Models;` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-221">Update the `using ProductsApp.Models;` statement to `using ProductsCore.Models;`.</span></span>

<span data-ttu-id="a2cd7-222">ASP.NET Core 中不存在下列元件：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-222">The following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="a2cd7-223">`ApiController` 類別</span><span class="sxs-lookup"><span data-stu-id="a2cd7-223">`ApiController` class</span></span>
* <span data-ttu-id="a2cd7-224">`System.Web.Http` 命名空間</span><span class="sxs-lookup"><span data-stu-id="a2cd7-224">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="a2cd7-225">`IHttpActionResult` 介面</span><span class="sxs-lookup"><span data-stu-id="a2cd7-225">`IHttpActionResult` interface</span></span>

<span data-ttu-id="a2cd7-226">進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-226">Make the following changes:</span></span>

1. <span data-ttu-id="a2cd7-227">將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-227">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="a2cd7-228">新增 `using Microsoft.AspNetCore.Mvc;` 以解析 `ControllerBase` 參考。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-228">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="a2cd7-229">刪除 `using System.Web.Http;`。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-229">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="a2cd7-230">將 `GetProduct` 動作的傳回類型從變更 `IHttpActionResult` 為 `ActionResult<Product>` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-230">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>
1. <span data-ttu-id="a2cd7-231">將 `GetProduct` 動作的 `return` 語句簡化為下列內容：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-231">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

    ```csharp
    return product;
    ```

## <a name="configure-routing"></a><span data-ttu-id="a2cd7-232">設定路由</span><span class="sxs-lookup"><span data-stu-id="a2cd7-232">Configure routing</span></span>

<span data-ttu-id="a2cd7-233">設定路由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-233">Configure routing as follows:</span></span>

1. <span data-ttu-id="a2cd7-234">標記 `ProductsController` 具有下列屬性的類別：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-234">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="a2cd7-235">上述屬性會設定 [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) 控制器的屬性路由模式。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-235">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="a2cd7-236">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-236">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="a2cd7-237">屬性路由支援權杖，例如 `[controller]` 和 `[action]` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-237">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="a2cd7-238">在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-238">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="a2cd7-239">權杖會減少專案中的魔術字串數目。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-239">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="a2cd7-240">當套用自動重新命名重構時，權杖也會確保路由與對應的控制器和動作保持同步。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-240">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="a2cd7-241">將專案的相容性模式設定為 ASP.NET Core 2.2：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-241">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/2.x/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="a2cd7-242">先前的變更：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-242">The preceding change:</span></span>

    * <span data-ttu-id="a2cd7-243">需要使用 `[ApiController]` 控制器層級的屬性。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-243">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="a2cd7-244">加入宣告 ASP.NET Core 2.2 中引進的可能中斷行為。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-244">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="a2cd7-245">對動作啟用 HTTP Get 要求 `ProductController` ：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-245">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="a2cd7-246">將 [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) 屬性套用至 `GetAllProducts` 動作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-246">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="a2cd7-247">將 `[HttpGet("{id}")]` 屬性套用至 `GetProduct` 動作。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-247">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="a2cd7-248">執行已遷移的專案，並流覽至 `/api/products` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-248">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="a2cd7-249">隨即會顯示三個產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-249">A full list of three products appears.</span></span> <span data-ttu-id="a2cd7-250">瀏覽至 `/api/products/1`。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-250">Browse to `/api/products/1`.</span></span> <span data-ttu-id="a2cd7-251">第一個產品隨即出現。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-251">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="a2cd7-252">相容性填充碼</span><span class="sxs-lookup"><span data-stu-id="a2cd7-252">Compatibility shim</span></span>

<span data-ttu-id="a2cd7-253">[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫會提供相容性填充碼，以將 ASP.NET 4.X Web API 專案移至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-253">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="a2cd7-254">相容性填充碼會擴充 ASP.NET Core，以支援 ASP.NET 4.x Web API 2 中的一些慣例。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-254">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="a2cd7-255">先前在本檔中移植的範例，其基本程度足以使相容性填充碼不必要。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-255">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="a2cd7-256">對於較大的專案，使用相容性填充碼有助於暫時橋接 ASP.NET Core 和 ASP.NET 4.x Web API 2 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-256">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="a2cd7-257">Web API 相容性填充碼是用來做為暫時的量值，以支援將大型 ASP.NET 4.x Web API 專案遷移至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-257">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="a2cd7-258">經過一段時間，專案應該更新為使用 ASP.NET Core 模式，而不是依賴相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-258">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="a2cd7-259">包含的相容性功能 `Microsoft.AspNetCore.Mvc.WebApiCompatShim` 包括：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-259">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="a2cd7-260">加入 `ApiController` 類型，讓控制器的基底類型不需要更新。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-260">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="a2cd7-261">啟用 Web API 樣式的模型系結。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-261">Enables Web API-style model binding.</span></span> <span data-ttu-id="a2cd7-262">根據預設，ASP.NET Core MVC 模型系結函式類似于 ASP.NET 4.x MVC 5 的功能。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-262">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="a2cd7-263">相容性填充碼會將模型系結變更為類似于 ASP.NET 4.x Web API 2 模型系結慣例。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-263">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="a2cd7-264">例如，複雜型別會自動系結至要求主體。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-264">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="a2cd7-265">擴充模型系結，讓控制器動作可以接受類型的參數 `HttpRequestMessage` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-265">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="a2cd7-266">加入訊息格式器，允許動作傳回類型的結果 `HttpResponseMessage` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-266">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="a2cd7-267">新增 Web API 2 動作可能用來服務回應的其他回應方法：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-267">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="a2cd7-268">`HttpResponseMessage`產生器</span><span class="sxs-lookup"><span data-stu-id="a2cd7-268">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="a2cd7-269">動作結果方法：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-269">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="a2cd7-270">將的實例新增 `IContentNegotiator` 至應用程式的相依性插入（DI）容器，並提供來自[WebApi](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)的內容協商相關類型。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-270">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="a2cd7-271">這類類型的範例包括 `DefaultContentNegotiator` 和 `MediaTypeFormatter` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-271">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="a2cd7-272">若要使用相容性填充碼：</span><span class="sxs-lookup"><span data-stu-id="a2cd7-272">To use the compatibility shim:</span></span>

1. <span data-ttu-id="a2cd7-273">安裝[AspNetCore WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-273">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="a2cd7-274">在中呼叫，以向應用程式的 DI 容器註冊相容性填充碼的服務 `services.AddMvc().AddWebApiConventions()` `Startup.ConfigureServices` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-274">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="a2cd7-275">`MapWebApiRoute` `IRouteBuilder` 在應用程式的呼叫中，使用來定義 Web API 特定的路由 `IApplicationBuilder.UseMvc` 。</span><span class="sxs-lookup"><span data-stu-id="a2cd7-275">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="a2cd7-276">其他資源</span><span class="sxs-lookup"><span data-stu-id="a2cd7-276">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
::: moniker-end
