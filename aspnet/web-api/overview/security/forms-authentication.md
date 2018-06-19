---
uid: web-api/overview/security/forms-authentication
title: ASP.NET Web API 中表單驗證 |Microsoft 文件
author: MikeWasson
description: 描述如何在 ASP.NET Web API 中使用表單驗證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
ms.prod: .net-framework
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 9027d76bcf8854fc85f11906d3651511f350cd32
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26508307"
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的表單驗證
====================
由[Mike Wasson](https://github.com/MikeWasson)

表單驗證會使用 HTML 表單，將使用者的認證傳送到伺服器。 它不是網際網路標準。 表單驗證是只適用於 web 應用程式開發介面中的 web 應用程式中，從呼叫，讓使用者可以互動 HTML 表單。

| 優點 | 缺點 |
| --- | --- |
| -容易實作： 內建於 ASP.NET。 -使用 ASP.NET 成員資格提供者，可讓您輕鬆地管理使用者帳戶。 | -不標準 HTTP 驗證機制。使用 HTTP cookie，而不是標準的授權標頭。 -需要瀏覽器用戶端。 認證會以純文字傳送。 -很容易遭受跨網站要求偽造 (CSRF);需要反 CSRF 量值。 -很難從 nonbrowser 用戶端使用。 登入需要瀏覽器。 在要求中傳送使用者認證。 -有些使用者停用 cookie。 |

簡言之，在 ASP.NET 表單驗證的運作方式如下：

1. 用戶端要求需要驗證的資源。
2. 如果使用者未經過驗證，伺服器就會傳回 HTTP 302 （找到），並將重新導向至登入頁面。
3. 使用者輸入認證，並送出表單。
4. 伺服器會傳回另一個 HTTP 302 重新導向回到原始的 URI。 此回應包含驗證 cookie。
5. 用戶端一次要求的資源。 要求包含的驗證 cookie，讓伺服器授與的要求。

![](forms-authentication/_static/image1.png)

如需詳細資訊，請參閱[的表單驗證概觀。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>使用表單驗證搭配 Web API

若要建立使用表單驗證的應用程式，請在 MVC 4 專案精靈中選取 「 網際網路應用程式 」 範本。 此範本所建立帳戶管理的 MVC 的控制器。 您也可以使用 「 單一網頁應用程式 」 範本，適用於 ASP.NET 改 2012 Update。

在您 web 應用程式開發介面的控制站，您可以限制存取使用`[Authorize]`屬性中所述[使用 [Authorize] 屬性](authentication-and-authorization-in-aspnet-web-api.md#auth3)。

表單驗證會使用工作階段 cookie 來驗證要求。 瀏覽器會自動傳送至目的地網站的所有相關的 cookie。 這項功能，會使表單驗證可能容易遭受跨網站要求偽造 (CSRF) 攻擊，請參閱[防止跨站台要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。

表單驗證不會加密使用者的認證。 因此，不安全，除非您搭配 SSL 使用表單驗證。 請參閱[使用 Web 應用程式開發介面中的 SSL](working-with-ssl-in-web-api.md)。
