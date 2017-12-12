---
uid: web-api/overview/error-handling/exception-handling
title: "ASP.NET Web API 的例外狀況處理 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/12/2012
ms.topic: article
ms.assetid: cbebeb37-2594-41f2-b71a-f4f26520d512
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/error-handling/exception-handling
msc.type: authoredcontent
ms.openlocfilehash: c65ddcca012840d70ab5a33af92edb30041be971
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="exception-handling-in-aspnet-web-api"></a>ASP.NET Web API 的例外狀況處理
====================
由[Mike Wasson](https://github.com/MikeWasson)

本文說明錯誤和例外狀況處理中 ASP.NET Web API。

- [HttpResponseException](#httpresponserexception)
- [例外狀況篩選條件](#exception_filters)
- [註冊例外狀況篩選條件](#registering_exception_filters)
- [HttpError](#httperror)

<a id="httpresponserexception"></a>
## <a name="httpresponseexception"></a>HttpResponseException

如果 Web API 控制器會擲回無法攔截的例外狀況會怎樣？ 根據預設，大部分的例外狀況會轉譯為 HTTP 回應狀態碼 500 內部伺服器錯誤。

**HttpResponseException**型別是特殊案例。 這個例外狀況傳回您指定的例外狀況建構函式中的任何 HTTP 狀態碼。 例如，下列方法會傳回 404 找不到，如果*識別碼*參數無效。

[!code-csharp[Main](exception-handling/samples/sample1.cs)]

如需更多控制回應的詳細資訊，也可以建構整個回應訊息和將它與包含**HttpResponseException:** 

[!code-csharp[Main](exception-handling/samples/sample2.cs)]

<a id="exception_filters"></a>
## <a name="exception-filters"></a>例外狀況篩選條件

您可以自訂 Web 應用程式開發介面撰寫所處理的例外狀況*例外狀況篩選條件*。 例外狀況篩選條件執行控制器方法擲回任何未處理的例外狀況時*不* **HttpResponseException**例外狀況。 **HttpResponseException**型別是特殊案例中，因為它設計為傳回 HTTP 回應。

例外狀況篩選條件實作**System.Web.Http.Filters.IExceptionFilter**介面。 撰寫例外狀況篩選條件的最簡單方式是衍生自**System.Web.Http.Filters.ExceptionFilterAttribute**類別並覆寫**OnException**方法。

> [!NOTE]
> ASP.NET Web API 中的例外狀況篩選條件是 ASP.NET MVC 中類似的項目。 不過，它們是不同的命名空間和函式中個別宣告。 特別是， **HandleErrorAttribute**在 MVC 中使用的類別不會處理由 Web API 控制器會擲回的例外狀況。


以下是將篩選**NotImplementedException** HTTP 狀態例外狀況程式碼 501，未實作：

[!code-csharp[Main](exception-handling/samples/sample3.cs)]

**回應**屬性**HttpActionExecutedContext**物件包含 HTTP 回應訊息將傳送至用戶端。

<a id="registering_exception_filters"></a>
## <a name="registering-exception-filters"></a>註冊例外狀況篩選條件

有幾種方式可以註冊 Web 應用程式開發介面的例外狀況篩選條件：

- 動作
- 控制站
- 全域

若要將篩選套用至特定的動作，新增的屬性篩選器動作：

[!code-csharp[Main](exception-handling/samples/sample4.cs)]

將篩選套用到所有的控制站上的動作，加入篩選條件做為屬性加入控制器類別：

[!code-csharp[Main](exception-handling/samples/sample5.cs)]

將篩選套用到全域所有的 Web API 控制器，加入篩選，以執行個體**GlobalConfiguration.Configuration.Filters**集合。 在此集合中的例外狀況篩選會套用至任何 Web API 控制器動作。

[!code-csharp[Main](exception-handling/samples/sample6.cs)]

如果您使用 「 ASP.NET MVC 4 Web 應用程式 」 專案範本來建立專案時，將 Web API 組態程式碼內`WebApiConfig`類別，位於應用程式\_開始資料夾：

[!code-csharp[Main](exception-handling/samples/sample7.cs?highlight=5)]

<a id="httperror"></a>
## <a name="httperror"></a>HttpError

**HttpError**物件提供一致的方式回應主體中傳回的錯誤訊息。 下列範例會示範如何傳回 HTTP 狀態碼 404 （找不到） 與**HttpError**回應主體中。

[!code-csharp[Main](exception-handling/samples/sample8.cs)]

**CreateErrorResponse**擴充方法定義中**System.Net.Http.HttpRequestMessageExtensions**類別。 就內部而言， **CreateErrorResponse**建立**HttpError**執行個體，然後建立**HttpResponseMessage**包含**HttpError**.

在此範例中，如果該方法成功，它會傳回 HTTP 回應中的產品。 但如果找不到要求的產品，HTTP 回應將包含**HttpError**要求主體中。 回應可能如下所示：

[!code-console[Main](exception-handling/samples/sample9.cmd)]

請注意， **HttpError**已序列化為 JSON，在此範例中。 使用的其中一個優點**HttpError**是它會進行相同[內容交涉](../formats-and-model-binding/content-negotiation.md)和序列化處理任何其他的強型別模型。

### <a name="httperror-and-model-validation"></a>HttpError 與模型驗證

模型驗證，您可以傳遞至模型狀態**CreateErrorResponse**，若要在回應中包含的驗證錯誤：

[!code-csharp[Main](exception-handling/samples/sample10.cs)]

此範例中，可能會傳回下列回應：

[!code-console[Main](exception-handling/samples/sample11.cmd)]

如需模型驗證的詳細資訊，請參閱[ASP.NET Web API 中的模型驗證](../formats-and-model-binding/model-validation-in-aspnet-web-api.md)。

### <a name="using-httperror-with-httpresponseexception"></a>使用 HttpResponseException HttpError

前一個範例傳回**HttpResponseMessage**訊息從控制器動作，但您也可以使用**HttpResponseException**傳回**HttpError**。 這可讓您在一般的成功情況中，傳回強型別模型，而仍可傳回**HttpError**如果發生錯誤：

[!code-csharp[Main](exception-handling/samples/sample12.cs)]
