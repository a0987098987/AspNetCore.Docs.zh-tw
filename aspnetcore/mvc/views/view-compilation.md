---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
ms.author: riande
ms.custom: mvc
ms.date: 4/8/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 0afd39fdb5a6f570e0e78ad54f6c436460bad3a6
ms.sourcegitcommit: 6f1b516e0c899a49afe9a29044a2383ce2ada3c7
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/13/2020
ms.locfileid: "81223955"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="6262c-103">ASP.NET Core 中 Razor 檔案的先行編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="6262c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="6262c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="6262c-105">具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-105">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="6262c-106">您可以透過設定應用程式，選擇性地啟用執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-106">Runtime compilation may be optionally enabled by configuring your application.</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="6262c-107">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-107">Razor compilation</span></span>

<span data-ttu-id="6262c-108">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-108">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="6262c-109">啟用時，執行階段編譯會補充建置時間編譯，以允許更新 Razor 檔案 (如果該檔案已被編輯)。</span><span class="sxs-lookup"><span data-stu-id="6262c-109">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they are edited.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="6262c-110">執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-110">Runtime compilation</span></span>

<span data-ttu-id="6262c-111">要為所有環境與設定模式啟用執行時編譯,:</span><span class="sxs-lookup"><span data-stu-id="6262c-111">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="6262c-112">安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。</span><span class="sxs-lookup"><span data-stu-id="6262c-112">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="6262c-113">更新專案`Startup.ConfigureServices`的方法以包括<xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>對 的調用。</span><span class="sxs-lookup"><span data-stu-id="6262c-113">Update the project's `Startup.ConfigureServices` method to include a call to <xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>.</span></span> <span data-ttu-id="6262c-114">例如：</span><span class="sxs-lookup"><span data-stu-id="6262c-114">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="6262c-115">有條件開啟執行時編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-115">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="6262c-116">可以啟用運行時編譯,使其僅可用於本地開發。</span><span class="sxs-lookup"><span data-stu-id="6262c-116">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="6262c-117">以這種方式有條件地啟用可確保發布的輸出:</span><span class="sxs-lookup"><span data-stu-id="6262c-117">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="6262c-118">使用已編譯的視圖。</span><span class="sxs-lookup"><span data-stu-id="6262c-118">Uses compiled views.</span></span>
* <span data-ttu-id="6262c-119">尺寸較小。</span><span class="sxs-lookup"><span data-stu-id="6262c-119">Is smaller in size.</span></span>
* <span data-ttu-id="6262c-120">在生產中不啟用文件觀察程式。</span><span class="sxs-lookup"><span data-stu-id="6262c-120">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="6262c-121">要根據環境與設定模式啟用執行時編譯,:</span><span class="sxs-lookup"><span data-stu-id="6262c-121">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="6262c-122">根據活動`Configuration`值有條件地引用[Microsoft.AspNetCore.Mvc.Razor.執行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)套件:</span><span class="sxs-lookup"><span data-stu-id="6262c-122">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="6262c-123">更新專案`Startup.ConfigureServices`的方法以包括`AddRazorRuntimeCompilation`對 的調用。</span><span class="sxs-lookup"><span data-stu-id="6262c-123">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="6262c-124">有條件地`AddRazorRuntimeCompilation`執行,以便僅`ASPNETCORE_ENVIRONMENT`當 變數設定`Development`為 : 時,它才在除錯模式下執行:</span><span class="sxs-lookup"><span data-stu-id="6262c-124">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

  [!code-csharp[](~/mvc/views/view-compilation/sample/Startup.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="6262c-125">其他資源</span><span class="sxs-lookup"><span data-stu-id="6262c-125">Additional resources</span></span>

* <span data-ttu-id="6262c-126">[Razor 編譯上建構和Razor編譯上發佈](xref:razor-pages/sdk#properties)屬性。</span><span class="sxs-lookup"><span data-stu-id="6262c-126">[RazorCompileOnBuild and RazorCompileOnPublish](xref:razor-pages/sdk#properties) properties.</span></span>
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
* <span data-ttu-id="6262c-127">有關顯示跨項目進行執行時編譯工作的範例,請參閱[GitHub 上的執行時編譯範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation)。</span><span class="sxs-lookup"><span data-stu-id="6262c-127">See the [runtimecompilation sample on GitHub](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation) for a sample that shows making runtime compilation work across projects.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="6262c-128">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-128">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="6262c-129">Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-129">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="6262c-130">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-130">Razor compilation</span></span>

<span data-ttu-id="6262c-131">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="6262c-131">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="6262c-132">在建置階段中，支援更新 Razor 檔案後進行編輯。</span><span class="sxs-lookup"><span data-stu-id="6262c-132">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="6262c-133">根據預設，只有已編譯的 *Views.dll* 會與您的應用程式一起部署，而且在編譯 Razor 檔案時不需要任何 *.cshtml* 檔案或參考組件。</span><span class="sxs-lookup"><span data-stu-id="6262c-133">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="6262c-134">先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。</span><span class="sxs-lookup"><span data-stu-id="6262c-134">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="6262c-135">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="6262c-135">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="6262c-136">只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。</span><span class="sxs-lookup"><span data-stu-id="6262c-136">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="6262c-137">例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="6262c-137">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="6262c-138">執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="6262c-138">Runtime compilation</span></span>

<span data-ttu-id="6262c-139">建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。</span><span class="sxs-lookup"><span data-stu-id="6262c-139">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="6262c-140">當 *.cshtml* 檔案的內容變更時，ASP.NET Core MVC 將重新編譯 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="6262c-140">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="6262c-141">其他資源</span><span class="sxs-lookup"><span data-stu-id="6262c-141">Additional resources</span></span>

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
