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
ms.openlocfilehash: acc049a69574968d1e304d6c504cb89243387d6c
ms.sourcegitcommit: 477d38e33530a305405eaf19faa29c6d805273aa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/08/2018
---
# <a name="aspnet-core-razor-sdk"></a>ASP.NET Core Razor SDK

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[!INCLUDE[](~/includes/2.1-SDK.md)] 包含 `Microsoft.NET.Sdk.Razor` MSBuild SDK (Razor SDK)。 Razor SDK：

* 標準化關於建置、封裝和發行專案 (包含 ASP.NET Core MVC 架構專案的 [Razor](xref:mvc/views/razor) 檔案) 的體驗。
* 包含一組預先定義的目標、屬性和項目，可讓您自訂 Razor 檔案的編譯作業。

## <a name="prerequisites"></a>必要條件

[!INCLUDE[](~/includes/2.1-SDK.md)]

## <a name="using-the-razor-sdk"></a>使用 Razor SDK

大部分的 Web 應用程式不需要明確參考 Razor SDK。 

若要使用 Razor SDK 來建置包含 Razor 檢視或 Razor 頁面的類別庫：

* 使用 `Microsoft.NET.Sdk.Razor` 來取代 `Microsoft.NET.Sdk`：
```xml
<Project SDK="Microsoft.NET.Sdk.Razor">
  ...
</Project>
```

* 通常需要有 `Microsoft.AspNetCore.Mvc` 的套件參考，才能納入建置和編譯 Razor 頁面與 Razor 檢視所需的其他相依性。 您的專案最少需要將套件參考新增至：

    * `Microsoft.AspNetCore.Razor.Design` 
    * `Microsoft.AspNetCore.Mvc.Razor.Extensions`
    
 `Microsoft.AspNetCore.Mvc` 中包含上述套件。 下列標記會顯示基本 *.csproj* 檔案，該檔案使用 Razor SDK 來建置 ASP.NET Core Razor 頁面應用程式的 Razor 檔案：
    
 [!code-xml[Main](sdk/sample/RazorSDK.csproj)]

### <a name="properties"></a>屬性

下列屬性可在專案建置期間控制 Razor 的 SDK 行為：

* `RazorCompileOnBuild`：如果是 `true`，請在建置專案期間編譯和發出 Razor 組件。 預設值為 `true`。
* `RazorCompileOnPublish`：如果是 `true`，請在發行專案期間編譯和發出 Razor 組件。 預設值為 `true`。

下列的屬性和項目可用來設定 Razor SDK 的輸入和輸出：

| 項目                                         | 描述                                                                   |
| ------------                                  | -------------                                                                 |
| RazorGenerate                                 | 其為程式碼產生目標輸入的項目元素 (*.cshtml* 檔案)。 |
| RazorCompile                                  | 其為 Razor 編譯目標輸入的項目元素 (.cs 檔案)。 請使用此 ItemGroup 來指定要編譯成 Razor 組件的其他檔案。 |
| RazorTargetAssemblyAttribute                  | 用於 Razor 組件的程式碼產生屬性的項目元素。 例如:   <br />`<RazorAssemblyAttribute ` <br />  `Include="System.Reflection.AssemblyMetadataAttribute"`<br />`  _Parameter1="BuildSource" _Parameter2="https://docs.asp.net/">` |
| RazorEmbeddedResource                         | 新增為所產生 Razor 組件之內嵌資源的項目元素 |

| 屬性                                      | 描述                                                                   |
| ------------                                  | -------------                                                                 |
| RazorTargetName                               | Razor 所產生組件的檔案名稱 (不含副檔名)。 | 
| RazorOutputPath                               | Razor 輸出目錄。                                      |
| RazorCompileToolset                           | 用來判斷可用來建置 Razor 組件的工具組。 有效值為 `Implicit` 和 `PrecompilationTool`。 |
| EnableDefaultContentItems                     | 如果是 `true`，請包括特定檔案類型 (例如 *.cshtml* 檔案)，作為專案中的內容。 透過 Microsoft.NET.Sdk.Web 參考時，也包含 *wwwroot* 下的所有檔案，以及組態檔。         |
| EnableDefaultRazorGenerateItems               | 如果是 `true`，請包括來自 `RazorGenerate` 項目之 `Content` 項目的 *.cshtml* 檔案。 |
| GenerateRazorTargetAssemblyInfo               | 如果是 `true`，請產生包含 `RazorAssemblyAttribute` 所指定屬性的 *.cs* 檔案，並將其包含在編譯輸出中。 |
| EnableDefaultRazorTargetAssemblyInfoAttributes | 如果是 `true`，請將一組預設的組件屬性新增至 `RazorAssemblyAttribute`。 |
| CopyRazorGenerateFilesToPublishDirectory       | 如果是 `true`，請將 RazorGenerate 項目 (*.cshtml*) 檔案複製到發行目錄。 如果已發行的應用程式在建置階段或發行階段參與編譯，通常不需要 Razor 檔案。 預設值為 `false`。 |
| CopyRefAssembliesToPublishDirectory            | 如果是 `true`，請將參考組件項目複製到發行目錄。 如果在建置階段或發行階段進行 Razor 編譯，已發行的應用程式通常不需要參考組件。 如果已發行的應用程式需要執行階段編譯，例如在執行階段修改 cshtml 檔案或是使用內嵌檢視，請設定為 `true`。 預設值為 `false`。 |
| IncludeRazorContentInPack                      | 如果是 `true`，所有 Razor 內容項目 (*.cshtml* 檔案) 將會標示為要包含在產生的 NuGet 套件中。 預設值為 `false`。 |
| EmbedRazorGenerateSources | 如果是 `true`，請將 RazorGenerate (*.cshtml*) 項目當作內嵌檔案新增至產生的 Razor 組件。 預設值為 `false`。 |
| UseRazorBuildServer                           | 如果是 `true`，請使用持續性組建伺服器處理序來卸載程式碼產生工作。 預設值為 `UseSharedCompilation`。 |

### <a name="targets"></a>目標
Razor SDK 會定義兩個主要目標：

* `RazorGenerate` - 程式碼會從 RazorGenerate 項目元素產生 *.cs*。 請使用 `RazorGenerateDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。
* `RazorCompile` - 將產生的 *.cs* 檔案編譯成 Razor 組件。 請使用 `RazorCompileDependsOn` 屬性來指定可在此目標之前或之後執行的其他目標。
