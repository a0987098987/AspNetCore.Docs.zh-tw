---
title: 從 ASP.NET Web API 遷移至 ASP.NET Core
author: ardalis
description: 瞭解如何將 Web API 的執行，從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/05/2019
uid: migration/webapi
ms.openlocfilehash: c68cf83f427f53b110075168c6d5e4d021808782
ms.sourcegitcommit: c0b72b344dadea835b0e7943c52463f13ab98dd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/06/2019
ms.locfileid: "74881137"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>從 ASP.NET Web API 遷移至 ASP.NET Core

作者： [Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)

ASP.NET 4.x Web API 是一種 HTTP 服務，可觸及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Core 將 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型統一成較簡單的程式設計模型，稱為 ASP.NET Core MVC。 本文示範從 ASP.NET 4.x Web API 遷移至 ASP.NET Core MVC 所需的步驟。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件：

[!INCLUDE [prerequisites](../includes/net-core-prereqs-vs2019-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>審查 ASP.NET 4.x Web API 專案

這篇文章的起點是使用消費者入門中建立的*ProductsApp*專案[ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)。 在該專案中，會依照下列方式設定簡單的 ASP.NET 4.x Web API 專案。

在*Global.asax.cs*中，會對 `WebApiConfig.Register`進行呼叫：

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` 類別可以在*App_Start*資料夾中找到，而且具有靜態 `Register` 方法：

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，雖然它實際上並不會在專案中使用。 它也會設定 ASP.NET Web API 使用的路由表。 在此情況下，ASP.NET 4.x Web API 會預期 Url 符合 `/api/{controller}/{id}`的格式，`{id}` 是選擇性的。

*ProductsApp*專案包含一個控制器。 控制器繼承自 `ApiController` 並包含兩個動作：

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

`ProductsController` 所使用的 `Product` 模型是一個簡單的類別：

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

下列各節示範如何將 Web API 專案遷移至 ASP.NET Core MVC。

## <a name="create-destination-project"></a>建立目的地專案

在 Visual Studio 中，完成下列步驟：

* 前往 **檔案 > ** **新**的 > **專案** > **其他專案類型** > **Visual Studio 方案**。 選取 [**空白方案**]，並將方案命名為*WebAPIMigration*。 按一下 [確定] 按鈕。
* 將現有的*ProductsApp*專案新增至方案。
* 將新的**ASP.NET Core Web 應用程式**專案加入至方案。 從下拉式選單選取 [ **.Net Core**目標 framework]，然後選取 [ **API** ] 專案範本。 將專案命名為*ProductsCore*，然後按一下 [**確定]** 按鈕。

方案現在包含兩個專案。 下列各節說明如何將*ProductsApp*專案的內容遷移至*ProductsCore*專案。

## <a name="migrate-configuration"></a>遷移設定

ASP.NET Core 不會使用*App_Start*資料夾或*global.asax*檔案，而且*web.config*檔案會在發行時加入。 *Startup.cs*是*global.asax*的取代，位於專案根目錄中。 `Startup` 類別會處理所有的應用程式啟動工作。 如需詳細資訊，請參閱<xref:fundamentals/startup>。

在 ASP.NET Core MVC 中，在 `Startup.Configure`中呼叫 <xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*> 時，預設會包含屬性路由。 下列 `UseMvc` 呼叫會取代*ProductsApp*專案的*App_Start/webapiconfig.cs*檔案：

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>遷移模型和控制器

複製*ProductApp*專案的控制器和它所使用的模型。 請依照下列步驟：

1. 將*控制器/productscontroller.cs*從原始專案複製到新的專案。
1. 將整個 [*模型*] 資料夾從原始專案複製到新的專案。
1. 變更所複製檔案的命名空間，使其符合新的專案名稱（*ProductsCore*）。 也請在*ProductsController.cs*中調整 `using ProductsApp.Models;` 語句。

此時，建立應用程式會產生一些編譯錯誤。 因為下列元件不存在於 ASP.NET Core 中，所以會發生錯誤：

* `ApiController` 類別
* `System.Web.Http` 命名空間
* `IHttpActionResult` 介面

修正錯誤，如下所示：

1. 將 `ApiController` 變更為 <xref:Microsoft.AspNetCore.Mvc.ControllerBase>。 加入 `using Microsoft.AspNetCore.Mvc;` 以解析 `ControllerBase` 參考。
1. 刪除 `using System.Web.Http;`。
1. 將 `GetProduct` 動作的傳回型別從 `IHttpActionResult` 變更為 `ActionResult<Product>`。

簡化 `GetProduct` 動作的 `return` 語句，如下所示：

```csharp
return product;
```

## <a name="configure-routing"></a>設定路由

設定路由，如下所示：

1. 將具有下列屬性的 `ProductsController` 類別標記為：

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    前面的[`[Route]`](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)屬性會設定控制器的屬性路由模式。 [`[ApiController]`](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由成為此控制器中所有動作的需求。

    屬性路由支援權杖，例如 `[controller]` 和 `[action]`。 在執行時間，每個權杖會分別取代為已套用屬性之控制器或動作的名稱。 權杖會減少專案中的魔術字串數目。 當套用自動重新命名重構時，權杖也會確保路由與對應的控制器和動作保持同步。
1. 將專案的相容性模式設定為 ASP.NET Core 2.2：

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    先前的變更：

    * 必須使用控制器層級的 `[ApiController]` 屬性。
    * 加入宣告 ASP.NET Core 2.2 中引進的可能中斷行為。
1. 啟用 `ProductController` 動作的 HTTP Get 要求：
    * 將[`[HttpGet]`](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性套用至 `GetAllProducts` 動作。
    * 將 `[HttpGet("{id}")]` 屬性套用至 `GetProduct` 動作。

在上述變更並移除未使用的 `using` 語句之後， *ProductsController.cs*檔案看起來像這樣：

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

執行已遷移的專案，然後流覽至 `/api/products`。 隨即會顯示三個產品的完整清單。 瀏覽至 `/api/products/1`。 第一個產品隨即出現。

## <a name="compatibility-shim"></a>相容性填充碼

[WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫會提供相容性填充碼，以將 ASP.NET 4.X Web API 專案移至 ASP.NET Core。 相容性填充碼會擴充 ASP.NET Core，以支援 ASP.NET 4.x Web API 2 中的一些慣例。 先前在本檔中移植的範例，其基本程度足以使相容性填充碼不必要。 對於較大的專案，使用相容性填充碼有助於暫時橋接 ASP.NET Core 和 ASP.NET 4.x Web API 2 之間的 API 差距。

Web API 相容性填充碼是用來做為暫時的量值，以支援將大型 ASP.NET 4.x Web API 專案遷移至 ASP.NET Core。 經過一段時間，專案應該更新為使用 ASP.NET Core 模式，而不是依賴相容性填充碼。

`Microsoft.AspNetCore.Mvc.WebApiCompatShim` 中包含的相容性功能包括：

* 加入 `ApiController` 類型，讓控制器的基底類型不需要更新。
* 啟用 Web API 樣式的模型系結。 根據預設，ASP.NET Core MVC 模型系結函式類似于 ASP.NET 4.x MVC 5 的功能。 相容性填充碼會將模型系結變更為類似于 ASP.NET 4.x Web API 2 模型系結慣例。 例如，複雜型別會自動系結至要求主體。
* 擴充模型系結，讓控制器動作可以接受 `HttpRequestMessage`類型的參數。
* 加入訊息格式器，允許動作傳回類型 `HttpResponseMessage`的結果。
* 新增 Web API 2 動作可能用來服務回應的其他回應方法：
  * `HttpResponseMessage` 產生器：
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * 動作結果方法：
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* 將 `IContentNegotiator` 的實例新增至應用程式的相依性插入（DI）容器，並提供[WebApi](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)中與內容協商相關的類型。 這類類型的範例包括 `DefaultContentNegotiator` 和 `MediaTypeFormatter`。

若要使用相容性填充碼：

1. 安裝[AspNetCore WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。
1. 藉由呼叫 `Startup.ConfigureServices`中的 `services.AddMvc().AddWebApiConventions()`，向應用程式的 DI 容器註冊相容性填充碼服務。
1. 在應用程式的 `IApplicationBuilder.UseMvc` 呼叫中，使用 `IRouteBuilder` 上的 `MapWebApiRoute` 來定義 Web API 特定的路由。

## <a name="additional-resources"></a>其他資源

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
