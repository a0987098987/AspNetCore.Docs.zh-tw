---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
title: ASP.NET MVC Routing 概觀 (VB) |Microsoft 文件
author: StephenWalther
description: 在本教學課程中，作者： Stephen Walther 會顯示 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作的方式。
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/19/2008
ms.topic: article
ms.assetid: 4bc8d19a-80f1-44b4-adbf-95ed22d691ca
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-vb
msc.type: authoredcontent
ms.openlocfilehash: 3de0e21552a4aa03aa21f21a4e26028f1475f3e9
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-mvc-routing-overview-vb"></a>ASP.NET MVC Routing 概觀 (VB)
====================
由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，作者： Stephen Walther 會顯示 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作的方式。


在本教學課程中，您可以引入呼叫每個 ASP.NET MVC 應用程式的重要功能*ASP.NET 路由*。 ASP.NET 路由模組負責將連入的瀏覽器要求對應至特定 MVC 控制器的動作。 本教學課程結束時，您將了解標準的路由表如何將要求對應到控制器的動作。

## <a name="using-the-default-route-table"></a>使用預設路由表

當您建立新的 ASP.NET MVC 應用程式時，應用程式被設定為使用 ASP.NET 路由。 ASP.NET 路由是在兩個地方的安裝程式。

首先，ASP.NET 路由中已啟用您的應用程式的 Web 組態檔 （Web.config 檔案）。 有四個區段的組態檔中與路由相關： system.web.httpModules 區段、 system.web.httpHandlers 區段、 system.webserver.modules 區段中和 system.webserver.handlers > 一節。 請小心不要刪除這些區段，因為這些區段不路由將無法再運作。

第二個，而更重要的是，應用程式的 Global.asax 檔案中建立路由表。 Global.asax 檔案是特殊的檔案，其中包含 ASP.NET 應用程式生命週期事件的事件處理常式。 應用程式的啟動事件期間建立的路由表。

程式碼範例 1 中的檔案包含 ASP.NET MVC 應用程式的預設 Global.asax 檔。

**列出 1-Global.asax.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample1.vb)]

MVC 應用程式第一次啟動時，應用程式\_呼叫 start （） 方法。 這個方法，又呼叫 RegisterRoutes() 方法。 RegisterRoutes() 方法建立路由表。

預設路由表包含單一的路由 （名為預設值）。 預設路由將 URL 的第一個區段對應至控制器名稱、 控制器的動作，URL 的第二個區段和名為參數的第三個區段**識別碼**。

假設您在網頁瀏覽器的網址列輸入下列 URL:

/Home/Index/3

預設路由會將這個 URL 對應到下列參數：

- 控制器 = Home

- 動作 = 索引

- id = 3

當您要求 URL /Home/索引/3 時，會執行下列程式碼：

HomeController.Index(3)

預設路由包含所有的三個參數的預設值。 如果您並未提供控制器、 控制器參數的預設值的值**首頁**。 如果您並未提供動作，action 參數預設為值**索引**。 最後，如果您未提供的識別碼，id 參數的預設值為空字串。

讓我們看看如何預設路由將 Url 對應至控制器動作的一些範例。 假設您的瀏覽器網址列輸入下列 URL:

/ 首頁

由於預設路由參數預設值，輸入此 URL 會導致 HomeController 類別的 index （） 方法的清單 2 來呼叫。

**列出 2-HomeController.vb**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample2.vb)]

在列出 2 中，HomeController 類別包含名為 index （） 可接受名為識別碼的單一參數的方法URL /Home 會導致 index （） 方法呼叫的值做為 Id 參數的值執行任何動作。

MVC 架構會叫用非同步控制器動作的方式，因為 URL /Home 也會比對中列出的 3 HomeController 類別的 index （） 方法。

**列出 3-HomeController.vb （沒有參數的索引動作）**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample3.vb)]

範例 3 中的 index （） 方法不接受任何參數。 URL /Home 會導致這個 index （） 方法來呼叫。 URL /Home/索引/3 也會叫用這個方法 （識別碼會被忽略）。

URL /Home 也會比對中列出的 4 HomeController 類別的 index （） 方法。

**列出 4-HomeController.vb （具有可為 null 的參數索引動作）**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample4.vb)]

在列出 4 中，index （） 方法會有一個整數參數。 因為參數是可為 null 的參數 （可以具有值執行任何動作），index （） 可以呼叫而不會引發錯誤。

最後，叫用 5 與 URL /Home 中的 index （） 方法會發生例外狀況，因為 Id 參數*不*可為 null 的參數。 如果您嘗試叫用 index （） 方法時您會取得在圖 1 顯示的錯誤。

**列出 5-HomeController.vb （使用 Id 參數的索引動作）**

[!code-vb[Main](asp-net-mvc-routing-overview-vb/samples/sample5.vb)]


[![叫用控制器動作，預期的參數值](asp-net-mvc-routing-overview-vb/_static/image1.jpg)](asp-net-mvc-routing-overview-vb/_static/image1.png)

**圖 01**： 叫用控制器動作，預期的參數值 ([按一下以檢視完整大小的影像](asp-net-mvc-routing-overview-vb/_static/image2.png))


URL /Home/索引/3，相反地，具有索引控制器動作，列出 5 中正常運作。 要求 /Home/Index/3 使 index （） 方法用值 3 Id 參數來呼叫。

## <a name="summary"></a>總結

本教學課程的目標是為您提供 ASP.NET 路由的簡介。 我們會檢查您取得新的 ASP.NET MVC 應用程式的預設路由表。 您已學習如何預設路由將 Url 對應至控制器的動作。

> [!div class="step-by-step"]
> [上一頁](creating-an-action-cs.md)
> [下一頁](understanding-action-filters-vb.md)
