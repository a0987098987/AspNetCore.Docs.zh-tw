---
title: ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件
author: Rick-Anderson
description: Microsoft.AspNetCore.All 中繼套件不建議用於 ASP.NET Core 2.1 和更新版本。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/25/2018
uid: fundamentals/metapackage
ms.openlocfilehash: d95bafd412969bb8db38499bd2ff01af510d872c
ms.sourcegitcommit: 54655f1e1abf0b64d19506334d94cfdb0caf55f6
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/26/2018
ms.locfileid: "50148846"
---
# <a name="microsoftaspnetcoreall-metapackage-for-aspnet-core-20"></a><span data-ttu-id="f1aab-103">ASP.NET Core 2.0 的 Microsoft.AspNetCore.All 中繼套件</span><span class="sxs-lookup"><span data-stu-id="f1aab-103">Microsoft.AspNetCore.All metapackage for ASP.NET Core 2.0</span></span>

> [!NOTE]
> <span data-ttu-id="f1aab-104">建議以 ASP.NET Core 2.1 及更新版本為目標的應用程式使用 [Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，而非此套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-104">We recommend applications targeting ASP.NET Core 2.1 and later use the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) rather than this package.</span></span> <span data-ttu-id="f1aab-105">請參閱本文章中的[從 Microsoft.AspNetCore.All 移轉到 Microsoft.AspNetCore.App](#migrate)。</span><span class="sxs-lookup"><span data-stu-id="f1aab-105">See [Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App](#migrate) in this article.</span></span>

<span data-ttu-id="f1aab-106">這項功能需要以 .NET Core 2.x 為目標的 ASP.NET Core 2.x。</span><span class="sxs-lookup"><span data-stu-id="f1aab-106">This feature requires ASP.NET Core 2.x targeting .NET Core 2.x.</span></span>

<span data-ttu-id="f1aab-107">ASP.NET Core 的 [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) 中繼套件包括：</span><span class="sxs-lookup"><span data-stu-id="f1aab-107">The [Microsoft.AspNetCore.All](https://www.nuget.org/packages/Microsoft.AspNetCore.All) metapackage for ASP.NET Core includes:</span></span>

* <span data-ttu-id="f1aab-108">所有由 ASP.NET Core 小組支援的套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-108">All supported packages by the ASP.NET Core team.</span></span>
* <span data-ttu-id="f1aab-109">所有由 Entity Framework Core 支援的套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-109">All supported packages by the Entity Framework Core.</span></span>
* <span data-ttu-id="f1aab-110">ASP.NET Core 與 Entity Framework Core 所使用的內部與第三人相依性。</span><span class="sxs-lookup"><span data-stu-id="f1aab-110">Internal and 3rd-party dependencies used by ASP.NET Core and Entity Framework Core.</span></span>

<span data-ttu-id="f1aab-111">`Microsoft.AspNetCore.All` 套件包含 ASP.NET Core 2.x 和 Entity Framework Core 2.x 的所有功能。</span><span class="sxs-lookup"><span data-stu-id="f1aab-111">All the features of ASP.NET Core 2.x and Entity Framework Core 2.x are included in the `Microsoft.AspNetCore.All` package.</span></span> <span data-ttu-id="f1aab-112">以 ASP.NET Core 2.0 為目標的預設專案範本會使用此套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-112">The default project templates targeting ASP.NET Core 2.0 use this package.</span></span>

<span data-ttu-id="f1aab-113">`Microsoft.AspNetCore.All` 中繼套件的版本號碼代表 ASP.NET Core 版本和 Entity Framework Core 版本。</span><span class="sxs-lookup"><span data-stu-id="f1aab-113">The version number of the `Microsoft.AspNetCore.All` metapackage represents the ASP.NET Core version and Entity Framework Core version.</span></span>

<span data-ttu-id="f1aab-114">使用 `Microsoft.AspNetCore.All` 中繼套件的應用程式會自動利用 [.NET Core 執行階段存放區](/dotnet/core/deploying/runtime-store)。</span><span class="sxs-lookup"><span data-stu-id="f1aab-114">Applications that use the `Microsoft.AspNetCore.All` metapackage automatically take advantage of the [.NET Core Runtime Store](/dotnet/core/deploying/runtime-store).</span></span> <span data-ttu-id="f1aab-115">此執行階段存放區包含執行 ASP.NET Core 2.x 應用程式所需的所有執行階段資產。</span><span class="sxs-lookup"><span data-stu-id="f1aab-115">The Runtime Store contains all the runtime assets needed to run ASP.NET Core 2.x applications.</span></span> <span data-ttu-id="f1aab-116">當您使用 `Microsoft.AspNetCore.All` 中繼套件時，**不會**使用應用程式部署所參考之 ASP.NET Core NuGet 套件的任何資產 &mdash; .NET Core 執行階段存放區包含這些資產。</span><span class="sxs-lookup"><span data-stu-id="f1aab-116">When you use the `Microsoft.AspNetCore.All` metapackage, **no** assets from the referenced ASP.NET Core NuGet packages are deployed with the application &mdash; the .NET Core Runtime Store contains these assets.</span></span> <span data-ttu-id="f1aab-117">執行階段存放區中的資產會先行編譯，以改善應用程式啟動時間。</span><span class="sxs-lookup"><span data-stu-id="f1aab-117">The assets in the Runtime Store are precompiled to improve application startup time.</span></span>

<span data-ttu-id="f1aab-118">您可以使用套件修剪流程來移除未使用的套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-118">You can use the package trimming process to remove packages that you don't use.</span></span> <span data-ttu-id="f1aab-119">發行的應用程式輸出中會排除已修剪的套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-119">Trimmed packages are excluded in published application output.</span></span>

<span data-ttu-id="f1aab-120">下列 *.csproj* 檔案參考 ASP.NET Core 的 `Microsoft.AspNetCore.All` 中繼套件：</span><span class="sxs-lookup"><span data-stu-id="f1aab-120">The following *.csproj* file references the `Microsoft.AspNetCore.All` metapackage for ASP.NET Core:</span></span>

[!code-xml[](metapackage/samples/Metapackage.All.Example.csproj?highlight=8)]

::: moniker range=">= aspnetcore-2.1"

## <a name="implicit-versioning"></a><span data-ttu-id="f1aab-121">隱含的版本設定</span><span class="sxs-lookup"><span data-stu-id="f1aab-121">Implicit versioning</span></span>

<span data-ttu-id="f1aab-122">在 ASP.NET Core 2.1 或更新版本中，您可指定不含版本的 `Microsoft.AspNetCore.All` 套件參考。</span><span class="sxs-lookup"><span data-stu-id="f1aab-122">In ASP.NET Core 2.1 or later, you can specify the `Microsoft.AspNetCore.All` package reference without a version.</span></span> <span data-ttu-id="f1aab-123">當未指定版本時，SDK (`Microsoft.NET.Sdk.Web`) 會指定隱含的版本。</span><span class="sxs-lookup"><span data-stu-id="f1aab-123">When the version isn't specified, an implicit version is specified by the SDK (`Microsoft.NET.Sdk.Web`).</span></span> <span data-ttu-id="f1aab-124">建議依賴 SDK 指定的隱含版本，而不要明確設定套件參考的版本號碼。</span><span class="sxs-lookup"><span data-stu-id="f1aab-124">We recommend relying on the implicit version specified by the SDK and not explicitly setting the version number on the package reference.</span></span> <span data-ttu-id="f1aab-125">如果您對此方法有疑問，請在 [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430) (Microsoft.AspNetCore.App 隱含版本討論區) 留下 GitHub 意見。</span><span class="sxs-lookup"><span data-stu-id="f1aab-125">If you have questions about this approach, leave a GitHub comment at the [Discussion for the Microsoft.AspNetCore.App implicit version](https://github.com/aspnet/Docs/issues/6430).</span></span>

<span data-ttu-id="f1aab-126">可攜式應用程式的隱含版本會設定為 `major.minor.0`。</span><span class="sxs-lookup"><span data-stu-id="f1aab-126">The implicit version is set to `major.minor.0` for portable apps.</span></span> <span data-ttu-id="f1aab-127">共用架構向前復原機制會在已安裝共用架構中的最新相容版本上，執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="f1aab-127">The shared framework roll-forward mechanism runs the app on the latest compatible version among the installed shared frameworks.</span></span> <span data-ttu-id="f1aab-128">為了保證開發、測試和生產均使用相同版本，請務必在所有環境中安裝相同版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="f1aab-128">To guarantee the same version is used in development, test, and production, ensure the same version of the shared framework is installed in all environments.</span></span> <span data-ttu-id="f1aab-129">針對獨立應用程式，隱含版本號碼會設定為已安裝 SDK 隨附共用架構的 `major.minor.patch`。</span><span class="sxs-lookup"><span data-stu-id="f1aab-129">For self-contained apps, the implicit version number is set to the `major.minor.patch` of the shared framework bundled in the installed SDK.</span></span>

<span data-ttu-id="f1aab-130">在 `Microsoft.AspNetCore.All` 套件參考上指定版本號碼**不**保證會選擇該版本的共用架構。</span><span class="sxs-lookup"><span data-stu-id="f1aab-130">Specifying a version number on the `Microsoft.AspNetCore.All` package reference does **not** guarantee that version of the shared framework is chosen.</span></span> <span data-ttu-id="f1aab-131">例如，假設指定版本 "2.1.1"，但安裝 "2.1.3"。</span><span class="sxs-lookup"><span data-stu-id="f1aab-131">For example, suppose version "2.1.1" is specified, but "2.1.3" is installed.</span></span> <span data-ttu-id="f1aab-132">在此情況下，應用程式會使用 "2.1.3"。</span><span class="sxs-lookup"><span data-stu-id="f1aab-132">In that case, the app will use "2.1.3".</span></span> <span data-ttu-id="f1aab-133">您可以停用向前復原 (修補及/或次要)，但不建議這樣做。</span><span class="sxs-lookup"><span data-stu-id="f1aab-133">Although not recommended, you can disable roll forward (patch and/or minor).</span></span> <span data-ttu-id="f1aab-134">如需 dotnet 主機向前復原及如何設定其行為的詳細資訊，請參閱 [dotnet 主機向前復原](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md)。</span><span class="sxs-lookup"><span data-stu-id="f1aab-134">For more information regarding dotnet host roll-forward and how to configure its behavior, see [dotnet host roll forward](https://github.com/dotnet/core-setup/blob/master/Documentation/design-docs/roll-forward-on-no-candidate-fx.md).</span></span>

<span data-ttu-id="f1aab-135">在專案檔中，專案的 SDK 必須設定為 `Microsoft.NET.Sdk.Web`，才能使用 `Microsoft.AspNetCore.All` 的隱含版本。</span><span class="sxs-lookup"><span data-stu-id="f1aab-135">The project's SDK must be set to `Microsoft.NET.Sdk.Web` in the project file to use the implicit version of `Microsoft.AspNetCore.All`.</span></span> <span data-ttu-id="f1aab-136">當指定 `Microsoft.NET.Sdk` SDK 時 (專案檔頂端的 `<Project Sdk="Microsoft.NET.Sdk">`)，就會產生以下警告：</span><span class="sxs-lookup"><span data-stu-id="f1aab-136">When the `Microsoft.NET.Sdk` SDK is specified (`<Project Sdk="Microsoft.NET.Sdk">` at the top of the project file), the following warning is generated:</span></span>

<span data-ttu-id="f1aab-137">*警告 NU1604：專案相依性 Microsoft.AspNetCore.All 未包含內含的下限。請在相依性版本包含下限，以確保還原結果一致。*</span><span class="sxs-lookup"><span data-stu-id="f1aab-137">*Warning NU1604: Project dependency Microsoft.AspNetCore.All does not contain an inclusive lower bound. Include a lower bound in the dependency version to ensure consistent restore results.*</span></span>

<span data-ttu-id="f1aab-138">這是.NET Core 2.1 SDK 的已知的問題，並將於 .NET Core 2.2 SDK 中修正。</span><span class="sxs-lookup"><span data-stu-id="f1aab-138">This is a known issue with the .NET Core 2.1 SDK and will be fixed in the .NET Core 2.2 SDK.</span></span>

::: moniker-end

<a name="migrate"></a>

## <a name="migrating-from-microsoftaspnetcoreall-to-microsoftaspnetcoreapp"></a><span data-ttu-id="f1aab-139">從 Microsoft.AspNetCore.All 移轉到 Microsoft.AspNetCore.App</span><span class="sxs-lookup"><span data-stu-id="f1aab-139">Migrating from Microsoft.AspNetCore.All to Microsoft.AspNetCore.App</span></span>

<span data-ttu-id="f1aab-140">下列套件隨附於 `Microsoft.AspNetCore.All`，而不是 `Microsoft.AspNetCore.App` 套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-140">The following packages are included in `Microsoft.AspNetCore.All` but not the `Microsoft.AspNetCore.App` package.</span></span>

* `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServices.HostingStartup`
* `Microsoft.AspNetCore.AzureAppServicesIntegration`
* `Microsoft.AspNetCore.DataProtection.AzureKeyVault`
* `Microsoft.AspNetCore.DataProtection.AzureStorage`
* `Microsoft.AspNetCore.Server.Kestrel.Transport.Libuv`
* `Microsoft.AspNetCore.SignalR.Redis`
* `Microsoft.Data.Sqlite`
* `Microsoft.Data.Sqlite.Core`
* `Microsoft.EntityFrameworkCore.Sqlite`
* `Microsoft.EntityFrameworkCore.Sqlite.Core`
* `Microsoft.Extensions.Caching.Redis`
* `Microsoft.Extensions.Configuration.AzureKeyVault`
* `Microsoft.Extensions.Logging.AzureAppServices`
* `Microsoft.VisualStudio.Web.BrowserLink`

<span data-ttu-id="f1aab-141">若要從 `Microsoft.AspNetCore.All` 移到 `Microsoft.AspNetCore.App`，如果您的應用程式使用來自上述套件的任何 API，或這些套件中隨附的套件，請將參考新增到專案中的這些套件。</span><span class="sxs-lookup"><span data-stu-id="f1aab-141">To move from `Microsoft.AspNetCore.All` to `Microsoft.AspNetCore.App`, if your app uses any APIs from the above packages, or packages brought in by those packages, add references to those packages in your project.</span></span>

<span data-ttu-id="f1aab-142">若前述套件的任何相依性不是 `Microsoft.AspNetCore.App` 的相依性，即不包含在內。</span><span class="sxs-lookup"><span data-stu-id="f1aab-142">Any dependencies of the preceding packages that otherwise aren't dependencies of `Microsoft.AspNetCore.App` are not included implicitly.</span></span> <span data-ttu-id="f1aab-143">例如: </span><span class="sxs-lookup"><span data-stu-id="f1aab-143">For example:</span></span>

* <span data-ttu-id="f1aab-144">`StackExchange.Redis` 為 `Microsoft.Extensions.Caching.Redis` 的相依性</span><span class="sxs-lookup"><span data-stu-id="f1aab-144">`StackExchange.Redis` as a dependency of `Microsoft.Extensions.Caching.Redis`</span></span>
* <span data-ttu-id="f1aab-145">`Microsoft.ApplicationInsights` 為 `Microsoft.AspNetCore.ApplicationInsights.HostingStartup` 的相依性</span><span class="sxs-lookup"><span data-stu-id="f1aab-145">`Microsoft.ApplicationInsights` as a dependency of `Microsoft.AspNetCore.ApplicationInsights.HostingStartup`</span></span>

## <a name="update-aspnet-core-21"></a><span data-ttu-id="f1aab-146">更新 ASP.NET Core 2.1</span><span class="sxs-lookup"><span data-stu-id="f1aab-146">Update ASP.NET Core 2.1</span></span>

<span data-ttu-id="f1aab-147">建議您移轉到 `Microsoft.AspNetCore.App` 中繼套件 2.1 與更新版本。</span><span class="sxs-lookup"><span data-stu-id="f1aab-147">We recommend migrating to the `Microsoft.AspNetCore.App` metapackage for 2.1 and later.</span></span> <span data-ttu-id="f1aab-148">若要繼續使用 `Microsoft.AspNetCore.All` 中繼套件並確保已部署最新的更新程式版本：</span><span class="sxs-lookup"><span data-stu-id="f1aab-148">To keep using the `Microsoft.AspNetCore.All` metapackage and ensure the latest patch version is deployed:</span></span>

* <span data-ttu-id="f1aab-149">在開發電腦和組建伺服器上：安裝最新的 [.NET Core SDK](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="f1aab-149">On development machines and build servers: Install the latest [.NET Core SDK](https://www.microsoft.com/net/download).</span></span>
* <span data-ttu-id="f1aab-150">在部署伺服器上：安裝最新的 [.NET Core 執行階段](https://www.microsoft.com/net/download)。</span><span class="sxs-lookup"><span data-stu-id="f1aab-150">On deployment servers: Install the latest [.NET Core runtime](https://www.microsoft.com/net/download).</span></span>
 <span data-ttu-id="f1aab-151">您的應用程式會在應用程式重新開機時，向前復原到已安裝的最新版本。</span><span class="sxs-lookup"><span data-stu-id="f1aab-151">Your app will roll forward to the latest installed version on an application restart.</span></span>
