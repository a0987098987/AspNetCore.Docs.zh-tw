---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中的驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64897155"
---
# <a name="authentication-samples-for-aspnet-core"></a><span data-ttu-id="b6682-103">ASP.NET Core 的驗證範例</span><span class="sxs-lookup"><span data-stu-id="b6682-103">Authentication samples for ASP.NET Core</span></span>

<span data-ttu-id="b6682-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b6682-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b6682-105">[ASP.NET Core 儲存機制](https://github.com/aspnet/AspNetCore)包含中的下列驗證範例*AspNetCore/src/Security/samples*資料夾：</span><span class="sxs-lookup"><span data-stu-id="b6682-105">The [ASP.NET Core repository](https://github.com/aspnet/AspNetCore) contains the following authentication samples in the *AspNetCore/src/Security/samples* folder:</span></span>

* [<span data-ttu-id="b6682-106">宣告轉型</span><span class="sxs-lookup"><span data-stu-id="b6682-106">Claims transformation</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [<span data-ttu-id="b6682-107">Cookie 驗證</span><span class="sxs-lookup"><span data-stu-id="b6682-107">Cookie authentication</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [<span data-ttu-id="b6682-108">自訂原則提供者-IAuthorizationPolicyProvider</span><span class="sxs-lookup"><span data-stu-id="b6682-108">Custom policy provider - IAuthorizationPolicyProvider</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [<span data-ttu-id="b6682-109">動態驗證配置和選項</span><span class="sxs-lookup"><span data-stu-id="b6682-109">Dynamic authentication schemes and options</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [<span data-ttu-id="b6682-110">外部宣告</span><span class="sxs-lookup"><span data-stu-id="b6682-110">External claims</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [<span data-ttu-id="b6682-111">選取 cookie 與另一種根據要求的驗證配置</span><span class="sxs-lookup"><span data-stu-id="b6682-111">Selecting between cookie and another authentication scheme based on the request</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [<span data-ttu-id="b6682-112">限制存取靜態檔案</span><span class="sxs-lookup"><span data-stu-id="b6682-112">Restricts access to static files</span></span>](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a><span data-ttu-id="b6682-113">執行範例</span><span class="sxs-lookup"><span data-stu-id="b6682-113">Run the samples</span></span>

* <span data-ttu-id="b6682-114">選取 [分支](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b6682-114">Select a [branch](https://github.com/aspnet/AspNetCore).</span></span> <span data-ttu-id="b6682-115">例如： `release/2.2` </span><span class="sxs-lookup"><span data-stu-id="b6682-115">For example, `release/2.2`</span></span>
* <span data-ttu-id="b6682-116">複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。</span><span class="sxs-lookup"><span data-stu-id="b6682-116">Clone or download the [ASP.NET Core repository](https://github.com/aspnet/AspNetCore).</span></span>
* <span data-ttu-id="b6682-117">確認您已安裝[.NET Core SDK](https://www.microsoft.com/net/download/all)比對的 ASP.NET Core 存放庫複製的版本。</span><span class="sxs-lookup"><span data-stu-id="b6682-117">Verify you have installed the [.NET Core SDK](https://www.microsoft.com/net/download/all) version matching the clone of the ASP.NET Core repository.</span></span>
* <span data-ttu-id="b6682-118">瀏覽至中的範例*AspNetCore/src/Security/samples*並執行範例，其中含有`dotnet run`。</span><span class="sxs-lookup"><span data-stu-id="b6682-118">Navigate to a sample in *AspNetCore/src/Security/samples* and run the sample with `dotnet run`.</span></span>
