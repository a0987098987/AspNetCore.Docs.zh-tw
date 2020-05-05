---
title: 從 ASP.NET Web API 遷移至 ASP.NET Core
author: ardalis
description: 瞭解如何將 Web API 的執行，從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/webapi
ms.openlocfilehash: dda457daa0cb8a59ccd4c326a601e375fe4a81bb
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82766585"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a><span data-ttu-id="98dce-103">從 ASP.NET Web API 遷移至 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="98dce-103">Migrate from ASP.NET Web API to ASP.NET Core</span></span>

<span data-ttu-id="98dce-104">作者： [Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)</span><span class="sxs-lookup"><span data-stu-id="98dce-104">By [Scott Addie](https://twitter.com/scott_addie) and [Steve Smith](https://ardalis.com/)</span></span>

<span data-ttu-id="98dce-105">ASP.NET 4.x Web API 是一種 HTTP 服務，可觸及各種用戶端，包括瀏覽器和行動裝置。</span><span class="sxs-lookup"><span data-stu-id="98dce-105">An ASP.NET 4.x Web API is an HTTP service that reaches a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="98dce-106">ASP.NET Core 將 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型統一成較簡單的程式設計模型，稱為 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="98dce-106">ASP.NET Core unifies ASP.NET 4.x's MVC and Web API app models into a simpler programming model known as ASP.NET Core MVC.</span></span> <span data-ttu-id="98dce-107">本文示範從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC 所需的步驟。</span><span class="sxs-lookup"><span data-stu-id="98dce-107">This article demonstrates the steps required to migrate from ASP.NET 4.x Web API to ASP.NET Core MVC.</span></span>

<span data-ttu-id="98dce-108">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample)（[如何下載](xref:index#how-to-download-a-sample)）</span><span class="sxs-lookup"><span data-stu-id="98dce-108">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) ([how to download](xref:index#how-to-download-a-sample))</span></span>

## <a name="prerequisites"></a><span data-ttu-id="98dce-109">先決條件</span><span class="sxs-lookup"><span data-stu-id="98dce-109">Prerequisites</span></span>

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a><span data-ttu-id="98dce-110">審查 ASP.NET 4.x Web API 專案</span><span class="sxs-lookup"><span data-stu-id="98dce-110">Review ASP.NET 4.x Web API project</span></span>

<span data-ttu-id="98dce-111">這篇文章的起點是使用消費者入門中建立的*ProductsApp*專案[ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)。</span><span class="sxs-lookup"><span data-stu-id="98dce-111">As a starting point, this article uses the *ProductsApp* project created in [Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api).</span></span> <span data-ttu-id="98dce-112">在該專案中，會依照下列方式設定簡單的 ASP.NET 4.x Web API 專案。</span><span class="sxs-lookup"><span data-stu-id="98dce-112">In that project, a simple ASP.NET 4.x Web API project is configured as follows.</span></span>

<span data-ttu-id="98dce-113">在*Global.asax.cs*中，會對進行呼叫`WebApiConfig.Register`：</span><span class="sxs-lookup"><span data-stu-id="98dce-113">In *Global.asax.cs*, a call is made to `WebApiConfig.Register`:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

<span data-ttu-id="98dce-114">在`WebApiConfig` *App_Start*資料夾中找到類別，並具有靜態`Register`方法：</span><span class="sxs-lookup"><span data-stu-id="98dce-114">The `WebApiConfig` class is found in the *App_Start* folder and has a static `Register` method:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

<span data-ttu-id="98dce-115">這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，雖然它實際上並不會在專案中使用。</span><span class="sxs-lookup"><span data-stu-id="98dce-115">This class configures [attribute routing](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2), although it's not actually being used in the project.</span></span> <span data-ttu-id="98dce-116">它也會設定 ASP.NET Web API 使用的路由表。</span><span class="sxs-lookup"><span data-stu-id="98dce-116">It also configures the routing table, which is used by ASP.NET Web API.</span></span> <span data-ttu-id="98dce-117">在此情況下，ASP.NET 4.x Web API 會預期 Url 符合格式`/api/{controller}/{id}`，而且`{id}`是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="98dce-117">In this case, ASP.NET 4.x Web API expects URLs to match the format `/api/{controller}/{id}`, with `{id}` being optional.</span></span>

<span data-ttu-id="98dce-118">*ProductsApp*專案包含一個控制器。</span><span class="sxs-lookup"><span data-stu-id="98dce-118">The *ProductsApp* project includes one controller.</span></span> <span data-ttu-id="98dce-119">控制器繼承自`ApiController` ，且包含兩個動作：</span><span class="sxs-lookup"><span data-stu-id="98dce-119">The controller inherits from `ApiController` and contains two actions:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

<span data-ttu-id="98dce-120">所`Product`使用的模型`ProductsController`是一個簡單的類別：</span><span class="sxs-lookup"><span data-stu-id="98dce-120">The `Product` model used by `ProductsController` is a simple class:</span></span>

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

<span data-ttu-id="98dce-121">下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。</span><span class="sxs-lookup"><span data-stu-id="98dce-121">The following sections demonstrate migration of the Web API project to ASP.NET Core MVC.</span></span>

## <a name="create-destination-project"></a><span data-ttu-id="98dce-122">建立目的地專案</span><span class="sxs-lookup"><span data-stu-id="98dce-122">Create destination project</span></span>

<span data-ttu-id="98dce-123">在 Visual Studio 中，完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="98dce-123">Complete the following steps in Visual Studio:</span></span>

* <span data-ttu-id="98dce-124">移至**File** > [檔案] [**新增** > **專案** > ] [**其他專案類型** > ]**Visual Studio 方案**]。</span><span class="sxs-lookup"><span data-stu-id="98dce-124">Go to **File** > **New** > **Project** > **Other Project Types** > **Visual Studio Solutions**.</span></span> <span data-ttu-id="98dce-125">選取 [**空白方案**]，並將方案命名為*WebAPIMigration*。</span><span class="sxs-lookup"><span data-stu-id="98dce-125">Select **Blank Solution**, and name the solution *WebAPIMigration*.</span></span> <span data-ttu-id="98dce-126">按一下 [確定]\*\*\*\* 按鈕。</span><span class="sxs-lookup"><span data-stu-id="98dce-126">Click the **OK** button.</span></span>
* <span data-ttu-id="98dce-127">將現有的*ProductsApp*專案新增至方案。</span><span class="sxs-lookup"><span data-stu-id="98dce-127">Add the existing *ProductsApp* project to the solution.</span></span>
* <span data-ttu-id="98dce-128">將新的**ASP.NET Core Web 應用程式**專案加入至方案。</span><span class="sxs-lookup"><span data-stu-id="98dce-128">Add a new **ASP.NET Core Web Application** project to the solution.</span></span> <span data-ttu-id="98dce-129">從下拉式選單選取 [ **.Net Core**目標 framework]，然後選取 [ **API** ] 專案範本。</span><span class="sxs-lookup"><span data-stu-id="98dce-129">Select the **.NET Core** target framework from the drop-down, and select the **API** project template.</span></span> <span data-ttu-id="98dce-130">將專案命名為*ProductsCore*，然後按一下 [**確定]** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="98dce-130">Name the project *ProductsCore*, and click the **OK** button.</span></span>

<span data-ttu-id="98dce-131">方案現在包含兩個專案。</span><span class="sxs-lookup"><span data-stu-id="98dce-131">The solution now contains two projects.</span></span> <span data-ttu-id="98dce-132">下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。</span><span class="sxs-lookup"><span data-stu-id="98dce-132">The following sections explain migrating the *ProductsApp* project's contents to the *ProductsCore* project.</span></span>

## <a name="migrate-configuration"></a><span data-ttu-id="98dce-133">移轉組態</span><span class="sxs-lookup"><span data-stu-id="98dce-133">Migrate configuration</span></span>

<span data-ttu-id="98dce-134">ASP.NET Core 不會使用*App_Start*資料夾或*global.asax*檔案，而且*web.config*檔案會在發行時加入。</span><span class="sxs-lookup"><span data-stu-id="98dce-134">ASP.NET Core doesn't use the *App_Start* folder or the *Global.asax* file, and the *web.config* file is added at publish time.</span></span> <span data-ttu-id="98dce-135">*Startup.cs*是*global.asax*的取代，位於專案根目錄中。</span><span class="sxs-lookup"><span data-stu-id="98dce-135">*Startup.cs* is the replacement for *Global.asax* and is located in the project root.</span></span> <span data-ttu-id="98dce-136">`Startup`類別會處理所有的應用程式啟動工作。</span><span class="sxs-lookup"><span data-stu-id="98dce-136">The `Startup` class handles all app startup tasks.</span></span> <span data-ttu-id="98dce-137">如需詳細資訊，請參閱<xref:fundamentals/startup>。</span><span class="sxs-lookup"><span data-stu-id="98dce-137">For more information, see <xref:fundamentals/startup>.</span></span>

<span data-ttu-id="98dce-138">在 ASP.NET Core MVC 中，在中<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> `Startup.Configure`呼叫時，預設會包含屬性路由。</span><span class="sxs-lookup"><span data-stu-id="98dce-138">In ASP.NET Core MVC, attribute routing is included by default when <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> is called in `Startup.Configure`.</span></span> <span data-ttu-id="98dce-139">下列`UseMvc`呼叫會取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔：</span><span class="sxs-lookup"><span data-stu-id="98dce-139">The following `UseMvc` call replaces the *ProductsApp* project's *App_Start/WebApiConfig.cs* file:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a><span data-ttu-id="98dce-140">遷移模型和控制器</span><span class="sxs-lookup"><span data-stu-id="98dce-140">Migrate models and controllers</span></span>

<span data-ttu-id="98dce-141">複製*ProductApp*專案的控制器和它所使用的模型。</span><span class="sxs-lookup"><span data-stu-id="98dce-141">Copy over the *ProductApp* project's controller and the model it uses.</span></span> <span data-ttu-id="98dce-142">請遵循下列步驟：</span><span class="sxs-lookup"><span data-stu-id="98dce-142">Follow these steps:</span></span>

1. <span data-ttu-id="98dce-143">將*控制器/productscontroller.cs*從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="98dce-143">Copy *Controllers/ProductsController.cs* from the original project to the new one.</span></span>
1. <span data-ttu-id="98dce-144">將整個 [*模型*] 資料夾從原始專案複製到新的專案。</span><span class="sxs-lookup"><span data-stu-id="98dce-144">Copy the entire *Models* folder from the original project to the new one.</span></span>
1. <span data-ttu-id="98dce-145">變更所複製檔案的命名空間，使其符合新的專案名稱（*ProductsCore*）。</span><span class="sxs-lookup"><span data-stu-id="98dce-145">Change the copied files' namespaces to match the new project name (*ProductsCore*).</span></span> <span data-ttu-id="98dce-146">也請`using ProductsApp.Models;`調整*ProductsController.cs*中的語句。</span><span class="sxs-lookup"><span data-stu-id="98dce-146">Adjust the `using ProductsApp.Models;` statement in *ProductsController.cs* too.</span></span>

<span data-ttu-id="98dce-147">此時，建立應用程式會產生一些編譯錯誤。</span><span class="sxs-lookup"><span data-stu-id="98dce-147">At this point, building the app results in a number of compilation errors.</span></span> <span data-ttu-id="98dce-148">因為下列元件不存在於 ASP.NET Core 中，所以會發生錯誤：</span><span class="sxs-lookup"><span data-stu-id="98dce-148">The errors occur because the following components don't exist in ASP.NET Core:</span></span>

* <span data-ttu-id="98dce-149">`ApiController` 類別</span><span class="sxs-lookup"><span data-stu-id="98dce-149">`ApiController` class</span></span>
* <span data-ttu-id="98dce-150">`System.Web.Http` 命名空間</span><span class="sxs-lookup"><span data-stu-id="98dce-150">`System.Web.Http` namespace</span></span>
* <span data-ttu-id="98dce-151">`IHttpActionResult` 介面</span><span class="sxs-lookup"><span data-stu-id="98dce-151">`IHttpActionResult` interface</span></span>

<span data-ttu-id="98dce-152">修正錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="98dce-152">Fix the errors as follows:</span></span>

1. <span data-ttu-id="98dce-153">將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。</span><span class="sxs-lookup"><span data-stu-id="98dce-153">Change `ApiController` to <xref:Microsoft.AspNetCore.Mvc.ControllerBase>.</span></span> <span data-ttu-id="98dce-154">新增`using Microsoft.AspNetCore.Mvc;`以解析`ControllerBase`參考。</span><span class="sxs-lookup"><span data-stu-id="98dce-154">Add `using Microsoft.AspNetCore.Mvc;` to resolve the `ControllerBase` reference.</span></span>
1. <span data-ttu-id="98dce-155">刪除 `using System.Web.Http;`。</span><span class="sxs-lookup"><span data-stu-id="98dce-155">Delete `using System.Web.Http;`.</span></span>
1. <span data-ttu-id="98dce-156">將`GetProduct`動作的傳回類型從`IHttpActionResult`變更為`ActionResult<Product>`。</span><span class="sxs-lookup"><span data-stu-id="98dce-156">Change the `GetProduct` action's return type from `IHttpActionResult` to `ActionResult<Product>`.</span></span>

<span data-ttu-id="98dce-157">將`GetProduct`動作的`return`語句簡化為下列內容：</span><span class="sxs-lookup"><span data-stu-id="98dce-157">Simplify the `GetProduct` action's `return` statement to the following:</span></span>

```csharp
return product;
```

## <a name="configure-routing"></a><span data-ttu-id="98dce-158">設定路由</span><span class="sxs-lookup"><span data-stu-id="98dce-158">Configure routing</span></span>

<span data-ttu-id="98dce-159">設定路由，如下所示：</span><span class="sxs-lookup"><span data-stu-id="98dce-159">Configure routing as follows:</span></span>

1. <span data-ttu-id="98dce-160">標記具有`ProductsController`下列屬性的類別：</span><span class="sxs-lookup"><span data-stu-id="98dce-160">Mark the `ProductsController` class with the following attributes:</span></span>

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    <span data-ttu-id="98dce-161">上述[`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)屬性會設定控制器的屬性路由模式。</span><span class="sxs-lookup"><span data-stu-id="98dce-161">The preceding [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) attribute configures the controller's attribute routing pattern.</span></span> <span data-ttu-id="98dce-162">[`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。</span><span class="sxs-lookup"><span data-stu-id="98dce-162">The [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute) attribute makes attribute routing a requirement for all actions in this controller.</span></span>

    <span data-ttu-id="98dce-163">屬性路由支援權杖，例如`[controller]`和。 `[action]`</span><span class="sxs-lookup"><span data-stu-id="98dce-163">Attribute routing supports tokens, such as `[controller]` and `[action]`.</span></span> <span data-ttu-id="98dce-164">在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。</span><span class="sxs-lookup"><span data-stu-id="98dce-164">At runtime, each token is replaced with the name of the controller or action, respectively, to which the attribute has been applied.</span></span> <span data-ttu-id="98dce-165">權杖會減少專案中的魔術字串數目。</span><span class="sxs-lookup"><span data-stu-id="98dce-165">The tokens reduce the number of magic strings in the project.</span></span> <span data-ttu-id="98dce-166">當套用自動重新命名重構時，權杖也會確保路由與對應的控制器和動作保持同步。</span><span class="sxs-lookup"><span data-stu-id="98dce-166">The tokens also ensure routes remain synchronized with the corresponding controllers and actions when automatic rename refactorings are applied.</span></span>
1. <span data-ttu-id="98dce-167">將專案的相容性模式設定為 ASP.NET Core 2.2：</span><span class="sxs-lookup"><span data-stu-id="98dce-167">Set the project's compatibility mode to ASP.NET Core 2.2:</span></span>

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    <span data-ttu-id="98dce-168">先前的變更：</span><span class="sxs-lookup"><span data-stu-id="98dce-168">The preceding change:</span></span>

    * <span data-ttu-id="98dce-169">需要使用控制器層級`[ApiController]`的屬性。</span><span class="sxs-lookup"><span data-stu-id="98dce-169">Is required to use the `[ApiController]` attribute at the controller level.</span></span>
    * <span data-ttu-id="98dce-170">加入宣告 ASP.NET Core 2.2 中引進的可能中斷行為。</span><span class="sxs-lookup"><span data-stu-id="98dce-170">Opts in to potentially breaking behaviors introduced in ASP.NET Core 2.2.</span></span>
1. <span data-ttu-id="98dce-171">對`ProductController`動作啟用 HTTP Get 要求：</span><span class="sxs-lookup"><span data-stu-id="98dce-171">Enable HTTP Get requests to the `ProductController` actions:</span></span>
    * <span data-ttu-id="98dce-172">將[`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性套用至`GetAllProducts`動作。</span><span class="sxs-lookup"><span data-stu-id="98dce-172">Apply the [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) attribute to the `GetAllProducts` action.</span></span>
    * <span data-ttu-id="98dce-173">將`[HttpGet("{id}")]`屬性套用至`GetProduct`動作。</span><span class="sxs-lookup"><span data-stu-id="98dce-173">Apply the `[HttpGet("{id}")]` attribute to the `GetProduct` action.</span></span>

<span data-ttu-id="98dce-174">在上述變更並移除未使用`using`的語句之後， *ProductsController.cs*檔案看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="98dce-174">After the preceding changes and the removal of unused `using` statements, *ProductsController.cs* file looks like this:</span></span>

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

<span data-ttu-id="98dce-175">執行已遷移的專案，並流覽`/api/products`至。</span><span class="sxs-lookup"><span data-stu-id="98dce-175">Run the migrated project, and browse to `/api/products`.</span></span> <span data-ttu-id="98dce-176">隨即會顯示三個產品的完整清單。</span><span class="sxs-lookup"><span data-stu-id="98dce-176">A full list of three products appears.</span></span> <span data-ttu-id="98dce-177">瀏覽至 `/api/products/1`。</span><span class="sxs-lookup"><span data-stu-id="98dce-177">Browse to `/api/products/1`.</span></span> <span data-ttu-id="98dce-178">第一個產品隨即出現。</span><span class="sxs-lookup"><span data-stu-id="98dce-178">The first product appears.</span></span>

## <a name="compatibility-shim"></a><span data-ttu-id="98dce-179">相容性填充碼</span><span class="sxs-lookup"><span data-stu-id="98dce-179">Compatibility shim</span></span>

<span data-ttu-id="98dce-180">[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫會提供相容性填充碼，以將 ASP.NET 4.X Web API 專案移至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="98dce-180">The [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) library provides a compatibility shim to move ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="98dce-181">相容性填充碼會擴充 ASP.NET Core，以支援 ASP.NET 4.x Web API 2 中的一些慣例。</span><span class="sxs-lookup"><span data-stu-id="98dce-181">The compatibility shim extends ASP.NET Core to support a number of conventions from ASP.NET 4.x Web API 2.</span></span> <span data-ttu-id="98dce-182">先前在本檔中移植的範例，其基本程度足以使相容性填充碼不必要。</span><span class="sxs-lookup"><span data-stu-id="98dce-182">The sample ported previously in this document is basic enough that the compatibility shim was unnecessary.</span></span> <span data-ttu-id="98dce-183">對於較大的專案，使用相容性填充碼有助於暫時橋接 ASP.NET Core 和 ASP.NET 4.x Web API 2 之間的 API 差距。</span><span class="sxs-lookup"><span data-stu-id="98dce-183">For larger projects, using the compatibility shim can be useful for temporarily bridging the API gap between ASP.NET Core and ASP.NET 4.x Web API 2.</span></span>

<span data-ttu-id="98dce-184">Web API 相容性填充碼是用來做為暫時的量值，以支援將大型 ASP.NET 4.x Web API 專案遷移至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="98dce-184">The Web API compatibility shim is meant to be used as a temporary measure to support migrating large ASP.NET 4.x Web API projects to ASP.NET Core.</span></span> <span data-ttu-id="98dce-185">經過一段時間，專案應該更新為使用 ASP.NET Core 模式，而不是依賴相容性填充碼。</span><span class="sxs-lookup"><span data-stu-id="98dce-185">Over time, projects should be updated to use ASP.NET Core patterns instead of relying on the compatibility shim.</span></span>

<span data-ttu-id="98dce-186">包含的`Microsoft.AspNetCore.Mvc.WebApiCompatShim`相容性功能包括：</span><span class="sxs-lookup"><span data-stu-id="98dce-186">Compatibility features included in `Microsoft.AspNetCore.Mvc.WebApiCompatShim` include:</span></span>

* <span data-ttu-id="98dce-187">加入`ApiController`類型，讓控制器的基底類型不需要更新。</span><span class="sxs-lookup"><span data-stu-id="98dce-187">Adds an `ApiController` type so that controllers' base types don't need to be updated.</span></span>
* <span data-ttu-id="98dce-188">啟用 Web API 樣式的模型系結。</span><span class="sxs-lookup"><span data-stu-id="98dce-188">Enables Web API-style model binding.</span></span> <span data-ttu-id="98dce-189">根據預設，ASP.NET Core MVC 模型系結函式類似于 ASP.NET 4.x MVC 5 的功能。</span><span class="sxs-lookup"><span data-stu-id="98dce-189">ASP.NET Core MVC model binding functions similarly to that of ASP.NET 4.x MVC 5, by default.</span></span> <span data-ttu-id="98dce-190">相容性填充碼會將模型系結變更為類似于 ASP.NET 4.x Web API 2 模型系結慣例。</span><span class="sxs-lookup"><span data-stu-id="98dce-190">The compatibility shim changes model binding to be more similar to ASP.NET 4.x Web API 2 model binding conventions.</span></span> <span data-ttu-id="98dce-191">例如，複雜型別會自動系結至要求主體。</span><span class="sxs-lookup"><span data-stu-id="98dce-191">For example, complex types are automatically bound from the request body.</span></span>
* <span data-ttu-id="98dce-192">擴充模型系結，讓控制器動作可以接受類型`HttpRequestMessage`的參數。</span><span class="sxs-lookup"><span data-stu-id="98dce-192">Extends model binding so that controller actions can take parameters of type `HttpRequestMessage`.</span></span>
* <span data-ttu-id="98dce-193">加入訊息格式器，允許動作傳回類型`HttpResponseMessage`的結果。</span><span class="sxs-lookup"><span data-stu-id="98dce-193">Adds message formatters allowing actions to return results of type `HttpResponseMessage`.</span></span>
* <span data-ttu-id="98dce-194">新增 Web API 2 動作可能用來服務回應的其他回應方法：</span><span class="sxs-lookup"><span data-stu-id="98dce-194">Adds additional response methods that Web API 2 actions may have used to serve responses:</span></span>
  * <span data-ttu-id="98dce-195">`HttpResponseMessage`產生器</span><span class="sxs-lookup"><span data-stu-id="98dce-195">`HttpResponseMessage` generators:</span></span>
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * <span data-ttu-id="98dce-196">動作結果方法：</span><span class="sxs-lookup"><span data-stu-id="98dce-196">Action result methods:</span></span>
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* <span data-ttu-id="98dce-197">將的`IContentNegotiator`實例新增至應用程式的相依性插入（DI）容器，並提供來自[WebApi](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)的內容協商相關類型。</span><span class="sxs-lookup"><span data-stu-id="98dce-197">Adds an instance of `IContentNegotiator` to the app's dependency injection (DI) container and makes available the content negotiation-related types from [Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/).</span></span> <span data-ttu-id="98dce-198">這類類型的範例`DefaultContentNegotiator`包括`MediaTypeFormatter`和。</span><span class="sxs-lookup"><span data-stu-id="98dce-198">Examples of such types include `DefaultContentNegotiator` and `MediaTypeFormatter`.</span></span>

<span data-ttu-id="98dce-199">若要使用相容性填充碼：</span><span class="sxs-lookup"><span data-stu-id="98dce-199">To use the compatibility shim:</span></span>

1. <span data-ttu-id="98dce-200">安裝[AspNetCore WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="98dce-200">Install the [Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet package.</span></span>
1. <span data-ttu-id="98dce-201">在中`services.AddMvc().AddWebApiConventions()` `Startup.ConfigureServices`呼叫，以向應用程式的 DI 容器註冊相容性填充碼的服務。</span><span class="sxs-lookup"><span data-stu-id="98dce-201">Register the compatibility shim's services with the app's DI container by calling `services.AddMvc().AddWebApiConventions()` in `Startup.ConfigureServices`.</span></span>
1. <span data-ttu-id="98dce-202">在應用程式的`MapWebApiRoute` `IRouteBuilder` `IApplicationBuilder.UseMvc`呼叫中，使用來定義 Web API 特定的路由。</span><span class="sxs-lookup"><span data-stu-id="98dce-202">Define web API-specific routes using `MapWebApiRoute` on the `IRouteBuilder` in the app's `IApplicationBuilder.UseMvc` call.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="98dce-203">其他資源</span><span class="sxs-lookup"><span data-stu-id="98dce-203">Additional resources</span></span>

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
