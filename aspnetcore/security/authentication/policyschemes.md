---
title: ASP.NET Core 中的原則配置
author: rick-anderson
description: 驗證原則配置可讓您更輕鬆地擁有單一邏輯驗證架構
ms.author: riande
ms.date: 12/05/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/policyschemes
ms.openlocfilehash: ddedf62c5e8363bd93c9948fd2d3418abc566539
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82767313"
---
# <a name="policy-schemes-in-aspnet-core"></a>ASP.NET Core 中的原則配置

驗證原則配置可讓您更輕鬆地讓單一邏輯驗證架構使用多種方法。 例如，原則配置可能會針對挑戰使用 Google 驗證，並針對其他所有專案使用 cookie 驗證。 驗證原則配置會使其成為：

* 輕鬆將任何驗證動作轉送至另一個配置。
* 根據要求動態轉送。

所有使用衍生<xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions>的驗證配置和相關聯[的\<AuthenticationHandler TOptions>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)：

* 會在 ASP.NET Core 2.1 和更新版本中自動進行原則配置。
* 可以透過設定配置的選項來啟用。

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>範例

下列範例顯示結合較低層級配置的較高層級架構。 Google 驗證用於挑戰，而 cookie 驗證則用於其他所有專案：

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

下列範例會針對每個要求，啟用動態選取配置。 也就是說，如何混合使用 cookie 和 API 驗證：

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
