---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: 動作會導致 Web API 2 |Microsoft Docs
author: MikeWasson
description: ''
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 7726829ac9eba339ff3ac1c94c86132cb1090783
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37395525"
---
<a name="action-results-in-web-api-2"></a>Web API 2 中的動作結果
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

本主題說明 ASP.NET Web API 將傳回的值轉換從控制器動作至 HTTP 回應訊息的方式。

Web API 控制器動作可以傳回下列其中一項：

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. 其他類型

根據這些傳回時，Web API 會使用不同的機制來建立 HTTP 回應。

| 傳回型別 | Web API 建立回應的方式 |
| --- | --- |
| void | 傳回空 204 （沒有內容） |
| **HttpResponseMessage** | 直接將轉換的 HTTP 回應訊息。 |
| **IHttpActionResult** | 呼叫**ExecuteAsync**來建立**HttpResponseMessage**，然後將轉換為 HTTP 回應訊息。 |
| 其他類型 | 將序列化的傳回值寫入至回應主體中;傳回 200 （確定）。 |

本主題的其餘部分描述更詳細的每個選項。

## <a name="void"></a>void

如果傳回的型別`void`，Web API 只會傳回空的 HTTP 回應狀態碼 204 （沒有內容）。

範例控制器：

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP 回應：

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

如果此動作會傳回[HttpResponseMessage](https://msdn.microsoft.com/library/system.net.http.httpresponsemessage.aspx)，Web API 轉換為傳回值直接 HTTP 回應訊息使用的屬性**HttpResponseMessage**來填入的物件回應。

此選項可讓您控制回應訊息很多。 例如，下列控制器動作設定 Cache-control 標頭。

[!code-csharp[Main](action-results/samples/sample3.cs)]

回應：

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

如果您傳遞至網域模型**CreateResponse**方法中，Web API 會使用[媒體格式器](../formats-and-model-binding/media-formatters.md)寫入回應主體中序列化的模型。

[!code-csharp[Main](action-results/samples/sample5.cs)]

若要選擇的格式器，web API 會使用 Accept 標頭在要求中。 如需詳細資訊，請參閱 <<c0> [ 內容交涉](../formats-and-model-binding/content-negotiation.md)。

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** Web API 2 中引進了介面。 基本上，它會定義**HttpResponseMessage** factory。 以下是使用的一些優點**IHttpActionResult**介面：

- 可簡化[單元測試](../testing-and-debugging/unit-testing-controllers-in-web-api.md)控制器。
- 將移到另一個類別建立 HTTP 回應的一般邏輯。
- 藉由隱藏建構回應的低層級的詳細資料可更清楚，控制器動作的意圖。

**IHttpActionResult**包含單一方法**ExecuteAsync**，以非同步方式建立**HttpResponseMessage**執行個體。

[!code-csharp[Main](action-results/samples/sample6.cs)]

如果控制器動作傳回**IHttpActionResult**，Web API 會呼叫**ExecuteAsync**方法，以建立**HttpResponseMessage**。 然後它會將轉換**HttpResponseMessage**至 HTTP 回應訊息。

以下是簡單的實作的**IHttpActionResult**所建立的純文字回應：

[!code-csharp[Main](action-results/samples/sample7.cs)]

範例控制器動作：

[!code-csharp[Main](action-results/samples/sample8.cs)]

回應：

[!code-console[Main](action-results/samples/sample9.cmd)]

通常您會使用**IHttpActionResult**中所定義的實作**[System.Web.Http.Results](https://msdn.microsoft.com/library/system.web.http.results.aspx)** 命名空間。 **ApiController**類別定義會傳回這些內建動作結果的 helper 方法。

在下列範例中，如果要求不符合現有的產品識別碼，控制器會呼叫[ApiController.NotFound](https://msdn.microsoft.com/library/system.web.http.apicontroller.notfound.aspx)建立 404 （找不到） 回應。 否則，控制器會呼叫[ApiController.OK](https://msdn.microsoft.com/library/dn314591.aspx)，這會建立 200 （確定） 回應，包含產品。

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>其他傳回型別

對於所有其他傳回類型，Web API 會使用[媒體格式器](../formats-and-model-binding/media-formatters.md)序列化傳回的值。 Web API 會寫入回應主體中序列化的值。 回應狀態碼為 200 （確定）。

[!code-csharp[Main](action-results/samples/sample11.cs)]

這種方法的缺點是您不能直接傳回錯誤碼 404 等。 不過，您可以擲回**HttpResponseException**錯誤代碼。 如需詳細資訊，請參閱 < [ASP.NET Web API 中的例外狀況處理](../error-handling/exception-handling.md)。

若要選擇的格式器，web API 會使用 Accept 標頭在要求中。 如需詳細資訊，請參閱 <<c0> [ 內容交涉](../formats-and-model-binding/content-negotiation.md)。

範例要求

[!code-console[Main](action-results/samples/sample12.cmd)]

範例回應：

[!code-console[Main](action-results/samples/sample13.cmd)]
