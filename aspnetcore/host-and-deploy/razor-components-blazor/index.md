---
title: 裝載和部署 Razor 元件和 Blazor
author: guardrex
description: 探索如何裝載和部署 Razor 元件和 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 03/28/2019
uid: host-and-deploy/razor-components-blazor/index
ms.openlocfilehash: eeaa27bef584523b77d8b47000b310fe0cac7341
ms.sourcegitcommit: 6bde1fdf686326c080a7518a6725e56e56d8886e
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/08/2019
ms.locfileid: "59070305"
---
# <a name="host-and-deploy-razor-components-and-blazor"></a>裝載和部署 Razor 元件和 Blazor

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>發行應用程式

應用程式在發行組態中使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令發佈以供部署。 整合式開發環境 (IDE) 可能會自動使用內建的發佈功能處理執行 `dotnet publish` 命令，因此可能不需要以手動方式從命令提示字元執行命令，視使用中的開發工具而定。

```console
dotnet publish -c Release
```

`dotnet publish` 會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)，並[建置](/dotnet/core/tools/dotnet-build)專案，然後才建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。 部署會建立在 */bin/Release/{TARGET FRAMEWORK}/publish* 資料夾中。

*publish* 資料夾中的資產會部署至網頁伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="deployment"></a>部署

如需部署指導方針，請參閱下列主題：

* [用戶端 Blazor](xref:host-and-deploy/razor-components-blazor/blazor)
* [伺服器端 ASP.NET Core Razor 元件](xref:host-and-deploy/razor-components-blazor/razor-components)
