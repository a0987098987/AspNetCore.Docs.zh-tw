---
title: "從 ASP.NET Web API 移轉"
author: ardalis
description: 
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 4f0564b4-ed4e-4e1e-9755-c1144d21a0ef
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/webapi
ms.openlocfilehash: 4acb7ccf7f944df5d08ac7faa342f0c72cf9d1a7
ms.sourcegitcommit: 732cd2684246e49e796836596643a8d37e20c46d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/01/2017
---
# <a name="migrating-from-aspnet-web-api"></a>從 ASP.NET Web API 移轉

由[Steve Smith](https://ardalis.com/)和[Scott Addie](https://scottaddie.com)

Web 應用程式開發介面為連線用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。 ASP.NET Core MVC 包括建置 Web 應用程式開發介面提供單一且一致的方式，建立 web 應用程式的支援。 在本文中，我們會示範從 ASP.NET Web API 的 Web API 實作移轉至 ASP.NET Core MVC 所需的步驟。

[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/migration/webapi/sample)([如何下載](xref:tutorials/index#how-to-download-a-sample))

## <a name="review-aspnet-web-api-project"></a>檢閱 ASP.NET Web API 專案

本文使用範例專案， *ProductsApp*建立發行項的[開始使用 ASP.NET Web API](https://docs.microsoft.com/aspnet/web-api/overview/getting-started-with-aspnet-web-api/tutorial-your-first-web-api)做為起點。 在該專案中，簡單的 ASP.NET Web API 專案設定，如下所示。

在*Global.asax.cs*，進行呼叫以`WebApiConfig.Register`:

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Global.asax.cs?highlight=14)]

`WebApiConfig`定義於*App_Start*，且具有一個靜態`Register`方法：

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/App_Start/WebApiConfig.cs?highlight=15,16,17,18,19,20)]


這個類別會設定[屬性路由](https://docs.microsoft.com/aspnet/web-api/overview/web-api-routing-and-actions/attribute-routing-in-web-api-2)，但實際上不會在專案中使用。 它也會設定 ASP.NET Web API 所使用的路由表。 在此情況下，ASP.NET Web 應用程式開發介面會預期以符合格式的 Url */api/ {controller} / {id}*，與*{id}*為選擇性。

*ProductsApp*專案包含一個簡單的控制器，繼承自`ApiController`和公開兩個方法：

[!code-csharp[Main](../migration/webapi/sample/ProductsApp/Controllers/ProductsController.cs?highlight=19,24)]

最後，模型*產品*中，由*ProductsApp*，是簡單的類別：

[!code-csharp[Main](webapi/sample/ProductsApp/Models/Product.cs)]

現在，我們有簡單的專案開始，我們可以示範如何將這個 Web API 專案移轉至 ASP.NET Core MVC。

## <a name="create-the-destination-project"></a>建立目的專案

使用 Visual Studio，建立新的空白方案，並將它命名*WebAPIMigration*。 加入現有*ProductsApp*專案，然後將新的 ASP.NET Core Web 應用程式專案加入方案。 將新專案*ProductsCore*。

![網站範本來開啟 [新增專案] 對話方塊](webapi/_static/add-web-project.png)

接下來，選擇 Web API 專案範本。 我們將會移轉*ProductsApp*內容到這個新的專案。

![ASP.NET Core 範本清單中選取的 Web API 的專案範本的新 Web 應用程式對話方塊](webapi/_static/aspnet-5-webapi.png)

刪除`Project_Readme.html`檔案從新的專案。 您的方案現在看起來應該像這樣：

![在 [方案總管] 中顯示檔案和資料夾的 ProductsApp 和 ProductsCore 專案中開啟應用程式方案](webapi/_static/webapimigration-solution.png)

## <a name="migrate-configuration"></a>移轉設定

不會再使用 ASP.NET Core *Global.asax*， *web.config*，或*App_Start*資料夾。 相反地，完成所有啟動工作*Startup.cs*專案的根目錄中 (請參閱[應用程式啟動](../fundamentals/startup.md))。 在 ASP.NET Core MVC 中，屬性為基礎的路由現在會包含預設時`UseMvc()`稱為; 而且，這是建議的方法來設定 Web API 路由 （而且是 Web API 入門專案處理路由的方式）。

[!code-none[Main](../migration/webapi/sample/ProductsCore/Startup.cs?highlight=40)]

假設您想要使用屬性路由從現在開始在專案中，不需要進行其他設定。 只需套用的屬性視您的控制器和動作，如同您在此範例`ValuesController`Web API 的入門專案中包含的類別：

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ValuesController.cs?highlight=9,13,20,27,33,39)]

請注意是否存在*[控制器]*第 8 行上。 屬性為基礎的路由現在支援特定語彙基元，例如*[控制器]*和*[動作]*。 這些語彙基元會取代在執行階段的控制器或動作，名稱分別屬性套用。 這是用來減少的識別字串，在專案中，並且確保路由將會保留其對應的控制器和動作與同步處理時自動重新命名重構功能，就會套用。

若要移轉的產品的 API 控制器，我們必須先將複製*ProductsController*到新的專案。 然後只需要包含在控制器上的路由屬性：

```csharp
[Route("api/[controller]")]
```

您也需要加入`[HttpGet]`屬性設定為兩種方法，因為它們都應該會透過 HTTP Get 呼叫。 針對屬性中包含的 「 識別碼 」 參數的預期`GetProduct()`:

```csharp
// /api/products
[HttpGet]
...

// /api/products/1
[HttpGet("{id}")]
```

此時，路由已正確設定。不過，我們無法尚未測試它。 必須先進行其他變更*ProductsController*會進行編譯。

## <a name="migrate-models-and-controllers"></a>移轉模型和控制站

這個簡單的 Web API 專案的移轉程序的最後一個步驟是複製到控制器，他們使用的任何模型。 在此情況下，只需複製*Controllers/ProductsController.cs*從原始專案到新的專案。 然後，將整個模型資料夾從原始專案複製到新的專案。 調整以符合新的專案名稱的命名空間 (*ProductsCore*)。  此時，您可以建置應用程式，而您會發現多個編譯錯誤。 這些通常應該可分成下列類別：

* *ApiController*不存在

* *System.Web.Http*命名空間不存在

* *IHttpActionResult*不存在

幸運的是，這些是所有很容易修正：

* 變更*ApiController*至*控制器*(您可能需要加入*使用 Microsoft.AspNetCore.Mvc*)

* 刪除任何使用陳述式參考*System.Web.Http*

* 變更任何方法傳回*IHttpActionResult*傳回*IActionResult*

一旦這些變更都已和未使用 using 陳述式移除，移轉*ProductsController*類別看起來像這樣：

[!code-csharp[Main](../migration/webapi/sample/ProductsCore/Controllers/ProductsController.cs?highlight=1,2,6,8,9,27)]

您現在應該能夠執行移轉的專案，並瀏覽至*/api/產品*; 而且，您應該會看到 3 產品的完整清單。 瀏覽至*/api/products/1*和您應該會看到第一次的產品。

## <a name="summary"></a>總結

將簡單的 ASP.NET Web API 專案移轉至 ASP.NET Core MVC 就相當簡單，這適用於 ASP.NET Core MVC 中 Web Api 的內建支援。 每個 ASP.NET Web API 專案必須移轉的主要部分是路由、 控制器、 和模型，以及更新至控制器和動作所使用的類型。
