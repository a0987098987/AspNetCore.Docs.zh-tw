---
title: ASP.NET Core MVC 的相容性版本
author: rick-anderson
description: 探索 Startup 類別如何在 ASP.NET Core 中設定服務和應用程式的要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 9/25/2019
uid: mvc/compatibility-version
ms.openlocfilehash: b29e2ee49aaf0f557f1acd0cf03e9e82d5ea0105
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75357727"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="7b353-103">ASP.NET Core MVC 的相容性版本</span><span class="sxs-lookup"><span data-stu-id="7b353-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="7b353-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="7b353-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="7b353-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法是 ASP.NET Core 3.0 應用程式的免費作業。</span><span class="sxs-lookup"><span data-stu-id="7b353-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method is a no-op for ASP.NET Core 3.0 apps.</span></span> <span data-ttu-id="7b353-106">也就是說，以任何 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion> 值呼叫 `SetCompatibilityVersion` 不會影響應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b353-106">That is, calling `SetCompatibilityVersion` with any value of <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion> has no impact on the application.</span></span>

* <span data-ttu-id="7b353-107">下一個次要版本的 ASP.NET Core 可能會提供新的 `CompatibilityVersion` 值。</span><span class="sxs-lookup"><span data-stu-id="7b353-107">The next minor version of ASP.NET Core may provide a new `CompatibilityVersion` value.</span></span>
* <span data-ttu-id="7b353-108">透過 `Version_2_2` `Version_2_0` 的 `CompatibilityVersion` 值會標示 `[Obsolete(...)]`。</span><span class="sxs-lookup"><span data-stu-id="7b353-108">`CompatibilityVersion` values `Version_2_0` through `Version_2_2` are marked `[Obsolete(...)]`.</span></span>
* <span data-ttu-id="7b353-109">請參閱[Antiforgery、CORS、診斷、Mvc 和路由中的重大 API 變更](https://github.com/aspnet/Announcements/issues/387)。</span><span class="sxs-lookup"><span data-stu-id="7b353-109">See [Breaking API changes in Antiforgery, CORS, Diagnostics, Mvc, and Routing](https://github.com/aspnet/Announcements/issues/387).</span></span> <span data-ttu-id="7b353-110">這份清單包含相容性參數的重大變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-110">This list includes breaking changes for compatibility switches.</span></span>

<span data-ttu-id="7b353-111">若要查看 `SetCompatibilityVersion` 如何與 ASP.NET Core 2.x 應用程式搭配運作，請選取[此文章的 ASP.NET Core 2.2 版本](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2)。</span><span class="sxs-lookup"><span data-stu-id="7b353-111">To see how `SetCompatibilityVersion` works with ASP.NET Core 2.x apps, select the [ASP.NET Core 2.2 version of this article](https://docs.microsoft.com/aspnet/core/mvc/compatibility-version?view=aspnetcore-2.2).</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="7b353-112"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓 ASP.NET Core 2.x 應用程式加入宣告或退出 ASP.NET Core MVC 2.1 或2.2 中引進的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-112">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an ASP.NET Core 2.x app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or 2.2.</span></span> <span data-ttu-id="7b353-113">這些可能的重大行為變更通常在於 MVC 子系統的運作方式，以及執行階段呼叫**您的程式碼**的方式。</span><span class="sxs-lookup"><span data-stu-id="7b353-113">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="7b353-114">透過選擇加入，您可以取得最新的行為和 ASP.NET Core 的長期行為。</span><span class="sxs-lookup"><span data-stu-id="7b353-114">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="7b353-115">下列程式碼會將相容性模式設定為 ASP.NET Core 2.2：</span><span class="sxs-lookup"><span data-stu-id="7b353-115">The following code sets the compatibility mode to ASP.NET Core 2.2:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="7b353-116">建議您使用最新版本 (`CompatibilityVersion.Latest`) 測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="7b353-116">We recommend you test your app using the latest version (`CompatibilityVersion.Latest`).</span></span> <span data-ttu-id="7b353-117">預計大部分的應用程式都不會使用最新版本進行重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-117">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="7b353-118">呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` 的應用程式會受到保護，以防止在 ASP.NET Core 2.1/2.2 MVC 版本中引進的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-118">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1/2.2 MVC versions.</span></span> <span data-ttu-id="7b353-119">這項保護：</span><span class="sxs-lookup"><span data-stu-id="7b353-119">This protection:</span></span>

* <span data-ttu-id="7b353-120">不適用於所有 2.1 和更新版本的變更，它的目標是 MVC 子系統中的可能重大 ASP.NET Core 執行階段行為變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-120">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="7b353-121">不會延伸至 ASP.NET Core 3.0。</span><span class="sxs-lookup"><span data-stu-id="7b353-121">Does not extend to ASP.NET Core 3.0.</span></span>

<span data-ttu-id="7b353-122">**未**呼叫 `SetCompatibilityVersion` 之 ASP.NET Core 2.1 和2.2 應用程式的預設相容性為2.0 相容性。</span><span class="sxs-lookup"><span data-stu-id="7b353-122">The default compatibility for ASP.NET Core 2.1 and 2.2 apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="7b353-123">也就是說，不呼叫 `SetCompatibilityVersion` 等同於呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`。</span><span class="sxs-lookup"><span data-stu-id="7b353-123">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="7b353-124">下列程式碼會將相容性模式設定為 ASP.NET Core 2.2，但下列行為除外：</span><span class="sxs-lookup"><span data-stu-id="7b353-124">The following code sets the compatibility mode to ASP.NET Core 2.2, except for the following behaviors:</span></span>

* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowCombiningAuthorizeFilters>
* <xref:Microsoft.AspNetCore.Mvc.MvcOptions.InputFormatterExceptionPolicy>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="7b353-125">對於出現重大行為變更的應用程式，使用適當的相容性參數：</span><span class="sxs-lookup"><span data-stu-id="7b353-125">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="7b353-126">可讓您使用最新版本，並選擇退出特定的重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-126">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="7b353-127">讓您有時間來更新您的應用程式，使其適用於最新的變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-127">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="7b353-128"><xref:Microsoft.AspNetCore.Mvc.MvcOptions> 文件充分說明變更的項目，以及所做變更對於大部分使用者來說都得到改善的原因。</span><span class="sxs-lookup"><span data-stu-id="7b353-128">The <xref:Microsoft.AspNetCore.Mvc.MvcOptions> documentation has a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="7b353-129">使用 ASP.NET Core 3.0，已移除相容性參數所支援的舊行為。</span><span class="sxs-lookup"><span data-stu-id="7b353-129">With ASP.NET Core 3.0, old behaviors supported by compatibility switches have been removed.</span></span> <span data-ttu-id="7b353-130">我們覺得這些是有利於幾乎所有使用者的正向變更。</span><span class="sxs-lookup"><span data-stu-id="7b353-130">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="7b353-131">藉由在2.1 和2.2 中引進這些變更，大部分的應用程式都可以受益，有些則有時間更新。</span><span class="sxs-lookup"><span data-stu-id="7b353-131">By introducing these changes in 2.1 and 2.2, most apps can benefit, while others have time to update.</span></span>
::: moniker-end
