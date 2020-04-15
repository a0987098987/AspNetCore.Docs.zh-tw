---
title: ASP.NET核心的身份驗證範例
author: rick-anderson
description: 提供指向ASP.NET核心存儲庫中的身份驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b33eaa5c1bda9e23b2815cd1663e02ae06fec856
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81308211"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="d6b33-103">ASP.NET核心的身份驗證範例</span><span class="sxs-lookup"><span data-stu-id="d6b33-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="d6b33-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="d6b33-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="d6b33-105">[ASP.NET核心儲存函式庫](https://github.com/dotnet/AspNetCore)在*AspNetCore/src/安全/範例*資料夾中包含以下身份驗證範例:</span><span class="sxs-lookup"><span data-stu-id="d6b33-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="d6b33-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="d6b33-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="d6b33-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="d6b33-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)
* [<span data-ttu-id="d6b33-108">自訂政策提供者 - 授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="d6b33-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="d6b33-109">動態認證方案和選項</span><span class="sxs-lookup"><span data-stu-id="d6b33-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="d6b33-110">外部索賠</span><span class="sxs-lookup"><span data-stu-id="d6b33-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="d6b33-111">根據要求在 Cookie 和另一個身份驗證方案之間進行選擇</span><span class="sxs-lookup"><span data-stu-id="d6b33-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="d6b33-112">限制對靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="d6b33-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="d6b33-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="d6b33-113">Run the samples</span></span>

* <span data-ttu-id="d6b33-114">選擇[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="d6b33-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="d6b33-115">例如， `release/3.1`</span><span class="sxs-lookup"><span data-stu-id="d6b33-115">For example, `release/3.1`</span></span>
* <span data-ttu-id="d6b33-116">KSP.[NET 核心儲存函式庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="d6b33-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="d6b33-117">驗證已安裝與ASP.NET核心儲存庫的克隆相匹配的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="d6b33-117">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="d6b33-118">導航到*AspNetCore/src/Security/範例*中的範例,`dotnet run`然後使用 執行範例。</span><span class="sxs-lookup"><span data-stu-id="d6b33-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="d6b33-119">[ASP.NET核心儲存函式庫](https://github.com/dotnet/AspNetCore)在*AspNetCore/src/安全/範例*資料夾中包含以下身份驗證範例:</span><span class="sxs-lookup"><span data-stu-id="d6b33-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="d6b33-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="d6b33-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="d6b33-121">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="d6b33-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="d6b33-122">自訂政策提供者 - 授權原則提供者</span><span class="sxs-lookup"><span data-stu-id="d6b33-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="d6b33-123">動態認證方案和選項</span><span class="sxs-lookup"><span data-stu-id="d6b33-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="d6b33-124">外部索賠</span><span class="sxs-lookup"><span data-stu-id="d6b33-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="d6b33-125">根據要求在 Cookie 和另一個身份驗證方案之間進行選擇</span><span class="sxs-lookup"><span data-stu-id="d6b33-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="d6b33-126">限制對靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="d6b33-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="d6b33-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="d6b33-127">Run the samples</span></span>

* <span data-ttu-id="d6b33-128">選擇[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="d6b33-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="d6b33-129">例如， `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="d6b33-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="d6b33-130">KSP.[NET 核心儲存函式庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="d6b33-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="d6b33-131">驗證已安裝與ASP.NET核心儲存庫的克隆相匹配的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="d6b33-131">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="d6b33-132">導航到*AspNetCore/src/Security/範例*中的範例,`dotnet run`然後使用 執行範例。</span><span class="sxs-lookup"><span data-stu-id="d6b33-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
