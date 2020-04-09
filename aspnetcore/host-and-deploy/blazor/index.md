---
title: 管理管及部署ASP.NET核心Blazor
author: guardrex
description: 瞭解如何託管和部署Blazor應用。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/11/2020
no-loc:
- Blazor
- SignalR
uid: host-and-deploy/blazor/index
ms.openlocfilehash: ddf70da29a82d462422c1bdf74ff45b92bb10b56
ms.sourcegitcommit: 72792e349458190b4158fcbacb87caf3fc605268
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "79434261"
---
# <a name="host-and-deploy-aspnet-core-blazor"></a>裝載及部署 ASP.NET Core Blazor

作者：[Luke Latham](https://github.com/guardrex)、[Rainer Stropek](https://www.timecockpit.com) 和 [Daniel Roth](https://github.com/danroth27)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="publish-the-app"></a>發佈應用程式

應用程式會在發行組態中發佈以供部署。

# <a name="visual-studio"></a>[Visual Studio](#tab/visual-studio)

1. 從導航欄中選擇**生成** > **發佈 [應用]。**
1. 選取「發佈目標」**。 若要在本機發佈，請選取 [資料夾]****。
1. 接受 [選擇資料夾]**** 欄位中的預設位置，或指定不同的位置。 選取 [發佈]**** 按鈕。

# <a name="net-core-cli"></a>[.NET Core CLI](#tab/netcore-cli)

使用 [dotnet publish](/dotnet/core/tools/dotnet-publish) 命令，搭配發行組態來發佈應用程式：

```dotnetcli
dotnet publish -c Release
```

---

發佈應用程式會觸發專案相依性的[還原](/dotnet/core/tools/dotnet-restore)並[建置](/dotnet/core/tools/dotnet-build)專案，然後才會建立資產以供部署。 在建置程序中，會移除未使用的方法和組件，減少應用程式的下載大小和載入時間。

發佈位置:

* Blazor網路組裝
  * 獨立&ndash;應用程式發佈到 */bin/發佈/[目標框架]/發佈/wwwroot*檔夾中。 要將應用部署為靜態網站,請將*wwwroot*資料夾的內容複製到靜態網站主機。
  * 託管&ndash;客戶Blazor端 WebAssembly 應用與伺服器應用的任何其他靜態 Web 資源一起發布到伺服器應用的 */bin/發佈/目標框架\/發佈/wwwroot*資料夾中。 將*發佈*資料夾的內容部署到主機。
* Blazor伺服器&ndash;應用發佈到 */bin/發佈/[目標框架]/發佈*資料夾中。 將*發佈*資料夾的內容部署到主機。

該資料夾中的資產均會部署至 Web 伺服器。 部署可能是手動或自動的程序，視使用中的開發工具而定。

## <a name="app-base-path"></a>應用程式基底路徑

*應用基本路徑*是應用的根 URL 路徑。 請考慮以下ASP.NET核心應用和Blazor子應用:

* ASP.NET核心應用程式命名為`MyApp`:
  * 該應用程式實際駐留在*d:/MyApp*。
  * 要求在`https://www.contoso.com/{MYAPP RESOURCE}`接收 。
* 名為Blazor的`CoolApp`應用程式 是`MyApp`: 的 子應用:
  * 子應用物理駐留在*d:/MyApp/CoolApp*。
  * 要求在`https://www.contoso.com/CoolApp/{COOLAPP RESOURCE}`接收 。

如果不為`CoolApp`指定的其他配置,此方案中的子應用不知道它駐留在伺服器上的位置。 例如,如果應用程式位於相對 URL`/CoolApp/`路徑中,則無法為其資源構造正確的相對 URL。

Blazor要為 應用程式的基本路徑提供`https://www.contoso.com/CoolApp/`設定`<base>`, 標`href`記的屬性 設定為*Pages/_Host.cshtml*檔案(Blazor伺服器)或*wwwroot/index.html*Blazor檔案(WebAssembly) 的相對根路徑:

```html
<base href="/CoolApp/">
```

Blazor伺服器套用在應用程式的要求導<xref:Microsoft.AspNetCore.Builder.UsePathBaseExtensions.UsePathBase*>`Startup.Configure`管中呼叫 :

```csharp
app.UsePathBase("/CoolApp");
```

通過提供相對 URL 路徑,不在根目錄中的元件可以構造相對於應用根路徑的 URL。 目錄結構不同級別的元件可以在整個應用的位置生成指向其他資源的連結。 應用基本路徑還用於攔截連結`href`目標位於應用基本路徑URI空間內的選定超連結。 路由器Blazor處理內部導航。

在許多託管方案中,應用的相對 URL 路徑是應用的根目錄。 在這些情況下,應用的相對 URL 基本路徑是前進斜杠`<base href="/" />`(), 這是Blazor應用的默認配置。 在其他託管方案中(如 GitHub 主頁和 IIS 子應用)中,應用基本路徑必須設置為應用的伺服器相對 URL 路徑。

要設置應用的基本路徑,請更新*Pages/_Host.cshtml*檔(Blazor伺服器) 或*wwwroot/index.html*檔(WebAssembly)Blazor`<base>``<head>`的標記元素中的標記。 將`href`屬性值設置`/{RELATIVE URL PATH}/`為 (需要尾隨斜杠),其中`{RELATIVE URL PATH}`應用的完整相對 URL 路徑。

對於具有Blazor非根相對 URL 路徑的 WebAssembly`<base href="/CoolApp/">`應用(例如 ),該應用*在本地運行時*找不到其資源。 若要在本機開發和測試期間解決這個問題，您可以提供「基底路徑」** 引數，讓它在執行時符合 `<base>` 標籤的 `href` 值。 不要包括尾隨斜杠。 要在本地端執行應用程式時傳遞路徑基參數`dotnet run`,`--pathbase`請使用以下選項從應用程式的目錄中執行命令:

```dotnetcli
dotnet run --pathbase=/{RELATIVE URL PATH (no trailing slash)}
```

對於相對BlazorURL`/CoolApp/`路徑的 WebAssembly 應用`<base href="/CoolApp/">`, 指令是:

```dotnetcli
dotnet run --pathbase=/CoolApp
```

WebAssemblyBlazor應用在`http://localhost:port/CoolApp`本地回應。

## <a name="deployment"></a>部署

如需部署指導方針，請參閱下列主題：

* <xref:host-and-deploy/blazor/webassembly>
* <xref:host-and-deploy/blazor/server>
