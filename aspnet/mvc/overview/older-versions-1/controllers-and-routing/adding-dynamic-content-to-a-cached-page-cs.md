---
uid: mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
title: 將動態內容加入至快取的頁面 (C#) |Microsoft 文件
author: microsoft
description: 了解如何混用動態和快取的內容，在相同的頁面。 快取後替換可讓您顯示橫幅廣告 o 之類的動態內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/27/2009
ms.topic: article
ms.assetid: 2ddd4407-d143-4a94-877c-21771bfb97a6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/controllers-and-routing/adding-dynamic-content-to-a-cached-page-cs
msc.type: authoredcontent
ms.openlocfilehash: 9f91cc07bc531cfb3edf577ab79e91fd94a57a3c
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30868577"
---
<a name="adding-dynamic-content-to-a-cached-page-c"></a>將動態內容加入至快取的頁面 (C#)
====================
by [Microsoft](https://github.com/microsoft)

> 了解如何混用動態和快取的內容，在相同的頁面。 快取後替換可讓您顯示橫幅廣告或具有已輸出快取網頁內的新聞項目，例如的動態內容。


藉由運用輸出快取，您可以大幅改善 ASP.NET MVC 應用程式的效能。 而是重新產生頁面，每次要求頁面時，可以產生一次並快取在記憶體中的多個使用者 頁面。

但有問題。 如果您需要在頁面中顯示動態內容嗎？ 例如，假設您想要在頁面中顯示橫幅廣告。 您不想要快取，讓每個使用者會看到相同的廣告橫幅廣告。 您不會進行任何 money 如此 ！

幸運的是，沒有簡單的解決方案。 您可以利用一項功能稱為 ASP.NET framework 的*快取後替換*。 快取後替換可讓您以取代在記憶體中快取的頁面中的動態內容。


一般來說，當您使用 [OutputCache] 屬性來輸出快取的頁面，頁面會快取伺服器和用戶端 （web 瀏覽器） 中。 當您使用快取後替換時，則是只能在伺服器上快取頁面。


#### <a name="using-post-cache-substitution"></a>使用快取後替換

使用快取後替換需要兩個步驟。 首先，您需要定義傳回字串，表示您想要在快取的頁面中顯示的動態內容的方法。 接下來，您呼叫 HttpResponse.WriteSubstitution() 方法來插入頁面的動態內容。

例如，假設您想要隨機快取的頁面中顯示不同的新聞項目。 程式碼範例 1 中的類別會公開單一方法，名為 RenderNews() 隨機傳回一個新聞項目清單中的 3 個新聞項目。

**列出 1 – Models\News.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample1.cs)]

若要充分利用快取後替換，您可以呼叫 HttpResponse.WriteSubstitution() 方法。 WriteSubstitution() 方法會設定要取代動態內容的快取的頁面區域的程式碼。 WriteSubstitution() 方法用來顯示的檢視清單 2 中的隨機新聞項目。

**列出 2 – Views\Home\Index.aspx**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample2.aspx)]

RenderNews 方法會傳遞至 WriteSubstitution() 方法。 請注意 RenderNews 方法不會呼叫 （有沒有括號）。 不過此方法的參考傳遞至 WriteSubstitution()。

索引檢視會快取。 檢視會傳回所列出的 3 中的控制站。 請注意，index （） 動作會造成快取為 60 秒的索引檢視的 [OutputCache] 屬性裝飾。

**列出 3 – Controllers\HomeController.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample3.cs)]

即使索引檢視會快取，當您要求的索引頁面時，會顯示不同的隨機新聞項目。 當您要求的索引頁面時，頁面所顯示的時間不會變更 （請參閱圖 1） 的 60 秒。 時間不會變更的事實證明快取頁面。 不過，內容插入 WriteSubstitution() 方法-隨機新聞項目 – 變更每個要求。

**圖 1 – 插入快取的頁面中的動態新聞項目**

![clip_image002](adding-dynamic-content-to-a-cached-page-cs/_static/image1.jpg)

#### <a name="using-post-cache-substitution-in-helper-methods"></a>使用快取後替換中的 Helper 方法

若要善用快取後替換更簡單的方法是封裝 WriteSubstitution() 方法內的自訂 helper 方法的呼叫。 這種方法是 helper 方法，在列出的 4 所示。

**列出 4 – AdHelper.cs**

[!code-csharp[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample4.cs)]

列出 4 包含靜態類別，公開兩個方法： RenderBanner() 和 RenderBannerInternal()。 RenderBanner() 方法代表實際的 helper 方法。 此方法會擴充標準的 ASP.NET MVC HtmlHelper 類別，可讓您呼叫 Html.RenderBanner() 就像任何其他 helper 方法一樣檢視中。

RenderBanner() 方法呼叫傳遞到 WriteSubsitution() 方法的 RenderBannerInternal() 方法 HttpResponse.WriteSubstitution() 方法。

RenderBannerInternal() 方法是私用方法。 這個方法不會公開為 helper 方法。 隨機 RenderBannerInternal() 方法會從三個橫幅廣告映像的清單傳回一個橫幅廣告映像。

列出 5 中修改的索引檢視表會說明如何使用 RenderBanner() helper 方法。 請注意，額外&lt;%@ 匯入 %&gt;指示詞就會包含要匯入 MvcApplication1.Helpers 命名空間之檢視的頂端。 如果您沒有匯入此命名空間，RenderBanner() 方法將會顯示為 Html 屬性上的方法。

**列出 5 – Views\Home\Index.aspx （具有 RenderBanner() 方法）**

[!code-aspx[Main](adding-dynamic-content-to-a-cached-page-cs/samples/sample5.aspx)]

當您要求列表 5 中的檢視所呈現的頁面時，每個要求會顯示不同的橫幅廣告 （請參閱圖 2）。 頁面會快取，但 RenderBanner() helper 方法以動態方式注入橫幅廣告。

**圖 2 – 顯示隨機橫幅廣告的索引檢視**

![clip_image004](adding-dynamic-content-to-a-cached-page-cs/_static/image2.jpg)

#### <a name="summary"></a>總結

本教學課程將說明如何以動態方式更新快取的頁面中的內容。 您已學習如何使用 HttpResponse.WriteSubstitution() 方法來啟用無法插入快取的頁面中的動態內容。 您也學到如何封裝 WriteSubstitution() 方法內的 HTML helper 方法的呼叫。

利用快取盡可能 – 它對 web 應用程式的效能有很大的影響。 本教學課程所述，您可以利用快取，即使您要在您的網頁中顯示動態內容。

## 

## 

> [!div class="step-by-step"]
> [上一頁](improving-performance-with-output-caching-cs.md)
> [下一頁](creating-a-controller-cs.md)
