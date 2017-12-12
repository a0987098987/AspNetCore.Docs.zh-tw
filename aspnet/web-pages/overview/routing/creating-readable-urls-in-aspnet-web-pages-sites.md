---
uid: web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
title: "建立可讀取的 Url 中 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本文說明在 ASP.NET Web Pages (Razor) 網站，然後這如何可讓您使用更容易讀取與 seo 更好的 Url 路由。 您將的會..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/17/2014
ms.topic: article
ms.assetid: a8aac1ac-89de-4415-afe0-97a41c6423d2
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/routing/creating-readable-urls-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 7858b7cbd6dccafb2867ed9a1d102561e211e435
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-readable-urls-in-aspnet-web-pages-razor-sites"></a>在 ASP.NET Web Pages (Razor) 網站中建立可讀取的 Url
====================
由[Tom FitzMacken](https://github.com/tfitzmac)

> 本文說明在 ASP.NET Web Pages (Razor) 網站，然後這如何可讓您使用更容易讀取與 seo 更好的 Url 路由。
> 
> 您將學習：
> 
> - ASP.NET 如何使用路由可讓您使用更具可讀性和可搜尋的 Url。
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

您的網站中頁面的 Url 可能會影響網站運作的程度。 URL 的&quot;易記&quot;更容易讓人使用的站台。 它也會協助搜尋引擎最佳化 (SEO) 站台。 ASP.NET 網站包括自動使用易記 Url 的能力。

ASP.NET 可讓您建立有意義描述使用者動作，而不是指向伺服器上的檔案的 Url。 請考量這些 Url 虛構的部落格：

- `http://www.contoso.com/Blog/blog.cshtml?categories=hardware`
- `http://www.contoso.com//Blog/blog.cshtml?startdate=2009-11-01&enddate=2009-11-30`

比較這些 Url 的下列項目：

- `http://www.contoso.com/Blog/categories/hardware/`
- `http://www.contoso.com/Blog/2009/November`

在第一對中，使用者必須知道部落格會顯示使用*blog.cshtml*頁面，然後就必須建構取得正確的類別目錄或日期範圍的查詢字串。 第二組範例會比較容易理解，並建立項目。

第一個範例中的 Url 也直接指向特定的檔案 (*blog.cshtml*)。 如果基於某些原因部落格已移至另一個資料夾的伺服器上，或已改寫部落格，以使用不同的網頁，連結就會發生錯誤。 第二個一組 Url 未指向特定的頁面上，因此，即使部落格實作或位置變更時，Url 仍然會有效。

ASP.NET Web Pages 中，您可以建立更容易使用的 Url，類似上述範例中，因為 ASP.NET 會使用*路由*。 路由與可以完成要求的 URL 頁面 （或頁面） 中建立邏輯對應。 因為對應邏輯 （不是實體，特定檔案），路由會提供更多如何為您的網站定義 Url 的彈性。

## <a name="how-routing-works"></a>路由的運作方式

當 ASP.NET 處理要求時，它會讀取 URL，以判斷如何將它路由傳送。 ASP.NET 會嘗試比對個別檔案在磁碟上，會從左到右的 URL 區段。 如果沒有相符項目，在 URL 中所剩餘的任何項目會傳遞至與頁面*路徑資訊*。

假設有其他人會使用此 URL 的要求：

`http://www.contoso.com/a/b/c`

搜尋會像這樣：

1. 是否有檔案路徑和名稱*/a/b/c.cshtml*嗎？ 如果是這樣，執行該頁面，並將任何資訊傳遞給它。 否則...
2. 是否有檔案路徑和名稱*/a/b.cshtml*嗎？ 因此，執行該頁面，並將值傳遞如果`c`給它。 否則...
3. 是否有檔案路徑和名稱*/a.cshtml*嗎？ 因此，執行該頁面，並將值傳遞如果`b/c`給它。

如果找到不精確的搜尋相符項目如*.cshtml*依次尋找這些檔案會指定的資料夾中的檔案，ASP.NET 繼續：

1. */a/b/c/default.cshtml* （沒有路徑資訊）。
2. */a/b/c/index.cshtml* （沒有路徑資訊）。

> [!NOTE]
> 若要很清楚地，對特定網頁的要求 (也就是所提出的要求*.cshtml*副檔名) 運作方式就如同您預期的。 要求喜歡`http://www.contoso.com/a/b.cshtml`會執行頁面*b.cshtml*很好。


在頁面上，就可以透過網頁的路徑資訊`UrlData`屬性，這是字典。 假設您有名稱為的檔案*ViewCustomers.cshtml*和您的網站取得此要求：

`http://mysite.com/myWebSite/ViewCustomers/1000`

上述規則所述，要求將會移至您的頁面。 在頁面上，您可以使用下列程式碼取得並顯示路徑資訊 (在此情況下，值&quot;1000年&quot;):

[!code-html[Main](creating-readable-urls-in-aspnet-web-pages-sites/samples/sample1.html)]

> [!NOTE]
> 因為路由不包含完整檔案名稱，所以可以有模稜兩可有具有相同的頁面名稱但不同的檔案名稱副檔名 (例如， *MyPage.cshtml*和*MyPage.html*). 為了避免發生路由問題，所以最好先確定您不需要頁面在您的網站名稱差異只在於其擴充功能。


<a id="Additional_Resources"></a>
## <a name="additional-resources"></a>其他資源

[WebMatrix-Url、 UrlData 和路由 seo](http://www.mikesdotnetting.com/Article/165/WebMatrix-URLs-UrlData-and-Routing-for-SEO)。 Mike Brind 此部落格文章提供如何路由適用於 ASP.NET Web Pages 中的其他詳細資料。
