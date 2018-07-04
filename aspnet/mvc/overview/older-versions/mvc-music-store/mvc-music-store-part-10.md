---
uid: mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
title: 第 10 節： 導覽及網站設計，結論的最後更新 |Microsoft Docs
author: jongalloway
description: 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 部分 10 涵蓋最後更新瀏覽和 S...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/21/2011
ms.topic: article
ms.assetid: 0c6e4c2f-fcdb-4978-9656-1990c6f15727
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-10
msc.type: authoredcontent
ms.openlocfilehash: a5b1d02d268530d6288ed2bc502679336d06d403
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37366316"
---
<a name="part-10-final-updates-to-navigation-and-site-design-conclusion"></a>第 10 節： 最後更新導覽及網站設計，結論
====================
藉由[Jon Galloway](https://github.com/jongalloway)

> MVC Music 市集是介紹，並逐步說明如何使用 ASP.NET MVC 和 Visual Studio 進行 web 開發的教學課程應用程式。  
>   
> MVC Music 市集是銷售線上音樂 album 和實作基本的網站管理、 使用者登入時，和 「 購物車 」 功能的輕量級的範例存放區實作。  
>   
> 本教學課程系列會詳細說明所有建置 ASP.NET MVC Music 市集範例應用程式所採取的步驟。 部分 10 涵蓋導覽及網站設計，結論的最後更新。


我們已經完成我們的網站中，所有主要的功能，但我們仍有一些功能新增到網站巡覽、 首頁上，以及存放區瀏覽 頁面。

## <a name="creating-the-shopping-cart-summary-partial-view"></a>建立購物車摘要部分檢視

我們想要公開 （expose） 至整個網站的使用者的購物車中的項目數。

![](mvc-music-store-part-10/_static/image1.png)

我們可以輕鬆實作這藉由建立部分加入至我們 Site.master 檢視。

如先前所示，ShoppingCart 控制器就會包含 CartSummary 動作方法，此方法會傳回部分檢視：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample1.cs)]

若要建立 CartSummary 部分檢視，以滑鼠右鍵按一下 [檢視/ShoppingCart] 資料夾，並選取 [新增檢視]。 檢視 CartSummary 命名，並選取 [建立部分檢視] 核取方塊，如下所示。

![](mvc-music-store-part-10/_static/image2.png)

CartSummary 部分檢視是很簡單-它是只連結至 ShoppingCart 索引 檢視會顯示購物車中的項目數目。 CartSummary.cshtml 的完整程式碼如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample2.cshtml)]

我們可以在站台，包括站台主機，使用 Html.RenderAction 方法中的任何網頁包含部分檢視。 RenderAction 需要我們指定動作名稱 ("CartSummary") 和控制器名稱 ("ShoppingCart") 做為下面。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample3.cshtml)]

之前將它新增到網站的版面配置中，我們也會建立 [內容類型] 功能表中讓我們可以將所有我們 Site.master 更新一次。

## <a name="creating-the-genre-menu-partial-view"></a>建立部分檢視的內容類型 功能表

我們可以讓我們來瀏覽存放區中，藉由新增內容類型 功能表會列出所有的內容類型可以使用我們的存放區中的使用者輕鬆許多。

![](mvc-music-store-part-10/_static/image3.png)

我們將遵循相同步驟也會建立 GenreMenu 部分檢視，並接著我們可以加入這兩個網站主版。 首先，加入 StoreController 下列 GenreMenu 控制器動作：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample4.cs)]

此動作會傳回部分的檢視中，接下來，我們將建立會顯示的內容類型清單。

*注意： 我們已新增 [ChildActionOnly] 屬性，此控制器動作，這表示我們只想在部分檢視中使用此動作。這個屬性會防止執行瀏覽至 /Store/GenreMenu 控制器動作。這不是必要的部分檢視，但良好的做法，因為我們想要確定我們的控制器動作做為我們想。我們也要傳回 PartialView，而不是檢視，可讓檢視引擎知道它不應該針對此檢視中，使用配置為包含在其他檢視中。*

以滑鼠右鍵按一下 GenreMenu 控制器動作，並建立名為 GenreMenu 它強型別，如下所示，使用內容類型的檢視資料類別的部分檢視。

![](mvc-music-store-part-10/_static/image4.png)

更新要使用的未排序的清單，如下所示的項目顯示的 GenreMenu 部分檢視的檢視程式碼。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample5.cshtml)]

## <a name="updating-site-layout-to-display-our-partial-views"></a>若要顯示我們的部分檢視更新的網站配置

我們可以將我們的部分檢視新增至網站配置 (/檢視/Shared/\_Layout.cshtml) 藉由呼叫 Html.RenderAction()。 我們將新增兩者，以及一些其他的標記，以顯示它們，如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample6.cshtml)]

現在當我們執行應用程式時，我們會看到左邊的導覽區域中的內容類型和購物車摘要頂端。

## <a name="update-to-the-store-browse-page"></a>更新存放區瀏覽的頁面

存放區瀏覽 頁面可運作，但是看起來很理想。 我們可以更新頁面以顯示專輯中的較佳的版面配置更新檢視程式碼 （/Views/Store/Browse.cshtml 中找到），如下所示：

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample7.cshtml)]

這裡我們正在進行，讓我們可以套用的連結，以包含專輯作品的特殊格式，其中使用了 Url.Action，而不是 Html.ActionLink 使用。

*注意： 我們會顯示這些相簿的泛型的專輯封面。這項資訊儲存在資料庫中，並編輯透過存放區管理員。您可以將您自己的作品。*

現在當我們瀏覽至內容類型，我們會看到具有專輯作品的方格中顯示的 album。

![](mvc-music-store-part-10/_static/image5.png)

## <a name="updating-the-home-page-to-show-top-selling-albums"></a>更新以顯示前銷售相簿的首頁

我們想要我們最暢銷的專輯來增加銷售量的首頁上的功能。 我們要處理，並新增一些額外的圖形我們 HomeController 進行部分更新。

首先，我們將新增的導覽屬性到專輯類別，讓 EntityFramework 可讓您知道它們相關聯。 最後幾行我們**專輯**類別現在看起來應該像這樣：

[!code-csharp[Main](mvc-music-store-part-10/samples/sample8.cs)]

*注意： 這將需要新增 using 陳述式，以納入 System.Collections.Generic 命名空間。*

首先，我們將新增 storeDB 欄位和 MvcMusicStore.Models using 陳述式，如同我們的其他控制站。 接下來，我們會將下列方法加入 HomeController 查詢來尋找最上層的銷售專輯 OrderDetails 根據資料庫。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample9.cs)]

這會是私用的方法，因為我們不想要使它成為控制器動作。 我們會將它包含在 HomeController，為了簡單起見，但建議您使用將您的商務邏輯移至 視需要個別的服務類別。

有了它之後，我們可以更新查詢前 5 項暢銷專輯，並將其傳回檢視的索引控制器動作。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample10.cs)]

更新 HomeController 的完整程式碼如下所示。

[!code-csharp[Main](mvc-music-store-part-10/samples/sample11.cs)]

最後，我們需要更新首頁索引檢視，使它可以藉由更新的模型型別，並加入底部的 [專輯] 清單中顯示一份專輯。 我們能利用這個機會來也加入至頁面的 標題 」 和 「 升級 > 一節。

[!code-cshtml[Main](mvc-music-store-part-10/samples/sample12.cshtml)]

現在當我們執行應用程式時，我們會看到我們已更新的首頁上，最上層的銷售專輯和促銷的訊息。

![](mvc-music-store-part-10/_static/image1.jpg)

## <a name="conclusion"></a>結論

我們已了解，ASP.NET MVC 可讓您輕鬆建立複雜的網站與資料庫存取權，成員資格，AJAX，等等。 很快。 希望本教學課程提供具有您要開始建置您自己的 ASP.NET MVC 應用程式的工具 ！


> [!div class="step-by-step"]
> [上一步](mvc-music-store-part-9.md)
