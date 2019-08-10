---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor 的支援平臺。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948248"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="bd966-103">ASP.NET Core Blazor 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="bd966-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="bd966-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="bd966-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="bd966-105">瀏覽器需求</span><span class="sxs-lookup"><span data-stu-id="bd966-105">Browser requirements</span></span>

### <a name="blazor-client-side"></a><span data-ttu-id="bd966-106">Blazor 用戶端</span><span class="sxs-lookup"><span data-stu-id="bd966-106">Blazor client-side</span></span>

| <span data-ttu-id="bd966-107">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="bd966-107">Browser</span></span>                          | <span data-ttu-id="bd966-108">版本</span><span class="sxs-lookup"><span data-stu-id="bd966-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="bd966-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="bd966-109">Microsoft Edge</span></span>                   | <span data-ttu-id="bd966-110">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-110">Current</span></span>               |
| <span data-ttu-id="bd966-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="bd966-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="bd966-112">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-112">Current</span></span>               |
| <span data-ttu-id="bd966-113">Google Chrome, 包括 Android</span><span class="sxs-lookup"><span data-stu-id="bd966-113">Google Chrome, including Android</span></span> | <span data-ttu-id="bd966-114">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-114">Current</span></span>               |
| <span data-ttu-id="bd966-115">Safari, 包括 iOS</span><span class="sxs-lookup"><span data-stu-id="bd966-115">Safari, including iOS</span></span>            | <span data-ttu-id="bd966-116">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-116">Current</span></span>               |
| <span data-ttu-id="bd966-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="bd966-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="bd966-118">不受支援&dagger;</span><span class="sxs-lookup"><span data-stu-id="bd966-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="bd966-119">&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。</span><span class="sxs-lookup"><span data-stu-id="bd966-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server-side"></a><span data-ttu-id="bd966-120">Blazor 伺服器端</span><span class="sxs-lookup"><span data-stu-id="bd966-120">Blazor server-side</span></span>

| <span data-ttu-id="bd966-121">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="bd966-121">Browser</span></span>                          | <span data-ttu-id="bd966-122">版本</span><span class="sxs-lookup"><span data-stu-id="bd966-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="bd966-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="bd966-123">Microsoft Edge</span></span>                   | <span data-ttu-id="bd966-124">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-124">Current</span></span>    |
| <span data-ttu-id="bd966-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="bd966-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="bd966-126">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-126">Current</span></span>    |
| <span data-ttu-id="bd966-127">Google Chrome, 包括 Android</span><span class="sxs-lookup"><span data-stu-id="bd966-127">Google Chrome, including Android</span></span> | <span data-ttu-id="bd966-128">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-128">Current</span></span>    |
| <span data-ttu-id="bd966-129">Safari, 包括 iOS</span><span class="sxs-lookup"><span data-stu-id="bd966-129">Safari, including iOS</span></span>            | <span data-ttu-id="bd966-130">目前</span><span class="sxs-lookup"><span data-stu-id="bd966-130">Current</span></span>    |
| <span data-ttu-id="bd966-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="bd966-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="bd966-132">英寸&dagger;</span><span class="sxs-lookup"><span data-stu-id="bd966-132">11&dagger;</span></span> |

<span data-ttu-id="bd966-133">&dagger;需要額外的 polyfills (例如, 可透過[Polyfill.io](https://polyfill.io/v3/)配套新增承諾)。</span><span class="sxs-lookup"><span data-stu-id="bd966-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bd966-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="bd966-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
