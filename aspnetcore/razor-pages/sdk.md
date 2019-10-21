---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 08/23/2019
uid: razor-pages/sdk
ms.openlocfilehash: 606d2bdca3fa4fb1c81df73ac697d2175c3ab633
ms.sourcegitcommit: 07d98ada57f2a5f6d809d44bdad7a15013109549
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 10/15/2019
ms.locfileid: "72334029"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

## <a name="overview"></a>總覽

@No__t_0 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK （Razor SDK）。 Razor SDK：

::: moniker range=">= aspnetcore-3.0"

* 需要建立、封裝和發行包含[Razor](xref:mvc/views/razor)檔案的專案，以用於 ASP.NET Core MVC 或[Blazor](xref:blazor/index)專案。
* 包含一組預先定義的目標、屬性和專案，可讓您自訂 Razor （*cshtml*或*Razor*）檔案的編譯。

Razor SDK 包括 `Include` 屬性設為 `**\*.cshtml` 和 `**\*.razor` 萬用字元模式的 `Content` 專案。 已發行相符的檔案。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

* 標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。
* 包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。

Razor SDK 包含 `Content` 專案，其中 `Include` 屬性設定為 `**\*.cshtml` 的萬用字元模式。 已發行相符的檔案。

::: moniker-end

## <a name="prerequisites"></a>Prerequisites

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="use-the-razor-sdk"></a>使用 Razor SDK

大部分的 web 應用程式都不需要明確參考 Razor SDK。

::: moniker range=">= aspnetcore-3.0"

若要使用 Razor SDK 來建立包含 Razor views 或 Razor Pages 的類別庫，建議您從 Razor 類別庫（RCL）專案範本開始。 用來建立 Blazor （*razor*）檔案的 RCL，最少需要[AspNetCore 元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)套件的參考。 用來建立 Razor views 或 pages （*cshtml*檔案）的 RCL，至少需要以 `netcoreapp3.0` 或更新版本為目標，並且在其專案檔中具有[AspNetCore 中繼套件](xref:fundamentals/metapackage-app)的 `FrameworkReference`。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：

* 使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    <!-- omitted for brevity -->
  </Project>
  ```

* 通常，需要 `Microsoft.AspNetCore.Mvc` 的套件參考，才能收到建立和編譯 Razor Pages 和 Razor views 所需的其他相依性。 您的專案至少應該將套件參考新增至：

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
  * `Microsoft.AspNetCore.Mvc.Razor`
    
  @No__t_0 套件會提供專案的 Razor 編譯工作和目標。

  `Microsoft.AspNetCore.Mvc` 中包含上述套件。 下列標記顯示的專案檔會使用 Razor SDK 來建立 ASP.NET Core Razor Pages 應用程式的 Razor 檔案：
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]
  
::: moniker-end

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> @No__t_0 和 `Microsoft.AspNetCore.Mvc.Razor.Extensions` 套件都包含在[AspNetCore 應用程式中繼套件](xref:fundamentals/metapackage-app)中。 不過，無版本的 `Microsoft.AspNetCore.App` 套件參考會提供應用程式的中繼套件，其中不包含最新版本的 `Microsoft.AspNetCore.Razor.Design`。 專案必須參考一致版本的 `Microsoft.AspNetCore.Razor.Design` （或 `Microsoft.AspNetCore.Mvc`），才會包含 Razor 的最新組建時間修正程式。 如需詳細資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。

::: moniker-end

### <a name="properties"></a>內容

下列屬性可在專案建置期間控制 Razor 的 SDK 行為：

* `RazorCompileOnBuild` &ndash; `true` 時，會編譯併發出 Razor 元件，做為建立專案的一部分。 預設值為 `true`。
* 當 `true` 時，`RazorCompileOnPublish` &ndash;，會在發行專案時編譯和發出 Razor 元件。 預設值為 `true`。

下表中的屬性和專案可用來設定 Razor SDK 的輸入和輸出。

::: moniker range=">= aspnetcore-3.0"

> [!WARNING]
> 從 ASP.NET Core 3.0 開始，如果專案檔中的 `RazorCompileOnBuild` 或 `RazorCompileOnPublish` MSBuild 屬性已停用，則預設不會提供 MVC Views 或 Razor Pages。 如果應用程式依賴執行時間編譯來處理*cshtml*檔案，則應用程式必須新增[microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation)封裝的明確參考。

::: moniker-end

| 項目 | 描述 |
| ----- | ----------- |
| `RazorGenerate` | 屬於程式碼產生之輸入的專案元素（ *. cshtml*檔案）。 |
| `RazorComponent` | 做為 Razor 元件程式碼產生輸入的專案元素（*razor*檔案）。 |
| `RazorCompile` | 做為 Razor 編譯目標輸入的專案元素（ *.cs*檔案）。 使用此 `ItemGroup` 來指定要編譯成 Razor 元件的其他檔案。 |
| `RazorTargetAssemblyAttribute` | 用於 Razor 組件的程式碼產生屬性的項目元素。 例如:  <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | 做為內嵌資源加入至產生的 Razor 元件的專案元素。 |

| 屬性 | 描述 |
| -------- | ----------- |
| `RazorTargetName` | Razor 所產生組件的檔案名稱 (不含副檔名)。 | 
| `RazorOutputPath` | Razor 輸出目錄。 |
| `RazorCompileToolset` | 用來判斷可用來建置 Razor 組件的工具組。 有效值為 `Implicit`、`RazorSDK` 及 `PrecompilationTool`。 |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | 預設為 `true`。 當 `true`*時，會包含 web.config*、 *. json*和*cshtml*檔案做為專案中的內容。 透過 `Microsoft.NET.Sdk.Web` 來參考時，也會包含*wwwroot*和 config 檔案底下的檔案。 |
| `EnableDefaultRazorGenerateItems` | 如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。 |
| `GenerateRazorTargetAssemblyInfo` | 當 `true` 時，會產生 *.cs*檔案，其中包含 `RazorAssemblyAttribute` 指定的屬性，並在編譯輸出中包含檔案。 |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | 如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。 |
| `CopyRazorGenerateFilesToPublishDirectory` | 當 `true` 時，會將 `RazorGenerate` 專案（*cshtml*）檔案複製到發行目錄。 通常，如果已發行的應用程式在組建時間或發行時間參與編譯，則不需要 Razor 檔案。 預設值為 `false`。 |
| `CopyRefAssembliesToPublishDirectory` | 如果是 `true`，請將參考組件項目複製到發行目錄。 一般來說，如果在組建時間或發行時間發生 Razor 編譯，則發行的應用程式不需要參考元件。 如果您已發佈的應用程式需要執行時間編譯，請設定為 `true`。 例如，如果應用程式在執行時間修改 cshtml 檔案，或使用內嵌的視圖，請將值設定為 `true` *。* 預設值為 `false`。 |
| `IncludeRazorContentInPack` | 當 `true` 時，所有 Razor 內容專案（*cshtml*檔案）都會標示為包含在產生的 NuGet 套件中。 預設值為 `false`。 |
| `EmbedRazorGenerateSources` | 如果是 `true`，請將 RazorGenerate ( *.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。 預設值為 `false`。 |
| `UseRazorBuildServer` | 如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 |
| `GenerateMvcApplicationPartsAssemblyAttributes` | 當 `true` 時，SDK 會在執行時間產生 MVC 用來執行應用程式元件探索的其他屬性。 |

如需有關屬性的詳細資訊，請參閱 [MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。

### <a name="targets"></a>目標

Razor SDK 會定義兩個主要目標：

* `RazorGenerate` &ndash; 程式碼會從 `RazorGenerate` 專案元素產生 *.cs*檔案。 使用 [`RazorGenerateDependsOn`] 屬性來指定可在此目標之前或之後執行的其他目標。
* `RazorCompile` &ndash; 會將產生的 *.cs*檔案編譯到 Razor 元件中。 使用 [`RazorCompileDependsOn`] 指定可在此目標之前或之後執行的其他目標。
* `RazorComponentGenerate` &ndash; 程式碼會為 `RazorComponent` 專案元素產生 *.cs*檔案。 使用 [`RazorComponentGenerateDependsOn`] 屬性來指定可在此目標之前或之後執行的其他目標。

### <a name="runtime-compilation-of-razor-views"></a>Razor 檢視的執行階段編譯

* 根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。 當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。 將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。

* 針對 web 應用程式，請確定您的應用程式是以 `Microsoft.NET.Sdk.Web` SDK 為目標。

## <a name="razor-language-version"></a>Razor 語言版本

以 `Microsoft.NET.Sdk.Web` SDK 為目標時，會從應用程式的目標 framework 版本推斷 Razor 語言版本。 針對以 `Microsoft.NET.Sdk.Razor` SDK 為目標的專案，或在罕見的情況下，應用程式需要比推斷值不同的 Razor 語言版本，您可以在應用程式的專案檔中設定 `<RazorLangVersion>` 屬性來設定版本：

```xml
<PropertyGroup>
  <RazorLangVersion>{VERSION}</RazorLangVersion>
</PropertyGroup>
```

Razor 的語言版本與所建立之執行時間的版本緊密整合。 不支援以不是針對執行時間設計的語言版本為目標，而且可能會產生組建錯誤。

## <a name="additional-resources"></a>其他資源

* [適用於 .NET Core 之 csproj 格式的新增項目](/dotnet/core/tools/csproj)
* [一般 MSBuild 專案項目](/visualstudio/msbuild/common-msbuild-project-items)
