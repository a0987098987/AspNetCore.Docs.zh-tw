---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661102"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="cbacb-103">ASP.NET Core 中 Razor 檔案的先行編譯</span><span class="sxs-lookup"><span data-stu-id="cbacb-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="cbacb-104">由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供</span><span class="sxs-lookup"><span data-stu-id="cbacb-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="cbacb-105">叫用關聯的 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-105">A Razor file is compiled at runtime, when the associated MVC view is invoked.</span></span> <span data-ttu-id="cbacb-106">不支援在建置階段發佈 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="cbacb-106">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="cbacb-107">您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="cbacb-107">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cbacb-108">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-108">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="cbacb-109">不支援在建置階段發佈 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="cbacb-109">Build-time Razor file publishing is unsupported.</span></span> <span data-ttu-id="cbacb-110">您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。</span><span class="sxs-lookup"><span data-stu-id="cbacb-110">Razor files can optionally be compiled at publish time and deployed with the app&mdash;using the precompilation tool.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="cbacb-111">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-111">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="cbacb-112">Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-112">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cbacb-113">具有*cshtml*副檔名的 Razor 檔案會使用[razor SDK](xref:razor-pages/sdk)，在組建和發行時間進行編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-113">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="cbacb-114">您可以透過設定應用程式，選擇性地啟用執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-114">Runtime compilation may be optionally enabled by configuring your application.</span></span>

::: moniker-end

## <a name="razor-compilation"></a><span data-ttu-id="cbacb-115">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="cbacb-115">Razor compilation</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cbacb-116">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-116">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="cbacb-117">啟用時，執行階段編譯會補充建置時間編譯，以允許更新 Razor 檔案 (如果該檔案已被編輯)。</span><span class="sxs-lookup"><span data-stu-id="cbacb-117">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

<span data-ttu-id="cbacb-118">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-118">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="cbacb-119">在建置階段中，支援更新 Razor 檔案後進行編輯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-119">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="cbacb-120">根據預設，只有已編譯的 *Views.dll* 會與您的應用程式一起部署，而且在編譯 Razor 檔案時不需要任何 *.cshtml* 檔案或參考組件。</span><span class="sxs-lookup"><span data-stu-id="cbacb-120">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbacb-121">先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。</span><span class="sxs-lookup"><span data-stu-id="cbacb-121">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="cbacb-122">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="cbacb-122">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="cbacb-123">只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。</span><span class="sxs-lookup"><span data-stu-id="cbacb-123">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="cbacb-124">例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="cbacb-124">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

<span data-ttu-id="cbacb-125">如果您的專案以 .NET Framework 為目標，請安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件：</span><span class="sxs-lookup"><span data-stu-id="cbacb-125">If your project targets .NET Framework, install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package:</span></span>

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

<span data-ttu-id="cbacb-126">如果您專案的目標設為 .NET Core，則不需要進行任何變更。</span><span class="sxs-lookup"><span data-stu-id="cbacb-126">If your project targets .NET Core, no changes are necessary.</span></span>

<span data-ttu-id="cbacb-127">根據預設，ASP.NET Core 2.x 專案範本會隱含地將 `MvcRazorCompileOnPublish` 屬性設定成 `true`。</span><span class="sxs-lookup"><span data-stu-id="cbacb-127">The ASP.NET Core 2.x project templates implicitly set the `MvcRazorCompileOnPublish` property to `true` by default.</span></span> <span data-ttu-id="cbacb-128">因此，可以安全地將元素從 *.csproj* 檔案中移除。</span><span class="sxs-lookup"><span data-stu-id="cbacb-128">Consequently, this element can be safely removed from the *.csproj* file.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="cbacb-129">先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。</span><span class="sxs-lookup"><span data-stu-id="cbacb-129">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="cbacb-130">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="cbacb-130">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="cbacb-131">在 ASP.NET Core 2.0 中執行[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，無法使用 Razor 檔案先行編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-131">Razor file precompilation is unavailable when performing a [self-contained deployment (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) in ASP.NET Core 2.0.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-1.1"

<span data-ttu-id="cbacb-132">將 `MvcRazorCompileOnPublish` 屬性設定成 `true`，然後安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="cbacb-132">Set the `MvcRazorCompileOnPublish` property to `true`, and install the [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet package.</span></span> <span data-ttu-id="cbacb-133">下列 *.csproj* 範例會反白顯示這些設定：</span><span class="sxs-lookup"><span data-stu-id="cbacb-133">The following *.csproj* sample highlights these settings:</span></span>

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

<span data-ttu-id="cbacb-134">使用 [.NET Core CLI 發行命令](/dotnet/core/deploying/#framework-dependent-deployments-fdd)針對[框架相依部署](/dotnet/core/tools/dotnet-publish)準備該應用程式。</span><span class="sxs-lookup"><span data-stu-id="cbacb-134">Prepare the app for a [framework-dependent deployment](/dotnet/core/deploying/#framework-dependent-deployments-fdd) with the [.NET Core CLI publish command](/dotnet/core/tools/dotnet-publish).</span></span> <span data-ttu-id="cbacb-135">例如，在專案的根目錄下執行下列命令：</span><span class="sxs-lookup"><span data-stu-id="cbacb-135">For example, execute the following command at the project root:</span></span>

```dotnetcli
dotnet publish -c Release
```

<span data-ttu-id="cbacb-136">先行編譯成功時，會產生 *\<專案名稱>.PrecompiledViews.dll* 檔案，其中包含編譯過的 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="cbacb-136">A *\<project_name>.PrecompiledViews.dll* file, containing the compiled Razor files, is produced when precompilation succeeds.</span></span> <span data-ttu-id="cbacb-137">例如，以下螢幕擷取畫面為 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：</span><span class="sxs-lookup"><span data-stu-id="cbacb-137">For example, the screenshot below depicts the contents of *Index.cshtml* within *WebApplication1.PrecompiledViews.dll*:</span></span>

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a><span data-ttu-id="cbacb-139">執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="cbacb-139">Runtime compilation</span></span>

::: moniker range="= aspnetcore-2.1"

<span data-ttu-id="cbacb-140">建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。</span><span class="sxs-lookup"><span data-stu-id="cbacb-140">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="cbacb-141">當 *.cshtml* 檔案的內容變更時，ASP.NET Core MVC 將重新編譯 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="cbacb-141">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

::: moniker-end

::: moniker range="= aspnetcore-2.2"

<span data-ttu-id="cbacb-142">建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。</span><span class="sxs-lookup"><span data-stu-id="cbacb-142">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="cbacb-143"><xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 取得或設定值，以決定是否要重新編譯 Razor 檔案（Razor views 和 Razor Pages），並在磁片上的檔案變更時進行更新。</span><span class="sxs-lookup"><span data-stu-id="cbacb-143">The <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> gets or sets a value that determines if Razor files (Razor views and Razor Pages) are recompiled and updated if files change on disk.</span></span>

<span data-ttu-id="cbacb-144">針對下列項目，預設值是 `true`：</span><span class="sxs-lookup"><span data-stu-id="cbacb-144">The default value is `true` for:</span></span>

* <span data-ttu-id="cbacb-145">如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> 或更早版本。</span><span class="sxs-lookup"><span data-stu-id="cbacb-145">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> or earlier</span></span>
* <span data-ttu-id="cbacb-146">如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> 或更新版本，而且應用程式位於 <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*> 開發環境中。</span><span class="sxs-lookup"><span data-stu-id="cbacb-146">If the app's compatibility version is set to <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> or later and the app is in the Development environment <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*>.</span></span> <span data-ttu-id="cbacb-147">換句話說，除非明確設定 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>，否則 Razor 檔案不會在非開發環境中重新編譯。</span><span class="sxs-lookup"><span data-stu-id="cbacb-147">In other words, Razor files wouldn't recompile in non-Development environment unless <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> is explicitly set.</span></span>

<span data-ttu-id="cbacb-148">如需設定應用程式相容性版本的指導方針與範例，請參閱<xref:mvc/compatibility-version>。</span><span class="sxs-lookup"><span data-stu-id="cbacb-148">For guidance and examples of setting the app's compatibility version, see <xref:mvc/compatibility-version>.</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="cbacb-149">若要啟用所有環境和設定模式的執行時間編譯：</span><span class="sxs-lookup"><span data-stu-id="cbacb-149">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="cbacb-150">安裝 [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="cbacb-150">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="cbacb-151">更新專案的 `Startup.ConfigureServices` 方法，以包含對 `AddRazorRuntimeCompilation`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="cbacb-151">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="cbacb-152">例如：</span><span class="sxs-lookup"><span data-stu-id="cbacb-152">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="cbacb-153">有條件地啟用執行時間編譯</span><span class="sxs-lookup"><span data-stu-id="cbacb-153">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="cbacb-154">可以啟用執行時間編譯，使其僅適用于本機開發。</span><span class="sxs-lookup"><span data-stu-id="cbacb-154">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="cbacb-155">以這種方式有條件地啟用會確保已發行的輸出：</span><span class="sxs-lookup"><span data-stu-id="cbacb-155">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="cbacb-156">使用已編譯的視圖。</span><span class="sxs-lookup"><span data-stu-id="cbacb-156">Uses compiled views.</span></span>
* <span data-ttu-id="cbacb-157">大小較小。</span><span class="sxs-lookup"><span data-stu-id="cbacb-157">Is smaller in size.</span></span>
* <span data-ttu-id="cbacb-158">不會在生產環境中啟用檔案監看員。</span><span class="sxs-lookup"><span data-stu-id="cbacb-158">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="cbacb-159">若要根據環境和設定模式啟用執行時間編譯：</span><span class="sxs-lookup"><span data-stu-id="cbacb-159">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="cbacb-160">根據作用中的 `Configuration` 值，有條件地參考[microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)套件：</span><span class="sxs-lookup"><span data-stu-id="cbacb-160">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="cbacb-161">更新專案的 `Startup.ConfigureServices` 方法，以包含對 `AddRazorRuntimeCompilation`的呼叫。</span><span class="sxs-lookup"><span data-stu-id="cbacb-161">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="cbacb-162">有條件地執行 `AddRazorRuntimeCompilation`，使其只有在 `ASPNETCORE_ENVIRONMENT` 變數設定為 `Development`時，才會以 Debug 模式執行：</span><span class="sxs-lookup"><span data-stu-id="cbacb-162">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a><span data-ttu-id="cbacb-163">其他資源</span><span class="sxs-lookup"><span data-stu-id="cbacb-163">Additional resources</span></span>

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
