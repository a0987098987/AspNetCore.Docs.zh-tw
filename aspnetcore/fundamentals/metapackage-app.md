---
title: ASP.NET Core 2.1 和更新版本的 Microsoft.AspNetCore.App 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.App 中繼套件包含所有支援的 ASP.NET Core 和 Entity Framework Core 套件。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/20/2017
uid: fundamentals/metapackage-app
ms.openlocfilehash: 68b5aca60273a8c6ef03c0a29842e6a5305adeb3
ms.sourcegitcommit: 517bb1366da2a28b0014e384fa379755c21b47d8
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/26/2018
ms.locfileid: "47230161"
---
# <a name="microsoftaspnetcoreapp-metapackage-for-aspnet-core-21"></a><span data-ttu-id="23d63-103">ASP.NET Core 2.1 的 Microsoft.AspNetCore.App 中繼套件</span><span class="sxs-lookup"><span data-stu-id="23d63-103">Microsoft.AspNetCore.App metapackage for ASP.NET Core 2.1</span></span>

<span data-ttu-id="23d63-104">這項功能需要以 .NET Core 2.1 和更新版本為目標的 ASP.NET Core 2.1 和更新版本。</span><span class="sxs-lookup"><span data-stu-id="23d63-104">This feature requires ASP.NET Core 2.1 and later targeting .NET Core 2.1 and later.</span></span>

<span data-ttu-id="23d63-105">ASP.NET Core 的 [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [中繼套件](/dotnet/core/packages#metapackages)：</span><span class="sxs-lookup"><span data-stu-id="23d63-105">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="23d63-106">不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。</span><span class="sxs-lookup"><span data-stu-id="23d63-106">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="23d63-107">這些協力廠商相依性是確保主要架構功能運作的必要項。</span><span class="sxs-lookup"><span data-stu-id="23d63-107">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="23d63-108">包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="23d63-108">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="23d63-109">包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="23d63-109">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="23d63-110">`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本的所有功能。</span><span class="sxs-lookup"><span data-stu-id="23d63-110">All the features of ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="23d63-111">以 ASP.NET Core 2.1 和更新版本為目標的預設專案範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="23d63-111">The default project templates targeting ASP.NET Core 2.1 and later use this package.</span></span> <span data-ttu-id="23d63-112">建議以 ASP.NET Core 2.1 和更新版本以及 Entity Framework Core 2.1 和更新版本為目標的應用程式使用 `Microsoft.AspNetCore.App` 套件。</span><span class="sxs-lookup"><span data-stu-id="23d63-112">We recommend applications targeting ASP.NET Core 2.1 and later and Entity Framework Core 2.1 and later use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="23d63-113">`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。</span><span class="sxs-lookup"><span data-stu-id="23d63-113">The version number of the `Microsoft.AspNetCore.App` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="23d63-114">使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="23d63-114">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="23d63-115">如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="23d63-115">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="23d63-116">其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。</span><span class="sxs-lookup"><span data-stu-id="23d63-116">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="23d63-117">版本一致性有助於確保可靠的體驗。</span><span class="sxs-lookup"><span data-stu-id="23d63-117">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="23d63-118">`Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。</span><span class="sxs-lookup"><span data-stu-id="23d63-118">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="23d63-119">使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。</span><span class="sxs-lookup"><span data-stu-id="23d63-119">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="23d63-120">當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**在應用程式中部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="23d63-120">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application&mdash;the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="23d63-121">共用架構中的資產會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="23d63-121">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="23d63-122">如需詳細資訊，請參閱 [.NET Core 發佈封裝](/dotnet/core/build/distribution-packaging)中的「共用架構」。</span><span class="sxs-lookup"><span data-stu-id="23d63-122">For more information, see "shared framework" in [.NET Core distribution packaging](/dotnet/core/build/distribution-packaging).</span></span>

<span data-ttu-id="23d63-123">下列專案檔參考 ASP.NET Core 的 `Microsoft.AspNetCore.App` 中繼套件，並呈現出標準的 ASP.NET Core 2.1 範本：</span><span class="sxs-lookup"><span data-stu-id="23d63-123">The following project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core and represents a typical ASP.NET Core 2.1 template:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.1</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="23d63-124">上述標記代表一般 ASP.NET Core 2.1 和更新版本的範本。</span><span class="sxs-lookup"><span data-stu-id="23d63-124">The preceding markup represents a typical ASP.NET Core 2.1 and later template.</span></span> <span data-ttu-id="23d63-125">它不會指定 `Microsoft.AspNetCore.App` 套件參考的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="23d63-125">It doesn't specify a version number for the `Microsoft.AspNetCore.App` package reference.</span></span> <span data-ttu-id="23d63-126">未指定版本時，SDK 會指定[隱含](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)版本，也就是 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="23d63-126">When the version is not specified, an [implicit](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) version is specified by the SDK, that is, `Microsoft.NET.Sdk.Web`.</span></span> <span data-ttu-id="23d63-127">建議依賴 SDK 指定的隱含版本，而不要明確設定套件參考的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="23d63-127">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="23d63-128">如果您對此方法有疑問，請在 [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Microsoft.AspNetCore.App 隱含版本討論區) 留下 GitHub 意見。</span><span class="sxs-lookup"><span data-stu-id="23d63-128">If you have questions on this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="23d63-129">可攜式應用程式的隱含版本會設定為 `major.minor.0`。</span><span class="sxs-lookup"><span data-stu-id="23d63-129">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="23d63-130">共用架構向前復原機制會在已安裝共用架構中的最新相容版本上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="23d63-130">The shared framework roll-forward mechanism will run the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="23d63-131">為了保證開發、測試和生產均使用相同版本，請務必在所有環境中安裝相同版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="23d63-131">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="23d63-132">針對獨立應用程式，隱含版本號碼會設定為已安裝 SDK 隨附之共用架構的 `major.minor.patch`。</span><span class="sxs-lookup"><span data-stu-id="23d63-132">For self contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="23d63-133">指定 `Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會選擇該版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="23d63-133">Specifying a version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be chosen.</span></span> <span data-ttu-id="23d63-134">例如，假設指定版本 "2.1.1"，但安裝 "2.1.3"。</span><span class="sxs-lookup"><span data-stu-id="23d63-134">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="23d63-135">在此情況下，應用程式會使用 "2.1.3"。</span><span class="sxs-lookup"><span data-stu-id="23d63-135">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="23d63-136">您可以停用向前復原 (修補及/或次要)，但不建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="23d63-136">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="23d63-137">如需 dotnet 主機向前復原及如何設定其行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。</span><span class="sxs-lookup"><span data-stu-id="23d63-137">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="23d63-138">`<Project Sdk` 必須設定為 `Microsoft.NET.Sdk.Web`，才能使用隱含版本 `Microsoft.AspNetCore.App`。</span><span class="sxs-lookup"><span data-stu-id="23d63-138">`<Project Sdk` must be set to `Microsoft.NET.Sdk.Web` to use the implicit version `Microsoft.AspNetCore.App`.</span></span>  <span data-ttu-id="23d63-139">當使用 `<Project Sdk="Microsoft.NET.Sdk">` (不含後置 `.Web`) 時：</span><span class="sxs-lookup"><span data-stu-id="23d63-139">When `<Project Sdk="Microsoft.NET.Sdk">` (without the trailing `.Web`) is used:</span></span>

* <span data-ttu-id="23d63-140">會產生下列警告：</span><span class="sxs-lookup"><span data-stu-id="23d63-140">The following warning is generated:</span></span>

     <span data-ttu-id="23d63-141">*警告 NU1604: 專案相依性 Microsoft.AspNetCore.App 未包含內含的下限。請在相依性版本包含下限，以確保還原結果一致。*</span><span class="sxs-lookup"><span data-stu-id="23d63-141">*Warning NU1604: Project dependency Microsoft.AspNetCore.App does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>
* <span data-ttu-id="23d63-142">這是.NET Core 2.1 SDK 的已知的問題，並將於 .NET Core 2.2 SDK 中修正。</span><span class="sxs-lookup"><span data-stu-id="23d63-142">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

<a name="update"></a>

## <a name="update-aspnet-core"></a><span data-ttu-id="23d63-143">更新 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="23d63-143">Update ASP.NET Core</span></span>

<span data-ttu-id="23d63-144">`Microsoft.AspNetCore.App` [中繼套件](/dotnet/core/packages#metapackages)不是從 NuGet 更新的傳統套件。</span><span class="sxs-lookup"><span data-stu-id="23d63-144">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) isn't a traditional package that's updated from NuGet.</span></span> <span data-ttu-id="23d63-145">類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。</span><span class="sxs-lookup"><span data-stu-id="23d63-145">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="23d63-146">如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。</span><span class="sxs-lookup"><span data-stu-id="23d63-146">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="23d63-147">更新 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="23d63-147">To update ASP.NET Core:</span></span>

* <span data-ttu-id="23d63-148">在開發電腦和組建伺服器上：下載並安裝 [.NET Core SDK](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="23d63-148">On development machines and build servers: Download and install the [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="23d63-149">在部署伺服器上：下載並安裝 [.NET Core 執行階段](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="23d63-149">On deployment servers: Download and install the [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>

 <span data-ttu-id="23d63-150">應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。</span><span class="sxs-lookup"><span data-stu-id="23d63-150">Applications will roll forward to the latest installed version on application restart.</span></span> <span data-ttu-id="23d63-151">您不必更新專案檔中的 `Microsoft.AspNetCore.App` 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="23d63-151">It's not necessary to update the `Microsoft.AspNetCore.App` version number in the project file.</span></span> <span data-ttu-id="23d63-152">如需詳細資訊，請參閱[向前復原 Framework 相依的應用程式](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="23d63-152">For more information, see [Framework-dependent apps roll forward](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="23d63-153">如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。</span><span class="sxs-lookup"><span data-stu-id="23d63-153">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>
