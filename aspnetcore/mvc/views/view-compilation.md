---
title: ASP.NET Core 中的 Razor 檢視編譯和先行編譯
author: rick-anderson
description: 了解如何啟用 ASP.NET Core 應用程式中的 MVC Razor 檢視編譯和先行編譯。
manager: wpickett
ms.author: riande
ms.date: 12/13/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/view-compilation
ms.openlocfilehash: 5d971645106a79497a9902063c7774dc6d546395
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
# <a name="razor-view-compilation-and-precompilation-in-aspnet-core"></a>ASP.NET Core 中的 Razor 檢視編譯和先行編譯

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

叫用檢視時，會在執行階段編譯 Razor 檢視。 ASP.NET Core 1.1.0 和更新版本可以選擇性地編譯 Razor 檢視，以及使用應用程式進行部署&mdash;稱為先行編譯的程序。 ASP.NET Core 2.x 專案範本預設會啟用先行編譯。

> [!IMPORTANT]
> 在 ASP.NET Core 2.0 中執行[獨立式部署 (SCD)](/dotnet/core/deploying/#self-contained-deployments-scd) 時，目前無法提供 Razor 檢視先行編譯。 2.1 版時，此功能可供 SCD 使用。 如需詳細資訊，請參閱[交叉編譯 Linux on Windows 時檢視編譯失敗](https://github.com/aspnet/MvcPrecompilation/issues/102)。

先行編譯考量：

* 先行編譯檢視會導致較小的已發行套件組合以及更快的啟動時間。
* 在您先行編譯檢視之後，就無法編輯 Razor 檔案。 編輯過的檢視將不會出現在已發行的套件組合中。 

部署先行編譯的檢視：

#### <a name="aspnet-core-2xtabaspnetcore2x"></a>[ASP.NET Core 2.x](#tab/aspnetcore2x/)
如果您專案的目標設為 .NET Framework，請包含 [Microsoft.AspNetCore.Mvc.Razor.ViewCompilation](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.Razor.ViewCompilation/) 的套件參考：

```xml
<PackageReference Include="Microsoft.AspNetCore.Mvc.Razor.ViewCompilation" Version="2.0.0" PrivateAssets="All" />
```

如果您專案的目標設為 .NET Core，則不需要進行任何變更。

ASP.NET Core 2.x 專案範本預設會將 `MvcRazorCompileOnPublish` 隱含地設定為 `true`，這表示可以從 *.csproj* 檔案安全地移除此節點。 如果您想要更為明確，則請將 `MvcRazorCompileOnPublish` 屬性設定為 `true`。 下列 *.csproj* 範例會反白顯示此設定：

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish2.csproj?highlight=5)]

#### <a name="aspnet-core-1xtabaspnetcore1x"></a>[ASP.NET Core 1.x](#tab/aspnetcore1x/)
將 `MvcRazorCompileOnPublish` 設定為 `true`，並包括 `Microsoft.AspNetCore.Mvc.Razor.ViewCompilation` 的套件參考。 下列 *.csproj* 範例會反白顯示這些設定：

[!code-xml[](view-compilation/sample/MvcRazorCompileOnPublish.csproj?highlight=5,12)]

使用 [.NET Core CLI 發行命令](/dotnet/core/tools/dotnet-publish)針對[框架相依部署](/dotnet/core/deploying/#framework-dependent-deployments-fdd)準備該應用程式。 例如，在專案的根目錄下執行下列命令：

```console
dotnet publish -c Release
```

先行編譯成功時，會產生 <專案名稱>.PrecompiledViews.dll 檔案 (包含編譯過的 Razor 檢視)。 例如，下列螢幕擷取畫面描述 *WebApplication1.PrecompiledViews.dll* 內 *Index.cshtml* 的內容：

![DLL 內部的 Razor 檢視](view-compilation/_static/razor-views-in-dll.png)
