---
title: ASP.NET Core 中的要求功能
author: ardalis
description: 了解有關 HTTP 要求和回應的網頁伺服器實作詳細資料，其定義於 ASP.NET Core 的介面中。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: fundamentals/request-features
ms.openlocfilehash: e26a1a7b35d40c1214bbc40269571545cbd2c235
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82776021"
---
# <a name="request-features-in-aspnet-core"></a>ASP.NET Core 中的要求功能

作者：[Steve Smith](https://ardalis.com/)

有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。 伺服器實作與中介軟體會使用這些介面，建立及修改應用程式的裝載管線。

## <a name="feature-interfaces"></a>功能介面

ASP.NET Core 可定義 `Microsoft.AspNetCore.Http.Features` 中伺服器用來識別其支援功能的 HTTP 功能介面的數目。 下列功能介面會處理要求並傳回回應：

`IHttpRequestFeature` 定義 HTTP 要求的結構，包括通訊協定、路徑、查詢字串、標頭和主體。

`IHttpResponseFeature` 定義 HTTP 回應的結構，包括狀態碼、標頭和回應主體。

`IHttpAuthenticationFeature` 定義根據 `ClaimsPrincipal` 識別使用者和指定驗證處理常式的支援。

`IHttpUpgradeFeature` 定義 [HTTP 升級](https://tools.ietf.org/html/rfc2616.html#section-14.42)的支援，這可讓用戶端指定它在伺服器想要切換通訊協定時要使用哪些其他通訊協定。

`IHttpBufferingFeature` 定義停用要求和/或回應之緩衝處理的方法。

`IHttpConnectionFeature` 定義本機和遠端位址與連接埠的屬性。

`IHttpRequestLifetimeFeature`　定義中止連線或偵測要求是否已提前終止 (例如由用戶端中斷連線) 的支援。

`IHttpSendFileFeature` 定義以非同步方式傳送檔案的方法。

`IHttpWebSocketFeature` 定義支援 Web 通訊端的 API。

`IHttpRequestIdentifierFeature` 新增您可以實作的屬性來唯一識別要求。

`ISessionFeature` 定義支援使用者工作階段的 `ISessionFactory` 和 `ISession` 抽象概念。

`ITlsConnectionFeature` 定義擷取用戶端憑證的 API。

`ITlsTokenBindingFeature` 定義使用 TLS 權杖繫結參數的方法。

> [!NOTE]
> `ISessionFeature` 不是伺服器功能，但卻由 `SessionMiddleware` 實作 (請參閱[管理應用程式狀態](app-state.md))。

## <a name="feature-collections"></a>功能集合

`HttpContext` 的 `Features` 屬性提供一個介面來取得和設定目前要求的可用 HTTP 功能。 由於功能集合即使在要求內容中都是可變動的，因此可以使用中介軟體來修改該集合，並新增其他功能的支援。

## <a name="middleware-and-request-features"></a>中介軟體和要求功能

雖然伺服器負責建立功能集合，但中介軟體可以同時新增至此集合和取用集合中的功能。 例如，`StaticFileMiddleware` 可存取 `IHttpSendFileFeature` 功能。 如果有該功能，它用來從其實體路徑傳送要求的靜態檔案。 否則，會使用較慢的替代方法來傳送檔案。 可用時，`IHttpSendFileFeature` 允許作業系統開啟檔案，並執行直接核心模式複製到網路卡。

此外，中介軟體還可以新增至伺服器所建立的功能集合。 現有的功能甚至可以取代為中介軟體，讓中介軟體來增強伺服器的功能。 新增至集合的功能稍後在要求管線中，可立即用於其他中介軟體或基礎應用程式本身。

藉由結合自訂伺服器實作和特定中介軟體增強功能，可建構應用程式所需的一組精確功能。 這允許新增遺漏的功能而不需要變更伺服器，並確保只公開少量的功能，以減少受攻擊面區域並改善效能。

## <a name="summary"></a>摘要

功能介面定義給定的要求可能支援的特定 HTTP 功能。 伺服器定義功能的集合，以及一組該伺服器所支援，但中介軟體可用來增強這些功能的初始功能。

## <a name="additional-resources"></a>其他資源

* [伺服器](xref:fundamentals/servers/index)
* [中介軟體](xref:fundamentals/middleware/index)
* [Open Web Interface for .NET (OWIN)](xref:fundamentals/owin)
