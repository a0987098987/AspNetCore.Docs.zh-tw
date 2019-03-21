---
title: ASP.NET Core 中 Razor 檔案的先行編譯
author: rick-anderson
description: 了解在 ASP.NET Core 應用程式中發生 Razor 檔案編譯的方式。
monikerRange: '>= aspnetcore-1.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2019
uid: mvc/views/view-compilation
ms.openlocfilehash: 11195f00e922f6817a0fa0988fad9d8082dea30a
ms.sourcegitcommit: 10e14b85490f064395e9b2f423d21e3c2d39ed8b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/19/2019
ms.locfileid: "58142325"
---
# <a name="razor-file-compilation-in-aspnet-core"></a>ASP.NET Core 中 Razor 檔案的先行編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

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

Razor 檔案會在建置和發佈階段使用 [Razor SDK](xref:razor-pages/sdk) 來編譯。 您可以透過設定應用程式，選擇性地啟用執行階段編譯。

::: moniker-end

## <a name="razor-compilation"></a>Razor 編譯

::: moniker range=">= aspnetcore-3.0"
Razor SDK 預設會啟用 Razor 檔案的建置和發佈階段編譯。 啟用時，執行階段編譯將補充建置時間編譯，以允許更新 Razor 檔案 (如果檔案已經過編輯)。

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

使用 [.NET Core CLI 發行命令](/dotnet/core/tools/dotnet-publish)針對[框架相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)準備該應用程式。 例如，在專案的根目錄下執行下列命令：

```console
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

建置時間編譯會透過 Razor 檔案的執行階段編譯來補充。 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions> <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange> 可取得或設定可決定磁碟上的檔案變更時是否要重新編譯 Razor files (Razor 檢視與 Razor Pages) 的值。

針對下列項目，預設值是 `true`：

* 如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_1> 或更早版本。
* 如果應用程式的相容性版本設定為 <xref:Microsoft.AspNetCore.Mvc.CompatibilityVersion.Version_2_2> 或更新版本且應用程式位於 <xref:Microsoft.AspNetCore.Hosting.HostingEnvironmentExtensions.IsDevelopment*> 開發環境中。 換句話說，除非明確設定 <xref:Microsoft.AspNetCore.Mvc.Razor.RazorViewEngineOptions.AllowRecompilingViewsOnFileChange>，否則 Razor 檔案不會在非開發環境中重新編譯。

如需設定應用程式相容性版本的指導方針與範例，請參閱<xref:mvc/compatibility-version>。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

使用 `Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation` 封裝來啟用執行階段編譯。 若要啟用執行階段編譯，應用程式必須：

* 安裝 [Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.RuntimeCompilation/) NuGet 套件。
* 更新應用程式的 `ConfigureServices` 以包含對 `AddMvcRazorRuntimeCompilation` 的呼叫：

  ```csharp
  services
      .AddMvc()
      .AddRazorRuntimeCompilation()
  ```

針對要在部署時運作的執行階段編譯，應用程式必須額外修改其專案檔，以將 `PreserveCompilationReferences` 設定為 `true`。

[!code-xml[](view-compilation/sample/RuntimeCompilation.csproj?highlight=3)]

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
