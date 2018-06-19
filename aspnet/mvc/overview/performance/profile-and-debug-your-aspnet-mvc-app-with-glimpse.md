---
uid: mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
title: 設定檔，然後使用 glimpse 進行 ASP.NET MVC 應用程式進行偵錯 |Microsoft 文件
author: Rick-Anderson
description: 初探是蓬勃發展和成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和 ASP.NET 的診斷資訊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/26/2015
ms.topic: article
ms.assetid: c205805f-efdd-4fa7-9616-f26eab180611
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/performance/profile-and-debug-your-aspnet-mvc-app-with-glimpse
msc.type: authoredcontent
ms.openlocfilehash: 6ac23256c57116de81c7bf690d5ce743301c75ce
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872971"
---
<a name="profile-and-debug-your-aspnet-mvc-app-with-glimpse"></a>分析和偵錯使用 glimpse 進行 ASP.NET MVC 應用程式
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 初探是蓬勃發展和成長系列的開放原始碼 NuGet 套件，提供詳細的效能、 偵錯和診斷資訊的 ASP.NET 應用程式。 它是 trivial 安裝、 輕量級超快，並在每一頁底部顯示關鍵效能度量。 它可讓您向下鑽研至您的應用程式時您需要了解在伺服器上運作。 初探提供許多有用的資訊我們建議您在整個開發週期，包括 Azure 測試環境使用它。 雖然[Fiddler](http://www.telerik.com/fiddler)和[F-12 開發工具](https://msdn.microsoft.com/library/ie/gg589512(v=vs.85).aspx)提供用戶端 檢視中，瀏覽提供從伺服器的詳細的檢視。 本教學課程著重於使用 glimpse 進行 ASP.NET MVC 和 EF 封裝，但是許多其他封裝可以使用。 盡可能將連結至適當[以前並未使用過的文件](http://getglimpse.com/Docs/)這可協助維護。 初探開放原始碼專案，您太可以參與原始碼及文件。


- [安裝初探](#ig)
- [啟用初探 localhost](#eg)
- [[時間軸] 索引標籤](#Time)
- [模型繫結](#mb)
- [路由](#route)
- [在 Azure 上使用 glimpse 進行](#da)
- [其他資源](#addRes)

<a id="ig"></a>
## <a name="installing-glimpse"></a>安裝初探

您可以在從 NuGet 封裝管理員主控台或從安裝初探**管理 NuGet 封裝**主控台。 對於這個示範，我要安裝的 Mvc5 和 EF6 封裝：

![從 NuGet Dlg 安裝初探](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image1.png)

搜尋*Glimpse.EF*

![從 NuGet 安裝新細明體 Glimpse.EF](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image2.png)

藉由選取**安裝封裝**，您可以查看已安裝的 Glimpse 相依模組：

![新細明體從已安裝瀏覽封裝](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image3.png)

下列命令會從封裝管理員主控台安裝初探 MVC5 和 EF6 模組：

[!code-console[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample1.cmd)]

<a id="eg"></a>
## <a name="enable-glimpse-for-localhost"></a>啟用初探 localhost

瀏覽至http://localhost:&lt;連接埠 #&gt;/glimpse.axd，然後按一下<strong>開啟初探上</strong> 按鈕。

![初探 axd-isapid-2.0-64 頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image4.png)

如果您有我的最愛列顯示，您可以拖曳和卸除的瀏覽按鈕並將其新增為 bookmarklets:

![使用 glimpse 進行 boookmarklets IE](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image5.png)

您現在可以瀏覽您的應用程式和**抬頭顯示器**（抬頭顯示器） 會顯示在頁面底部。

![抬頭顯示器與連絡人管理員頁面](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image6.png)

[初探抬頭顯示器頁面](http://getglimpse.com/Docs/Heads-up-Display)詳細說明以上所示的計時資訊。 不顯眼的效能資料抬頭顯示器顯示會通知您發生問題的立即-之前測試週期。 按一下&quot;g&quot;在右下角會顯示初探面板：

![初探面板](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image7.png)

在上圖中，[執行 索引標籤](http://getglimpse.com/Docs/Execution-Tab)選取時，會顯示在管線中的動作和篩選的計時詳細資料。 您可以看到我[停止監看式篩選器計時器](http://www.nuget.org/packages/StopWatch/)在管線階段 6 開始。 雖然我輕量型計時器可以提供有用的設定檔計時資料，它會遺漏所有時間花在授權和呈現檢視。 您可以閱讀我在計時器[分析和時間 ASP.NET MVC 應用程式到 Azure](https://blogs.msdn.com/b/webdev/archive/2014/07/29/profile-and-time-your-asp-net-mvc-app-all-the-way-to-azure.aspx)。 [索引標籤](http://getglimpse.com/Docs/Tabs)頁面每個索引標籤會提供詳細資訊連結。

<a id="Time"></a>
## <a name="the-timeline-tab"></a>[時間軸] 索引標籤

修改了 Tom Dykstra 尚未處理完畢[EF 6 MVC 5 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)為下列程式碼變更講師控制站：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample2.cs?highlight=1,20-31)]

上述程式碼可讓我傳入查詢字串 (`eager`) 來控制 eager 或明確載入的資料。 在下面的影像中，會使用明確式載入和計時 頁面會顯示在載入每個註冊`Index`動作方法：

![Explicit Loading - 明確載入](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image8.png)

下列程式碼中，指定 eager，且每個註冊會擷取之後`Index`檢視則稱為：

![eager 會指定](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image9.png)

您可以將滑鼠停留在時間區段，以取得詳細的計時資訊：

![若要查看詳細的計時的動態顯示](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image10.png)

<a id="mb"></a>
## <a name="model-binding"></a>模型繫結

[模型繫結索引標籤](http://getglimpse.com/Docs/Model-Binding-Tab)提供豐富的資訊可協助您了解您的表單變數的繫結的方式，以及為什麼有些不會繫結所預期。 下的圖顯示**嗎？** 圖示，您可以按一下以顯示該功能的瀏覽說明頁面。

![繫結檢視的瀏覽模型](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image11.png)

<a id="route"></a>
## <a name="routes"></a>路由

 初探路由索引標籤將可協助您偵錯和了解路由。 在下列影像中，選取產品路由，並會呈現綠色，初探慣例。 ![選取的產品名稱](profile-and-debug-your-aspnet-mvc-app-with-glimpse/_static/image12.png)路由條件約束、 區域和資料語彙基元也會顯示。 請參閱[初探路由](http://getglimpse.com/Docs/Routes-Tab)和[屬性路由在 ASP.NET MVC 5](https://blogs.msdn.com/b/webdev/archive/2013/10/17/attribute-routing-in-asp-net-mvc-5.aspx)如需詳細資訊。 

<a id="da"></a>
## <a name="using-glimpse-on-azure"></a>在 Azure 上使用 glimpse 進行

初探預設的安全性原則只允許從本機主機要顯示的資料瀏覽。 您可以變更此安全性原則，讓您可以檢視這項資料 （例如在 Azure 上的 web 應用程式） 的遠端伺服器上。 對於在 Azure 上的測試環境，新增到底部的反白顯示的標記*web.confg*檔案，以啟用瀏覽：

[!code-xml[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample3.xml?highlight=2-6)]

此單獨的變更，任何使用者可以看到遠端站台上的瀏覽資料。 請考慮將上方的標記加入至發行設定檔，因此有只部署套用時使用的發行設定檔的 (程式例如，是 Azure 測試 proifle。)若要限制瀏覽資料，我們將加入`canViewGlimpseData`角色並只允許使用者在此角色，以檢視瀏覽資料。

移除從註解*GlimpseSecurityPolicy.cs*檔案，並且變更[IsInRole](https://msdn.microsoft.com/library/system.security.principal.iprincipal.isinrole(v=vs.110).aspx)從呼叫`Administrator`至`canViewGlimpseData`角色：

[!code-csharp[Main](profile-and-debug-your-aspnet-mvc-app-with-glimpse/samples/sample4.cs?highlight=6)]

> [!WARNING]
> 安全性-瀏覽所提供的豐富資料可能會公開您的應用程式的安全性。 Microsoft 不用於生產應用程式上執行安全性稽核的 Glimpse。


如需加入角色的資訊，請參閱我[成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 web 應用程式部署至 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)教學課程。

<a id="addRes"></a>
## <a name="additional-resources"></a>其他資源

- [將成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 5 應用程式部署至 Azure](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-aspnet-mvc-app-membership-oauth-sql-database/)
- [以前並未設定使用過](http://getglimpse.com/Docs/Configuration)-設定索引標籤中，執行階段原則、 記錄和多個文件頁面。
