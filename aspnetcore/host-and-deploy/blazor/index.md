---
title: 裝載及部署 ASP.NET Core Blazor
author: guardrex
description: 探索如何裝載和部署 Blazor 應用程式。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 09/05/2019
uid: host-and-deploy/blazor/index
ms.openlocfilehash: 0ded2979b8576f10812e20ae3385c94fd29689c2
ms.sourcegitcommit: 215954a638d24124f791024c66fd4fb9109fd380
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2019
ms.locfileid: "71081041"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>裝載及部署 ASP.NET Core Blazor

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

## <a name="publish-the-app"></a>發行應用程式

應用程式會在發行組態中發佈以供部署。

# <a name="visual-studiotabvisual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從巡覽列中選取 [建置] > [發佈 {應用程式}]。
1. 選取「發佈目標」。 若要在本機發佈，請選取 [資料夾]。
1. 接受 [選擇資料夾] 欄位中的預設位置，或指定不同的位置。 選取 [發行] 按鈕。

# <a name="net-core-clitabnetcore-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：

```dotnetcli
dotnet publish -c Release
```

---

發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。

Blazor WebAssembly 應用程式會發佈至 */BIN/RELEASE/{TARGET FRAMEWORK}/PUBLISH/{ASSEMBLY NAME}/dist*資料夾。 Blazor 伺服器應用程式會發佈至 */BIN/RELEASE/{TARGET FRAMEWORK}/publish*資料夾。

該資料夾中的資產均會部署至 Web 伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="app-base-path"></a>應用程式基底路徑

*應用程式基底路徑*是應用程式的根 URL 路徑。 請考慮下列主要應用程式和 Blazor 應用程式：

* 主要的應用程式稱為`MyApp`：
  * 應用程式實際位於 *\\d： MyApp*。
  * 會在`https://www.contoso.com/{MYAPP RESOURCE}`收到要求。
* 名`CoolApp`為的 Blazor 應用程式是的`MyApp`子應用程式：
  * 子應用程式實際上位於*d：\\MyApp\\CoolApp*。
  * 會在`https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`收到要求。

若未指定的`CoolApp`其他設定，此案例中的子應用程式就不會知道其位於伺服器上的位置。 例如，應用程式無法在不知道其位於相對 URL 路徑`/CoolApp/`的情況下，對其資源建立正確的相對 url。

若要為 Blazor 應用程式的`https://www.contoso.com/CoolApp/`基底路徑提供設定`<base>` ，標記的`href`屬性會設為*wwwroot/index.html*檔案中的相對根路徑：

```html
<base href="/CoolApp/">
```

藉由提供相對的 URL 路徑，不在根目錄中的元件可以針對應用程式的根路徑來建立 Url。 不同目錄結構層級的元件可以在整個應用程式的位置建立其他資源的連結。 應用程式基底路徑也會用來攔截超連結點擊，其中連結的 `href` 目標是在應用程式基底路徑 URI 空間內&mdash;Blazor 路由器會處理內部瀏覽。

在許多裝載案例中，應用程式的相對 URL 路徑是應用程式的根目錄。 在這些情況下，應用程式的相對 URL 基底路徑是正斜線`<base href="/" />`（），這是 Blazor 應用程式的預設設定。 在其他裝載案例（例如 GitHub 頁面和 IIS 子應用程式）中，應用程式基底路徑必須設定為應用程式的伺服器相對 URL 路徑。

若要設定應用程式的基底路徑，請更新 *wwwroot/index.html* 檔案 `<head>` 標籤項目內的 `<base>` 標籤。 將屬性值設定為`/{RELATIVE URL PATH}/` （需要尾端斜線），其中`{RELATIVE URL PATH}`是應用程式的完整相對 URL 路徑。 `href`

針對具有非根相對 URL 路徑的應用程式（ `<base href="/CoolApp/">`例如），在*本機執行時*，應用程式無法找到其資源。 若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。 若要在本機執行應用程式時傳遞 path base 引數， `dotnet run`請`--pathbase`使用下列選項從應用程式的目錄執行命令：

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

針對具有（ `/CoolApp/` `<base href="/CoolApp/">`）之相對 URL 路徑的應用程式，命令為：

```dotnetcli
dotnet run --pathbase=/CoolApp
```

應用程式在 `http://localhost:port/CoolApp` 本機回應。

## <a name="deployment"></a>部署

如需部署指導方針，請參閱下列主題：

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
