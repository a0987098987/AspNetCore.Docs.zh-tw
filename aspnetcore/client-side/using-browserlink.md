---
title: ASP.NET核心中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結如何是一個可視化工作室功能,該功能將開發環境與一個或多個 Web 瀏覽器連結。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 19cc3c2ed91bd9e05df3c036123c78ecbf81fcc0
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658848"
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET核心中的瀏覽器連結

由[尼科萊·卡蘭迪尼](https://github.com/ncarandini)、[邁克·瓦森](https://github.com/MikeWasson)和[湯姆·戴克斯特拉](https://github.com/tdykstra)

瀏覽器連結是視覺工作室功能。 它在開發環境與一個或多個 Web 瀏覽器之間創建通信通道。 您可以使用瀏覽器連結同時刷新多個瀏覽器中的 Web 應用,這對於跨瀏覽器測試非常有用。

## <a name="browser-link-setup"></a>瀏覽器連結設定

::: moniker range=">= aspnetcore-3.0"

將[Microsoft.VisualStudio.Web.瀏覽器連結](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包添加到您的專案中。 對於ASP.NET核心剃刀頁面或MVC專案,也啟用剃刀 *(.cshtml)* 檔的<xref:mvc/views/view-compilation>運行時編譯,如中所述。 僅當啟用運行時編譯時,才會應用 Razor 語法更改。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

將ASP.NET酷睿2.0專案轉換為ASP.NET酷睿2.1並過渡到[微軟.AspNetCore.App元包](xref:fundamentals/metapackage-app)時,安裝[微軟.VisualStudio.Web.瀏覽器鏈接](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包,用於瀏覽器連結功能。 默認情況下,ASP.NET Core 2.1 專案`Microsoft.AspNetCore.App`範本使用元包。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET核心2.0 **Web應用程式**、**空**和**Web API**專案範本使用[微軟.AspNetCore.所有元包](xref:fundamentals/metapackage),其中包含[微軟的](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件引用。 因此,`Microsoft.AspNetCore.All`使用元包不需要進一步的操作,以使瀏覽器連結可供使用。

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET核心 1.x **Web 應用程式**專案範本具有[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)包的包引用。 其他項目類型要求您添加對`Microsoft.VisualStudio.Web.BrowserLink`的包引用。

::: moniker-end

### <a name="configuration"></a>組態

呼叫 `Startup.Configure` 方法中的 `UseBrowserLink`：

```csharp
app.UseBrowserLink();
```

`UseBrowserLink`調用通常放置在僅在開發環境中啟用`if`瀏覽器鏈接的塊內。 例如：

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

如需詳細資訊，請參閱 <xref:fundamentals/environments>。

## <a name="how-to-use-browser-link"></a>如何使用瀏覽器連結

開啟 ASP.NET 核心項目時,Visual Studio 會在**除錯目標**工具列控制者旁邊顯示瀏覽器連結工具列控制項:

![瀏覽器連結下拉選單](using-browserlink/_static/browserLink-dropdown-menu.png)

從瀏覽器連結工具列控制件中,您可以:

* 一次在多個瀏覽器中刷新 Web 應用。
* 開啟**瀏覽器連結儀表板**。
* 開啟或停用**瀏覽器連結**。 注意:默認情況下,在可視化工作室中禁用瀏覽器連結。
* 開啟或關閉[CSS 自動同步](#enable-or-disable-css-auto-sync)。

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>在多個瀏覽器中刷新 Web 應用

要選擇啟動項目時要啟動的單一網頁瀏覽器,請使用**除錯目標**工具列控制項中的下拉選單:

![F5 下拉選單](using-browserlink/_static/debug-target-dropdown-menu.png)

要同時開啟多個瀏覽器,請從同一下拉清單中選擇 **「使用...** 按住<kbd>Ctrl</kbd>鍵以選擇所需的瀏覽器,然後按下 **::**

![一次開啟多個瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

以下螢幕截圖顯示了開啟索引檢視和兩個打開瀏覽器的可視化工作室:

![與兩個瀏覽器同步範例](using-browserlink/_static/sync-with-two-browsers-example.png)

將滑鼠的移動在瀏覽器連結工具列控制程式上以檢視連接到專案的瀏覽器:

![暫停提示](using-browserlink/_static/hoover-tip.png)

變更索引檢視,按下瀏覽器連結刷新按鈕時,將更新所有連接的瀏覽器:

![瀏覽器同步到變更](using-browserlink/_static/browsers-sync-to-changes.png)

瀏覽器連結還適用於從 Visual Studio 外部啟動並導航到應用 URL 的瀏覽器。

### <a name="the-browser-link-dashboard"></a>瀏覽器連結儀表板

從瀏覽器連結下拉選單開啟**瀏覽器連結儀表板**視窗,以管理與開啟瀏覽器的連接:

![開啟瀏覽器連結-儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

如果沒有連接瀏覽器,則可以通過選擇 **「瀏覽器中的檢視」** 連結啟動非調試會話:

![瀏覽器連結-儀表板-無連線](using-browserlink/_static/browserlink-dashboard-no-connections.png)

否則,連接的瀏覽器將顯示每個瀏覽器顯示的頁面路徑:

![瀏覽器連結-儀錶板-雙連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

您還可以按一下單個瀏覽器名稱以僅刷新該瀏覽器。

### <a name="enable-or-disable-browser-link"></a>開啟或停用瀏覽器連結

禁用瀏覽器連結後重新啟用瀏覽器連結時,必須刷新瀏覽器以重新連接它們。

### <a name="enable-or-disable-css-auto-sync"></a>開啟或關閉 CSS 自動同步

啟用 CSS 自動同步後,當您對 CSS 檔進行任何更改時,將自動刷新已連接的瀏覽器。

## <a name="how-it-works"></a>運作方式

瀏覽器連結用於[SignalR](xref:signalr/introduction)在可視化工作室和瀏覽器之間創建通信通道。 啟用瀏覽器連結後,Visual Studio 充SignalR當多個用戶端(瀏覽器)可以連接到的伺服器。 瀏覽器連結還在ASP.NET核心請求管道中註冊中間件元件。 此元件向來自伺服器`<script>`的每個頁面請求注入特殊引用。 透過瀏覽器中選擇 **「檢視來源」** 並捲軸到標記內容`<body>`的末尾,可以查看文稿引用:

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

源檔未修改。 中間件元件動態注入腳本引用。

因為瀏覽器端代碼是所有 JAvaScript,因此它適用於所有支援的瀏覽器SignalR, 而無需瀏覽器外掛程式。
