---
title: 在類別庫中使用 ASP.NET Core Api
author: scottaddie
description: 瞭解如何在類別庫中使用 ASP.NET Core Api。
ms.author: scaddie
ms.custom: mvc
ms.date: 12/16/2019
no-loc:
- Blazor
uid: fundamentals/target-aspnetcore
ms.openlocfilehash: 72096fc2f03033dfe8325b5129e074913a2fbd1f
ms.sourcegitcommit: 2cb857f0de774df421e35289662ba92cfe56ffd1
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/25/2019
ms.locfileid: "75463885"
---
# <a name="use-aspnet-core-apis-in-a-class-library"></a>在類別庫中使用 ASP.NET Core Api

作者：[Scott Addie](https://github.com/scottaddie)

本檔提供在類別庫中使用 ASP.NET Core Api 的指引。 如需所有其他程式庫指引，請參閱[開放原始碼程式庫指引](/dotnet/standard/library-guidance/)。

## <a name="determine-which-aspnet-core-versions-to-support"></a>判斷支援的 ASP.NET Core 版本

ASP.NET Core 遵守[.Net Core 支援原則](https://dotnet.microsoft.com/platform/support/policy/dotnet-core)。 判斷要在程式庫中支援的 ASP.NET Core 版本時，請參閱支援原則。 程式庫應該：

* 請致力於支援分類為*長期支援*（LTS）的所有 ASP.NET Core 版本。
* 不是為了支援分類為*生命週期結束*（EOL）的 ASP.NET Core 版本而感到義務。

當 ASP.NET Core 的預覽版本可供使用時，會在[aspnet/公告](https://github.com/aspnet/Announcements/issues)GitHub 存放庫中公佈重大變更。 在開發架構功能時，可以執行程式庫的相容性測試。

## <a name="use-the-aspnet-core-shared-framework"></a>使用 ASP.NET Core 共用架構

隨著 .NET Core 3.0 的發行，許多 ASP.NET Core 元件不再發佈至 NuGet 作為套件。 相反地，元件會包含在與 .NET Core SDK 和執行時間安裝程式一起安裝的 `Microsoft.AspNetCore.App` 共用架構中。 如需不再發佈的封裝清單，請參閱[移除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。

從 .NET Core 3.0，使用 `Microsoft.NET.Sdk.Web` MSBuild SDK 的專案會隱含地參考共用架構。 使用 `Microsoft.NET.Sdk` 或 `Microsoft.NET.Sdk.Razor` SDK 的專案必須參考 ASP.NET Core，才能在共用架構中使用 ASP.NET Core Api。

若要參考 ASP.NET Core，請將下列 `<FrameworkReference>` 元素新增至您的專案檔：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj?highlight=8)]

只有以 .NET Core 3.x 為目標的專案才支援以這種方式參考 ASP.NET Core。

## <a name="include-opno-locblazor-extensibility"></a>包含 Blazor 擴充性

Blazor 支援 WebAssembly （WASM）和伺服器[裝載模型](xref:blazor/hosting-models)。 除非有特定的理由不這麼做，否則[Razor 元件](xref:blazor/components)程式庫應該支援這兩種裝載模型。 Razor 元件程式庫必須使用[NET.TCP sdk](xref:razor-pages/sdk)。

### <a name="support-both-hosting-models"></a>同時支援這兩種裝載模型

若要同時支援[Blazor 伺服器](xref:blazor/hosting-models#blazor-server)和[Blazor WASM](xref:blazor/hosting-models#blazor-webassembly)專案的 Razor 元件耗用量，請針對您的編輯器使用下列指示。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

使用 [ **Razor 類別庫**] 專案範本。 應取消選取範本的 [**支援頁面和流覽**器] 核取方塊。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在整合式終端機中執行下列命令：

```dotnetcli
dotnet new razorclasslib
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

使用 [ **Razor 類別庫**] 專案範本。

---

從範本產生的專案會執行下列動作：

* 目標 .NET Standard 2.0。
* 將 `RazorLangVersion` 屬性設定為 `3.0`。 `3.0` 是 .NET Core 3.x 的預設值。
* 新增下列套件參考：
  * [AspNetCore 元件](https://www.nuget.org/packages/Microsoft.AspNetCore.Components)
  * [AspNetCore。 Web](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.Web)

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-components-library.csproj)]

### <a name="support-a-specific-hosting-model"></a>支援特定的主控模型

支援單一 Blazor 裝載模型，這種情況較不常見。 例如，為了僅支援來自[Blazor Server](xref:blazor/hosting-models#blazor-server)專案的 Razor 元件耗用量：

* 以 .NET Core 3.x 為目標。
* 新增共用架構的 `<FrameworkReference>` 元素。

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-components-library.csproj)]

如需包含 Razor 元件之程式庫的詳細資訊，請參閱[ASP.NET Core Razor 元件類別庫](xref:blazor/class-libraries)。

## <a name="include-mvc-extensibility"></a>包含 MVC 擴充性

本節概述程式庫的建議，包括：

* [Razor views 或 Razor Pages](#razor-views-or-razor-pages)
* [標記協助程式](#tag-helpers)
* [檢視元件](#view-components)

本節不會討論多個目標，以支援多個版本的 MVC。 如需支援多個 ASP.NET Core 版本的指引，請參閱[支援多個 ASP.NET Core 版本](#support-multiple-aspnet-core-versions)。

### <a name="razor-views-or-razor-pages"></a>Razor views 或 Razor Pages

包含[Razor views](xref:mvc/views/overview)或[Razor Pages](xref:razor-pages/index)的專案必須使用[Microsoft. net.tcp sdk](xref:razor-pages/sdk)。

如果專案是以 .NET Core 3.x 為目標，則需要：

* `AddRazorSupportForMvc` MSBuild 屬性設定為 `true`。
* 共用架構的 `<FrameworkReference>` 元素。

**Razor 類別庫**專案範本滿足上述以 .net Core 3.x 為目標的專案需求。 針對您的編輯器使用下列指示。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

使用 [ **Razor 類別庫**] 專案範本。 應選取範本的 [**支援頁面和流覽**器] 核取方塊。

# <a name="visual-studio-codetabvisual-studio-code"></a>[Visual Studio Code](#tab/visual-studio-code)

在整合式終端機中執行下列命令：

```dotnetcli
dotnet new razorclasslib -s
```

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[Visual Studio for Mac](#tab/visual-studio-mac)

目前沒有任何專案範本支援。

---

例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-razor-views-pages-library.csproj)]

如果專案是以 .NET Standard 為目標，則需要[AspNetCore 的 Mvc](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc)封裝參考。 `Microsoft.AspNetCore.Mvc` 套件已移至 ASP.NET Core 3.0 中的共用架構，因此不再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-razor-views-pages-library.csproj?highlight=8)]

### <a name="tag-helpers"></a>標籤協助程式

包含[標記](xref:mvc/views/tag-helpers/intro)協助程式的專案應該使用 `Microsoft.NET.Sdk` SDK。 如果目標是 .NET Core 3.x，請加入共用架構的 `<FrameworkReference>` 元素。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

如果目標 .NET Standard （以支援早于 ASP.NET Core 3.x 的版本），請將套件參考新增至[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)。 `Microsoft.AspNetCore.Mvc.Razor` 套件已移至共用架構中，因此不會再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-tag-helpers-library.csproj)]

### <a name="view-components"></a>檢視元件

包含[View 元件](xref:mvc/views/view-components)的專案應該使用 `Microsoft.NET.Sdk` SDK。 如果目標是 .NET Core 3.x，請加入共用架構的 `<FrameworkReference>` 元素。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netcoreapp3.0-basic-library.csproj)]

如果目標 .NET Standard （以支援早于 ASP.NET Core 3.x 的版本），請將套件參考新增至[AspNetCore ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures)。 `Microsoft.AspNetCore.Mvc.ViewFeatures` 套件已移至共用架構中，因此不會再發佈。 例如：

[!code-xml[](target-aspnetcore/samples/single-tfm/netstandard2.0-view-components-library.csproj)]

## <a name="support-multiple-aspnet-core-versions"></a>支援多個 ASP.NET Core 版本

撰寫支援多個 ASP.NET Core 變體的程式庫時，需要多目標。 假設有一個標記協助程式程式庫必須支援下列 ASP.NET Core 變體的情況：

* ASP.NET Core 2.1 目標 .NET Framework 4.6。1
* 以 .NET Core 2.x 為目標的 ASP.NET Core 2。x
* 以 .NET Core 3.x 為目標的 ASP.NET Core 3。x

下列專案檔透過 `TargetFrameworks` 屬性來支援這些變體：

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

使用上述專案檔：

* 會為所有取用者新增 `Markdig` 套件。
* 針對以 .NET Framework 4.6.1 或更新版本或 .NET Core 2.x 為目標的取用者，會加入[AspNetCore](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor)的參考。 由於回溯相容性，套件的版本2.1.0 會與 ASP.NET Core 2.2 搭配運作。
* 針對以 .NET Core 3.x 為目標的取用者，會參考共用架構。 `Microsoft.AspNetCore.Mvc.Razor` 套件包含在共用架構中。

或者，.NET Standard 2.0 可能會以為目標，而不是以 .NET Core 2.1 和 .NET Framework 4.6.1 為目標：

[!code-xml[](target-aspnetcore/samples/multi-tfm/alternative-tag-helpers-library.csproj?highlight=4)]

使用上述專案檔時，會有下列注意事項：

* 由於程式庫只包含標籤協助程式，因此以 ASP.NET Core 執行的特定平臺為目標會更直接： .NET Core 和 .NET Framework。 標籤協助程式無法供其他 .NET Standard 2.0 相容的目標架構（例如 Unity、UWP 和 Xamarin）使用。
* 使用 .NET Framework 中的 .NET Standard 2.0，會有一些在 .NET Framework 4.7.2 中已經解決的問題。 您可以藉由將目標設為 .NET Framework 4.6.1，藉由使用 .NET Framework 4.6.1 透過4.7.1 來改善取用者的體驗。

如果您的程式庫需要呼叫平臺特定的 Api，請以特定的 .NET 部署為目標，而不是 .NET Standard。 如需詳細資訊，請參閱[多目標](/dotnet/standard/library-guidance/cross-platform-targeting#multi-targeting)。

## <a name="use-an-api-that-hasnt-changed"></a>使用未變更的 API

想像您要將中介軟體程式庫從 .NET Core 2.2 升級至3.0 的案例。 程式庫中使用的 ASP.NET Core 中介軟體 Api 未在 ASP.NET Core 2.2 和3.0 之間變更。 若要繼續支援 .NET Core 3.0 中的中介軟體程式庫，請執行下列步驟：

* 遵循[標準程式庫指引](/dotnet/standard/library-guidance/)。
* 如果對應的元件不存在於共用架構中，則為每個 API 的 NuGet 套件新增套件參考。

## <a name="use-an-api-that-changed"></a>使用已變更的 API

假設您要將程式庫從 .NET Core 2.2 升級至 .NET Core 3.0。 程式庫中使用的 ASP.NET Core API 在 ASP.NET Core 3.0 中有[重大變更](/dotnet/core/compatibility/breaking-changes)。 請考慮是否可以將程式庫改寫成不要在所有版本中使用中斷的 API。

如果您可以重新撰寫程式庫，請執行此動作，並繼續以較早的目標架構（例如，.NET Standard 2.0 或 .NET Framework 4.6.1）為目標，並搭配套件參考。

如果您無法重寫程式庫，請採取下列步驟：

* 新增 .NET Core 3.0 的目標。
* 新增共用架構的 `<FrameworkReference>` 元素。
* 使用[#if 預處理器](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)指示詞搭配適當的目標 framework 符號，以有條件地編譯器代碼。

例如，HTTP 要求和回應資料流程的同步讀取和寫入預設會停用，從 ASP.NET Core 3.0。 ASP.NET Core 2.2 預設支援同步行為。 假設有一個中介軟體程式庫，在發生 IO 時應該啟用同步讀取和寫入。 程式庫應該括住程式碼，以在適當的預處理器指示詞中啟用同步功能。 例如：

[!code-csharp[](target-aspnetcore/samples/middleware.cs?highlight=9-24)]

## <a name="use-an-api-introduced-in-30"></a>使用3.0 中引進的 API

假設您想要使用 ASP.NET Core 3.0 中引進的 ASP.NET Core API。 請考慮下列問題：

1. 程式庫的功能是否需要新的 API？
1. 程式庫可以用不同的方式來執行這項功能嗎？

如果程式庫的功能需要 API，而且沒有任何方法可將它執行于下層：

* 僅以 .NET Core 2.x 為目標。
* 新增共用架構的 `<FrameworkReference>` 元素。

如果程式庫可以使用不同的方式來執行功能：

* 新增 .NET Core 3.x 做為目標架構。
* 新增共用架構的 `<FrameworkReference>` 元素。
* 使用[#if 預處理器](/dotnet/csharp/language-reference/preprocessor-directives/preprocessor-if)指示詞搭配適當的目標 framework 符號，以有條件地編譯器代碼。

例如，下列標記協助程式會使用 ASP.NET Core 3.0 中引進的 <xref:Microsoft.AspNetCore.Hosting.IWebHostEnvironment> 介面。 以 .NET Core 3.0 為目標的取用者，會執行 `NETCOREAPP3_0` 目標 framework 符號所定義的程式碼路徑。 標籤協助程式的函式參數類型會變更為 .NET Core 2.1 和 .NET Framework 4.6.1 取用者的 <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment>。 這是必要的變更，因為 ASP.NET Core 3.0 標示 `IHostingEnvironment` 為過時，並建議 `IWebHostEnvironment` 取代。

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

下列多目標專案檔支援此標記協助程式案例：

[!code-xml[](target-aspnetcore/samples/multi-tfm/recommended-tag-helpers-library.csproj)]

## <a name="use-an-api-removed-from-the-shared-framework"></a>使用從共用架構移除的 API

若要使用已從共用架構中移除的 ASP.NET Core 元件，請新增適當的套件參考。 如需 ASP.NET Core 3.0 中從共用架構移除的套件清單，請參閱[移除過時的套件參考](xref:migration/22-to-30#remove-obsolete-package-references)。

例如，若要新增 Web API 用戶端：

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
* [.NET 執行支援](/dotnet/standard/net-standard#net-implementation-support)
* [.NET 支援原則](https://dotnet.microsoft.com/platform/support/policy)
