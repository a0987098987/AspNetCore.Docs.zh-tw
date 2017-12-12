---
title: "Razor 檢視編譯和先行編譯"
author: rick-anderson
description: "說明如何啟用 MVC Razor 檢視編譯和先行編譯 ASP.NET Core 應用程式中的參考文件。"
keywords: "ASP.NET Core Razor 檢視編譯、 Razor 預先編譯、 Razor 先行編譯"
ms.author: riande
manager: wpickett
ms.date: 12/05/2017
ms.topic: article
ms.assetid: ab4705b7-1638-1638-bc97-ea7f292fe92a
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/view-compilation
ms.openlocfilehash: 873f6203f9e7b5bb14968dcec3f8d8e5548bd834
ms.sourcegitcommit: 282f69e8dd63c39bde97a6d72783af2970d92040
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/05/2017
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>Razor 檢視編譯和先行編譯的 ASP.NET Core

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

檢視叫用時，會在執行階段編譯 razor 檢視。 ASP.NET 核心 1.1.0 和更新版本可以選擇性地編譯 Razor 檢視，以及將其部署與應用程式&mdash;稱為先行編譯的程序。 ASP.NET Core 2.x 專案範本預設會啟用先行編譯。

> [!NOTE]
> 執行時，是目前無法使用 razor 檢視先行編譯[獨立的部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) ASP.NET Core 2.0 中。 2.1 版本時，此功能可供 Scd。 如需詳細資訊，請參閱[檢視編譯會失敗，在 Windows 上的 Linux 交互編譯時](https://github.com/aspnet/MvcPrecompilation/issues/102)。

先行編譯的考量：

* 先行編譯的檢視會導致較小的大小已發行的組合和更快的啟動時間。
* 您無法編輯 Razor 檔案之後您先行編譯的檢視。 已編輯的檢視將不會出現在已發行的套件組合。 

若要部署先行編譯的檢視：

# <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x)

如果您的專案以.NET Framework 為目標，包含的封裝參考[Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/):

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

如果您的專案的目標.NET Core，不不需要任何變更。

隱含地設定 ASP.NET Core 2.x 專案範本`MvcRazorCompileOnPublish`至`true`根據預設，這表示這個節點可以安全地移除從*.csproj*檔案。 如果您想要明確，是在設定中的沒有壞處`MvcRazorCompileOnPublish`屬性`true`。 下列*.csproj*範例強調這項設定：

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish2.csproj?highlight=5)]

# <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x)

設定`MvcRazorCompileOnPublish`至`true`，和包含的封裝參考`Microsoft.AspNetCore.Mvc.Razor.ViewCompilation`。 下列*.csproj*範例會反白顯示這些設定：

[!code-xml[Main](view-compilation\sample\MvcRazorCompileOnPublish.csproj?highlight=5,12)]

---

A *< project_name >。PrecompiledViews.dll*當先行編譯成功，則會產生包含已編譯的 Razor 檢視的檔案。 例如，下列螢幕擷取畫面會描述的內容*Index.cshtml*內*WebApplication1.PrecompiledViews.dll*:

![DLL 內的 razor 檢視](view-compilation/_static/razor-views-in-dll.png)
