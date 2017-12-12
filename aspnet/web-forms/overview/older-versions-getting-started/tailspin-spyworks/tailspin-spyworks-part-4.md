---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: "第 4 部分： 列出產品 |Microsoft 文件"
author: JoeStagner
description: "此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋 GridView contr.清單的產品..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: 420cdbcc002bcbfff619d399a7a374009999d754
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="part-4-listing-products"></a>第 4 部分： 列出產品
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋的 GridView 控制項的產品清單。


## <a id="_Toc260221670"></a>列出產品與 GridView 控制項

讓我們開始實作我們 ProductsList.aspx 頁面 」 以滑鼠右鍵按一下 「 我們的解決方案，然後選取 「 新增 」 和 「 新的項目 」。

![](tailspin-spyworks-part-4/_static/image1.jpg)

選擇 Web 表單使用主版頁面 」，並輸入 ProductsList.aspx 頁面名稱 」。

按一下 [加入]。

![](tailspin-spyworks-part-4/_static/image2.jpg)

接下來選擇放置於 Site.Master 頁面的 「 樣式 」 資料夾，然後從資料夾的 「 內容 」 視窗中選取。

![](tailspin-spyworks-part-4/_static/image3.jpg)

按一下 「 確定 」 建立頁面。

我們的資料庫會填入產品資料，如下所示。

![](tailspin-spyworks-part-4/_static/image4.jpg)

建立我們網頁之後我們試要用為實體資料來源來存取該產品的資料，但是我們需要這個執行個體中選取的產品實體，而且我們要限制傳回給只有選取的類別目錄的項目。

若要完成此我們會告訴 EntityDataSource 來自動產生 WHERE 子句，我們將會指定 WhereParameter。

請記得，當我們 [產品類別目錄功能表] 中建立功能表項目我們以動態方式建立連結 CatagoryID 加入每個連結的 QueryString。 我們會告訴實體資料來源是位置參數衍生自該查詢字串參數。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

接下來，我們稍後會設定 ListView 控制項顯示的產品清單。 若要建立購物懽呏枔我們會壓縮數種精簡功能到我們 ListVew 中顯示每個個別產品。

- 產品名稱是產品的詳細資料檢視的連結。
- 將顯示產品的價格。
- 將顯示產品的映像，我們以動態方式將選取映像目錄從映像，在我們的應用程式。
- 我們將包括立即將特定的產品加入購物車的連結。

以下是我們的 ListView 控制項執行個體的標記。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

我們以動態方式所建立每個顯示的產品數個的連結。

此外，我們測試自己的新頁面之前，我們需要建立目錄結構的產品類別目錄影像，如下所示。

![](tailspin-spyworks-part-4/_static/image1.png)

可存取我們的產品映像之後，我們可以測試我們的產品清單頁面。

![](tailspin-spyworks-part-4/_static/image5.jpg)

從網站的首頁上，按一下其中一個類別目錄清單連結。

![](tailspin-spyworks-part-4/_static/image6.jpg)

現在我們需要實作 ProductDetials.apsx 頁面和 AddToCart 功能。

使用檔案-&gt;新增 建立 ProductDetails.aspx 如同我們先前使用的站台主版頁面的頁面名稱。

我們一次用來存取資料庫中的特定產品記錄 EntityDataSource 控制項，我們會使用 ASP.NET FormView 控制項來顯示產品資料，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

如果格式設定的位元居然看起來您，擔心。 上述的標記挪出空間顯示版面配置中的一組功能，我們將稍後在實作。

購物車將代表我們的應用程式在更複雜的邏輯。 若要開始使用，請使用檔案-&gt;新增 建立一個稱為 MyShoppingCart.aspx 的頁面。

請注意，我們不會選擇 ShoppingCart.aspx 的名稱。

我們的資料庫包含名為"ShoppingCart"的資料表。 我們產生實體資料模型時已針對資料庫中的每個資料表建立的類別。 因此，實體資料模型會產生名為"ShoppingCart"實體類別。 我們無法編輯此模型，讓我們無法使用該名稱，我們的購物車實作或擴充為我們的需求，但我們會選擇改為直接請選取將會避免衝突的名稱。

另外值得注意的是，我們將建立簡單購物車和內嵌的購物車顯示購物車邏輯。 我們也可能會選擇實作我們的購物車中完全不同的商務層。

>[!div class="step-by-step"]
[上一頁](tailspin-spyworks-part-3.md)
[下一頁](tailspin-spyworks-part-5.md)
