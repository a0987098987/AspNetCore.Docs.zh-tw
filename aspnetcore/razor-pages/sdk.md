---
title: ASP.NET Core Razor SDK
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.1'
ms.author: riande
ms.custom: mvc, seodec18
ms.date: 10/25/2018
uid: razor-pages/sdk
ms.openlocfilehash: 0e6cfeb1863ed14ffe670cf082e99f28b26718dd
ms.sourcegitcommit: ca5f03210bedc61c6639a734ae5674bfe095dee8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/26/2019
ms.locfileid: "55073097"
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)]包含`Microsoft.NET.Sdk.Razor`MSBuild SDK (Razor SDK)。 Razor SDK：

* 標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。
* 包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>使用 Razor SDK

大部分的 web 應用程式不需要明確參考 Razor SDK。

若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：

* 使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：

  ```xml
  <Project SDK="Microsoft.NET.Sdk.Razor">
    ...
  </Project>
  ```

* 一般而言，套件參考至`Microsoft.AspNetCore.Mvc`，才能接收來建置和編譯 Razor 頁面與 Razor 檢視所需的其他相依性。 最少，您的專案應該新增套件參考：

  * `Microsoft.AspNetCore.Razor.Design` 
  * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
  `Microsoft.AspNetCore.Razor.Design`套件提供 Razor 編譯工作和目標專案。

  `Microsoft.AspNetCore.Mvc` 中包含上述套件。 下列標記會顯示使用 Razor SDK 來建置 ASP.NET Core Razor 頁面應用程式的 Razor 檔案的專案檔：
    
  [!code-xml[](sdk/sample/RazorSDK.csproj)]

::: moniker range="= aspnetcore-2.1"

> [!WARNING]
> `Microsoft.AspNetCore.Razor.Design`並`Microsoft.AspNetCore.Mvc.Razor.Extensions`套件中包含[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)。 不過，版本小於`Microsoft.AspNetCore.App`封裝參考文件提供的中繼套件，且不包含最新版本的應用程式`Microsoft.AspNetCore.Razor.Design`。 專案必須參考的版本一致性`Microsoft.AspNetCore.Razor.Design`(或`Microsoft.AspNetCore.Mvc`)，以便包含最新的建置時間修正 razor。 如需詳細資訊，請參閱 <<c0> [ 此 GitHub 問題](https://github.com/aspnet/Razor/issues/2553)。

::: moniker-end

### <a name="properties"></a>屬性

下列屬性可在專案建置期間控制 Razor 的 SDK 行為：

* `RazorCompileOnBuild` &ndash; 當`true`、 編譯和建置專案時的 Razor 組件就會發出。 預設值為 `true`。
* `RazorCompileOnPublish` &ndash; 當`true`、 編譯，並會發出 Razor 組件，在發行專案。 預設值為 `true`。

下表中的項目與屬性用來設定輸入和輸出至 Razor SDK。

| 項目 | 描述 |
| ----- | ----------- |
| `RazorGenerate` | 其為程式碼產生目標輸入的項目元素 (*.cshtml* 檔案)。 |
| `RazorCompile` | Item 項目 (*.cs*檔案)，是 Razor 編譯目標的輸入。 請使用此 ItemGroup 來指定要編譯成 Razor 組件的其他檔案。 |
| `RazorTargetAssemblyAttribute` | 用於 Razor 組件的程式碼產生屬性的項目元素。 例如:   <br>`RazorAssemblyAttribute`<br>`Include="System.Reflection.AssemblyMetadataAttribute"`<br>`_Parameter1="BuildSource" _Parameter2="https://docs.microsoft.com/">` |
| `RazorEmbeddedResource` | 加入做為內嵌資源產生的 Razor 組件的項目。 |

| 屬性 | 描述 |
| -------- | ----------- |
| `RazorTargetName` | Razor 所產生組件的檔案名稱 (不含副檔名)。 | 
| `RazorOutputPath` | Razor 輸出目錄。 |
| `RazorCompileToolset` | 用來判斷可用來建置 Razor 組件的工具組。 有效值為 `Implicit`、`RazorSDK` 及 `PrecompilationTool`。 |
| [EnableDefaultContentItems](https://github.com/aspnet/websdk/blob/rel-2.0.0/src/ProjectSystem/Microsoft.NET.Sdk.Web.ProjectSystem.Targets/netstandard1.0/Microsoft.NET.Sdk.Web.ProjectSystem.targets#L21) | 預設為 `true`。 當`true`，包括*web.config*， *.json*，和 *.cshtml*檔案做為專案中的內容。 透過參考時`Microsoft.NET.Sdk.Web`，檔案底下*wwwroot*和組態檔也會包含在內。 |
| `EnableDefaultRazorGenerateItems` | 如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。 |
| `GenerateRazorTargetAssemblyInfo` | 當`true`，會產生 *.cs*檔案，其中包含指定屬性`RazorAssemblyAttribute`和包含編譯輸出中的檔案。 |
| `EnableDefaultRazorTargetAssemblyInfoAttributes` | 如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。 |
| `CopyRazorGenerateFilesToPublishDirectory` | 當`true`，複製`RazorGenerate`項目 (*.cshtml*) 至發行目錄的檔案。 一般而言，Razor 檔案不需要針對已發佈的應用程式，如果它們參與在建置階段 」 或 「 發行階段的編譯。 預設值為 `false`。 |
| `CopyRefAssembliesToPublishDirectory` | 如果是 `true`，請將參考組件項目複製到發行目錄。 一般而言，參考組件不需要的已發佈的應用程式，如果 Razor 編譯，就會發生在建置階段或發行時間。 若要設定`true`如果您已發佈的應用程式需要執行階段編譯。 例如，將值設定為`true`如果應用程式修改 *.cshtml*檔案在執行階段，或使用內嵌的檢視。 預設值為 `false`。 |
| `IncludeRazorContentInPack` | 當`true`，所有 Razor 內容項目 (*.cshtml*檔案) 會標示為要包含在產生的 NuGet 套件。 預設值為 `false`。 |
| `EmbedRazorGenerateSources` | 如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。 預設值為 `false`。 |
| `UseRazorBuildServer` | 如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 |

如需有關屬性的詳細資訊，請參閱 [MSBuild 屬性](/visualstudio/msbuild/msbuild-properties)。

### <a name="targets"></a>目標

Razor SDK 會定義兩個主要目標：

* `RazorGenerate` &ndash; 程式碼會產生 *.cs*從檔案`RazorGenerate`item 項目。 請使用 `RazorGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。
* `RazorCompile` &ndash; 編譯會產生 *.cs* Razor 組件中的檔案。 請使用 `RazorCompileDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。

### <a name="runtime-compilation-of-razor-views"></a>Razor 檢視的執行階段編譯

* 根據預設，Razor SDK 不會發佈執行執行階段編譯所需的參考組件。 當應用程式模型依賴執行階段編譯時，這會導致編譯失敗&mdash;例如，發佈應用程式之後，應用程式使用內嵌的檢視或變更檢視。 將 `CopyRefAssembliesToPublishDirectory` 設為 `true` 以繼續發佈參考組件。

* Web 應用程式，請確定您的應用程式的目標`Microsoft.NET.Sdk.Web`SDK。
