---
title: ASP.NET Core 中的原則配置
author: rick-anderson
description: 驗證原則配置可讓您更輕鬆地擁有單一邏輯驗證架構
ms.author: riande
ms.date: 12/05/2019
uid: security/authentication/policyschemes
ms.openlocfilehash: f02d8e5cac20a9b60c5eddbd28253efacf682ea1
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660731"
---
# <a name="policy-schemes-in-aspnet-core"></a><span data-ttu-id="d9d49-103">ASP.NET Core 中的原則配置</span><span class="sxs-lookup"><span data-stu-id="d9d49-103">Policy schemes in ASP.NET Core</span></span>

<span data-ttu-id="d9d49-104">驗證原則配置可讓您更輕鬆地讓單一邏輯驗證架構使用多種方法。</span><span class="sxs-lookup"><span data-stu-id="d9d49-104">Authentication policy schemes make it easier to have a single logical authentication scheme potentially use multiple approaches.</span></span> <span data-ttu-id="d9d49-105">例如，原則配置可能會針對挑戰使用 Google 驗證，並針對其他所有專案使用 cookie 驗證。</span><span class="sxs-lookup"><span data-stu-id="d9d49-105">For example, a policy scheme might use Google authentication for challenges, and cookie authentication for everything else.</span></span> <span data-ttu-id="d9d49-106">驗證原則配置會使其成為：</span><span class="sxs-lookup"><span data-stu-id="d9d49-106">Authentication policy schemes make it:</span></span>

* <span data-ttu-id="d9d49-107">輕鬆將任何驗證動作轉送至另一個配置。</span><span class="sxs-lookup"><span data-stu-id="d9d49-107">Easy to forward any authentication action to another scheme.</span></span>
* <span data-ttu-id="d9d49-108">根據要求動態轉送。</span><span class="sxs-lookup"><span data-stu-id="d9d49-108">Forward dynamically based on the request.</span></span>

<span data-ttu-id="d9d49-109">使用衍生 <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> 的所有驗證配置和相關聯的[AuthenticationHandler\<TOptions >](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1)：</span><span class="sxs-lookup"><span data-stu-id="d9d49-109">All authentication schemes that use derived <xref:Microsoft.AspNetCore.Authentication.AuthenticationSchemeOptions> and the associated [AuthenticationHandler\<TOptions>](/dotnet/api/microsoft.aspnetcore.authentication.authenticationhandler-1):</span></span>

* <span data-ttu-id="d9d49-110">會在 ASP.NET Core 2.1 和更新版本中自動進行原則配置。</span><span class="sxs-lookup"><span data-stu-id="d9d49-110">Are automatically policy schemes in ASP.NET Core 2.1 and later.</span></span>
* <span data-ttu-id="d9d49-111">可以透過設定配置的選項來啟用。</span><span class="sxs-lookup"><span data-stu-id="d9d49-111">Can be enabled via configuring the scheme's options.</span></span>

[!code-csharp[sample](policyschemes/samples/AuthenticationSchemeOptions.cs?name=snippet)]

## <a name="examples"></a><span data-ttu-id="d9d49-112">範例</span><span class="sxs-lookup"><span data-stu-id="d9d49-112">Examples</span></span>

<span data-ttu-id="d9d49-113">下列範例顯示結合較低層級配置的較高層級架構。</span><span class="sxs-lookup"><span data-stu-id="d9d49-113">The following example shows a higher level scheme that combines lower level schemes.</span></span> <span data-ttu-id="d9d49-114">Google 驗證用於挑戰，而 cookie 驗證則用於其他所有專案：</span><span class="sxs-lookup"><span data-stu-id="d9d49-114">Google authentication is used for challenges, and cookie authentication is used for everything else:</span></span>

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet1)]

<span data-ttu-id="d9d49-115">下列範例會針對每個要求，啟用動態選取配置。</span><span class="sxs-lookup"><span data-stu-id="d9d49-115">The following example enables dynamic selection of schemes on a per request basis.</span></span> <span data-ttu-id="d9d49-116">也就是說，如何混合使用 cookie 和 API 驗證：</span><span class="sxs-lookup"><span data-stu-id="d9d49-116">That is, how to mix cookies and API authentication:</span></span>

 <!-- REVIEW, missing If set in public Func<HttpContext, string> ForwardDefaultSelector -->

[!code-csharp[sample](policyschemes/samples/Startup.cs?name=snippet2)]
