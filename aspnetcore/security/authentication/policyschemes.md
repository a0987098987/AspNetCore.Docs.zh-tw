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
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="086e9-103">ASP.NET Core 中的原則配置</span><span class="sxs-lookup"><span data-stu-id="086e9-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="086e9-104">驗證原則配置可讓您更輕鬆地讓單一邏輯驗證架構使用多種方法。</span><span class="sxs-lookup"><span data-stu-id="086e9-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="086e9-105">例如，原則配置可能會針對挑戰使用 Google 驗證，並針對其他所有專案使用 cookie 驗證。</span><span class="sxs-lookup"><span data-stu-id="086e9-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="086e9-106">驗證原則配置會使其成為：</span><span class="sxs-lookup"><span data-stu-id="086e9-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="086e9-107">輕鬆將任何驗證動作轉送至另一個配置。</span><span class="sxs-lookup"><span data-stu-id="086e9-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="086e9-108">根據要求動態轉送。</span><span class="sxs-lookup"><span data-stu-id="086e9-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="086e9-109">所有使用衍生<xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions>的驗證配置和相關聯[的\<AuthenticationHandler TOptions>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)：</span><span class="sxs-lookup"><span data-stu-id="086e9-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [AuthenticationHandler\<TOptions>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="086e9-110">會在 ASP.NET Core 2.1 和更新版本中自動進行原則配置。</span><span class="sxs-lookup"><span data-stu-id="086e9-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="086e9-111">可以透過設定配置的選項來啟用。</span><span class="sxs-lookup"><span data-stu-id="086e9-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="086e9-112">範例</span><span class="sxs-lookup"><span data-stu-id="086e9-112">Examples</span></span>

<span data-ttu-id="086e9-113">下列範例顯示結合較低層級配置的較高層級架構。</span><span class="sxs-lookup"><span data-stu-id="086e9-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="086e9-114">Google 驗證用於挑戰，而 cookie 驗證則用於其他所有專案：</span><span class="sxs-lookup"><span data-stu-id="086e9-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="086e9-115">下列範例會針對每個要求，啟用動態選取配置。</span><span class="sxs-lookup"><span data-stu-id="086e9-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="086e9-116">也就是說，如何混合使用 cookie 和 API 驗證：</span><span class="sxs-lookup"><span data-stu-id="086e9-116">That is, how to mix cookies and API authentication:</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
