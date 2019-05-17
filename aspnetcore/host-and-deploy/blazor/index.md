---
title: 裝載和部署 Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 04/24/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: c8a65b08582102af9129cf71ac4a108a905e49fc
ms.sourcegitcommit: dd9c73db7853d87b566eef136d2162f648a43b85
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/06/2019
ms.locfileid: "65085539"
---
# <a name="host-and-deploy-blazor"></a>裝載和部署 Blazor

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>發行應用程式

應用程式會在發行組態中發佈以供部署。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從巡覽列中選取 [建置] > [發佈 {應用程式}]。
1. 選取「發佈目標」。 若要在本機發佈，請選取 [資料夾]。
1. 接受 [選擇資料夾] 欄位中的預設位置，或指定不同的位置。 選取 [發行] 按鈕。


# <a name="visual-studio-code--net-core-clitabvisual-studio-codenetcore-cli"></a>[Visual Studio Code / .NET Core CLI](#tab/visual-studio-code+netcore-cli)

使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：

```console
dotnet publish -c Release
```

---

發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。

Blazor 用戶端應用程式會發佈至 */bin/Release/{目標 FRAMEWORK}/dist* 資料夾。 Blazor 伺服器端應用程式會發佈至 */bin/Release/{目標 FRAMEWORK}/publish* 資料夾。

該資料夾中的資產均會部署至 Web 伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="deployment"></a>部署

如需部署指導方針，請參閱下列主題：

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>
