---
title: 裝載及部署 ASP.NET Core Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 06/14/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: d18abbf33c71dca5130bfc6b503b46c1d5bce537
ms.sourcegitcommit: 776367717e990bdd600cb3c9148ffb905d56862d
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/09/2019
ms.locfileid: "68913931"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>裝載及部署 ASP.NET Core Blazor

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>發行應用程式

應用程式會在發行組態中發佈以供部署。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從巡覽列中選取 [建置]   > [發佈 {應用程式}]  。
1. 選取「發佈目標」  。 若要在本機發佈，請選取 [資料夾]  。
1. 接受 [選擇資料夾]  欄位中的預設位置，或指定不同的位置。 選取 [發行]  按鈕。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：

```console
dotnet publish -c Release
```

---

發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。

Blazor 用戶端應用程式會發佈至 /bin/Release/{目標 FRAMEWORK}/publish/{組件名稱}/dist  資料夾。 Blazor 伺服器端應用程式會發佈至 */bin/Release/{目標 FRAMEWORK}/publish* 資料夾。

該資料夾中的資產均會部署至 Web 伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="deployment"></a>部署

如需部署指導方針，請參閱下列主題：

* <xref:host-and-deploy/blazor/client-side>
* <xref:host-and-deploy/blazor/server-side>

## <a name="blazor-serverless-hosting-with-azure-storage"></a>使用 Azure Storage 的 Blazor 無伺服器裝載

Blazor 用戶端應用程式可從 [Azure Storage](https://azure.microsoft.com/services/storage/) 直接從儲存體容器以靜態內容方式提供。

如需詳細資訊，請參閱[裝載及部署 ASP.NET Core Blazor 用戶端 (獨立部署)：Azure 儲存體](xref:host-and-deploy/blazor/client-side#azure-storage)。
