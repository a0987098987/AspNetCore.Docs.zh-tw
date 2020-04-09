---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor 頁面如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 03/26/2020
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 2284131ce2d45ec6bc01ce38f91e2c951b108605
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "80321004"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="54fd6-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="54fd6-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="54fd6-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="54fd6-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="54fd6-105">概觀</span><span class="sxs-lookup"><span data-stu-id="54fd6-105">Overview</span></span>

<span data-ttu-id="54fd6-106">[!INCLUDE[](~/includes/2.1-SDK.md)] 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="54fd6-107">Razor SDK：</span><span class="sxs-lookup"><span data-stu-id="54fd6-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="54fd6-108">需要為基於 ASP.NET的 Icore MVC 或[Blazor](xref:blazor/index)專案生成、打包和發佈包含[Razor](xref:mvc/views/razor)檔案的專案。</span><span class="sxs-lookup"><span data-stu-id="54fd6-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="54fd6-109">包括一組預定義的目標、屬性和項,這些目標、屬性和項允許自定義 Razor *(.cshtml*或 *.razor)* 檔的編譯。</span><span class="sxs-lookup"><span data-stu-id="54fd6-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="54fd6-110">`Content`Razor SDK`Include`包括屬性`**\*.cshtml``**\*.razor`設定為和 globing 模式的專案。</span><span class="sxs-lookup"><span data-stu-id="54fd6-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="54fd6-111">將發表匹配的檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="54fd6-112">標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。</span><span class="sxs-lookup"><span data-stu-id="54fd6-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="54fd6-113">包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。</span><span class="sxs-lookup"><span data-stu-id="54fd6-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="54fd6-114">Razor SDK`Content`包括`Include`一個 屬性設置`**\*.cshtml`為 globing 模式的項。</span><span class="sxs-lookup"><span data-stu-id="54fd6-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="54fd6-115">將發表匹配的檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="54fd6-116">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="54fd6-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="54fd6-117">使用剃刀 SDK</span><span class="sxs-lookup"><span data-stu-id="54fd6-117">Use the Razor SDK</span></span>

<span data-ttu-id="54fd6-118">大多數 Web 應用不需要顯式引用 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="54fd6-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="54fd6-119">要使用 Razor SDK 構建包含 Razor 檢視或 Razor 頁面的類庫,我們建議從 Razor 類庫 (RCL) 項目範本開始。</span><span class="sxs-lookup"><span data-stu-id="54fd6-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="54fd6-120">用於建構 Blazor *(.razor*) 檔案的 RCL 需要引用[Microsoft.AspNetCore.元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)包。</span><span class="sxs-lookup"><span data-stu-id="54fd6-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="54fd6-121">編譯 Razor 檢視或頁面 *(.cshtml*檔案)`netcoreapp3.0`的 RCL`FrameworkReference`的檔案中具有到[Microsoft.AspNetCore.App 中繼 。](xref:fundamentals/metapackage-app)</span><span class="sxs-lookup"><span data-stu-id="54fd6-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="54fd6-122">若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：</span><span class="sxs-lookup"><span data-stu-id="54fd6-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="54fd6-123">使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：</span><span class="sxs-lookup"><span data-stu-id="54fd6-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="54fd6-124">通常,需要包`Microsoft.AspNetCore.Mvc`引用來接收生成和編譯 Razor 頁面和 Razor 視圖所需的其他依賴項。</span><span class="sxs-lookup"><span data-stu-id="54fd6-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="54fd6-125">至少,您的項目應加入包引照到:</span><span class="sxs-lookup"><span data-stu-id="54fd6-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  <span data-ttu-id="54fd6-126">該`Microsoft.AspNetCore.Razor.Design`包為專案提供 Razor 編譯任務和目標。</span><span class="sxs-lookup"><span data-stu-id="54fd6-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="54fd6-127">`Microsoft.AspNetCore.Mvc` 中包含上述套件。</span><span class="sxs-lookup"><span data-stu-id="54fd6-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="54fd6-128">下面的標記顯示了一個項目檔,該檔案使用 Razor SDK 為 ASP.NET 核心剃刀頁面應用建構 Razor 檔:</span><span class="sxs-lookup"><span data-stu-id="54fd6-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="54fd6-129">和`Microsoft.AspNetCore.Razor.Design``Microsoft.AspNetCore.Mvc.Razor.Extensions`包包含在[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="54fd6-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="54fd6-130">但是,無`Microsoft.AspNetCore.App`版本的包引用向不包含 最新版本的`Microsoft.AspNetCore.Razor.Design`應用 提供了元包。</span><span class="sxs-lookup"><span data-stu-id="54fd6-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="54fd6-131">項目必須引用 (`Microsoft.AspNetCore.Razor.Design``Microsoft.AspNetCore.Mvc`或 ) 的一致版本,以便包含 Razor 的最新生成時修補程式。</span><span class="sxs-lookup"><span data-stu-id="54fd6-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="54fd6-132">如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/aspnet/Razor/issues/2553) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="54fd6-133">屬性</span><span class="sxs-lookup"><span data-stu-id="54fd6-133">Properties</span></span>

<span data-ttu-id="54fd6-134">下列屬性可在專案建置期間控制 Razor 的 SDK 行為：</span><span class="sxs-lookup"><span data-stu-id="54fd6-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="54fd6-135">`RazorCompileOnBuild`&ndash;當`true`時, 編譯和發出 Razor 程式集作為建構專案的一部分。</span><span class="sxs-lookup"><span data-stu-id="54fd6-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="54fd6-136">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-136">Defaults to `true`.</span></span>
* <span data-ttu-id="54fd6-137">`RazorCompileOnPublish`&ndash;當`true`時 ,編譯和發出 Razor 程式集作為發佈專案的一部分。</span><span class="sxs-lookup"><span data-stu-id="54fd6-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="54fd6-138">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-138">Defaults to `true`.</span></span>

<span data-ttu-id="54fd6-139">下表中的屬性和項用於配置輸入和輸出到 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="54fd6-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="54fd6-140">從 ASP.NET酷 3.0 開始,預設情況下,如果禁用專案檔`RazorCompileOnBuild``RazorCompileOnPublish`中 的或 MSBuild 屬性,則不提供 MVC 視圖或 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="54fd6-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="54fd6-141">如果應用依賴於運行時編譯來處理 *.cshtml*檔,則應用程式必須添加對[Microsoft.AspNetCore.Mvc.Razor.執行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)包的顯式引用。</span><span class="sxs-lookup"><span data-stu-id="54fd6-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="54fd6-142">項目</span><span class="sxs-lookup"><span data-stu-id="54fd6-142">Items</span></span> | <span data-ttu-id="54fd6-143">描述</span><span class="sxs-lookup"><span data-stu-id="54fd6-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="54fd6-144">作為代碼生成輸入的項目 *(.cshtml*檔案)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="54fd6-145">作為 Razor 元件代碼生成輸入的項目 *(.razor*檔案)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="54fd6-146">作為 Razor 編譯目標的輸入的項目*元素 (.cs*檔)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="54fd6-147">使用這個選項`ItemGroup`可指定要編譯到 Razor 程式集中的其他檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="54fd6-148">用於 Razor 組件的程式碼產生屬性的項目元素。</span><span class="sxs-lookup"><span data-stu-id="54fd6-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="54fd6-149">例如：</span><span class="sxs-lookup"><span data-stu-id="54fd6-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="54fd6-150">作為嵌入資源添加到生成的 Razor 程式集的項目。</span><span class="sxs-lookup"><span data-stu-id="54fd6-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

::: moniker range=">= aspnetcore-3.0"

| <span data-ttu-id="54fd6-151">屬性</span><span class="sxs-lookup"><span data-stu-id="54fd6-151">Property</span></span> | <span data-ttu-id="54fd6-152">描述</span><span class="sxs-lookup"><span data-stu-id="54fd6-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="54fd6-153">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-153">File name (without extension) of the assembly produced by Razor.</span></span> |
| `RazorOutputPath` | <span data-ttu-id="54fd6-154">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="54fd6-155">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="54fd6-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="54fd6-156">有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="54fd6-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="54fd6-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="54fd6-158">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-158">Default is `true`.</span></span> <span data-ttu-id="54fd6-159">當`true`時 ,包括*Web.config* *、.json*和 *.cshtml*檔案作為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="54fd6-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="54fd6-160">當通過`Microsoft.NET.Sdk.Web`引用時 *,wwwroot*下的檔和配置檔也包括在內。</span><span class="sxs-lookup"><span data-stu-id="54fd6-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="54fd6-161">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="54fd6-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="54fd6-162">當`true`生成 *.cs*檔,`RazorAssemblyAttribute`其中包含指定的屬性,並在編譯輸出中包含該檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="54fd6-163">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="54fd6-164">當`true``RazorGenerate`時 ,將項目 *(.cshtml*) 檔案複製到發佈目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="54fd6-165">通常,如果已發佈的應用在生成時間或發佈時參與編譯,則不需要 Razor 檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="54fd6-166">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-166">Defaults to `false`.</span></span> |
| `PreserveCompilationReferences` | <span data-ttu-id="54fd6-167">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="54fd6-168">通常,如果 Razor 編譯發生在生成時間或發佈時間,則發布應用不需要引用程式集。</span><span class="sxs-lookup"><span data-stu-id="54fd6-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="54fd6-169">如果已`true`發佈的應用需要運行時編譯,則設置為"已發佈應用"。</span><span class="sxs-lookup"><span data-stu-id="54fd6-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="54fd6-170">例如,`true`如果應用在執行時修改 *.cshtml*檔或使用嵌入的檢視,則將該值設置為。</span><span class="sxs-lookup"><span data-stu-id="54fd6-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="54fd6-171">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="54fd6-172">當`true`時,所有 Razor 內容項 *(.cshtml*檔)都標記為包含在生成的 NuGet 包中。</span><span class="sxs-lookup"><span data-stu-id="54fd6-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="54fd6-173">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="54fd6-174">如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="54fd6-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="54fd6-175">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="54fd6-176">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="54fd6-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="54fd6-177">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="54fd6-178">當`true`時,SDK 生成 MVC 在執行時用於執行應用程式部件發現的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="54fd6-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |
| `DefaultWebContentItemExcludes` | <span data-ttu-id="54fd6-179">在 Web 或 Razor SDK 的專案中`Content`要從專案群組中排除的項目的 globing 模式</span><span class="sxs-lookup"><span data-stu-id="54fd6-179">A globbing pattern for item elements that are to be excluded from the `Content` item group in projects targeting the Web or Razor SDK</span></span> |
| `ExcludeConfigFilesFromBuildOutput` | <span data-ttu-id="54fd6-180">當`true` *.config*和 *.json*檔不會複製到生成輸出目錄時。</span><span class="sxs-lookup"><span data-stu-id="54fd6-180">When `true`, *.config* and *.json* files do not get copied to the build output directory.</span></span> |
| `AddRazorSupportForMvc` | <span data-ttu-id="54fd6-181">當`true`時,配置 Razor SDK 以添加在建譯包含 MVC 檢視或 Razor 頁面的應用程式時所需的 MVC 配置的支援。</span><span class="sxs-lookup"><span data-stu-id="54fd6-181">When `true`, configures the Razor SDK to add support for the MVC configuration that is required when building applications containing MVC views or Razor Pages.</span></span> <span data-ttu-id="54fd6-182">此屬性隱式設定為 .NET Core 3.0 或針對 Web SDK 的更高專案</span><span class="sxs-lookup"><span data-stu-id="54fd6-182">This property is implicitly set for .NET Core 3.0 or later projects targeting the Web SDK</span></span> |
| `RazorLangVersion` | <span data-ttu-id="54fd6-183">要定位的 Razor 語言的版本。</span><span class="sxs-lookup"><span data-stu-id="54fd6-183">The version of the Razor Language to target.</span></span> |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| <span data-ttu-id="54fd6-184">屬性</span><span class="sxs-lookup"><span data-stu-id="54fd6-184">Property</span></span> | <span data-ttu-id="54fd6-185">描述</span><span class="sxs-lookup"><span data-stu-id="54fd6-185">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="54fd6-186">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-186">File name (without extension) of the assembly produced by Razor.</span></span> |
| `RazorOutputPath` | <span data-ttu-id="54fd6-187">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-187">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="54fd6-188">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="54fd6-188">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="54fd6-189">有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-189">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="54fd6-190">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="54fd6-190">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="54fd6-191">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-191">Default is `true`.</span></span> <span data-ttu-id="54fd6-192">當`true`時 ,包括*Web.config* *、.json*和 *.cshtml*檔案作為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="54fd6-192">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="54fd6-193">當通過`Microsoft.NET.Sdk.Web`引用時 *,wwwroot*下的檔和配置檔也包括在內。</span><span class="sxs-lookup"><span data-stu-id="54fd6-193">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="54fd6-194">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="54fd6-194">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="54fd6-195">當`true`生成 *.cs*檔,`RazorAssemblyAttribute`其中包含指定的屬性,並在編譯輸出中包含該檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-195">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="54fd6-196">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-196">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="54fd6-197">當`true``RazorGenerate`時 ,將項目 *(.cshtml*) 檔案複製到發佈目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-197">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="54fd6-198">通常,如果已發佈的應用在生成時間或發佈時參與編譯,則不需要 Razor 檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-198">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="54fd6-199">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-199">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="54fd6-200">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="54fd6-200">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="54fd6-201">通常,如果 Razor 編譯發生在生成時間或發佈時間,則發布應用不需要引用程式集。</span><span class="sxs-lookup"><span data-stu-id="54fd6-201">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="54fd6-202">如果已`true`發佈的應用需要運行時編譯,則設置為"已發佈應用"。</span><span class="sxs-lookup"><span data-stu-id="54fd6-202">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="54fd6-203">例如,`true`如果應用在執行時修改 *.cshtml*檔或使用嵌入的檢視,則將該值設置為。</span><span class="sxs-lookup"><span data-stu-id="54fd6-203">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="54fd6-204">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-204">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="54fd6-205">當`true`時,所有 Razor 內容項 *(.cshtml*檔)都標記為包含在生成的 NuGet 包中。</span><span class="sxs-lookup"><span data-stu-id="54fd6-205">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="54fd6-206">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-206">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="54fd6-207">如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="54fd6-207">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="54fd6-208">預設為 `false`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-208">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="54fd6-209">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="54fd6-209">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="54fd6-210">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="54fd6-210">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="54fd6-211">當`true`時,SDK 生成 MVC 在執行時用於執行應用程式部件發現的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="54fd6-211">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |
| `DefaultWebContentItemExcludes` | <span data-ttu-id="54fd6-212">在 Web 或 Razor SDK 的專案中`Content`要從專案群組中排除的項目的 globing 模式</span><span class="sxs-lookup"><span data-stu-id="54fd6-212">A globbing pattern for item elements that are to be excluded from the `Content` item group in projects targeting the Web or Razor SDK</span></span> |
| `ExcludeConfigFilesFromBuildOutput` | <span data-ttu-id="54fd6-213">當`true` *.config*和 *.json*檔不會複製到生成輸出目錄時。</span><span class="sxs-lookup"><span data-stu-id="54fd6-213">When `true`, *.config* and *.json* files do not get copied to the build output directory.</span></span> |
| `AddRazorSupportForMvc` | <span data-ttu-id="54fd6-214">當`true`時,配置 Razor SDK 以添加在建譯包含 MVC 檢視或 Razor 頁面的應用程式時所需的 MVC 配置的支援。</span><span class="sxs-lookup"><span data-stu-id="54fd6-214">When `true`, configures the Razor SDK to add support for the MVC configuration that is required when building applications containing MVC views or Razor Pages.</span></span> <span data-ttu-id="54fd6-215">此屬性隱式設定為 .NET Core 3.0 或針對 Web SDK 的更高專案</span><span class="sxs-lookup"><span data-stu-id="54fd6-215">This property is implicitly set for .NET Core 3.0 or later projects targeting the Web SDK</span></span> |
| `RazorLangVersion` | <span data-ttu-id="54fd6-216">要定位的 Razor 語言的版本。</span><span class="sxs-lookup"><span data-stu-id="54fd6-216">The version of the Razor Language to target.</span></span> |

::: moniker-end

<span data-ttu-id="54fd6-217">有關屬性的詳細資訊,請參閱[MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。</span><span class="sxs-lookup"><span data-stu-id="54fd6-217">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="54fd6-218">目標</span><span class="sxs-lookup"><span data-stu-id="54fd6-218">Targets</span></span>

<span data-ttu-id="54fd6-219">Razor SDK 會定義兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="54fd6-219">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="54fd6-220">`RazorGenerate`&ndash;代碼從`RazorGenerate`項目生成 *.cs*檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-220">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="54fd6-221">使用`RazorGenerateDependsOn`屬性指定可以在此目標之前或之後運行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="54fd6-221">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="54fd6-222">`RazorCompile`&ndash;將生成的 *.cs*檔編譯到 Razor 程式集中。</span><span class="sxs-lookup"><span data-stu-id="54fd6-222">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="54fd6-223">使用`RazorCompileDependsOn`指定可以在此目標之前或之後運行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="54fd6-223">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="54fd6-224">`RazorComponentGenerate`&ndash;代碼為`RazorComponent`項目生成 *.cs*檔。</span><span class="sxs-lookup"><span data-stu-id="54fd6-224">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="54fd6-225">使用`RazorComponentGenerateDependsOn`屬性指定可以在此目標之前或之後運行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="54fd6-225">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="54fd6-226">Razor 檢視的執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="54fd6-226">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="54fd6-227">根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。</span><span class="sxs-lookup"><span data-stu-id="54fd6-227">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="54fd6-228">當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。</span><span class="sxs-lookup"><span data-stu-id="54fd6-228">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="54fd6-229">將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。</span><span class="sxs-lookup"><span data-stu-id="54fd6-229">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="54fd6-230">對於 Web 應用,請確保你的應用`Microsoft.NET.Sdk.Web`定位為 SDK。</span><span class="sxs-lookup"><span data-stu-id="54fd6-230">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="54fd6-231">剃刀語言版本</span><span class="sxs-lookup"><span data-stu-id="54fd6-231">Razor language version</span></span>

<span data-ttu-id="54fd6-232">定位`Microsoft.NET.Sdk.Web`SDK 時,Razor 語言版本將從應用的目標框架版本推斷出來。</span><span class="sxs-lookup"><span data-stu-id="54fd6-232">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the app's target framework version.</span></span> <span data-ttu-id="54fd6-233">對於針對`Microsoft.NET.Sdk.Razor`SDK 的專案,或在應用需要與推斷值不同的 Razor 語言版本的情況下,可以透過在應用`<RazorLangVersion>`的專案檔中設定 屬性來配置版本:</span><span class="sxs-lookup"><span data-stu-id="54fd6-233">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="54fd6-234">Razor 的語言版本與構建的運行時版本緊密整合。</span><span class="sxs-lookup"><span data-stu-id="54fd6-234">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="54fd6-235">定位不是為運行時設計的語言版本不受支持,並且可能會生成錯誤。</span><span class="sxs-lookup"><span data-stu-id="54fd6-235">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="54fd6-236">其他資源</span><span class="sxs-lookup"><span data-stu-id="54fd6-236">Additional resources</span></span>

* [<span data-ttu-id="54fd6-237">適用於 .NET Core 之 csproj 格式的新增項目</span><span class="sxs-lookup"><span data-stu-id="54fd6-237">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="54fd6-238">常見 MS 產生項目項目</span><span class="sxs-lookup"><span data-stu-id="54fd6-238">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
