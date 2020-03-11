---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 3d7e28f6e501bd8bd3908ca4b314a63cee52ebe3
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658414"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="2f9b1-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="2f9b1-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="2f9b1-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="2f9b1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="2f9b1-105">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="2f9b1-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="2f9b1-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="2f9b1-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="2f9b1-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="2f9b1-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="2f9b1-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="2f9b1-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="2f9b1-109">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="2f9b1-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="2f9b1-110">外部宣告</span><span class="sxs-lookup"><span data-stu-id="2f9b1-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="2f9b1-111">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="2f9b1-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="2f9b1-112">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="2f9b1-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="2f9b1-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="2f9b1-113">Run the samples</span></span>

* <span data-ttu-id="2f9b1-114">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="2f9b1-115">例如， `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="2f9b1-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="2f9b1-116">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="2f9b1-117">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="2f9b1-118">流覽至*AspNetCore/src/Security/samples*中的範例，並使用 `dotnet run`執行範例。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f9b1-119">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="2f9b1-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="2f9b1-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="2f9b1-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="2f9b1-121">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="2f9b1-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="2f9b1-122">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="2f9b1-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="2f9b1-123">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="2f9b1-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="2f9b1-124">外部宣告</span><span class="sxs-lookup"><span data-stu-id="2f9b1-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="2f9b1-125">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="2f9b1-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="2f9b1-126">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="2f9b1-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="2f9b1-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="2f9b1-127">Run the samples</span></span>

* <span data-ttu-id="2f9b1-128">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="2f9b1-129">例如， `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="2f9b1-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="2f9b1-130">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="2f9b1-131">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="2f9b1-132">流覽至*AspNetCore/src/Security/samples*中的範例，並使用 `dotnet run`執行範例。</span><span class="sxs-lookup"><span data-stu-id="2f9b1-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
