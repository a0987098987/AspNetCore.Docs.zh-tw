---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: 有效率地分頁大量資料 (VB) |Microsoft Docs
author: rick-anderson
description: 使用大量的資料，作為其基礎資料來源控制項 retriev 時，不適合資料簡報控制項的預設分頁選項...
ms.author: aspnetcontent
ms.date: 08/15/2006
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 9b00e18287bdb791a353b7ebd1bbb6cc0ab586b5
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37805502"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>有效率地分頁大量資料 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe)或[下載 PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> 資料簡報控制項的預設分頁選項時，不適合使用大量的資料，當其基礎資料來源控制項擷取所有的記錄，即使在顯示的資料子集。 必須在此情況下，我們開啟自訂分頁。


## <a name="introduction"></a>簡介

如我們所討論在先前的教學課程中，您可以實作分頁中有兩種,：

- **預設的分頁**可實作只要選取 [啟用分頁] 選項中的資料 Web 控制項 s 智慧標籤; 不過，只要檢視的資料頁，ObjectDataSource 擷取*所有*的記錄，甚至是不過它們的子集會顯示在頁面
- **自訂分頁**可改善效能的預設分頁擷取只有這些記錄從資料庫中要顯示使用者所要求資料的特定頁面，不過，自訂分頁牽涉到多一點工夫來實作比預設分頁

由於易於實作只核取核取方塊，且您完成了 ！ 預設的分頁是一個不錯的選擇。 在擷取的所有記錄，其 na ve 方法，可讓 implausible 的選擇時分頁夠大的資料或站台的數量，與許多並行使用者。 在此情況下，我們必須啟用自訂分頁以提供回應的系統。

自訂分頁的挑戰要能夠撰寫可傳回的精確記錄所需的特定頁面的資料集的查詢。 幸運的是，Microsoft SQL Server 2005 提供新的關鍵字排名結果，這讓我們撰寫可以有效率地擷取記錄的適當子集的查詢。 在本教學課程中，我們會看到如何使用這個新的 SQL Server 2005 關鍵字 GridView 控制項中實作自訂分頁。 使用者介面的自訂分頁時相同的預設分頁時，從一頁跳到下一步 時使用自訂分頁可以的差距速度比預設分頁。

> [!NOTE]
> 自訂分頁，所展現的確切的效能改善取決於正在呼叫透過記錄並放在資料庫伺服器上的負載的總數。 在本教學課程結尾處我們將探討一些粗略的計量，展示透過自訂分頁所獲得的效能優點。


## <a name="step-1-understanding-the-custom-paging-process"></a>步驟 1： 了解自訂的分頁處理程序

當資料進行分頁，顯示在頁面中的精確記錄會依所要求的資料頁和每頁顯示的記錄數目而定。 例如，想像一下，我們想要透過 81 的產品，頁面上顯示每頁 10 項產品。 當您檢視第一頁，d 希望產品 1 到 10;檢視第二個頁面時 d 我們會想要的產品 11 到 20，依此類推。

有三個變數，以指定要擷取需要的記錄及呈現分頁介面的方式：

- **啟動 資料列索引**要顯示的資料頁面中的第一列的索引; 此索引的頁面索引乘以每頁顯示的記錄，並新增一個計算。 比方說，當分頁 （其頁面索引為 0） 的第一頁的資料錄 10 次，啟動資料列索引為 0 \* 10 + 1 或 1; 第二個頁面 （頁面索引為 1），開始的資料列索引為 1 \* 10 + 1或 11。
- **最大資料列**每頁顯示的資料錄的數目上限。 此變數被指最大資料列因為最後一個頁面有可能是較少的記錄傳回比頁面大小。 比方說，當每個頁面的 81 產品 10 記錄進行分頁，第九個和最後一個頁面必須只有一項記錄。 不過，任何頁面上，會不顯示較多的記錄超過最大資料列的值。
- **總計記錄計數**透過正在分頁的記錄總數。 雖然此變數不是 t 需要判斷哪些記錄來擷取指定的頁面時，它會指示分頁介面。 比方說，如果有 81 產品正在透過分頁，分頁介面知道分頁 UI 中顯示九個的頁碼。

使用預設分頁開始的資料列索引會計算為頁面索引和頁面大小加上一個項目，產品，而最大資料列，則只是頁面大小。 因為預設的分頁，擷取所有記錄的是已知資料庫轉譯資料，每個資料列索引的任何頁面時，藉此讓 移至啟動的資料列索引資料列是簡單的工作。 此外，記錄總數是供使用，因為它 s DataTable （或任何物件用來保存資料庫的結果） 中的記錄數目。

指定在開始資料列索引和最大資料列變數，自訂的分頁實作必須只傳回之後，開始啟動資料列索引和最多的資料錄的最大資料列數目的記錄的精確子集。 自訂分頁提供兩個挑戰：

- 我們必須能夠有效率地將資料列索引與整個資料，以便我們可以開始傳回指定的開始資料列索引處的記錄，透過正在分頁中的每個資料列產生關聯
- 我們需要提供透過正在分頁的記錄總數

在下面兩個步驟中，我們將檢驗這些兩個挑戰回應所需的 SQL 指令碼。 除了 SQL 指令碼中，我們也要 BLL 和 DAL 中實作方法。

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>步驟 2： 傳回透過正在分頁的記錄總數

我們將探討如何擷取所顯示的網頁記錄的精確子集之前，讓 s 先看看如何傳回透過正在分頁的記錄總數。 這項資訊才能正確地設定 [分頁] 使用者介面。 可以使用來取得特定的 SQL 查詢所傳回的記錄總數[`COUNT`彙總函式](https://msdn.microsoft.com/library/ms175997.aspx)。 例如，若要判斷中的記錄總數`Products`資料表中，我們可以使用下列查詢：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

可讓新增方法以傳回這項資訊我們 DAL 的 s。 特別的是，我們將建立一個稱為的 DAL 方法`TotalNumberOfProducts()`執行`SELECT`如上所示的陳述式。

首先開啟`Northwind.xsd`中的型別資料集檔案`App_Code/DAL`資料夾。 接下來，以滑鼠右鍵按一下`ProductsTableAdapter`設計工具中，然後選擇 加入查詢。 因為我們已在先前的教學課程中看到這可讓我們新增對 DAL 的方法，叫用時，會執行特定 SQL 陳述式或預存程序。 如同我們在先前的教學課程中的 TableAdapter 方法，這次選擇使用特定 SQL 陳述式。


![使用特定 SQL 陳述式](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**圖 1**： 使用特定 SQL 陳述式


在下一個畫面中，我們可以指定何種查詢來建立。 因為此查詢會傳回單一純量值中的記錄總數`Products`資料表選擇`SELECT`傳回單一值 選項。


![設定用於傳回單一值的 SELECT 陳述式的查詢](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**圖 2**： 設定用於傳回單一值的 SELECT 陳述式的查詢


之後，指出要使用的查詢類型，我們接下來必須指定查詢。


![使用選取的 COUNT(*) 從產品的查詢](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**圖 3**： 使用 SELECT COUNT (\*) FROM 產品查詢


最後，指定方法的名稱。 使用上述、 let s `TotalNumberOfProducts`。


![命名 DAL 方法 TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**圖 4**： 命名 DAL 方法 TotalNumberOfProducts


之後按一下 [完成]，精靈會將`TotalNumberOfProducts`DAL 的方法。 DAL 中的純量傳回方法會傳回可為 null 的型別，如果 SQL 查詢的結果是`NULL`。 我們`COUNT`查詢，不過，一律會傳回非`NULL`值，不論如何，DAL 方法會傳回可為 null 的整數。

除了使用 DAL 方法時，我們也會需要到 BLL 中的方法。 開啟`ProductsBLL`類別檔案，然後將`TotalNumberOfProducts`方法，只會呼叫向下到 DAL`TotalNumberOfProducts`方法：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s`TotalNumberOfProducts`方法會傳回可為 null 的整數; 不過，我們建立的 ve`ProductsBLL`類別的`TotalNumberOfProducts`方法，讓它傳回標準的整數。 因此，我們需要有`ProductsBLL`類別 s`TotalNumberOfProducts`方法會傳回可為 null 的整數 DAL s 所傳回的值部分`TotalNumberOfProducts`方法。 若要在呼叫`GetValueOrDefault()`傳回的值可為 null 的整數，如果有的話，可為 null 的整數是否`null`，不過，它會傳回預設的整數值為 0。

## <a name="step-3-returning-the-precise-subset-of-records"></a>步驟 3： 傳回記錄的精確子集

下一步是建立 DAL 和 BLL 接受開始的資料列索引中的方法和稍早所述最大資料列變數，並傳回適當的記錄。 這樣做之前，可讓 s 先來看看所需的 SQL 指令碼。 我們面臨的挑戰是，我們必須能夠有效率地將索引指派給每個資料列中，這樣我們就可以還原起始數量為啟動的資料列索引 （最多的記錄最大記錄數目） 的記錄，透過正在分頁的整個結果。

如果做為資料列索引的資料庫資料表中已經有資料行，這是不一項挑戰。 第一眼看我們可能會認為`Products`表格 s`ProductID`欄位即已足夠，因為第一項產品有`ProductID`為 1，2，第二個，依此類推。 不過，正在刪除產品會留下間距的順序，請取消這種方法。

有兩個一般技巧可用來有效率地將資料列索引產生關聯的資料進行分頁，因此會啟用要擷取的記錄的精確子集：

- **使用 SQL Server 2005 s`ROW_NUMBER()`關鍵字**到 SQL Server 2005、 新`ROW_NUMBER()`關鍵字關聯每個傳回的記錄，根據某些排序次序。 這個等級可以作為每個資料列的資料列索引。
- **使用資料表變數和`SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT`陳述式](https://msdn.microsoft.com/library/ms188774.aspx)可用來指定查詢應處理終止; 之前的總記錄數目[資料表變數](http://www.sqlteam.com/item.asp?ItemID=9454)是可以保存表格式資料、 akin 至本機 T-SQL 變數[暫存資料表](http://www.sqlteam.com/item.asp?ItemID=2029)。 這個方法同樣適用於 Microsoft SQL Server 2005 和 SQL Server 2000 (而`ROW_NUMBER()`方法僅適用於 SQL Server 2005)。  
  
  這裡的概念是建立資料表變數具有`IDENTITY`資料行和資料表的主要金鑰，透過呼叫其中的資料，資料行。 接下來，透過呼叫其中的資料，資料表的內容傾印到資料表變數中，藉此建立關聯的循序資料列的索引 (透過`IDENTITY`資料行) 資料表中的每一筆記錄。 一旦已填入此資料表變數，`SELECT`資料表變數中，陳述式與基礎資料表的聯結，可以執行，以提取出特定的記錄。 `SET ROWCOUNT`陳述式用來以智慧方式限制要傾印到資料表變數的記錄數目。  
  
  這種方法的效率根據所要求的頁面數目為`SET ROWCOUNT`值會指派值開始的資料列索引，再加上最大資料列。 透過低編號的頁面，例如第一個分頁的資料的幾個頁面時這種方法是非常有效率。 不過，它所表現的預設分頁類似的效能時擷取即將結束頁面。

本教學課程可讓您實作自訂分頁使用`ROW_NUMBER()`關鍵字。 如需有關使用資料表變數和`SET ROWCOUNT`技巧，請參閱 <<c2> [ 詳細有效率的方法之分頁透過大型結果集](http://www.4guysfromrolla.com/webtech/042606-1.shtml)。

`ROW_NUMBER()`透過使用下列語法的特定順序傳回每一筆記錄與關鍵字相關聯的等級：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` 傳回指定的陣序與指定的順序的每一筆記錄的數值。 例如，若要查看每項產品的順序排序大部分的陣序成本最低，我們可以使用下列查詢：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

圖 5 顯示這項查詢 s 透過 Visual Studio 中的 查詢 視窗中執行時的結果。 請注意，產品會按照價格，以及每個資料列的價格陣序規範。


![價格陣序規範所包含的每個傳回的記錄](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**圖 5**: 價格陣序規範所包含的每個傳回的記錄


> [!NOTE]
> `ROW_NUMBER()` 有許多新的排名函數，其中有一個 SQL Server 2005。 如需更深入瞭解`ROW_NUMBER()`，以及其他排名函式，讀取[傳回等級結果與 Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml)。


當指定排名結果`ORDER BY`中的資料行`OVER`子句 (`UnitPrice`，在上述範例中)，SQL Server 必須排序結果。 這是快速的作業，如果沒有叢集的索引上的結果，依排序的資料行，或是否涵蓋編製索引，但可以否則成本更高。 若要改善夠大的查詢的效能，請考慮將結果依排序的資料行的非叢集索引。 請參閱[排名函式和 SQL Server 2005 中的效能](http://www.sql-server-performance.com/ak_ranking_functions.asp)的效能考量，詳細查看。

所傳回的等級資訊`ROW_NUMBER()`無法直接用於`WHERE`子句。 不過，使用衍生的資料表，傳回`ROW_NUMBER()`結果，然後可以出現在`WHERE`子句。 例如，下列查詢會使用衍生的資料表傳回產品名稱和單價的資料行，連同`ROW_NUMBER()`結果，然後再使用`WHERE`子句只傳回這些產品的價格陣序規範為 11 到 20 之間：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

擴充這個概念更進一步，我們可以利用這種方法來擷取資料所需的啟動資料列索引和資料列的上限值的特定頁面：


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> 我們將在本教學課程中，於稍後看到*`StartRowIndex`* 所提供的 ObjectDataSource 會編製索引開頭為零，而`ROW_NUMBER()`SQL Server 2005 所傳回的值編製索引從 1 開始。 因此，`WHERE`子句會傳回的記錄位置`PriceRank`必定大於*`StartRowIndex`* 且小於或等於*`StartRowIndex`*  + *`MaximumRows`*.


現在我們已討論過如何`ROW_NUMBER()`可以是用來擷取特定分頁開始的資料列索引和資料列數上限值的資料，我們現在必須實作此邏輯為 BLL 和 DAL 中的方法。

建立此查詢，我們必須決定的順序時的結果排列次序;可讓 s 依其名稱的字母順序排序的產品。 這表示，使用自訂分頁實作在本教學課程中我們將無法建立自訂的分頁的報表，也可以儲存非。 在下一個教學課程中，不過，我們會看到如何可以提供這類功能。

在上一節中，我們會建立 DAL 方法當成特定 SQL 陳述式。 不幸的是，在 Visual Studio 中使用 TableAdapter 精靈不 t，例如 T-SQL 剖析器`OVER`所使用的語法`ROW_NUMBER()`函式。 因此，我們必須建立這個 DAL 方法做為預存程序。 從 [檢視] 功能表 （或叫用的 Ctrl + Alt + S） 中選取 [伺服器總管] 中，展開`NORTHWND.MDF`節點。 若要加入新的預存程序，以滑鼠右鍵按一下 預存程序 節點，並選擇 加入新的預存程序 （請參閱 圖 6）。


![加入新的預存程序，透過產品的分頁](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**圖 6**： 加入新的預存程序，透過產品的分頁


這個預存程序應該接受兩個整數輸入的參數-`@startRowIndex`和`@maximumRows`並用`ROW_NUMBER()`函式依`ProductName`欄位中，僅傳回那些資料列大於指定`@startRowIndex`和小於或相等`@startRowIndex`  +  `@maximumRow` s。 下列指令碼輸入新的預存程序，然後按一下 加入資料庫中的預存程序的 儲存 圖示。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

建立預存程序之後, 請花一點時間加以測試。以滑鼠右鍵按一下`GetProductsPaged`預存程序 [伺服器總管] 中的名稱，然後選擇 [執行] 選項。 Visual Studio 接著會提示您輸入的參數，如`@startRowIndex`和`@maximumRow`s （請參閱 圖 7）。 請嘗試不同的值，並檢查結果。


![輸入的值@startRowIndex和@maximumRows參數](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>圖 7</strong>： 輸入的值@startRowIndex和@maximumRows參數


在之後選擇這些輸入參數值，[輸出] 視窗會顯示結果。 圖 8 顯示兩個 10 中傳遞時的結果`@startRowIndex`和`@maximumRows`參數。


[![會傳回記錄，就會出現在第二個頁面的資料](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**圖 8**: 記錄，就會出現在第二個頁面的資料會傳回 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


與這個預存程序建立，我們準備好建立`ProductsTableAdapter`方法。 開啟`Northwind.xsd`具類型的資料集，以滑鼠右鍵按一下`ProductsTableAdapter`，然後選擇 加入查詢選項。 而不是建立使用特定 SQL 陳述式的查詢，建立使用現有的預存程序。


![建立使用現有的預存程序的 DAL 方法](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**圖 9**： 建立使用現有的預存程序的 DAL 方法


接下來，我們會提示您選取要叫用的預存程序。 挑選`GetProductsPaged`預存程序，從下拉式清單。


![選擇 GetProductsPaged 預存程序，從下拉式清單](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**圖 10**： 選擇 GetProductsPaged 預存程序，從下拉式清單


下一個畫面則會要求您的資料類型由預存程序： 表格式資料、 單一值或沒有值。 因為`GetProductsPaged`預存程序可以傳回多筆記錄，表示它會傳回表格式資料。


![指出預存程序會傳回表格式資料](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**圖 11**： 指出預存程序會傳回表格式資料


最後，指明您要建立方法的名稱。 如同我們先前的教學課程，請繼續使用這兩種填滿方法 DataTable 及建立傳回 DataTable。 第一個方法命名`FillPaged`而第二個`GetProductsPaged`。


![名稱方法 FillPaged 和 GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**圖 12**： 名稱方法 FillPaged 和 GetProductsPaged


此外若要建立 DAL 方法，以傳回特定頁面的產品，我們也要提供這類 BLL 中的功能。 DAL 方法，例如 BLL 的 GetProductsPaged 方法必須接受兩個整數輸入來指定開始的資料列索引和最大資料列，並必須傳回只在指定的範圍內的記錄。 建立這類 BLL 方法在 ProductsBLL 類別只是呼叫向下的 DAL 的 GetProductsPaged 方法，就像這樣：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

您可以使用任何名稱對於 BLL 方法 s 輸入參數，但就像我們在不久之後，會選擇使用`startRowIndex`和`maximumRows`命名可以免去從額外的設定才能使用此方法的 ObjectDataSource 時的工作。

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>步驟 4： 設定要使用自訂分頁 ObjectDataSource

存取完整的記錄的特定子集的 BLL 和 DAL 方法，我們準備好建立 GridView 控制該頁面，透過其基礎的記錄，使用自訂分頁。 首先開啟`EfficientPaging.aspx`頁面中`PagingAndSorting`資料夾中，加入頁面上的 GridView，並將它設定為使用新的 ObjectDataSource 控制項。 我們過去的教學課程中，我們通常必須設定為使用 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 此時，不過，我們想要使用`GetProductsPaged`方法相反地，因為`GetProducts`方法會傳回*所有*資料庫中的產品而`GetProductsPaged`傳回特定部分的記錄。


![設定為使用 ProductsBLL 類別的 GetProductsPaged 方法的 ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**圖 13**： 設定為使用 ProductsBLL 類別的 GetProductsPaged 方法的 ObjectDataSource


之後重新建立唯讀的 GridView，花一點時間來設定 方法 下拉式清單中插入、 更新和刪除 （無） 索引標籤。

接下來，ObjectDataSource 精靈會提示我們輸入的來源`GetProductsPaged`方法 s`startRowIndex`和`maximumRows`輸入參數值。 這些輸入的參數實際上為 GridView 會自動因此保留來源設定為 None，按一下 [完成]。


![保留為 「 無的輸入的參數來源](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**圖 14**： 保留為 「 無的輸入的參數來源


完成 ObjectDataSource 精靈之後，GridView 會包含 BoundField 或 CheckBoxField 每個產品的資料欄位。 請隨意適當地調整 GridView 外觀。 我選擇只顯示 ve `ProductName`， `CategoryName`， `SupplierName`， `QuantityPerUnit`，和`UnitPrice`BoundFields。 此外，設定 GridView，以支援分頁，藉由檢查它的智慧標籤中啟用分頁 核取方塊。 在這些變更之後, 的 GridView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

如果您瀏覽透過瀏覽器頁面，不過，GridView 是位置來找到。


![GridView 會不會顯示](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**圖 15**: 的 GridView 會不會顯示


GridView 遺漏，因為 ObjectDataSource 是目前正在使用 0 做為值的兩個`GetProductsPaged``startRowIndex`和`maximumRows`輸入參數。 因此，產生的 SQL 查詢會傳回任何記錄，並因此 GridView 不會顯示。

若要解決此問題，我們需要設定要使用自訂分頁 ObjectDataSource。 這可以完成下列步驟：

1. **設定 ObjectDataSource s`EnablePaging`屬性，以`true`** 這表示它必須將它傳遞至 ObjectDataSource`SelectMethod`兩個額外參數： 一個用來指定開始的資料列索引 ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx))，和一個用來指定最大資料列 ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx))。
2. **設定 ObjectDataSource s`StartRowIndexParameterName`並`MaximumRowsParameterName`據此屬性**`StartRowIndexParameterName`並`MaximumRowsParameterName`屬性會指示傳入輸入參數的名稱`SelectMethod`自訂分頁進行。 根據預設，這些參數名稱為`startIndexRow`並`maximumRows`，這就是為什麼要建立時`GetProductsPaged`方法在 BLL，我必須使用這些值的輸入參數。 如果您選擇使用不同的參數名稱，對於 BLL s`GetProductsPaged`方法，例如`startIndex`並`maxRows`，則請針對您想要的範例設定 ObjectDataSource s`StartRowIndexParameterName`和`MaximumRowsParameterName`屬性據以 （例如針對 startIndex`StartRowIndexParameterName`並為 maxRows `MaximumRowsParameterName`)。
3. **設定 ObjectDataSource s [ `SelectCountMethod`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx)的總計數字的記錄正在分頁會透過傳回的方法名稱 (`TotalNumberOfProducts`)** 請記得，`ProductsBLL`類別的`TotalNumberOfProducts`方法會傳回透過執行的 DAL 方法正在分頁的記錄總數`SELECT COUNT(*) FROM Products`查詢。 ObjectDataSource 需要這項資訊才能正確地呈現分頁介面。
4. **移除`startRowIndex`並`maximumRows``<asp:Parameter>`從 ObjectDataSource s 宣告式標記的項目**在設定 ObjectDataSource 執行精靈時，Visual Studio 會自動加入兩個`<asp:Parameter>`項目針對`GetProductsPaged`方法 s 的輸入參數。 藉由設定`EnablePaging`要`true`，將會自動傳遞這些參數; 它們也會出現在宣告式語法中，如果 ObjectDataSource 會嘗試傳遞*四*參數`GetProductsPaged`方法與兩個參數`TotalNumberOfProducts`方法。 如果您忘了移除這些`<asp:Parameter>`項目，瀏覽的頁面，透過瀏覽器就會顯示如下的錯誤訊息： *ObjectDataSource 'ObjectDataSource1' 找不到 přepisuje neobecnou metodu 'TotalNumberOfProducts' 具有參數： startRowIndex，maximumRows*。

進行這些變更之後，ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

請注意，`EnablePaging`並`SelectCountMethod`已設定屬性和`<asp:Parameter>`已移除項目。 [圖 16] 顯示 [屬性] 視窗的螢幕擷取畫面進行這些變更之後。


![若要使用自訂分頁，設定 ObjectDataSource 控制項](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**圖 16**： 若要使用自訂分頁，設定 ObjectDataSource 控制項


進行這些變更之後，請瀏覽此頁面，透過瀏覽器。 您應該會看到列出，10 項產品按字母順序排序。 請花一點時間逐步一頁資料一次。 自訂分頁更有效率地進行大量資料頁面從使用者觀點的 s 預設分頁與自訂分頁之間的視覺化差異時，，因為它只會擷取所需為指定的頁面會顯示這些記錄使用。


[![資料、 依產品名稱、 Ordered 是分頁使用自訂分頁](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**圖 17**: 資料、 依產品名稱、 Ordered 是分頁使用自訂分頁 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> 使用自訂分頁時，頁面計數 ObjectDataSource s 所傳回的值`SelectCountMethod`會儲存在 GridView 的檢視狀態。 其他的 GridView 變數`PageIndex`， `EditIndex`， `SelectedIndex`，`DataKeys`集合，並依此類推會儲存在*控制狀態*，其中會保存的 GridView s 的值為何`EnableViewState`屬性。 因為`PageCount`值保存在回傳時使用的分頁介面包含連結帶您前往最後一頁，使用檢視狀態，請務必啟用 GridView 的檢視狀態。 （如果分頁介面沒有直接連結到最後一個頁面上，則您可能會停用檢視狀態）。


按一下最後一個頁面連結造成回傳，並指示 GridView，以更新其`PageIndex`屬性。 按一下最後一個頁面連結時，如果 GridView 會將指派其`PageIndex`屬性設為其中一個值小於其`PageCount`屬性。 停用檢視狀態`PageCount`值將會遺失在回傳和`PageIndex`改為指派的最大整數值。 GridView 接下來，會嘗試判斷起始的資料列索引乘以`PageSize`和`PageCount`屬性。 這會導致`OverflowException`因為產品超過允許的最大整數大小。

## <a name="implement-custom-paging-and-sorting"></a>實作自訂分頁和排序

我們目前的自訂分頁實作需要的資料分頁透過的順序指定以靜態方式建立時`GetProductsPaged`預存程序。 不過，您可能已記下 GridView s 智慧標籤會包含除了啟用分頁 選項之外，啟用排序核取方塊。 不幸的是，加入我們目前的自訂分頁實作 GridView 的排序支援時，只會排序資料的目前檢視的網頁上的記錄。 比方說，如果您設定也支援分頁，並檢視資料的第一頁時再，依產品名稱，依遞減順序排序 GridView 它就會在第 1 頁上反轉產品的順序。 如 [圖 18] 所示，例如顯示初探為第一項產品以反向字母順序，會忽略 71 其他產品隨附之後初探，依字母順序，排序時第一頁上的這些記錄會被視為在排序。


[![只顯示資料目前頁面上排序](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**圖 18**： 只顯示資料目前頁面上的排序 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


因為排序作業後已擷取的資料，從 BLL s 排序只適用於資料的目前頁面`GetProductsPaged`方法，並且此方法只會傳回特定網頁的記錄。 若要實作正確排序，我們需要將排序運算式，以`GetProductsPaged`方法，以便資料可以適當地排序傳回的資料的特定頁面之前。 我們將了解如何在我們的下一個教學課程中完成這項作業。

## <a name="implementing-custom-paging-and-deleting"></a>實作自訂分頁和刪除

如果您啟用的 GridView，其資料使用自訂分頁技術，您會刪除從最後一頁的最後一筆記錄時發現的分頁中的刪除功能的 GridView 會消失而不是適當減量 GridView 的`PageIndex`. 若要重現這個錯誤，啟用刪除本教學課程只是我們剛才建立的。 移至最後一頁 （第 9 頁），因為我們會透過 81 產品，一次 10 項產品分頁，應該看到單一產品。 刪除此產品。

一旦刪除最後一個產品，也就是 GridView*應該*自動移至第八個頁面中，並使用預設分頁時的這類功能時顯露出來。 使用自訂分頁時，不過，在刪除最後一個產品的最後一頁之後, GridView 直接從畫面消失完全。 精確的原因*為什麼*發生這種情況有點已超出本教學課程的範圍，請參閱[刪除最後一個記錄的最後一頁從使用自訂分頁 GridView](http://scottonwriting.net/sowblog/posts/7326.aspx)的低層級的詳細資訊，告訴的來源此問題。 總之它因為下列一連串步驟，按一下 [刪除] 按鈕時，由 GridView 的 s:

1. 刪除記錄
2. 取得適當的記錄，以顯示指定`PageIndex`和 `PageSize`
3. 檢查，確定`PageIndex`不會超過資料來源中的資料頁數，如果它存在，會自動遞減 GridView 的`PageIndex`屬性
4. 將適當的頁面資料的繫結至 GridView 使用在步驟 2 中取得的記錄

問題的起因是該在步驟 2`PageIndex`時，使用擷取要顯示的記錄仍然`PageIndex`就已刪除其唯一記錄的最後一頁。 因此，在步驟 2*沒有*會傳回記錄，因為該最後一頁的資料不再包含任何記錄。 然後，在步驟 3: GridView 發現，其`PageIndex`屬性大於資料來源中的頁面總數 （因為我們已刪除中的最後一頁的最後一個記錄），因此將其`PageIndex`屬性。 步驟 4 中的 GridView 會嘗試繫結到在步驟 2 中，擷取的資料不過，在步驟 2 中不傳回的任何記錄，因此導致空的 GridView。 使用預設分頁時，此問題不 t 介面因為在步驟 2*所有*從資料來源擷取記錄。

若要修正此問題，我們會有兩個選項。 第一個是建立事件處理常式 GridView s`RowDeleted`決定只是已刪除的頁面中所顯示的多少筆記錄的事件處理常式。 如果發生只有一筆記錄，然後將它刪除的記錄必須已經被最後一個，我們需要遞減 GridView 的`PageIndex`。 當然，我們只想要更新`PageIndex`實際上已成功刪除作業時，這可藉由確保`e.Exception`屬性是`null`。

這種方式適用，因為它會更新`PageIndex`在步驟 1 之後但在步驟 2 之前。 因此，在步驟 2 中，會傳回適當的記錄集。 若要達成此目的，使用程式碼，如下所示：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

一種解決方法是建立 ObjectDataSource s 的事件處理常式`RowDeleted`事件，並設定`AffectedRows`屬性設為 1 的值。 GridView 更新後刪除記錄在步驟 1 中 （但然後再重新擷取步驟 2 中的資料），其`PageIndex`如果一或多個資料列作業所影響的屬性。 不過，`AffectedRows`屬性未設定 ObjectDataSource 的因此略過此步驟。 具有執行此步驟的一種方法是手動設定`AffectedRows`如果順利完成刪除作業的屬性。 這可以使用如下的程式碼來完成：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

這兩個這些事件處理常式的程式碼可在程式碼後置類別`EfficientPaging.aspx`範例。

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>預設和自訂分頁的效能比較

因為自訂分頁只擷取所需的記錄，則傳回預設的分頁*所有*的每個頁面記錄檢視，它 s 清除自訂分頁的效率高於預設分頁。 但是就是如何更有效率的是自訂分頁嗎？ 從預設的分頁移至自訂分頁，可以看到什麼樣的效能提升？

不幸的是，沒有 s 沒有一個大小是否符合所有這裡回答。 提升的效能取決於許多因素，最著名兩位透過正在分頁的記錄和負載的數字放在 web 伺服器和資料庫伺服器之間的資料庫伺服器和通訊通道。 對於具有少數數十個記錄的小型資料表，效能差異可能微不足道。 對於大型資料表時，有數千到數十萬個資料列，不過，效能差異。 嚴重

我的發行項[SQL Server 2005 的 ASP.NET 2.0 中的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)，包含造成效能時逐頁查看的資料庫資料表有下列兩個分頁技術之間的差異執行一些效能測試50,000 筆記錄。 在這些測試中，我檢查的時間來執行 SQL 伺服器層級的查詢 (使用[SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) 並在 ASP.NET 網頁使用[ASP.NET 追蹤功能，s](https://msdn.microsoft.com/library/y13fw6we.aspx)。 請記住，這些測試已在使用單一的作用中使用者，我開發電腦上執行，因此非，不會模擬一般網站的負載模式。 不論如何，結果將說明的預設和自訂分頁的執行時間的相對差異時使用夠大的資料量。


|  | **Avg.持續時間 （秒）** | **讀取** |
| --- | --- | --- |
| **預設分頁 SQL Profiler** | 1.411 | 383 |
| **自訂的分頁 SQL Profiler** | 0.002 | 29 |
| **預設的分頁 ASP.NET 追蹤** | 2.379 | *N/A* |
| **自訂的分頁 ASP.NET 追蹤** | 0.029 | *N/A* |


如您所見，擷取資料的特定頁面平均需要較少讀取 354，完成一小部分的時間。 在 ASP.NET 頁面中，自訂頁面無法呈現在接近 1/100<sup>th</sup>所花費時間時使用的預設分頁。 請參閱[我的文章](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)如對這些結果，以及程式碼和資料庫的詳細資訊，您可以下載重現您自己的環境中的這些測試。

## <a name="summary"></a>總結

預設的分頁是非常容易實作只檢查資料 Web 控制項 s 智慧標籤中啟用分頁的核取方塊，但這類簡單代價效能。 使用預設分頁時，當使用者要求資料的任何頁面*所有*會傳回記錄，即使只有其中一小部分可能會顯示。 若要克服這樣的效能負荷，ObjectDataSource 會提供替代分頁選項自訂分頁。

藉由擷取所需顯示，這些記錄分頁 s 效能問題的預設值為基礎加以改進自訂分頁時它 s 更為複雜，若要實作自訂分頁。 首先，您必須正確 （且有效率地） 存取要求的記錄的特定子集是撰寫查詢。 這可在數種方式;在本教學課程中，我們檢查的是使用新的 SQL Server 2005 的`ROW_NUMBER()`陣序規範的函式的結果，然後只傳回結果的排名落在指定的範圍內。 此外，我們需要新增方法以確定透過正在分頁的記錄總數。 建立之後這些 DAL 和 BLL 方法，我們也需要設定 ObjectDataSource，讓它可以判斷多少的記錄總數正在透過分頁，以及可以正確地啟動資料列索引和最大資料列將值傳遞至 BLL。

實作自訂分頁確實需要一些步驟，而且幾乎不簡單，只要預設的分頁功能，而自訂分頁時，需要夠大量的資料進行分頁。 檢查結果顯示的自訂分頁可以減輕從 ASP.NET 頁面轉譯時間的秒數，可以一或多個大幅度減輕資料庫伺服器上的負載。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](paging-and-sorting-report-data-vb.md)
> [下一頁](sorting-custom-paged-data-vb.md)
