---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 81025c14ba68971ca5d3cfc9387c2f50dd247654
ms.sourcegitcommit: 983b31449fe398e6e922eb13e9eb6f4287ec91e8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/24/2019
ms.locfileid: "70017403"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="05a4c-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="05a4c-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="05a4c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="05a4c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="05a4c-105">總覽</span><span class="sxs-lookup"><span data-stu-id="05a4c-105">Overview</span></span>

<span data-ttu-id="05a4c-106">[!INCLUDE[](~/includes/2.1-SDK.md)]包含`Microsoft.NET.Sdk.Razor`MSBuild SDK (Razor SDK)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="05a4c-107">Razor SDK：</span><span class="sxs-lookup"><span data-stu-id="05a4c-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="05a4c-108">需要建立、封裝和發行包含[Razor](xref:mvc/views/razor)檔案的專案, 以用於 ASP.NET Core MVC 或[Blazor](xref:blazor/index)專案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="05a4c-109">包含一組預先定義的目標、屬性和專案, 可讓您自訂 Razor (*cshtml*或*Razor*) 檔案的編譯。</span><span class="sxs-lookup"><span data-stu-id="05a4c-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="05a4c-110">Razor `Content` SDK 包含`Include`屬性設定為`**\*.cshtml`和`**\*.razor`通配模式的專案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="05a4c-111">已發行相符的檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="05a4c-112">標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。</span><span class="sxs-lookup"><span data-stu-id="05a4c-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="05a4c-113">包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。</span><span class="sxs-lookup"><span data-stu-id="05a4c-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="05a4c-114">Razor SDK 包含一個`Content`專案`Include` , 並將屬性設定為`**\*.cshtml`萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="05a4c-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="05a4c-115">已發行相符的檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="05a4c-116">必要條件</span><span class="sxs-lookup"><span data-stu-id="05a4c-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="05a4c-117">使用 Razor SDK</span><span class="sxs-lookup"><span data-stu-id="05a4c-117">Use the Razor SDK</span></span>

<span data-ttu-id="05a4c-118">大部分的 web 應用程式不需要明確參考 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="05a4c-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="05a4c-119">若要使用 Razor SDK 來建立包含 Razor views 或 Razor Pages 的類別庫, 建議您從 Razor 類別庫 (RCL) 專案範本開始。</span><span class="sxs-lookup"><span data-stu-id="05a4c-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="05a4c-120">用來建立 Blazor (*razor*) 檔案的 RCL, 最少需要[AspNetCore 元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)套件的參考。</span><span class="sxs-lookup"><span data-stu-id="05a4c-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="05a4c-121">用來建立 Razor views 或 pages (*cshtml*檔案) 的 RCL, 至少需要`netcoreapp3.0` `FrameworkReference`以或更新版本為目標, 並在其專案檔中具有[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="05a4c-122">若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：</span><span class="sxs-lookup"><span data-stu-id="05a4c-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="05a4c-123">使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：</span><span class="sxs-lookup"><span data-stu-id="05a4c-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="05a4c-124">一般而言，套件參考至`Microsoft.AspNetCore.Mvc`，才能接收來建置和編譯 Razor 頁面與 Razor 檢視所需的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="05a4c-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="05a4c-125">最少，您的專案應該新增套件參考：</span><span class="sxs-lookup"><span data-stu-id="05a4c-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="05a4c-126">`Microsoft.AspNetCore.Razor.Design`套件提供 Razor 編譯工作和目標專案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="05a4c-127">`Microsoft.AspNetCore.Mvc` 中包含上述套件。</span><span class="sxs-lookup"><span data-stu-id="05a4c-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="05a4c-128">下列標記會顯示使用 Razor SDK 來建置 ASP.NET Core Razor 頁面應用程式的 Razor 檔案的專案檔：</span><span class="sxs-lookup"><span data-stu-id="05a4c-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="05a4c-129">`Microsoft.AspNetCore.Razor.Design`並`Microsoft.AspNetCore.Mvc.Razor.Extensions`套件中包含[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="05a4c-130">不過，版本小於`Microsoft.AspNetCore.App`封裝參考文件提供的中繼套件，且不包含最新版本的應用程式`Microsoft.AspNetCore.Razor.Design`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="05a4c-131">專案必須參考的版本一致性`Microsoft.AspNetCore.Razor.Design`(或`Microsoft.AspNetCore.Mvc`)，以便包含最新的建置時間修正 razor。</span><span class="sxs-lookup"><span data-stu-id="05a4c-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="05a4c-132">如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="05a4c-133">屬性</span><span class="sxs-lookup"><span data-stu-id="05a4c-133">Properties</span></span>

<span data-ttu-id="05a4c-134">下列屬性可在專案建置期間控制 Razor 的 SDK 行為：</span><span class="sxs-lookup"><span data-stu-id="05a4c-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="05a4c-135">`RazorCompileOnBuild` &ndash; 當`true`、 編譯和建置專案時的 Razor 組件就會發出。</span><span class="sxs-lookup"><span data-stu-id="05a4c-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="05a4c-136">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-136">Defaults to `true`.</span></span>
* <span data-ttu-id="05a4c-137">`RazorCompileOnPublish` &ndash; 當`true`、 編譯，並會發出 Razor 組件，在發行專案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="05a4c-138">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-138">Defaults to `true`.</span></span>

<span data-ttu-id="05a4c-139">下表中的項目與屬性用來設定輸入和輸出至 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="05a4c-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="05a4c-140">從 ASP.NET Core 3.0 開始, 如果專案檔中的`RazorCompileOnBuild`或`RazorCompileOnPublish` MSBuild 屬性已停用, 則預設不會提供 MVC Views 或 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="05a4c-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="05a4c-141">如果應用程式依賴執行時間編譯來處理*cshtml*檔案, 則應用程式必須新增[microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)封裝的明確參考。</span><span class="sxs-lookup"><span data-stu-id="05a4c-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="05a4c-142">項目</span><span class="sxs-lookup"><span data-stu-id="05a4c-142">Items</span></span> | <span data-ttu-id="05a4c-143">描述</span><span class="sxs-lookup"><span data-stu-id="05a4c-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="05a4c-144">屬於程式碼產生之輸入的專案元素 ( *. cshtml*檔案)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="05a4c-145">做為 Razor 元件程式碼產生輸入的專案元素 (*razor*檔案)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="05a4c-146">做為 Razor 編譯目標輸入的專案元素 ( *.cs*檔案)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="05a4c-147">使用此`ItemGroup`來指定要編譯成 Razor 元件的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="05a4c-148">用於 Razor 組件的程式碼產生屬性的項目元素。</span><span class="sxs-lookup"><span data-stu-id="05a4c-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="05a4c-149">例如:</span><span class="sxs-lookup"><span data-stu-id="05a4c-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="05a4c-150">加入做為內嵌資源產生的 Razor 組件的項目。</span><span class="sxs-lookup"><span data-stu-id="05a4c-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="05a4c-151">屬性</span><span class="sxs-lookup"><span data-stu-id="05a4c-151">Property</span></span> | <span data-ttu-id="05a4c-152">描述</span><span class="sxs-lookup"><span data-stu-id="05a4c-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="05a4c-153">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-153">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="05a4c-154">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="05a4c-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="05a4c-155">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="05a4c-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="05a4c-156">有效值為 `Implicit`、`RazorSDK` 及 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="05a4c-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="05a4c-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="05a4c-158">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-158">Default is `true`.</span></span> <span data-ttu-id="05a4c-159">當`true`時, 會包含*web.config*、 *. json*和*cshtml*檔案做為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="05a4c-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="05a4c-160">透過參考時`Microsoft.NET.Sdk.Web`，檔案底下*wwwroot*和組態檔也會包含在內。</span><span class="sxs-lookup"><span data-stu-id="05a4c-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="05a4c-161">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="05a4c-162">當`true`，會產生 *.cs*檔案，其中包含指定屬性`RazorAssemblyAttribute`和包含編譯輸出中的檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="05a4c-163">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="05a4c-164">當`true`，複製`RazorGenerate`項目 ( *.cshtml*) 至發行目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="05a4c-165">一般而言，Razor 檔案不需要針對已發佈的應用程式，如果它們參與在建置階段 」 或 「 發行階段的編譯。</span><span class="sxs-lookup"><span data-stu-id="05a4c-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="05a4c-166">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="05a4c-167">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="05a4c-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="05a4c-168">一般而言，參考組件不需要的已發佈的應用程式，如果 Razor 編譯，就會發生在建置階段或發行時間。</span><span class="sxs-lookup"><span data-stu-id="05a4c-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="05a4c-169">若要設定`true`如果您已發佈的應用程式需要執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="05a4c-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="05a4c-170">例如，將值設定為`true`如果應用程式修改 *.cshtml*檔案在執行階段，或使用內嵌的檢視。</span><span class="sxs-lookup"><span data-stu-id="05a4c-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="05a4c-171">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="05a4c-172">當`true`，所有 Razor 內容項目 ( *.cshtml*檔案) 會標示為要包含在產生的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="05a4c-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="05a4c-173">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="05a4c-174">如果是 `true`，請將 RazorGenerate ( *.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="05a4c-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="05a4c-175">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="05a4c-176">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="05a4c-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="05a4c-177">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="05a4c-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="05a4c-178">當`true`時, SDK 會在執行時間產生 MVC 用來執行應用程式元件探索的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="05a4c-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |

<span data-ttu-id="05a4c-179">如需有關屬性的詳細資訊，請參閱 [MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。</span><span class="sxs-lookup"><span data-stu-id="05a4c-179">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="05a4c-180">目標</span><span class="sxs-lookup"><span data-stu-id="05a4c-180">Targets</span></span>

<span data-ttu-id="05a4c-181">Razor SDK 會定義兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="05a4c-181">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="05a4c-182">`RazorGenerate` &ndash; 程式碼會產生 *.cs*從檔案`RazorGenerate`item 項目。</span><span class="sxs-lookup"><span data-stu-id="05a4c-182">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="05a4c-183">`RazorGenerateDependsOn`使用屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="05a4c-183">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="05a4c-184">`RazorCompile` &ndash; 編譯會產生 *.cs* Razor 組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="05a4c-184">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="05a4c-185">`RazorCompileDependsOn`使用來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="05a4c-185">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="05a4c-186">`RazorComponentGenerate`程式碼會產生專案元素`RazorComponent`的 .cs 檔案。 &ndash;</span><span class="sxs-lookup"><span data-stu-id="05a4c-186">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="05a4c-187">`RazorComponentGenerateDependsOn`使用屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="05a4c-187">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="05a4c-188">Razor 檢視的執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="05a4c-188">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="05a4c-189">根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。</span><span class="sxs-lookup"><span data-stu-id="05a4c-189">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="05a4c-190">當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。</span><span class="sxs-lookup"><span data-stu-id="05a4c-190">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="05a4c-191">將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。</span><span class="sxs-lookup"><span data-stu-id="05a4c-191">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="05a4c-192">Web 應用程式，請確定您的應用程式的目標`Microsoft.NET.Sdk.Web`SDK。</span><span class="sxs-lookup"><span data-stu-id="05a4c-192">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="05a4c-193">Razor 語言版本</span><span class="sxs-lookup"><span data-stu-id="05a4c-193">Razor language version</span></span>

<span data-ttu-id="05a4c-194">以`Microsoft.NET.Sdk.Web` SDK 為目標時, 會從應用程式的目標 framework 版本推斷 Razor 語言版本。</span><span class="sxs-lookup"><span data-stu-id="05a4c-194">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the the app's target framework version.</span></span> <span data-ttu-id="05a4c-195">針對以 SDK 為`Microsoft.NET.Sdk.Razor`目標的專案, 或在罕見的情況下, 應用程式需要比推斷值不同的 Razor 語言版本, 您可以在應用程式`<RazorLangVersion>`的專案檔中設定屬性來設定版本:</span><span class="sxs-lookup"><span data-stu-id="05a4c-195">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="05a4c-196">Razor 的語言版本與所建立之執行時間的版本緊密整合。</span><span class="sxs-lookup"><span data-stu-id="05a4c-196">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="05a4c-197">不支援以不是針對執行時間設計的語言版本為目標, 而且可能會產生組建錯誤。</span><span class="sxs-lookup"><span data-stu-id="05a4c-197">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="05a4c-198">其他資源</span><span class="sxs-lookup"><span data-stu-id="05a4c-198">Additional resources</span></span>

* [<span data-ttu-id="05a4c-199">適用於 .NET Core 之 csproj 格式的新增項目</span><span class="sxs-lookup"><span data-stu-id="05a4c-199">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="05a4c-200">通用的 MSBuild 專案項目</span><span class="sxs-lookup"><span data-stu-id="05a4c-200">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
