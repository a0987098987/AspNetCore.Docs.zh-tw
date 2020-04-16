---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
ms.author: riande
ms.custom: mvc
ms.date: 04/14/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 3d871ab960de28a565280d9e4cb2c597832e2455
ms.sourcegitcommit: 6c8cff2d6753415c4f5d2ffda88159a7f6f7431a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/16/2020
ms.locfileid: "81440931"
---
# <a name="razor-file-compilation-in-aspnet-core"></a><span data-ttu-id="2f1a8-103">ASP.NET Core 中 Razor 檔案的先行編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-103">Razor file compilation in ASP.NET Core</span></span>

<span data-ttu-id="2f1a8-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="2f1a8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

::: moniker range=">= aspnetcore-3.1"

<span data-ttu-id="2f1a8-105">具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-105">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="2f1a8-106">通過配置專案,可以選擇啟用運行時編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-106">Runtime compilation may be optionally enabled by configuring your project.</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="2f1a8-107">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-107">Razor compilation</span></span>

<span data-ttu-id="2f1a8-108">Razor SDK 預設啟用 Razor 檔的生成時間和發佈時間編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-108">Build-time and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2f1a8-109">啟用後,執行時編譯將補充生成時間編譯,允許在編輯 Razor 檔時對其進行更新。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-109">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they're edited.</span></span>

## <a name="enable-runtime-compilation-at-project-creation"></a><span data-ttu-id="2f1a8-110">在項目建立時開啟執行時編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-110">Enable runtime compilation at project creation</span></span>

<span data-ttu-id="2f1a8-111">Razor 頁面和 MVC 專案範本包括一個選項,用於在創建專案時啟用運行時編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-111">The Razor Pages and MVC project templates include an option to enable runtime compilation when the project is created.</span></span> <span data-ttu-id="2f1a8-112">ASP.NET酷3.1及更高版本支援此選項。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-112">This option is supported in ASP.NET Core 3.1 and later.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="2f1a8-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="2f1a8-113">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="2f1a8-114">在 **'建立新ASP.NET核心 Web 應用程式**對話框中:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-114">In the **Create a new ASP.NET Core web application** dialog:</span></span>

1. <span data-ttu-id="2f1a8-115">選擇**Web 應用程式**或 Web**應用程式(模型檢視控制器)** 專案範本。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-115">Select either the **Web Application** or the **Web Application (Model-View-Controller)** project template.</span></span>
1. <span data-ttu-id="2f1a8-116">勾選中啟用**Razor 執行時編譯**複選框。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-116">Select the **Enable Razor runtime compilation** check box.</span></span>

# <a name="net-core-cli"></a>[<span data-ttu-id="2f1a8-117">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="2f1a8-117">.NET Core CLI</span></span>](#tab/netcore-cli)

<span data-ttu-id="2f1a8-118">使用`-rrc`或`--razor-runtime-compilation`範本選項。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-118">Use the `-rrc` or `--razor-runtime-compilation` template option.</span></span> <span data-ttu-id="2f1a8-119">例如,以下指令建立啟用執行時編譯的新 Razor Pages 專案:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-119">For example, the following command creates a new Razor Pages project with runtime compilation enabled:</span></span>

```dotnetcli
dotnet new webapp --razor-runtime-compilation
```

---

## <a name="enable-runtime-compilation-in-an-existing-project"></a><span data-ttu-id="2f1a8-120">在現有項目中開啟執行時編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-120">Enable runtime compilation in an existing project</span></span>

<span data-ttu-id="2f1a8-121">要為現有項目中的所有環境啟用運行時編譯,:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-121">To enable runtime compilation for all environments in an existing project:</span></span>

1. <span data-ttu-id="2f1a8-122">安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-122">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
1. <span data-ttu-id="2f1a8-123">更新專案`Startup.ConfigureServices`的方法以包括<xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>對 的調用。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-123">Update the project's `Startup.ConfigureServices` method to include a call to <xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>.</span></span> <span data-ttu-id="2f1a8-124">例如：</span><span class="sxs-lookup"><span data-stu-id="2f1a8-124">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

## <a name="conditionally-enable-runtime-compilation-in-an-existing-project"></a><span data-ttu-id="2f1a8-125">在現有項目中有條件執行時編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-125">Conditionally enable runtime compilation in an existing project</span></span>

<span data-ttu-id="2f1a8-126">可以啟用運行時編譯,使其僅可用於本地開發。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-126">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="2f1a8-127">以這種方式有條件地啟用可確保發布的輸出:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-127">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="2f1a8-128">使用已編譯的視圖。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-128">Uses compiled views.</span></span>
* <span data-ttu-id="2f1a8-129">在生產中不啟用文件觀察程式。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-129">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="2f1a8-130">要只在開發環境中開啟執行時編譯:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-130">To enable runtime compilation only in the Development environment:</span></span>

1. <span data-ttu-id="2f1a8-131">安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-131">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>
1. <span data-ttu-id="2f1a8-132">變更`environmentVariables`*啟動設定的*啟動檔部份 :</span><span class="sxs-lookup"><span data-stu-id="2f1a8-132">Modify the launch profile `environmentVariables` section in *launchSettings.json*:</span></span>
    * <span data-ttu-id="2f1a8-133">認證`ASPNETCORE_ENVIRONMENT`設定`"Development"`為 。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-133">Verify `ASPNETCORE_ENVIRONMENT` is set to `"Development"`.</span></span>
    * <span data-ttu-id="2f1a8-134">將 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 設定為 `"Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"`。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-134">Set `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` to `"Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"`.</span></span>

<span data-ttu-id="2f1a8-135">在下面的範例中,在與`IIS Express``RazorPagesApp`啟動設定檔的開發環境中開啟執行時編譯:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-135">In the following example, runtime compilation is enabled in the Development environment for the `IIS Express` and `RazorPagesApp` launch profiles:</span></span>

[!code-json[](~/mvc/views/view-compilation/samples/3.1/launchSettings.json?highlight=15-16,24-25)]

<span data-ttu-id="2f1a8-136">專案`Startup`類不需要更改代碼。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-136">No code changes are needed in the project's `Startup` class.</span></span> <span data-ttu-id="2f1a8-137">在執行時,ASP.NET Core`Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`搜尋中的[程式集級託管啟動屬性](xref:fundamentals/configuration/platform-specific-configuration#hostingstartup-attribute)。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-137">At runtime, ASP.NET Core searches for an [assembly-level HostingStartup attribute](xref:fundamentals/configuration/platform-specific-configuration#hostingstartup-attribute) in `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`.</span></span> <span data-ttu-id="2f1a8-138">該`HostingStartup`屬性指定要執行的應用啟動代碼。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-138">The `HostingStartup` attribute specifies the app startup code to execute.</span></span> <span data-ttu-id="2f1a8-139">該啟動代碼支援運行時編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-139">That startup code enables runtime compilation.</span></span>

## <a name="enable-runtime-compilation-for-a-razor-class-library"></a><span data-ttu-id="2f1a8-140">為 Razor 函式庫啟用執行時編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-140">Enable runtime compilation for a Razor Class Library</span></span>

<span data-ttu-id="2f1a8-141">請考慮 Razor 頁面專案引用名為*MyClassLib*的[Razor 類庫 (RCL)](xref:razor-pages/ui-class)的方案。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-141">Consider a scenario in which a Razor Pages project references a [Razor Class Library (RCL)](xref:razor-pages/ui-class) named *MyClassLib*.</span></span> <span data-ttu-id="2f1a8-142">RCL 包含 *_Layout.cshtml*檔,所有團隊的 MVC 和 Razor 頁面專案都使用該檔。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-142">The RCL contains a *_Layout.cshtml* file that all of your team's MVC and Razor Pages projects consume.</span></span> <span data-ttu-id="2f1a8-143">您希望為該 RCL 中的 *_Layout.cshtml*檔啟用執行時編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-143">You want to enable runtime compilation for the *_Layout.cshtml* file in that RCL.</span></span> <span data-ttu-id="2f1a8-144">在 Razor 頁面項目中進行以下變更:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-144">Make the following changes in the Razor Pages project:</span></span>

1. <span data-ttu-id="2f1a8-145">使用[「有條件地」中的指令在現有項目中啟用執行時編譯](#conditionally-enable-runtime-compilation-in-an-existing-project), 啟用執行時編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-145">Enable runtime compilation with the instructions at [Conditionally enable runtime compilation in an existing project](#conditionally-enable-runtime-compilation-in-an-existing-project).</span></span>
1. <span data-ttu-id="2f1a8-146">在`Startup.ConfigureServices`中 配置運行時編譯選項。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-146">Configure the runtime compilation options in `Startup.ConfigureServices`:</span></span>

    [!code-csharp[](~/mvc/views/view-compilation/samples/3.1/Startup.cs?name=snippet_ConfigureServices&highlight=5-10)]

    <span data-ttu-id="2f1a8-147">在前面的代碼中,建構了*MyClassLib* RCL的絕對路徑。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-147">In the preceding code, an absolute path to the *MyClassLib* RCL is constructed.</span></span> <span data-ttu-id="2f1a8-148">[實體檔提供者 API](xref:fundamentals/file-providers#physicalfileprovider)用於尋找該絕對路徑上的目錄和檔案。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-148">The [PhysicalFileProvider API](xref:fundamentals/file-providers#physicalfileprovider) is used to locate directories and files at that absolute path.</span></span> <span data-ttu-id="2f1a8-149">最後,實例`PhysicalFileProvider`被添加到檔提供程式集合中,允許造訪RCL的 *.cshtml*檔。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-149">Finally, the `PhysicalFileProvider` instance is added to a file providers collection, which allows access to the RCL's *.cshtml* files.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f1a8-150">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f1a8-150">Additional resources</span></span>

* <span data-ttu-id="2f1a8-151">[Razor 編譯上建構和Razor編譯上發佈](xref:razor-pages/sdk#properties)屬性。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-151">[RazorCompileOnBuild and RazorCompileOnPublish](xref:razor-pages/sdk#properties) properties.</span></span>
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

<span data-ttu-id="2f1a8-152">具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-152">Razor files with a *.cshtml* extension are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span> <span data-ttu-id="2f1a8-153">您可以透過設定應用程式，選擇性地啟用執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-153">Runtime compilation may be optionally enabled by configuring your application.</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="2f1a8-154">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-154">Razor compilation</span></span>

<span data-ttu-id="2f1a8-155">Razor SDK 預設啟用 Razor 檔的生成時間和發佈時間編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-155">Build-time and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2f1a8-156">啟用後,執行時編譯將補充生成時間編譯,允許在編輯 Razor 檔時對其進行更新。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-156">When enabled, runtime compilation complements build-time compilation, allowing Razor files to be updated if they're edited.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="2f1a8-157">執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-157">Runtime compilation</span></span>

<span data-ttu-id="2f1a8-158">要為所有環境與設定模式啟用執行時編譯,:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-158">To enable runtime compilation for all environments and configuration modes:</span></span>

1. <span data-ttu-id="2f1a8-159">安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-159">Install the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet package.</span></span>

1. <span data-ttu-id="2f1a8-160">更新專案`Startup.ConfigureServices`的方法以包括<xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>對 的調用。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-160">Update the project's `Startup.ConfigureServices` method to include a call to <xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>.</span></span> <span data-ttu-id="2f1a8-161">例如：</span><span class="sxs-lookup"><span data-stu-id="2f1a8-161">For example:</span></span>

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a><span data-ttu-id="2f1a8-162">有條件開啟執行時編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-162">Conditionally enable runtime compilation</span></span>

<span data-ttu-id="2f1a8-163">可以啟用運行時編譯,使其僅可用於本地開發。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-163">Runtime compilation can be enabled such that it's only available for local development.</span></span> <span data-ttu-id="2f1a8-164">以這種方式有條件地啟用可確保發布的輸出:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-164">Conditionally enabling in this manner ensures that the published output:</span></span>

* <span data-ttu-id="2f1a8-165">使用已編譯的視圖。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-165">Uses compiled views.</span></span>
* <span data-ttu-id="2f1a8-166">尺寸較小。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-166">Is smaller in size.</span></span>
* <span data-ttu-id="2f1a8-167">在生產中不啟用文件觀察程式。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-167">Doesn't enable file watchers in production.</span></span>

<span data-ttu-id="2f1a8-168">要根據環境與設定模式啟用執行時編譯,:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-168">To enable runtime compilation based on the environment and configuration mode:</span></span>

1. <span data-ttu-id="2f1a8-169">根據活動`Configuration`值有條件地引用[Microsoft.AspNetCore.Mvc.Razor.執行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)套件:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-169">Conditionally reference the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) package based on the active `Configuration` value:</span></span>

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. <span data-ttu-id="2f1a8-170">更新專案`Startup.ConfigureServices`的方法以包括`AddRazorRuntimeCompilation`對 的調用。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-170">Update the project's `Startup.ConfigureServices` method to include a call to `AddRazorRuntimeCompilation`.</span></span> <span data-ttu-id="2f1a8-171">有條件地`AddRazorRuntimeCompilation`執行,以便僅`ASPNETCORE_ENVIRONMENT`當 變數設定`Development`為 : 時,它才在除錯模式下執行:</span><span class="sxs-lookup"><span data-stu-id="2f1a8-171">Conditionally execute `AddRazorRuntimeCompilation` such that it only runs in Debug mode when the `ASPNETCORE_ENVIRONMENT` variable is set to `Development`:</span></span>

    [!code-csharp[](~/mvc/views/view-compilation/samples/3.0/Startup.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="2f1a8-172">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f1a8-172">Additional resources</span></span>

* <span data-ttu-id="2f1a8-173">[Razor 編譯上建構和Razor編譯上發佈](xref:razor-pages/sdk#properties)屬性。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-173">[RazorCompileOnBuild and RazorCompileOnPublish](xref:razor-pages/sdk#properties) properties.</span></span>
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
* <span data-ttu-id="2f1a8-174">有關顯示跨項目進行執行時編譯工作的範例,請參閱[GitHub 上的執行時編譯範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation)。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-174">See the [runtime compilation sample on GitHub](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation) for a sample that shows making runtime compilation work across projects.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="2f1a8-175">叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-175">A Razor file is compiled at runtime, when the associated Razor Page or MVC view is invoked.</span></span> <span data-ttu-id="2f1a8-176">Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-176">Razor files are compiled at both build and publish time using the [Razor SDK](xref:razor-pages/sdk).</span></span>

## <a name="razor-compilation"></a><span data-ttu-id="2f1a8-177">Razor 編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-177">Razor compilation</span></span>

<span data-ttu-id="2f1a8-178">Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-178">Build- and publish-time compilation of Razor files is enabled by default by the Razor SDK.</span></span> <span data-ttu-id="2f1a8-179">在建置階段中，支援更新 Razor 檔案後進行編輯。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-179">Editing Razor files after they're updated is supported at build time.</span></span> <span data-ttu-id="2f1a8-180">根據預設，只有已編譯的 *Views.dll* 會與您的應用程式一起部署，而且在編譯 Razor 檔案時不需要任何 *.cshtml* 檔案或參考組件。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-180">By default, only the compiled *Views.dll* and no *.cshtml* files or references assemblies required to compile Razor files are deployed with your app.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="2f1a8-181">先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-181">The precompilation tool has been deprecated, and will be removed in ASP.NET Core 3.0.</span></span> <span data-ttu-id="2f1a8-182">建議移轉到 [Razor SDK](xref:razor-pages/sdk)。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-182">We recommend migrating to [Razor Sdk](xref:razor-pages/sdk).</span></span>
>
> <span data-ttu-id="2f1a8-183">只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-183">The Razor SDK is effective only when no precompilation-specific properties are set in the project file.</span></span> <span data-ttu-id="2f1a8-184">例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-184">For instance, setting the *.csproj* file's `MvcRazorCompileOnPublish` property to `true` disables the Razor SDK.</span></span>

## <a name="runtime-compilation"></a><span data-ttu-id="2f1a8-185">執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="2f1a8-185">Runtime compilation</span></span>

<span data-ttu-id="2f1a8-186">建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-186">Build-time compilation is supplemented by runtime compilation of Razor files.</span></span> <span data-ttu-id="2f1a8-187">當 *.cshtml* 檔案的內容變更時，ASP.NET Core MVC 將重新編譯 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="2f1a8-187">ASP.NET Core MVC will recompile Razor files when the contents of a *.cshtml* file change.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="2f1a8-188">其他資源</span><span class="sxs-lookup"><span data-stu-id="2f1a8-188">Additional resources</span></span>

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
