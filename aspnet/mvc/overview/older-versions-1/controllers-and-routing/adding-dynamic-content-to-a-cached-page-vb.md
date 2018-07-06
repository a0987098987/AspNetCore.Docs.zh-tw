---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
title: 將動態內容新增至快取的頁面 (VB) |Microsoft Docs
author: microsoft
description: 了解如何混合使用動態和快取的內容，在相同的頁面。 快取後替代作業可讓您顯示橫幅廣告 o 之類的動態內容...
ms.author: aspnetcontent
ms.date: 01/27/2009
ms.assetid: 68acd884-fb57-4486-a1be-aaa93e380780
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-vb
msc.type: authoredcontent
ms.openlocfilehash: 802b712cffd0584d984a1b17e8b6b0c70c3db7c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37832178"
---
<a name="adding-dynamic-content-to-a-cached-page-vb"></a>將動態內容新增至快取的頁面 (VB)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何混合使用動態和快取的內容，在相同的頁面。 快取後替代作業可讓您顯示動態內容，例如橫幅廣告或新聞項目，內有已輸出快取的頁面。


利用輸出快取，您可以大幅改善 ASP.NET MVC 應用程式的效能。 而不是重新產生頁面每一次要求頁面時，網頁可以產生一次並快取在記憶體中的多個使用者。

但還有一個問題。 如果您需要在網頁中顯示動態內容嗎？ 例如，假設您想要在網頁中顯示橫幅廣告。 您不想要快取，讓每個使用者會看到相同的廣告橫幅廣告。 您不會如此一來開始任何賺錢 ！

幸運的是，沒有簡單的解決方案。 您可以利用呼叫 ASP.NET framework 的功能*快取後替代*。 快取後替代作業可讓您取代已經在記憶體中快取的頁面中的動態內容。


一般來說，您在輸出時使用快取頁面&lt;OutputCache&gt;屬性，頁面會快取伺服器和用戶端 （網頁瀏覽器） 上。 當您使用快取後替代時，則是只能在伺服器上快取頁面。


#### <a name="using-post-cache-substitution"></a>使用快取後替代

使用快取後替代作業需要兩個步驟。 首先，您需要定義傳回字串，表示您想要在快取的頁面中顯示的動態內容的方法。 接下來，您可以呼叫 HttpResponse.WriteSubstitution() 方法，將插入頁面的動態內容。

例如，假設您想要隨機快取的頁面中顯示不同的新聞項目。 列表 1 中的類別會公開單一方法，名為 RenderNews() 隨機的三個新項目清單會傳回一個新聞項目。

**列表 1 – Models\News.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample1.vb)]

若要利用快取後替代，您可以呼叫 HttpResponse.WriteSubstitution(); 方法。 WriteSubstitution() 方法會設定以取代動態內容的快取的頁面區域的程式碼。 WriteSubstitution() 方法用來在 列表 2 中的檢視中顯示隨機的新聞項目。

**列表 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample2.aspx)]

RenderNews 方法會傳遞至 WriteSubstitution() 方法。 請注意，不會呼叫 RenderNews 方法。 改為參考的方法被傳遞至 WriteSubstitution() AddressOf 運算子的協助。

[索引] 檢視會快取。 列表 3 中的控制站會傳回檢視。 請注意，index （） 動作以裝飾&lt;OutputCache&gt;會導致快取為 60 秒的 [索引] 檢視的屬性。

**列表 3 – Controllers\HomeController.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample3.vb)]

即使索引檢視會快取，當您要求 [索引] 頁面時，會顯示不同的隨機新聞項目。 當您要求 索引 頁面時，頁面所顯示的時間就不會變更 （請參閱 圖 1） 的 60 秒。 時間不會變更的事實證明，頁面會快取。 不過，將內容插入 WriteSubstitution() 方法 – 隨機的新聞項目 – 變更每個要求。

**圖 1 – 插入快取的頁面中的動態的新聞項目**

![clip_image002](adding-dynamic-content-to-a-cached-page-vb/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>協助程式方法中使用快取後替代

利用快取後替代作業更簡單的方法是封裝 WriteSubstitution() 方法，在自訂的協助程式方法的呼叫。 Helper 方法，在 列表 4 說明這種方法。

**列表 4 – Helpers\AdHelper.vb**

[!code-vb[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample4.vb)]

列表 4 包含會公開兩種方法的 Visual Basic 模組： RenderBanner() 和 RenderBannerInternal()。 RenderBanner() 方法代表實際的 helper 方法。 此方法會擴充標準的 ASP.NET MVC HtmlHelper 類別，可讓您檢視就像任何其他的 helper 方法中呼叫 Html.RenderBanner()。

RenderBanner() 方法呼叫將 RenderBannerInternal() 方法傳遞至 WriteSubsitution() 方法 HttpResponse.WriteSubstitution() 方法。

RenderBannerInternal() 方法是私用方法。 這個方法不會公開為 helper 方法。 隨機 RenderBannerInternal() 方法會從三個橫幅廣告映像的清單傳回一個橫幅廣告影像。

修改過的索引檢視表 5 中會說明如何使用 RenderBanner() helper 方法。 請注意，額外&lt;匯入 %@ %&gt;指示詞就會包含要匯入 MvcApplication1.Helpers 命名空間之檢視的頂端。 如果您不願匯入此命名空間，RenderBanner() 方法將不再顯示為 Html 屬性中的方法。

**列表 5 – Views\Home\Index.aspx （含 RenderBanner() 方法）**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-vb/samples/sample5.aspx)]

當您要求表 5] 中檢視轉譯的頁面時，不同的橫幅廣告會顯示與每個要求 （請參閱 [圖 2）。 頁面會快取，但橫幅廣告插入動態 RenderBanner() helper 方法。

**圖 2 – 顯示隨機橫幅廣告的 索引 檢視**

![clip_image004](adding-dynamic-content-to-a-cached-page-vb/_static/image2.jpg)

#### <a name="summary"></a>總結

本教學課程說明如何以動態方式更新快取的頁面中的內容。 您已了解如何使用 HttpResponse.WriteSubstitution() 方法來啟用快取的頁面中插入的動態內容。 您也學到如何封裝內的 HTML 協助程式方法 WriteSubstitution() 方法的呼叫。

利用快取盡可能 – 它可以對 web 應用程式的效能有顯著的影響。 本教學課程中所述，您可以利用快取即使在您需要在頁面中顯示動態內容。

> [!div class="step-by-step"]
> [上一頁](improving-performance-with-output-caching-vb.md)
> [下一頁](creating-a-controller-vb.md)
