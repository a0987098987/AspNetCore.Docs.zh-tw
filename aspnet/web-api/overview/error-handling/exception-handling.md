---
uid: web-api/overview/error-handling/exception-handling
title: ASP.NET Web API 中處理的例外狀況 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
ms.date: 03/12/2012
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: 2db00c1ab241e0dad051c2a3bcd3b0e4bbf4d5c4
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37807657"
---
<a name="exception-handling-in-aspnet-web-api"></a>ASP.NET Web API 中處理的例外狀況
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本文說明錯誤和 ASP.NET Web API 中處理的例外狀況。

- [HttpResponseException](#httpresponserexception)
- [例外狀況篩選條件](#exception_filters)
- [註冊的例外狀況篩選條件](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

如果 Web API 控制器會擲回未攔截到例外狀況，會發生什麼事？ 根據預設，大部分的例外狀況會轉譯成 HTTP 回應狀態碼 500 內部伺服器錯誤。

**HttpResponseException**型別是特殊案例。 這個例外狀況傳回您指定的例外狀況的建構函式中的任何 HTTP 狀態碼。 例如，下列方法會傳回 404，找不到，如果*識別碼*參數無效。

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

進一步控制回應的詳細資訊，您可以也建構整個回應訊息並將它包含與**HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>例外狀況篩選條件

您可以自訂 Web API 撰寫所處理的例外狀況*例外狀況篩選條件*。 控制器方法擲回任何未處理例外狀況時，會執行例外狀況篩選條件*未* **HttpResponseException**例外狀況。 **HttpResponseException**型別是特殊案例，因為它專為傳回 HTTP 回應。

例外狀況篩選條件實作**System.Web.Http.Filters.IExceptionFilter**介面。 撰寫例外狀況篩選條件的最簡單方式是衍生自**System.Web.Http.Filters.ExceptionFilterAttribute**類別並覆寫**OnException**方法。

> [!NOTE]
> ASP.NET Web API 中的例外狀況篩選條件會類似於 ASP.NET MVC 中的項目。 不過，它們是不同的命名空間和函式中個別宣告。 特別是， **HandleErrorAttribute**在 MVC 中使用的類別不會處理 Web API 控制器所擲回的例外狀況。


以下是將篩選條件**NotImplementedException**例外狀況變成 HTTP 狀態碼 501，未實作：

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**回應**屬性**HttpActionExecutedContext**物件包含將傳送至用戶端的 HTTP 回應訊息。

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>註冊的例外狀況篩選條件

有數種方式來註冊 Web API 例外狀況篩選條件：

- 由動作
- 控制站
- 全域

若要將篩選套用至特定的動作，新增的屬性篩選條件的動作：

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

若要將篩選套用至所有控制站上的動作，新增的屬性篩選條件至控制器類別：

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

若要將篩選全域套用至所有的 Web API 控制器，將新增的篩選，以執行個體**GlobalConfiguration.Configuration.Filters**集合。 在此集合中的例外狀況篩選會套用至任何 Web API 控制器動作。

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將您的 Web API 組態程式碼內`WebApiConfig`類別，其位於應用程式\_開始資料夾：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError**物件提供一致的方式，回應主體中傳回錯誤資訊。 下列範例會示範如何傳回 HTTP 狀態碼 404 （找不到） 與**HttpError**回應主體中。

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse**擴充方法定義於**System.Net.Http.HttpRequestMessageExtensions**類別。 就內部而言， **CreateErrorResponse**會建立**HttpError**執行個體，然後建立**HttpResponseMessage**包含**HttpError**.

在此範例中，如果方法成功，它會傳回 HTTP 回應中的產品。 但如果找不到要求的產品，HTTP 回應會包含**HttpError**要求主體中。 回應看起來可能如下所示：

[!code-console[Main](exception-handling/samples/sample9.cmd)]

請注意， **HttpError**已序列化為 JSON，在此範例中。 使用的優點之一**HttpError**是它會通過相同[內容交涉](../formats-and-model-binding/content-negotiation.md)和序列化處理任何其他的強型別模型。

### <a name="httperror-and-model-validation"></a>HttpError 和驗證模型

驗證模型，您可以傳遞至模型狀態**CreateErrorResponse**，以便在回應中包含的驗證錯誤：

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

此範例中，可能會傳回下列回應：

[!code-console[Main](exception-handling/samples/sample11.cmd)]

如需有關模型驗證的詳細資訊，請參閱 < [ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。

### <a name="using-httperror-with-httpresponseexception"></a>使用 HttpResponseException HttpError

前一個範例會傳回**HttpResponseMessage**訊息從控制器動作，但您也可以使用**HttpResponseException**返回**HttpError**。 這可讓您在一般的成功的案例中，傳回強型別模型，而仍可傳回**HttpError**如果發生錯誤：

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
