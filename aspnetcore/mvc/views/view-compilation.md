---
title: ASP.NET Core 中 Razor 檔案的編譯和先行編譯
author: rick-anderson
description: 了解先行編譯 Razor 檔案的好處，以及如何完成 ASP.NET Core 應用程式中 Razor 檔案的先行編譯。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 01/23/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 2720708f8e58fdc55b82bfb56665005170e79934
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889752"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="65080-103">ASP.NET Core 中 Razor 檔案的先行編譯</span><span class="sxs-lookup"><span data-stu-id="65080-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="65080-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="65080-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="65080-105">叫用關聯的 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="65080-106">不支援在建置階段發佈 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="65080-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="65080-107">您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="65080-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="65080-108">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="65080-109">不支援在建置階段發佈 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="65080-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="65080-110">您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="65080-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="65080-111">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="65080-112">Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

## <a name="precompilation-considerations"></a><span data-ttu-id="65080-113">先行編譯的考量</span><span class="sxs-lookup"><span data-stu-id="65080-113">Precompilation considerations</span></span>

<span data-ttu-id="65080-114">以下是先行編譯 Razor 檔案的副作用：</span><span class="sxs-lookup"><span data-stu-id="65080-114">The following are side effects of precompiling Razor files:</span></span>

* <span data-ttu-id="65080-115">較小的發佈組合</span><span class="sxs-lookup"><span data-stu-id="65080-115">A smaller published bundle</span></span>
* <span data-ttu-id="65080-116">更快速的啟動時間</span><span class="sxs-lookup"><span data-stu-id="65080-116">A faster startup time</span></span>
* <span data-ttu-id="65080-117">您無法編輯 Razor 檔案，因為發佈組合中沒有關聯的內容。</span><span class="sxs-lookup"><span data-stu-id="65080-117">You can't edit Razor files&mdash;the associated content is absent from the published bundle.</span></span>

## <a name="deploy-precompiled-files"></a><span data-ttu-id="65080-118">部署先行編譯的檔案</span><span class="sxs-lookup"><span data-stu-id="65080-118">Deploy precompiled files</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="65080-119">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-119">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="65080-120">在建置階段中，支援更新 Razor 檔案後進行編輯。</span><span class="sxs-lookup"><span data-stu-id="65080-120">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="65080-121">根據預設，只有編譯的 *Views.dll* 會與應用程式一起部署，而 *.cshtml* 檔案則否。</span><span class="sxs-lookup"><span data-stu-id="65080-121">By default, only the compiled *Views.dll* and no *.cshtml* files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65080-122">ASP.NET Core 3.0 中將移除先行編譯工具。</span><span class="sxs-lookup"><span data-stu-id="65080-122">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="65080-123">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="65080-123">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="65080-124">只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。</span><span class="sxs-lookup"><span data-stu-id="65080-124">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="65080-125">例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="65080-125">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="65080-126">如果您的專案以 .NET Framework 為目標，請安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="65080-126">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="65080-127">如果您專案的目標設為 .NET Core，則不需要進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="65080-127">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="65080-128">根據預設，ASP.NET Core 2.x 專案範本會隱含地將 `MvcRazorCompileOnPublish` 屬性設定成 `true`。</span><span class="sxs-lookup"><span data-stu-id="65080-128">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="65080-129">因此，可以安全地將元素從 *.csproj* 檔案中移除。</span><span class="sxs-lookup"><span data-stu-id="65080-129">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="65080-130">ASP.NET Core 3.0 中將移除先行編譯工具。</span><span class="sxs-lookup"><span data-stu-id="65080-130">The precompilation tool will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="65080-131">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="65080-131">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="65080-132">在 ASP.NET Core 2.0 中執行[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，無法使用 Razor 檔案先行編譯。</span><span class="sxs-lookup"><span data-stu-id="65080-132">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="65080-133">將 `MvcRazorCompileOnPublish` 屬性設定成 `true`，然後安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="65080-133">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="65080-134">下列 *.csproj* 範例會反白顯示這些設定：</span><span class="sxs-lookup"><span data-stu-id="65080-134">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="65080-135">使用 [.NET Core CLI 發行命令](/dotnet/core/tools/dotnet-publish)針對[框架相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)準備該應用程式。</span><span class="sxs-lookup"><span data-stu-id="65080-135">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="65080-136">例如，在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="65080-136">For example, execute the following command at the project root:</span></span>

```console
dotnet publish -c Release
```

<span data-ttu-id="65080-137">先行編譯成功時，會產生 <專案名稱>.PrecompiledViews.dll 檔案，其中包含編譯過的 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="65080-137">A *<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="65080-138">例如，以下螢幕擷取畫面為 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：</span><span class="sxs-lookup"><span data-stu-id="65080-138">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a><span data-ttu-id="65080-140">在發生變更時重新編譯 Razor 檔案</span><span class="sxs-lookup"><span data-stu-id="65080-140">Recompile Razor files on change</span></span>

<span data-ttu-id="65080-141"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` 可取得或設定可決定磁碟上的檔案變更時是否要重新編譯 Razor files (Razor 檢視與 Razor Pages) 的值。</span><span class="sxs-lookup"><span data-stu-id="65080-141">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> `AllowRecompilingViewsOnFileChange` gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="65080-142">當設定為 `true` 時，[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 會監視已設定之 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 執行個體中的 Razor 是否發生變更。</span><span class="sxs-lookup"><span data-stu-id="65080-142">When set to `true`, [IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) watches for changes to Razor files in configured <xref:Microsoft.Extensions.FileProviders.IFileProvider> instances.</span></span>

<span data-ttu-id="65080-143">針對下列項目，預設值是 `true`：</span><span class="sxs-lookup"><span data-stu-id="65080-143">The default value is `true` for:</span></span>

* <span data-ttu-id="65080-144">ASP.NET Core 2.1 或更舊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="65080-144">ASP.NET Core 2.1 or earlier apps.</span></span>
* <span data-ttu-id="65080-145">ASP.NET Core 2.2 或更新的應用程式 (在開發環境中)。</span><span class="sxs-lookup"><span data-stu-id="65080-145">ASP.NET Core 2.2 or later apps in the Development environment.</span></span>

<span data-ttu-id="65080-146">`AllowRecompilingViewsOnFileChange` 與相容性參數關聯，而且可以視已針對應用程式設定的相容性版本提供不同的行為。</span><span class="sxs-lookup"><span data-stu-id="65080-146">`AllowRecompilingViewsOnFileChange` is associated with a compatibility switch and can provide different behavior depending on the configured compatibility version for the app.</span></span> <span data-ttu-id="65080-147">透過設定 `AllowRecompilingViewsOnFileChange` 優先順序高於應用程式相容性版本意指的值，來設定應用程式。</span><span class="sxs-lookup"><span data-stu-id="65080-147">Configuring the app by setting `AllowRecompilingViewsOnFileChange` takes precedence over the value implied by the app's compatibility version.</span></span>

<span data-ttu-id="65080-148">若應用程式的相容性版本是設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> 或更舊版本，則除非已明確設定，否則 `AllowRecompilingViewsOnFileChange` 會設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="65080-148">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier, `AllowRecompilingViewsOnFileChange` is set to `true` unless explicitly configured.</span></span>

<span data-ttu-id="65080-149">若應用程式的相容性版本是設定為 `CompatibilityVersion.Version_2_2` 或更新版本，則除非環境為開發環境或已明確設定值，否則 `AllowRecompilingViewsOnFileChange` 會設定為 `false`。</span><span class="sxs-lookup"><span data-stu-id="65080-149">If the app's compatibility version is set to `CompatibilityVersion.Version_2_2` or later, `AllowRecompilingViewsOnFileChange` is set to `false` unless the environment is Development or the value is explicitly configured.</span></span>

<span data-ttu-id="65080-150">如需設定應用程式相容性版本的指導方針與範例，請參閱<xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="65080-150">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="65080-151">其他資源</span><span class="sxs-lookup"><span data-stu-id="65080-151">Additional resources</span></span>

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
