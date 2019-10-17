---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor 的支援平臺。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 4e86bd6967a747a59c99a515c1c838cc2c21770f
ms.sourcegitcommit: 35a86ce48041caaf6396b1e88b0472578ba24483
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/16/2019
ms.locfileid: "72391222"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="87658-103">ASP.NET Core Blazor 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="87658-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="87658-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="87658-104">By [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a><span data-ttu-id="87658-105">瀏覽器需求</span><span class="sxs-lookup"><span data-stu-id="87658-105">Browser requirements</span></span>

### <a name="blazor-webassembly"></a><span data-ttu-id="87658-106">Blazor WebAssembly</span><span class="sxs-lookup"><span data-stu-id="87658-106">Blazor WebAssembly</span></span>

| <span data-ttu-id="87658-107">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="87658-107">Browser</span></span>                          | <span data-ttu-id="87658-108">版本</span><span class="sxs-lookup"><span data-stu-id="87658-108">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="87658-109">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="87658-109">Microsoft Edge</span></span>                   | <span data-ttu-id="87658-110">目前</span><span class="sxs-lookup"><span data-stu-id="87658-110">Current</span></span>               |
| <span data-ttu-id="87658-111">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="87658-111">Mozilla Firefox</span></span>                  | <span data-ttu-id="87658-112">目前</span><span class="sxs-lookup"><span data-stu-id="87658-112">Current</span></span>               |
| <span data-ttu-id="87658-113">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="87658-113">Google Chrome, including Android</span></span> | <span data-ttu-id="87658-114">目前</span><span class="sxs-lookup"><span data-stu-id="87658-114">Current</span></span>               |
| <span data-ttu-id="87658-115">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="87658-115">Safari, including iOS</span></span>            | <span data-ttu-id="87658-116">目前</span><span class="sxs-lookup"><span data-stu-id="87658-116">Current</span></span>               |
| <span data-ttu-id="87658-117">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="87658-117">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="87658-118">不支援 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="87658-118">Not Supported&dagger;</span></span> |

<span data-ttu-id="87658-119">@no__t 0Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。</span><span class="sxs-lookup"><span data-stu-id="87658-119">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### <a name="blazor-server"></a><span data-ttu-id="87658-120">Blazor 伺服器</span><span class="sxs-lookup"><span data-stu-id="87658-120">Blazor Server</span></span>

| <span data-ttu-id="87658-121">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="87658-121">Browser</span></span>                          | <span data-ttu-id="87658-122">版本</span><span class="sxs-lookup"><span data-stu-id="87658-122">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="87658-123">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="87658-123">Microsoft Edge</span></span>                   | <span data-ttu-id="87658-124">目前</span><span class="sxs-lookup"><span data-stu-id="87658-124">Current</span></span>    |
| <span data-ttu-id="87658-125">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="87658-125">Mozilla Firefox</span></span>                  | <span data-ttu-id="87658-126">目前</span><span class="sxs-lookup"><span data-stu-id="87658-126">Current</span></span>    |
| <span data-ttu-id="87658-127">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="87658-127">Google Chrome, including Android</span></span> | <span data-ttu-id="87658-128">目前</span><span class="sxs-lookup"><span data-stu-id="87658-128">Current</span></span>    |
| <span data-ttu-id="87658-129">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="87658-129">Safari, including iOS</span></span>            | <span data-ttu-id="87658-130">目前</span><span class="sxs-lookup"><span data-stu-id="87658-130">Current</span></span>    |
| <span data-ttu-id="87658-131">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="87658-131">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="87658-132">11 @ no__t-0</span><span class="sxs-lookup"><span data-stu-id="87658-132">11&dagger;</span></span> |

<span data-ttu-id="87658-133">需要 @no__t 0Additional polyfills （例如，可透過[Polyfill.io](https://polyfill.io/v3/)配套新增承諾）。</span><span class="sxs-lookup"><span data-stu-id="87658-133">&dagger;Additional polyfills are required (for example, promises can be added via a [Polyfill.io](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="87658-134">其他資源</span><span class="sxs-lookup"><span data-stu-id="87658-134">Additional resources</span></span>

* <xref:blazor/hosting-models>
