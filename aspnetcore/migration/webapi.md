---
title: 從 ASP.NET Web API 移轉至 ASP.NET Core
author: ardalis
description: 了解如何從 ASP.NET 4.x Web API 的 web API 實作移轉至 ASP.NET Core MVC。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/10/2018
uid: migration/webapi
ms.openlocfilehash: ea14b7128582a21b36c70041d59fb638eebdbef0
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895005"
---
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>從 ASP.NET Web API 移轉至 ASP.NET Core

藉由[Scott Addie](https://twitter.com/scott_addie)和[Steve Smith](https://ardalis.com/)

ASP.NET 4.x Web API 是一種 HTTP 服務，達到各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Core 可整合 ASP.NET 4.x 的 MVC 和 Web API 應用程式模型到簡單的程式設計模型，稱為 ASP.NET Core MVC。 本文示範如何從 ASP.NET 4.x Web API 移轉至 ASP.NET Core MVC 所需的步驟。

[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:index#how-to-download-a-sample))

## <a name="prerequisites"></a>必要條件

[!INCLUDE [net-core-prereqs-vs-2.2](../includes/net-core-prereqs-vs-2.2.md)]

## <a name="review-aspnet-4x-web-api-project"></a>檢閱 ASP.NET 4.x Web API 專案

本文章使用做為起點*ProductsApp*中建立專案[Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)。 在該專案中，簡單的 ASP.NET 4.x Web API 專案設定，如下所示。

在  *Global.asax.cs*，進行呼叫`WebApiConfig.Register`:

[!code-csharp[](webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig`類別位於*App_Start*資料夾和擁有的靜態`Register`方法：

[!code-csharp[](webapi/sample/ProductsApp/App_Start/WebApiConfig.cs)]

這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不是專案中使用。 它也會設定路由表，由 ASP.NET Web API。 在此情況下，ASP.NET 4.x Web API 必須要有符合格式的 Url `/api/{controller}/{id}`，使用`{id}`為選擇性。

*ProductsApp*專案包含一個控制站。 控制器會繼承`ApiController`且包含兩個動作：

[!code-csharp[](webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=28,33)]

`Product`所使用的模型`ProductsController`是簡單的類別：

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

下列各節示範移轉到 ASP.NET Core MVC Web API 專案。

## <a name="create-destination-project"></a>建立目的專案

完成在 Visual Studio 中的下列步驟：

* 移至**檔案** > **新** > **專案** > **其他專案類型** > **Visual Studio 方案**。 選取 **空白方案**，再將方案命名*WebAPIMigration*。 按一下 [**確定**] 按鈕。
* 加入現有*ProductsApp*專案加入方案。
* 加入新**ASP.NET Core Web 應用程式**專案加入方案。 選取  **.NET Core**下拉式清單中，從 framework 為目標，然後選取**API**專案範本。 將專案命名為*ProductsCore*，然後按一下**確定** 按鈕。

此方案現在包含兩個專案。 下列各節說明移轉*ProductsApp*專案的內容，以*ProductsCore*專案。

## <a name="migrate-configuration"></a>移轉組態

不會使用 ASP.NET Core *App_Start*資料夾或*Global.asax*檔案，而*web.config*檔案會新增在發行時。 *Startup.cs*會取代*Global.asax*而且位於專案根目錄中。 `Startup`類別會處理所有的應用程式啟動工作。 如需詳細資訊，請參閱 <xref:fundamentals/startup>。

依預設包含 ASP.NET Core MVC 中，屬性路由時<xref:Microsoft.AspNetCore.Builder.MvcApplicationBuilderExtensions.UseMvc*>呼叫`Startup.Configure`。 下列`UseMvc`呼叫取代*ProductsApp*專案組*app_start/webapiconfig.cs*檔案：

[!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_Configure&highlight=13])]

## <a name="migrate-models-and-controllers"></a>移轉模型和控制器

透過複製*ProductApp*專案的控制器和它所使用的模型。 請依照下列步驟：

1. 複製*Controllers/ProductsController.cs*從原始到新專案。
1. 複製整個*模型*從原始專案到新的資料夾。
1. 變更已複製的檔案命名空間，以符合新的專案名稱 (*ProductsCore*)。 調整`using ProductsApp.Models;`中的陳述式*ProductsController.cs*太。

此時，建置應用程式會導致編譯錯誤數目。 因為下列元件不存在於 ASP.NET Core，就會發生錯誤：

* `ApiController` 類別
* `System.Web.Http` 命名空間
* `IHttpActionResult` 介面

請修正錯誤，如下所示：

1. 變更`ApiController`至<xref:Microsoft.AspNetCore.Mvc.ControllerBase>。 新增`using Microsoft.AspNetCore.Mvc;`若要解決`ControllerBase`參考。
1. 刪除 `using System.Web.Http;`。
1. 變更`GetProduct`動作的傳回類型從`IHttpActionResult`至`ActionResult<Product>`。

簡化`GetProduct`動作的`return`如下的陳述式：

```csharp
return product;
```

## <a name="configure-routing"></a>設定路由

設定路由，如下所示：

1. 裝飾`ProductsController`類別具有下列屬性：

    ```csharp
    [Route("api/[controller]")]
    [ApiController]
    ```

    上述[[路由]](xref:Microsoft.AspNetCore.Mvc.RouteAttribute)屬性會設定控制器的屬性路由的模式。 [[ApiController]](xref:Microsoft.AspNetCore.Mvc.ApiControllerAttribute)屬性會讓屬性路由此控制器中的所有動作的需求。

    屬性路由支援權杖，例如`[controller]`和`[action]`。 在執行階段，每個語彙基元會取代控制器或動作，名稱分別屬性套用。 權杖會減少專案中的魔術字串數目。 路由與保持同步的相對應的控制站，並套用時自動重新命名重構的動作，也請確定語彙基元。
1. 專案的相容性模式設定為 ASP.NET Core 2.2:

    [!code-csharp[](webapi/sample/ProductsCore/Startup.cs?name=snippet_ConfigureServices&highlight=4)]

    上述的變更：

    * 需要使用`[ApiController]`在控制器層級的屬性。
    * 選擇加入可能最新 ASP.NET Core 2.2 中所導入行為。
1. 啟用 HTTP Get 要求以`ProductController`動作：
    * 適用於[[HttpGet]](xref:Microsoft.AspNetCore.Mvc.HttpGetAttribute)屬性設定為`GetAllProducts`動作。
    * 適用於`[HttpGet("{id}")]`屬性設定為`GetProduct`動作。

在前述的變更和移除未使用之後`using`陳述式， *ProductsController.cs*檔案看起來像這樣：

[!code-csharp[](webapi/sample/ProductsCore/Controllers/ProductsController.cs)]

執行移轉的專案，並瀏覽至`/api/products`。 顯示三個產品的完整清單。 瀏覽至 `/api/products/1`。 第一項產品會出現。

## <a name="compatibility-shim"></a>相容性填充碼

[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫提供相容性填充碼移動到 ASP.NET Core 的 ASP.NET 4.x Web API 專案。 相容性填充碼會擴充 ASP.NET Core 以支援一些從 ASP.NET 4.x Web API 2 的慣例。 本文件中先前移轉的範例是基本相容性填充碼是不必要的。 針對大型專案，在使用相容性填充碼可用於暫時之間溝通落差 API ASP.NET Core 和 ASP.NET 4.x Web API 2。

Web API 相容性填充碼是要作為暫時的措施來支援大型 ASP.NET 4.x Web API 專案移轉至 ASP.NET Core。 經過一段時間，您應該更新專案，以使用 ASP.NET Core 的模式，而不是依賴相容性填充碼。

中包含的相容性功能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`包括：

* 新增`ApiController`類型，以便控制站的基底型別不需要更新。
* 可讓 Web API 式模型繫結。 ASP.NET Core MVC 模型繫結的功能，類似於 ASP.NET 的預設的 MVC 5，4.x。 相容性填充碼變更的模型繫結更類似於 ASP.NET 4.x Web API 2 模型繫結慣例。 例如，複雜型別會自動繫結從要求主體中。
* 擴充模型繫結，以便可以採取的型別參數的控制器動作`HttpRequestMessage`。
* 新增訊息格式器允許的動作，以傳回結果的型別`HttpResponseMessage`。
* 新增其他回應 Web API 2 動作可能已用來處理回應的方法：
  * `HttpResponseMessage` 產生器：
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * 動作結果的方法：
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* 將加入的執行個體`IContentNegotiator`應用程式的相依性插入 (DI) 容器，並提供內容的交涉相關型別從[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)。 這種類型的範例包括`DefaultContentNegotiator`和`MediaTypeFormatter`。

若要使用相容性填充碼：

1. 安裝[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。
1. 向應用程式的 DI 容器中的相容性填充碼的服務，藉由呼叫`services.AddMvc().AddWebApiConventions()`在`Startup.ConfigureServices`。
1. 定義的 web API 特定路由使用`MapWebApiRoute`上`IRouteBuilder`在應用程式的`IApplicationBuilder.UseMvc`呼叫。

## <a name="additional-resources"></a>其他資源

* <xref:web-api/index>
* <xref:web-api/action-return-types>
* <xref:mvc/compatibility-version>
