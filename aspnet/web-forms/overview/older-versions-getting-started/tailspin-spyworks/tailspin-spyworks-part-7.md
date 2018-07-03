---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 第 7 節： 新增功能 |Microsoft Docs
author: JoeStagner
description: 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 7 節新增其他功能，例如帳戶 revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 8cdde10981835877e5ac2f65860010920a68d0a2
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389172"
---
<a name="part-7-adding-features"></a>第 7 節： 新增功能
====================
藉由[Joe Stagner](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可擴充的應用程式，適用於.NET 平台是如何富含簡單。 它會展示如何在 ASP.NET 4 中使用最棒的新功能，建置線上商店，包括購物、 簽出，以及系統管理。
> 
> 本教學課程系列會詳細說明所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 第 7 節新增其他功能，例如帳戶檢閱、 產品評論和 「 熱門項目 」 和 「 也已購買 「 使用者控制項。


## <a id="_Toc260221673"></a>  新增功能

雖然使用者可以瀏覽我們的目錄，將項目放在購物車，並完成結帳程序，有幾個支援的功能，我們將會包含改善我們的網站。

1. 帳戶檢閱 （清單放訂單，並檢視詳細資料。）
2. 首頁中加入一些內容的特定內容。
3. 新增功能，讓使用者檢閱產品類別目錄中。
4. 建立使用者控制項以顯示 在首頁上的 熱門項目和位置，以控制。
5. 建立 「 也購買 」 使用者控制項，並將它新增至產品詳細資料頁面。
6. 新增連絡人 頁面。
7. 新增有關頁面。
8. 全域錯誤

## <a id="_Toc260221674"></a>  帳戶檢閱

在 [帳戶] 資料夾中建立一個具名的 OrderList.aspx 和其他具名的 OrderDetails.aspx 的兩個.aspx 頁面

OrderList.aspx 就像我們之前，將會利用的 GridView 和 EntityDataSoure 控制項。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure 從 Orders 資料表的使用者名稱篩選選取記錄 （請參閱 WhereParameter） 的使用者記錄的時，我們設定工作階段變數中。

也請注意，gridview HyperlinkField 中的這些參數：

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

這些指定作為 OrderDetails.aspx 頁面的查詢字串參數中指定 [訂單號碼] 欄位的每項產品的訂單詳細資料檢視的連結。

## <a id="_Toc260221675"></a>  OrderDetails.aspx

若要存取訂單和 FormView 顯示訂單資料，另一個 EntityDataSource 與 GridView，以顯示所有訂單行項目，我們將使用 EntityDataSource 控制項。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

中的程式碼後置檔案 (OrderDetails.aspx.cs) 中，我們有兩個小小的位元的內部管理。

首先，我們需要確定 OrderDetails 一律取得 OrderId。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

我們也需要計算並顯示訂單總計的明細項目。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  [首頁] 頁面

讓我們新增一些靜態內容至 Default.aspx 頁面。

首先我要建立的 「 內容 」 的資料夾和其中一個 Images 資料夾 （而且我也會包含在首頁上要使用映像）。

到 Default.aspx 頁面底部預留位置，加入下列標記。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  產品評論

第一次我們將新增按鈕，以連結至表單，我們可以使用輸入產品評論。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

請注意，我們會將 ProductID 傳遞查詢字串中

下一步 讓我們將新增名為 ReviewAdd.aspx 頁面

此頁面會使用 ASP.NET AJAX Control Toolkit。 如果您沒有已完成，您可以下載從[DevExpress](http://devexpress.com/act)還有指引設定與 Visual Studio 搭配使用這裡的工具組[ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)。

在設計模式中，從 [工具箱] 拖曳控制項並驗證程式，並建置一個表單，像下面這樣。

![](tailspin-spyworks-part-7/_static/image2.jpg)

標記看起來會像這樣。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

現在，我們可以輸入評論，可讓產品頁面上顯示這些評論。

您可以將此標記加入 ProductDetails.aspx 頁面。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

執行現在應用程式，並瀏覽至產品示範的產品資訊，包括客戶評論。

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  熱門的項目控制項 （建立使用者控制項）

以提高您的網站上的銷售中，我們會將數個功能加入 「 建議的銷售 」 常用或相關產品。

這些功能的第一個會在我們的產品類別目錄的更多熱門產品的清單。

我們將建立 「 使用者控制項 」 以顯示最暢銷的應用程式的首頁上的項目。 因為這會是控制項，我們可以在任何頁面上使用直接拖放控制項到我們的任何頁面上的 Visual Studio 的設計工具中。

在 Visual Studio 的 [方案總管] 中，以滑鼠右鍵按一下方案名稱，並建立一個名為 「 控制項 」 的新目錄。 雖然您不需要這樣做，我們將協助讓我們依 「 控制項 」 目錄中建立我們所有的使用者控制項的專案。

Controls 資料夾上按一下滑鼠右鍵，然後選擇 新增項目 」:

![](tailspin-spyworks-part-7/_static/image4.jpg)

指定的 「 PopularItems"我們控制項的名稱。 請注意使用者控制項檔案的副檔名為.ascx 不.aspx。

會定義我們的受歡迎的項目使用者控制項如下所示。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

這裡我們使用我們有尚未使用此應用程式中的方法。 我們使用 repeater 控制項，而不使用資料來源控制項我們要繫結 Repeater 控制項的 LINQ to Entities 查詢的結果。

在我們的控制碼後置我們這麼做，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

也請注意重要此行，在我們的控制項標記的頂端。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

因為最受歡迎的項目不會變更以時間為基礎，所以我們可以新增發疼的指示詞，以改善我們的應用程式的效能。 這個指示詞會導致控制項程式碼才可執行控制項的快取的輸出過期時。 否則，會使用這個控制項的輸出快取的版本。

現在我們只需要已在我們 Default.aspc 頁面中包含我們新的控制項。

使用拖放開啟的資料行的預設表單控制項的執行個體。

![](tailspin-spyworks-part-7/_static/image5.jpg)

現在當我們執行應用程式首頁會顯示最受歡迎的項目。

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  「 也購買 」 控制項 （具有參數的使用者控制項）

我們將建立第二個使用者控制項將會建議加入內容精確性銷售上一層樓。

若要計算的最上層的 「 也購買 」 項目邏輯是重要的。

我們 「 也購買 」 的控制會選取目前選取的 ProductID OrderDetails 記錄 （先前購買），並抓取找到每個唯一訂單的 Orderid。

然後我們會選取所有的產品從所有這些訂單及購買數量的總和。 我們會依該數量的總和排序的產品，並顯示前五個項目。

指定此邏輯的複雜度，我們會實作這個演算法做為預存程序。

T-SQL 預存程序如下所示。

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

請注意，我們會包含它在我們的應用程式和我們產生實體資料模型，我們已指定，除了資料表和檢視表，我們需要實體資料模型時在資料庫中已存在此預存程序 (SelectPurchasedWithProducts)應該包含此預存程序。

若要從實體資料模型，我們要匯入函式中存取預存程序。

按兩下實體資料模型，在 方案總管 中，以在設計工具中開啟它，並開啟 模型瀏覽器中，然後以滑鼠右鍵按一下設計工具中，選取 「 新增函式匯入 」。

![](tailspin-spyworks-part-7/_static/image1.png)

這樣會開啟此對話方塊。

![](tailspin-spyworks-part-7/_static/image2.png)

填寫欄位，您看到上方，選取 「 SelectPurchasedWithProducts"，並使用我們已匯入的函式名稱的程序名稱。

按一下 [確定]。

需要這麼做，我們可能會在模型中的任何其他項目時，我們可以只在針對預存程序進行程式設計。

因此，我們的 「 控制項 」 資料夾中建立新的使用者控制項，名為 AlsoPurchased.ascx。

此控制項的標記看起來會非常類似 PopularItems 控制項。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

值得注意的差異是，未快取輸出因為項目的轉譯會因產品而異。

ProductId 會控制 「 屬性 」。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

控制項的 PreRender 事件處理常式中我們 eed 做三件事。

1. 請確定已設定 ProductID。
2. 看看是否有與目前已購買任何產品。
3. 輸出某些項目 #2 中所決定。

請注意，呼叫預存程序，透過模型是多麼容易。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

判斷，那里 「 也購買 」 之後我們可以直接將繫結 repeater 查詢所傳回的結果。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

如果沒有任何 「 也購買 」 項目則我們只會顯示其他熱門的項目從我們的目錄。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

若要檢視 「 也購買 」 項目，開啟 ProductDetails.aspx 頁面並拖曳 AlsoPurchased 控制項從 [方案總管] 中，使其出現在標記中的這個位置中。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

如此一來，將在 ProductDetails 頁面頂端建立控制項的參考。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

因為 AlsoPurchased 使用者控制項需要 ProductId 的數字我們會使用針對目前的資料模型項目頁面的根 Eval 陳述式來設定我們的掌控中的 [ProductID] 屬性。

![](tailspin-spyworks-part-7/_static/image3.png)

當我們建置並立即執行，並瀏覽至產品我們會看到 「 也購買 」 項目。

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-6.md)
> [下一頁](tailspin-spyworks-part-8.md)
