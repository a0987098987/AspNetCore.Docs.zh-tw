---
uid: mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
title: ASP.NET MVC 路由概觀 (C#) |Microsoft Docs
author: StephenWalther
description: 在本教學課程中，Stephen Walther 會示範如何 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作。
ms.author: aspnetcontent
ms.date: 08/19/2008
ms.assetid: 5b39d2d5-4bf9-4d04-94c7-81b84dfeeb31
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/asp-net-mvc-routing-overview-cs
msc.type: authoredcontent
ms.openlocfilehash: 2b43c3dca1587d79a60c5a89a451dda989403d56
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37819143"
---
<a name="aspnet-mvc-routing-overview-c"></a>ASP.NET MVC 路由概觀 (C#)
====================
藉由[Stephen Walther](https://github.com/StephenWalther)

> 在本教學課程中，Stephen Walther 會示範如何 ASP.NET MVC 架構將瀏覽器要求對應至控制器動作。


您會在本教學課程中，引進了一項重要功能的每個呼叫的 ASP.NET MVC 應用程式*ASP.NET 路由*。 ASP.NET 路由模組負責將傳入的瀏覽器要求對應至特定的 MVC 控制器動作。 本教學課程結束時，您將了解標準的路由表如何將要求對應至控制器動作。

## <a name="using-the-default-route-table"></a>使用預設路由表

當您建立新的 ASP.NET MVC 應用程式時，應用程式已設定要使用 ASP.NET 路由。 ASP.NET 路由是在兩個位置的設定。

首先，ASP.NET 路由中已啟用您的應用程式的 Web 組態檔 （Web.config 檔案中）。 有四個組態檔中的區段與路由相關： system.web.httpModules 區段、 system.web.httpHandlers 區段、 system.webserver.modules 區段中和 system.webserver.handlers 一節。 請小心不要刪除這些區段，因為這些區段不路由將無法再運作。

第二，以及更重要的是，應用程式的 Global.asax 檔中建立路由表。 Global.asax 檔案是特殊的檔案，其中包含 ASP.NET 應用程式生命週期事件的事件處理常式。 應用程式啟動的事件期間，會建立路由表。

列表 1 中的檔案包含 ASP.NET MVC 應用程式的預設 Global.asax 檔案。

**列表 1-Global.asax.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample1.cs)]

MVC 應用程式第一次啟動時，應用程式\_呼叫 start （） 方法。 接著，此方法中，會呼叫 RegisterRoutes() 方法。 RegisterRoutes() 方法會建立路由表。

預設路由表包含單一的路由 （名為預設值）。 預設路由會將 URL 的第一個區段對應至控制器名稱、 控制器動作，URL 的第二個區段和到名稱為參數的第三個區段**識別碼**。

假設您在網頁瀏覽器的網址列輸入下列 URL:

/ Home/Index/3

預設路由會將此 URL 對應到下列參數：

- 控制器 = 首頁

- 動作 = 索引

- id = 3

當您要求 URL /Home/索引/3 時，則會執行下列程式碼：

HomeController.Index(3)

預設路由包含所有的三個參數的預設值。 如果您未提供的控制器，控制器參數的預設值的值**首頁**。 如果您未提供的動作，將 action 參數會預設為值**Index**。 最後，如果您未提供 id，id 參數的預設值為空字串。

讓我們看看幾個範例的預設路由至控制器動作所對應的 Url。 想像一下您的瀏覽器網址列輸入下列 URL:

/ 首頁

由於預設路由參數預設值，輸入此 URL 會導致 HomeController 類別的 index （） 方法呼叫的列表 2。

**列表 2-HomeController.cs**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample2.cs)]

在 列表 2，HomeController 類別包含一個名為接受單一參數，名為 id 的 index （） 方法URL /Home 會導致 index （） 方法呼叫取代為空字串，做為 Id 參數的值。

MVC 架構會叫用控制器動作的方式，因為 URL /Home 也會比對 HomeController 類別，在 列表 3 中的 index （） 方法。

**列表 3-HomeController.cs （不含參數的索引動作）**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample3.cs)]

在 列表 3 中的 index （） 方法不接受任何參數。 URL /Home 會呼叫這個 index （） 方法。 URL /Home/索引/3 也會叫用這個方法 （識別碼會被忽略）。

URL /Home 也會比對 HomeController 類別，在 列表 4 中的 index （） 方法。

**列表 4-HomeController.cs （使用可為 null 的參數索引動作）**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample4.cs)]

在列表 4 中，index （） 方法會有一個整數參數。 因為參數是可為 null 的參數 （可以有 Null 值），可以呼叫 index （），而不會引發錯誤。

最後，叫用 [表 5] 中使用 URL /Home index （） 方法會導致例外狀況之後的 Id 參數*不是*可為 null 的參數。 如果您嘗試叫用 index （） 方法，您會取得顯示在 圖 1 中的錯誤。

**列表 5-HomeController.cs （使用 Id 參數的索引動作）**

[!code-csharp[Main](asp-net-mvc-routing-overview-cs/samples/sample5.cs)]


[![叫用控制器動作，以參數值](asp-net-mvc-routing-overview-cs/_static/image1.jpg)](asp-net-mvc-routing-overview-cs/_static/image1.png)

**圖 01**： 叫用控制器動作，以參數值 ([按一下以檢視完整大小的影像](asp-net-mvc-routing-overview-cs/_static/image2.png))


URL /Home/索引/3，相反地，會使用索引控制器動作，在 列表 5 中正常運作。 要求 /Home/Index/3 會導致使用 Id 參數，其值為 3 會呼叫 index （） 方法。

## <a name="summary"></a>總結

本教學課程的目標是為您提供 ASP.NET 路由的簡介。 我們會檢查您取得新的 ASP.NET MVC 應用程式的預設路由表。 您已了解預設路由至控制器動作所對應的 Url。

> [!div class="step-by-step"]
> [下一步](understanding-action-filters-cs.md)
