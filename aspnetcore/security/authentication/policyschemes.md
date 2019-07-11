---
title: ASP.NET Core 中的原則配置
author: rick-anderson
description: 驗證原則配置輕鬆地擁有單一邏輯的驗證配置
ms.author: riande
ms.date: 02/28/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: be03f349455c673b0739935ad20e596325c8cb74
ms.sourcegitcommit: 8516b586541e6ba402e57228e356639b85dfb2b9
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/11/2019
ms.locfileid: "67815289"
---
# <a name="policy-schemes-in-aspnet-core"></a>ASP.NET Core 中的原則配置

驗證原則配置讓您更容易有可能使用多個方法的單一邏輯的驗證配置。 比方說，原則配置可能會使用 Google 驗證的挑戰和 cookie 驗證的所有其他項目。 驗證原則配置讓：

* 輕鬆地將轉送到另一個配置的任何驗證動作。
* 會根據要求，以動態方式正向。

使用衍生的所有驗證配置<xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions>和相關聯[ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):

* 會自動在 ASP.NET Core 2.1 和更新版本的原則配置。
* 您可以啟用透過設定的配置選項。

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a>範例

下列範例會示範較高的層級配置，結合了較低層級的配置。 使用 Google 驗證的挑戰，並 cookie 驗證使用於其他所有項目：

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

下列範例可讓您動態選取個別要求基礎上的配置。 也就是如何混用 cookie 和 API 驗證：

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
