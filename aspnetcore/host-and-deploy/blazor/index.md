---
title: 裝載和部署 Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/18/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 774fbb6fdaab14a015db4fb39de2e1ea73a1837b
ms.sourcegitcommit: eb784a68219b4829d8e50c8a334c38d4b94e0cfa
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/22/2019
ms.locfileid: "59982636"
---
# <a name="host-and-deploy-blazor"></a>裝載和部署 Blazor

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

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
