---
title: RazorASP.NET Core 中的檔案編譯
author: rick-anderson
description: 瞭解檔案編譯如何 Razor 在 ASP.NET Core 應用程式中發生。
ms.author: riande
ms.custom: mvc
ms.date: 04/14/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/view-compilation
ms.openlocfilehash: 71487ff2d5d7d7cf96835778f386e5f30fa32254
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85405441"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>RazorASP.NET Core 中的檔案編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.1"

Razor具有*cshtml*副檔名的檔案會使用[ Razor SDK](xref:razor-pages/sdk)在組建和發行時間進行編譯。 設定專案時，可以選擇性地啟用執行時間編譯。

## <a name="razor-compilation"></a>Razor編譯

RazorSDK 預設會啟用檔案的組建時間和發行時間編譯 Razor 。 啟用時，執行時間編譯會補充組建階段編譯，讓檔案可以在 Razor 編輯時進行更新。

## <a name="enable-runtime-compilation-at-project-creation"></a>在建立專案時啟用執行時間編譯

Razor頁面和 MVC 專案範本包含一個選項，可在建立專案時啟用執行時間編譯。 ASP.NET Core 3.1 和更新版本支援此選項。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 [**建立新的 ASP.NET Core web 應用程式**] 對話方塊中：

1. 選取 [ **Web 應用程式**] 或 [ **web 應用程式（模型-視圖控制器）** ] 專案範本。
1. 選取 [**啟用 Razor 執行時間編譯**] 核取方塊。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用 `-rrc` 或 `--razor-runtime-compilation` 範本選項。 例如，下列命令會建立新的 Razor 頁面專案，並啟用執行時間編譯：

```dotnetcli
dotnet new webapp --razor-runtime-compilation
```

---

## <a name="enable-runtime-compilation-in-an-existing-project"></a>在現有的專案中啟用執行時間編譯

若要在現有專案中啟用所有環境的執行時間編譯：

1. 請安裝[AspNetCore Razor 。Microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。
1. 更新專案的 `Startup.ConfigureServices` 方法，以包含對的呼叫 <xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*> 。 例如：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

## <a name="conditionally-enable-runtime-compilation-in-an-existing-project"></a>有條件地啟用現有專案中的執行時間編譯

可以啟用執行時間編譯，使其僅適用于本機開發。 以這種方式有條件地啟用會確保已發行的輸出：

* 使用已編譯的視圖。
* 不會在生產環境中啟用檔案監看員。

若只要在開發環境中啟用執行時間編譯：

1. 請安裝[AspNetCore Razor 。Microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。
1. 修改 `environmentVariables` *launchSettings.js開啟*的啟動設定檔區段：
    * 確認 `ASPNETCORE_ENVIRONMENT` 設定為 `"Development"` 。
    * 將 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 設定為 `"Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"`。

在下列範例中，會在開發環境中啟用 `IIS Express` 和啟動設定檔的執行時間編譯 `RazorPagesApp` ：

[!code-json[](~/mvc/views/view-compilation/samples/3.1/launchSettings.json?highlight=15-16,24-25)]

專案的類別中不需要變更程式碼 `Startup` 。 在執行時間，ASP.NET Core 會在中搜尋[元件層級的 HostingStartup 屬性](xref:fundamentals/configuration/platform-specific-configuration#hostingstartup-attribute) `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` 。 `HostingStartup`屬性會指定要執行的應用程式啟動程式碼。 該啟動程式碼會啟用執行時間編譯。

## <a name="enable-runtime-compilation-for-a-razor-class-library"></a>啟用類別庫的執行時間編譯 Razor

假設有個 Razor 頁面專案參考名為*MyClassLib*的[ Razor 類別庫（RCL）](xref:razor-pages/ui-class)的案例。 RCL 包含您的所有小組 MVC 和頁面專案都會使用的 *_Layout. cshtml*檔案 Razor 。 您想要啟用該 RCL 中 *_Layout cshtml*檔案的執行時間編譯。 在 Pages 專案中進行下列變更 Razor ：

1. 使用在[現有專案中有條件地啟用執行時間編譯中](#conditionally-enable-runtime-compilation-in-an-existing-project)的指示，啟用執行時間編譯。
1. 在中設定執行時間編譯選項 `Startup.ConfigureServices` ：

    [!code-csharp[](~/mvc/views/view-compilation/samples/3.1/Startup.cs?name=snippet_ConfigureServices&highlight=5-10)]

    在上述程式碼中，會結構化*MyClassLib* RCL 的絕對路徑。 [PHYSICALFILEPROVIDER API](xref:fundamentals/file-providers#physicalfileprovider)是用來尋找位於該絕對路徑的目錄和檔案。 最後， `PhysicalFileProvider` 實例會加入至檔案提供者集合，讓您可以存取 RCL 的 *. cshtml*檔案。

## <a name="additional-resources"></a>其他資源

* [RazorCompileOnBuild 和 RazorCompileOnPublish](xref:razor-pages/sdk#properties)屬性。
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end

::: moniker range="= aspnetcore-3.0"

Razor具有*cshtml*副檔名的檔案會使用[ Razor SDK](xref:razor-pages/sdk)在組建和發行時間進行編譯。 您可以透過設定應用程式，選擇性地啟用執行階段編譯。

## <a name="razor-compilation"></a>Razor編譯

RazorSDK 預設會啟用檔案的組建時間和發行時間編譯 Razor 。 啟用時，執行時間編譯會補充組建階段編譯，讓檔案可以在 Razor 編輯時進行更新。

## <a name="runtime-compilation"></a>執行階段編譯

若要啟用所有環境和設定模式的執行時間編譯：

1. 請安裝[AspNetCore Razor 。Microsoft.aspnetcore.mvc.razor.runtimecompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。

1. 更新專案的 `Startup.ConfigureServices` 方法，以包含對的呼叫 <xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*> 。 例如：

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

1. 有條件地參考[AspNetCore Razor 。](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)根據使用中的值來 microsoft.aspnetcore.mvc.razor.runtimecompilation 套件 `Configuration` ：

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. 更新專案的 `Startup.ConfigureServices` 方法，以包含對的呼叫 `AddRazorRuntimeCompilation` 。 有條件 `AddRazorRuntimeCompilation` 地執行，使其只有在變數設定為時，才會在「Debug 模式」中執行 `ASPNETCORE_ENVIRONMENT` `Development` ：

    [!code-csharp[](~/mvc/views/view-compilation/samples/3.0/Startup.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* [RazorCompileOnBuild 和 RazorCompileOnPublish](xref:razor-pages/sdk#properties)屬性。
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
* 請參閱[GitHub 上的執行時間編譯範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation)，以取得示範如何在專案之間進行執行時間編譯的範例。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

當叫用 Razor 相關聯的 Razor 頁面或 MVC 視圖時，會在執行時間編譯檔案。 Razor檔案會使用[ Razor SDK](xref:razor-pages/sdk)在組建和發行時間進行編譯。

## <a name="razor-compilation"></a>Razor編譯

RazorSDK 預設會啟用檔案的組建和發行時間編譯 Razor 。 Razor在檔案更新之後編輯檔案，會在建立時受到支援。 根據預設，系統只會使用您的 *.cshtml*應用程式部署編譯的*Views.dll* ，以及編譯檔案所需的任何 cshtml 檔案或參考元件。 Razor

> [!IMPORTANT]
> 先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。 我們建議您遷移至[ Razor Sdk](xref:razor-pages/sdk)。
>
> Razor只有在專案檔中未設定預先編譯特定的屬性時，SDK 才會生效。 例如，將 *.csproj*檔案的 `MvcRazorCompileOnPublish` 屬性設定為會停用 `true` Razor SDK。

## <a name="runtime-compilation"></a>執行階段編譯

組建階段編譯是由檔案的執行時間編譯來補充 Razor 。 Razor當*cshtml*檔案的內容變更時，ASP.NET Core MVC 會重新編譯檔案。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
