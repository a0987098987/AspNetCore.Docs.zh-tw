---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core 支援的平臺 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: adf27fa84acb3929a1639b561c728c2db29723f6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401931"
---
# <a name="aspnet-core-blazor-supported-platforms"></a><span data-ttu-id="48a13-103">ASP.NET Core Blazor 支援的平臺</span><span class="sxs-lookup"><span data-stu-id="48a13-103">ASP.NET Core Blazor supported platforms</span></span>

<span data-ttu-id="48a13-104">作者：[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="48a13-104">By [Luke Latham](https://github.com/guardrex)</span></span>

## <a name="browser-requirements"></a><span data-ttu-id="48a13-105">瀏覽器需求</span><span class="sxs-lookup"><span data-stu-id="48a13-105">Browser requirements</span></span>

### Blazor WebAssembly

| <span data-ttu-id="48a13-106">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="48a13-106">Browser</span></span>                          | <span data-ttu-id="48a13-107">版本</span><span class="sxs-lookup"><span data-stu-id="48a13-107">Version</span></span>               |
| -------------------------------- | :-------------------: |
| <span data-ttu-id="48a13-108">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="48a13-108">Microsoft Edge</span></span>                   | <span data-ttu-id="48a13-109">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-109">Current</span></span>               |
| <span data-ttu-id="48a13-110">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="48a13-110">Mozilla Firefox</span></span>                  | <span data-ttu-id="48a13-111">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-111">Current</span></span>               |
| <span data-ttu-id="48a13-112">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="48a13-112">Google Chrome, including Android</span></span> | <span data-ttu-id="48a13-113">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-113">Current</span></span>               |
| <span data-ttu-id="48a13-114">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="48a13-114">Safari, including iOS</span></span>            | <span data-ttu-id="48a13-115">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-115">Current</span></span>               |
| <span data-ttu-id="48a13-116">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="48a13-116">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="48a13-117">不受支援&dagger;</span><span class="sxs-lookup"><span data-stu-id="48a13-117">Not Supported&dagger;</span></span> |

<span data-ttu-id="48a13-118">&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。</span><span class="sxs-lookup"><span data-stu-id="48a13-118">&dagger;Microsoft Internet Explorer doesn't support [WebAssembly](https://webassembly.org).</span></span>

### Blazor Server

| <span data-ttu-id="48a13-119">瀏覽器</span><span class="sxs-lookup"><span data-stu-id="48a13-119">Browser</span></span>                          | <span data-ttu-id="48a13-120">版本</span><span class="sxs-lookup"><span data-stu-id="48a13-120">Version</span></span>    |
| -------------------------------- | :--------: |
| <span data-ttu-id="48a13-121">Microsoft Edge</span><span class="sxs-lookup"><span data-stu-id="48a13-121">Microsoft Edge</span></span>                   | <span data-ttu-id="48a13-122">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-122">Current</span></span>    |
| <span data-ttu-id="48a13-123">Mozilla Firefox</span><span class="sxs-lookup"><span data-stu-id="48a13-123">Mozilla Firefox</span></span>                  | <span data-ttu-id="48a13-124">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-124">Current</span></span>    |
| <span data-ttu-id="48a13-125">Google Chrome，包括 Android</span><span class="sxs-lookup"><span data-stu-id="48a13-125">Google Chrome, including Android</span></span> | <span data-ttu-id="48a13-126">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-126">Current</span></span>    |
| <span data-ttu-id="48a13-127">Safari，包括 iOS</span><span class="sxs-lookup"><span data-stu-id="48a13-127">Safari, including iOS</span></span>            | <span data-ttu-id="48a13-128">目前</span><span class="sxs-lookup"><span data-stu-id="48a13-128">Current</span></span>    |
| <span data-ttu-id="48a13-129">Microsoft Internet Explorer</span><span class="sxs-lookup"><span data-stu-id="48a13-129">Microsoft Internet Explorer</span></span>      | <span data-ttu-id="48a13-130">英寸&dagger;</span><span class="sxs-lookup"><span data-stu-id="48a13-130">11&dagger;</span></span> |

<span data-ttu-id="48a13-131">&dagger;需要額外的 polyfills （例如，可透過配套新增承諾 [`Polyfill.io`](https://polyfill.io/v3/) ）。</span><span class="sxs-lookup"><span data-stu-id="48a13-131">&dagger;Additional polyfills are required (for example, promises can be added via a [`Polyfill.io`](https://polyfill.io/v3/) bundle).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="48a13-132">其他資源</span><span class="sxs-lookup"><span data-stu-id="48a13-132">Additional resources</span></span>

* <xref:blazor/hosting-models>
