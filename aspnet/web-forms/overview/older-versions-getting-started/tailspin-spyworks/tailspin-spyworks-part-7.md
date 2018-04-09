---
uid: web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
title: 第 7 部分： 加入功能 |Microsoft 文件
author: JoeStagner
description: 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 部分 7 加入其他功能，例如帳戶 revie...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/21/2010
ms.topic: article
ms.assetid: 50223ee9-11b9-4cf3-bca2-e2f10bf471f3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/tailspin-spyworks/tailspin-spyworks-part-7
msc.type: authoredcontent
ms.openlocfilehash: 17f068155f6726047901e2f7d580d375a4e07c87
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="part-7-adding-features"></a>第 7 部分： 新增功能
====================
由[Joe stagner 以](https://github.com/JoeStagner)

> Tailspin Spyworks 示範建立功能強大、 可調整的應用程式的.NET 平台是如何 hierarchy 簡單。 它會顯示如何使用 ASP.NET 4 中的強大的新功能，建置線上商店，包括購物、 簽出，以及管理關閉。
> 
> 此教學課程系列詳細列出所有建置 Tailspin Spyworks 範例應用程式所採取的步驟。 部分 7 加入其他功能，例如帳戶檢閱、 產品評論和 「 熱門項目 」 和 「 也已購買 「 使用者控制項。


## <a id="_Toc260221673"></a>  加入功能

使用者可以瀏覽我們的目錄，將項目放在購物車，並完成簽出程序，還是有一些支援的功能，我們將包含改善我們的網站。

1. 帳戶檢閱 （清單放訂單，並檢視詳細資料。）
2. 前端網頁加入內容的特定內容。
3. 新增功能，讓使用者檢閱產品類別目錄中。
4. 建立使用者控制項加入 [前端] 頁面上顯示常用的項目和位置，以控制。
5. 建立 「 也購買 」 的使用者控制項，並將它加入至產品詳細資料頁面上。
6. 新增連絡人 頁面。
7. 新增有關頁面。
8. 全域錯誤

## <a id="_Toc260221674"></a>  帳戶檢閱

在 [帳戶] 資料夾中建立一個具名的 OrderList.aspx 和其他具名的 OrderDetails.aspx 的兩個.aspx 頁面

就像我們之前有 OrderList.aspx 會利用 GridView 和 EntityDataSoure 控制項。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample1.aspx)]

EntityDataSoure Orders 資料表中的使用者名稱上篩選選取記錄 （請參閱 WhereParameter） 我們中工作階段變數的使用者記錄時設定。

此外請注意，這些參數的 GridView HyperlinkField 中：

[!code-xml[Main](tailspin-spyworks-part-7/samples/sample2.xml)]

這樣會指定 OrderID 欄位指定為查詢字串參數，以 OrderDetails.aspx 頁面每項產品的訂單詳細資料檢視的連結。

## <a id="_Toc260221675"></a>  OrderDetails.aspx

我們將使用 EntityDataSource 控制項存取訂單和 FormView 顯示訂單資料，另一個與 GridView EntityDataSource，以顯示所有訂單行項目。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample3.aspx)]

在程式碼後置檔案 (OrderDetails.aspx.cs) 中有兩個小的位元環境維護。

首先，我們需要確定 OrderDetails 永遠都會取得 OrderId。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample4.cs)]

我們也要計算並顯示訂單總計的明細項目。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample5.cs)]

## <a id="_Toc260221676"></a>  [首頁] 頁面

讓我們來加入靜態內容至 Default.aspx 頁面。

首先，我要建立的 「 內容 」 的資料夾並在其中一個 Images 資料夾 （和我會包括在 [首頁] 頁面上使用的影像）。

Default.aspx 頁面底部版面配置，加入下列標記。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample6.aspx)]

## <a id="_Toc260221677"></a>  產品評論

第一次我們會將含有連結按鈕新增至表單，讓我們可以輸入的產品評論。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample7.aspx)]

![](tailspin-spyworks-part-7/_static/image1.jpg)

請注意我們的查詢字串中傳遞 ProductID

下一步 讓我們加入名為 ReviewAdd.aspx 頁面

此頁面會使用 ASP.NET AJAX Control Toolkit。 如果您沒有已完成，您可以下載從[DevExpress](http://devexpress.com/act)而上設定 Visual Studio 搭配使用此工具組的指引[ https://www.asp.net/learn/ajax-videos/video-76.aspx ](../../../videos/ajax-control-toolkit/how-do-i-get-started-with-the-aspnet-ajax-control-toolkit.md)。

在設計模式中，從 [工具箱] 拖曳控制項和驗證程式，建立如下的表單。

![](tailspin-spyworks-part-7/_static/image2.jpg)

標記看起來會像這樣。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample8.aspx)]

現在，我們可以輸入評論，可讓這些檢閱顯示在 [產品] 頁面上。

將這個標記加入至 ProductDetails.aspx 頁面。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample9.aspx)]

執行現在應用程式，以及巡覽至產品顯示產品資訊，包括客戶評論。

![](tailspin-spyworks-part-7/_static/image3.jpg)

## <a id="_Toc260221678"></a>  熱門項目控制項 （建立使用者控制項）

以增加您的網站上的銷售我們將 「 建議賣出 「 熱門或相關產品加入一組功能。

這些功能的第一個會在我們的產品目錄中常見的產品的清單。

我們將建立 「 使用者控制項 」 以顯示最暢銷的首頁上的應用程式的項目。 由於這會為控制項，我們可以使用它的任何頁面直接拖曳到控制項放入任何頁面中，我們希望 Visual Studio 設計工具中。

在 Visual Studio 方案總管 中，以滑鼠右鍵按一下方案名稱，建立名為 「 控制項 」 的新目錄。 雖然您不需要這樣做，我們會協助讓我們依 「 控制項 」 目錄中建立我們所有的使用者控制項的專案。

以滑鼠右鍵按一下 [控制項] 資料夾，然後選擇 「 新的項目 」:

![](tailspin-spyworks-part-7/_static/image4.jpg)

指定我們的掌控"PopularItems 」 的名稱。 請注意使用者控制項檔案的副檔名為.ascx 不.aspx。

我們熱門項目使用者控制項將如下所示定義。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample10.aspx)]

這裡我們使用我們有尚未使用此應用程式中的方法。 我們會使用在中繼器控制項，而不使用資料來源控制項我們要繫結中繼器控制項至 LINQ to Entities 查詢的結果。

在我們的掌控背後的程式碼我們這樣做，如下所示。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample11.cs)]

也請注意我們的掌控標記頂端重要此行。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample12.aspx)]

由於最受歡迎的項目將不會變更以分鐘以分鐘為基礎，我們可以加入發疼的指示詞，以改善我們的應用程式的效能。 這個指示詞會使控制項的程式碼只執行快取的輸出控制項的過期時。 否則，會使用這個控制項的輸出快取的版本。

現在我們只需要是在我們 Default.aspc 頁面中包含我們新的控制項。

使用拖放在開啟的資料行的預設表單中放置控制項的執行個體。

![](tailspin-spyworks-part-7/_static/image5.jpg)

現在當我們執行我們的應用程式首頁會顯示最受歡迎的項目。

![](tailspin-spyworks-part-7/_static/image6.jpg)

## <a id="_Toc260221679"></a>  「 也購買 」 控制 （具有參數的使用者控制項）

我們將建立第二個使用者控制項將會建議加入內容精確性銷售到下一個層級。

為非一般的邏輯，以計算前 」 也購買 」 項目。

我們"也購買 」 控制項將會選取目前選取的 ProductID OrderDetails 記錄 （先前購買） 和抓取找到每個唯一訂單的訂單。

然後我們將會選取所有產品從所有這些訂單及購買數量的總和。 我們將依據數量加總產品進行排序，並顯示前五個項目。

指定此邏輯的複雜度，我們會實作這個演算法做為預存程序。

T-SQL 預存程序如下所示。

[!code-sql[Main](tailspin-spyworks-part-7/samples/sample13.sql)]

請注意，當我們包含它在我們的應用程式中，我們產生實體資料模型，我們除了的資料表和檢視，我們需要實體資料模型已指定，此預存程序 (SelectPurchasedWithProducts) 存在於資料庫應該包含此預存程序。

若要存取實體資料模型中的預存程序，我們需要匯入函式。

實體資料模型設計工具中開啟它，然後將模型瀏覽器中，開啟 [方案總管] 中按兩下然後以滑鼠右鍵按一下設計工具中，並選取 「 新增函式匯入 」。

![](tailspin-spyworks-part-7/_static/image1.png)

這樣會開啟此對話方塊。

![](tailspin-spyworks-part-7/_static/image2.png)

填寫您，請參閱上述選取"SelectPurchasedWithProducts 」 欄位，並使用我們匯入的函式名稱的程序名稱。

按一下 「 確定 」。

需要這樣做我們只是可以撰寫預存程序針對如同我們可能會在模型中的任何其他項目。

因此，我們 「 控制項 」 資料夾中建立新的使用者控制項，名為 AlsoPurchased.ascx。

此控制項的標記看起來非常熟悉 PopularItems 控制項。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample14.aspx)]

顯著的差異在於，會無法快取輸出因為項目的轉譯會因產品而異。

ProductId 將控制項的 「 屬性 」。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample15.cs)]

控制項的 PreRender 事件處理常式中我們 eed 做三件事。

1. 請確定設定的 ProductID。
2. 查看是否有與目前已購買任何產品。
3. 輸出 #2 中決定的某些項目。

請注意，呼叫預存程序，透過模型是多麼的輕鬆。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample16.cs)]

決定，那里"也購買 」 之後無法直接繫結中繼器至查詢所傳回的結果。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample17.cs)]

如果沒有任何 「 也購買 」 項目我們將只是從我們的目錄中顯示其他常用的項目。

[!code-csharp[Main](tailspin-spyworks-part-7/samples/sample18.cs)]

若要檢視 「 也購買 」 項目，開啟 ProductDetails.aspx 頁面，並從 [方案總管] 中拖曳 AlsoPurchased 控制項，使其出現在這個標記中的位置。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample19.aspx)]

這樣會在頁面頂端的 ProductDetails 建立控制項的參考。

[!code-aspx[Main](tailspin-spyworks-part-7/samples/sample20.aspx)]

因為 AlsoPurchased 使用者控制項需要 ProductId 數字，我們會使用目前的資料模型項目頁面的 Eval 陳述式設定我們的掌控中的 [ProductID] 屬性。

![](tailspin-spyworks-part-7/_static/image3.png)

當我們建置並立即執行，以及瀏覽至產品我們會看到 「 也購買 」 項目。

![](tailspin-spyworks-part-7/_static/image7.jpg)

> [!div class="step-by-step"]
> [上一頁](tailspin-spyworks-part-6.md)
> [下一頁](tailspin-spyworks-part-8.md)
