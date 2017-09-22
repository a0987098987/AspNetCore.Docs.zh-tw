---
title: "ASP.NET Core 目錄結構"
author: guardrex
description: "已發行的 ASP.NET Core 應用程式目錄結構。"
keywords: "ASP.NET Core，目錄結構"
ms.author: riande
manager: wpickett
ms.date: 03/15/2017
ms.topic: article
ms.assetid: e55eb131-d42e-4bf6-b130-fd626082243c
ms.technology: aspnet
ms.prod: asp.net-core
uid: hosting/directory-structure
ms.openlocfilehash: 9ec635b596520e3d19f4040758dd59c1cbca97f4
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="directory-structure-of-published-aspnet-core-apps"></a>已發行的 ASP.NET Core 應用程式的目錄結構

由[Luke Latham](https://github.com/GuardRex)

在 ASP.NET Core，應用程式目錄，*發行*，組成應用程式檔案、 組態檔、 靜態資產、 封裝和執行階段 （適用於獨立的應用程式）。 這是舊版 ASP.NET 中，整個應用程式所在的 web 根目錄內相同的目錄結構。

| 應用程式類型 | 目錄結構 |
| --- | --- |
| Framework 相依的部署 | <ul><li>發行\*<ul><li>記錄檔\*（如果包含在 publishOptions）</li><li>refs\*</li><li>執行階段\*</li><li>檢視\*（如果包含在 publishOptions）</li><li>wwwroot\* （如果包含在 publishOptions）</li><li>.dll 檔案</li><li>myapp.deps.json</li><li>命名為 myapp.dll</li><li>myapp.pdb</li><li>myapp。PrecompiledViews.dll （如果先行編譯 Razor 檢視）</li><li>myapp。PrecompiledViews.pdb （如果先行編譯 Razor 檢視）</li><li>myapp.runtimeconfig.json</li><li>（如果包含在 publishOptions） 的 web.config</li></ul></li></ul> |
| 獨立的部署 | <ul><li>發行\*<ul><li>記錄檔\*（如果包含在 publishOptions）</li><li>refs\*</li><li>檢視\*（如果包含在 publishOptions）</li><li>wwwroot\* （如果包含在 publishOptions）</li><li>.dll 檔案</li><li>myapp.deps.json</li><li>myapp.exe</li><li>myapp.pdb</li><li>myapp。PrecompiledViews.dll （如果先行編譯 Razor 檢視）</li><li>myapp。PrecompiledViews.pdb （如果先行編譯 Razor 檢視）</li><li>myapp.runtimeconfig.json</li><li>（如果包含在 publishOptions） 的 web.config</li></ul></li></ul> |
\*表示目錄

內容*發行*目錄代表*內容的根路徑*，也稱為*應用程式基底路徑*的部署。 若要指定任何名稱*發行*部署中的目錄，它的位置做為託管的應用程式伺服器的實體路徑。 *Wwwroot*目錄中，如果有的話，只包含靜態資產。 *記錄*可能藉由建立專案中加入部署中包含目錄`<Target>`項目，如下所示，以您*.csproj*檔案或實體上建立目錄伺服器。

```xml
<Target Name="CreateLogsFolder" AfterTargets="AfterPublish">
  <MakeDir Directories="$(PublishDir)logs" Condition="!Exists('$(PublishDir)logs')" />
  <MakeDir Directories="$(PublishUrl)Logs" Condition="!Exists('$(PublishUrl)Logs')" />
</Target>
```

第一個`<MakeDir>`元素，其會使用`PublishDir`屬性，可由.NET Core CLI 來決定發行作業的目標位置。 第二個`<MakeDir>`元素，其會使用`PublishUrl`屬性，由 Visual Studio 用來判斷目標位置。 Visual Studio 會使用`PublishUrl`與非.NET Core 專案相容性的屬性。

部署目錄中需要讀取/執行權限，雖然*記錄*目錄需要讀取/寫入權限。 其他目錄將會寫入資產需要讀取/寫入權限。
