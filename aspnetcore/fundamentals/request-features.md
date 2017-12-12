---
title: "要求 ASP.NET 核心的功能"
author: ardalis
description: "深入了解與 HTTP 要求和回應，適用於 ASP.NET Core 介面中所定義的 web 伺服器實作詳細資料。"
keywords: ASP.NET Core,
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: d1fbd23c-2ff9-4216-b908-0201ff3afb7c
ms.technology: aspnet
ms.prod: asp.net-core
uid: fundamentals/request-features
ms.openlocfilehash: b689d82d16c6ef55485691b3474a070765c8144b
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
# <a name="request-features-in-aspnet-core"></a>要求 ASP.NET 核心的功能

由[Steve Smith](https://ardalis.com/)

有關 HTTP 要求和回應的網頁伺服器實作詳細資料，定義於介面中。 伺服器實作和中介軟體會使用這些介面來建立及修改應用程式的裝載管線。

## <a name="feature-interfaces"></a>功能的介面

ASP.NET Core 定義 HTTP 功能介面數目`Microsoft.AspNetCore.Http.Features`伺服器用來識別其支援的功能。 下列功能的介面會處理要求，並傳回回應：

`IHttpRequestFeature`定義 HTTP 要求，包括通訊協定、 路徑、 查詢字串、 標頭和主體的結構。

`IHttpResponseFeature`定義 HTTP 回應，包括狀態碼、 標頭和回應主體的結構。

`IHttpAuthenticationFeature`定義用來根據使用者識別的支援`ClaimsPrincipal`和指定驗證處理常式。

`IHttpUpgradeFeature`定義支援[HTTP 升級](https://tools.ietf.org/html/rfc2616.html#section-14.42)，可讓用戶端指定的其他通訊協定想要使用如果想要切換通訊協定的伺服器。

`IHttpBufferingFeature`定義用來停用緩衝處理的要求和/或回應的方法。

`IHttpConnectionFeature`定義本機和遠端位址與連接埠的屬性。

`IHttpRequestLifetimeFeature`定義支援中止的連線，或偵測如果要求已終止提前，例如為 「 依用戶端中斷連線。

`IHttpSendFileFeature`定義以非同步方式傳送檔案的方法。

`IHttpWebSocketFeature`定義應用程式開發介面支援 web 通訊端。

`IHttpRequestIdentifierFeature`加入您可以實作來唯一識別要求的屬性。

`ISessionFeature`定義`ISessionFactory`和`ISession`來支援使用者工作階段的抽象概念。

`ITlsConnectionFeature`定義應用程式開發介面來擷取用戶端憑證。

`ITlsTokenBindingFeature`定義為使用 TLS 語彙基元繫結參數的方法。

> [!NOTE]
> `ISessionFeature`不是伺服器功能，而藉由`SessionMiddleware`(請參閱[管理應用程式狀態](app-state.md))。

## <a name="feature-collections"></a>功能集合

`Features`屬性`HttpContext`提供介面來取得和設定可用的 HTTP 功能，目前的要求。 由於功能集合是可變動的要求內容中，即使中, 介軟體可用來修改該集合，並加入其他功能的支援。

## <a name="middleware-and-request-features"></a>中介軟體和要求的功能

雖然伺服器負責建立功能集合中, 介軟體新增到此集合和取用集合中的功能。 例如，`StaticFileMiddleware`存取`IHttpSendFileFeature`功能。 如果有此功能，它用來傳送要求的靜態檔案從其實體路徑。 否則，較慢的替代方法用來傳送檔案。 如果有的話，`IHttpSendFileFeature`允許開啟檔案，並執行直接核心模式複製到網路卡的作業系統。

此外中, 介軟體可以新增至伺服器所建立的功能集合。 即使可以由中介軟體，讓中介軟體來加強伺服器的功能取代現有的功能。 加入至集合的功能可立即用於其他中介軟體或基礎應用程式本身要求管線中的更新版本。

藉由結合自訂伺服器實作和特定中介軟體的增強功能，可用於建構精確的應用程式需要的功能集。 這可以讓遺漏要加入，而不需要在伺服器中，變更的功能，可確保只有少量的功能會公開，以減少攻擊介面區，並改善效能。

## <a name="summary"></a>總結

功能的介面定義給定的要求可能支援的特定 HTTP 功能。 伺服器定義集合的功能，以及一組初始的該伺服器所支援的功能，但中介軟體可以用來增強這些功能。

## <a name="additional-resources"></a>其他資源

* [伺服器](servers/index.md)

* [中介軟體](middleware.md)

* [開啟 Web Interface for .NET (OWIN)](owin.md)
