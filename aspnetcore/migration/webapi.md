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
# <a name="migrate-from-aspnet-web-api-to-aspnet-core"></a>從 ASP.NET Web API 移轉至 ASP.NET Core

作者：[Steve Smith](https://ardalis.com/) 和 [Scott Addie](https://scottaddie.com)

Web Api 是 HTTP 服務並擴及各種用戶端，包括瀏覽器和行動裝置。 ASP.NET Core MVC 包括建置 Web Api 提供單一且一致的方式建置 web 應用程式的支援。 在本文中，我們會示範將從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC 所需的步驟。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>檢閱 ASP.NET Web API 專案

這篇文章會使用範例專案*ProductsApp*一文中建立[Getting Started with ASP.NET Web API 2](/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)做為起點。 在該專案中，簡單的 ASP.NET Web API 專案設定，如下所示。

在  *Global.asax.cs*，進行呼叫`WebApiConfig.Register`:

[!code-csharp[](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig` 定義於*App_Start*，且具有一個靜態`Register`方法：

[!code-csharp[](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]

這個類別會設定[屬性路由](/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上並不是專案中使用。 它也會設定路由表，由 ASP.NET Web API。 在此情況下，ASP.NET Web API 就會預期收到符合格式的 Url */api/ {controller} / {id}*，以 *{id}* 為選擇性。

*ProductsApp*專案包含一個簡單的控制器，繼承自`ApiController`和公開兩種方法：

[!code-csharp[](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

最後，模型中，*產品*中，由*ProductsApp*，是一個簡單的類別：

[!code-csharp[](webapi/sample/ProductsApp/Models/Product.cs)]

既然我們已經有一個簡單的專案，我們可以從該處開始，會示範如何將此 Web API 專案移轉至 ASP.NET Core MVC。

## <a name="create-the-destination-project"></a>建立目的地專案

使用 Visual Studio，建立新的空白方案，並將它命名*WebAPIMigration*。 加入現有*ProductsApp* ，專案，然後將新的 ASP.NET Core Web 應用程式專案加入方案。 將新專案命名*ProductsCore*。

![新增專案 對話方塊開啟至 Web 範本](webapi/_static/add-web-project.png)

接下來，選擇 Web API 專案範本。 我們將會移轉*ProductsApp*這個新專案的內容。

![使用 ASP.NET Core 範本清單中選取的 Web API 專案範本的新 Web 應用程式 對話方塊](webapi/_static/aspnet-5-webapi.png)

刪除`Project_Readme.html`來自新的專案檔。 您的方案現在看起來應該像這樣：

![在 [方案總管] 中顯示檔案和資料夾的 ProductsApp 和 ProductsCore 專案中開啟的應用程式方案](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>移轉組態

ASP.NET Core 不會再使用*Global.asax*， *web.config*，或*App_Start*資料夾。 相反地，完成所有啟動工作*Startup.cs*專案的根目錄中 (請參閱[應用程式啟動](../fundamentals/startup.md))。 在 ASP.NET Core MVC 中，屬性為基礎的路由現在預設包含當`UseMvc()`稱為;，這是建議的方法，來設定 Web API 路由 （也是 Web API 入門專案如何處理路由）。

[!code-csharp[](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=31)]

假設您想要使用您從現在開始的專案中的屬性路由，不需要進行其他設定。 只需套用的屬性，視您的控制器和動作，如同您在此範例`ValuesController`Web API 入門專案中包含的類別：

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

請注意出現與否 *[controller]* 第 8 行上。 屬性為基礎的路由現在支援特定的權杖，例如 *[controller]* 並 *[動作]*。 這些語彙基元會取代在執行階段的控制器或動作，名稱分別屬性套用。 這是用來減少的魔術字串，在專案中，並且確保路由將會保留其相對應的控制器和動作與同步時套用自動重新命名重構。

若要移轉產品的 API 控制器，我們必須先複製*ProductsController*至新的專案。 然後只要加入控制器的路由屬性：

```csharp
[Route("api/[controller]")]
```

您也需要新增`[HttpGet]`屬性設定為兩種方法，因為它們都應該透過 HTTP Get 呼叫。 包含之屬性中的 「 識別碼 」 參數的預期`GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

此時，路由已正確設定;不過，我們無法尚未進行測試。 必須進行其他變更，再*ProductsController*會進行編譯。

## <a name="migrate-models-and-controllers"></a>移轉模型和控制器

這個簡單的 Web API 專案的移轉程序的最後一個步驟是將複製的控制站和它們所使用的任何模型。 在此情況下，只要複製*Controllers/ProductsController.cs*從原始到新專案。 然後，將整個 Models 資料夾從原始專案複製到新的。 調整以符合新的專案名稱的命名空間 (*ProductsCore*)。  此時，您可以建立應用程式，，而且您會找到多個編譯錯誤。 這些通常應該分為下列類別：

* *ApiController*不存在

* *System.Web.Http*命名空間不存在

* *IHttpActionResult*不存在

幸運的是，這些是所有很容易修正：

* 變更*ApiController*要*控制站*(您可能需要新增*使用 Microsoft.AspNetCore.Mvc*)

* 刪除任何使用陳述式參考*System.Web.Http*

* 變更任何方法傳回*IHttpActionResult*返回*IActionResult*

一旦這些變更已建立且未使用 using 陳述式移除，移轉*ProductsController*類別看起來像這樣：

[!code-csharp[](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

您現在應該能夠執行已移轉的專案，並瀏覽至 */api/產品*; 而且，您應該會看到 3 個產品的完整清單。 瀏覽至 */api/products/1* ，您應該會看到第一項產品。

## <a name="aspnet-4x-web-api-2-compatibility-shim"></a>ASP.NET 4.x Web API 2 相容性填充碼

移轉 ASP.NET Web API 專案至 ASP.NET Core 時很有用的工具是[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim)程式庫。 相容性填充碼會擴充 ASP.NET Core，以允許要使用不同的 Web API 2 慣例的數目。 本文件中先前移轉的範例是基本相容性填充碼並非必要項目。 針對大型專案，在使用相容性填充碼可用於暫時之間溝通落差 API ASP.NET Core 和 ASP.NET Web API 2。

Web API 相容性填充碼是要作為暫時的措施來協助移轉至 ASP.NET Core 的大型 Web API 專案。 經過一段時間，您應該更新專案，以使用 ASP.NET Core 的模式，而不是依賴相容性填充碼。

中包含的相容性功能`Microsoft.AspNetCore.Mvc.WebApiCompatShim`包括：

* 新增`ApiController`類型，以便控制站的基底型別不需要更新。
* 可讓 Web API 式模型繫結。 ASP.NET Core MVC 模型繫結的功能類似於預設的 MVC 5。 相容性填充碼變更的模型繫結更類似於 Web API 2 模型繫結的慣例。 例如，複雜型別會自動繫結從要求主體中。
* 擴充模型繫結，以便可以採取的型別參數的控制器動作`HttpRequestMessage`。
* 新增訊息格式器允許的動作，以傳回結果的型別`HttpResponseMessage`。
* 新增其他回應 Web API 2 動作可能已用來處理回應的方法：
  * HttpResponseMessage 產生器：
    * `CreateResponse<T>`
    * `CreateErrorResponse`
  * 動作結果的方法：
    * `BadRequestErrorMessageResult`
    * `ExceptionResult`
    * `InternalServerErrorResult`
    * `InvalidModelStateResult`
    * `NegotiatedContentResult`
    * `ResponseMessageResult`
* 將加入的執行個體`IContentNegotiator`至應用程式的 DI 容器和進行內容交涉相關的類型，從[Microsoft.AspNet.WebApi.Client](https://www.nuget.org/packages/Microsoft.AspNet.WebApi.Client/)可用。 這包括像是類型`DefaultContentNegotiator`，`MediaTypeFormatter`等等。

若要使用相容性填充碼，您需要：

* 安裝[Microsoft.AspNetCore.Mvc.WebApiCompatShim](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.WebApiCompatShim) NuGet 套件。
* 向應用程式的 DI 容器中的相容性填充碼的服務，藉由呼叫`services.AddMvc().AddWebApiConventions()`應用程式的`Startup.ConfigureServices`方法。
* 定義使用的 Web API 特有路由`MapWebApiRoute`上`IRouteBuilder`在應用程式的`IApplicationBuilder.UseMvc`呼叫。

## <a name="summary"></a>總結

簡單的 ASP.NET Web API 專案移轉至 ASP.NET Core MVC 是相當直覺，感謝您 Web Api ASP.NET Core MVC 中的內建支援。 每個 ASP.NET Web API 專案必須移轉的主要部分會是路由、 控制器及模型，以及更新至控制器和動作所使用的類型。
