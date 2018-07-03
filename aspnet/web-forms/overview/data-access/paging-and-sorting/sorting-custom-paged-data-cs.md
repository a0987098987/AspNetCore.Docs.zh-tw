---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
title: 排序自訂的分頁資料 (C#) |Microsoft Docs
author: rick-anderson
description: 在上一個教學課程中我們已了解如何實作自訂分頁時 presentating 網頁上的資料。 在本教學課程中，我們了解如何擴充先前的內容...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 778baa4e-4af8-4665-947e-7a01d1a4dff2
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-cs
msc.type: authoredcontent
ms.openlocfilehash: 36080799c94b359a7e5b7ba6ccb47e359fc2161d
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37362793"
---
<a name="sorting-custom-paged-data-c"></a>排序自訂的分頁資料 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_CS.exe)或[下載 PDF](sorting-custom-paged-data-cs/_static/datatutorial26cs1.pdf)

> 在上一個教學課程中我們已了解如何實作自訂分頁時 presentating 網頁上的資料。 在本教學課程中，我們看到如何擴充以包含針對排序自訂的分頁支援上述的範例。


## <a name="introduction"></a>簡介

相較於預設分頁時，自訂分頁的差距，讓自訂分頁時進行大量的資料分頁的既定的分頁實作選擇可以改善效能的資料進行分頁。 實作自訂的分頁是比實作預設的分頁，不過，尤其是當加入至混合排序更複雜。 在本教學課程中，我們會擴大納入支援排序的前一個範例*和*自訂分頁。

> [!NOTE]
> 因為本教學課程是在前一個，才能開始花一點時間複製內的宣告式語法`<asp:Content>`從先前的教學課程的網頁上的項目 (`EfficientPaging.aspx`) 並貼上它之間`<asp:Content>`中的項目`SortParameter.aspx`  頁面。 回頭參考步驟 1[將驗證控制項加入編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)教學課程的上一個 ASP.NET 網頁的功能複寫至另一個的更詳細討論。


## <a name="step-1-reexamining-the-custom-paging-technique"></a>步驟 1： 再次檢驗自訂分頁技術

自訂分頁時才能正常運作，我們必須實作一種技術，可以有效率地擷取記錄參數的開始資料列索引和最大資料列的特定子集。 有少數幾個可用來達到這個目的的技術。 在先前教學課程中我們探討了完成這項作業使用新的 Microsoft SQL Server 2005 的`ROW_NUMBER()`排名函式。 簡單地說，`ROW_NUMBER()`排名函式會將指派給所指定的排序順序第一個查詢所傳回的每個資料列的資料列數目。 記錄的適當子集則會取得藉由傳回編號結果中的特定區段中。 下列查詢說明如何使用此技巧來傳回編號 11 到 20 時排名結果依字母順序排序的產品`ProductName`:


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample1.sql)]

此技術適用於使用特定的排序次序的分頁 (`ProductName`依字母順序排序，在此情況下)，但是查詢必須修改，以顯示依不同的排序運算式排序的結果。 在理想情況下，上述的查詢無法加以改寫才能使用中的參數`OVER`子句，就像這樣：


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample2.sql)]

不幸的是，參數化`ORDER BY`子句不允許。 相反地，我們必須建立可接受的預存程序`@sortExpression`輸入的參數，但使用一項因應措施：

- 撰寫每個可能使用; 的排序運算式的硬式編碼查詢然後，使用`IF/ELSE`T-SQL 陳述式來判斷要執行的查詢。
- 使用`CASE`陳述式來提供動態`ORDER BY`運算式根據`@sortExpressio`n 輸入參數，請參閱用來動態排序查詢結果中的一節[的 SQL Power`CASE`陳述式](http://www.4guysfromrolla.com/webtech/102704-1.shtml)如需詳細資訊。
- 製作適當的查詢，為預存程序中的字串，然後使用[`sp_executesql`系統預存程序](https://msdn.microsoft.com/library/ms188001.aspx)執行動態查詢。

每個這些因應措施有一些缺點。 第一個選項不是預期的容易維護與其他兩個因為它需要您建立每個可能的排序運算式的查詢。 因此，如果您稍後決定將新的、 可排序的欄位加入至 GridView 也會需要返回並更新預存程序。 第二種方法有一些微妙之處引進效能考量，對非字串的資料庫資料行排序時，也有相同的可維護性問題，與第一個。 如果攻擊者能夠執行傳入輸入的參數值，他們所選擇的預存程序的第三個選擇，會使用動態 SQL，引進了 SQL 資料隱碼攻擊的風險。

雖然沒有任何一種方式很完美，我認為第三個選項最適合的三個。 其使用動態 SQL 中，它會提供某種程度的彈性，其他兩個不這麼做。 此外，如果攻擊者能夠執行預存程序在自己選擇的輸入參數中傳遞，可以只利用 SQL 資料隱碼攻擊。 由於 DAL 會使用參數化的查詢，ADO.NET 會保護到架構中，資料庫會傳送這些參數表示，SQL 資料隱碼攻擊的弱點可能會只存在於如果攻擊者可以直接執行預存程序。

若要實作這項功能，，請在名為 Northwind 資料庫中建立新的預存程序`GetProductsPagedAndSorted`。 這個預存程序應該接受三個輸入參數： `@sortExpression`，輸入的參數的型別`nvarchar(100`)，指定如何應該排序結果，並直接在之後插入`ORDER BY`中的文字`OVER`子句; 以及`@startRowIndex`並`@maximumRows`，從相同的兩個整數輸入的參數`GetProductsPaged`預存程序檢查在先前的教學課程。 建立`GetProductsPagedAndSorted`預存程序使用下列指令碼：


[!code-sql[Main](sorting-custom-paged-data-cs/samples/sample3.sql)]

預存程序一開始透過確保值`@sortExpression`已指定參數。 如果遺失，結果會依排名`ProductID`。 接下來，會建構動態 SQL 查詢。 請注意與動態 SQL 查詢稍有不同我們先前的查詢，用來從 Products 資料表中擷取所有資料列。 在先前的範例，我們取得每個產品 s 相關聯的分類和供應商的名稱使用子查詢。 回到才做出此決定[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，而不使用已完成`JOIN`s 因為 TableAdapter 無法自動建立關聯的插入、 更新和刪除這類方法執行查詢。 `GetProductsPagedAndSorted`預存程序，不過，必須使用`JOIN`s 依照分類或供應商的名稱來排序結果。

此動態查詢打造藉由串連的靜態查詢部分並`@sortExpression`， `@startRowIndex`，和`@maximumRows`參數。 由於`@startRowIndex`和`@maximumRows`整數參數，它們必須先轉換成 nvarchars 才能正確地串連。 一旦已建構此動態 SQL 查詢，它在透過執行`sp_executesql`。

請花一點時間來測試使用不同的值，如這個預存程序`@sortExpression`， `@startRowIndex`，和`@maximumRows`參數。 從 伺服器總管 中，以滑鼠右鍵按一下預存程序名稱，然後選擇 執行。 這會顯示 執行預存程序 對話方塊中，您可以在其中輸入 （請參閱 圖 1） 的輸入的參數。 若要依類別目錄排序結果，使用 類別名稱的`@sortExpression`參數的值; 依供應商的公司名稱排序，請使用公司名稱。 提供的參數值之後, 按一下 [確定]。 結果會顯示在 [輸出] 視窗中。 圖 2 顯示的結果，藉由排序時，傳回產品排名 11 到 20 時`UnitPrice`以遞減順序。


![預存程序 s 三個輸入參數嘗試不同的值](sorting-custom-paged-data-cs/_static/image1.png)

**圖 1**： 嘗試不同的值，如預存程序 s，三個輸入參數


[![結果會顯示在 [輸出] 視窗中的預存程序 s](sorting-custom-paged-data-cs/_static/image3.png)](sorting-custom-paged-data-cs/_static/image2.png)

**圖 2**： 結果會顯示在 [輸出] 視窗中的預存程序 s ([按一下以檢視完整大小的影像](sorting-custom-paged-data-cs/_static/image4.png))


> [!NOTE]
> 當指定排名結果`ORDER BY`中的資料行`OVER`子句，SQL Server 必須排序結果。 這是快速的作業，如果結果正在當做排序依據的資料行上沒有叢集的索引，或是否涵蓋編製索引，但可以否則成本更高。 若要改善夠大的查詢的效能，請考慮將結果依排序的資料行的非叢集索引。 請參閱[排名函式和 SQL Server 2005 中的效能](http://www.sql-server-performance.com/ak_ranking_functions.asp)如需詳細資訊。


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>步驟 2： 增強的資料存取和商務邏輯層

使用`GetProductsPagedAndSorted`建立預存程序中，我們的下一個步驟是能用來執行該預存程序，透過我們的應用程式架構。 這需要 DAL 和 BLL 新增適當的方法。 可讓著手將方法加入 DAL 的 s。 開啟`Northwind.xsd`具類型的資料集，以滑鼠右鍵按一下`ProductsTableAdapter`，，然後從操作功能表中選擇 加入查詢選項。 如同我們在先前的教學課程中，我們想要設定這個新的 DAL 方法，以使用現有的預存程序- `GetProductsPagedAndSorted`，在此案例。 藉由指示您想要使用現有新的 TableAdapter 方法啟動預存程序。


![選擇使用現有的預存程序](sorting-custom-paged-data-cs/_static/image5.png)

**圖 3**： 選擇使用現有的預存程序


若要指定要使用的預存程序，請選取`GetProductsPagedAndSorted`儲存程序，從下拉式清單中下一個畫面。


![使用 GetProductsPagedAndSorted 預存程序](sorting-custom-paged-data-cs/_static/image6.png)

**圖 4**： 使用 GetProductsPagedAndSorted 預存程序


這個預存程序會傳回一組記錄，因為其結果因此，在下一個畫面中，表示它會傳回表格式資料。


![指出預存程序會傳回表格式資料](sorting-custom-paged-data-cs/_static/image7.png)

**圖 5**： 指出預存程序會傳回表格式資料


最後，建立 DAL 方法中使用這兩種填滿 DataTable，並傳回 DataTable 模式、 命名方法`FillPagedAndSorted`和`GetProductsPagedAndSorted`分別。


![選擇 方法名稱](sorting-custom-paged-data-cs/_static/image8.png)

**圖 6**： 選擇 方法名稱


現在我們已擴充 DAL 中，我們準備好辦法 BLL。 開啟`ProductsBLL`類別檔案，並新增一個新的方法， `GetProductsPagedAndSorted`。 這個方法必須接受三個輸入參數`sortExpression`， `startRowIndex`，並`maximumRows`，應該直接向下呼叫到 DAL 的`GetProductsPagedAndSorted`方法，就像這樣：


[!code-csharp[Main](sorting-custom-paged-data-cs/samples/sample4.cs)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>步驟 3： 設定 SortExpression 參數中的行程 ObjectDataSource

DAL 和 BLL，包括使用的方法需要擴增`GetProductsPagedAndSorted`預存程序，所有會保持為設定內的 ObjectDataSource`SortParameter.aspx`頁面，即可使用新的 BLL 方法，並傳入`SortExpression`參數為基礎使用者已要求來排序結果所依據的資料行。

開始先變更 ObjectDataSource s`SelectMethod`從`GetProductsPaged`至`GetProductsPagedAndSorted`。 這可以透過設定資料來源精靈 」，從 [屬性] 視窗中，或直接透過宣告式語法完成。 接下來，我們必須為 ObjectDataSource s 提供一個值[`SortParameterName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)。 如果設定這個屬性，ObjectDataSource 會嘗試將傳入的 GridView s`SortExpression`屬性設`SelectMethod`。 ObjectDataSource 特別的是，尋找其名稱是相等的值為輸入參數`SortParameterName`屬性。 因為 BLL s`GetProductsPagedAndSorted`方法具有名為排序的運算式輸入的參數`sortExpression`，設定 ObjectDataSource 的`SortExpression`sortExpression 的屬性。

進行這兩項變更之後, 的 ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-custom-paged-data-cs/samples/sample5.aspx)]

> [!NOTE]
> 如先前的教學課程中，確認 ObjectDataSource 就*不*其 SelectParameters 集合中包含的 sortExpression、 startRowIndex 或 maximumRows 的輸入的參數。


若要啟用排序 GridView 裡，只要核取方塊啟用排序 GridView s 智慧標籤中，它會設定 GridView s`AllowSorting`屬性設`true`並導致每個資料行轉譯為 LinkButton 的標頭文字。 當使用者按一下其中一個標頭 Linkbutton 時，回傳是兩邊彼此乾瞪眼並發生下列步驟：

1. GridView 更新其[`SortExpression`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)的值`SortExpression`其標頭已按下連結的欄位
2. ObjectDataSource 會叫用的 BLL s`GetProductsPagedAndSorted`方法並傳入 GridView s`SortExpression`屬性做為方法 s 的值`sortExpression`輸入的參數 (以及適當`startRowIndex`和`maximumRows`輸入參數值)
3. BLL 叫用的 DAL s`GetProductsPagedAndSorted`方法
4. 執行 DAL`GetProductsPagedAndSorted`預存程序，並傳遞`@sortExpression`參數 (連同`@startRowIndex`和`@maximumRows`輸入參數值)
5. 預存程序傳回 BLL，bll ObjectDataSource; 將它傳回適當的資料子集這項資料是繫結至 GridView，轉譯為 HTML，然後傳送給使用者

[圖 7] 顯示時依排序的結果的第一頁`UnitPrice`以遞增順序。


[![結果會依照 UnitPrice](sorting-custom-paged-data-cs/_static/image10.png)](sorting-custom-paged-data-cs/_static/image9.png)

**圖 7**： 結果會依照 UnitPrice ([按一下以檢視完整大小的影像](sorting-custom-paged-data-cs/_static/image11.png))


目前的實作可以正確排序的結果依產品名稱、 類別名稱、 每個單位和單價的數量，而嘗試排列結果的順序與供應商名稱結果在執行階段例外狀況 （請參閱 圖 8）。


![嘗試將排序結果，您可以在下列執行階段例外狀況的供應商結果](sorting-custom-paged-data-cs/_static/image12.png)

**圖 8**： 嘗試將排序結果，您可以在下列執行階段例外狀況的供應商結果


因為發生這個例外狀況`SortExpression`的 GridView s `SupplierName` BoundField 設`SupplierName`。 供應商 s 中的名稱但`Suppliers`資料表稱為實際上`CompanyName`我們已經被別名做為此資料行名稱`SupplierName`。 不過，`OVER`所使用的子句`ROW_NUMBER()`函式無法使用此別名，而且必須使用實際的資料行名稱。 因此，變更`SupplierName`BoundField 的`SortExpression`從供應商名稱至 CompanyName （請參閱 圖 9）。 如 [圖 10] 所示，這項變更之後的結果可依供應商。


![將 供應商的供應商名稱 BoundField 的 SortExpression](sorting-custom-paged-data-cs/_static/image13.png)

**圖 9**： 將供應商名稱 BoundField 的 SortExpression 變更為 公司名稱


[![現在可以供應商提供的排序結果](sorting-custom-paged-data-cs/_static/image15.png)](sorting-custom-paged-data-cs/_static/image14.png)

**圖 10**: 結果可立即依供應商 ([按一下以檢視完整大小的影像](sorting-custom-paged-data-cs/_static/image16.png))


## <a name="summary"></a>總結

在先前的教學課程中，我們檢查的自訂分頁實作所需的結果已排序的順序會在設計階段指定。 簡單地說，這表示，我們實作的自訂分頁實作可能不是，在此同時，提供排序功能。 在本教學課程克服這項限制藉由從要包含第一個擴充預存程序`@sortExpression`結果無法排序的輸入的參數。

建立此預存程序和 BLL 和 DAL 中建立新的方法之後，我們能夠實作的 GridView 會提供這兩個排序和自訂設定來傳入 GridView s 目前 ObjectDataSource 分頁`SortExpression`BLL 屬性`SelectMethod`.

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Carlos 環境。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](efficiently-paging-through-large-amounts-of-data-cs.md)
> [下一頁](creating-a-customized-sorting-user-interface-cs.md)
