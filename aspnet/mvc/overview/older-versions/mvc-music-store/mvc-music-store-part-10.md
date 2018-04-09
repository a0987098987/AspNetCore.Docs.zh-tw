---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 組件 10： 瀏覽及站台的設計，結論最後更新 |Microsoft 文件
author: jongalloway
description: 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 組件 10 涵蓋最後更新瀏覽和 S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: b40d194c4d08f3564da59bacde4b5d3d7663373a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>瀏覽及站台的設計，結論的組件 10： 最後更新
====================
由[Jon Galloway](https://github.com/jongalloway)

> MVC Music Store 是介紹並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 針對 web 程式開發的教學課程應用程式。  
>   
> MVC Music Store 是輕量級範例存放區實作銷售音樂專輯上線，並實作基本的網站管理、 使用者登入和購物車的功能。  
>   
> 此教學課程系列詳細列出所有建置 ASP.NET MVC 商店範例應用程式所採取的步驟。 組件 10 涵蓋瀏覽及站台的設計，結論最後的更新。


我們已經完成的所有主要功能為我們的網站，但我們仍有一些功能，可新增至站台瀏覽、 首頁和存放區瀏覽 頁面。

## <a name="creating-the-shopping-cart-summary-partial-view"></a>建立購物車摘要部分檢視

我們想要在整個網站中公開使用者的購物車中的項目數。

![](mvc-music-store-part-10/_static/image1.png)

我們可以輕鬆實作這藉由建立部分加入至我們 Site.master 檢視。

如先前所示，ShoppingCart 控制器包含 CartSummary 動作方法會傳回部分檢視：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

若要建立 CartSummary 部分檢視，檢視/ShoppingCart 資料夾上按一下滑鼠右鍵並選取 [新增檢視]。 CartSummary 檢視的名稱，並檢查 「 建立部分檢視 」 的核取方塊，如下所示。

![](mvc-music-store-part-10/_static/image2.png)

真正簡易 CartSummary 部分檢視-它是只 ShoppingCart 索引檢視中顯示的項目數購物車中的連結。 CartSummary.cshtml 的完整程式碼如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

我們可以併入任何在網站中，使用 Html.RenderAction 方法包括網站主版頁面的部分檢視。 RenderAction 需要我們指定動作名稱 ("CartSummary") 和控制器名稱 ("ShoppingCart") 做為下面。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

之前加入這個站台版面配置，我們也會建立內容類型功能表讓我們進行的所有我們 Site.master 更新一次。

## <a name="creating-the-genre-menu-partial-view"></a>建立部分檢視的內容類型 功能表

我們可以讓我們來瀏覽市集，藉由新增內容類型 功能表會列出所有內容類型可以使用我們的存放區中的使用者來得簡單許多。

![](mvc-music-store-part-10/_static/image3.png)

我們會遵循相同步驟也會建立 GenreMenu 部分檢視，並將我們可以加入兩者主要站台。 首先，加入下列 GenreMenu 控制器動作 StoreController:

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

這個動作會傳回一份這會顯示由部分檢視，接下來，我們將建立的內容類型。

*注意： 我們加入了 [ChildActionOnly] 屬性此控制器動作，這表示我們只想要從部分檢視使用此動作。這個屬性會防止從正在執行瀏覽至 /Store/GenreMenu 控制器動作。這不是必要的部分檢視，但最好的作法是，因為我們想要確定我們想在使用我們的控制器動作。我們也會傳回 PartialView，而不是檢視，可讓檢視引擎知道它不應該使用這個檢視中，配置為包含在其他檢視中。*

以滑鼠右鍵按一下 GenreMenu 控制器動作，並建立名為 GenreMenu 其強型別使用的內容類型 檢視資料類別，如下所示的部分檢視。

![](mvc-music-store-part-10/_static/image4.png)

更新要使用的未排序的清單，如下所示的項目顯示的 GenreMenu 部分檢視的檢視程式碼。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>若要顯示我們的部分檢視更新的站台版面配置

我們可以加入我們的部分檢視的網站配置 (/檢視/共用/\_Layout.cshtml) 藉由呼叫 Html.RenderAction()。 我們會將兩者中，以及一些額外的標記，以顯示它們，如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

現在當我們執行應用程式時，我們將會看到左邊的導覽區域中的內容類型和購物車摘要頂端。

## <a name="update-to-the-store-browse-page"></a>更新資料存放區瀏覽頁

存放區瀏覽 頁面可運作，但是看起來很理想。 我們可以更新頁面以顯示更好的版面配置中的專輯，藉由更新檢視程式碼 （/Views/Store/Browse.cshtml 中找到），如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

這裡我們正在使用 Url.Action，而不是 Html.ActionLink，讓我們可以套用特殊格式以包含專輯圖檔連結。

*注意： 我們會顯示這些相簿的泛型專輯封面。此資訊儲存在資料庫中，並透過存放區管理員可加以編輯。您可以隨意地加入您自己的作品。*

現在當我們瀏覽至內容類型，我們將會看到具有專輯作品的方格中顯示的相簿。

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>更新以顯示頂端銷售專輯首頁

我們想要的功能我們最暢銷的專輯在首頁上，來增加銷售量。 我們要處理，並新增一些其他的圖形以及我們 HomeController 進行部分更新。

首先，我們會加入導覽屬性專輯類別以便 EntityFramework 知道它們的相關聯。 最後幾行程式我們**專輯**類別現在看起來應該像這樣：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*注意： 這將需要加入 using System.Collections.Generic 命名空間中的陳述式。*

首先，我們會將 storeDB 欄位以及 MvcMusicStore.Models using 陳述式，如同我們控制站。 接下來，我們會將下列方法加入 HomeController 會查詢資料庫以尋找 OrderDetails 根據最上層銷售的相簿。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

因為我們不想要使它成為控制器動作，這是私用方法。 我們為了簡單起見，HomeController 中加入它，但建議您使用成個別的服務類別，適當地移動您的商務邏輯。

具有該位置中，我們可以更新查詢前 5 項暢銷相簿，並將其傳回檢視的索引控制器動作。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

更新 HomeController 的完整程式碼如下所示。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

最後，我們需要更新我們的首頁索引檢視，使它可以顯示一份專輯更新的模型型別，並新增專輯清單底部。 我們將利用這個機會也新增至頁面的標題和升級一節。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

現在當我們執行應用程式時，我們會看到我們更新的首頁上方銷售專輯和我們促銷的訊息。

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>結論

我們已看到，ASP.NET MVC 可讓您輕鬆建立複雜的網站與資料庫存取權，成員資格、 AJAX 等。 很快。 希望本教學課程已提供您要開始建置您自己的 ASP.NET MVC 應用程式所需的工具 ！


> [!div class="step-by-step"]
> [上一步](mvc-music-store-part-9.md)
