---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
title: 第 4 部分： 列出產品 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋與 GridView contr.列出產品...
ms.author: riande
ms.date: 07/21/2010
ms.assetid: 4fab47d5-a6ec-4fdc-91f0-651a093a24b9
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-4
msc.type: authoredcontent
ms.openlocfilehash: ca7eccd684473d9a1ec4a8adfd8690b291fe702f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834670"
---
<a name="part-4-listing-products"></a>第 4 部分： 列出產品
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 4 部分涵蓋列出產品使用 GridView 控制項。


## <a id="_Toc260221670"></a>  列出產品使用 GridView 控制項

讓我們開始實作我們 ProductsList.aspx 頁面 」 以滑鼠右鍵按一下 「 我們的解決方案，並選取 新增 和 新增項目 」。

![](tailspin-spyworks-part-4/_static/image1.jpg)

選擇 Web 表單使用主版頁面並輸入 ProductsList.aspx 頁面名稱。 」

按一下 [新增]。

![](tailspin-spyworks-part-4/_static/image2.jpg)

接下來選擇我們放置 Site.Master 頁面的 「 樣式 」 資料夾，並從 [資料夾的內容] 視窗中選取。

![](tailspin-spyworks-part-4/_static/image3.jpg)

按一下 [確定] 以建立頁面。

我們的資料庫會填入產品資料，如下所示。

![](tailspin-spyworks-part-4/_static/image4.jpg)

建立我們的頁面之後我們再將使用實體資料來源來存取該產品的資料，但我們要選取的產品實體在本例中，我們需要限制為只有那些針對選取的類別會傳回的項目。

若要完成這項作業我們會告訴來自動產生 EntityDataSource WHERE 子句，我們會指定 WhereParameter。

您應該還記得，我們建立我們 [產品類別目錄功能表] 中的功能表項目時我們以動態方式建立連結 CatagoryID 加入每個連結的 QueryString。 我們會告訴實體資料來源，從該查詢字串參數中衍生的位置參數。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample1.aspx)]

接下來，我們將設定 ListView 控制項來顯示產品清單。 若要建立最佳的購物體驗，我們會到每個個別的產品，顯示在我們 ListVew 壓縮數種簡潔的功能。

- 產品名稱會是產品的詳細資料檢視的連結。
- 產品的價格將會顯示。
- 將顯示產品的映像，我們會以動態方式從選取映像目錄映像的目錄在我們的應用程式。
- 我們將立即將特定的產品加入購物車的連結。

以下是我們的 ListView 控制項執行個體的標記。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample2.aspx)]

我們會以動態方式建置每個顯示產品的幾個連結。

此外，我們測試自己的新頁面之前我們需要建立目錄結構的產品類別目錄的映像，如下所示。

![](tailspin-spyworks-part-4/_static/image1.png)

存取我們的產品映像之後，我們可以測試我們的產品清單頁面。

![](tailspin-spyworks-part-4/_static/image5.jpg)

從網站的首頁上，按一下其中一個類別目錄清單連結。

![](tailspin-spyworks-part-4/_static/image6.jpg)

現在我們要實作 ProductDetials.apsx 頁面和 AddToCart 功能。

使用檔案-&gt;新建立的頁面名稱 ProductDetails.aspx 先前一樣，使用網站主版頁面。

我們再次 EntityDataSource 控制項用來存取資料庫中的特定產品記錄，我們將使用 ASP.NET FormView 控制項來顯示產品的資料，如下所示。

[!code-aspx[Main](tailspin-spyworks-part-4/samples/sample3.aspx)]

別擔心，如果格式看起來是有點有趣給您。 上述標記挪出空間顯示版面配置中的幾個更新版本，我們將實作的功能。

「 購物車 」 將會代表我們的應用程式在更複雜的邏輯。 若要開始使用，請使用檔案-&gt;新增 以建立一個稱為 MyShoppingCart.aspx 的頁面。

請注意，我們不會選擇 ShoppingCart.aspx 的名稱。

我們的資料庫包含名為"ShoppingCart"的資料表。 當我們產生 Entity Data Model 類別被建立在資料庫中的每個資料表。 因此，實體資料模型會產生名為"ShoppingCart"實體類別。 我們無法編輯模型，讓我們可以使用我們的購物車實作該名稱，或將其延伸為我們的需求，但我們會選擇改為直接請選取可避免衝突的名稱。

另外值得注意的是，我們將建立簡單的購物車和購物車 」 顯示的內嵌功能的購物車 」 邏輯。 我們還可以選擇在完全不同的商務層中實作我們的購物車。

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-3.md)
> [下一頁](tailspin-spyworks-part-5.md)
