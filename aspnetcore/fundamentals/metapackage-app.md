---
title: 適用于 ASP.NET Core 的 AspNetCore 應用程式中繼套件
author: Rick-Anderson
description: AspNetCore 應用程式共用架構
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 09/24/2019
uid: fundamentals/metapackage-app
ms.openlocfilehash: b30c90116f5a53ba487f88544514f36e388233d3
ms.sourcegitcommit: d64ef143c64ee4fdade8f9ea0b753b16752c5998
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/18/2020
ms.locfileid: "79511375"
---
# <a name="microsoftaspnetcoreapp-for-aspnet-core"></a><span data-ttu-id="5622c-103">ASP.NET Core 的 AspNetCore 應用程式</span><span class="sxs-lookup"><span data-stu-id="5622c-103">Microsoft.AspNetCore.App for ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

 <span data-ttu-id="5622c-104">ASP.NET Core 共用架構（`Microsoft.AspNetCore.App`）包含 Microsoft 開發及支援的元件。</span><span class="sxs-lookup"><span data-stu-id="5622c-104">The ASP.NET Core shared framework (`Microsoft.AspNetCore.App`) contains assemblies that are developed and supported by Microsoft.</span></span> <span data-ttu-id="5622c-105">安裝[.Net Core 3.0 或更新版本的 SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0)時，會安裝 `Microsoft.AspNetCore.App`。</span><span class="sxs-lookup"><span data-stu-id="5622c-105">`Microsoft.AspNetCore.App` is installed when the [.NET Core 3.0 or later SDK](https://dotnet.microsoft.com/download/dotnet-core/3.0) is installed.</span></span> <span data-ttu-id="5622c-106">*共用架構*是一組安裝在電腦上的元件（ *.dll*檔案），其中包含執行時間元件和目標套件。</span><span class="sxs-lookup"><span data-stu-id="5622c-106">The *shared framework* is the set of assemblies (*.dll* files) that are installed on the machine and includes a runtime component and a targeting pack.</span></span> <span data-ttu-id="5622c-107">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="5622c-107">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

* <span data-ttu-id="5622c-108">以 `Microsoft.NET.Sdk.Web` SDK 為目標的專案會隱含地參考 `Microsoft.AspNetCore.App` 架構。</span><span class="sxs-lookup"><span data-stu-id="5622c-108">Projects that target the `Microsoft.NET.Sdk.Web` SDK implicitly reference the `Microsoft.AspNetCore.App` framework.</span></span>

<span data-ttu-id="5622c-109">這些專案不需要其他參考：</span><span class="sxs-lookup"><span data-stu-id="5622c-109">No additional references are required for these projects:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">
  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>
    ...
</Project>
```

<span data-ttu-id="5622c-110">ASP.NET Core 共用架構：</span><span class="sxs-lookup"><span data-stu-id="5622c-110">The ASP.NET Core shared framework:</span></span>

* <span data-ttu-id="5622c-111">不包含協力廠商相依性。</span><span class="sxs-lookup"><span data-stu-id="5622c-111">Doesn't include third-party dependencies.</span></span>
* <span data-ttu-id="5622c-112">包含 ASP.NET Core 小組所支援的所有封裝。</span><span class="sxs-lookup"><span data-stu-id="5622c-112">Includes all supported packages by the ASP.NET Core team.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="5622c-113">這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="5622c-113">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="5622c-114">適用于 ASP.NET Core 的[AspNetCore 應用程式](https://www.nuget.org/packages/Microsoft.AspNetCore.App)[中繼套件](/dotnet/core/packages#metapackages)：</span><span class="sxs-lookup"><span data-stu-id="5622c-114">The [Microsoft.AspNetCore.App](https://www.nuget.org/packages/Microsoft.AspNetCore.App) [metapackage](/dotnet/core/packages#metapackages) for ASP.NET Core:</span></span>

* <span data-ttu-id="5622c-115">不包含協力廠商相依性，[Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/)、[Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/) 和 [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/) 除外。</span><span class="sxs-lookup"><span data-stu-id="5622c-115">Does not include third-party dependencies except for [Json.NET](https://www.nuget.org/packages/Newtonsoft.Json/), [Remotion.Linq](https://www.nuget.org/packages/Remotion.Linq/), and [IX-Async](https://www.nuget.org/packages/System.Interactive.Async/).</span></span> <span data-ttu-id="5622c-116">這些協力廠商相依性是確保主要架構功能運作的必要項。</span><span class="sxs-lookup"><span data-stu-id="5622c-116">These 3rd-party dependencies are deemed necessary to ensure the major frameworks features function.</span></span>
* <span data-ttu-id="5622c-117">包含所有由 ASP.NET Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="5622c-117">Includes all supported packages by the ASP.NET Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>
* <span data-ttu-id="5622c-118">包含所有由 Entity Framework Core 小組支援的套件，除了含有協力廠商相依性的套件以外 (非先前所述)。</span><span class="sxs-lookup"><span data-stu-id="5622c-118">Includes all supported packages by the Entity Framework Core team except those that contain third-party dependencies (other than those previously mentioned).</span></span>

<span data-ttu-id="5622c-119">`Microsoft.AspNetCore.App` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="5622c-119">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.App` package.</span></span> <span data-ttu-id="5622c-120">以 ASP.NET Core 2.x 為目標的預設專案範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="5622c-120">The default project templates targeting ASP.NET Core 2.x use this package.</span></span> <span data-ttu-id="5622c-121">我們建議以 ASP.NET Core 2.x 和 Entity Framework Core 2.x 為目標的應用程式會使用 `Microsoft.AspNetCore.App` 套件。</span><span class="sxs-lookup"><span data-stu-id="5622c-121">We recommend applications targeting ASP.NET Core 2.x and Entity Framework Core 2.x use the `Microsoft.AspNetCore.App` package.</span></span>

<span data-ttu-id="5622c-122">`Microsoft.AspNetCore.App` 中繼套件的版本號碼代表最低的 ASP.NET Core 版本和 Entity Framework Core 版本。</span><span class="sxs-lookup"><span data-stu-id="5622c-122">The version number of the `Microsoft.AspNetCore.App` metapackage represents the minimum ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="5622c-123">使用 `Microsoft.AspNetCore.App` 中繼套件提供版本限制來保護您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="5622c-123">Using the `Microsoft.AspNetCore.App` metapackage provides version restrictions that protect your app:</span></span>

* <span data-ttu-id="5622c-124">如果內含套件具有 `Microsoft.AspNetCore.App` 中套件的可轉移 (非直接) 相依性，而且這些版本號碼不同，則 NuGet 會產生錯誤。</span><span class="sxs-lookup"><span data-stu-id="5622c-124">If a package is included that has a transitive (not direct) dependency on a package in `Microsoft.AspNetCore.App`, and those version numbers differ, NuGet will generate an error.</span></span>
* <span data-ttu-id="5622c-125">其他新增至您應用程式的套件無法變更 `Microsoft.AspNetCore.App` 中包含的套件版本。</span><span class="sxs-lookup"><span data-stu-id="5622c-125">Other packages added to your app cannot change the version of packages included in `Microsoft.AspNetCore.App`.</span></span>
* <span data-ttu-id="5622c-126">版本一致性有助於確保可靠的體驗。</span><span class="sxs-lookup"><span data-stu-id="5622c-126">Version consistency ensures a reliable experience.</span></span> <span data-ttu-id="5622c-127">`Microsoft.AspNetCore.App` 的設計目的是為了防止相關位元之未經測試的版本組合在同一個應用程式中搭配使用。</span><span class="sxs-lookup"><span data-stu-id="5622c-127">`Microsoft.AspNetCore.App` was designed to prevent untested version combinations of related bits being used together in the same app.</span></span>

<span data-ttu-id="5622c-128">使用 `Microsoft.AspNetCore.App` 中繼套件的應用程式會自動利用 ASP.NET Core 共用架構。</span><span class="sxs-lookup"><span data-stu-id="5622c-128">Applications that use the `Microsoft.AspNetCore.App` metapackage automatically take advantage of the ASP.NET Core shared framework.</span></span> <span data-ttu-id="5622c-129">當您使用 `Microsoft.AspNetCore.App` 中繼套件時，**不會**在應用程式中部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; ASP.NET Core 共用架構包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="5622c-129">When you use the `Microsoft.AspNetCore.App` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application&mdash;the ASP.NET Core shared framework contains these assets.</span></span> <span data-ttu-id="5622c-130">共用架構中的資產會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="5622c-130">The assets in the shared framework are precompiled to improve application startup time.</span></span> <span data-ttu-id="5622c-131">如需詳細資訊，請參閱[共用的架構](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="5622c-131">For more information, see [The shared framework](https://natemcmaster.com/blog/2018/08/29/netcore-primitives-2/).</span></span>

<span data-ttu-id="5622c-132">下列專案檔會參考 ASP.NET Core 的 `Microsoft.AspNetCore.App` 中繼套件，並代表一般的 ASP.NET Core 2.2 範本：</span><span class="sxs-lookup"><span data-stu-id="5622c-132">The following project file references the `Microsoft.AspNetCore.App` metapackage for ASP.NET Core and represents a typical ASP.NET Core 2.2 template:</span></span>

```xml
<Project Sdk="Microsoft.NET.Sdk.Web">

  <PropertyGroup>
    <TargetFramework>netcoreapp2.2</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

</Project>
```

<span data-ttu-id="5622c-133">上述標記代表一般 ASP.NET Core 2.x 範本。</span><span class="sxs-lookup"><span data-stu-id="5622c-133">The preceding markup represents a typical ASP.NET Core 2.x template.</span></span> <span data-ttu-id="5622c-134">它不會指定 `Microsoft.AspNetCore.App` 套件參考的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="5622c-134">It doesn't specify a version number for the `Microsoft.AspNetCore.App` package reference.</span></span> <span data-ttu-id="5622c-135">未指定版本時，SDK 會指定[隱含](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md)版本，也就是 `Microsoft.NET.Sdk.Web`。</span><span class="sxs-lookup"><span data-stu-id="5622c-135">When the version is not specified, an [implicit](https://github.com/dotnet/core/blob/master/release-notes/1.0/sdk/1.0-rc3-implicit-package-refs.md) version is specified by the SDK, that is, `Microsoft.NET.Sdk.Web`.</span></span> <span data-ttu-id="5622c-136">建議依賴 SDK 指定的隱含版本，而不要明確設定套件參考的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="5622c-136">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="5622c-137">如果您對此方法有疑問，請在 [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/dotnet/AspNetCore.Docs/issues/6430) (Microsoft.AspNetCore.App 隱含版本討論區) 留下 GitHub 意見。</span><span class="sxs-lookup"><span data-stu-id="5622c-137">If you have questions on this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/dotnet/AspNetCore.Docs/issues/6430).</span></span>

<span data-ttu-id="5622c-138">可攜式應用程式的隱含版本會設定為 `major.minor.0`。</span><span class="sxs-lookup"><span data-stu-id="5622c-138">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="5622c-139">共用架構向前復原機制會在已安裝共用架構中的最新相容版本上執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="5622c-139">The shared framework roll-forward mechanism will run the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="5622c-140">為了保證開發、測試和生產均使用相同版本，請務必在所有環境中安裝相同版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="5622c-140">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="5622c-141">針對獨立應用程式，隱含版本號碼會設定為已安裝 SDK 隨附之共用架構的 `major.minor.patch`。</span><span class="sxs-lookup"><span data-stu-id="5622c-141">For self contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="5622c-142">指定 `Microsoft.AspNetCore.App` 參考的版本號碼**不**保證會選擇該版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="5622c-142">Specifying a version number on the `Microsoft.AspNetCore.App` reference does **not** guarantee that version of the shared framework will be chosen.</span></span> <span data-ttu-id="5622c-143">例如，假設已指定 "2.2.1" 版本，但已安裝 "2.2.3"。</span><span class="sxs-lookup"><span data-stu-id="5622c-143">For example, suppose version "2.2.1" is specified, but "2.2.3" is installed.</span></span> <span data-ttu-id="5622c-144">在此情況下，應用程式將會使用 "2.2.3"。</span><span class="sxs-lookup"><span data-stu-id="5622c-144">In that case, the app will use "2.2.3".</span></span> <span data-ttu-id="5622c-145">您可以停用向前復原 (修補及/或次要)，但不建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="5622c-145">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="5622c-146">如需 dotnet 主機向前復原及如何設定其行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。</span><span class="sxs-lookup"><span data-stu-id="5622c-146">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="5622c-147">`<Project Sdk` 必須設定為 `Microsoft.NET.Sdk.Web`，才能使用隱含版本 `Microsoft.AspNetCore.App`。</span><span class="sxs-lookup"><span data-stu-id="5622c-147">`<Project Sdk` must be set to `Microsoft.NET.Sdk.Web` to use the implicit version `Microsoft.AspNetCore.App`.</span></span> <span data-ttu-id="5622c-148">當使用 `<Project Sdk="Microsoft.NET.Sdk">` (不含後置 `.Web`) 時：</span><span class="sxs-lookup"><span data-stu-id="5622c-148">When `<Project Sdk="Microsoft.NET.Sdk">` (without the trailing `.Web`) is used:</span></span>

* <span data-ttu-id="5622c-149">會產生下列警告：</span><span class="sxs-lookup"><span data-stu-id="5622c-149">The following warning is generated:</span></span>

  <span data-ttu-id="5622c-150">*警告 NU1604：專案相依性 AspNetCore。應用程式未包含內含下限。請在相依性版本中包含下限，以確保一致的還原結果。*</span><span class="sxs-lookup"><span data-stu-id="5622c-150">*Warning NU1604: Project dependency Microsoft.AspNetCore.App does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

* <span data-ttu-id="5622c-151">這是 .NET Core 2.1 SDK 的已知問題。</span><span class="sxs-lookup"><span data-stu-id="5622c-151">This is a known issue with the .NET Core 2.1 SDK.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<a name="update"></a>

## <a name="update-aspnet-core"></a><span data-ttu-id="5622c-152">更新 ASP.NET Core</span><span class="sxs-lookup"><span data-stu-id="5622c-152">Update ASP.NET Core</span></span>

<span data-ttu-id="5622c-153">`Microsoft.AspNetCore.App`[中繼套件](/dotnet/core/packages#metapackages)並不是從 NuGet 更新的傳統套件。</span><span class="sxs-lookup"><span data-stu-id="5622c-153">The `Microsoft.AspNetCore.App` [metapackage](/dotnet/core/packages#metapackages) isn't a traditional package that's updated from NuGet.</span></span> <span data-ttu-id="5622c-154">類似於 `Microsoft.NETCore.App`，`Microsoft.AspNetCore.App` 代表共用執行階段，其具有在 NuGet 外部處理的特殊版本控制語意。</span><span class="sxs-lookup"><span data-stu-id="5622c-154">Similar to `Microsoft.NETCore.App`, `Microsoft.AspNetCore.App` represents a shared runtime, which has special versioning semantics handled outside of NuGet.</span></span> <span data-ttu-id="5622c-155">如需詳細資訊，請參閱[套件、中繼套件和架構](/dotnet/core/packages)。</span><span class="sxs-lookup"><span data-stu-id="5622c-155">For more information, see [Packages, metapackages and frameworks](/dotnet/core/packages).</span></span>

<span data-ttu-id="5622c-156">更新 ASP.NET Core：</span><span class="sxs-lookup"><span data-stu-id="5622c-156">To update ASP.NET Core:</span></span>

* <span data-ttu-id="5622c-157">在開發電腦和組建伺服器上：下載並安裝 [.NET Core SDK](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="5622c-157">On development machines and build servers: Download and install the [.NET Core SDK](https://dotnet.microsoft.com/download).</span></span>
* <span data-ttu-id="5622c-158">在部署伺服器上：下載並安裝 [.NET Core 執行階段](https://dotnet.microsoft.com/download)。</span><span class="sxs-lookup"><span data-stu-id="5622c-158">On deployment servers: Download and install the [.NET Core runtime](https://dotnet.microsoft.com/download).</span></span>

 <span data-ttu-id="5622c-159">應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。</span><span class="sxs-lookup"><span data-stu-id="5622c-159">Applications will roll forward to the latest installed version on application restart.</span></span> <span data-ttu-id="5622c-160">您不必更新專案檔中的 `Microsoft.AspNetCore.App` 版本號碼。</span><span class="sxs-lookup"><span data-stu-id="5622c-160">It's not necessary to update the `Microsoft.AspNetCore.App` version number in the project file.</span></span> <span data-ttu-id="5622c-161">如需詳細資訊，請參閱[向前復原 Framework 相依的應用程式](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward)。</span><span class="sxs-lookup"><span data-stu-id="5622c-161">For more information, see [Framework-dependent apps roll forward](/dotnet/core/versions/selection#framework-dependent-apps-roll-forward).</span></span>

<span data-ttu-id="5622c-162">如果您的應用程式先前使用 `Microsoft.AspNetCore.All`，請參閱[從 Microsoft.AspNetCore.All 移轉至 Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate)。</span><span class="sxs-lookup"><span data-stu-id="5622c-162">If your application previously used `Microsoft.AspNetCore.All`, see [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](xref:fundamentals/metapackage#migrate).</span></span>

::: moniker-end
