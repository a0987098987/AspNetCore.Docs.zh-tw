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
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

## <a name="overview"></a>概觀

[!INCLUDE[](~/includes/2.1-SDK.md)] 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK)。 Razor SDK：

::: moniker range=">= aspnetcore-3.0"

* 需要為基於 ASP.NET的 Icore MVC 或[Blazor](xref:blazor/index)專案生成、打包和發佈包含[Razor](xref:mvc/views/razor)檔案的專案。
* 包括一組預定義的目標、屬性和項,這些目標、屬性和項允許自定義 Razor *(.cshtml*或 *.razor)* 檔的編譯。

`Content`Razor SDK`Include`包括屬性`**\*.cshtml``**\*.razor`設定為和 globing 模式的專案。 將發表匹配的檔。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。
* 包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。

Razor SDK`Content`包括`Include`一個 屬性設置`**\*.cshtml`為 globing 模式的項。 將發表匹配的檔。

::: moniker-end

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>使用剃刀 SDK

大多數 Web 應用不需要顯式引用 Razor SDK。

::: moniker range=">= aspnetcore-3.0"

要使用 Razor SDK 構建包含 Razor 檢視或 Razor 頁面的類庫,我們建議從 Razor 類庫 (RCL) 項目範本開始。 用於建構 Blazor *(.razor*) 檔案的 RCL 需要引用[Microsoft.AspNetCore.元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)包。 編譯 Razor 檢視或頁面 *(.cshtml*檔案)`netcoreapp3.0`的 RCL`FrameworkReference`的檔案中具有到[Microsoft.AspNetCore.App 中繼 。](xref:fundamentals/metapackage-app)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：

* 使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* 通常,需要包`Microsoft.AspNetCore.Mvc`引用來接收生成和編譯 Razor 頁面和 Razor 視圖所需的其他依賴項。 至少,您的項目應加入包引照到:

  * `Microsoft.AspNetCore.Razor.Design`
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`

  該`Microsoft.AspNetCore.Razor.Design`包為專案提供 Razor 編譯任務和目標。

  `Microsoft.AspNetCore.Mvc` 中包含上述套件。 下面的標記顯示了一個項目檔,該檔案使用 Razor SDK 為 ASP.NET 核心剃刀頁面應用建構 Razor 檔:

  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> 和`Microsoft.AspNetCore.Razor.Design``Microsoft.AspNetCore.Mvc.Razor.Extensions`包包含在[Microsoft.AspNetCore.App 元包](xref:fundamentals/metapackage-app)中。 但是,無`Microsoft.AspNetCore.App`版本的包引用向不包含 最新版本的`Microsoft.AspNetCore.Razor.Design`應用 提供了元包。 項目必須引用 (`Microsoft.AspNetCore.Razor.Design``Microsoft.AspNetCore.Mvc`或 ) 的一致版本,以便包含 Razor 的最新生成時修補程式。 如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/aspnet/Razor/issues/2553) \(英文\)。

::: moniker-end

### <a name="properties"></a>屬性

下列屬性可在專案建置期間控制 Razor 的 SDK 行為：

* `RazorCompileOnBuild`&ndash;當`true`時, 編譯和發出 Razor 程式集作為建構專案的一部分。 預設為 `true`。
* `RazorCompileOnPublish`&ndash;當`true`時 ,編譯和發出 Razor 程式集作為發佈專案的一部分。 預設為 `true`。

下表中的屬性和項用於配置輸入和輸出到 Razor SDK。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> 從 ASP.NET酷 3.0 開始,預設情況下,如果禁用專案檔`RazorCompileOnBuild``RazorCompileOnPublish`中 的或 MSBuild 屬性,則不提供 MVC 視圖或 Razor 頁面。 如果應用依賴於運行時編譯來處理 *.cshtml*檔,則應用程式必須添加對[Microsoft.AspNetCore.Mvc.Razor.執行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)包的顯式引用。

::: moniker-end

| 項目 | 描述 |
| ----- | ----------- |
| `RazorGenerate` | 作為代碼生成輸入的項目 *(.cshtml*檔案)。 |
| `RazorComponent` | 作為 Razor 元件代碼生成輸入的項目 *(.razor*檔案)。 |
| `RazorCompile` | 作為 Razor 編譯目標的輸入的項目*元素 (.cs*檔)。 使用這個選項`ItemGroup`可指定要編譯到 Razor 程式集中的其他檔。 |
| `RazorTargetAssemblyAttribute` | 用於 Razor 組件的程式碼產生屬性的項目元素。 例如：  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | 作為嵌入資源添加到生成的 Razor 程式集的項目。 |

::: moniker range=">= aspnetcore-3.0"

| 屬性 | 描述 |
| -------- | ----------- |
| `RazorTargetName` | Razor 所產生組件的檔案名稱 (不含副檔名)。 |
| `RazorOutputPath` | Razor 輸出目錄。 |
| `RazorCompileToolset` | 用來判斷可用來建置 Razor 組件的工具組。 有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。 |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | 預設值為 `true`。 當`true`時 ,包括*Web.config* *、.json*和 *.cshtml*檔案作為專案中的內容。 當通過`Microsoft.NET.Sdk.Web`引用時 *,wwwroot*下的檔和配置檔也包括在內。 |
| `EnableDefaultRazorGenerateItems` | 如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。 |
| `GenerateRazorTargetAssemblyInfo` | 當`true`生成 *.cs*檔,`RazorAssemblyAttribute`其中包含指定的屬性,並在編譯輸出中包含該檔。 |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | 如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。 |
| `CopyRazorGenerateFilesToPublishDirectory` | 當`true``RazorGenerate`時 ,將項目 *(.cshtml*) 檔案複製到發佈目錄。 通常,如果已發佈的應用在生成時間或發佈時參與編譯,則不需要 Razor 檔。 預設為 `false`。 |
| `PreserveCompilationReferences` | 如果是 `true`，請將參考組件項目複製到發行目錄。 通常,如果 Razor 編譯發生在生成時間或發佈時間,則發布應用不需要引用程式集。 如果已`true`發佈的應用需要運行時編譯,則設置為"已發佈應用"。 例如,`true`如果應用在執行時修改 *.cshtml*檔或使用嵌入的檢視,則將該值設置為。 預設為 `false`。 |
| `IncludeRazorContentInPack` | 當`true`時,所有 Razor 內容項 *(.cshtml*檔)都標記為包含在生成的 NuGet 包中。 預設為 `false`。 |
| `EmbedRazorGenerateSources` | 如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。 預設為 `false`。 |
| `UseRazorBuildServer` | 如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 |
| `GenerateMvcApplicationPartsAssemblyAttributes` | 當`true`時,SDK 生成 MVC 在執行時用於執行應用程式部件發現的其他屬性。 |
| `DefaultWebContentItemExcludes` | 在 Web 或 Razor SDK 的專案中`Content`要從專案群組中排除的項目的 globing 模式 |
| `ExcludeConfigFilesFromBuildOutput` | 當`true` *.config*和 *.json*檔不會複製到生成輸出目錄時。 |
| `AddRazorSupportForMvc` | 當`true`時,配置 Razor SDK 以添加在建譯包含 MVC 檢視或 Razor 頁面的應用程式時所需的 MVC 配置的支援。 此屬性隱式設定為 .NET Core 3.0 或針對 Web SDK 的更高專案 |
| `RazorLangVersion` | 要定位的 Razor 語言的版本。 |

::: moniker-end

::: moniker range="< aspnetcore-3.0"

| 屬性 | 描述 |
| -------- | ----------- |
| `RazorTargetName` | Razor 所產生組件的檔案名稱 (不含副檔名)。 |
| `RazorOutputPath` | Razor 輸出目錄。 |
| `RazorCompileToolset` | 用來判斷可用來建置 Razor 組件的工具組。 有效值是 `Implicit`、`RazorSDK` 和 `PrecompilationTool`。 |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | 預設值為 `true`。 當`true`時 ,包括*Web.config* *、.json*和 *.cshtml*檔案作為專案中的內容。 當通過`Microsoft.NET.Sdk.Web`引用時 *,wwwroot*下的檔和配置檔也包括在內。 |
| `EnableDefaultRazorGenerateItems` | 如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。 |
| `GenerateRazorTargetAssemblyInfo` | 當`true`生成 *.cs*檔,`RazorAssemblyAttribute`其中包含指定的屬性,並在編譯輸出中包含該檔。 |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | 如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。 |
| `CopyRazorGenerateFilesToPublishDirectory` | 當`true``RazorGenerate`時 ,將項目 *(.cshtml*) 檔案複製到發佈目錄。 通常,如果已發佈的應用在生成時間或發佈時參與編譯,則不需要 Razor 檔。 預設為 `false`。 |
| `CopyRefAssembliesToPublishDirectory` | 如果是 `true`，請將參考組件項目複製到發行目錄。 通常,如果 Razor 編譯發生在生成時間或發佈時間,則發布應用不需要引用程式集。 如果已`true`發佈的應用需要運行時編譯,則設置為"已發佈應用"。 例如,`true`如果應用在執行時修改 *.cshtml*檔或使用嵌入的檢視,則將該值設置為。 預設為 `false`。 |
| `IncludeRazorContentInPack` | 當`true`時,所有 Razor 內容項 *(.cshtml*檔)都標記為包含在生成的 NuGet 包中。 預設為 `false`。 |
| `EmbedRazorGenerateSources` | 如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。 預設為 `false`。 |
| `UseRazorBuildServer` | 如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 |
| `GenerateMvcApplicationPartsAssemblyAttributes` | 當`true`時,SDK 生成 MVC 在執行時用於執行應用程式部件發現的其他屬性。 |
| `DefaultWebContentItemExcludes` | 在 Web 或 Razor SDK 的專案中`Content`要從專案群組中排除的項目的 globing 模式 |
| `ExcludeConfigFilesFromBuildOutput` | 當`true` *.config*和 *.json*檔不會複製到生成輸出目錄時。 |
| `AddRazorSupportForMvc` | 當`true`時,配置 Razor SDK 以添加在建譯包含 MVC 檢視或 Razor 頁面的應用程式時所需的 MVC 配置的支援。 此屬性隱式設定為 .NET Core 3.0 或針對 Web SDK 的更高專案 |
| `RazorLangVersion` | 要定位的 Razor 語言的版本。 |

::: moniker-end

有關屬性的詳細資訊,請參閱[MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。

### <a name="targets"></a>目標

Razor SDK 會定義兩個主要目標：

* `RazorGenerate`&ndash;代碼從`RazorGenerate`項目生成 *.cs*檔。 使用`RazorGenerateDependsOn`屬性指定可以在此目標之前或之後運行的其他目標。
* `RazorCompile`&ndash;將生成的 *.cs*檔編譯到 Razor 程式集中。 使用`RazorCompileDependsOn`指定可以在此目標之前或之後運行的其他目標。
* `RazorComponentGenerate`&ndash;代碼為`RazorComponent`項目生成 *.cs*檔。 使用`RazorComponentGenerateDependsOn`屬性指定可以在此目標之前或之後運行的其他目標。

### <a name="runtime-compilation-of-razor-views"></a>Razor 檢視的執行階段編譯

* 根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。 當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。 將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。

* 對於 Web 應用,請確保你的應用`Microsoft.NET.Sdk.Web`定位為 SDK。

## <a name="razor-language-version"></a>剃刀語言版本

定位`Microsoft.NET.Sdk.Web`SDK 時,Razor 語言版本將從應用的目標框架版本推斷出來。 對於針對`Microsoft.NET.Sdk.Razor`SDK 的專案,或在應用需要與推斷值不同的 Razor 語言版本的情況下,可以透過在應用`<RazorLangVersion>`的專案檔中設定 屬性來配置版本:

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Razor 的語言版本與構建的運行時版本緊密整合。 定位不是為運行時設計的語言版本不受支持,並且可能會生成錯誤。

## <a name="additional-resources"></a>其他資源

* [適用於 .NET Core 之 csproj 格式的新增項目](/dotnet/core/tools/csproj)
* [常見 MS 產生項目項目](/visualstudio/msbuild/common-msbuild-project-items)
