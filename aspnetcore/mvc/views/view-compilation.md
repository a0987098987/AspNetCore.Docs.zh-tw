---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
ms.author: riande
ms.custom: mvc
ms.date: 4/8/2020
uid: mvc/views/view-compilation
ms.openlocfilehash: 7f329ffb4c63e8699663f49720145984bb8802fd
ms.sourcegitcommit: 9a46e78c79d167e5fa0cddf89c1ef584e5fe1779
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/09/2020
ms.locfileid: "80994599"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 中 Razor 檔案的先行編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

具有 *.cshtml*副檔名的 Razor 檔在使用[Razor SDK](xref:razor-pages/sdk)在生成和發表時間編譯。 您可以透過設定應用程式，選擇性地啟用執行階段編譯。

## <a name="razor-compilation"></a>Razor 編譯

Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 啟用時，執行階段編譯會補充建置時間編譯，以允許更新 Razor 檔案 (如果該檔案已被編輯)。

## <a name="runtime-compilation"></a>執行階段編譯

要為所有環境與設定模式啟用執行時編譯,:

1. 安裝[微軟.AspNetCore.Mvc.Razor.運行時編譯](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/)NuGet包。

1. 更新專案`Startup.ConfigureServices`的方法以包括`AddRazorRuntimeCompilation`對 的調用。 例如：

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

## <a name="additional-resources"></a>其他資源

* <xref:razor-pages/index>
* <xref:mvc/views/overview>
* <xref:razor-pages/sdk>

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