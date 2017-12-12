---
uid: web-api/overview/getting-started-with-aspnet-web-api/action-results
title: "動作會導致 Web API 2 |Microsoft 文件"
author: MikeWasson
description: 
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/03/2014
ms.topic: article
ms.assetid: 2fc4797c-38ef-4cc7-926c-ca431c4739e8
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/getting-started-with-aspnet-web-api/action-results
msc.type: authoredcontent
ms.openlocfilehash: 68b82661b97434795e1c306b168033dfcde529bc
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="action-results-in-web-api-2"></a>Web API 2 中的動作結果
====================
由[Mike Wasson](https://github.com/MikeWasson)

本主題說明 ASP.NET Web 應用程式開發介面如何從控制器動作轉換為 HTTP 回應訊息轉換的傳回值。

Web API 控制器動作，可能會傳回下列其中一項：

1. void
2. **HttpResponseMessage**
3. **IHttpActionResult**
4. 其他類型

根據這些傳回時，Web 應用程式開發介面使用不同的機制來建立 HTTP 回應。

| 傳回型別 | Web 應用程式開發介面建立回應的方式 |
| --- | --- |
| void | 傳回空 204 （沒有內容） |
| **HttpResponseMessage** | 直接轉換成 HTTP 回應訊息。 |
| **IHttpActionResult** | 呼叫**ExecuteAsync**建立**HttpResponseMessage**，然後將轉換為 HTTP 回應訊息。 |
| 其他類型 | 序列化的傳回值寫入至回應主體。傳回 200 （確定）。 |

本主題的其餘部分將描述每個選項更多詳細資料中。

## <a name="void"></a>void

如果傳回型別`void`，Web 應用程式開發介面只會傳回空的 HTTP 回應狀態碼 204 （沒有內容）。

範例控制站：

[!code-csharp[Main](action-results/samples/sample1.cs)]

HTTP 回應：

[!code-console[Main](action-results/samples/sample2.cmd)]

## <a name="httpresponsemessage"></a>HttpResponseMessage

如果動作傳回[HttpResponseMessage](https://msdn.microsoft.com/en-us/library/system.net.http.httpresponsemessage.aspx)，Web API 傳回的值直接將 HTTP 回應訊息，使用轉換的內容**HttpResponseMessage**來擴展物件回應。

此選項可讓您大量控制回應訊息。 例如，下列的控制器動作設定快取控制標頭。

[!code-csharp[Main](action-results/samples/sample3.cs)]

回應：

[!code-console[Main](action-results/samples/sample4.cmd?highlight=2)]

如果您網域模型傳遞至**CreateResponse**方法時，Web 應用程式開發介面使用[媒體格式器](../formats-and-model-binding/media-formatters.md)序列化的模型寫入至回應主體。

[!code-csharp[Main](action-results/samples/sample5.cs)]

Web 應用程式開發介面會使用 Accept 標頭在要求中選擇的格式器。 如需詳細資訊，請參閱[內容交涉](../formats-and-model-binding/content-negotiation.md)。

## <a name="ihttpactionresult"></a>IHttpActionResult

**IHttpActionResult** Web API 2 中引進介面。 基本上，它會定義**HttpResponseMessage** factory。 以下是使用的一些優點**IHttpActionResult**介面：

- 簡化[單元測試](../testing-and-debugging/unit-testing-controllers-in-web-api.md)您控制站。
- 移到另一個類別建立 HTTP 回應的一般邏輯。
- 藉由隱藏建構回應的低層級的詳細資料可更清楚，控制器動作的意圖。

**IHttpActionResult**包含單一方法**ExecuteAsync**，以非同步方式建立**HttpResponseMessage**執行個體。

[!code-csharp[Main](action-results/samples/sample6.cs)]

如果控制器動作傳回**IHttpActionResult**，Web 應用程式開發介面呼叫**ExecuteAsync**方法來建立**HttpResponseMessage**。 然後它會轉換**HttpResponseMessage**至 HTTP 回應訊息。

以下是簡單的實作**IHttpActionResult**所建立的純文字格式的回應：

[!code-csharp[Main](action-results/samples/sample7.cs)]

範例控制器動作：

[!code-csharp[Main](action-results/samples/sample8.cs)]

回應：

[!code-console[Main](action-results/samples/sample9.cmd)]

通常，您將使用**IHttpActionResult**中定義的實作 **[System.Web.Http.Results](https://msdn.microsoft.com/en-us/library/system.web.http.results.aspx)** 命名空間。 **ApiController**類別會定義 helper 方法會傳回這些內建動作結果。

在下列範例中，如果要求不符合現有的產品識別碼，控制器會呼叫[ApiController.NotFound](https://msdn.microsoft.com/en-us/library/system.web.http.apicontroller.notfound.aspx)建立 404 （找不到） 回應。 否則，會呼叫控制器[ApiController.OK](https://msdn.microsoft.com/en-us/library/dn314591.aspx)，它會建立回應 200 （確定），包含產品。

[!code-csharp[Main](action-results/samples/sample10.cs)]

## <a name="other-return-types"></a>其他傳回型別

對於所有其他傳回型別、 Web API 會使用[媒體格式器](../formats-and-model-binding/media-formatters.md)將傳回值序列化。 Web API 寫入回應主體中序列化的值。 回應狀態碼為 200 （確定）。

[!code-csharp[Main](action-results/samples/sample11.cs)]

這種方法的缺點是您不能直接傳回錯誤碼 404 等。 不過，您可以擲回**HttpResponseException**的錯誤碼。 如需詳細資訊，請參閱[ASP.NET Web API 的例外狀況處理](../error-handling/exception-handling.md)。

Web 應用程式開發介面會使用 Accept 標頭在要求中選擇的格式器。 如需詳細資訊，請參閱[內容交涉](../formats-and-model-binding/content-negotiation.md)。

範例要求

[!code-console[Main](action-results/samples/sample12.cmd)]

範例回應：

[!code-console[Main](action-results/samples/sample13.cmd)]
