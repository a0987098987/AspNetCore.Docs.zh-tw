---
title: 從 ASP.NET Web API 移轉至 ASP.NET Core
author: ardalis
description: 了解如何從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC。
manager: wpickett
ms.author: riande
ms.date: 05/10/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: migration/webapi
ms.openlocfilehash: 8d842877e49e317323d453e71ebb3302245f388d
ms.sourcegitcommit: 3d071fabaf90e32906df97b08a8d00e602db25c0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/10/2018
ms.locfileid: "34009068"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="11227-103">從 ASP.NET Web API 移轉至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="11227-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="11227-104">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="11227-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="11227-105">Web 應用程式開發介面為連線用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="11227-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="11227-106">ASP.NET Core MVC 包括建置 Web 應用程式開發介面提供單一且一致的方式，建立 web 應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="11227-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="11227-107">在本文中，我們會示範從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="11227-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="11227-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="11227-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="11227-109">檢閱 ASP.NET Web API 專案</span><span class="sxs-lookup"><span data-stu-id="11227-109">Review ASP.NET Web API Project</span></span>

<span data-ttu-id="11227-110">本文使用範例專案， *ProductsApp*建立發行項的[開始使用 ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)做為起點。</span><span class="sxs-lookup"><span data-stu-id="11227-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="11227-111">在該專案中，簡單的 ASP.NET Web API 專案設定，如下所示。</span><span class="sxs-lookup"><span data-stu-id="11227-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="11227-112">在*Global.asax.cs*，進行呼叫以`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="11227-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="11227-113">`WebApiConfig` 定義於*App_Start*，且具有一個靜態`Register`方法：</span><span class="sxs-lookup"><span data-stu-id="11227-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


<span data-ttu-id="11227-114">這個類別會設定[屬性路由](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上不會在專案中使用。</span><span class="sxs-lookup"><span data-stu-id="11227-114">This class configures [attribute routing](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="11227-115">它也會設定路由表，由 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="11227-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="11227-116">在此情況下，ASP.NET Web 應用程式開發介面會預期以符合格式的 Url */api/ {controller} / {id}*，與 *{id}* 為選擇性。</span><span class="sxs-lookup"><span data-stu-id="11227-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="11227-117">*ProductsApp*專案包含一個簡單的控制器，繼承自`ApiController`和公開兩個方法：</span><span class="sxs-lookup"><span data-stu-id="11227-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="11227-118">最後，模型*產品*中，由*ProductsApp*，是簡單的類別：</span><span class="sxs-lookup"><span data-stu-id="11227-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="11227-119">現在，我們有簡單的專案開始，我們可以示範如何將這個 Web API 專案移轉至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="11227-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="11227-120">建立目的專案</span><span class="sxs-lookup"><span data-stu-id="11227-120">Create the Destination Project</span></span>

<span data-ttu-id="11227-121">使用 Visual Studio，建立新的空白方案，並將它命名*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="11227-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="11227-122">加入現有*ProductsApp*專案，然後將新的 ASP.NET Core Web 應用程式專案加入方案。</span><span class="sxs-lookup"><span data-stu-id="11227-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="11227-123">將新專案*ProductsCore*。</span><span class="sxs-lookup"><span data-stu-id="11227-123">Name the new project *ProductsCore*.</span></span>

![網站範本來開啟 [新增專案] 對話方塊](webapi/_static/add-web-project.png)

<span data-ttu-id="11227-125">接下來，選擇 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="11227-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="11227-126">我們將會移轉*ProductsApp*內容到這個新的專案。</span><span class="sxs-lookup"><span data-stu-id="11227-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![ASP.NET Core 範本清單中選取的 Web API 的專案範本的新 Web 應用程式對話方塊](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="11227-128">刪除`Project_Readme.html`檔案從新的專案。</span><span class="sxs-lookup"><span data-stu-id="11227-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="11227-129">您的方案現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="11227-129">Your solution should now look like this:</span></span>

![在 [方案總管] 中顯示檔案和資料夾的 ProductsApp 和 ProductsCore 專案中開啟應用程式方案](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="11227-131">移轉設定</span><span class="sxs-lookup"><span data-stu-id="11227-131">Migrate Configuration</span></span>

<span data-ttu-id="11227-132">不會再使用 ASP.NET Core *Global.asax*， *web.config*，或*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="11227-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="11227-133">相反地，完成所有啟動工作*Startup.cs*專案的根目錄中 (請參閱[應用程式啟動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="11227-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="11227-134">在 ASP.NET Core MVC 中，屬性為基礎的路由現在會包含預設時`UseMvc()`稱為; 而且，這是建議的方法來設定 Web API 路由 （而且是 Web API 入門專案處理路由的方式）。</span><span class="sxs-lookup"><span data-stu-id="11227-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="11227-135">假設您想要使用屬性路由從現在開始在專案中，不需要進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="11227-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="11227-136">只需套用的屬性視您的控制器和動作，如同您在此範例`ValuesController`Web API 的入門專案中包含的類別：</span><span class="sxs-lookup"><span data-stu-id="11227-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="11227-137">請注意是否存在 *[控制器]* 第 8 行上。</span><span class="sxs-lookup"><span data-stu-id="11227-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="11227-138">屬性為基礎的路由現在支援特定語彙基元，例如 *[控制器]* 和 *[動作]*。</span><span class="sxs-lookup"><span data-stu-id="11227-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="11227-139">這些語彙基元會取代在執行階段的控制器或動作，名稱分別屬性套用。</span><span class="sxs-lookup"><span data-stu-id="11227-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="11227-140">這是用來減少的識別字串，在專案中，並且確保路由將會保留其對應的控制器和動作與同步處理時自動重新命名重構功能，就會套用。</span><span class="sxs-lookup"><span data-stu-id="11227-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="11227-141">若要移轉的產品的 API 控制器，我們必須先將複製*ProductsController*到新的專案。</span><span class="sxs-lookup"><span data-stu-id="11227-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="11227-142">然後只需要包含在控制器上的路由屬性：</span><span class="sxs-lookup"><span data-stu-id="11227-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="11227-143">您也需要加入`[HttpGet]`屬性設定為兩種方法，因為它們都應該會透過 HTTP Get 呼叫。</span><span class="sxs-lookup"><span data-stu-id="11227-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="11227-144">針對屬性中包含的 「 識別碼 」 參數的預期`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="11227-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="11227-145">此時，路由已正確設定。不過，我們無法尚未測試它。</span><span class="sxs-lookup"><span data-stu-id="11227-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="11227-146">必須先進行其他變更*ProductsController*會進行編譯。</span><span class="sxs-lookup"><span data-stu-id="11227-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="11227-147">移轉模型和控制站</span><span class="sxs-lookup"><span data-stu-id="11227-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="11227-148">這個簡單的 Web API 專案的移轉程序的最後一個步驟是複製到控制器，他們使用的任何模型。</span><span class="sxs-lookup"><span data-stu-id="11227-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="11227-149">在此情況下，只需複製*Controllers/ProductsController.cs*從原始專案到新的專案。</span><span class="sxs-lookup"><span data-stu-id="11227-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="11227-150">然後，將整個模型資料夾從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="11227-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="11227-151">調整以符合新的專案名稱的命名空間 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="11227-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="11227-152">此時，您可以建置應用程式，而您會發現多個編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="11227-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="11227-153">這些通常應該可分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="11227-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="11227-154">*ApiController*不存在</span><span class="sxs-lookup"><span data-stu-id="11227-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="11227-155">*System.Web.Http*命名空間不存在</span><span class="sxs-lookup"><span data-stu-id="11227-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="11227-156">*IHttpActionResult*不存在</span><span class="sxs-lookup"><span data-stu-id="11227-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="11227-157">幸運的是，這些是所有很容易修正：</span><span class="sxs-lookup"><span data-stu-id="11227-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="11227-158">變更*ApiController*至*控制器*(您可能需要加入*使用 Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="11227-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="11227-159">刪除任何使用陳述式參考*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="11227-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="11227-160">變更任何方法傳回*IHttpActionResult*傳回*IActionResult*</span><span class="sxs-lookup"><span data-stu-id="11227-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="11227-161">一旦這些變更都已和未使用 using 陳述式移除，移轉*ProductsController*類別看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="11227-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="11227-162">您現在應該能夠執行移轉的專案，並瀏覽至 */api/產品*; 而且，您應該會看到 3 產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="11227-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="11227-163">瀏覽至 */api/products/1*和您應該會看到第一次的產品。</span><span class="sxs-lookup"><span data-stu-id="11227-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="microsoftaspnetcoremvcwebapicompatshim"></a><span data-ttu-id="11227-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span><span class="sxs-lookup"><span data-stu-id="11227-164">Microsoft.AspNetCore.Mvc.WebApiCompatShim</span></span>

<span data-ttu-id="11227-165">移轉 ASP.NET Web API 專案到 ASP.NET Core 時相當有用的工具是[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫。</span><span class="sxs-lookup"><span data-stu-id="11227-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="11227-166">相容性填充碼延伸到允許的數量要使用不同的 Web API 2 慣例 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="11227-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="11227-167">基本相容性填充碼不需要移植先前在此文件中的範例。</span><span class="sxs-lookup"><span data-stu-id="11227-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="11227-168">針對大型專案，在使用相容性填充碼可用於暫時的溝通落差 API ASP.NET Core 與 ASP.NET Web API 2 之間。</span><span class="sxs-lookup"><span data-stu-id="11227-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="11227-169">Web 應用程式開發介面相容性填充碼是用來做為暫時的措施用於簡化大型 Web API 專案移轉至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="11227-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="11227-170">經過一段時間，您應該更新專案，以使用 ASP.NET Core 模式，而不是依賴相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="11227-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span> 

<span data-ttu-id="11227-171">Microsoft.AspNetCore.Mvc.WebApiCompatShim 中包含的相容性功能包括：</span><span class="sxs-lookup"><span data-stu-id="11227-171">Compatibility features included in Microsoft.AspNetCore.Mvc.WebApiCompatShim include:</span></span>

* <span data-ttu-id="11227-172">新增`ApiController`型別，如此控制站的基底型別不需要更新。</span><span class="sxs-lookup"><span data-stu-id="11227-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="11227-173">可讓 Web 應用程式開發介面樣式模型繫結。</span><span class="sxs-lookup"><span data-stu-id="11227-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="11227-174">ASP.NET Core MVC 模型繫結的功能類似於預設的 MVC 5。</span><span class="sxs-lookup"><span data-stu-id="11227-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="11227-175">相容性填充碼變更的模型繫結才能比較類似於 Web API 2 模型繫結慣例。</span><span class="sxs-lookup"><span data-stu-id="11227-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="11227-176">例如，複雜型別會自動繫結從要求主體中。</span><span class="sxs-lookup"><span data-stu-id="11227-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="11227-177">延伸模型繫結，讓控制器的動作可能需要型別參數的`HttpRequestMessage`。</span><span class="sxs-lookup"><span data-stu-id="11227-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="11227-178">新增訊息格式器允許動作，以傳回結果的型別`HttpResponseMessage`。</span><span class="sxs-lookup"><span data-stu-id="11227-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="11227-179">新增其他回應 Web API 2 動作可能已用來處理回應的方法：</span><span class="sxs-lookup"><span data-stu-id="11227-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
    * <span data-ttu-id="11227-180">HttpResponseMessage 產生器：</span><span class="sxs-lookup"><span data-stu-id="11227-180">HttpResponseMessage generators:</span></span>
        * `CreateResponse<T>`
        * `CreateErrorResponse`
    * <span data-ttu-id="11227-181">動作結果的方法：</span><span class="sxs-lookup"><span data-stu-id="11227-181">Action result methods:</span></span>
        * `BadResuestErrorMessageResult`
        * `ExceptionResult`
        * `InternalServerErrorResult`
        * `InvalidModelStateResult`
        * `NegotiatedContentResult`
        * `ResponseMessageResult`
* <span data-ttu-id="11227-182">將執行個體加入`IContentNegotiator`應用程式的 DI 容器和建立內容交涉相關的類型，從[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)可用。</span><span class="sxs-lookup"><span data-stu-id="11227-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="11227-183">這包括類類型`DefaultContentNegotiator`，`MediaTypeFormatter`等等。</span><span class="sxs-lookup"><span data-stu-id="11227-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="11227-184">若要使用相容性填充碼，您需要：</span><span class="sxs-lookup"><span data-stu-id="11227-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="11227-185">參考[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="11227-185">Reference the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="11227-186">向應用程式的 DI 容器中的相容性填充碼的服務，藉由呼叫`services.AddWebApiConventions()`在應用程式的`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="11227-186">Register the compatibility shim's services with the app's DI container by calling `services.AddWebApiConventions()` in the application's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="11227-187">定義使用的 Web 應用程式開發介面特定路由`MapWebApiRoute`上`IRouteBuilder`在應用程式的`IApplicationBuilder.UseMvc`呼叫。</span><span class="sxs-lookup"><span data-stu-id="11227-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the application's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="11227-188">總結</span><span class="sxs-lookup"><span data-stu-id="11227-188">Summary</span></span>

<span data-ttu-id="11227-189">將簡單的 ASP.NET Web API 專案移轉至 ASP.NET Core MVC 就相當簡單，這適用於 ASP.NET Core MVC 中 Web Api 的內建支援。</span><span class="sxs-lookup"><span data-stu-id="11227-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="11227-190">每個 ASP.NET Web API 專案必須移轉的主要部分是路由、 控制器、 和模型，以及更新至控制器和動作所使用的類型。</span><span class="sxs-lookup"><span data-stu-id="11227-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
