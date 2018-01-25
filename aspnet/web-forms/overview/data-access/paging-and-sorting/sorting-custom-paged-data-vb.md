---
uid: web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
title: "排序自訂分頁資料 (VB) |Microsoft 文件"
author: rick-anderson
description: "在上一個教學課程中我們學到如何實作自訂分頁時 presentating 網頁上的資料。 在本教學課程中，我們看到如何擴充上述..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 4823a186-caaf-4116-a318-c7ff4d955ddc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/sorting-custom-paged-data-vb
msc.type: authoredcontent
ms.openlocfilehash: ee02915a5c69d824c6450157b0c734a2e2ab5c11
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="sorting-custom-paged-data-vb"></a>排序自訂分頁資料 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_26_VB.exe)或[下載 PDF](sorting-custom-paged-data-vb/_static/datatutorial26vb1.pdf)

> 在上一個教學課程中我們學到如何實作自訂分頁時 presentating 網頁上的資料。 在本教學課程中，我們看到如何擴充上述的範例包括排序自訂分頁的支援。


## <a name="introduction"></a>簡介

相較於預設分頁時，自訂分頁的差距，進行自訂時進行大量的資料分頁的分頁既定的分頁實作選擇可以改善效能的資料進行分頁。 實作自訂分頁會比實作預設的分頁，不過，尤其是加入至混合排序更複雜的。 在本教學課程中，我們將會延長從可支援排序的前一個範例*和*自訂分頁。

> [!NOTE]
> 由於本教學課程是根據前一個，才能開始花點時間複製內的宣告式語法`<asp:Content>`先前的教學課程的網頁上的項目 (`EfficientPaging.aspx`) 並貼上它之間`<asp:Content>`中的項目`SortParameter.aspx`頁面。 指的步驟 1[新增驗證控制項的編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-vb.md)教學課程的上一個 ASP.NET 網頁的功能複寫至另一個的更詳細討論。


## <a name="step-1-reexamining-the-custom-paging-technique"></a>步驟 1： 再次檢驗自訂分頁技術

自訂分頁時才能正常運作，我們必須實作一種技術，可以有效率地擷取的記錄開始的資料列索引和最大資料列參數的特定子集。 有很多的技術，可用來達到這個目的。 在前述我們探討了完成此教學課程中使用新的 Microsoft SQL Server 2005 的`ROW_NUMBER()`排名函數。 簡單地說，`ROW_NUMBER()`排名函數會將指派給傳回的查詢，並依照指定的排序順序的每個資料列的資料列數目。 然後藉由傳回特定的已編號的結果區段取得記錄的適當子集合。 下列查詢說明如何使用這項技術，以傳回這些產品編號 11 到 20 的排名結果依字母順序排序時`ProductName`:


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample1.sql)]

這項技術非常適合使用特定的排序次序的分頁 (`ProductName`依字母順序排序，在此情況下)，但是查詢必須修改顯示依不同的排序運算式排序的結果。 在理想情況下，上述的查詢可以改寫為使用中的參數`OVER`子句，就像這樣：


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample2.sql)]

不幸的是，參數化`ORDER BY`不允許子句。 相反地，我們必須建立可接受的預存程序`@sortExpression`輸入的參數，但會使用一種下列因應措施：

- 每個排序運算式可使用; 撰寫硬式編碼的查詢然後，使用`IF/ELSE`T-SQL 陳述式來判斷要執行的查詢。
- 使用`CASE`陳述式來提供動態`ORDER BY`運算式根據`@sortExpressio`n 輸入參數，請參閱 < 以動態方式排序查詢結果區段中用以[SQL 電源`CASE`陳述式](http://www.4guysfromrolla.com/webtech/102704-1.shtml)如需詳細資訊。
- 預存程序以字串形式製作適當的查詢，然後使用[`sp_executesql`系統預存程序](https://msdn.microsoft.com/library/ms188001.aspx)執行動態查詢。

每個這些因應措施有一些缺點。 無法為可維護與其他兩個因為它需要您建立的每個可能的排序運算式查詢的第一個選項。 因此，如果您稍後決定將新的、 可排序的欄位加入至 GridView 您也必須返回並更新預存程序。 第二種方法都有一些微妙的非字串的資料庫資料行排序時造成效能問題，而且也會受到這些相同的可維護性問題，第一個。 如果攻擊者能夠執行傳入輸入的參數值，他們所選擇的預存程序的第三個選擇，會使用動態 SQL，導入的 SQL 資料隱碼攻擊的風險。

雖然沒有任何一種方法是完美，我認為第三個選項最適合的三個。 它使用動態 SQL 中，它提供其他兩個不相符的彈性層的級。 此外，如果攻擊者能夠執行他所選擇的輸入參數中傳遞的預存程序，可以只利用 SQL 資料隱碼攻擊。 DAL 會使用參數化的查詢，因為 ADO.NET 會保護通過此架構中，資料庫會傳送這些參數表示 SQL 資料隱碼攻擊的弱點可能會只存在如果攻擊者可以直接執行預存程序。

若要實作這項功能，建立新的預存程序中名為 Northwind 資料庫`GetProductsPagedAndSorted`。 這個預存程序應該接受三個輸入參數： `@sortExpression`，輸入的參數的型別`nvarchar(100`)，指定如何排序結果，並插入之後`ORDER BY`中的文字`OVER`子句; 和`@startRowIndex`和`@maximumRows`，從相同的兩個整數輸入的參數`GetProductsPaged`預存程序檢查前述教學課程。 建立`GetProductsPagedAndSorted`預存程序使用下列指令碼：


[!code-sql[Main](sorting-custom-paged-data-vb/samples/sample3.sql)]

如此可確保的值會啟動預存程序`@sortExpression`已指定參數。 如果遺失，結果會根據項目`ProductID`。 接下來，動態 SQL 查詢會建構。 請注意動態 SQL 查詢此處從我們先前用來從 Products 資料表中擷取所有資料列的查詢稍有不同。 在先前範例中，我們取得每個產品 s 相關聯的類別目錄和供應商的名稱使用子查詢。 此決策提出回[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，而不使用完成`JOIN`s 因為 TableAdapter 無法自動建立相關聯的插入、 更新和刪除這種方法執行查詢。 `GetProductsPagedAndSorted`預存程序，不過，必須使用`JOIN`s 來分類或供應商名稱排序結果。

此動態查詢建立藉由串連靜態查詢部分和`@sortExpression`， `@startRowIndex`，和`@maximumRows`參數。 因為`@startRowIndex`和`@maximumRows`整數參數，它們必須先轉換成 nvarchars 才能正確的串連。 一旦已建構此動態 SQL 查詢，執行透過`sp_executesql`。

請花一點時間來測試不同的值與這個預存程序`@sortExpression`， `@startRowIndex`，和`@maximumRows`參數。 從 伺服器總管 中，以滑鼠右鍵按一下預存程序名稱，然後選擇 執行。 這會顯示 [執行預存程序] 對話方塊，您可以輸入的輸入的參數 （請參閱圖 1）。 若要依類別目錄名稱排序結果，使用 類別名稱的`@sortExpression`參數的值; 依供應商的公司名稱排序，請使用 供應商。 提供的參數值之後, 按一下 [確定]。 結果會顯示在 [輸出] 視窗中。 圖 2 顯示時由排序時，傳回產品排名 11 到 20 的結果`UnitPrice`以遞減的順序。


![嘗試不同的值的預存程序 s 三個輸入參數](sorting-custom-paged-data-vb/_static/image1.png)

**圖 1**： 預存程序 s 三個輸入參數，請嘗試不同的值


[![預存程序的結果會顯示在 [輸出] 視窗](sorting-custom-paged-data-vb/_static/image3.png)](sorting-custom-paged-data-vb/_static/image2.png)

**圖 2**: 預存程序的結果會顯示在 [輸出] 視窗 ([按一下以檢視完整大小的影像](sorting-custom-paged-data-vb/_static/image4.png))


> [!NOTE]
> 當藉由指定排名結果`ORDER BY`中的資料行`OVER`子句，SQL Server 必須排序結果。 這是快速的作業，如果沒有叢集的索引上的結果所排序的資料行，或者沒有涵蓋索引，但可否則成本更高。 若要改善夠大的查詢的效能，請考慮加入非叢集索引的結果依排序的資料行。 請參閱[排名函式和 SQL Server 2005 中的效能](http://www.sql-server-performance.com/ak_ranking_functions.asp)如需詳細資訊。


## <a name="step-2-augmenting-the-data-access-and-business-logic-layers"></a>步驟 2： 加強資料存取和商務邏輯層

與`GetProductsPagedAndSorted`建立預存程序中，我們的下一個步驟是提供一種機制來執行該預存程序，透過我們的應用程式架構。 這需要 DAL 和 BLL 加入適當的方法。 可讓 s dal 將一種方法來開始。 開啟`Northwind.xsd`具類型的資料集，以滑鼠右鍵按一下`ProductsTableAdapter`，，然後從內容功能表中選擇 加入查詢選項。 我們在先前的教學課程中我們想要設定這個新的 DAL 方法，以使用現有的預存程序- `GetProductsPagedAndSorted`，在此情況下。 開始會指出您想要使用現有的預存程序的新 TableAdapter 方法。


![選擇使用現有的預存程序](sorting-custom-paged-data-vb/_static/image5.png)

**圖 3**： 選擇使用現有的預存程序


若要指定要使用的預存程序，請選取`GetProductsPagedAndSorted`儲存程序，從下拉式清單中下一個畫面。


![使用 GetProductsPagedAndSorted 預存程序](sorting-custom-paged-data-vb/_static/image6.png)

**圖 4**： 使用 GetProductsPagedAndSorted 預存程序


這個預存程序傳回一組記錄，因為其結果，在下一個畫面中，表示它會傳回表格式資料。


![指出預存程序傳回表格式資料](sorting-custom-paged-data-vb/_static/image7.png)

**圖 5**： 指出預存程序傳回表格式資料


最後，建立 DAL 方法使用的填滿 DataTable，並傳回 DataTable 模式、 命名方法`FillPagedAndSorted`和`GetProductsPagedAndSorted`分別。


![選擇 方法名稱](sorting-custom-paged-data-vb/_static/image8.png)

**圖 6**： 選擇的方法名稱


現在我們 ve 擴充 DAL，我們將 BLL 準備好。 開啟`ProductsBLL`類別檔案，然後將新的方法， `GetProductsPagedAndSorted`。 這個方法只需要接受三個輸入參數`sortExpression`， `startRowIndex`，和`maximumRows`，應該直接向下呼叫至 DAL 的`GetProductsPagedAndSorted`方法，就像這樣：


[!code-vb[Main](sorting-custom-paged-data-vb/samples/sample4.vb)]

## <a name="step-3-configuring-the-objectdatasource-to-pass-in-the-sortexpression-parameter"></a>步驟 3： 設定 SortExpression 參數中傳遞至 ObjectDataSource

DAL 和 BLL，包括使用的方法具有夾帶`GetProductsPagedAndSorted`預存程序，是設定在 ObjectDataSource`SortParameter.aspx`頁面，即可使用新的 BLL 方法並傳入`SortExpression`參數為基礎使用者已要求來排序結果所依據的資料行。

藉由變更 ObjectDataSource s 啟動`SelectMethod`從`GetProductsPaged`至`GetProductsPagedAndSorted`。 這可以透過設定資料來源精靈，從 [屬性] 視窗中，或直接透過宣告式語法完成。 接下來，我們必須提供值給 ObjectDataSource s [ `SortParameterName`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sortparametername.aspx)。 如果設定這個屬性，ObjectDataSource 嘗試傳入 GridView s`SortExpression`屬性`SelectMethod`。 ObjectDataSource 特別的是，其名稱的值等於輸入參數會尋找`SortParameterName`屬性。 因為 BLL s`GetProductsPagedAndSorted`方法有名稱為排序的運算式輸入的參數`sortExpression`，設定 ObjectDataSource 的`SortExpression`sortExpression 的屬性。

這兩個變更之後，ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](sorting-custom-paged-data-vb/samples/sample5.aspx)]

> [!NOTE]
> 如先前的教學課程中，確認 ObjectDataSource 就*不*sortExpression、 startRowIndex、 或 maximumRows 輸入的參數集合中包含其 SelectParameters。


若要啟用在 GridView 中排序，只會檢查啟用排序 核取方塊在 GridView s 智慧標籤中，這會設定 GridView s`AllowSorting`屬性`true`，進而導致每個資料行轉譯為 LinkButton 標頭文字。 當使用者按一下其中一個標頭 LinkButtons 時，回傳展示和經過下列步驟：

1. GridView 更新其[`SortExpression`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.gridview.sortexpression.aspx)值`SortExpression`已按下其標頭連結的欄位
2. ObjectDataSource 叫用 BLL s`GetProductsPagedAndSorted`方法，傳入 GridView s`SortExpression`屬性做為方法的值`sortExpression`輸入的參數 (以及適當`startRowIndex`和`maximumRows`輸入參數值)
3. BLL 叫用 DAL 的`GetProductsPagedAndSorted`方法
4. DAL 執行`GetProductsPagedAndSorted`中傳遞預存程序，`@sortExpression`參數 (連同`@startRowIndex`和`@maximumRows`輸入參數值)
5. 預存程序會傳回適當的資料子集為 BLL，回到 ObjectDataSource;這項資料然後繫結至 GridView，轉譯為 HTML，並向下傳送給使用者

圖 7 顯示結果時，依排序的第一頁`UnitPrice`以遞增順序。


[![結果會依照 UnitPrice](sorting-custom-paged-data-vb/_static/image10.png)](sorting-custom-paged-data-vb/_static/image9.png)

**圖 7**: 結果會依照 UnitPrice ([按一下以檢視完整大小的影像](sorting-custom-paged-data-vb/_static/image11.png))


目前的實作可以正確排序結果的產品名稱、 類別名稱、 每個單位和單價的數量，而嘗試排列結果的順序與供應商名稱結果在執行階段例外狀況 （請參閱圖 8）。


![嘗試將排序結果，您可以在下列執行階段例外狀況的供應商結果](sorting-custom-paged-data-vb/_static/image12.png)

**圖 8**： 嘗試將排序結果，您可以在下列執行階段例外狀況的供應商結果


發生這個例外狀況，因為`SortExpression`的 GridView s `SupplierName` BoundField 設`SupplierName`。 供應商 s 中的名稱。 不過，`Suppliers`資料表實際上稱為`CompanyName`我們已經被別名做為此資料行名稱`SupplierName`。 不過，`OVER`子句使用`ROW_NUMBER()`函式不能使用別名，而且必須使用實際的資料行名稱。 因此，變更`SupplierName`BoundField 的`SortExpression`從供應商名稱至供應商 （請參閱圖 9）。 如圖 10 所示，這項變更後的結果可依供應商。


![變更 CompanyName 供應商名稱 BoundField 的 SortExpression](sorting-custom-paged-data-vb/_static/image13.png)

**圖 9**： 變更 CompanyName 供應商名稱 BoundField 的 SortExpression


[![現在可以依供應商排序結果](sorting-custom-paged-data-vb/_static/image15.png)](sorting-custom-paged-data-vb/_static/image14.png)

**圖 10**: 結果可立即依供應商 ([按一下以檢視完整大小的影像](sorting-custom-paged-data-vb/_static/image16.png))


## <a name="summary"></a>總結

我們探討前述教學課程的自訂分頁實作所需，您在設計階段指定之結果所要排序的順序。 簡單地說，這意味著，我們實作的自訂分頁實作無法，在相同的時間，提供排序功能。 在此教學課程中我們可以克服這項限制藉由擴充預存程序，從第一個包含`@sortExpression`結果無法排序的輸入的參數。

建立此預存程序和 DAL 和 BLL 中建立新的方法之後，我們能夠實作的 GridView 會提供這兩個排序和自訂分頁藉由設定傳入 GridView s 目前 ObjectDataSource `SortExpression` BLL 屬性`SelectMethod`.

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 Carlos Santos。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](efficiently-paging-through-large-amounts-of-data-vb.md)
[下一頁](creating-a-customized-sorting-user-interface-vb.md)
