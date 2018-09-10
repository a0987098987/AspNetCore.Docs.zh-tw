---
title: ASP.NET Core 2.1 和更新版本的 Microsoft.AspNetCore.App 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.App 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 95fd6b7e73cf325674f1c1e03f9eea88cbc1af13
ms.sourcegitcommit: f3538693a12cf55b7f124a6519677239170b7c43
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/28/2018
ms.locfileid: "43114771"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="060ca-103">ASP.NET Core 2.1 的 Microsoft.AspNetCore.App 中繼套件</span><span class="sxs-lookup"><span data-stu-id="060ca-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="060ca-104">這項功能需要以 .NET Core 2.1 和更新版本為目標的 ASP.NET Core 2.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="060ca-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="060ca-105">ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [中繼套件](/dotnet/core/packages#metapackages)：</span><span class="sxs-lookup"><span data-stu-id="060ca-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="060ca-106">不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。</span><span class="sxs-lookup"><span data-stu-id="060ca-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="060ca-107">這些協力廠商相依性是確保主要架構功能運作的必要項。</span><span class="sxs-lookup"><span data-stu-id="060ca-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="060ca-108">包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="060ca-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="060ca-109">包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="060ca-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="060ca-110">`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本的所有功能。</span><span class="sxs-lookup"><span data-stu-id="060ca-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="060ca-111">以 ASP.NET Core 2.1 和更新版本為目標的預設專案範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="060ca-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="060ca-112">建議以 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本為目標的應用程式使用 `Microsoft.AspNetCore.App` 套件。</span><span class="sxs-lookup"><span data-stu-id="060ca-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="060ca-113">`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。</span><span class="sxs-lookup"><span data-stu-id="060ca-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="060ca-114">使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="060ca-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="060ca-115">如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="060ca-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="060ca-116">其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。</span><span class="sxs-lookup"><span data-stu-id="060ca-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="060ca-117">版本一致性有助於確保可靠的體驗。</span><span class="sxs-lookup"><span data-stu-id="060ca-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="060ca-118">`Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。</span><span class="sxs-lookup"><span data-stu-id="060ca-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="060ca-119">使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。</span><span class="sxs-lookup"><span data-stu-id="060ca-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="060ca-120">當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**在應用程式中部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="060ca-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application&mdash;the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="060ca-121">共用架構中的資產會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="060ca-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="060ca-122">如需詳細資訊，請參閱 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)中的「共用架構」。</span><span class="sxs-lookup"><span data-stu-id="060ca-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="060ca-123">下列專案檔參考 ASP.NET Core 的 `Microsoft.AspNetCore.App` 中繼套件，並呈現出標準的 ASP.NET Core 2.1 範本：</span><span class="sxs-lookup"><span data-stu-id="060ca-123">The following project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core and represents a typical ASP.NET Core 2.1 template:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" Version="2.1.1" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="060ca-124">`Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會使用該版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="060ca-124">The version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be used.</span></span> <span data-ttu-id="060ca-125">例如，假設指定版本 `2.1.1`，但安裝 `2.1.3`。</span><span class="sxs-lookup"><span data-stu-id="060ca-125">For example, suppose version `2.1.1` is specified, but `2.1.3` is installed.</span></span> <span data-ttu-id="060ca-126">在此情況下，應用程式會使用 `2.1.3`。</span><span class="sxs-lookup"><span data-stu-id="060ca-126">In that case, the app uses `2.1.3`.</span></span> <span data-ttu-id="060ca-127">您可以停用向前復原行為 (修補及 (或) 次要)，但不建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="060ca-127">Although not recommended, you can disable roll-forward behavior (patch and/or minor).</span></span> <span data-ttu-id="060ca-128">如需套件版本向前復原行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。</span><span class="sxs-lookup"><span data-stu-id="060ca-128">For more information on package version roll-forward behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

## <a name="update-aspnet-core"></a><span data-ttu-id="060ca-129">更新 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="060ca-129">Update ASP.NET Core</span></span>

<span data-ttu-id="060ca-130">`Microsoft.AspNetCore.App` [中繼套件](/dotnet/core/packages#metapackages)不是從 NuGet 更新的傳統套件。</span><span class="sxs-lookup"><span data-stu-id="060ca-130">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) isn't a traditional package that's updated from NuGet.</span></span> <span data-ttu-id="060ca-131">類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。</span><span class="sxs-lookup"><span data-stu-id="060ca-131">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="060ca-132">如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。</span><span class="sxs-lookup"><span data-stu-id="060ca-132">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="060ca-133">更新 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="060ca-133">To update ASP.NET Core:</span></span>

* <span data-ttu-id="060ca-134">在開發電腦和組建伺服器上：下載並安裝 [.NET Core SDK](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="060ca-134">On development machines and build servers: Download and install the [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="060ca-135">在部署伺服器上：下載並安裝 [.NET Core 執行階段](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="060ca-135">On deployment servers: Download and install the [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>

 <span data-ttu-id="060ca-136">應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。</span><span class="sxs-lookup"><span data-stu-id="060ca-136">Applications will roll forward to the latest installed version on application restart.</span></span> <span data-ttu-id="060ca-137">您不必更新專案檔中的 `Microsoft.AspNetCore.App` 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="060ca-137">It's not necessary to update the `Microsoft.AspNetCore.App` version number in the project file.</span></span> <span data-ttu-id="060ca-138">如需詳細資訊，請參閱[向前復原 Framework 相依的應用程式](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="060ca-138">For more information, see [Framework-dependent apps roll forward](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="060ca-139">如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。</span><span class="sxs-lookup"><span data-stu-id="060ca-139">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>
