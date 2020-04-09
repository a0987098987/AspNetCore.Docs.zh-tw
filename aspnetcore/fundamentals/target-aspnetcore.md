---
title: 在類別庫中使用ASP.NET核心 API
author: scottaddie
description: 瞭解如何在類庫中使用ASP.NET核心 API。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
no-loc:
- Blazor
uid: fundamentals/target-aspnetcore
ms.openlocfilehash: 5374d7eec4334223a4bba7ee26cb6e2f208ed20b
ms.sourcegitcommit: f0aeeab6ab6e09db713bb9b7862c45f4d447771b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2020
ms.locfileid: "80977193"
---
# <a name="use-aspnet-core-apis-in-a-class-library"></a>在類別庫中使用ASP.NET核心 API

作者：[Scott Addie](https://github.com/scottaddie)

本文件提供有關在類庫中使用ASP.NET核心 API 的指導。 有關所有其他庫指南,請參閱[開源庫指南](/dotnet/standard/library-guidance/)。

## <a name="determine-which-aspnet-core-versions-to-support"></a>確定哪些ASP.NET核心版本要支援

ASP.NET核心遵守[.NET 核心支援策略](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。 在確定庫中要支援哪些ASP.NET核心版本時,請參閱支援策略。 庫應:

* 努力支援分類為*長期支援*(LTS) 的所有 ASP.NET 核心版本。
* 不覺得有義務支援ASP.NET核心版本歸類為*生命終止*(EOL)。

當ASP.NET核心的預覽版本可用時,重大更改將張貼在[aspnet/公告](https://github.com/aspnet/Announcements/issues)GitHub 儲存庫中。 在開發框架功能時,可以對庫進行相容性測試。

## <a name="use-the-aspnet-core-shared-framework"></a>使用ASP.NET核心共用框架

隨著 .NET Core 3.0 的發佈,許多ASP.NET核心程式集不再作為包發佈到 NuGet。 相反,程式集包含在共用框架中`Microsoft.AspNetCore.App`,該框架與 .NET Core SDK 和運行時安裝程式一起安裝。 有關不再發布的套件的清單,請參閱[刪除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。

自 .NET Core 3.0 起`Microsoft.NET.Sdk.Web`,使用 MSBuild SDK 的專案隱式引用共用框架。 使用`Microsoft.NET.Sdk`或`Microsoft.NET.Sdk.Razor`SDK 的項目必須引用 ASP.NET 核心在共用框架中使用 ASP.NET 核心 API。

要參考ASP.NET核心,請向專案`<FrameworkReference>`檔案加入以下元素:

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj?highlight=8)]

僅支援以這種方式引用ASP.NET核心。""專案的目標是 .NET Core 3.x。

## <a name="include-blazor-extensibility"></a>包括布拉佐可擴充性

布拉佐爾支援網路組裝 (WASM) 與伺服器[託管模型](xref:blazor/hosting-models)。 除非有特定原因,否則[Razor 元件](xref:blazor/components)庫應支援這兩個託管模型。 Razor 元件庫必須使用[Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk)。

### <a name="support-both-hosting-models"></a>支援兩種託管模式

要支援[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)和[Blazor WASM](xref:blazor/hosting-models#blazor-webassembly)專案的 Razor 元件消耗,請使用以下說明為您的編輯器。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

使用**Razor 類庫**專案範本。 應取消選中範本的支持**頁面和視圖**複選框。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在整合終端機中執行以下指令:

```dotnetcli
dotnet new razorclasslib
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

使用**Razor 類庫**專案範本。

---

從樣本產生的項目執行以下操作:

* 目標 .NET 標準 2.0.
* 將 `RazorLangVersion` 屬性設定為 `3.0`。 `3.0`是 .NET 核心 3.x 的預設值。
* 新增以下包引言:
  * [微軟.AspNetCore.元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
  * [微軟.AspNetCore.元件.Web](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-components-library.csproj)]

### <a name="support-a-specific-hosting-model"></a>支援特定的託管模型

支援單個 Blazor 託管模式並不常見。 例如,要僅支援[Blazor Server](xref:blazor/hosting-models#blazor-server)專案中的 Razor 元件消耗:

* 目標 .NET 核心 3.x.
* 為`<FrameworkReference>`共用框架添加元素。

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-components-library.csproj)]

有關包含 Razor 元件的函式庫的詳細資訊,請參閱[ASP.NET 核心剃刀元件類庫](xref:blazor/class-libraries)。

## <a name="include-mvc-extensibility"></a>包含 MVC 可擴充性

本節概述了對庫的建議,其中包括:

* [剃刀檢視或剃刀頁面](#razor-views-or-razor-pages)
* [標記說明器](#tag-helpers)
* [檢視元件](#view-components)

本節不討論多目標以支援多個版本的 MVC。 有關支援多個ASP.NET核心版本的指南,請參閱[支援多個ASP.NET核心版本](#support-multiple-aspnet-core-versions)。

### <a name="razor-views-or-razor-pages"></a>剃刀檢視或剃刀頁面

包含 Razor[檢視](xref:mvc/views/overview)或[Razor 頁面](xref:razor-pages/index)的項目必須使用[Microsoft.NET.Sdk.Razor SDK](xref:razor-pages/sdk)。

如果項目的目標是 .NET Core 3.x,則需要:

* 設定為`AddRazorSupportForMvc``true`的 MSBuild 屬性設定為 。
* `<FrameworkReference>`共用框架的元素。

**Razor 函式庫**專案樣本滿足針對 .NET Core 3.x 的專案的上述要求。 對編輯器使用以下說明。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

使用**Razor 類庫**專案範本。 應選擇範本的支持**頁面和視圖**複選框。

# <a name="visual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在整合終端機中執行以下指令:

```dotnetcli
dotnet new razorclasslib -s
```

# <a name="visual-studio-for-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

目前不支援專案範本。

---

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-views-pages-library.csproj)]

如果專案以 .NET 標準為目標,則需要[Microsoft.AspNetCore.Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc)包引用。 該`Microsoft.AspNetCore.Mvc`包在ASP.NET Core 3.0 中進入共用框架,因此不再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-views-pages-library.csproj?highlight=8)]

### <a name="tag-helpers"></a>標籤協助程式

包含[標記說明器](xref:mvc/views/tag-helpers/intro)的專案應`Microsoft.NET.Sdk`使用 SDK。 如果以 .NET Core`<FrameworkReference>`3.x 為目標,則為共用框架添加一個元素。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

如果定位為 .NET 標準(以支援ASP.NET Core 3.x 之前的版本),則添加對[Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的包引用。 該`Microsoft.AspNetCore.Mvc.Razor`包移動到共用框架中,因此不再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-tag-helpers-library.csproj)]

### <a name="view-components"></a>檢視元件

包含[檢視元件](xref:mvc/views/view-components)的專案應使用`Microsoft.NET.Sdk`SDK。 如果以 .NET Core`<FrameworkReference>`3.x 為目標,則為共用框架添加一個元素。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

如果針對 .NET 標準(以支援早於 ASP.NET Core 3.x 的版本),則添加指向[Microsoft.AspNetCore.Mvc.View 功能](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures)的包引用。 該`Microsoft.AspNetCore.Mvc.ViewFeatures`包移動到共用框架中,因此不再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-view-components-library.csproj)]

## <a name="support-multiple-aspnet-core-versions"></a>支援多個ASP.NET核心版本

需要多目標來編寫支援ASP.NET核心的多個變體的庫。 請考慮標記說明器庫必須支援以下ASP.NET核心變體的情況:

* ASP.NET核心 2.1 目標 .NET 框架 4.6.1
* ASP.NET核心 2.x 目標 .NET 核心 2.x
* ASP.NET核心 3.x 目標 .NET 核心 3.x

以下項目檔使用`TargetFrameworks`屬性支援這些變體:

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

使用前面的項目檔:

* 該`Markdig`包將針對所有消費者添加。
* 為面向 .NET 框架 4.6.1 或更高版本或 .NET Core 2.x 的消費者添加了對[Microsoft.AspNetCore.Mvc.Razor](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的引用。 由於向後相容性,包的版本 2.1.0 與 ASP.NET Core 2.2 相容。
* 面向 .NET Core 3.x 的消費者引用了共用框架。 該`Microsoft.AspNetCore.Mvc.Razor`包包含在共用框架中。

或者,.NET 標準 2.0 可以針對目標,而不是同時定位 .NET 核心 2.1 和 .NET 框架 4.6.1:

[!code-xml[](target-aspnetcore/samples/multi-tfm/alternative-tag-helpers-library.csproj?highlight=4)]

使用前面的項目檔,存在以下注意事項:

* 由於庫僅包含標記説明器,因此將目標ASP.NET Core 運行的特定平臺定位起來更為簡單:.NET Core 和 .NET 框架。 標記説明器不能由其他符合 .NET 標準 2.0 的目標框架(如 Unity、UWP 和 Xamarin)使用。
* 使用 .NET Framework 中的 .NET Standard 2.0，會有一些在 .NET Framework 4.7.2 中已經解決的問題。 您可以通過定位 .NET 框架 4.6.1 來改善使用 .NET 框架 4.6.1 到 4.7.1 的消費者體驗。

如果您的庫需要調用特定於平臺的 API,請針對特定的 .NET 實現而不是 .NET 標準。 有關詳細資訊,請參閱[多目標](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting)。

## <a name="use-an-api-that-hasnt-changed"></a>使用未變更的 API

設想一個方案,您將中間件庫從 .NET Core 2.2 升級到 3.0。 庫中正在使用的ASP.NET核心中間件 API 在 ASP.NET 酷 2.2 和 3.0 之間沒有變化。 要繼續支援 .NET Core 3.0 中的中間件庫,請執行以下步驟:

* 遵循[標準庫指南](/dotnet/standard/library-guidance/)。
* 如果共用框架中不存在相應的程式集,則為每個 API 的 NuGet 包添加包引用。

## <a name="use-an-api-that-changed"></a>使用已變更的 API

設想一個將庫從 .NET Core 2.2 升級到 .NET Core 3.0 的方案。 函式庫中正在使用ASP.NET核心 API 在 ASP.NET Core 3.0[中發生重大變更](/dotnet/core/compatibility/breaking-changes)。 考慮是否可以重寫庫,以便在所有版本中不使用損壞的 API。

如果可以重寫庫,請重寫庫,然後繼續使用包引用來定位較早的目標框架(例如 .NET 標準 2.0 或 .NET 框架 4.6.1)。

如果無法重寫庫,請執行以下步驟:

* 添加 .NET 核心 3.0 的目標。
* 為`<FrameworkReference>`共用框架添加元素。
* 使用帶有相應目標框架符號[#if预处理器指令](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)有條件地編譯代碼。

例如,默認情況下,從ASP.NET Core 3.0 起,HTTP 請求和回應流的同步讀取和寫入將禁用。 默認情況下,ASP.NET Core 2.2 支援同步行為。 考慮一個中間件庫,其中應啟用同步讀取和寫入,其中發生I/O。 庫應包含代碼,以便在相應的預處理器指令中啟用同步功能。 例如：

[!code-csharp[](target-aspnetcore/samples/middleware.cs?highlight=9-24)]

## <a name="use-an-api-introduced-in-30"></a>使用 3.0 中引入的 API

假設您要使用ASP.NET Core 3.0 中引入的ASP.NET核心 API。 請考慮下列問題：

1. 庫在功能上是否需要新的 API?
1. 庫能否以不同的方式實現此功能?

如果庫在功能上需要 API,並且無法在級別下實現它:

* 僅目標 .NET 核心 3.x。
* 為`<FrameworkReference>`共用框架添加元素。

如果庫可以以不同的方式實現該功能:

* 將 .NET Core 3.x 添加為目標框架。
* 為`<FrameworkReference>`共用框架添加元素。
* 使用帶有相應目標框架符號[#if预处理器指令](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)有條件地編譯代碼。

例如,以下標記説明程式使用<xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment>ASP.NET Core 3.0 中引入的介面。 面向 .NET Core 3.0`NETCOREAPP3_0`的使用者執行 目標框架符號定義的代碼路徑。 標記説明器的建構函數參數類型更改為<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>.NET Core 2.1 和 .NET 框架 4.6.1 消費者。 這一更改是必要的,因為ASP.NET Core `IHostingEnvironment` 3.0`IWebHostEnvironment`標記為過時,建議作為替換。

```csharp
[HtmlTargetElement("script", Attributes = "asp-inline")]
public class ScriptInliningTagHelper : TagHelper
{
    private readonly IFileProvider _wwwroot;

#if NETCOREAPP3_0
    public ScriptInliningTagHelper(IWebHostEnvironment env)
#else
    public ScriptInliningTagHelper(IHostingEnvironment env)
#endif
    {
        _wwwroot = env.WebRootFileProvider;
    }

    // code omitted for brevity
}
```

以下多目標項目檔案支援此標記説明程式方案:

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

## <a name="use-an-api-removed-from-the-shared-framework"></a>使用從共享框架中移除的 API

要使用從共用框架中刪除的ASP.NET核心程式集,添加相應的包引用。 有關從 ASP.NET Core 3.0 中的共用框架中移除的套件的清單,請參閱[刪除過時的包參考](xref:migration/22-to-30#remove-obsolete-package-references)。

例如,要新增 Web API 用戶端:

```xml
<Project Sdk="Microsoft.NET.Sdk">

  <PropertyGroup>
    <TargetFramework>netcoreapp3.0</TargetFramework>
  </PropertyGroup>

  <ItemGroup>
    <FrameworkReference Include="Microsoft.AspNetCore.App" />
  </ItemGroup>

  <ItemGroup>
    <PackageReference Include="Microsoft.AspNet.WebApi.Client" Version="5.2.7" />
  </ItemGroup>

</Project>
```

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/ui-class>
* <xref:blazor/class-libraries>
* [.NET 實作支援](/dotnet/standard/net-standard#net-implementation-support)
* [.NET 支援原則](https://dotnet.microsoft.com/platform/support/policy)
