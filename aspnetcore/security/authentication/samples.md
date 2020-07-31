---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/samples
ms.openlocfilehash: 3e5e487adafc09d38400ea58936c5c2e8385e84f
ms.sourcegitcommit: 5a36758cca2861aeb10840093e46d273a6e6e91d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303595"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="af65a-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="af65a-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="af65a-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="af65a-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="af65a-105">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="af65a-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="af65a-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="af65a-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="af65a-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="af65a-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)
* [<span data-ttu-id="af65a-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="af65a-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="af65a-109">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="af65a-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* <span data-ttu-id="af65a-110">[外部宣告](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)</span><span class="sxs-lookup"><span data-stu-id="af65a-110">[External claims](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)</span></span>
* [<span data-ttu-id="af65a-111">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="af65a-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="af65a-112">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="af65a-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="af65a-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="af65a-113">Run the samples</span></span>

* <span data-ttu-id="af65a-114">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="af65a-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="af65a-115">例如， `release/3.1`</span><span class="sxs-lookup"><span data-stu-id="af65a-115">For example, `release/3.1`</span></span>
* <span data-ttu-id="af65a-116">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="af65a-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="af65a-117">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="af65a-117">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="af65a-118">流覽至*AspNetCore/src/Security/samples*中的範例，並使用來執行範例 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="af65a-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="af65a-119">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="af65a-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="af65a-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="af65a-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="af65a-121">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="af65a-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Cookies)
* [<span data-ttu-id="af65a-122">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="af65a-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="af65a-123">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="af65a-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/DynamicSchemes)
* <span data-ttu-id="af65a-124">[外部宣告](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Identity.ExternalClaims)</span><span class="sxs-lookup"><span data-stu-id="af65a-124">[External claims](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Identity.ExternalClaims)</span></span>
* [<span data-ttu-id="af65a-125">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="af65a-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="af65a-126">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="af65a-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="af65a-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="af65a-127">Run the samples</span></span>

* <span data-ttu-id="af65a-128">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="af65a-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="af65a-129">例如， `release/2.1`</span><span class="sxs-lookup"><span data-stu-id="af65a-129">For example, `release/2.1`</span></span>
* <span data-ttu-id="af65a-130">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="af65a-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="af65a-131">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="af65a-131">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="af65a-132">流覽至*AspNetCore/src/Security/samples*中的範例，並使用來執行範例 `dotnet run` 。</span><span class="sxs-lookup"><span data-stu-id="af65a-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
