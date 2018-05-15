---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
manager: wpickett
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.date: 4/12/2018
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: content
uid: mvc/razor-pages/sdk
ms.openlocfilehash: 2cbebb12ccd1098e1950aa7eeb22fab4ffc689e6
ms.sourcegitcommit: c79fd3592f444d58e17518914f8873d0a11219c0
ms.contentlocale: zh-TW
ms.lasthandoff: 04/18/2018
---
# <a name="aspnet-core-razor-sdk"></a><span data-ttu-id="fe9a9-103">ASP.NET Core Razor SDK</span><span class="sxs-lookup"><span data-stu-id="fe9a9-103">ASP.NET Core Razor SDK</span></span>

<span data-ttu-id="fe9a9-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fe9a9-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

[!INCLUDE[](~/includes/2.1.md)]

<span data-ttu-id="fe9a9-105">[!INCLUDE[](~/includes/2.1-SDK.md)] 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK)。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-105">The [!INCLUDE[](~/includes/2.1-SDK.md)] includes the `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK).</span></span> <span data-ttu-id="fe9a9-106">Razor SDK：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-106">The Razor SDK:</span></span>

* <span data-ttu-id="fe9a9-107">標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-107">Standardizes the experience around building, packaging, and publishing projects containing [Razor](xref:mvc/views/razor) files for ASP.NET Core MVC-based projects.</span></span>
* <span data-ttu-id="fe9a9-108">包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-108">Includes a set of predefined targets, properties, and items that allow customizing the compilation of Razor files.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="fe9a9-109">必要條件</span><span class="sxs-lookup"><span data-stu-id="fe9a9-109">Prerequisites</span></span>

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a><span data-ttu-id="fe9a9-110">使用 Razor SDK</span><span class="sxs-lookup"><span data-stu-id="fe9a9-110">Using the Razor SDK</span></span>

<span data-ttu-id="fe9a9-111">大部分的 Web 應用程式不需要明確參考 Razor SDK。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-111">Most web apps don't need to expressly reference the Razor SDK.</span></span> 

<span data-ttu-id="fe9a9-112">若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-112">To use the Razor SDK to build class libraries containing Razor views or Razor Pages:</span></span>

* <span data-ttu-id="fe9a9-113">使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-113">Use `Microsoft.NET.Sdk.Razor` instead of `Microsoft.NET.Sdk`:</span></span>
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* <span data-ttu-id="fe9a9-114">通常需要有 `Microsoft.AspNetCore.Mvc` 的套件參考，才能納入建置和編譯 Razor 頁面與 Razor 檢視所需的其他相依性。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-114">Typically a package reference to `Microsoft.AspNetCore.Mvc` is required to bring in additional dependencies required to build and compile Razor Pages and Razor views.</span></span> <span data-ttu-id="fe9a9-115">您的專案最少需要將套件參考新增至：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-115">At minimum, your project needs to add package references to:</span></span>

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 <span data-ttu-id="fe9a9-116">`Microsoft.AspNetCore.Mvc` 中包含上述套件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-116">The preceding packages are included in `Microsoft.AspNetCore.Mvc`.</span></span> <span data-ttu-id="fe9a9-117">下列標記會顯示基本 *.csproj* 檔案，該檔案使用 Razor SDK 來建置 ASP.NET Core Razor 頁面應用程式的 Razor 檔案：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-117">The following markup shows a basic *.csproj* file that uses the Razor SDK to build Razor files for an ASP.NET Core Razor Pages app:</span></span>
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a><span data-ttu-id="fe9a9-118">屬性</span><span class="sxs-lookup"><span data-stu-id="fe9a9-118">Properties</span></span>

<span data-ttu-id="fe9a9-119">下列屬性可在專案建置期間控制 Razor 的 SDK 行為：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-119">The following properties control the Razor's SDK behavior as part of a project build:</span></span>

* <span data-ttu-id="fe9a9-120">`RazorCompileOnBuild`：如果是 `true`，請在建置專案期間編譯和發出 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-120">`RazorCompileOnBuild` : When `true`, compiles and emits the Razor assembly as part of building the project.</span></span> <span data-ttu-id="fe9a9-121">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-121">Defaults to `true`.</span></span>
* <span data-ttu-id="fe9a9-122">`RazorCompileOnPublish`：如果是 `true`，請在發行專案期間編譯和發出 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-122">`RazorCompileOnPublish` : When `true`, compiles and emits the Razor assembly as part of publishing the project.</span></span> <span data-ttu-id="fe9a9-123">預設值為 `true`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-123">Defaults to `true`.</span></span>

<span data-ttu-id="fe9a9-124">下列的屬性和項目可用來設定 Razor SDK 的輸入和輸出：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-124">The following properties and items are used to configure inputs and output to the Razor SDK:</span></span>

| <span data-ttu-id="fe9a9-125">項目</span><span class="sxs-lookup"><span data-stu-id="fe9a9-125">Items</span></span>                                         | <span data-ttu-id="fe9a9-126">描述</span><span class="sxs-lookup"><span data-stu-id="fe9a9-126">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="fe9a9-127">RazorGenerate</span><span class="sxs-lookup"><span data-stu-id="fe9a9-127">RazorGenerate</span></span>                                 | <span data-ttu-id="fe9a9-128">其為程式碼產生目標輸入的項目元素 (*.cshtml* 檔案)。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-128">Item elements (*.cshtml* files) that are inputs to code generation targets.</span></span> |
| <span data-ttu-id="fe9a9-129">RazorCompile</span><span class="sxs-lookup"><span data-stu-id="fe9a9-129">RazorCompile</span></span>                                  | <span data-ttu-id="fe9a9-130">其為 Razor 編譯目標輸入的項目元素 (.cs 檔案)。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-130">Item elements (.cs files) that are inputs to  Razor compilation targets.</span></span> <span data-ttu-id="fe9a9-131">請使用此 ItemGroup 來指定要編譯成 Razor 組件的其他檔案。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-131">Use this ItemGroup to specify additional files to be compiled in to the Razor assembly.</span></span> |
| <span data-ttu-id="fe9a9-132">RazorAssemblyAttribute</span><span class="sxs-lookup"><span data-stu-id="fe9a9-132">RazorAssemblyAttribute</span></span>                        | <span data-ttu-id="fe9a9-133">用於 Razor 組件的程式碼產生屬性的項目元素。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-133">Item elements used to code generate attributes for the Razor assembly.</span></span> <span data-ttu-id="fe9a9-134">例如: </span><span class="sxs-lookup"><span data-stu-id="fe9a9-134">For example:</span></span>  <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| <span data-ttu-id="fe9a9-135">RazorEmbeddedResource</span><span class="sxs-lookup"><span data-stu-id="fe9a9-135">RazorEmbeddedResource</span></span>                         | <span data-ttu-id="fe9a9-136">新增為所產生 Razor 組件之內嵌資源的項目元素</span><span class="sxs-lookup"><span data-stu-id="fe9a9-136">Item elements added as embedded resources to the generated Razor assembly</span></span> |

| <span data-ttu-id="fe9a9-137">屬性</span><span class="sxs-lookup"><span data-stu-id="fe9a9-137">Property</span></span>                                      | <span data-ttu-id="fe9a9-138">描述</span><span class="sxs-lookup"><span data-stu-id="fe9a9-138">Description</span></span>                                                                   |
| ------------                                  | -------------                                                                 |
| <span data-ttu-id="fe9a9-139">RazorTargetName</span><span class="sxs-lookup"><span data-stu-id="fe9a9-139">RazorTargetName</span></span>                               | <span data-ttu-id="fe9a9-140">Razor 所產生組件的檔案名稱 (不含副檔名)。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-140">File name (without extension) of the assembly produced by Razor.</span></span> | 
| <span data-ttu-id="fe9a9-141">RazorOutputPath</span><span class="sxs-lookup"><span data-stu-id="fe9a9-141">RazorOutputPath</span></span>                               | <span data-ttu-id="fe9a9-142">Razor 輸出目錄。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-142">The Razor output directory.</span></span>                                      |
| <span data-ttu-id="fe9a9-143">RazorCompileToolset</span><span class="sxs-lookup"><span data-stu-id="fe9a9-143">RazorCompileToolset</span></span>                           | <span data-ttu-id="fe9a9-144">用來判斷可用來建置 Razor 組件的工具組。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-144">Used to determine the toolset used to build the Razor assembly.</span></span> <span data-ttu-id="fe9a9-145">有效值為 `Implicit` 和 `PrecompilationTool`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-145">Valid values are `Implicit`, , and `PrecompilationTool`.</span></span> |
| <span data-ttu-id="fe9a9-146">EnableDefaultContentItems</span><span class="sxs-lookup"><span data-stu-id="fe9a9-146">EnableDefaultContentItems</span></span>                     | <span data-ttu-id="fe9a9-147">如果是 `true`，請包括特定檔案類型 (例如 *.cshtml* 檔案)，作為專案中的內容。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-147">When `true`, includes certain file types, such as *.cshtml* files, as content in the project.</span></span> <span data-ttu-id="fe9a9-148">透過 Microsoft.NET.Sdk.Web 參考時，也包含 *wwwroot* 下的所有檔案，以及組態檔。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-148">When referenced via Microsoft.NET.Sdk.Web, also includes all files under *wwwroot*, and config files.</span></span>         |
| <span data-ttu-id="fe9a9-149">EnableDefaultRazorGenerateItems</span><span class="sxs-lookup"><span data-stu-id="fe9a9-149">EnableDefaultRazorGenerateItems</span></span>               | <span data-ttu-id="fe9a9-150">如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-150">When `true`, includes *.cshtml* files from `Content` items in `RazorGenerate` items.</span></span> |
| <span data-ttu-id="fe9a9-151">GenerateRazorTargetAssemblyInfo</span><span class="sxs-lookup"><span data-stu-id="fe9a9-151">GenerateRazorTargetAssemblyInfo</span></span>               | <span data-ttu-id="fe9a9-152">如果是 `true`，請產生包含 `RazorAssemblyAttribute` 所指定屬性的 *.cs* 檔案，並將其包含在編譯輸出中。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-152">When `true`, generates a *.cs* file containing attributes specified by `RazorAssemblyAttribute` and includes it in the compile output.</span></span> |
| <span data-ttu-id="fe9a9-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span><span class="sxs-lookup"><span data-stu-id="fe9a9-153">EnableDefaultRazorTargetAssemblyInfoAttributes</span></span> | <span data-ttu-id="fe9a9-154">如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-154">When `true`, adds a default set of assembly attributes to `RazorAssemblyAttribute`.</span></span> |
| <span data-ttu-id="fe9a9-155">CopyRazorGenerateFilesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="fe9a9-155">CopyRazorGenerateFilesToPublishDirectory</span></span>       | <span data-ttu-id="fe9a9-156">如果是 `true`，請將 RazorGenerate 項目 (*.cshtml*) 檔案複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-156">When `true`, copies RazorGenerate items (*.cshtml*) files to the publish directory.</span></span> <span data-ttu-id="fe9a9-157">如果已發行的應用程式在建置階段或發行階段參與編譯，通常不需要 Razor 檔案。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-157">Typically Razor files are not needed for a published application if they participate in compilation at build-time or publish-time.</span></span> <span data-ttu-id="fe9a9-158">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-158">Defaults to `false`.</span></span> |
| <span data-ttu-id="fe9a9-159">CopyRefAssembliesToPublishDirectory</span><span class="sxs-lookup"><span data-stu-id="fe9a9-159">CopyRefAssembliesToPublishDirectory</span></span>            | <span data-ttu-id="fe9a9-160">如果是 `true`，請將參考組件項目複製到發行目錄。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-160">When `true`, copy reference assembly items to the publish directory.</span></span> <span data-ttu-id="fe9a9-161">如果在建置階段或發行階段進行 Razor 編譯，已發行的應用程式通常不需要參考組件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-161">Typically reference assemblies are not needed for a published application if Razor compilation occurs at build-time or publish-time.</span></span> <span data-ttu-id="fe9a9-162">如果已發行的應用程式需要執行階段編譯，例如在執行階段修改 cshtml 檔案或是使用內嵌檢視，請設定為 `true`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-162">Set to `true`, if your published application requires runtime compilation, for example, modifies cshtml files at runtime, or uses embedded views.</span></span> <span data-ttu-id="fe9a9-163">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-163">Defaults to `false`.</span></span> |
| <span data-ttu-id="fe9a9-164">IncludeRazorContentInPack</span><span class="sxs-lookup"><span data-stu-id="fe9a9-164">IncludeRazorContentInPack</span></span>                      | <span data-ttu-id="fe9a9-165">如果是 `true`，所有 Razor 內容項目 (*.cshtml* 檔案) 將會標示為要包含在產生的 NuGet 套件中。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-165">When `true`, all Razor content items (*.cshtml* files) will be marked for inclusion in the generated NuGet package.</span></span> <span data-ttu-id="fe9a9-166">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-166">Defaults to `false`.</span></span> |
| <span data-ttu-id="fe9a9-167">EmbedRazorGenerateSources</span><span class="sxs-lookup"><span data-stu-id="fe9a9-167">EmbedRazorGenerateSources</span></span> | <span data-ttu-id="fe9a9-168">如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-168">When `true`, adds RazorGenerate (*.cshtml*) items as embedded files to the generated Razor assembly.</span></span> <span data-ttu-id="fe9a9-169">預設值為 `false`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-169">Defaults to `false`.</span></span> |
| <span data-ttu-id="fe9a9-170">UseRazorBuildServer</span><span class="sxs-lookup"><span data-stu-id="fe9a9-170">UseRazorBuildServer</span></span>                           | <span data-ttu-id="fe9a9-171">如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-171">When `true`, uses a persistent build server process to offload code generation work.</span></span> <span data-ttu-id="fe9a9-172">預設值為 `UseSharedCompilation`。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-172">Defaults to the value of `UseSharedCompilation`.</span></span> |

### <a name="targets"></a><span data-ttu-id="fe9a9-173">目標</span><span class="sxs-lookup"><span data-stu-id="fe9a9-173">Targets</span></span>
<span data-ttu-id="fe9a9-174">Razor SDK 會定義兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="fe9a9-174">The Razor SDK defines two primary targets:</span></span>

* <span data-ttu-id="fe9a9-175">`RazorGenerate` - 程式碼會從 RazorGenerate 項目元素產生 *.cs*。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-175">`RazorGenerate` - Code generates *.cs* files from RazorGenerate item elements.</span></span> <span data-ttu-id="fe9a9-176">請使用 `RazorGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-176">Use `RazorGenerateDependsOn` property to specify additional targets that can run before or after this target.</span></span>
* <span data-ttu-id="fe9a9-177">`RazorCompile` - 將產生的 *.cs* 檔案編譯成 Razor 組件。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-177">`RazorCompile` - Compiles generated *.cs* files in to a Razor assembly.</span></span> <span data-ttu-id="fe9a9-178">請使用 `RazorCompileDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。</span><span class="sxs-lookup"><span data-stu-id="fe9a9-178">Use `RazorCompileDependsOn` to specify additional targets that can run before or after this target.</span></span>