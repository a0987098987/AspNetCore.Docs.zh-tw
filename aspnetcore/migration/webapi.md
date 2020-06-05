---
title: 從 ASP.NET Web API 遷移至 ASP.NET Core
author: ardalis
description: 瞭解如何將 Web API 的執行，從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC。
ms.author: scaddie
ms.custom: mvc
ms.date: 05/26/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: migration/webapi
ms.openlocfilehash: 3c8bf27a97de92a42817d4af625976a4920001aa
ms.sourcegitcommit: cd73744bd75fdefb31d25ab906df237f07ee7a0a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/05/2020
ms.locfileid: "84145547"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>從 ASP.NET Web API 遷移至 ASP.NET Core

作者： [Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)

ASP.NET 4.x Web API 是一種 HTTP 服務，可觸及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Core 將 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型結合成單一程式設計模型，稱為 ASP.NET Core MVC。 本文示範從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC 所需的步驟。

[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample)（[如何下載](xref:index#how-to-download-a-sample)）

::: moniker range=">= aspnetcore-3.0"

## <a name="prerequisites"></a>必要條件

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs-3.1.md)]

## <a name="review-aspnet-4x-web-api-project"></a>審查 ASP.NET 4.x Web API 專案

本文使用[ASP.NET Web API 2 消費者入門](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)中建立的*ProductsApp*專案。 在該專案中，會依照下列方式設定基本的 ASP.NET 4.x Web API 專案。

在*Global.asax.cs*中，會對進行呼叫 `WebApiConfig.Register` ：

[!code-csharp[](webapi/sample/3.x/ProductsApp/Global.asax.cs?highlight=14)]

在 `WebApiConfig` *App_Start*資料夾中找到類別，並具有靜態 `Register` 方法：

[!code-csharp[](webapi/sample/3.x/ProductsApp/App_Start/WebApiConfig.cs)]

上述類別：

* 會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不會使用它。
* 設定路由表。
範例程式碼需要 Url 以符合格式 `/api/{controller}/{id}` ，而且 `{id}` 是選擇性的。

下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。

## <a name="create-the-destination-project"></a>建立目的地專案

在 Visual Studio 中建立新的空白方案，並新增 ASP.NET 4.x Web API 專案以進行遷移：

1. 從 [檔案]**** 功能表選取 [新增]**[專案]** > ****。
1. 選取 [**空白解決方案**] 範本，然後選取 **[下一步]**。
1. 將方案命名為*WebAPIMigration*。 選取 [建立]。
1. 將現有的*ProductsApp*專案新增至方案。

新增要遷移至的新 API 專案：

1. 將新的**ASP.NET Core Web 應用程式**專案加入至方案。
1. 在 [**設定您的新專案**] 對話方塊中，將專案命名為*ProductsCore*，然後選取 [**建立**]。
1. 在 [**建立新的 ASP.NET Core Web 應用程式**] 對話方塊中，確認已選取 [ **.net Core** ] 和 [ **ASP.NET Core 3.1** ]。 選取 [API]**** 專案範本，然後選取 [確定]****。
1. 從新的*ProductsCore*專案中移除*WeatherForecast.cs*和 controller */WeatherForecastController*範例檔案。

方案現在包含兩個專案。 下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。

## <a name="migrate-configuration"></a>移轉組態

ASP.NET Core 不會使用*App_Start*資料夾或*global.asax*檔案。 此外，也會在發行時新增*web.config*檔案。

`Startup` 類別：

* 取代*global.asax*。
* 處理所有應用程式啟動工作。

如需詳細資訊，請參閱 <xref:fundamentals/startup> 。

## <a name="migrate-models-and-controllers"></a>遷移模型和控制器

下列程式碼顯示 `ProductsController` 要更新 ASP.NET Core 的：

[!code-csharp[](webapi/sample/3.x/ProductsApp/Controllers/ProductsController.cs)]

更新 `ProductsController` ASP.NET Core 的：

1. 將*控制器/productscontroller.cs*和 [*模型*] 資料夾從原始專案複製到新的專案。
1. 將複製的檔案根命名空間變更為 `ProductsCore` 。
1. 將 `using ProductsApp.Models;` 語句更新為 `using ProductsCore.Models;` 。

ASP.NET Core 中不存在下列元件：

* `ApiController` 類別
* `System.Web.Http` 命名空間
* `IHttpActionResult` 介面

進行下列變更：

1. 將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。 新增 `using Microsoft.AspNetCore.Mvc;` 以解析 `ControllerBase` 參考。
1. 刪除 `using System.Web.Http;`。
1. 將 `GetProduct` 動作的傳回類型從變更 `IHttpActionResult` 為 `ActionResult<Product>` 。
1. 將 `GetProduct` 動作的 `return` 語句簡化為下列內容：

    ```csharp
    return product;
    ```

## <a name="configure-routing"></a>設定路由

ASP.NET Core *API*專案範本會在產生的程式碼中包含端點路由設定。

下列 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseRouting%2A> 和會 <xref:Microsoft.AspNetCore.Builder.EndpointRoutingApplicationBuilderExtensions.UseEndpoints%2A> 呼叫：

* 在[中介軟體](xref:fundamentals/middleware/index)管線中註冊路由對應和端點執行。
* 取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔案。

[!code-csharp[](webapi/sample/3.x/ProductsCore/Startup.cs?name=snippet_Configure&highlight=10,14)]

設定路由，如下所示：

1. 標記 `ProductsController` 具有下列屬性的類別：

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    上述屬性會設定 [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) 控制器的屬性路由模式。 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。

    屬性路由支援權杖，例如 `[controller]` 和 `[action]` 。 在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。 權杖：
    * 減少專案中的魔術字串數目。
    * 當套用自動重新命名重構時，請確定路由與對應的控制器和動作保持同步。
1. 對動作啟用 HTTP Get 要求 `ProductController` ：
    * 將 [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) 屬性套用至 `GetAllProducts` 動作。
    * 將 `[HttpGet("{id}")]` 屬性套用至 `GetProduct` 動作。

執行已遷移的專案，並流覽至 `/api/products` 。 隨即會顯示三個產品的完整清單。 瀏覽至 `/api/products/1`。 第一個產品隨即出現。

## <a name="additional-resources"></a>其他資源

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>

::: moniker-end

::: moniker range="<= aspnetcore-2.2"
## <a name="prerequisites"></a>必要條件

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>審查 ASP.NET 4.x Web API 專案

本文使用[ASP.NET Web API 2 消費者入門](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)中建立的*ProductsApp*專案。 在該專案中，會依照下列方式設定基本的 ASP.NET 4.x Web API 專案。

在*Global.asax.cs*中，會對進行呼叫 `WebApiConfig.Register` ：

[!code-csharp[](webapi/sample/2.x/ProductsApp/Global.asax.cs?highlight=14)]

在 `WebApiConfig` *App_Start*資料夾中找到類別，並具有靜態 `Register` 方法：

[!code-csharp[](webapi/sample/2.x/ProductsApp/App_Start/WebApiConfig.cs)]

這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，雖然它實際上並不會在專案中使用。 它也會設定 ASP.NET Web API 使用的路由表。 在此情況下，ASP.NET 4.x Web API 會預期 Url 符合格式 `/api/{controller}/{id}` ，而且 `{id}` 是選擇性的。

下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。

## <a name="create-the-destination-project"></a>建立目的地專案

在 Visual Studio 中，完成下列步驟：

* 移至 **[** 檔案] [  >  **新增**  >  **專案**] [  >  **其他專案類型**]  >  **Visual Studio 方案**]。 選取 [**空白方案**]，並將方案命名為*WebAPIMigration*。 按一下 [確定]**** 按鈕。
* 將現有的*ProductsApp*專案新增至方案。
* 將新的**ASP.NET Core Web 應用程式**專案加入至方案。 從下拉式選單選取 [ **.Net Core**目標 framework]，然後選取 [ **API** ] 專案範本。 將專案命名為*ProductsCore*，然後按一下 [**確定]** 按鈕。

方案現在包含兩個專案。 下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。

## <a name="migrate-configuration"></a>移轉組態

ASP.NET Core 不會使用：

* *App_Start*資料夾或*global.asax*檔案
* *web.config 檔案*會在發行時加入。

`Startup` 類別：

* 取代*global.asax*。
* 處理所有應用程式啟動工作。

如需詳細資訊，請參閱 <xref:fundamentals/startup> 。

在 ASP.NET Core MVC 中，在中呼叫時，預設會包含屬性路由 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> `Startup.Configure` 。 下列 `UseMvc` 呼叫會取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔：

[!code-csharp[](webapi/sample/2.x/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13)]

## <a name="migrate-models-and-controllers"></a>遷移模型和控制器

下列程式碼顯示 `ProductsController` ASP.NET Core 的更新：[!code-csharp[](webapi/sample/2.x/ProductsApp/Controllers/ProductsController.cs)]

更新 `ProductsController` ASP.NET Core 的：

1. 將*控制器/productscontroller.cs*從原始專案複製到新的專案。
1. 將 [*模型*] 資料夾從原始專案複製到新的專案。
1. 將複製的檔案根命名空間變更為 `ProductsCore` 。
1. 將 `using ProductsApp.Models;` 語句更新為 `using ProductsCore.Models;` 。

ASP.NET Core 中不存在下列元件：

* `ApiController` 類別
* `System.Web.Http` 命名空間
* `IHttpActionResult` 介面

進行下列變更：

1. 將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。 新增 `using Microsoft.AspNetCore.Mvc;` 以解析 `ControllerBase` 參考。
1. 刪除 `using System.Web.Http;`。
1. 將 `GetProduct` 動作的傳回類型從變更 `IHttpActionResult` 為 `ActionResult<Product>` 。
1. 將 `GetProduct` 動作的 `return` 語句簡化為下列內容：

    ```csharp
    return product;
    ```

## <a name="configure-routing"></a>設定路由

設定路由，如下所示：

1. 標記 `ProductsController` 具有下列屬性的類別：

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    上述屬性會設定 [`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute) 控制器的屬性路由模式。 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。

    屬性路由支援權杖，例如 `[controller]` 和 `[action]` 。 在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。 權杖會減少專案中的魔術字串數目。 當套用自動重新命名重構時，權杖也會確保路由與對應的控制器和動作保持同步。
1. 將專案的相容性模式設定為 ASP.NET Core 2.2：

    [!code-csharp[](webapi/sample/2.x/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    先前的變更：

    * 需要使用 `[ApiController]` 控制器層級的屬性。
    * 加入宣告 ASP.NET Core 2.2 中引進的可能中斷行為。
1. 對動作啟用 HTTP Get 要求 `ProductController` ：
    * 將 [`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute) 屬性套用至 `GetAllProducts` 動作。
    * 將 `[HttpGet("{id}")]` 屬性套用至 `GetProduct` 動作。

執行已遷移的專案，並流覽至 `/api/products` 。 隨即會顯示三個產品的完整清單。 瀏覽至 `/api/products/1`。 第一個產品隨即出現。

## <a name="compatibility-shim"></a>相容性填充碼

[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫會提供相容性填充碼，以將 ASP.NET 4.X Web API 專案移至 ASP.NET Core。 相容性填充碼會擴充 ASP.NET Core，以支援 ASP.NET 4.x Web API 2 中的一些慣例。 先前在本檔中移植的範例，其基本程度足以使相容性填充碼不必要。 對於較大的專案，使用相容性填充碼有助於暫時橋接 ASP.NET Core 和 ASP.NET 4.x Web API 2 之間的 API 差距。

Web API 相容性填充碼是用來做為暫時的量值，以支援將大型 ASP.NET 4.x Web API 專案遷移至 ASP.NET Core。 經過一段時間，專案應該更新為使用 ASP.NET Core 模式，而不是依賴相容性填充碼。

包含的相容性功能 `Microsoft.AspNetCore.Mvc.WebApiCompatShim` 包括：

* 加入 `ApiController` 類型，讓控制器的基底類型不需要更新。
* 啟用 Web API 樣式的模型系結。 根據預設，ASP.NET Core MVC 模型系結函式類似于 ASP.NET 4.x MVC 5 的功能。 相容性填充碼會將模型系結變更為類似于 ASP.NET 4.x Web API 2 模型系結慣例。 例如，複雜型別會自動系結至要求主體。
* 擴充模型系結，讓控制器動作可以接受類型的參數 `HttpRequestMessage` 。
* 加入訊息格式器，允許動作傳回類型的結果 `HttpResponseMessage` 。
* 新增 Web API 2 動作可能用來服務回應的其他回應方法：
  * `HttpResponseMessage`產生器
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * 動作結果方法：
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* 將的實例新增 `IContentNegotiator` 至應用程式的相依性插入（DI）容器，並提供來自[WebApi](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)的內容協商相關類型。 這類類型的範例包括 `DefaultContentNegotiator` 和 `MediaTypeFormatter` 。

若要使用相容性填充碼：

1. 安裝[AspNetCore WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。
1. 在中呼叫，以向應用程式的 DI 容器註冊相容性填充碼的服務 `services.AddMvc().AddWebApiConventions()` `Startup.ConfigureServices` 。
1. `MapWebApiRoute` `IRouteBuilder` 在應用程式的呼叫中，使用來定義 Web API 特定的路由 `IApplicationBuilder.UseMvc` 。

## <a name="additional-resources"></a>其他資源

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
::: moniker-end
