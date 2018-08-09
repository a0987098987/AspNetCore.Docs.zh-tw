---
title: ASP.NET Core 中的瀏覽器連結
author: ncarandini
description: 說明瀏覽器連結如何連結一或多個網頁瀏覽器的開發環境的 Visual Studio 功能。
ms.author: riande
ms.custom: H1Hack27Feb2017
ms.date: 09/22/2017
uid: client-side/using-browserlink
ms.openlocfilehash: 5ab15c841c472e6c9d47bad70fcf5e0c6dc3010f
ms.sourcegitcommit: ea7ec8d47f94cfb8e008d771f647f86bbb4baa44
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/06/2018
ms.locfileid: "37894175"
---
# <a name="browser-link-in-aspnet-core"></a>ASP.NET Core 中的瀏覽器連結

藉由[Nicolò Carandini](https://github.com/ncarandini)， [Mike Wasson](https://github.com/MikeWasson)，和[Tom Dykstra](https://github.com/tdykstra)

瀏覽器連結是在 Visual Studio 中建立開發環境與一或多個網頁瀏覽器之間的通訊通道的功能。 您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。

## <a name="browser-link-setup"></a>瀏覽器連結設定

::: moniker range=">= aspnetcore-2.1"

將 ASP.NET Core 2.0 專案轉換成 ASP.NET Core 2.1 並移轉到時[Microsoft.AspNetCore.App 中繼套件](xref:fundamentals/metapackage-app)，安裝[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)封裝BrowserLink 功能。 ASP.NET Core 2.1 專案範本會使用`Microsoft.AspNetCore.App`預設的中繼套件。

::: moniker-end

::: moniker range="= aspnetcore-2.0"

ASP.NET Core 2.0 **Web 應用程式**，**空白**，並**Web API**專案範本會使用[Microsoft.AspNetCore.All 中繼套件](xref:fundamentals/metapackage)其中包含的套件參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)。 因此，使用`Microsoft.AspNetCore.All`中繼套件不需要任何進一步的動作，若要使瀏覽器連結可供使用。

::: moniker-end

::: moniker range="<= aspnetcore-1.1"

ASP.NET Core 1.x **Web 應用程式**專案範本有的套件參考[Microsoft.VisualStudio.Web.BrowserLink](https://www.nuget.org/packages/Microsoft.VisualStudio.Web.BrowserLink/)封裝。 **空**或是**Web API**範本專案會要求您新增的套件參考`Microsoft.VisualStudio.Web.BrowserLink`。

因為這是 Visual Studio 功能，最簡單的方式將封裝加入**空**或是**Web API**範本專案是以開啟**Package Manager Console** (**檢視** > **其他 Windows** > **Package Manager Console**)，然後執行下列命令：

```console
install-package Microsoft.VisualStudio.Web.BrowserLink
```

或者，您可以使用**NuGet 套件管理員**。 中的專案名稱上按一下滑鼠右鍵**方案總管**，然後選擇**管理 NuGet 套件**:

![開啟 NuGet 套件管理員](using-browserlink/_static/open-nuget-package-manager.png)

尋找並安裝套件：

![新增套件使用 NuGet 封裝管理員](using-browserlink/_static/add-package-with-nuget-package-manager.png)

::: moniker-end

### <a name="configuration"></a>組態

在 `Startup.Configure` 方法中：

```csharp
app.UseBrowserLink();
```

程式碼通常是內部`if`區塊可讓瀏覽器連結只在開發環境中，如下所示：

```csharp
if (env.IsDevelopment())
{
    app.UseDeveloperExceptionPage();
    app.UseBrowserLink();
}
```

如需詳細資訊，請參閱[使用多重環境](xref:fundamentals/environments)。

## <a name="how-to-use-browser-link"></a>如何使用瀏覽器連結

當您有 ASP.NET Core 專案開啟時，Visual Studio 顯示瀏覽器連結工具列控制項旁**偵錯目標**工具列控制項：

![瀏覽器連結下拉式選單](using-browserlink/_static/browserLink-dropdown-menu.png)

從瀏覽器連結 工具列控制項中，您可以：

* 重新整理 web 應用程式在數個瀏覽器中的一次。
* 開啟**瀏覽器連結儀表板**。
* 啟用或停用**瀏覽器連結**。 注意： 根據預設，Visual Studio 2017 (15.3) 中，停用瀏覽器連結。
* 啟用或停用[CSS 自動同步](#enable-or-disable-css-auto-sync)。

> [!NOTE]
> 某些 Visual Studio 外掛程式，最值得注意的是*Web 延伸模組組件 2015年*並*Web 延伸模組組件 2017年*，針對瀏覽器連結，提供擴充的功能，但某些額外的功能不適用於 ASP.NET Core 專案。

## <a name="refresh-the-web-app-in-several-browsers-at-once"></a>一次重新整理在數個瀏覽器中的 web 應用程式

若要啟動啟動專案時選擇單一網頁瀏覽器，請使用**偵錯目標**工具列控制項內的下拉式選單：

![F5 下拉式選單](using-browserlink/_static/debug-target-dropdown-menu.png)

若要一次開啟多個瀏覽器，請從相同的下拉式清單選擇 **瀏覽...** 。 按住 CTRL 鍵以選取您想要的瀏覽器，然後按一下 **瀏覽**:

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

以下是顯示 Visual Studio 開啟 Index 檢視和兩個開啟瀏覽器的螢幕擷取畫面：

![同步處理兩個瀏覽器範例](using-browserlink/_static/sync-with-two-browsers-example.png)

將滑鼠停留在瀏覽器連接工具控制項以查看連接到專案的瀏覽器：

![暫留時的秘訣](using-browserlink/_static/hoover-tip.png)

按一下瀏覽器連結重新整理按鈕時，將變更 Index 檢視並更新所有連線的瀏覽器：

![瀏覽器-同步處理-至-變更](using-browserlink/_static/browsers-sync-to-changes.png)

瀏覽器連結也可以搭配您從 Visual Studio 外部啟動，然後瀏覽至應用程式 URL 的瀏覽器。

### <a name="the-browser-link-dashboard"></a>瀏覽器連結儀表板

從瀏覽器連結 下拉式功能表來管理連線開啟的瀏覽器中開啟瀏覽器連結儀表板：

![開啟-browserslink 儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

如果連線沒有瀏覽器，您可以選取來啟動非偵錯工作階段*瀏覽器中的檢視*連結：

![browserlink-dashboard-no-connections](using-browserlink/_static/browserlink-dashboard-no-connections.png)

否則，連線的瀏覽器會顯示每個瀏覽器會顯示頁面的路徑：

![browserlink-dashboard-two-connections](using-browserlink/_static/browserlink-dashboard-two-connections.png)

如有需要，您可以按一下列出的瀏覽器的名稱，以重新整理該單一瀏覽器。

### <a name="enable-or-disable-browser-link"></a>啟用或停用瀏覽器連結

當您重新啟用瀏覽器連結停用它之後時，您必須重新整理瀏覽器再重新連線。

### <a name="enable-or-disable-css-auto-sync"></a>啟用或停用 CSS 自動同步處理

啟用 CSS 自動同步處理時，CSS 檔案中進行任何變更時，會自動重新整理連線的瀏覽器。

## <a name="how-it-works"></a>它的運作方式

瀏覽器連結會使用 SignalR 來建立 Visual Studio 和瀏覽器之間的通訊通道。 啟用瀏覽器連結時，Visual Studio 就會作為多個用戶端 （瀏覽器） 可以連線到 SignalR 伺服器。 瀏覽器連結也會註冊在 ASP.NET 要求管線的中介軟體元件。 此元件會插入特殊`<script>`到每一個網頁要求從伺服器中的參考。 您可以選取，以查看指令碼參考**檢視原始檔**在瀏覽器，並向下捲動到結尾`<body>`標記的內容：

```html
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

您的原始程式檔不會被修改。 中介層元件會以動態方式插入指令碼參考。

由於瀏覽器端的程式碼全部是 JavaScript，它適用於所有 SignalR 支援的瀏覽器，而不需要瀏覽器外掛程式。
