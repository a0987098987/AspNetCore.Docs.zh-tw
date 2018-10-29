---
title: 從 ASP.NET Web API 移轉至 ASP.NET Core
author: ardalis
description: 了解如何從 ASP.NET 4.x Web API 的 web API 實作移轉至 ASP.NET Core MVC。
ms.author: scaddie
ms.custom: mvc
ms.date: 10/01/2018
uid: migration/webapi
ms.openlocfilehash: f5d886a7c3182b5cd372762ade67c2e748051049
ms.sourcegitcommit: 375e9a67f5e1f7b0faaa056b4b46294cc70f55b7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/29/2018
ms.locfileid: "50207273"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="770b7-103">從 ASP.NET Web API 移轉至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="770b7-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="770b7-104">藉由[Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="770b7-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="770b7-105">ASP.NET 4.x Web API 是一種 HTTP 服務，達到各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="770b7-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="770b7-106">ASP.NET Core 可整合 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型到簡單的程式設計模型，稱為 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="770b7-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="770b7-107">本文示範如何從 ASP.NET 4.x Web API 移轉至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="770b7-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="770b7-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="770b7-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="770b7-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="770b7-109">Prerequisites</span></span>

* [<span data-ttu-id="770b7-110">.NET Core 2.1 SDK 或更新版本</span><span class="sxs-lookup"><span data-stu-id="770b7-110">.NET Core 2.1 SDK or later</span></span>](https://www.microsoft.com/net/download/all)
* <span data-ttu-id="770b7-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) 15.7.3 版或更新版本，包含 **ASP.NET 及網頁程式開發**工作負載</span><span class="sxs-lookup"><span data-stu-id="770b7-111">[Visual Studio 2017](https://www.visualstudio.com/downloads/) version 15.7.3 or later with the **ASP.NET and web development** workload</span></span>

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="770b7-112">檢閱 ASP.NET 4.x Web API 專案</span><span class="sxs-lookup"><span data-stu-id="770b7-112">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="770b7-113">本文章使用做為起點*ProductsApp*中建立專案[Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="770b7-113">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="770b7-114">在該專案中，簡單的 ASP.NET 4.x Web API 專案設定，如下所示。</span><span class="sxs-lookup"><span data-stu-id="770b7-114">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="770b7-115">在  *Global.asax.cs*，進行呼叫`WebApiConfig.Register`:</span><span class="sxs-lookup"><span data-stu-id="770b7-115">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="770b7-116">`WebApiConfig` 定義於*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="770b7-116">`WebApiConfig` is defined in the *App_Start* folder.</span></span> <span data-ttu-id="770b7-117">它有一個靜態`Register`方法：</span><span class="sxs-lookup"><span data-stu-id="770b7-117">It has just one static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15-20)]

<span data-ttu-id="770b7-118">這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不是專案中使用。</span><span class="sxs-lookup"><span data-stu-id="770b7-118">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="770b7-119">它也會設定路由表，由 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="770b7-119">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="770b7-120">在此情況下，ASP.NET 4.x Web API 必須要有符合格式的 Url `/api/{controller}/{id}`，使用`{id}`為選擇性。</span><span class="sxs-lookup"><span data-stu-id="770b7-120">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="770b7-121">*ProductsApp*專案包含一個控制站。</span><span class="sxs-lookup"><span data-stu-id="770b7-121">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="770b7-122">控制器會繼承`ApiController`和公開兩種方法：</span><span class="sxs-lookup"><span data-stu-id="770b7-122">The controller inherits from `ApiController` and exposes two methods:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

<span data-ttu-id="770b7-123">`Product`所使用的模型`ProductsController`是簡單的類別：</span><span class="sxs-lookup"><span data-stu-id="770b7-123">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="770b7-124">下列各節示範移轉到 ASP.NET Core MVC Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="770b7-124">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="770b7-125">建立目的專案</span><span class="sxs-lookup"><span data-stu-id="770b7-125">Create destination project</span></span>

<span data-ttu-id="770b7-126">完成在 Visual Studio 中的下列步驟：</span><span class="sxs-lookup"><span data-stu-id="770b7-126">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="770b7-127">移至**檔案** > **新** > **專案** > **其他專案類型** > **Visual Studio 方案**。</span><span class="sxs-lookup"><span data-stu-id="770b7-127">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="770b7-128">選取 **空白方案**，再將方案命名*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="770b7-128">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="770b7-129">按一下 [**確定**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="770b7-129">Click the **OK** button.</span></span>
* <span data-ttu-id="770b7-130">加入現有*ProductsApp*專案加入方案。</span><span class="sxs-lookup"><span data-stu-id="770b7-130">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="770b7-131">加入新**ASP.NET Core Web 應用程式**專案加入方案。</span><span class="sxs-lookup"><span data-stu-id="770b7-131">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="770b7-132">選取  **.NET Core**下拉式清單中，從 framework 為目標，然後選取**API**專案範本。</span><span class="sxs-lookup"><span data-stu-id="770b7-132">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="770b7-133">將專案命名為*ProductsCore*，然後按一下**確定** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="770b7-133">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="770b7-134">此方案現在包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="770b7-134">The solution now contains two projects.</span></span> <span data-ttu-id="770b7-135">下列各節說明移轉*ProductsApp*專案的內容，以*ProductsCore*專案。</span><span class="sxs-lookup"><span data-stu-id="770b7-135">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="770b7-136">移轉組態</span><span class="sxs-lookup"><span data-stu-id="770b7-136">Migrate configuration</span></span>

<span data-ttu-id="770b7-137">不會使用 ASP.NET Core *App_Start*資料夾或*Global.asax*檔案，而*web.config*檔案會新增在發行時。</span><span class="sxs-lookup"><span data-stu-id="770b7-137">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="770b7-138">*Startup.cs*會取代*Global.asax*而且位於專案根目錄中。</span><span class="sxs-lookup"><span data-stu-id="770b7-138">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="770b7-139">`Startup`類別會處理所有的應用程式啟動工作。</span><span class="sxs-lookup"><span data-stu-id="770b7-139">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="770b7-140">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="770b7-140">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="770b7-141">依預設包含 ASP.NET Core MVC 中，屬性路由時<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>呼叫`Startup.Configure`。</span><span class="sxs-lookup"><span data-stu-id="770b7-141">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="770b7-142">下列`UseMvc`呼叫取代*ProductsApp*專案組*app_start/webapiconfig.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="770b7-142">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="770b7-143">移轉模型和控制器</span><span class="sxs-lookup"><span data-stu-id="770b7-143">Migrate models and controllers</span></span>

<span data-ttu-id="770b7-144">透過複製*ProductApp*專案的控制器和它所使用的模型。</span><span class="sxs-lookup"><span data-stu-id="770b7-144">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="770b7-145">請依照下列步驟：</span><span class="sxs-lookup"><span data-stu-id="770b7-145">Follow these steps:</span></span>

1. <span data-ttu-id="770b7-146">複製*Controllers/ProductsController.cs*從原始到新專案。</span><span class="sxs-lookup"><span data-stu-id="770b7-146">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="770b7-147">複製整個*模型*從原始專案到新的資料夾。</span><span class="sxs-lookup"><span data-stu-id="770b7-147">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="770b7-148">變更已複製的檔案命名空間，以符合新的專案名稱 (*ProductsCore*)。</span><span class="sxs-lookup"><span data-stu-id="770b7-148">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="770b7-149">調整`using ProductsApp.Models;`中的陳述式*ProductsController.cs*太。</span><span class="sxs-lookup"><span data-stu-id="770b7-149">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="770b7-150">此時，建置應用程式會導致編譯錯誤數目。</span><span class="sxs-lookup"><span data-stu-id="770b7-150">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="770b7-151">因為下列元件不存在於 ASP.NET Core，就會發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="770b7-151">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="770b7-152">`ApiController` 類別</span><span class="sxs-lookup"><span data-stu-id="770b7-152">`ApiController` class</span></span>
* <span data-ttu-id="770b7-153">`System.Web.Http` 命名空間</span><span class="sxs-lookup"><span data-stu-id="770b7-153">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="770b7-154">`IHttpActionResult` 介面</span><span class="sxs-lookup"><span data-stu-id="770b7-154">`IHttpActionResult` interface</span></span>

<span data-ttu-id="770b7-155">請修正錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="770b7-155">Fix the errors as follows:</span></span>

1. <span data-ttu-id="770b7-156">變更`ApiController`至<xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="770b7-156">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="770b7-157">新增`using Microsoft.AspNetCore.Mvc;`若要解決`ControllerBase`參考。</span><span class="sxs-lookup"><span data-stu-id="770b7-157">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="770b7-158">刪除 `using System.Web.Http;`。</span><span class="sxs-lookup"><span data-stu-id="770b7-158">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="770b7-159">變更`GetProduct`動作的傳回類型從`IHttpActionResult`至`ActionResult<Product>`。</span><span class="sxs-lookup"><span data-stu-id="770b7-159">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

## <a name="configure-routing"></a><span data-ttu-id="770b7-160">設定路由</span><span class="sxs-lookup"><span data-stu-id="770b7-160">Configure routing</span></span>

<span data-ttu-id="770b7-161">設定路由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="770b7-161">Configure routing as follows:</span></span>

1. <span data-ttu-id="770b7-162">裝飾`ProductsController`類別具有下列屬性：</span><span class="sxs-lookup"><span data-stu-id="770b7-162">Decorate the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="770b7-163">上述[[路由]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)屬性會設定控制器的屬性路由的模式。</span><span class="sxs-lookup"><span data-stu-id="770b7-163">The preceding [[Route]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="770b7-164">[[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由此控制器中的所有動作的需求。</span><span class="sxs-lookup"><span data-stu-id="770b7-164">The [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="770b7-165">屬性路由支援權杖，例如`[controller]`和`[action]`。</span><span class="sxs-lookup"><span data-stu-id="770b7-165">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="770b7-166">在執行階段，每個語彙基元會取代控制器或動作，名稱分別屬性套用。</span><span class="sxs-lookup"><span data-stu-id="770b7-166">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="770b7-167">權杖會減少專案中的魔術字串數目。</span><span class="sxs-lookup"><span data-stu-id="770b7-167">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="770b7-168">路由與保持同步的相對應的控制站，並套用時自動重新命名重構的動作，也請確定語彙基元。</span><span class="sxs-lookup"><span data-stu-id="770b7-168">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="770b7-169">啟用 HTTP Get 要求以`ProductController`動作：</span><span class="sxs-lookup"><span data-stu-id="770b7-169">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="770b7-170">適用於[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性設定為`GetAllProducts`動作。</span><span class="sxs-lookup"><span data-stu-id="770b7-170">Apply the [[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="770b7-171">適用於`[HttpGet("{id}")]`屬性設定為`GetProduct`動作。</span><span class="sxs-lookup"><span data-stu-id="770b7-171">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="770b7-172">這些變更並移除未使用之後`using`陳述式， *ProductsController.cs*檔案看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="770b7-172">After these changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="770b7-173">執行移轉的專案，並瀏覽至`/api/products`。</span><span class="sxs-lookup"><span data-stu-id="770b7-173">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="770b7-174">顯示三個產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="770b7-174">A full list of three products appears.</span></span> <span data-ttu-id="770b7-175">瀏覽至 `/api/products/1`。</span><span class="sxs-lookup"><span data-stu-id="770b7-175">Browse to `/api/products/1`.</span></span> <span data-ttu-id="770b7-176">第一項產品會出現。</span><span class="sxs-lookup"><span data-stu-id="770b7-176">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="770b7-177">相容性填充碼</span><span class="sxs-lookup"><span data-stu-id="770b7-177">Compatibility shim</span></span>

<span data-ttu-id="770b7-178">[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫提供相容性填充碼移動到 ASP.NET Core 的 ASP.NET 4.x Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="770b7-178">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="770b7-179">相容性填充碼會擴充 ASP.NET Core 以支援一些從 ASP.NET 4.x Web API 2 的慣例。</span><span class="sxs-lookup"><span data-stu-id="770b7-179">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="770b7-180">本文件中先前移轉的範例是基本相容性填充碼是不必要的。</span><span class="sxs-lookup"><span data-stu-id="770b7-180">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="770b7-181">針對大型專案，在使用相容性填充碼可用於暫時之間溝通落差 API ASP.NET Core 和 ASP.NET 4.x Web API 2。</span><span class="sxs-lookup"><span data-stu-id="770b7-181">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="770b7-182">Web API 相容性填充碼是要作為暫時的措施來支援大型 ASP.NET 4.x Web API 專案移轉至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="770b7-182">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="770b7-183">經過一段時間，您應該更新專案，以使用 ASP.NET Core 的模式，而不是依賴相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="770b7-183">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="770b7-184">中包含的相容性功能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`包括：</span><span class="sxs-lookup"><span data-stu-id="770b7-184">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="770b7-185">新增`ApiController`類型，以便控制站的基底型別不需要更新。</span><span class="sxs-lookup"><span data-stu-id="770b7-185">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="770b7-186">可讓 Web API 式模型繫結。</span><span class="sxs-lookup"><span data-stu-id="770b7-186">Enables Web API-style model binding.</span></span> <span data-ttu-id="770b7-187">ASP.NET Core MVC 模型繫結的功能，類似於 ASP.NET 的預設的 MVC 5，4.x。</span><span class="sxs-lookup"><span data-stu-id="770b7-187">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="770b7-188">相容性填充碼變更的模型繫結更類似於 ASP.NET 4.x Web API 2 模型繫結慣例。</span><span class="sxs-lookup"><span data-stu-id="770b7-188">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="770b7-189">例如，複雜型別會自動繫結從要求主體中。</span><span class="sxs-lookup"><span data-stu-id="770b7-189">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="770b7-190">擴充模型繫結，以便可以採取的型別參數的控制器動作`HttpRequestMessage`。</span><span class="sxs-lookup"><span data-stu-id="770b7-190">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="770b7-191">新增訊息格式器允許的動作，以傳回結果的型別`HttpResponseMessage`。</span><span class="sxs-lookup"><span data-stu-id="770b7-191">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="770b7-192">新增其他回應 Web API 2 動作可能已用來處理回應的方法：</span><span class="sxs-lookup"><span data-stu-id="770b7-192">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="770b7-193">`HttpResponseMessage` 產生器：</span><span class="sxs-lookup"><span data-stu-id="770b7-193">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="770b7-194">動作結果的方法：</span><span class="sxs-lookup"><span data-stu-id="770b7-194">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="770b7-195">將加入的執行個體`IContentNegotiator`應用程式的相依性插入 (DI) 容器，並提供內容的交涉相關型別從[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)。</span><span class="sxs-lookup"><span data-stu-id="770b7-195">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="770b7-196">這種類型的範例包括`DefaultContentNegotiator`和`MediaTypeFormatter`。</span><span class="sxs-lookup"><span data-stu-id="770b7-196">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="770b7-197">若要使用相容性填充碼：</span><span class="sxs-lookup"><span data-stu-id="770b7-197">To use the compatibility shim:</span></span>

1. <span data-ttu-id="770b7-198">安裝[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="770b7-198">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="770b7-199">向應用程式的 DI 容器中的相容性填充碼的服務，藉由呼叫`services.AddMvc().AddWebApiConventions()`在`Startup.ConfigureServices`。</span><span class="sxs-lookup"><span data-stu-id="770b7-199">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="770b7-200">定義的 web API 特定路由使用`MapWebApiRoute`上`IRouteBuilder`在應用程式的`IApplicationBuilder.UseMvc`呼叫。</span><span class="sxs-lookup"><span data-stu-id="770b7-200">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="770b7-201">其他資源</span><span class="sxs-lookup"><span data-stu-id="770b7-201">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
