---
title: "在 ASP.NET Core 瀏覽器連結"
author: ncarandini
description: "Visual Studio 功能，連結一或多個網頁瀏覽器的開發環境"
keywords: "ASP.NET Core，瀏覽器連結 CSS 同步處理"
ms.author: riande
manager: wpickett
ms.date: 12/28/2016
ms.topic: article
ms.assetid: 11813d4c-3f8a-445a-b23b-e4a57d001abc
ms.technology: aspnet
ms.prod: asp.net-core
uid: client-side/using-browserlink
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 211dd5d03e6b8414e0b2ed3234d8970c92f72452
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-browser-link-in-aspnet-core"></a>在 ASP.NET Core 瀏覽器連結簡介 

由[Nicolò Carandini](https://github.com/ncarandini)， [Mike Wasson](https://github.com/MikeWasson)，和[Tom Dykstra](https://github.com/tdykstra)

瀏覽器連結是在 Visual Studio 開發環境和一或多個網頁瀏覽器之間的通訊通道所建立的功能。 您可以使用瀏覽器連結，以重新整理 web 應用程式在數個瀏覽器中的，這是適用於跨瀏覽器測試。

## <a name="browser-link-setup"></a>瀏覽器連結安裝

ASP.NET Core **Web 應用程式**專案範本在 Visual Studio 2015 和更新版本包含所需的瀏覽器連結的所有項目。

若要加入的專案，您建立使用 ASP.NET Core 的瀏覽器連結**空**或**Web API**範本，請遵循下列步驟：

1. 新增*Microsoft.VisualStudio.Web.BrowserLink.Loader*封裝 
2. 將組態程式碼中的加入*Startup.cs*檔案。

### <a name="add-the-package"></a>將封裝加入

由於這是 Visual Studio 功能，將封裝加入最簡單的方式是開啟**Package Manager Console** (**檢視 > 其他視窗 > Package Manager Console**)，然後執行下列命令：

```console
install-package Microsoft.VisualStudio.Web.BrowserLink.Loader
```

或者，您可以使用**NuGet 套件管理員**。  以滑鼠右鍵按一下專案名稱中的**方案總管] 中**，然後選擇 [**管理 NuGet 封裝**。 

![開啟 NuGet 封裝管理員](using-browserlink/_static/open-nuget-package-manager.png)

然後尋找並安裝封裝。

![新增封裝使用 NuGet 封裝管理員](using-browserlink/_static/add-package-with-nuget-package-manager.png)

### <a name="add-configuration-code"></a>將組態程式碼

開啟*Startup.cs*檔案，然後在`Configure`方法加入下列程式碼：

```csharp
app.UseBrowserLink();
```

該程式碼通常是內部`if`區塊，只在開發環境，可讓瀏覽器連結，如下所示：

[!code-csharp[Main](./using-browserlink/sample/BrowserLinkSample/src/BrowserLinkSample/Startup.cs?highlight=1,4&range=40-44)]

如需詳細資訊，請參閱[使用多個環境](../fundamentals/environments.md)。

## <a name="how-to-use-browser-link"></a>如何使用瀏覽器連結

當您開啟 ASP.NET Core 專案時，Visual Studio 顯示瀏覽器連結工具列控制項旁**偵錯目標**工具列控制項：

![瀏覽器連結下拉式功能表](using-browserlink/_static/browserLink-dropdown-menu.png)

從瀏覽器連結工具列控制項中，您可以：

- Web 應用程式在數個瀏覽器中的一次重新整理
- 開啟**瀏覽器連結儀表板**
- 啟用或停用**瀏覽器連結**
- 啟用或停用 CSS 自動同步處理

> [!NOTE]
> 某些 Visual Studio 外掛程式，最值得注意的是*Web 擴充功能組件 2015年*和*Web 擴充功能組件 2017年*、 瀏覽器連結提供擴充的功能，但與 ASP 不搭配使用的一些其他功能。.NET Core 專案。

## <a name="refresh-the-web-application-in-several-browsers-at-once"></a>Web 應用程式在數個瀏覽器中的一次重新整理

若要選擇單一網頁瀏覽器啟動專案時，使用下拉式功能表中的**偵錯目標**工具列控制項：

![F5 下拉式功能表](using-browserlink/_static/debug-target-dropdown-menu.png)

若要一次開啟多個瀏覽器，請選擇**與瀏覽...**相同的下拉式清單中。  按住 CTRL 鍵以選取您想，瀏的覽器，然後按一下 **瀏覽**:

![一次開啟許多瀏覽器](using-browserlink/_static/open-many-browsers-at-once.png)

以下是範例螢幕擷取畫面顯示與索引檢視的 Visual Studio 開啟和兩個開啟的瀏覽器：

![同步處理兩個瀏覽器範例](using-browserlink/_static/sync-with-two-browsers-example.png)

將滑鼠停留在瀏覽器連結工具列控制項，若要查看連接到專案的瀏覽器：

![將滑鼠停留提示](using-browserlink/_static/hoover-tip.png)

變更索引檢視，並按一下 瀏覽器連結的 重新整理 按鈕時，系統會更新所有已連線的瀏覽器：

![瀏覽器-sync-到-變更](using-browserlink/_static/browsers-sync-to-changes.png)

瀏覽器連結也可以搭配您從 Visual Studio 外部啟動，並瀏覽至應用程式 URL 的瀏覽器。

### <a name="the-browser-link-dashboard"></a>瀏覽器連結儀表板

從瀏覽器連結下拉式功能表來管理與開啟的瀏覽器連線，請開啟瀏覽器連結儀表板：

![開啟-browserslink 儀表板](using-browserlink/_static/open-browserlink-dashboard.png)

如果瀏覽器不處於連接狀態，您可以啟動非偵錯工作階段按一下_瀏覽器中的檢視_連結：

![browserlink 儀表板-無連接](using-browserlink/_static/browserlink-dashboard-no-connections.png)

否則，已連線的瀏覽器，以顯示每個瀏覽器顯示頁面的路徑：

![browserlink 儀表板的兩個連接](using-browserlink/_static/browserlink-dashboard-two-connections.png)

如果需要，您可以按一下列出的瀏覽器的名稱，以重新整理該單一瀏覽器。

### <a name="enable-or-disable-browser-link"></a>啟用或停用瀏覽器連結

當您重新啟用瀏覽器連結停用它之後時，您必須重新整理瀏覽器再重新連線。

### <a name="enable-or-disable-css-auto-sync"></a>啟用或停用 CSS 自動同步處理

啟用 CSS 自動同步處理時，CSS 檔案中進行任何變更時，會自動重新整理連接的瀏覽器。

## <a name="how-does-it-work"></a>它的運作狀況如何？

瀏覽器連結會使用 SignalR 建立 Visual Studio 和瀏覽器之間的通訊通道。 啟用瀏覽器連結時，Visual Studio 就會當做多個用戶端 （瀏覽器使用） 可以連接到的 SignalR 伺服器。 瀏覽器連結也會註冊 ASP.NET 要求管線中的中介軟體元件。 此元件會插入特殊`<script>`到每個頁面要求來自伺服器的參考。 您可以看到選取的指令碼參考**檢視原始檔**瀏覽器和向下捲動到結尾`<body>`標記的內容：

```javascript
    <!-- Visual Studio Browser Link -->
    <script type="application/json" id="__browserLink_initializationData">
        {"requestId":"a717d5a07c1741949a7cefd6fa2bad08","requestMappingFromServer":false}
    </script>
    <script type="text/javascript" src="http://localhost:54139/b6e36e429d034f578ebccd6a79bf19bf/browserLink" async="async"></script>
    <!-- End Browser Link -->
</body>
```

不會修改原始程式檔。 中介軟體元件會以動態方式插入指令碼參考。 

因為瀏覽器端程式碼是所有 JavaScript，它適用於所有瀏覽器支援 SignalR，而不需要任何瀏覽器外掛程式。
