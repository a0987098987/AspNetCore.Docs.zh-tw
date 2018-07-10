---
title: 從 ASP.NET Web API 移轉至 ASP.NET Core
author: ardalis
description: 了解如何從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC。
ms.author: riande
ms.date: 05/10/2018
uid: migration/webapi
ms.openlocfilehash: 8dd969c8644525606227989ca87e41fbfae5aed1
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894188"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="8a9d5-103">從 ASP.NET Web API 移轉至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="8a9d5-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="8a9d5-104">作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)</span><span class="sxs-lookup"><span data-stu-id="8a9d5-104">By [Steve Smith](https://ardalis.com/) and [Scott Addie](https://scottaddie.com)</span></span>

<span data-ttu-id="8a9d5-105">Web Api 是 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-105">Web APIs are HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="8a9d5-106">ASP.NET Core MVC 包括建置 Web Api 提供單一且一致的方式建置 web 應用程式的支援。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-106">ASP.NET Core MVC includes support for building Web APIs providing a single, consistent way of building web applications.</span></span> <span data-ttu-id="8a9d5-107">在本文中，我們會示範將從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-107">In this article, we demonstrate the steps required to migrate a Web API implementation from ASP.NET Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="8a9d5-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="8a9d5-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="review-aspnet-web-api-project"></a><span data-ttu-id="8a9d5-109">檢閱 ASP.NET Web API 專案</span><span class="sxs-lookup"><span data-stu-id="8a9d5-109">Review ASP.NET Web API project</span></span>

<span data-ttu-id="8a9d5-110">這篇文章會使用範例專案*ProductsApp*一文中建立[Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)做為起點。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-110">This article uses the sample project, *ProductsApp*, created in the article [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api) as its starting point.</span></span> <span data-ttu-id="8a9d5-111">在該專案中，簡單的 ASP.NET Web API 專案設定，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-111">In that project, a simple ASP.NET Web API  project is configured as follows.</span></span>

<span data-ttu-id="8a9d5-112">在  *Global.asax.cs*，進行呼叫`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="8a9d5-112">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="8a9d5-113">`WebApiConfig` 定義於*App_Start*，且具有一個靜態`Register`方法：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-113">`WebApiConfig` is defined in *App_Start*, and has just one static `Register` method:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

<span data-ttu-id="8a9d5-114">這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不是專案中使用。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-114">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="8a9d5-115">它也會設定路由表，由 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-115">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="8a9d5-116">在此情況下，ASP.NET Web API 就會預期收到符合格式的 Url */api/ {controller} / {id}*，以 *{id}* 為選擇性。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-116">In this case, ASP.NET Web API will expect URLs to match the format */api/{controller}/{id}*, with *{id}* being optional.</span></span>

<span data-ttu-id="8a9d5-117">*ProductsApp*專案包含一個簡單的控制器，繼承自`ApiController`和公開兩種方法：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-117">The *ProductsApp* project includes just one simple controller, which inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="8a9d5-118">最後，模型中，*產品*中，由*ProductsApp*，是一個簡單的類別：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-118">Finally, the model, *Product*, used by the *ProductsApp*, is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="8a9d5-119">既然我們已經有一個簡單的專案，我們可以從該處開始，會示範如何將此 Web API 專案移轉至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-119">Now that we have a simple project from which to start, we can demonstrate how to migrate this Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-the-destination-project"></a><span data-ttu-id="8a9d5-120">建立目的地專案</span><span class="sxs-lookup"><span data-stu-id="8a9d5-120">Create the Destination Project</span></span>

<span data-ttu-id="8a9d5-121">使用 Visual Studio，建立新的空白方案，並將它命名*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-121">Using Visual Studio, create a new, empty solution, and name it *WebAPIMigration*.</span></span> <span data-ttu-id="8a9d5-122">加入現有*ProductsApp* ，專案，然後將新的 ASP.NET Core Web 應用程式專案加入方案。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-122">Add the existing *ProductsApp* project to it, then, add a new ASP.NET Core Web Application Project to the solution.</span></span> <span data-ttu-id="8a9d5-123">將新專案命名*ProductsCore*。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-123">Name the new project *ProductsCore*.</span></span>

![新增專案 對話方塊開啟至 Web 範本](webapi/_static/add-web-project.png)

<span data-ttu-id="8a9d5-125">接下來，選擇 Web API 專案範本。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-125">Next, choose the Web API project template.</span></span> <span data-ttu-id="8a9d5-126">我們將會移轉*ProductsApp*這個新專案的內容。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-126">We will migrate the *ProductsApp* contents to this new project.</span></span>

![使用 ASP.NET Core 範本清單中選取的 Web API 專案範本的新 Web 應用程式 對話方塊](webapi/_static/aspnet-5-webapi.png)

<span data-ttu-id="8a9d5-128">刪除`Project_Readme.html`來自新的專案檔。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-128">Delete the `Project_Readme.html` file from the new project.</span></span> <span data-ttu-id="8a9d5-129">您的方案現在看起來應該像這樣：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-129">Your solution should now look like this:</span></span>

![在 [方案總管] 中顯示檔案和資料夾的 ProductsApp 和 ProductsCore 專案中開啟的應用程式方案](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a><span data-ttu-id="8a9d5-131">移轉組態</span><span class="sxs-lookup"><span data-stu-id="8a9d5-131">Migrate Configuration</span></span>

<span data-ttu-id="8a9d5-132">ASP.NET Core 不會再使用*Global.asax*， *web.config*，或*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-132">ASP.NET Core no longer uses *Global.asax*, *web.config*, or *App_Start* folders.</span></span> <span data-ttu-id="8a9d5-133">相反地，完成所有啟動工作*Startup.cs*專案的根目錄中 (請參閱[應用程式啟動](../fundamentals/startup.md))。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-133">Instead, all startup tasks are done in *Startup.cs* in the root of the project (see [Application Startup](../fundamentals/startup.md)).</span></span> <span data-ttu-id="8a9d5-134">在 ASP.NET Core MVC 中，屬性為基礎的路由現在預設包含當`UseMvc()`稱為;，這是建議的方法，來設定 Web API 路由 （也是 Web API 入門專案如何處理路由）。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-134">In ASP.NET Core MVC, attribute-based routing is now included by default when `UseMvc()` is called; and, this is the recommended approach for configuring Web API routes (and is how the Web API starter project handles routing).</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

<span data-ttu-id="8a9d5-135">假設您想要使用您從現在開始的專案中的屬性路由，不需要進行其他設定。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-135">Assuming you want to use attribute routing in your project going forward, no additional configuration is needed.</span></span> <span data-ttu-id="8a9d5-136">只需套用的屬性，視您的控制器和動作，如同您在此範例`ValuesController`Web API 入門專案中包含的類別：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-136">Simply apply the attributes as needed to your controllers and actions, as is done in the sample `ValuesController` class that's included in the Web API starter project:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

<span data-ttu-id="8a9d5-137">請注意出現與否 *[controller]* 第 8 行上。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-137">Note the presence of *[controller]* on line 8.</span></span> <span data-ttu-id="8a9d5-138">屬性為基礎的路由現在支援特定的權杖，例如 *[controller]* 並 *[動作]*。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-138">Attribute-based routing now supports certain tokens, such as *[controller]* and *[action]*.</span></span> <span data-ttu-id="8a9d5-139">這些語彙基元會取代在執行階段的控制器或動作，名稱分別屬性套用。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-139">These tokens are replaced at runtime with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="8a9d5-140">這是用來減少的魔術字串，在專案中，並且確保路由將會保留其相對應的控制器和動作與同步時套用自動重新命名重構。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-140">This serves to reduce the number of magic strings in the project, and it ensures the routes will be kept synchronized with their corresponding controllers and actions when automatic rename refactorings are applied.</span></span>

<span data-ttu-id="8a9d5-141">若要移轉產品的 API 控制器，我們必須先複製*ProductsController*至新的專案。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-141">To migrate the Products API controller, we must first copy *ProductsController* to the new project.</span></span> <span data-ttu-id="8a9d5-142">然後只要加入控制器的路由屬性：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-142">Then simply include the route attribute on the controller:</span></span>

```csharp
[Route("api/[controller]")]
```

<span data-ttu-id="8a9d5-143">您也需要新增`[HttpGet]`屬性設定為兩種方法，因為它們都應該透過 HTTP Get 呼叫。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-143">You also need to add the `[HttpGet]` attribute to the two methods, since they both should be called via HTTP Get.</span></span> <span data-ttu-id="8a9d5-144">包含之屬性中的 「 識別碼 」 參數的預期`GetProduct()`:</span><span class="sxs-lookup"><span data-stu-id="8a9d5-144">Include the expectation of an "id" parameter in the attribute for `GetProduct()`:</span></span>

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

<span data-ttu-id="8a9d5-145">此時，路由已正確設定;不過，我們無法尚未進行測試。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-145">At this point, routing is configured correctly; however, we can't yet test it.</span></span> <span data-ttu-id="8a9d5-146">必須進行其他變更，再*ProductsController*會進行編譯。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-146">Additional changes must be made before *ProductsController* will compile.</span></span>

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="8a9d5-147">移轉模型和控制器</span><span class="sxs-lookup"><span data-stu-id="8a9d5-147">Migrate Models and Controllers</span></span>

<span data-ttu-id="8a9d5-148">這個簡單的 Web API 專案的移轉程序的最後一個步驟是將複製的控制站和它們所使用的任何模型。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-148">The last step in the migration process for this simple Web API project is to copy over the Controllers and any Models they use.</span></span> <span data-ttu-id="8a9d5-149">在此情況下，只要複製*Controllers/ProductsController.cs*從原始到新專案。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-149">In this case, simply copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span> <span data-ttu-id="8a9d5-150">然後，將整個 Models 資料夾從原始專案複製到新的。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-150">Then, copy the entire Models folder from the original project to the new one.</span></span> <span data-ttu-id="8a9d5-151">調整以符合新的專案名稱的命名空間 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-151">Adjust the namespaces to match the new project name (*ProductsCore*).</span></span>  <span data-ttu-id="8a9d5-152">此時，您可以建立應用程式，，而且您會找到多個編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-152">At this point, you can build the application, and you will find a number of compilation errors.</span></span> <span data-ttu-id="8a9d5-153">這些通常應該分為下列類別：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-153">These should generally fall into the following categories:</span></span>

* <span data-ttu-id="8a9d5-154">*ApiController*不存在</span><span class="sxs-lookup"><span data-stu-id="8a9d5-154">*ApiController* does not exist</span></span>

* <span data-ttu-id="8a9d5-155">*System.Web.Http*命名空間不存在</span><span class="sxs-lookup"><span data-stu-id="8a9d5-155">*System.Web.Http* namespace does not exist</span></span>

* <span data-ttu-id="8a9d5-156">*IHttpActionResult*不存在</span><span class="sxs-lookup"><span data-stu-id="8a9d5-156">*IHttpActionResult* does not exist</span></span>

<span data-ttu-id="8a9d5-157">幸運的是，這些是所有很容易修正：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-157">Fortunately, these are all very easy to correct:</span></span>

* <span data-ttu-id="8a9d5-158">變更*ApiController*要*控制站*(您可能需要新增*使用 Microsoft.AspNetCore.Mvc*)</span><span class="sxs-lookup"><span data-stu-id="8a9d5-158">Change *ApiController* to *Controller* (you may need to add *using Microsoft.AspNetCore.Mvc*)</span></span>

* <span data-ttu-id="8a9d5-159">刪除任何使用陳述式參考*System.Web.Http*</span><span class="sxs-lookup"><span data-stu-id="8a9d5-159">Delete any using statement referring to *System.Web.Http*</span></span>

* <span data-ttu-id="8a9d5-160">變更任何方法傳回*IHttpActionResult*返回*IActionResult*</span><span class="sxs-lookup"><span data-stu-id="8a9d5-160">Change any method returning *IHttpActionResult* to return a *IActionResult*</span></span>

<span data-ttu-id="8a9d5-161">一旦這些變更已建立且未使用 using 陳述式移除，移轉*ProductsController*類別看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-161">Once these changes have been made and unused using statements removed, the migrated *ProductsController* class looks like this:</span></span>

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

<span data-ttu-id="8a9d5-162">您現在應該能夠執行已移轉的專案，並瀏覽至 */api/產品*; 而且，您應該會看到 3 個產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-162">You should now be able to run the migrated project and browse to */api/products*; and, you should see the full list of 3 products.</span></span> <span data-ttu-id="8a9d5-163">瀏覽至 */api/products/1* ，您應該會看到第一項產品。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-163">Browse to */api/products/1* and you should see the first product.</span></span>

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a><span data-ttu-id="8a9d5-164">ASP.NET 4.x Web API 2 相容性填充碼</span><span class="sxs-lookup"><span data-stu-id="8a9d5-164">ASP.NET 4.x Web API 2 compatibility shim</span></span>

<span data-ttu-id="8a9d5-165">移轉 ASP.NET Web API 專案至 ASP.NET Core 時很有用的工具是[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-165">A useful tool when migrating ASP.NET Web API projects to ASP.NET Core is the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library.</span></span> <span data-ttu-id="8a9d5-166">相容性填充碼會擴充 ASP.NET Core，以允許要使用不同的 Web API 2 慣例的數目。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-166">The compatibility shim extends ASP.NET Core to allow a number of different Web API 2 conventions to be used.</span></span> <span data-ttu-id="8a9d5-167">本文件中先前移轉的範例是基本相容性填充碼並非必要項目。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-167">The sample ported previously in this document is basic enough that the compatibility shim was not necessary.</span></span> <span data-ttu-id="8a9d5-168">針對大型專案，在使用相容性填充碼可用於暫時之間溝通落差 API ASP.NET Core 和 ASP.NET Web API 2。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-168">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET Web API 2.</span></span>

<span data-ttu-id="8a9d5-169">Web API 相容性填充碼是要作為暫時的措施來協助移轉至 ASP.NET Core 的大型 Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-169">The Web API compatibility shim is meant to be used as a temporary measure to facilitate migrating large Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="8a9d5-170">經過一段時間，您應該更新專案，以使用 ASP.NET Core 的模式，而不是依賴相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-170">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="8a9d5-171">中包含的相容性功能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`包括：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-171">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="8a9d5-172">新增`ApiController`類型，以便控制站的基底型別不需要更新。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-172">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="8a9d5-173">可讓 Web API 式模型繫結。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-173">Enables Web API-style model binding.</span></span> <span data-ttu-id="8a9d5-174">ASP.NET Core MVC 模型繫結的功能類似於預設的 MVC 5。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-174">ASP.NET Core MVC model binding functions similarly to MVC 5, by default.</span></span> <span data-ttu-id="8a9d5-175">相容性填充碼變更的模型繫結更類似於 Web API 2 模型繫結的慣例。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-175">The compatibility shim changes model binding to be more similar to Web API 2 model binding conventions.</span></span> <span data-ttu-id="8a9d5-176">例如，複雜型別會自動繫結從要求主體中。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-176">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="8a9d5-177">擴充模型繫結，以便可以採取的型別參數的控制器動作`HttpRequestMessage`。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-177">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="8a9d5-178">新增訊息格式器允許的動作，以傳回結果的型別`HttpResponseMessage`。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-178">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="8a9d5-179">新增其他回應 Web API 2 動作可能已用來處理回應的方法：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-179">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="8a9d5-180">HttpResponseMessage 產生器：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-180">HttpResponseMessage generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="8a9d5-181">動作結果的方法：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-181">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="8a9d5-182">將加入的執行個體`IContentNegotiator`至應用程式的 DI 容器和進行內容交涉相關的類型，從[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)可用。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-182">Adds an instance of `IContentNegotiator` to the app's DI container and makes content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/) available.</span></span> <span data-ttu-id="8a9d5-183">這包括像是類型`DefaultContentNegotiator`，`MediaTypeFormatter`等等。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-183">This includes types like `DefaultContentNegotiator`, `MediaTypeFormatter`, etc.</span></span>

<span data-ttu-id="8a9d5-184">若要使用相容性填充碼，您需要：</span><span class="sxs-lookup"><span data-stu-id="8a9d5-184">To use the compatibility shim, you need to:</span></span>

* <span data-ttu-id="8a9d5-185">安裝[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-185">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
* <span data-ttu-id="8a9d5-186">向應用程式的 DI 容器中的相容性填充碼的服務，藉由呼叫`services.AddMvc().AddWebApiConventions()`應用程式的`Startup.ConfigureServices`方法。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-186">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in the app's `Startup.ConfigureServices` method.</span></span>
* <span data-ttu-id="8a9d5-187">定義使用的 Web API 特有路由`MapWebApiRoute`上`IRouteBuilder`在應用程式的`IApplicationBuilder.UseMvc`呼叫。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-187">Define Web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="summary"></a><span data-ttu-id="8a9d5-188">總結</span><span class="sxs-lookup"><span data-stu-id="8a9d5-188">Summary</span></span>

<span data-ttu-id="8a9d5-189">簡單的 ASP.NET Web API 專案移轉至 ASP.NET Core MVC 是相當直覺，感謝您 Web Api ASP.NET Core MVC 中的內建支援。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-189">Migrating a simple ASP.NET Web API project to ASP.NET Core MVC is fairly straightforward, thanks to the built-in support for Web APIs in ASP.NET Core MVC.</span></span> <span data-ttu-id="8a9d5-190">每個 ASP.NET Web API 專案必須移轉的主要部分會是路由、 控制器及模型，以及更新至控制器和動作所使用的類型。</span><span class="sxs-lookup"><span data-stu-id="8a9d5-190">The main pieces every ASP.NET Web API project will need to migrate are routes, controllers, and models, along with updates to the types used by  controllers and actions.</span></span>
