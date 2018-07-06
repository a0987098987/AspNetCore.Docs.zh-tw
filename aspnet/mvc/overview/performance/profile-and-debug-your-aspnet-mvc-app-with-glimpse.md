---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: 分析與偵錯您的 ASP.NET MVC 應用程式，使用 Glimpse |Microsoft Docs
author: Rick-Anderson
description: Glimpse 是蓬勃發展及成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯及適用於 ASP.NET 的診斷資訊...
ms.author: aspnetcontent
ms.date: 03/26/2015
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 4d044217d6c33594b39747a765165b8702338027
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37808537"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>分析與偵錯您的 ASP.NET MVC 應用程式，使用 Glimpse
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> Glimpse 是蓬勃發展及成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和診斷資訊的 ASP.NET 應用程式。 它是一般安裝、 輕量級、 超快，並在每一頁底部顯示關鍵效能度量。 它可讓您向下切入至您的應用程式時您需要了解發生什麼情況在伺服器上。 Glimpse 提供更有價值的資訊我們建議您在您的開發週期，包括您的 Azure 測試環境中使用它。 雖然[Fiddler](http://www.telerik.com/fiddler)並[F-12 開發工具](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)提供用戶端檢視 Glimpse 並提供從伺服器的詳細的檢視。 本教學課程著重於使用初探 ASP.NET MVC 和 EF 的套件，但許多其他套件可供使用。 可能的話，我會連結至適當[Glimpse docs](http://getglimpse.com/Docs/)我協助維護。 Glimpse 是開放原始碼專案，您也可以參與的原始程式碼和文件。


- [安裝初探](#ig)
- [啟用 localhost 的初探](#eg)
- [[時間軸] 索引標籤](#Time)
- [模型繫結](#mb)
- [路由](#route)
- [在 Azure 上使用初探](#da)
- [其他資源](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>安裝初探

您可以從 NuGet 套件管理員主控台或從安裝 Glimpse**管理 NuGet 套件**主控台。 對於這個示範，我要安裝的 Mvc5 和 EF6 封裝：

![從 NuGet Dlg 安裝初探](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

搜尋*Glimpse.EF*

![從 NuGet 安裝 dlg Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

藉由選取**已安裝的套件**，您可以看到已安裝的初探相依模組：

![從 DLg 安裝 Glimpse 套件](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

下列命令會從套件管理員主控台安裝 Glimpse MVC5 和 EF6 的模組：

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>啟用 localhost 的初探

瀏覽至http://localhost:&lt; 連接埠 #&gt;/glimpse.axd，然後按一下<strong>開啟 Glimpse</strong>  按鈕。

![Glimpse axd-isapid-2.0-64 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

如果您有我的最愛列顯示，您可以拖放 Glimpse 按鈕並將它們新增為 bookmarklets:

![使用 Glimpse boookmarklets IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

您現在可以瀏覽您的應用程式，而**抬頭顯示器**（抬頭顯示器） 會顯示在頁面底部。

![抬頭顯示器 contact Manager 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[Glimpse 抬頭顯示器頁面](http://getglimpse.com/Docs/Heads-up-Display)詳述如上所示的計時資訊。 不顯眼的效能資料抬頭顯示器的顯示畫面可以立即通知您的問題-才能進入測試循環。 按一下&quot;g&quot;右上角顯示 Glimpse 面板：

![Glimpse 面板](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

在上圖中， [ 索引標籤執行](http://getglimpse.com/Docs/Execution-Tab)選取時，其中顯示管線中的動作和篩選條件的計時詳細資料。 您可以看到我[停止監看式篩選計時器](http://www.nuget.org/packages/StopWatch/)開始 管線階段 6。 雖然我的輕量計時器可以提供有用的設定檔/計時資料，它會遺漏所有的時間花費在記憶體授權，並呈現檢視。 您可以閱讀我在計時器[設定檔，然後在您的 ASP.NET MVC 應用程式，一直到 Azure 的時間](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)。 [索引標籤](http://getglimpse.com/Docs/Tabs)頁面提供每個索引標籤的詳細資訊的連結。

<a id="Time"></a>
## <a name="the-timeline-tab"></a>[時間軸] 索引標籤

我已修改 Tom Dykstra 傑出[EF 6/MVC 5 的教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)為下列程式碼中，變更為 instructors 控制器：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

上述程式碼可讓我以查詢字串來傳遞 (`eager`) 來控制 eager 或明確載入資料。 在下圖中，使用明確式載入和執行時間 頁面會顯示在載入每個註冊`Index`動作方法：

![Explicit Loading - 明確載入](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

下列程式碼中，指定積極式，且每個註冊會擷取之後`Index`檢視則稱為：

![立即指定](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

您可以暫留在某個時間區段，以取得詳細的計時資訊：

![暫留以查看詳細的計時](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>模型繫結

[模型繫結索引標籤](http://getglimpse.com/Docs/Model-Binding-Tab)提供豐富的資訊可協助您了解您的表單變數繫結的方式，以及為什麼某些未繫結如預期般。 下的圖顯示**嗎？** 圖示，您可以按一下以顯示該功能的初探說明頁面。

![一窺模型繫結檢視](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>路由

 Glimpse 路由索引標籤將可協助您偵錯和了解路由。 下圖中選取產品路由 （和會呈現綠色，一窺慣例）。 ![選取的產品名稱](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)路由條件約束、 區域和資料語彙基元也會顯示。 請參閱[Glimpse 路由](http://getglimpse.com/Docs/Routes-Tab)並[ASP.NET MVC 5 中的屬性路由](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)如需詳細資訊。 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>在 Azure 上使用初探

Glimpse 預設安全性原則只允許從本機主機顯示 Glimpse 資料。 您可以變更此安全性原則，讓您可以檢視這項資料 （例如在 Azure 上的 web 應用程式） 的遠端伺服器上。 針對在 Azure 上的測試環境，新增 反白顯示的標記到底部*web.confg*檔案以啟用 Glimpse:

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

透過此單獨的變更，任何使用者可以在遠端站台上查看 Glimpse 資料。 請考慮將上述的標記新增至發行設定檔，因此它只部署套用時使用的發行設定檔的 (程式比方說，是 Azure 的測試 proifle。)若要限制 Glimpse 資料，我們會新增`canViewGlimpseData`角色而只允許這個角色，才能檢視 Glimpse 資料的使用者。

移除的註解*GlimpseSecurityPolicy.cs*檔案，並變更[IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)從呼叫`Administrator`至`canViewGlimpseData`角色：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> 安全性-Glimpse 所提供的豐富資料可能會公開您的應用程式的安全性。 Microsoft 不用於生產應用程式上執行安全性稽核的初探。


在 新增角色的資訊，請參閱我[將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 web 應用程式部署至 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)教學課程。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [將使用成員資格、 OAuth 和 SQL Database 的安全 ASP.NET MVC 5 應用程式部署至 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [一窺組態](http://getglimpse.com/Docs/Configuration)-文件頁面上設定索引標籤、 執行階段原則、 記錄和更多功能。
