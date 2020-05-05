---
title: ASP.NET Core 中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結是一種 Visual Studio 功能，可將開發環境與一或多個網頁瀏覽器連結在一起。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 01/09/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: client-side/using-browserlink
ms.openlocfilehash: 619d19ba90298b2455d4a558fea138c86a751f07
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773653"
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET Core 中的瀏覽器連結

By [Nicolò Carandini](https://github.com/ncarandini)、 [Mike Wasson](https://github.com/MikeWasson)和[Tom 作者: dykstra](https://github.com/tdykstra)

瀏覽器連結是 Visual Studio 功能。 它會在開發環境與一或多個網頁瀏覽器之間建立通道。 您可以使用瀏覽器連結，一次在數個瀏覽器中重新整理您的 web 應用程式，這對於跨瀏覽器測試很有用。

## <a name="browser-link-setup"></a>瀏覽器連結設定

::: moniker range=">= aspnetcore-3.0"

將[BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件新增至您的專案。 針對 ASP.NET Core Razor頁面或 MVC 專案，也請依照中Razor <xref:mvc/views/view-compilation>的說明，啟用（*cshtml*）檔案的執行時間編譯。 Razor只有在已啟用執行時間編譯時，才會套用語法變更。

::: moniker-end

::: moniker range=">= aspnetcore-2.1 <= aspnetcore-2.2"

將 ASP.NET Core 2.0 專案轉換成 ASP.NET Core 2.1 並轉換成[AspNetCore 應用程式](xref:fundamentals/metapackage-app)時，請安裝 VisualStudio 瀏覽器連結功能的[BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)套件。 ASP.NET Core 2.1 專案範本預設會使用`Microsoft.AspNetCore.App`中繼套件。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **Web 應用程式**、**空**的和**Web API**專案範本都會使用[AspNetCore 中繼套件](xref:fundamentals/metapackage)，其中包含[VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)的套件參考。 因此，使用`Microsoft.AspNetCore.All`中繼套件不需要進一步的動作，即可讓瀏覽器連結可供使用。

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **Web 應用程式**專案範本具有 BrowserLink 套件的套件參考。（ [VisualStudio](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/) ）。 其他專案類型則需要您將套件參考新增至`Microsoft.VisualStudio.Web.BrowserLink`。

::: moniker-end

### <a name="configuration"></a>設定

呼叫 `Startup.Configure` 方法中的 `UseBrowserLink`：

```csharp
app.UseBrowserLink();
```

`UseBrowserLink`呼叫通常會放在只在`if`開發環境中啟用瀏覽器連結的區塊中。 例如：

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

如需詳細資訊，請參閱<xref:fundamentals/environments>。

## <a name="how-to-use-browser-link"></a>如何使用瀏覽器連結

當您開啟 ASP.NET Core 專案時，Visual Studio 會在 [ **Debug Target** ] 工具列控制項旁顯示瀏覽器連結工具列控制項：

![瀏覽器連結下拉式功能表](using-browserlink/_static/browserLink-dropdown-menu.png)

從瀏覽器連結工具列控制項，您可以：

* 一次在數個瀏覽器中重新整理 web 應用程式。
* 開啟**瀏覽器連結儀表板**。
* 啟用或停用**瀏覽器連結**。 注意： Visual Studio 中預設會停用瀏覽器連結。
* 啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)處理。

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>一次在數個瀏覽器中重新整理 web 應用程式

若要在啟動專案時選擇要啟動的單一 web 瀏覽器，請使用 [ **Debug Target** ] 工具列控制項中的下拉式功能表：

![F5 下拉式功能表](using-browserlink/_static/debug-target-dropdown-menu.png)

若要一次開啟多個瀏覽器，請從相同的下拉式選單選擇 **[流覽方式 ...]** 。 按住<kbd>Ctrl</kbd>鍵以選取您要的瀏覽器，然後按一下 **[流覽]**：

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

下列螢幕擷取畫面顯示索引視圖開啟和兩個開啟的瀏覽器的 Visual Studio：

![與兩個瀏覽器同步處理範例](using-browserlink/_static/sync-with-two-browsers-example.png)

將滑鼠停留在瀏覽器連結工具列控制項上，以查看已連接到專案的瀏覽器：

![暫留提示](using-browserlink/_static/hoover-tip.png)

變更 [索引] 視圖，當您按一下瀏覽器連結的 [重新整理] 按鈕時，就會更新所有連線的瀏覽器：

![瀏覽器-同步至變更](using-browserlink/_static/browsers-sync-to-changes.png)

瀏覽器連結也適用于從外部 Visual Studio 啟動的瀏覽器，並流覽至應用程式 URL。

### <a name="the-browser-link-dashboard"></a>瀏覽器連結儀表板

從瀏覽器連結下拉式功能表開啟**瀏覽器連結儀表板**視窗，以管理與開放式瀏覽器的連接：

![開啟-browserslink-儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

如果沒有連接的瀏覽器，您可以藉由選取 [**在瀏覽器中查看**] 連結來啟動非偵錯工具的會話：

![browserlink-儀表板-無連接](using-browserlink/_static/browserlink-dashboard-no-connections.png)

否則，會顯示已連線的瀏覽器，其中包含每個瀏覽器所顯示頁面的路徑：

![browserlink-儀表板-兩個連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

您也可以按一下個別的瀏覽器名稱，只重新整理該瀏覽器。

### <a name="enable-or-disable-browser-link"></a>啟用或停用瀏覽器連結

停用瀏覽器連結之後，您必須重新整理瀏覽器，才能重新連線。

### <a name="enable-or-disable-css-auto-sync"></a>啟用或停用 CSS 自動同步處理

啟用 CSS 自動同步處理時，會在您對 CSS 檔案進行任何變更時，自動重新整理連接的瀏覽器。

## <a name="how-it-works"></a>運作方式

瀏覽器連結[SignalR](xref:signalr/introduction)會使用來建立 Visual Studio 與瀏覽器之間的通道。 啟用瀏覽器連結時，Visual Studio 會作為多SignalR個用戶端（瀏覽器）可以連接的伺服器。 瀏覽器連結也會在 ASP.NET Core 要求管線中註冊中介軟體元件。 此元件會從`<script>`伺服器插入每個頁面要求的特殊參考。 您可以在瀏覽器中選取 [ **View source** ]，並將它滾動到`<body>`標記內容的結尾，以查看腳本參考：

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

您的原始程式檔不會遭到修改。 中介軟體元件會動態插入腳本參考。

因為瀏覽器端程式碼是所有的 JavaScript，所以它適用于所有SignalR支援的瀏覽器，而不需要瀏覽器外掛程式。
