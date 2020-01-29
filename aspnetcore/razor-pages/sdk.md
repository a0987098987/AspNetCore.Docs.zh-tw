---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor 頁面如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
no-loc:
- Blazor
uid: razor-pages/sdk
ms.openlocfilehash: 872d90662494735dc0e4caa01c46fcdcc2606bc6
ms.sourcegitcommit: b5ceb0a46d0254cc3425578116e2290142eec0f0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/28/2020
ms.locfileid: "76809129"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="9b69b-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="9b69b-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="9b69b-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="9b69b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="overview"></a><span data-ttu-id="9b69b-105">概觀</span><span class="sxs-lookup"><span data-stu-id="9b69b-105">Overview</span></span>

<span data-ttu-id="9b69b-106">[!INCLUDE[](~/includes/2.1-SDK.md)] 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK （Razor SDK）。</span><span class="sxs-lookup"><span data-stu-id="9b69b-106">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="9b69b-107">Razor SDK：</span><span class="sxs-lookup"><span data-stu-id="9b69b-107">The Razor SDK:</span></span>

::: moniker range=">= aspnetcore-3.0"

* <span data-ttu-id="9b69b-108">需要建立、封裝和發行包含[Razor](xref:mvc/views/razor)檔案的專案，以用於 ASP.NET Core MVC 或[Blazor](xref:blazor/index)專案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-108">Is required to build, package, and publish projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based or [Blazor](xref:blazor/index) projects.</span></span>
* <span data-ttu-id="9b69b-109">包含一組預先定義的目標、屬性和專案，可讓您自訂 Razor （*cshtml*或*Razor*）檔案的編譯。</span><span class="sxs-lookup"><span data-stu-id="9b69b-109">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor (*.cshtml* or *.razor*) files.</span></span>

<span data-ttu-id="9b69b-110">Razor SDK 包括 `Include` 屬性設為 `**\*.cshtml` 和 `**\*.razor` 萬用字元模式的 `Content` 專案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-110">The Razor SDK includes `Content` items with `Include` attributes set to the `**\*.cshtml` and `**\*.razor` globbing patterns.</span></span> <span data-ttu-id="9b69b-111">已發行相符的檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-111">Matching files are published.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* <span data-ttu-id="9b69b-112">標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。</span><span class="sxs-lookup"><span data-stu-id="9b69b-112">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="9b69b-113">包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。</span><span class="sxs-lookup"><span data-stu-id="9b69b-113">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

<span data-ttu-id="9b69b-114">Razor SDK 包含 `Content` 專案，其中 `Include` 屬性設定為 `**\*.cshtml` 的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="9b69b-114">The Razor SDK includes a `Content` item with an `Include` attribute set to the `**\*.cshtml` globbing pattern.</span></span> <span data-ttu-id="9b69b-115">已發行相符的檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-115">Matching files are published.</span></span>

::: moniker-end

## <a name="prerequisites"></a><span data-ttu-id="9b69b-116">必要條件：</span><span class="sxs-lookup"><span data-stu-id="9b69b-116">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a><span data-ttu-id="9b69b-117">使用 Razor SDK</span><span class="sxs-lookup"><span data-stu-id="9b69b-117">Use the Razor SDK</span></span>

<span data-ttu-id="9b69b-118">大部分的 web 應用程式都不需要明確參考 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="9b69b-118">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="9b69b-119">若要使用 Razor SDK 來建立包含 Razor views 或 Razor Pages 的類別庫，建議您從 Razor 類別庫（RCL）專案範本開始。</span><span class="sxs-lookup"><span data-stu-id="9b69b-119">To use the Razor SDK to build class libraries containing Razor views or Razor Pages, we recommend starting with the Razor class library (RCL) project template.</span></span> <span data-ttu-id="9b69b-120">用來建立 Blazor （*razor*）檔案的 RCL，最少需要[AspNetCore 元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)套件的參考。</span><span class="sxs-lookup"><span data-stu-id="9b69b-120">An RCL that's used to build Blazor (*.razor*) files minimally requires a reference to the [Microsoft.AspNetCore.Components](https://www.nuget.org/packages/Microsoft.AspNetCore.Components) package.</span></span> <span data-ttu-id="9b69b-121">用來建立 Razor views 或 pages （*cshtml*檔案）的 RCL，至少需要以 `netcoreapp3.0` 或更新版本為目標，並且在其專案檔中具有[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)的 `FrameworkReference`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-121">An RCL that's used to build Razor views or pages (*.cshtml* files) minimally requires targeting `netcoreapp3.0` or later and has a `FrameworkReference` to the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app) in its project file.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="9b69b-122">若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：</span><span class="sxs-lookup"><span data-stu-id="9b69b-122">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="9b69b-123">使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：</span><span class="sxs-lookup"><span data-stu-id="9b69b-123">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* <span data-ttu-id="9b69b-124">通常，需要 `Microsoft.AspNetCore.Mvc` 的套件參考，才能收到建立和編譯 Razor Pages 和 Razor views 所需的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="9b69b-124">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="9b69b-125">您的專案至少應該將套件參考新增至：</span><span class="sxs-lookup"><span data-stu-id="9b69b-125">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  <span data-ttu-id="9b69b-126">`Microsoft.AspNetCore.Razor.Design` 套件會提供專案的 Razor 編譯工作和目標。</span><span class="sxs-lookup"><span data-stu-id="9b69b-126">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="9b69b-127">`Microsoft.AspNetCore.Mvc` 中包含上述套件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-127">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="9b69b-128">下列標記顯示的專案檔會使用 Razor SDK 來建立 ASP.NET Core Razor Pages 應用程式的 Razor 檔案：</span><span class="sxs-lookup"><span data-stu-id="9b69b-128">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="9b69b-129">`Microsoft.AspNetCore.Razor.Design` 和 `Microsoft.AspNetCore.Mvc.Razor.Extensions` 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。</span><span class="sxs-lookup"><span data-stu-id="9b69b-129">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="9b69b-130">不過，無版本的 `Microsoft.AspNetCore.App` 套件參考會提供應用程式的中繼套件，其中不包含最新版本的 `Microsoft.AspNetCore.Razor.Design`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-130">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="9b69b-131">專案必須參考一致版本的 `Microsoft.AspNetCore.Razor.Design` （或 `Microsoft.AspNetCore.Mvc`），才會包含 Razor 的最新組建時間修正程式。</span><span class="sxs-lookup"><span data-stu-id="9b69b-131">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="9b69b-132">如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。</span><span class="sxs-lookup"><span data-stu-id="9b69b-132">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="9b69b-133">屬性</span><span class="sxs-lookup"><span data-stu-id="9b69b-133">Properties</span></span>

<span data-ttu-id="9b69b-134">下列屬性可在專案建置期間控制 Razor 的 SDK 行為：</span><span class="sxs-lookup"><span data-stu-id="9b69b-134">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="9b69b-135">`RazorCompileOnBuild` &ndash; `true`時，會編譯併發出 Razor 元件，做為建立專案的一部分。</span><span class="sxs-lookup"><span data-stu-id="9b69b-135">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="9b69b-136">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-136">Defaults to `true`.</span></span>
* <span data-ttu-id="9b69b-137">當 `true`時，`RazorCompileOnPublish` &ndash;，會在發行專案時編譯和發出 Razor 元件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-137">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="9b69b-138">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-138">Defaults to `true`.</span></span>

<span data-ttu-id="9b69b-139">下表中的屬性和專案可用來設定 Razor SDK 的輸入和輸出。</span><span class="sxs-lookup"><span data-stu-id="9b69b-139">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> <span data-ttu-id="9b69b-140">從 ASP.NET Core 3.0 開始，如果專案檔中的 `RazorCompileOnBuild` 或 `RazorCompileOnPublish` MSBuild 屬性已停用，則預設不會提供 MVC Views 或 Razor Pages。</span><span class="sxs-lookup"><span data-stu-id="9b69b-140">Starting with ASP.NET Core 3.0, MVC Views or Razor Pages aren't served by default if the `RazorCompileOnBuild` or `RazorCompileOnPublish` MSBuild properties in the project file are disabled.</span></span> <span data-ttu-id="9b69b-141">如果應用程式依賴執行時間編譯來處理*cshtml*檔案，則應用程式必須新增[microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)封裝的明確參考。</span><span class="sxs-lookup"><span data-stu-id="9b69b-141">Applications must add an explicit reference to the [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation) package if the app relies on runtime compilation to process *.cshtml* files.</span></span>

::: moniker-end

| <span data-ttu-id="9b69b-142">Items</span><span class="sxs-lookup"><span data-stu-id="9b69b-142">Items</span></span> | <span data-ttu-id="9b69b-143">描述</span><span class="sxs-lookup"><span data-stu-id="9b69b-143">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="9b69b-144">屬於程式碼產生之輸入的專案元素（ *. cshtml*檔案）。</span><span class="sxs-lookup"><span data-stu-id="9b69b-144">Item elements (*.cshtml* files) that are inputs to code generation.</span></span> |
| `RazorComponent` | <span data-ttu-id="9b69b-145">做為 Razor 元件程式碼產生輸入的專案元素（*razor*檔案）。</span><span class="sxs-lookup"><span data-stu-id="9b69b-145">Item elements (*.razor* files) that are inputs to Razor component code generation.</span></span> |
| `RazorCompile` | <span data-ttu-id="9b69b-146">做為 Razor 編譯目標輸入的專案元素（ *.cs*檔案）。</span><span class="sxs-lookup"><span data-stu-id="9b69b-146">Item elements (*.cs* files) that are inputs to Razor compilation targets.</span></span> <span data-ttu-id="9b69b-147">使用此 `ItemGroup` 來指定要編譯成 Razor 元件的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-147">Use this `ItemGroup` to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="9b69b-148">用於 Razor 組件的程式碼產生屬性的項目元素。</span><span class="sxs-lookup"><span data-stu-id="9b69b-148">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="9b69b-149">例如：</span><span class="sxs-lookup"><span data-stu-id="9b69b-149">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="9b69b-150">做為內嵌資源加入至產生的 Razor 元件的專案元素。</span><span class="sxs-lookup"><span data-stu-id="9b69b-150">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="9b69b-151">屬性</span><span class="sxs-lookup"><span data-stu-id="9b69b-151">Property</span></span> | <span data-ttu-id="9b69b-152">描述</span><span class="sxs-lookup"><span data-stu-id="9b69b-152">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="9b69b-153">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="9b69b-153">File name (without extension) of the assembly produced by Razor.</span></span> |
| `RazorOutputPath` | <span data-ttu-id="9b69b-154">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="9b69b-154">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="9b69b-155">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="9b69b-155">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="9b69b-156">有效值為 `Implicit`、`RazorSDK` 及 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-156">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="9b69b-157">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="9b69b-157">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="9b69b-158">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-158">Default is `true`.</span></span> <span data-ttu-id="9b69b-159">當 `true`*時，會包含 web.config*、 *. json*和*cshtml*檔案做為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="9b69b-159">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="9b69b-160">透過 `Microsoft.NET.Sdk.Web`來參考時，也會包含*wwwroot*和 config 檔案底下的檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-160">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="9b69b-161">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-161">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="9b69b-162">當 `true`時，會產生 *.cs*檔案，其中包含 `RazorAssemblyAttribute` 指定的屬性，並在編譯輸出中包含檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-162">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="9b69b-163">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-163">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="9b69b-164">當 `true`時，會將 `RazorGenerate` 專案（*cshtml*）檔案複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="9b69b-164">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="9b69b-165">通常，如果已發行的應用程式在組建時間或發行時間參與編譯，則不需要 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-165">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="9b69b-166">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-166">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="9b69b-167">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="9b69b-167">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="9b69b-168">一般來說，如果在組建時間或發行時間發生 Razor 編譯，則發行的應用程式不需要參考元件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-168">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="9b69b-169">如果您已發佈的應用程式需要執行時間編譯，請設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-169">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="9b69b-170">例如，如果應用程式在執行時間修改 cshtml 檔案，或使用內嵌的視圖，請將值設定為 `true` *。*</span><span class="sxs-lookup"><span data-stu-id="9b69b-170">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="9b69b-171">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-171">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="9b69b-172">當 `true`時，所有 Razor 內容專案（*cshtml*檔案）都會標示為包含在產生的 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="9b69b-172">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="9b69b-173">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-173">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="9b69b-174">如果是 `true`，請將 RazorGenerate ( *.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-174">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="9b69b-175">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-175">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="9b69b-176">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="9b69b-176">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="9b69b-177">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="9b69b-177">Defaults to the value of `UseSharedCompilation`.</span></span> |
| `GenerateMvcApplicationPartsAssemblyAttributes` | <span data-ttu-id="9b69b-178">當 `true`時，SDK 會在執行時間產生 MVC 用來執行應用程式元件探索的其他屬性。</span><span class="sxs-lookup"><span data-stu-id="9b69b-178">When `true`, the SDK generates additional attributes used by MVC at runtime to perform application part discovery.</span></span> |
| `DefaultWebContentItemExcludes` | <span data-ttu-id="9b69b-179">要從以 Web 或 Razor SDK 為目標之專案中的 `Content` 專案群組中排除之專案元素的萬用字元模式。</span><span class="sxs-lookup"><span data-stu-id="9b69b-179">A globbing pattern for item elements that are to be excluded from the `Content` item group in projects targeting the Web or Razor SDK</span></span> |
| `ExcludeConfigFilesFromBuildOutput` | <span data-ttu-id="9b69b-180">當 `true`時， *.config*和 *. json*檔案不會複製到組建輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="9b69b-180">When `true`, *.config* and *.json* files do not get copied to the build output directory.</span></span> |
| `AddRazorSupportForMvc` | <span data-ttu-id="9b69b-181">當 `true`時，會設定 Razor SDK 來新增建立包含 MVC views 或 Razor Pages 之應用程式時所需的 MVC 設定支援。</span><span class="sxs-lookup"><span data-stu-id="9b69b-181">When `true`, configures the Razor SDK to add support for the MVC configuration that is required when building applications containing MVC views or Razor Pages.</span></span> <span data-ttu-id="9b69b-182">針對以 Web SDK 為目標的 .NET Core 3.0 或更新版本專案，會以隱含方式設定此屬性。</span><span class="sxs-lookup"><span data-stu-id="9b69b-182">This property is implicitly set for .NET Core 3.0 or later projects targeting the Web SDK</span></span> |
| `RazorLangVersion` | <span data-ttu-id="9b69b-183">要設為目標的 Razor 語言版本。</span><span class="sxs-lookup"><span data-stu-id="9b69b-183">The version of the Razor Language to target.</span></span> |

<span data-ttu-id="9b69b-184">如需有關屬性的詳細資訊，請參閱 [MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。</span><span class="sxs-lookup"><span data-stu-id="9b69b-184">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="9b69b-185">目標</span><span class="sxs-lookup"><span data-stu-id="9b69b-185">Targets</span></span>

<span data-ttu-id="9b69b-186">Razor SDK 會定義兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="9b69b-186">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="9b69b-187">`RazorGenerate` &ndash; 程式碼會從 `RazorGenerate` 專案元素產生 *.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-187">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="9b69b-188">使用 [`RazorGenerateDependsOn`] 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="9b69b-188">Use the `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="9b69b-189">`RazorCompile` &ndash; 會將產生的 *.cs*檔案編譯到 Razor 元件中。</span><span class="sxs-lookup"><span data-stu-id="9b69b-189">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="9b69b-190">使用 [`RazorCompileDependsOn`] 指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="9b69b-190">Use the `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="9b69b-191">`RazorComponentGenerate` &ndash; 程式碼會為 `RazorComponent` 專案元素產生 *.cs*檔案。</span><span class="sxs-lookup"><span data-stu-id="9b69b-191">`RazorComponentGenerate` &ndash; Code generates *.cs* files for `RazorComponent` item elements.</span></span> <span data-ttu-id="9b69b-192">使用 [`RazorComponentGenerateDependsOn`] 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="9b69b-192">Use the `RazorComponentGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="9b69b-193">Razor 檢視的執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="9b69b-193">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="9b69b-194">根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-194">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="9b69b-195">當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。</span><span class="sxs-lookup"><span data-stu-id="9b69b-195">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="9b69b-196">將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。</span><span class="sxs-lookup"><span data-stu-id="9b69b-196">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="9b69b-197">針對 web 應用程式，請確定您的應用程式是以 `Microsoft.NET.Sdk.Web` SDK 為目標。</span><span class="sxs-lookup"><span data-stu-id="9b69b-197">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>

## <a name="razor-language-version"></a><span data-ttu-id="9b69b-198">Razor 語言版本</span><span class="sxs-lookup"><span data-stu-id="9b69b-198">Razor language version</span></span>

<span data-ttu-id="9b69b-199">以 `Microsoft.NET.Sdk.Web` SDK 為目標時，會從應用程式的目標 framework 版本推斷 Razor 語言版本。</span><span class="sxs-lookup"><span data-stu-id="9b69b-199">When targeting the `Microsoft.NET.Sdk.Web` SDK, the Razor language version is inferred from the app's target framework version.</span></span> <span data-ttu-id="9b69b-200">針對以 `Microsoft.NET.Sdk.Razor` SDK 為目標的專案，或在罕見的情況下，應用程式需要比推斷值不同的 Razor 語言版本，您可以在應用程式的專案檔中設定 `<RazorLangVersion>` 屬性來設定版本：</span><span class="sxs-lookup"><span data-stu-id="9b69b-200">For projects targeting the `Microsoft.NET.Sdk.Razor` SDK or in the rare case that the app requires a different Razor language version than the inferred value, a version can be configured by setting the `<RazorLangVersion>` property in the app's project file:</span></span>

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

<span data-ttu-id="9b69b-201">Razor 的語言版本與所建立之執行時間的版本緊密整合。</span><span class="sxs-lookup"><span data-stu-id="9b69b-201">Razor's language version is tightly integrated with the version of the runtime that it was built for.</span></span> <span data-ttu-id="9b69b-202">不支援以不是針對執行時間設計的語言版本為目標，而且可能會產生組建錯誤。</span><span class="sxs-lookup"><span data-stu-id="9b69b-202">Targeting a language version that isn't designed for the runtime is unsupported and likely produces build errors.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9b69b-203">其他資源</span><span class="sxs-lookup"><span data-stu-id="9b69b-203">Additional resources</span></span>

* [<span data-ttu-id="9b69b-204">適用於 .NET Core 之 csproj 格式的新增項目</span><span class="sxs-lookup"><span data-stu-id="9b69b-204">Additions to the csproj format for .NET Core</span></span>](/dotnet/core/tools/csproj)
* [<span data-ttu-id="9b69b-205">通用的 MSBuild 專案項目</span><span class="sxs-lookup"><span data-stu-id="9b69b-205">Common MSBuild project items</span></span>](/visualstudio/msbuild/common-msbuild-project-items)
