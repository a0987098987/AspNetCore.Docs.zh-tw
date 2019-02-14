---
title: ASP.NET Core 中 Razor 檔案的編譯和先行編譯
author: rick-anderson
description: 了解先行編譯 Razor 檔案的好處，以及如何完成 ASP.NET Core 應用程式中 Razor 檔案的先行編譯。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 02/13/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: c4e8f722fdf3d3f64807cc35ff9f349af7f32abd
ms.sourcegitcommit: 6ba5fb1fd0b7f9a6a79085b0ef56206e462094b7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 02/14/2019
ms.locfileid: "56248182"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 中 Razor 檔案的先行編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range="= aspnetcore-1.1"

叫用關聯的 MVC 檢視時，Razor 檔案便會在執行階段編譯。 不支援在建置階段發佈 Razor 檔案。 您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。 不支援在建置階段發佈 Razor 檔案。 您可以使用先行編譯工具，選擇性地在發佈階段編譯 Razor 檔案，並隨應用程式部署。

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

叫用關聯的 Razor 頁面或 MVC 檢視時，Razor 檔案便會在執行階段編譯。 Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。

::: moniker-end

## <a name="precompilation-considerations"></a>先行編譯的考量

以下是先行編譯 Razor 檔案的副作用：

* 較小的發佈組合
* 更快速的啟動時間
* 您無法編輯 Razor 檔案，因為發佈組合中沒有關聯的內容。

## <a name="deploy-precompiled-files"></a>部署先行編譯的檔案

::: moniker range=">= aspnetcore-2.1"

Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 在建置階段中，支援更新 Razor 檔案後進行編輯。 根據預設，只有編譯的 *Views.dll* 會與應用程式一起部署，而 *.cshtml* 檔案則否。

> [!IMPORTANT]
> ASP.NET Core 3.0 中將移除先行編譯工具。 建議移轉到 [Razor SDK](xref:razor-pages/sdk)。
>
> 只有當專案檔中未設定先行編譯特定的屬性時，Razor SDK 才會有效。 例如，將 *.csproj* 檔案的 `MvcRazorCompileOnPublish` 屬性設定成 `true`，便會停用 Razor SDK。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

如果您的專案以 .NET Framework 為目標，請安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件：

[!code-xml[](view-compilation/sample/DotNetFrameworkProject.csproj?name=snippet_ViewCompilationPackage)]

如果您專案的目標設為 .NET Core，則不需要進行任何變更。

根據預設，ASP.NET Core 2.x 專案範本會隱含地將 `MvcRazorCompileOnPublish` 屬性設定成 `true`。 因此，可以安全地將元素從 *.csproj* 檔案中移除。

> [!IMPORTANT]
> ASP.NET Core 3.0 中將移除先行編譯工具。 建議移轉到 [Razor SDK](xref:razor-pages/sdk)。
>
> 在 ASP.NET Core 2.0 中執行[自封式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，無法使用 Razor 檔案先行編譯。

::: moniker-end

::: moniker range="= aspnetcore-1.1"

將 `MvcRazorCompileOnPublish` 屬性設定成 `true`，然後安裝 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) NuGet 套件。 下列 *.csproj* 範例會反白顯示這些設定：

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=4,10)]

::: moniker-end

::: moniker range="<= aspnetcore-2.0"

使用 [.NET Core CLI 發行命令](/dotnet/core/tools/dotnet-publish)針對[框架相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)準備該應用程式。 例如，在專案的根目錄下執行下列命令：

```console
dotnet publish -c Release
```

先行編譯成功時，會產生 <專案名稱>.PrecompiledViews.dll 檔案，其中包含編譯過的 Razor 檔案。 例如，以下螢幕擷取畫面為 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)

::: moniker-end

## <a name="recompile-razor-files-on-change"></a>在發生變更時重新編譯 Razor 檔案

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 可取得或設定可決定磁碟上的檔案變更時是否要重新編譯 Razor files (Razor 檢視與 Razor Pages) 的值。

當設定為 `true` 時，[IFileProvider.Watch](xref:Microsoft.Extensions.FileProviders.IFileProvider.Watch*) 會監視已設定之 <xref:Microsoft.Extensions.FileProviders.IFileProvider> 執行個體中的 Razor 是否發生變更。

針對下列項目，預設值是 `true`：

* ASP.NET Core 2.1 或更舊的應用程式。
* ASP.NET Core 2.2 或更新的應用程式 (在開發環境中)。

<xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 與相容性參數關聯，而且可以視已針對應用程式設定的相容性版本提供不同的行為。 透過設定 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 優先順序高於應用程式相容性版本意指的值，來設定應用程式。

若應用程式的相容性版本是設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> 或更舊版本，則除非已明確設定，否則 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 會設定為 `true`。

若應用程式的相容性版本是設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> 或更新版本，則除非環境為開發環境或已明確設定值，否則 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 會設定為 `false`。

如需設定應用程式相容性版本的指導方針與範例，請參閱<xref:mvc/compatibility-version>。

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
