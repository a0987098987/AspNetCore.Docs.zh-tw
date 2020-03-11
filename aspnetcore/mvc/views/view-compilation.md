---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/05/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: cd096bba5eb580c0a606699a2bf7c36442fb56f7
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78661102"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 中 Razor 檔案的先行編譯

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

::: moniker range="= aspnetcore-1.1"

叫用關聯的 MVC 檢視時，Razor 檔案便會在執行階段編譯。 不支援在建置階段發佈 Razor 檔案。 您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。 不支援在建置階段發佈 Razor 檔案。 您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。 Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

具有*cshtml*副檔名的 Razor 檔案會使用[razor SDK](xref:razor-pages/sdk)，在組建和發行時間進行編譯。 您可以透過設定應用程式，選擇性地啟用執行階段編譯。

::: moniker-end

## <a name="razor-compilation"></a>Razor 編譯

::: moniker range=">= aspnetcore-3.0"

Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 啟用時，執行階段編譯會補充建置時間編譯，以允許更新 Razor 檔案 (如果該檔案已被編輯)。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 在建置階段中，支援更新 Razor 檔案後進行編輯。 根據預設，只有已編譯的 *Views.dll* 會與您的應用程式一起部署，而且在編譯 Razor 檔案時不需要任何 *.cshtml* 檔案或參考組件。

> [!IMPORTANT]
> 先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。 建議移轉到 [Razor SDK](xref:razor-pages/sdk)。
>
> 只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。 例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

如果您的專案以 .NET Framework 為目標，請安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件：

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

如果您專案的目標設為 .NET Core，則不需要進行任何變更。

根據預設，ASP.NET Core 2.x 專案範本會隱含地將 `MvcRazorCompileOnPublish` 屬性設定成 `true`。 因此，可以安全地將元素從 *.csproj* 檔案中移除。

> [!IMPORTANT]
> 先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。 建議移轉到 [Razor SDK](xref:razor-pages/sdk)。
>
> 在 ASP.NET Core 2.0 中執行[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，無法使用 Razor 檔案先行編譯。

::: moniker-end

::: moniker range="= aspnetcore-1.1"

將 `MvcRazorCompileOnPublish` 屬性設定成 `true`，然後安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件。 下列 *.csproj* 範例會反白顯示這些設定：

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

使用 [.NET Core CLI 發行命令](/dotnet/core/deploying/#framework-dependent-deployments-fdd)針對[框架相依部署](/dotnet/core/tools/dotnet-publish)準備該應用程式。 例如，在專案的根目錄下執行下列命令：

```dotnetcli
dotnet publish -c Release
```

先行編譯成功時，會產生 *\<專案名稱>.PrecompiledViews.dll* 檔案，其中包含編譯過的 Razor 檔案。 例如，以下螢幕擷取畫面為 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="runtime-compilation"></a>執行階段編譯

::: moniker range="= aspnetcore-2.1"

建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。 當 *.cshtml* 檔案的內容變更時，ASP.NET Core MVC 將重新編譯 Razor 檔案。

::: moniker-end

::: moniker range="= aspnetcore-2.2"

建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 取得或設定值，以決定是否要重新編譯 Razor 檔案（Razor views 和 Razor Pages），並在磁片上的檔案變更時進行更新。

針對下列項目，預設值是 `true`：

* 如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> 或更早版本。
* 如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> 或更新版本，而且應用程式位於 <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*> 開發環境中。 換句話說，除非明確設定 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>，否則 Razor 檔案不會在非開發環境中重新編譯。

如需設定應用程式相容性版本的指導方針與範例，請參閱<xref:mvc/compatibility-version>。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

若要啟用所有環境和設定模式的執行時間編譯：

1. 安裝 [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。

1. 更新專案的 `Startup.ConfigureServices` 方法，以包含對 `AddRazorRuntimeCompilation`的呼叫。 例如：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>有條件地啟用執行時間編譯

可以啟用執行時間編譯，使其僅適用于本機開發。 以這種方式有條件地啟用會確保已發行的輸出：

* 使用已編譯的視圖。
* 大小較小。
* 不會在生產環境中啟用檔案監看員。

若要根據環境和設定模式啟用執行時間編譯：

1. 根據作用中的 `Configuration` 值，有條件地參考[microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)套件：

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. 更新專案的 `Startup.ConfigureServices` 方法，以包含對 `AddRazorRuntimeCompilation`的呼叫。 有條件地執行 `AddRazorRuntimeCompilation`，使其只有在 `ASPNETCORE_ENVIRONMENT` 變數設定為 `Development`時，才會以 Debug 模式執行：

    ```csharp
    public IWebHostEnvironment Env { get; set; }

    public void ConfigureServices(IServiceCollection services)
    {
        IMvcBuilder builder = services.AddRazorPages();

    #if DEBUG
        if (Env.IsDevelopment())
        {
            builder.AddRazorRuntimeCompilation();
        }
    #endif

        // code omitted for brevity
    }
    ```

::: moniker-end

## <a name="additional-resources"></a>其他資源

::: moniker range="= aspnetcore-1.1"

* <xref:mvc/views/overview>

::: moniker-end

::: moniker range="= aspnetcore-2.0"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
