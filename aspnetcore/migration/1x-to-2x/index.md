---
title: "從 ASP.NET Core 1.x 移轉至 2.0"
author: scottaddie
description: "本文概述將 ASP.NET Core 1.x 專案移轉至 ASP.NET Core 2.0 的必要條件和最常見步驟。"
keywords: "ASP.NET Core,移轉"
ms.author: scaddie
manager: wpickett
ms.date: 08/01/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: migration/1x-to-2x/index
ms.openlocfilehash: c14e7d61e8b353c18fc4a4f2bf3658069982bad5
ms.sourcegitcommit: e832a9b9f41a8b26a8c88edfd8fc35b8bfd97d5d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/18/2017
---
# <a name="migrating-from-aspnet-core-1x-to-aspnet-core-20"></a>從 ASP.NET Core 1.x 移轉至 ASP.NET Core 2.0

作者：[Scott Addie](https://github.com/scottaddie)

在本文中，我們將引導您將現有的 ASP.NET Core 1.x 專案更新至 ASP.NET Core 2.0。 將應用程式移轉至 ASP.NET Core 2.0 可讓您充分利用[許多新功能和效能改進](https://go.microsoft.com/fwlink/?linkid=854094)。 

現有的 ASP.NET Core 1.x 應用程式是根據特定版本的專案範本。 隨著 ASP.NET Core 架構發展，其中內含的專案範本和起始程式碼也跟著演進。 除了更新 ASP.NET Core 架構之外，您還需要更新應用程式的程式碼。

<a name="prerequisites"></a>

## <a name="prerequisites"></a>必要條件
請參閱 [ASP.NET Core 使用者入門](xref:getting-started)。

<a name="tfm"></a>

## <a name="update-target-framework-moniker-tfm"></a>更新 Target Framework Moniker (TFM)
以 .NET Core 為目標的專案應該使用版本大於或等於 .NET Core 2.0 的 [TFM](/dotnet/standard/frameworks#referring-to-frameworks)。 在 *.csproj* 檔案中搜尋 `<TargetFramework>` 節點，並以 `netcoreapp2.0` 取代其內部文字：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=3)]

以 .NET Framework 為目標的專案應該使用版本大於或等於 .NET Framework 4.6.1 的 TFM。 在 *.csproj* 檔案中搜尋 `<TargetFramework>` 節點，並以 `net461` 取代其內部文字：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=4)]

> [!NOTE]
> .NET Core 2.0 提供了比 .NET Core 1.x 更大的介面區。 如果您只是因為 .NET Core 1.x 中遺漏了 API 而以 .NET Framework 為目標，以 .NET Core 2.0 為目標可能有效。

<a name="global-json"></a>

## <a name="update-net-core-sdk-version-in-globaljson"></a>更新 global.json 中的 .NET Core SDK 版本
如果您的方案依賴 [*global.json* ](https://docs.microsoft.com/dotnet/core/tools/global-json) 檔案而以特定的 .NET Core SDK 版本為目標，請更新其 `version` 屬性，以使用您電腦上安裝的 2.0 版：

[!code-json[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/global.json?highlight=3)]

<a name="package-reference"></a>

## <a name="update-package-references"></a>更新套件參考
1.x 專案中的 *.csproj* 檔案列出專案所使用的每個 NuGet 套件。

在以 .NET Core 2.0 為目標的 ASP.NET Core 2.0 專案中，*.csproj* 檔案中的單一[中繼套件](xref:fundamentals/metapackage) metapackage 參考會取代套件的集合：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=9-11)]

中繼套件包含 ASP.NET Core 2.0 和 Entity Framework Core 2.0 的所有功能。

以 .NET Framework 為目標時，ASP.NET Core 2.0 專案應該繼續參考個別的 NuGet 套件。 將每個 `<PackageReference />` 節點的 `Version` 屬性更新為 2.0.0。

例如，以下是以 .NET Framework 為目標的典型 ASP.NET Core 2.0 專案所使用的 `<PackageReference />` 節點清單：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=9-22)]

<a name="dot-net-cli-tool-reference"></a>

## <a name="update-net-core-cli-tools"></a>更新 .NET Core CLI 工具
在 *.csproj* 檔案中，將每個 `<DotNetCliToolReference />` 節點的 `Version` 屬性更新為 2.0.0。

例如，以下是以 .NET Core 2.0 為目標的典型 ASP.NET Core 2.0 專案所使用的 CLI 工具清單：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=13-17)]

<a name="package-target-fallback"></a>

## <a name="rename-package-target-fallback-property"></a>重新命名套件目標後援屬性
1.x 專案的 *.csproj* 檔案使用 `PackageTargetFallback` 節點和變數：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=5)]

將節點與變數都重新命名為 `AssetTargetFallback`：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App.csproj?range=5)]

<a name="program-cs"></a>

## <a name="update-main-method-in-programcs"></a>更新 Program.cs 中的 Main 方法
在 1.x 專案中，*Program.cs*的 `Main` 方法看起來像這樣：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCs&highlight=8-19)]

在 2.0 專案中，*Program.cs*的 `Main` 方法已經過簡化：

[!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore2.0App/AspNetCoreDotNetCore2.0App/Program.cs?highlight=8-11)]

強烈建議您採用這個新的 2.0 模式，如 [Entity Framework Core 移轉](xref:data/ef-mvc/migrations)之類的產品功能需要這個模式才能運作。 例如，從 [套件管理員主控台] 視窗執行 `Update-Database` 或從命令列 (在轉換成 ASP.NET Core 2.0 的專案中) 執行 `dotnet ef database update` 會產生下列錯誤：

```
Unable to create an object of type '<Context>'. Add an implementation of 'IDesignTimeDbContextFactory<Context>' to the project, or see https://go.microsoft.com/fwlink/?linkid=851728 for additional patterns supported at design time.
```

<a name="view-compilation"></a>

## <a name="review-your-razor-view-compilation-setting"></a>檢閱 Razor 檢視編譯設定
更快速的應用程式啟動時間和較小的發行組合對您而言極為重要。 基於這些原因，預設會在 ASP.NET Core 2.0 中啟用 [Razor 檢視編譯](xref:mvc/views/view-compilation)。

已不再需要將 `MvcRazorCompileOnPublish` 屬性設定為 true。 除非您停用檢視編譯，否則屬性可能會從 *.csproj* 檔案中移除。

以 .NET Framework 為目標時，仍然需要在 *.csproj* 檔案中明確參考 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation) NuGet 套件：

[!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App/AspNetCoreDotNetFx2.0App.csproj?range=15)]

<a name="app-insights"></a>

## <a name="rely-on-application-insights-light-up-features"></a>依賴 Application Insights 的「啟動」功能
輕鬆安裝應用程式效能檢測很重要。 您現在可以依賴 Visual Studio 2017 工具中提供的 [Application Insights](https://docs.microsoft.com/azure/application-insights/app-insights-overview)「啟動」功能。

根據預設，在 Visual Studio 2017 中建立的 ASP.NET Core 1.1 專案已新增 Application Insights。 如果您不要直接使用 Application Insights SDK，請在 *Program.cs* 和 *Startup.cs* 之外，遵循下列步驟：

1. 從 *.csproj* 檔案中移除下列 `<PackageReference />` 節點：
    
    [!code-xml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App.csproj?range=10)]

2. 從 *Program.cs* 中移除 `UseApplicationInsights` 擴充方法引動過程：

    [!code-csharp[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Program.cs?name=snippet_ProgramCsMain&highlight=8)]

3. 從 *_Layout.cshtml* 中移除 Application Insights 用戶端 API 呼叫。 它包含下列兩行程式碼：

    [!code-cshtml[Main](../1x-to-2x/samples/AspNetCoreDotNetCore1.1App/AspNetCoreDotNetCore1.1App/Views/Shared/_Layout.cshtml?range=1,19)]

如果您要直接使用 Application Insights SDK，請繼續執行這項操作。 2.0 [中繼套件](xref:fundamentals/metapackage)含有最新版本的 Application Insights，因此如果您參考的是較舊的版本，就會出現套件降級錯誤。

<a name="auth-and-identity"></a>

## <a name="adopt-authentication--identity-improvements"></a>採用驗證/身分識別的增強功能
ASP.NET Core 2.0 具有新的驗證模型和對 ASP.NET Core 身分識別的一些重大變更。 如果您在啟用個別使用者帳戶的情況下建立專案，或如果已手動新增驗證或身分識別，請參閱[將驗證和身分識別移轉至 ASP.NET Core 2.0](xref:migration/1x-to-2x/identity-2x)。

## <a name="additional-resources"></a>其他資源
- [ASP.NET Core 2.0 的重大變更](https://github.com/aspnet/announcements/issues?page=1&q=is%3Aissue+is%3Aopen+label%3A2.0.0+label%3A%22Breaking+change%22&utf8=%E2%9C%93)
