---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
ms.author: riande
ms.custom: mvc
ms.date: 04/13/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 67bbeb88cd944791b522900b69bd10cff38c9f3a
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81277261"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 中 Razor 檔案的先行編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.1"

具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。 通過配置專案,可以選擇啟用運行時編譯。

## <a name="razor-compilation"></a>Razor 編譯

Razor SDK 預設啟用 Razor 檔的生成時間和發佈時間編譯。 啟用後,執行時編譯將補充生成時間編譯,允許在編輯 Razor 檔時對其進行更新。

## <a name="enable-runtime-compilation-at-project-creation"></a>在項目建立時開啟執行時編譯

Razor 頁面和 MVC 專案範本包括一個選項,用於在創建專案時啟用運行時編譯。 ASP.NET酷3.1及更高版本支援此選項。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

在 **'建立新ASP.NET核心 Web 應用程式**對話框中:

1. 選擇**Web 應用程式**或 Web**應用程式(模型檢視控制器)** 專案範本。
1. 勾選中啟用**Razor 執行時編譯**複選框。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用`-rrc`或`--razor-runtime-compilation`範本選項。 例如,以下指令建立啟用執行時編譯的新 Razor Pages 專案:

```dotnetcli
dotnet new webapp --razor-runtime-compilation
```

---

## <a name="enable-runtime-compilation-in-an-existing-project"></a>在現有項目中開啟執行時編譯

要為現有項目中的所有環境啟用運行時編譯,:

1. 安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。
1. 更新專案`Startup.ConfigureServices`的方法以包括<xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>對 的調用。 例如：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

## <a name="conditionally-enable-runtime-compilation-in-an-existing-project"></a>在現有項目中有條件執行時編譯

可以啟用運行時編譯,使其僅可用於本地開發。 以這種方式有條件地啟用可確保發布的輸出:

* 使用已編譯的視圖。
* 在生產中不啟用文件觀察程式。

要只在開發環境中開啟執行時編譯:

1. 安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。
1. 變更`environmentVariables`*啟動設定的*啟動檔部份 :
    * 認證`ASPNETCORE_ENVIRONMENT`設定`"Development"`為 。
    * 將 `ASPNETCORE_HOSTINGSTARTUPASSEMBLIES` 設定為 `"Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation"`。

在下面的範例中,在與`IIS Express``RazorPagesApp`啟動設定檔的開發環境中開啟執行時編譯:

[!code-json[](~/mvc/views/view-compilation/samples/3.1/launchSettings.json?highlight=15-16,24-25)]

專案`Startup`類不需要更改代碼。 在執行時,ASP.NET Core`Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation`搜尋中的[程式集級託管啟動屬性](xref:fundamentals/configuration/platform-specific-configuration#hostingstartup-attribute)。 該`HostingStartup`屬性指定要執行的應用啟動代碼。 該啟動代碼支援運行時編譯。

## <a name="additional-resources"></a>其他資源

* [Razor 編譯上建構和Razor編譯上發佈](xref:razor-pages/sdk#properties)屬性。
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
* 有關顯示跨項目進行執行時編譯工作的範例,請參閱[GitHub 上的執行時編譯範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation)。

::: moniker-end

::: moniker range="= aspnetcore-3.0"

具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。 您可以透過設定應用程式，選擇性地啟用執行階段編譯。

## <a name="razor-compilation"></a>Razor 編譯

Razor SDK 預設啟用 Razor 檔的生成時間和發佈時間編譯。 啟用後,執行時編譯將補充生成時間編譯,允許在編輯 Razor 檔時對其進行更新。

## <a name="runtime-compilation"></a>執行階段編譯

要為所有環境與設定模式啟用執行時編譯,:

1. 安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。

1. 更新專案`Startup.ConfigureServices`的方法以包括<xref:Microsoft.Extensions.DependencyInjection.RazorRuntimeCompilationMvcBuilderExtensions.AddRazorRuntimeCompilation*>對 的調用。 例如：

    ```csharp
    public void ConfigureServices(IServiceCollection services)
    {
        services.AddRazorPages()
            .AddRazorRuntimeCompilation();

        // code omitted for brevity
    }
    ```

### <a name="conditionally-enable-runtime-compilation"></a>有條件開啟執行時編譯

可以啟用運行時編譯,使其僅可用於本地開發。 以這種方式有條件地啟用可確保發布的輸出:

* 使用已編譯的視圖。
* 尺寸較小。
* 在生產中不啟用文件觀察程式。

要根據環境與設定模式啟用執行時編譯,:

1. 根據活動`Configuration`值有條件地引用[Microsoft.AspNetCore.Mvc.Razor.執行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)套件:

    ```xml
    <PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation" Version="3.1.0" Condition="'$(Configuration)' == 'Debug'" />
    ```

1. 更新專案`Startup.ConfigureServices`的方法以包括`AddRazorRuntimeCompilation`對 的調用。 有條件地`AddRazorRuntimeCompilation`執行,以便僅`ASPNETCORE_ENVIRONMENT`當 變數設定`Development`為 : 時,它才在除錯模式下執行:

    [!code-csharp[](~/mvc/views/view-compilation/samples/3.0/Startup.cs?name=snippet)]

## <a name="additional-resources"></a>其他資源

* [Razor 編譯上建構和Razor編譯上發佈](xref:razor-pages/sdk#properties)屬性。
* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>
* 有關顯示跨項目進行執行時編譯工作的範例,請參閱[GitHub 上的執行時編譯範例](https://github.com/aspnet/samples/tree/master/samples/aspnetcore/mvc/runtimecompilation)。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。 Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。

## <a name="razor-compilation"></a>Razor 編譯

Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 在建置階段中，支援更新 Razor 檔案後進行編輯。 根據預設，只有已編譯的 *Views.dll* 會與您的應用程式一起部署，而且在編譯 Razor 檔案時不需要任何 *.cshtml* 檔案或參考組件。

> [!IMPORTANT]
> 先行編譯工具已被取代，且將在 ASP.NET Core 3.0 中移除。 建議移轉到 [Razor SDK](xref:razor-pages/sdk)。
>
> 只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。 例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。

## <a name="runtime-compilation"></a>執行階段編譯

建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。 當 *.cshtml* 檔案的內容變更時，ASP.NET Core MVC 將重新編譯 Razor 檔案。

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

::: moniker-end
