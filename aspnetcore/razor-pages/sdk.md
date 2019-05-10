---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: de51c9443e639cd64c234b6975cf7252bb7a2b9a
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64895295"
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="59056-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="59056-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="59056-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="59056-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="59056-105">[!INCLUDE[](~/includes/2.1-SDK.md)]包含`Microsoft.NET.Sdk.Razor`MSBuild SDK (Razor SDK)。</span><span class="sxs-lookup"><span data-stu-id="59056-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="59056-106">Razor SDK：</span><span class="sxs-lookup"><span data-stu-id="59056-106">The Razor SDK:</span></span>

* <span data-ttu-id="59056-107">標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。</span><span class="sxs-lookup"><span data-stu-id="59056-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="59056-108">包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。</span><span class="sxs-lookup"><span data-stu-id="59056-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="59056-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="59056-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="59056-110">使用 Razor SDK</span><span class="sxs-lookup"><span data-stu-id="59056-110">Using the Razor SDK</span></span>

<span data-ttu-id="59056-111">大部分的 web 應用程式不需要明確參考 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="59056-111">Most web apps aren't required to explicitly reference the Razor SDK.</span></span>

<span data-ttu-id="59056-112">若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：</span><span class="sxs-lookup"><span data-stu-id="59056-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="59056-113">使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：</span><span class="sxs-lookup"><span data-stu-id="59056-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* <span data-ttu-id="59056-114">一般而言，套件參考至`Microsoft.AspNetCore.Mvc`，才能接收來建置和編譯 Razor 頁面與 Razor 檢視所需的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="59056-114">Typically, a package reference to `Microsoft.AspNetCore.Mvc` is required to receive additional dependencies that are required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="59056-115">最少，您的專案應該新增套件參考：</span><span class="sxs-lookup"><span data-stu-id="59056-115">At a minimum, your project should add package references to:</span></span>

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  <span data-ttu-id="59056-116">`Microsoft.AspNetCore.Razor.Design`套件提供 Razor 編譯工作和目標專案。</span><span class="sxs-lookup"><span data-stu-id="59056-116">The `Microsoft.AspNetCore.Razor.Design` package provides the Razor compilation tasks and targets for the project.</span></span>

  <span data-ttu-id="59056-117">`Microsoft.AspNetCore.Mvc` 中包含上述套件。</span><span class="sxs-lookup"><span data-stu-id="59056-117">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="59056-118">下列標記會顯示使用 Razor SDK 來建置 ASP.NET Core Razor 頁面應用程式的 Razor 檔案的專案檔：</span><span class="sxs-lookup"><span data-stu-id="59056-118">The following markup shows a project file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> <span data-ttu-id="59056-119">`Microsoft.AspNetCore.Razor.Design`並`Microsoft.AspNetCore.Mvc.Razor.Extensions`套件中包含[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。</span><span class="sxs-lookup"><span data-stu-id="59056-119">The `Microsoft.AspNetCore.Razor.Design` and `Microsoft.AspNetCore.Mvc.Razor.Extensions` packages are included in the [Microsoft.AspNetCore.App metapackage](xref:fundamentals/metapackage-app).</span></span> <span data-ttu-id="59056-120">不過，版本小於`Microsoft.AspNetCore.App`封裝參考文件提供的中繼套件，且不包含最新版本的應用程式`Microsoft.AspNetCore.Razor.Design`。</span><span class="sxs-lookup"><span data-stu-id="59056-120">However, the version-less `Microsoft.AspNetCore.App` package reference provides a metapackage to the app that doesn't include the latest version of `Microsoft.AspNetCore.Razor.Design`.</span></span> <span data-ttu-id="59056-121">專案必須參考的版本一致性`Microsoft.AspNetCore.Razor.Design`(或`Microsoft.AspNetCore.Mvc`)，以便包含最新的建置時間修正 razor。</span><span class="sxs-lookup"><span data-stu-id="59056-121">Projects must reference a consistent version of `Microsoft.AspNetCore.Razor.Design` (or `Microsoft.AspNetCore.Mvc`) so that the latest build-time fixes for Razor are included.</span></span> <span data-ttu-id="59056-122">如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。</span><span class="sxs-lookup"><span data-stu-id="59056-122">For more information, see [this GitHub issue](https://github.com/aspnet/Razor/issues/2553).</span></span>

::: moniker-end

### <a name="properties"></a><span data-ttu-id="59056-123">屬性</span><span class="sxs-lookup"><span data-stu-id="59056-123">Properties</span></span>

<span data-ttu-id="59056-124">下列屬性可在專案建置期間控制 Razor 的 SDK 行為：</span><span class="sxs-lookup"><span data-stu-id="59056-124">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="59056-125">`RazorCompileOnBuild` &ndash; 當`true`、 編譯和建置專案時的 Razor 組件就會發出。</span><span class="sxs-lookup"><span data-stu-id="59056-125">`RazorCompileOnBuild` &ndash; When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="59056-126">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="59056-126">Defaults to `true`.</span></span>
* <span data-ttu-id="59056-127">`RazorCompileOnPublish` &ndash; 當`true`、 編譯，並會發出 Razor 組件，在發行專案。</span><span class="sxs-lookup"><span data-stu-id="59056-127">`RazorCompileOnPublish` &ndash; When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="59056-128">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="59056-128">Defaults to `true`.</span></span>

<span data-ttu-id="59056-129">下表中的項目與屬性用來設定輸入和輸出至 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="59056-129">The properties and items in the following table are used to configure inputs and output to the Razor SDK.</span></span>

| <span data-ttu-id="59056-130">項目</span><span class="sxs-lookup"><span data-stu-id="59056-130">Items</span></span> | <span data-ttu-id="59056-131">描述</span><span class="sxs-lookup"><span data-stu-id="59056-131">Description</span></span> |
| ----- | ----------- |
| `RazorGenerate` | <span data-ttu-id="59056-132">其為程式碼產生目標輸入的項目元素 (*.cshtml* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="59056-132">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| `RazorCompile` | <span data-ttu-id="59056-133">Item 項目 (*.cs*檔案)，是 Razor 編譯目標的輸入。</span><span class="sxs-lookup"><span data-stu-id="59056-133">Item elements (*.cs* files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="59056-134">請使用此 ItemGroup 來指定要編譯成 Razor 組件的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="59056-134">Use this ItemGroup to specify additional files to be compiled into the Razor assembly.</span></span> |
| `RazorTargetAssemblyAttribute` | <span data-ttu-id="59056-135">用於 Razor 組件的程式碼產生屬性的項目元素。</span><span class="sxs-lookup"><span data-stu-id="59056-135">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="59056-136">例如: </span><span class="sxs-lookup"><span data-stu-id="59056-136">For example:</span></span>  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | <span data-ttu-id="59056-137">加入做為內嵌資源產生的 Razor 組件的項目。</span><span class="sxs-lookup"><span data-stu-id="59056-137">Item elements added as embedded resources to the generated Razor assembly.</span></span> |

| <span data-ttu-id="59056-138">屬性</span><span class="sxs-lookup"><span data-stu-id="59056-138">Property</span></span> | <span data-ttu-id="59056-139">描述</span><span class="sxs-lookup"><span data-stu-id="59056-139">Description</span></span> |
| -------- | ----------- |
| `RazorTargetName` | <span data-ttu-id="59056-140">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="59056-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| `RazorOutputPath` | <span data-ttu-id="59056-141">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="59056-141">The Razor output directory.</span></span> |
| `RazorCompileToolset` | <span data-ttu-id="59056-142">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="59056-142">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="59056-143">有效值為 `Implicit`、`RazorSDK` 及 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="59056-143">Valid values are `Implicit`, `RazorSDK`, and `PrecompilationTool`.</span></span> |
| [<span data-ttu-id="59056-144">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="59056-144">EnableDefaultContentItems</span></span>](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | <span data-ttu-id="59056-145">預設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="59056-145">Default is `true`.</span></span> <span data-ttu-id="59056-146">當`true`，包括*web.config*， *.json*，和 *.cshtml*檔案做為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="59056-146">When `true`, includes *web.config*, *.json*, and *.cshtml* files as content in the project.</span></span> <span data-ttu-id="59056-147">透過參考時`Microsoft.NET.Sdk.Web`，檔案底下*wwwroot*和組態檔也會包含在內。</span><span class="sxs-lookup"><span data-stu-id="59056-147">When referenced via `Microsoft.NET.Sdk.Web`, files under *wwwroot* and config files are also included.</span></span> |
| `EnableDefaultRazorGenerateItems` | <span data-ttu-id="59056-148">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="59056-148">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| `GenerateRazorTargetAssemblyInfo` | <span data-ttu-id="59056-149">當`true`，會產生 *.cs*檔案，其中包含指定屬性`RazorAssemblyAttribute`和包含編譯輸出中的檔案。</span><span class="sxs-lookup"><span data-stu-id="59056-149">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes the file in the compile output.</span></span> |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | <span data-ttu-id="59056-150">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="59056-150">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| `CopyRazorGenerateFilesToPublishDirectory` | <span data-ttu-id="59056-151">當`true`，複製`RazorGenerate`項目 (*.cshtml*) 至發行目錄的檔案。</span><span class="sxs-lookup"><span data-stu-id="59056-151">When `true`, copies `RazorGenerate` items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="59056-152">一般而言，Razor 檔案不需要針對已發佈的應用程式，如果它們參與在建置階段 」 或 「 發行階段的編譯。</span><span class="sxs-lookup"><span data-stu-id="59056-152">Typically, Razor files aren't required for a published app if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="59056-153">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="59056-153">Defaults to `false`.</span></span> |
| `CopyRefAssembliesToPublishDirectory` | <span data-ttu-id="59056-154">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="59056-154">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="59056-155">一般而言，參考組件不需要的已發佈的應用程式，如果 Razor 編譯，就會發生在建置階段或發行時間。</span><span class="sxs-lookup"><span data-stu-id="59056-155">Typically, reference assemblies aren't required for a published app if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="59056-156">若要設定`true`如果您已發佈的應用程式需要執行階段編譯。</span><span class="sxs-lookup"><span data-stu-id="59056-156">Set to `true` if your published app requires runtime compilation.</span></span> <span data-ttu-id="59056-157">例如，將值設定為`true`如果應用程式修改 *.cshtml*檔案在執行階段，或使用內嵌的檢視。</span><span class="sxs-lookup"><span data-stu-id="59056-157">For example, set the value to `true` if the app modifies *.cshtml* files at runtime or uses embedded views.</span></span> <span data-ttu-id="59056-158">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="59056-158">Defaults to `false`.</span></span> |
| `IncludeRazorContentInPack` | <span data-ttu-id="59056-159">當`true`，所有 Razor 內容項目 (*.cshtml*檔案) 會標示為要包含在產生的 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="59056-159">When `true`, all Razor content items (*.cshtml* files) are marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="59056-160">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="59056-160">Defaults to `false`.</span></span> |
| `EmbedRazorGenerateSources` | <span data-ttu-id="59056-161">如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="59056-161">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="59056-162">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="59056-162">Defaults to `false`.</span></span> |
| `UseRazorBuildServer` | <span data-ttu-id="59056-163">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="59056-163">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="59056-164">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="59056-164">Defaults to the value of `UseSharedCompilation`.</span></span> |

<span data-ttu-id="59056-165">如需有關屬性的詳細資訊，請參閱 [MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。</span><span class="sxs-lookup"><span data-stu-id="59056-165">For more information on properties, see [MSBuild properties](/visualstudio/msbuild/msbuild-properties).</span></span>

### <a name="targets"></a><span data-ttu-id="59056-166">目標</span><span class="sxs-lookup"><span data-stu-id="59056-166">Targets</span></span>

<span data-ttu-id="59056-167">Razor SDK 會定義兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="59056-167">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="59056-168">`RazorGenerate` &ndash; 程式碼會產生 *.cs*從檔案`RazorGenerate`item 項目。</span><span class="sxs-lookup"><span data-stu-id="59056-168">`RazorGenerate` &ndash; Code generates *.cs* files from `RazorGenerate` item elements.</span></span> <span data-ttu-id="59056-169">請使用 `RazorGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="59056-169">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="59056-170">`RazorCompile` &ndash; 編譯會產生 *.cs* Razor 組件中的檔案。</span><span class="sxs-lookup"><span data-stu-id="59056-170">`RazorCompile` &ndash; Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="59056-171">請使用 `RazorCompileDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="59056-171">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>

### <a name="runtime-compilation-of-razor-views"></a><span data-ttu-id="59056-172">Razor 檢視的執行階段編譯</span><span class="sxs-lookup"><span data-stu-id="59056-172">Runtime compilation of Razor views</span></span>

* <span data-ttu-id="59056-173">根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。</span><span class="sxs-lookup"><span data-stu-id="59056-173">By default, the Razor SDK doesn't publish reference assemblies that are required to perform runtime compilation.</span></span> <span data-ttu-id="59056-174">當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。</span><span class="sxs-lookup"><span data-stu-id="59056-174">This results in compilation failures when the application model relies on runtime compilation&mdash;for example, the app uses embedded views or changes views after the app is published.</span></span> <span data-ttu-id="59056-175">將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。</span><span class="sxs-lookup"><span data-stu-id="59056-175">Set `CopyRefAssembliesToPublishDirectory` to `true` to continue publishing reference assemblies.</span></span>

* <span data-ttu-id="59056-176">Web 應用程式，請確定您的應用程式的目標`Microsoft.NET.Sdk.Web`SDK。</span><span class="sxs-lookup"><span data-stu-id="59056-176">For a web app, ensure your app is targeting the `Microsoft.NET.Sdk.Web` SDK.</span></span>
