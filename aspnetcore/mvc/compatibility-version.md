---
title: ASP.NET Core MVC 的相容性版本
author: rick-anderson
description: 探索 Startup 類別如何在 ASP.NET Core 中設定服務和應用程式的要求管線。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc
ms.date: 04/20/2018
uid: mvc/compatibility-version
ms.openlocfilehash: bedeeba07dcca19176e779f3541c445e94efcd07
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41910087"
---
# <a name="compatibility-version-for-aspnet-core-mvc"></a><span data-ttu-id="b8be7-103">ASP.NET Core MVC 的相容性版本</span><span class="sxs-lookup"><span data-stu-id="b8be7-103">Compatibility version for ASP.NET Core MVC</span></span>

<span data-ttu-id="b8be7-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="b8be7-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="b8be7-105"><xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> 方法可讓應用程式加入或退出 ASP.NET Core MVC 2.1 或更新版本所引入的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-105">The <xref:Microsoft.Extensions.DependencyInjection.MvcCoreMvcBuilderExtensions.SetCompatibilityVersion*> method allows an app to opt-in or opt-out of potentially breaking behavior changes introduced in ASP.NET Core MVC 2.1 or later.</span></span> <span data-ttu-id="b8be7-106">這些可能的重大行為變更通常在於 MVC 子系統的運作方式，以及執行階段呼叫**您的程式碼**的方式。</span><span class="sxs-lookup"><span data-stu-id="b8be7-106">These potentially breaking behavior changes are generally in how the MVC subsystem behaves and how **your code** is called by the runtime.</span></span> <span data-ttu-id="b8be7-107">透過選擇加入，您可以取得最新的行為和 ASP.NET Core 的長期行為。</span><span class="sxs-lookup"><span data-stu-id="b8be7-107">By opting in, you get the latest behavior, and the long-term behavior of ASP.NET Core.</span></span>

<span data-ttu-id="b8be7-108">下列程式碼會將相容性模式設定為 ASP.NET Core 2.1：</span><span class="sxs-lookup"><span data-stu-id="b8be7-108">The following code sets the compatibility mode to ASP.NET Core 2.1:</span></span>

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup.cs?name=snippet1)]

<span data-ttu-id="b8be7-109">建議您使用最新版本 (`CompatibilityVersion.Version_2_1`) 測試應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8be7-109">We recommend you test your app using the latest version (`CompatibilityVersion.Version_2_1`).</span></span> <span data-ttu-id="b8be7-110">預計大部分的應用程式都不會使用最新版本進行重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-110">We anticipate that most apps won't have breaking behavior changes using the latest version.</span></span>

<span data-ttu-id="b8be7-111">呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` 的應用程式會受到保護，防止執行 ASP.NET Core 2.1 MVC 和更新版本的 2.x 版所引進的可能重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-111">Apps that call `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)` are protected from potentially breaking behavior changes introduced in the ASP.NET Core 2.1 MVC and later 2.x versions.</span></span> <span data-ttu-id="b8be7-112">這項保護：</span><span class="sxs-lookup"><span data-stu-id="b8be7-112">This protection:</span></span>

* <span data-ttu-id="b8be7-113">不適用於所有 2.1 和更新版本的變更，它的目標是 MVC 子系統中的可能重大 ASP.NET Core 執行階段行為變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-113">Does not apply to all 2.1 and later changes, it's targeted to potentially breaking ASP.NET Core runtime behavior changes in the MVC subsystem.</span></span>
* <span data-ttu-id="b8be7-114">不會擴充到下一個主要版本。</span><span class="sxs-lookup"><span data-stu-id="b8be7-114">Does not extend to the next major version.</span></span>

<span data-ttu-id="b8be7-115">適用於 ASP.NET Core 2.1 和更新版本的 2.x 應用程式且**不會**呼叫 `SetCompatibilityVersion` 的預設相容性為 2.0 相容性。</span><span class="sxs-lookup"><span data-stu-id="b8be7-115">The default compatibility for ASP.NET Core 2.1 and later 2.x apps that do **not** call `SetCompatibilityVersion` is 2.0 compatibility.</span></span> <span data-ttu-id="b8be7-116">也就是說，不呼叫 `SetCompatibilityVersion` 等同於呼叫 `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`。</span><span class="sxs-lookup"><span data-stu-id="b8be7-116">That is, not calling `SetCompatibilityVersion` is the same as calling `SetCompatibilityVersion(CompatibilityVersion.Version_2_0)`.</span></span>

<span data-ttu-id="b8be7-117">下列程式碼會將相容性模式設定為 ASP.NET Core 2.1，但下列行為除外：</span><span class="sxs-lookup"><span data-stu-id="b8be7-117">The following code sets the compatibility mode to ASP.NET Core 2.1, except for the following behaviors:</span></span>

* [<span data-ttu-id="b8be7-118">AllowCombiningAuthorizeFilters</span><span class="sxs-lookup"><span data-stu-id="b8be7-118">AllowCombiningAuthorizeFilters</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)
* [<span data-ttu-id="b8be7-119">InputFormatterExceptionPolicy</span><span class="sxs-lookup"><span data-stu-id="b8be7-119">InputFormatterExceptionPolicy</span></span>](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs)

[!code-csharp[Main](compatibility-version/samples/2.x/CompatibilityVersionSample/Startup2.cs?name=snippet1)]

<span data-ttu-id="b8be7-120">對於出現重大行為變更的應用程式，使用適當的相容性參數：</span><span class="sxs-lookup"><span data-stu-id="b8be7-120">For apps that encounter breaking behavior changes, using the appropriate compatibility switches:</span></span>

* <span data-ttu-id="b8be7-121">可讓您使用最新版本，並選擇退出特定的重大行為變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-121">Allows you to use the latest release and opt out of specific breaking behavior changes.</span></span>
* <span data-ttu-id="b8be7-122">讓您有時間來更新您的應用程式，使其適用於最新的變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-122">Gives you time to update your app so it works with the latest changes.</span></span>

<span data-ttu-id="b8be7-123">[MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) 類別來源註解充分說明變更的項目，以及所做變更對於大部分使用者來說都得到改善的原因。</span><span class="sxs-lookup"><span data-stu-id="b8be7-123">The [MvcOptions](https://github.com/aspnet/Mvc/blob/master/src/Microsoft.AspNetCore.Mvc.Core/MvcOptions.cs) class source comments have a good explanation of what changed and why the changes are an improvement for most users.</span></span>

<span data-ttu-id="b8be7-124">在未來某個日期將會推出 [ASP.NET Core 3.0 版](https://github.com/aspnet/Home/wiki/Roadmap)。</span><span class="sxs-lookup"><span data-stu-id="b8be7-124">At some future date, there will be an [ASP.NET Core 3.0 version](https://github.com/aspnet/Home/wiki/Roadmap).</span></span> <span data-ttu-id="b8be7-125">3.0 版將移除相容性參數支援的舊行為。</span><span class="sxs-lookup"><span data-stu-id="b8be7-125">Old behaviors supported by compatibility switches will be removed in the 3.0 version.</span></span> <span data-ttu-id="b8be7-126">我們覺得這些是有利於幾乎所有使用者的正向變更。</span><span class="sxs-lookup"><span data-stu-id="b8be7-126">We feel these are positive changes benefitting nearly all users.</span></span> <span data-ttu-id="b8be7-127">現在引進這些變更，大部分的應用程式皆可獲益，而其他人則有時間來更新其應用程式。</span><span class="sxs-lookup"><span data-stu-id="b8be7-127">By introducing these changes now, most apps can benefit now, and the others will have time to update their apps.</span></span>
