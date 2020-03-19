---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b013d02a65e752bbb61a87a0bf502785125378a2
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511609"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="b71d4-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="b71d4-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="b71d4-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="b71d4-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="b71d4-105">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="b71d4-105">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b71d4-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="b71d4-106">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b71d4-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="b71d4-107">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="b71d4-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b71d4-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b71d4-109">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="b71d4-109">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b71d4-110">外部宣告</span><span class="sxs-lookup"><span data-stu-id="b71d4-110">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b71d4-111">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="b71d4-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b71d4-112">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="b71d4-112">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b71d4-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="b71d4-113">Run the samples</span></span>

* <span data-ttu-id="b71d4-114">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b71d4-114">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="b71d4-115">例如， `Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="b71d4-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="b71d4-116">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b71d4-116">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="b71d4-117">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="b71d4-117">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b71d4-118">流覽至*AspNetCore/src/Security/samples*中的範例，並使用 `dotnet run`執行範例。</span><span class="sxs-lookup"><span data-stu-id="b71d4-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="b71d4-119">[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="b71d4-119">The [ASP.NET Core repository](https://github.com/dotnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b71d4-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="b71d4-120">Claims transformation</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b71d4-121">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="b71d4-121">Cookie authentication</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="b71d4-122">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b71d4-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b71d4-123">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="b71d4-123">Dynamic authentication schemes and options</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b71d4-124">外部宣告</span><span class="sxs-lookup"><span data-stu-id="b71d4-124">External claims</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b71d4-125">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="b71d4-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b71d4-126">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="b71d4-126">Restricts access to static files</span></span>](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b71d4-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="b71d4-127">Run the samples</span></span>

* <span data-ttu-id="b71d4-128">選取[分支](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b71d4-128">Select a [branch](https://github.com/dotnet/AspNetCore).</span></span> <span data-ttu-id="b71d4-129">例如， `release/2.2`</span><span class="sxs-lookup"><span data-stu-id="b71d4-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="b71d4-130">複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b71d4-130">Clone or download the [ASP.NET Core repository](https://github.com/dotnet/AspNetCore).</span></span>
* <span data-ttu-id="b71d4-131">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。</span><span class="sxs-lookup"><span data-stu-id="b71d4-131">Verify you have installed the [.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b71d4-132">流覽至*AspNetCore/src/Security/samples*中的範例，並使用 `dotnet run`執行範例。</span><span class="sxs-lookup"><span data-stu-id="b71d4-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
