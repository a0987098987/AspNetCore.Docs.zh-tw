---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: 建立可讀取的 Url，在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: tfitzmac
description: 本文說明在 ASP.NET Web Pages (Razor) 網站，然後如何讓您使用更容易讀取與更好的 SEO 的 Url 路由。 將項目方法...
ms.author: riande
ms.date: 02/17/2014
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: b8405283dc5bf44a4cd8d1122d327346774d95e8
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831400"
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web Pages (Razor) 網站中建立可讀取的 Url
====================
藉由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明在 ASP.NET Web Pages (Razor) 網站，然後如何讓您使用更容易讀取與更好的 SEO 的 Url 路由。
> 
> 您將學到什麼：
> 
> - ASP.NET 如何使用路由可讓您使用更可讀取且可搜尋的 Url。
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a>在本教學課程中使用的軟體版本
> 
> 
> - ASP.NET Web Pages (Razor) 3
>   
> 
> 本教學課程也適用於 ASP.NET Web Pages 2。


## <a name="about-routing"></a>關於路由

中您的網站頁面的 Url 可能會影響網站運作的程度。 URL 的&quot;易記&quot;可以比較方便其他人使用的站台。 它也有助於與站台的搜尋引擎最佳化 (SEO)。 ASP.NET 網站能夠自動使用易記的 Url。

ASP.NET 可讓您建立有意義描述使用者動作，而非只指向伺服器上的檔案的 Url。 請考慮這些 Url 虛構的部落格：

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

請比較以下的這些 Url:

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

在第一組中，使用者必須知道使用顯示部落格*blog.cshtml*頁面，然後就必須建構查詢字串，取得正確的分類或日期範圍。 第二組範例會更容易理解和建立。

第一個範例的 Url 也直接指向特定檔案 (*blog.cshtml*)。 如果基於某些原因部落格已移至另一個資料夾的伺服器上，或部落格已重寫，以使用不同的頁面，則會發生錯誤的連結。 第二個集合的 Url 並未指向特定的頁面上，因此，即使部落格實作或位置變更時，Url 仍會有效。

ASP.NET Web Pages 中，您可以建立更容易使用的 Url，類似上述範例中，因為 ASP.NET 會使用*路由*。 路由會建立邏輯的對應，從頁面 （或頁面） 可以完成要求的 URL。 因為對應邏輯 （不是實體，對特定檔案），路由會提供絕佳的彈性，在您如何為您的網站定義的 Url。

## <a name="how-routing-works"></a>路由的運作方式

當 ASP.NET 處理要求時，它會讀取 URL，以判斷如何將它路由傳送。 ASP.NET 會嘗試比對的 URL，檔案在磁碟上，我們從左到右的個別區段。 如果沒有相符項目，在 URL 中所剩餘的任何項目傳遞至與頁面*路徑資訊*。

想像一下，有人會使用此 URL 的要求：

`http://www.contoso.com/a/b/c`

搜尋會像這樣：

1. 是否有檔案的路徑和名稱 */a/b/c.cshtml*嗎？ 若是如此，請執行該頁面，並將任何資訊傳遞給它。 否則...
2. 是否有檔案的路徑和名稱 */a/b.cshtml*嗎？ 如果是，執行該頁面上，將值傳遞`c`給它。 否則...
3. 是否有檔案的路徑和名稱 */a.cshtml*嗎？ 如果是，執行該頁面上，將值傳遞`b/c`給它。

如果找到不精確的搜尋相符 *.cshtml*其指定的資料夾中的檔案，ASP.NET 將繼續依序尋找這些檔案：

1. */a/b/c/default.cshtml* （沒有路徑資訊）。
2. */a/b/c/index.cshtml* （沒有路徑資訊）。

> [!NOTE]
> 若要明確地針對特定頁面的要求 (也就是所提出的要求 *.cshtml*副檔名) 運作就跟您預期的方式一樣。 要求喜歡`http://www.contoso.com/a/b.cshtml`會執行頁面*b.cshtml*沒有問題。


在頁面上，您可以取得透過頁面的路徑資訊`UrlData`屬性，這是一個字典。 假設您有一個名為檔案*ViewCustomers.cshtml*和您的網站取得這項要求：

`http://mysite.com/myWebSite/ViewCustomers/1000`

上述規則中所述，要求將會移至您的頁面。 在頁面上，您可以使用如下的程式碼，取得並顯示路徑資訊 (在此情況下，值&quot;1000年&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> 由於路由不需要用到完整的檔案名稱，因此可能會模稜兩可有具有相同的頁面副檔名但不同的檔案名稱 (例如*MyPage.cshtml*並*MyPage.html*). 若要避免發生路由問題，最好是確定您不需要在您的網站名稱的差別只在於其延伸模組頁面。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

[WebMatrix-Url、 UrlData 和路由 seo](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)。 由 Mike Brind 此部落格文章提供路由的運作方式 ASP.NET Web Pages 中的其他詳細資料。
