---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: d49aef198e926d88f1a6727f84b06f0861c8812d
ms.sourcegitcommit: d34b2627a69bc8940b76a949de830335db9701d3
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/23/2019
ms.locfileid: "71187294"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="a2042-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="a2042-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="a2042-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a2042-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="a2042-105">[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="a2042-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="a2042-106">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="a2042-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="a2042-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="a2042-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Cookies)
* [<span data-ttu-id="a2042-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="a2042-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="a2042-109">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="a2042-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="a2042-110">外部宣告</span><span class="sxs-lookup"><span data-stu-id="a2042-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="a2042-111">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="a2042-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="a2042-112">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="a2042-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/3.0/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="a2042-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="a2042-113">Run the samples</span></span>

* <span data-ttu-id="a2042-114">選取[分支](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="a2042-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="a2042-115">例如：`Tag:v3.0.0`</span><span class="sxs-lookup"><span data-stu-id="a2042-115">For example, `Tag:v3.0.0`</span></span>
* <span data-ttu-id="a2042-116">複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="a2042-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="a2042-117">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。</span><span class="sxs-lookup"><span data-stu-id="a2042-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="a2042-118">流覽至*AspNetCore/src/Security/samples*中的範例，並使用`dotnet run`來執行範例。</span><span class="sxs-lookup"><span data-stu-id="a2042-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="a2042-119">[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：</span><span class="sxs-lookup"><span data-stu-id="a2042-119">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="a2042-120">宣告轉換</span><span class="sxs-lookup"><span data-stu-id="a2042-120">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="a2042-121">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="a2042-121">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="a2042-122">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="a2042-122">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="a2042-123">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="a2042-123">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="a2042-124">外部宣告</span><span class="sxs-lookup"><span data-stu-id="a2042-124">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="a2042-125">根據要求在 cookie 和另一個驗證配置之間進行選取</span><span class="sxs-lookup"><span data-stu-id="a2042-125">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="a2042-126">限制靜態檔案的存取</span><span class="sxs-lookup"><span data-stu-id="a2042-126">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="a2042-127">執行範例</span><span class="sxs-lookup"><span data-stu-id="a2042-127">Run the samples</span></span>

* <span data-ttu-id="a2042-128">選取[分支](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="a2042-128">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="a2042-129">例如：`release/2.2`</span><span class="sxs-lookup"><span data-stu-id="a2042-129">For example, `release/2.2`</span></span>
* <span data-ttu-id="a2042-130">複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="a2042-130">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="a2042-131">確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://www.microsoft.com/net/download/all)版本。</span><span class="sxs-lookup"><span data-stu-id="a2042-131">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="a2042-132">流覽至*AspNetCore/src/Security/samples*中的範例，並使用`dotnet run`來執行範例。</span><span class="sxs-lookup"><span data-stu-id="a2042-132">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>

::: moniker-end
