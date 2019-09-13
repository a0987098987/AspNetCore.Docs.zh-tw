---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor 的支援平臺。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 042fbb1b2c7f92b7dc6443319f3f195a12a55adc
ms.sourcegitcommit: 092061c4f6ef46ed2165fa84de6273d3786fb97e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2019
ms.locfileid: "70963888"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="501bf-103">ASP.NET Core Blazor 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="501bf-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="501bf-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="501bf-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="501bf-105">瀏覽器需求</span><span class="sxs-lookup"><span data-stu-id="501bf-105">Browser requirements</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="501bf-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="501bf-106">Blazor WebAssembly</span></span>

| <span data-ttu-id="501bf-107">Browser</span><span class="sxs-lookup"><span data-stu-id="501bf-107">Browser</span></span>                          | <span data-ttu-id="501bf-108">Version</span><span class="sxs-lookup"><span data-stu-id="501bf-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="501bf-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="501bf-109">Microsoft Edge</span></span>                   | <span data-ttu-id="501bf-110">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-110">Current</span></span>               |
| <span data-ttu-id="501bf-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="501bf-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="501bf-112">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-112">Current</span></span>               |
| <span data-ttu-id="501bf-113">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="501bf-113">Google Chrome, including Android</span></span> | <span data-ttu-id="501bf-114">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-114">Current</span></span>               |
| <span data-ttu-id="501bf-115">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="501bf-115">Safari, including iOS</span></span>            | <span data-ttu-id="501bf-116">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-116">Current</span></span>               |
| <span data-ttu-id="501bf-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="501bf-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="501bf-118">不受支援&dagger;</span><span class="sxs-lookup"><span data-stu-id="501bf-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="501bf-119">&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。</span><span class="sxs-lookup"><span data-stu-id="501bf-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server"></a><span data-ttu-id="501bf-120">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="501bf-120">Blazor Server</span></span>

| <span data-ttu-id="501bf-121">Browser</span><span class="sxs-lookup"><span data-stu-id="501bf-121">Browser</span></span>                          | <span data-ttu-id="501bf-122">Version</span><span class="sxs-lookup"><span data-stu-id="501bf-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="501bf-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="501bf-123">Microsoft Edge</span></span>                   | <span data-ttu-id="501bf-124">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-124">Current</span></span>    |
| <span data-ttu-id="501bf-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="501bf-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="501bf-126">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-126">Current</span></span>    |
| <span data-ttu-id="501bf-127">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="501bf-127">Google Chrome, including Android</span></span> | <span data-ttu-id="501bf-128">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-128">Current</span></span>    |
| <span data-ttu-id="501bf-129">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="501bf-129">Safari, including iOS</span></span>            | <span data-ttu-id="501bf-130">目前</span><span class="sxs-lookup"><span data-stu-id="501bf-130">Current</span></span>    |
| <span data-ttu-id="501bf-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="501bf-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="501bf-132">英寸&dagger;</span><span class="sxs-lookup"><span data-stu-id="501bf-132">11&dagger;</span></span> |

<span data-ttu-id="501bf-133">&dagger;需要額外的 polyfills （例如，可透過[Polyfill.io](https://polyfill.io/v3/)配套新增承諾）。</span><span class="sxs-lookup"><span data-stu-id="501bf-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="501bf-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="501bf-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
