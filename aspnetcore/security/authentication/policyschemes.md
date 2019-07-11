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
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="d6d57-103">ASP.NET Core 中的原則配置</span><span class="sxs-lookup"><span data-stu-id="d6d57-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="d6d57-104">驗證原則配置讓您更容易有可能使用多個方法的單一邏輯的驗證配置。</span><span class="sxs-lookup"><span data-stu-id="d6d57-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="d6d57-105">比方說，原則配置可能會使用 Google 驗證的挑戰和 cookie 驗證的所有其他項目。</span><span class="sxs-lookup"><span data-stu-id="d6d57-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="d6d57-106">驗證原則配置讓：</span><span class="sxs-lookup"><span data-stu-id="d6d57-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="d6d57-107">輕鬆地將轉送到另一個配置的任何驗證動作。</span><span class="sxs-lookup"><span data-stu-id="d6d57-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="d6d57-108">會根據要求，以動態方式正向。</span><span class="sxs-lookup"><span data-stu-id="d6d57-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="d6d57-109">使用衍生的所有驗證配置<xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions>和相關聯[ `AuthenticationHandler<TOptions>` ](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span><span class="sxs-lookup"><span data-stu-id="d6d57-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [`AuthenticationHandler<TOptions>`](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="d6d57-110">會自動在 ASP.NET Core 2.1 和更新版本的原則配置。</span><span class="sxs-lookup"><span data-stu-id="d6d57-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="d6d57-111">您可以啟用透過設定的配置選項。</span><span class="sxs-lookup"><span data-stu-id="d6d57-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="d6d57-112">範例</span><span class="sxs-lookup"><span data-stu-id="d6d57-112">Examples</span></span>

<span data-ttu-id="d6d57-113">下列範例會示範較高的層級配置，結合了較低層級的配置。</span><span class="sxs-lookup"><span data-stu-id="d6d57-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="d6d57-114">使用 Google 驗證的挑戰，並 cookie 驗證使用於其他所有項目：</span><span class="sxs-lookup"><span data-stu-id="d6d57-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="d6d57-115">下列範例可讓您動態選取個別要求基礎上的配置。</span><span class="sxs-lookup"><span data-stu-id="d6d57-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="d6d57-116">也就是如何混用 cookie 和 API 驗證：</span><span class="sxs-lookup"><span data-stu-id="d6d57-116">That is, how to mix cookies and API authentication:</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
