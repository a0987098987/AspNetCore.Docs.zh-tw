---
uid: web-api/overview/security/forms-authentication
title: ASP.NET Web API 中表單驗證 |Microsoft Docs
author: MikeWasson
description: 描述如何在 ASP.NET Web API 中使用表單驗證。
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/12/2012
ms.topic: article
ms.assetid: 9f06c1f2-ffaa-4831-94a0-2e4a3befdf07
ms.technology: dotnet-webapi
msc.legacyurl: /web-api/overview/security/forms-authentication
msc.type: authoredcontent
ms.openlocfilehash: 7642a30816a04a88a25ef8bf4f01e766fda362ce
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365067"
---
<a name="forms-authentication-in-aspnet-web-api"></a>ASP.NET Web API 中的表單驗證
====================
藉由[Mike Wasson](https://github.com/MikeWasson)

表單驗證會使用 HTML 表單，使用者的認證傳送至伺服器。 它不是網際網路標準。 讓使用者可以互動的 HTML 表單，表單驗證僅適用於 web 的 web 應用程式中，從呼叫的 Api。

| 優點 | 缺點 |
| --- | --- |
| 輕鬆實作： 內建於 ASP.NET。 -使用 ASP.NET 成員資格提供者，方便您管理使用者帳戶。 | -不標準 HTTP 驗證機制;會使用 HTTP cookie，而不是標準的授權標頭。 -需要瀏覽器用戶端。 認證會以純文字傳送。 -容易遭受跨網站要求偽造 (CSRF);需要防 CSRF 的量值。 -很難從 nonbrowser 用戶端使用。 登入需要瀏覽器。 在要求中傳送使用者認證。 -部分的使用者停用 cookie。 |

簡單地說，在 ASP.NET 中的表單驗證的運作方式如下：

1. 用戶端要求需要驗證的資源。
2. 如果使用者未經過驗證，伺服器就會傳回 HTTP 302 （已找到），並將重新導向至登入頁面。
3. 使用者輸入認證，並送出表單。
4. 伺服器會傳回另一個 HTTP 302 重新導向回到原始的 URI。 此回應包含驗證 cookie。
5. 用戶端一次要求的資源。 要求會包含驗證 cookie，因此伺服器會授與的要求。

![](forms-authentication/_static/image1.png)

如需詳細資訊，請參閱[的表單驗證概觀。](../../../web-forms/overview/older-versions-security/introduction/an-overview-of-forms-authentication-cs.md)

## <a name="using-forms-authentication-with-web-api"></a>使用表單驗證來使用 Web API

若要建立使用表單驗證的應用程式，選取 [MVC 4 專案精靈] 中的 「 網際網路應用程式 」 範本。 此範本會建立適用於帳戶管理的 MVC 控制器。 您也可以使用 「 單一頁面應用程式 」 範本，在 ASP.NET Fall 2012 更新中提供。

在您的 web API 控制器，您可以使用限制存取`[Authorize]`屬性中所述[使用 [Authorize] 屬性](authentication-and-authorization-in-aspnet-web-api.md#auth3)。

表單驗證會使用工作階段 cookie 來驗證要求。 瀏覽器會自動將所有相關的 cookie 傳送至目的地網站。 這項功能，可讓表單驗證可能容易遭受跨網站要求偽造 (CSRF) 攻擊，請參閱[防止跨網站要求偽造 (CSRF) 攻擊](preventing-cross-site-request-forgery-csrf-attacks.md)。

表單驗證不會加密使用者認證。 因此，不安全，除非搭配 SSL 使用表單驗證。 請參閱[使用 Web API 中的 SSL](working-with-ssl-in-web-api.md)。
