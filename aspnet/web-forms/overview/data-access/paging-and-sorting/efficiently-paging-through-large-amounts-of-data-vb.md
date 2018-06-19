---
uid: web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
title: 有效率地進行大量的資料 (VB) 分頁 |Microsoft 文件
author: rick-anderson
description: 使用大量的資料，做為其基礎資料來源控制項 retriev 時，不適合資料簡報控制項之預設分頁選項...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/15/2006
ms.topic: article
ms.assetid: 3e20e64a-8808-4b49-88d6-014e2629d56f
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb
msc.type: authoredcontent
ms.openlocfilehash: 00057f9bfd9b1c479e500ac591db694388a5d358
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890693"
---
<a name="efficiently-paging-through-large-amounts-of-data-vb"></a>有效率地大量的資料 (VB) 進行分頁
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/9/c/1/9c1d03ee-29ba-4d58-aa1a-f201dcc822ea/ASPNET_Data_Tutorial_25_VB.exe)或[下載 PDF](efficiently-paging-through-large-amounts-of-data-vb/_static/datatutorial25vb1.pdf)

> 呈現控制項的預設分頁選項時，不適合處理大量的資料，當其基礎資料來源控制項擷取所有的記錄，即使在顯示的資料子集。 必須在這種情況下，我們開啟自訂分頁。


## <a name="introduction"></a>簡介

如我們所討論前述教學課程中，可以實作分頁在兩種方式之一：

- **預設分頁**可以只檢查啟用分頁 選項來實作資料 Web 控制項 s 中的智慧標籤; 不過，只要檢視的資料頁面，ObjectDataSource 擷取*所有*的記錄，即使雖然它們的子集也會顯示在頁面
- **自訂分頁**可改善效能的預設值，藉由擷取只需要特定的使用者; 要求的資料頁面會顯示資料庫中的分頁不過，自訂分頁牽涉到更多工作來實作比預設分頁

由於實作只核取核取方塊，並且重新輕鬆完成 ！ 預設分頁是理想的選擇。 在擷取的所有記錄，其 na ve 方法，可讓 implausible 選擇夠大的資料或站台的數量進行分頁具有許多並行使用者時。 在這種情況下，我們必須開啟自訂分頁才能提供回應的系統。

自訂分頁的挑戰能夠撰寫查詢以傳回精確的記錄所需的特定頁面的資料集。 幸運的是，Microsoft SQL Server 2005 提供新的關鍵字，排序結果，可讓我們來撰寫查詢，可以有效率地擷取記錄的適當子集合。 在本教學課程中，我們會看到如何使用這個新的 SQL Server 2005 關鍵字 GridView 控制項中實作自訂分頁。 由於自訂分頁的使用者介面是相同的預設分頁時，逐步執行各個頁面間到下一步 時使用自訂分頁可能的差距速度比預設分頁。

> [!NOTE]
> 自訂分頁所展現的精確的效能改善取決於正在透過呼叫記錄並放在資料庫伺服器上的負載的總數。 在本教學課程結尾處我們會探討展示透過自訂分頁所獲得的效能優點某些概略度量資訊。


## <a name="step-1-understanding-the-custom-paging-process"></a>步驟 1： 了解自訂分頁程序

若資料進行分頁，頁面中顯示的精確記錄取決於所要求的資料頁，以及每頁顯示的記錄數目。 例如，想像一下，我們想要透過 81 產品頁面上顯示每個分頁 10 項產品。 檢視時的第一頁，d 我們想要產品 1 到 10。檢視第二個頁面時，我們 d 興趣產品 11 到 20，依此類推。

有三個變數，指定要擷取需要的記錄和應該用來呈現分頁介面的方式：

- **啟動 資料列索引**要顯示的資料頁面中的第一列的索引; 這個索引的頁面索引乘以每頁顯示的記錄，並新增一個計算。 例如，若 10 一次第一個頁面記錄 （其頁面索引為 0） 進行分頁，開始的資料列索引為 0 \* 10 + 1 或 1; 第二個頁面 （頁面索引為 1），開始的資料列索引為 1 \* 10 + 1或為 11。
- **最大資料列**每頁顯示的記錄數目上限。 此變數被指最大資料列因為最後一個頁面有可能是較少的記錄傳回超過頁面大小。 比方說，當透過 81 產品 10 記錄，每個分頁的分頁，第九個和最後一個頁面必須只有一項記錄。 不過，任何頁面上，會不顯示超出最大資料列的值更多的記錄。
- **總記錄計數**透過正在呼叫的記錄總數。 雖然這個變數的目前 t 需要決定哪些記錄來擷取指定的頁面，它沒有指示分頁介面。 比方說，如果正在呼叫透過 81 產品，分頁介面知道在分頁 UI 中顯示九個頁碼。

使用預設分頁開始的資料列索引計算方式的頁面索引和頁面大小加上一個項目，產品而最大資料列，則只是頁面大小。 因為預設分頁，擷取所有記錄的資料庫呈現資料，每個資料列索引的任何頁面時已知，進而讓 移至啟動的資料列索引資料列是簡單的工作。 此外，記錄總數是立即可用，因為它 s DataTable （或保留資料庫結果使用的任何物件） 中的記錄數目。

指定開始的資料列索引和最大資料列的變數，自訂分頁實作必須只傳回記錄之後，開始啟動資料列索引和記錄的最大資料列數目最精確的子集。 自訂分頁提供兩個挑戰：

- 我們必須要能夠有效率地將資料列索引與正在透過分頁，好讓我們能夠開始傳回指定的開始資料列索引處的記錄之整體資料中的每個資料列產生關聯
- 我們需要提供透過正在呼叫的記錄總數

在下面兩個步驟中，我們將檢驗這些兩個挑戰回應所需的 SQL 指令碼。 除了 SQL 指令碼中，我們也要 DAL 和 BLL 中實作方法。

## <a name="step-2-returning-the-total-number-of-records-being-paged-through"></a>步驟 2： 傳回透過正在呼叫的記錄總數

我們檢驗如何擷取精確的頁面不會再顯示記錄子集時，可讓 s 先看看如何傳回透過正在呼叫的記錄總數。 需要這項資訊才能正確地設定 [分頁] 使用者介面。 可以使用來取得特定的 SQL 查詢所傳回的記錄總數[`COUNT`彙總函式](https://msdn.microsoft.com/library/ms175997.aspx)。 例如，若要判斷中記錄的總數`Products`資料表中，我們可以使用下列查詢：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample1.sql)]

將方法加入傳回這項資訊我們 DAL 的 s。 特別是，我們將建立呼叫 DAL 方法`TotalNumberOfProducts()`執行`SELECT`如上所示的陳述式。

先開啟`Northwind.xsd`中的型別資料集檔案`App_Code/DAL`資料夾。 接下來，以滑鼠右鍵按一下`ProductsTableAdapter`設計工具中，然後選擇 加入查詢。 當我們先前的教學課程中所見過這可讓我們加入 DAL 的新方法，叫用時，會執行特定 SQL 陳述式或預存程序。 如同先前的教學課程中我們 TableAdapter 方法，這其中一個選擇使用特定 SQL 陳述式。


![使用特定 SQL 陳述式](efficiently-paging-through-large-amounts-of-data-vb/_static/image1.png)

**圖 1**： 使用特定 SQL 陳述式


在下一個畫面中，我們可以指定何種查詢來建立。 因為此查詢會傳回單一純量值中的記錄總數`Products`資料表選擇`SELECT`傳回單一值選項。


![設定使用的 SELECT 陳述式會傳回單一值的查詢](efficiently-paging-through-large-amounts-of-data-vb/_static/image2.png)

**圖 2**： 設定使用的 SELECT 陳述式會傳回單一值的查詢


指出要使用查詢的類型之後, 我們接下來必須指定查詢。


![使用選取的 COUNT(*) 從產品的查詢](efficiently-paging-through-large-amounts-of-data-vb/_static/image3.png)

**圖 3**： 使用 SELECT COUNT (\*) FROM 產品查詢


最後，指定方法的名稱。 使用上述，讓 s `TotalNumberOfProducts`。


![命名 DAL 方法 TotalNumberOfProducts](efficiently-paging-through-large-amounts-of-data-vb/_static/image4.png)

**圖 4**： 命名 DAL 方法 TotalNumberOfProducts


之後按一下 [完成]，精靈會將`TotalNumberOfProducts`dal 的方法。 DAL 中的純量傳回方法會傳回可為 null 的類型，從 SQL 查詢的結果是`NULL`。 我們`COUNT`查詢，不過，一律會傳回非`NULL`值; 不過，DAL 方法會傳回可為 null 的整數。

除了 DAL 方法中，我們也會需要 BLL 中的方法。 開啟`ProductsBLL`類別檔案，然後將`TotalNumberOfProducts`方法只會呼叫向下到 DAL 的`TotalNumberOfProducts`方法：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample2.vb)]

DAL s`TotalNumberOfProducts`方法會傳回可為 null 的整數; 不過，我們建立 ve`ProductsBLL`類別的`TotalNumberOfProducts`方法，使它傳回標準的整數。 因此，我們必須將`ProductsBLL`類別 s`TotalNumberOfProducts`方法傳回的可為 null 整數 DAL s 所傳回的值部分`TotalNumberOfProducts`方法。 若要呼叫`GetValueOrDefault()`傳回的值可為 null 的整數，如果它存在時，是否可為 null 整數`null`，不過，它會傳回預設的整數值為 0。

## <a name="step-3-returning-the-precise-subset-of-records"></a>步驟 3： 傳回記錄的精確子集

下一步是建立 DAL 和 BLL 接受開始的資料列索引中的方法和最大資料列變數先前討論過，並傳回適當的記錄。 這樣做之前，可讓 s 先來看的所需的 SQL 指令碼。 我們面對的挑戰是，我們必須能夠有效率地將索引指派給正在透過分頁，因此，我們可以傳回啟動開始的資料列索引 （和最多為最大記錄數目的記錄） 只記錄整個結果中的每個資料列。

如果已經有資料行做為資料列索引的資料庫資料表中，這是不一項挑戰。 第一眼看起來我們可能會認為`Products`資料表 s`ProductID`欄位即已足夠，因為第一項產品有`ProductID`為 1，2，第二個，依此類推。 不過，刪除產品會保留在順序中，這種方式取消現有的間距。

有兩個一般使用的技術來有效地將資料列索引關聯的資料進行分頁，藉此讓要擷取的記錄的精確子集：

- **使用 SQL Server 2005 s`ROW_NUMBER()`關鍵字**新增至 SQL Server 2005、`ROW_NUMBER()`關鍵字關聯每個傳回的記錄，取決於一些排序次序。 這個等級可以作為每個資料列的資料列索引。
- **使用資料表變數和`SET ROWCOUNT`**  SQL Server s [ `SET ROWCOUNT`陳述式](https://msdn.microsoft.com/library/ms188774.aspx)可以用來指定查詢應該處理終止; 之前的總記錄數[資料表變數](http://www.sqlteam.com/item.asp?ItemID=9454)是本機 T-SQL 變數可以儲存表格式資料、 akin[暫存資料表](http://www.sqlteam.com/item.asp?ItemID=2029)。 這種方法同樣適用於 Microsoft SQL Server 2005 和 SQL Server 2000 (而`ROW_NUMBER()`方法只適用於 SQL Server 2005)。  
  
  這裡的做法是建立資料表變數，其中包含`IDENTITY`資料行和資料行的主索引鍵的資料表正在透過呼叫其資料。 正在透過呼叫其資料的資料表的內容放入資料表變數中，藉此建立關聯的循序資料列索引接下來，傾印 (透過`IDENTITY`資料行) 資料表中的每一筆記錄。 尚未擴展資料表變數，一旦`SELECT`陳述式在資料表變數中，可以執行與基礎資料表聯結，提取出特定的記錄。 `SET ROWCOUNT`陳述式用來以聰明的方式限制需要可傾印以放入資料表變數中的記錄數目。  
  
  這種方法的效率根據所要求的頁面數目為`SET ROWCOUNT`開始的資料列索引，再加上最大資料列的值指派給值。 當透過低編號的頁面，例如第一個分頁的資料幾個頁面則這個方法是非常有效率。 不過，它會表現預設分頁類似效能擷取頁面即將結束時。

本教學課程中實作自訂分頁使用`ROW_NUMBER()`關鍵字。 如需有關使用資料表變數和`SET ROWCOUNT`技術，請參閱[詳細有效率的方法之分頁透過大型結果集](http://www.4guysfromrolla.com/webtech/042606-1.shtml)。

`ROW_NUMBER()`關鍵字聯透過使用下列語法的特定順序傳回每一筆記錄等級：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample3.sql)]

`ROW_NUMBER()` 傳回數值，指定每一筆記錄與指定的排序次序。 例如，若要查看每項產品，從最排序次序成本最低，我們可以使用下列查詢：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample4.sql)]

圖 5 顯示這項查詢 s 透過 Visual Studio 中的 [查詢] 視窗中執行時的結果。 請注意，排序產品的價格，以及每個資料列的價格陣序規範。


![價格等級包括的每個傳回的記錄](efficiently-paging-through-large-amounts-of-data-vb/_static/image5.png)

**圖 5**: 價格陣序規範包括的每個傳回的記錄


> [!NOTE]
> `ROW_NUMBER()` 其中許多新的排名函數是用於 SQL Server 2005。 如需的更完整討論`ROW_NUMBER()`，以及其他排名函數讀取[傳回等級結果與 Microsoft SQL Server 2005](http://www.4guysfromrolla.com/webtech/010406-1.shtml)。


當藉由指定排名結果`ORDER BY`中的資料行`OVER`子句 (`UnitPrice`，在上述範例中)，SQL Server 必須排序結果。 這是快速的作業，如果沒有叢集的索引上的結果，所排序的資料行，或者沒有涵蓋索引，但是可以否則成本更高。 若要協助提升夠大的查詢的效能，請考慮加入非叢集索引的結果依排序的資料行。 請參閱[排名函式和 SQL Server 2005 中的效能](http://www.sql-server-performance.com/ak_ranking_functions.asp)如查看更詳細的效能考量。

所傳回的等級資訊`ROW_NUMBER()`無法直接用於`WHERE`子句。 然而，衍生的資料表可以用來傳回`ROW_NUMBER()`結果，然後可以出現在`WHERE`子句。 例如，下列查詢會使用衍生的資料表傳回產品名稱和單價的資料行，連同`ROW_NUMBER()`結果，然後再使用`WHERE`子句只傳回這些產品的價格陣序規範為 11 到 20 之間：


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample5.sql)]

擴充此概念一點，我們可以利用這種方法來擷取指定想要啟動的資料列索引和最大資料列的值資料的特定頁面：


[!code-html[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample6.html)]

> [!NOTE]
> 我們將會在本教學課程稍後看到*`StartRowIndex`* 所提供的 ObjectDataSource 編製索引零處開始而`ROW_NUMBER()`SQL Server 2005 所傳回的值從 1 開始索引。 因此，`WHERE`子句傳回的記錄位置`PriceRank`必定大於*`StartRowIndex`* 且小於或等於*`StartRowIndex`*  + *`MaximumRows`*.


現在我們已討論過如何`ROW_NUMBER()`可以是用來擷取特定的頁面上的啟動的資料列索引和資料列的上限值的資料，我們現在必須實作此邏輯為 DAL 和 BLL 中的方法。

建立這個我們必須決定排序的查詢時，結果排列次序;可讓其名稱的字母順序來排序產品的 s。 這表示，使用自訂分頁實作本教學課程中我們將無法建立自訂分頁的報表，也可以儲存比。 在下一個教學課程中，不過，我們會看到如何可以提供這類功能。

上一節中，我們會建立 DAL 方法當成特定 SQL 陳述式。 不幸的是，使用 TableAdapter 精靈規定 t，像是 Visual Studio 中的 T-SQL 剖析`OVER`所使用的語法`ROW_NUMBER()`函式。 因此，我們必須建立這個 DAL 方法做為預存程序。 選取 [伺服器總管] 從 [檢視] 功能表 （或叫用的 Ctrl + Alt + S），然後展開`NORTHWND.MDF`節點。 若要加入新的預存程序，以滑鼠右鍵按一下 預存程序 節點，並選擇 加入新的預存程序 （請參閱圖 6）。


![分頁的產品加入新的預存程序](efficiently-paging-through-large-amounts-of-data-vb/_static/image6.png)

**圖 6**： 分頁的產品加入新的預存程序


這個預存程序應該接受兩個整數輸入的參數-`@startRowIndex`和`@maximumRows`並用`ROW_NUMBER()`函式依`ProductName` 欄位中，僅傳回那些資料列大於指定`@startRowIndex`和小於或等於`@startRowIndex`  +  `@maximumRow` s。 輸入下列指令碼至新的預存程序，然後按一下 [儲存] 圖示，將預存程序新增至資料庫。


[!code-sql[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample7.sql)]

建立預存程序之後, 花點時間進行測試。以滑鼠右鍵按一下`GetProductsPaged`預存程序 [伺服器總管] 中的名稱，然後選擇 [執行] 選項。 Visual Studio 會提示您輸入的參數，`@startRowIndex`和`@maximumRow`s （請參閱圖 7）。 請嘗試不同的值，並檢查結果。


![輸入一個值@startRowIndex和@maximumRows參數](efficiently-paging-through-large-amounts-of-data-vb/_static/image7.png)

<strong>圖 7</strong>： 輸入的值@startRowIndex和@maximumRows參數


在之後選擇這些輸入參數值時，[輸出] 視窗會顯示結果。 圖 8 顯示兩個 10 中傳遞時的結果`@startRowIndex`和`@maximumRows`參數。


[![會傳回記錄，就會出現在第二個頁面的資料](efficiently-paging-through-large-amounts-of-data-vb/_static/image9.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image8.png)

**圖 8**: 記錄，就會出現在第二個頁面的資料會傳回 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image10.png))


與這個預存程序建立，我們來建立準備好`ProductsTableAdapter`方法。 開啟`Northwind.xsd`具類型的資料集，以滑鼠右鍵按一下在`ProductsTableAdapter`，並選擇 加入查詢選項。 而不是建立使用特定 SQL 陳述式的查詢，建立使用現有的預存程序。


![建立使用現有的預存程序的 DAL 方法](efficiently-paging-through-large-amounts-of-data-vb/_static/image11.png)

**圖 9**： 建立使用現有的預存程序的 DAL 方法


接下來，我們會提示您選取要叫用的預存程序。 挑選`GetProductsPaged`預存程序，從下拉式清單。


![選擇 GetProductsPaged 預存程序，從下拉式清單](efficiently-paging-through-large-amounts-of-data-vb/_static/image12.png)

**圖 10**： 選擇 GetProductsPaged 預存程序，從下拉式清單


下一個畫面中再會要求您的資料類型由預存程序： 表格式資料、 單一值或沒有值。 因為`GetProductsPaged`預存程序可以傳回多筆記錄，表示它會傳回表格式資料。


![指出預存程序傳回表格式資料](efficiently-paging-through-large-amounts-of-data-vb/_static/image13.png)

**圖 11**： 指出預存程序傳回表格式資料


最後，表示您想要建立方法的名稱。 如同我們先前的教學課程，請繼續建立方法使用的填滿 DataTable 和傳回 DataTable。 將第一個方法`FillPaged`，第二個`GetProductsPaged`。


![名稱方法 FillPaged 和 GetProductsPaged](efficiently-paging-through-large-amounts-of-data-vb/_static/image14.png)

**圖 12**： 名稱方法 FillPaged 和 GetProductsPaged


除了建立 DAL 方法傳回的產品的特定頁面，我們也要提供給 BLL 中的這類功能。 DAL 與方法一樣，BLL 的 GetProductsPaged 方法必須接受兩個整數輸入用來指定開始的資料列索引和最大資料列，而且必須傳回只在指定範圍內的記錄。 這種 BLL 方法在類別中建立 ProductsBLL 只呼叫向下的 DAL 的 GetProductsPaged 方法，就像這樣：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample8.vb)]

您可以使用任何名稱 BLL 方法 s 輸入參數，但是，我們將會看到在短時間內，選擇使用`startRowIndex`和`maximumRows`我們將額外的設定才能使用此方法 ObjectDataSource 時的工作。

## <a name="step-4-configuring-the-objectdatasource-to-use-custom-paging"></a>步驟 4： 設定要使用自訂分頁 ObjectDataSource

BLL 和 DAL 方法存取完整的記錄的特定子集，我們已備妥可建立 GridView re 控制該頁面，透過其基礎使用自訂分頁的記錄。 先開啟`EfficientPaging.aspx`頁面`PagingAndSorting`資料夾中，將 GridView 加入至頁面上，並將它設定為使用新的 ObjectDataSource 控制項。 在我們過去教學課程中，我們通常必須設定為使用 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 此時，不過，我們想要使用`GetProductsPaged`方法相反地，因為`GetProducts`方法會傳回*所有*資料庫中的產品而`GetProductsPaged`傳回只記錄的特定子集。


![設定為使用 ProductsBLL 類別的 GetProductsPaged 方法 ObjectDataSource](efficiently-paging-through-large-amounts-of-data-vb/_static/image15.png)

**圖 13**： 設定為使用 ProductsBLL 類別的 GetProductsPaged 方法 ObjectDataSource


因為我們重新建立唯讀的 GridView，花一點時間來設定 [方法] 下拉式清單中插入、 更新和刪除索引標籤，以 （無）。

接下來，ObjectDataSource 精靈會提示輸入我們的來源`GetProductsPaged`方法 s`startRowIndex`和`maximumRows`輸入參數值。 這些輸入的參數實際上為 GridView 自動只要保留來源設定為 None，按一下 [完成]。


![保留為 無的輸入的參數來源](efficiently-paging-through-large-amounts-of-data-vb/_static/image16.png)

**圖 14**： 保留為 無的輸入的參數來源


完成 ObjectDataSource 精靈之後，GridView 會包含 BoundField 或 CheckBoxField 每個產品資料欄位。 請隨意適當地調整 GridView 的外觀。 我已選擇只顯示`ProductName`， `CategoryName`， `SupplierName`， `QuantityPerUnit`，和`UnitPrice`BoundFields。 此外，設定以支援分頁，藉由檢查其智慧標籤中的啟用分頁 核取方塊 GridView。 這些變更之後，請 GridView 和 ObjectDataSource 宣告式標記看起來應該如下所示：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample9.aspx)]

如果您瀏覽透過瀏覽器頁面，不過，GridView 是位置可以找到。


![在 GridView 是不顯示](efficiently-paging-through-large-amounts-of-data-vb/_static/image17.png)

**圖 15**: GridView 是不顯示


在 GridView 因遺漏 ObjectDataSource 目前使用 0 做為值的兩個`GetProductsPaged``startRowIndex`和`maximumRows`輸入參數。 因此，產生的 SQL 查詢會傳回任何記錄，因此不會顯示在 GridView。

若要補救這種情況，我們需要設定要使用自訂分頁 ObjectDataSource。 這可以透過完成下列步驟：

1. **設定 ObjectDataSource s`EnablePaging`屬性`true`** 這表示它必須將它傳遞給 ObjectDataSource`SelectMethod`另外兩個參數： 一個用來指定開始的資料列索引 ([ `StartRowIndexParameterName` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.startrowindexparametername.aspx))，和一個用來指定最大資料列 ([`MaximumRowsParameterName`](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.maximumrowsparametername.aspx))。
2. **設定 ObjectDataSource s`StartRowIndexParameterName`和`MaximumRowsParameterName`據以屬性**`StartRowIndexParameterName`和`MaximumRowsParameterName`屬性會指出傳入輸入參數的名稱`SelectMethod`進行自訂分頁。 根據預設，這些參數名稱為`startIndexRow`和`maximumRows`，這是原因、 建立時`GetProductsPaged`方法在 BLL 我用於這些值的輸入參數。 如果您選擇要使用不同的參數名稱 BLL s`GetProductsPaged`方法，例如`startIndex`和`maxRows`的範例，您就必須設定 ObjectDataSource s`StartRowIndexParameterName`和`MaximumRowsParameterName`屬性據以 （例如 startIndex 為`StartRowIndexParameterName`和為 maxRows `MaximumRowsParameterName`)。
3. **設定 ObjectDataSource s [ `SelectCountMethod`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.selectcountmethod(VS.80).aspx)的總數目的記錄正在分頁透過傳回的方法名稱 (`TotalNumberOfProducts`)** 請記得，`ProductsBLL`類別的`TotalNumberOfProducts`方法會傳回透過使用 DAL 方法執行正在呼叫的記錄總數`SELECT COUNT(*) FROM Products`查詢。 Objectdatasource 需要這項資訊才能正確地呈現分頁介面。
4. **移除`startRowIndex`和`maximumRows``<asp:Parameter>`從 ObjectDataSource s 宣告式標記項目**設定時透過精靈 ObjectDataSource，Visual Studio 會自動加入兩個`<asp:Parameter>`項目如`GetProductsPaged`方法 s 輸入參數。 藉由設定`EnablePaging`至`true`，這些參數會自動傳遞給; ObjectDataSource 如果他們也會出現在宣告式語法，將會嘗試傳遞*四個*參數`GetProductsPaged`方法與兩個參數來`TotalNumberOfProducts`方法。 如果您忘記要移除這些`<asp:Parameter>`項目，瀏覽頁面透過瀏覽器，您會取得如下的錯誤訊息： *ObjectDataSource 'ObjectDataSource1' 找不到非泛型方法 'TotalNumberOfProducts' 具有參數： startRowIndex、 maximumRows*。

進行這些變更之後，ObjectDataSource s 宣告式語法看起來應該如下所示：


[!code-aspx[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample10.aspx)]

請注意，`EnablePaging`和`SelectCountMethod`內容已設定和`<asp:Parameter>`元素已經移除。 進行這些變更之後，圖 16 會顯示 [屬性] 視窗的螢幕擷取畫面。


![若要使用自訂分頁，設定 ObjectDataSource 控制項](efficiently-paging-through-large-amounts-of-data-vb/_static/image18.png)

**圖 16**： 若要使用自訂分頁，設定 ObjectDataSource 控制項


進行這些變更之後，請瀏覽此頁面，透過瀏覽器。 您應該會看到 10 項產品列中，依字母順序排序。 請花一點時間逐步執行一個頁面的資料，一次。 自訂分頁更有效率地透過大量的資料頁面從使用者 s 觀點來看預設分頁和自訂分頁之間沒有視覺差異時，，因為它只會擷取需要為指定的頁面顯示這些記錄使用。


[![資料、 Ordered 依產品名稱，是分頁使用自訂分頁](efficiently-paging-through-large-amounts-of-data-vb/_static/image20.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image19.png)

**圖 17**: 資料、 Ordered 依產品名稱，是分頁使用自訂分頁 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image21.png))


> [!NOTE]
> 使用自訂分頁時，頁面計數值傳回 ObjectDataSource 的`SelectCountMethod`會儲存在 GridView 的檢視狀態。 其他的 GridView 變數`PageIndex`， `EditIndex`， `SelectedIndex`，`DataKeys`集合，以及其他會儲存在*控制狀態*，其中會保存 GridView s 的值為何`EnableViewState`屬性。 因為`PageCount`值保存在回傳時使用的分頁介面，包括帶您前往最後一頁的連結使用檢視狀態，請務必啟用 GridView 的檢視狀態。 （如果分頁介面沒有直接連結到最後一個頁面上，則您可能會停用檢視狀態）。


按一下最後一個頁面連結導致回傳，並指示來更新 GridView 其`PageIndex`屬性。 按一下最後一個頁面連結時，會將指派 GridView 其`PageIndex`屬性設為其中一個值小於其`PageCount`屬性。 停用，檢視狀態`PageCount`值將會遺失在回傳而`PageIndex`改為指派的最大整數值。 GridView 接下來，會嘗試判斷資料列的起始索引乘以`PageSize`和`PageCount`屬性。 這會導致`OverflowException`因為產品超過允許的最大整數大小。

## <a name="implement-custom-paging-and-sorting"></a>實作自訂分頁和排序

我們目前的自訂分頁實作需要的資料透過分頁的順序指定以靜態方式建立時`GetProductsPaged`預存程序。 不過，您可能已記下 GridView s 智慧標籤包含除了啟用分頁 選項之外，啟用排序核取方塊。 不幸的是，加入我們目前的自訂分頁實作 GridView 中排序支援時，只會排序資料的目前檢視頁面上的記錄。 例如，如果您設定也支援分頁，並檢視資料，第一頁時，然後依產品名稱，依遞減順序排序 GridView 它會反轉產品的順序第 1 頁上。 如圖 18 所示，例如顯示初探為第一次的產品時以反向字母順序，會忽略 71 其他產品隨附之後初探，依字母順序，排序只有當記錄的第一頁會視為排序。


[![只顯示資料目前頁面上進行排序](efficiently-paging-through-large-amounts-of-data-vb/_static/image23.png)](efficiently-paging-through-large-amounts-of-data-vb/_static/image22.png)

**圖 18**： 只顯示資料目前頁面上進行排序 ([按一下以檢視完整大小的影像](efficiently-paging-through-large-amounts-of-data-vb/_static/image24.png))


因為排序從 BLL s 已擷取資料之後發生的排序只適用於目前頁面的資料項目`GetProductsPaged`方法，而且這個方法只會傳回特定網頁的記錄。 若要實作正確排序，我們需要將排序運算式，以`GetProductsPaged`方法，以便可以適當地排名資料，再傳回特定的資料頁面。 我們會了解如何完成這項作業在教學課程中我們下一步。

## <a name="implementing-custom-paging-and-deleting"></a>實作自訂分頁並刪除

如果您啟用刪除功能的 GridView 的資料分頁使用自訂分頁技術，您會發現從最後一頁刪除最後一筆記錄時，GridView 消失而不是適當遞減 GridView 的`PageIndex`. 若要重新產生這個 bug，啟用刪除只剛才所建立的教學課程。 移至最後一頁 （第 9 頁），其中您應該會看到單一產品因為我們分頁 81 產品，一次 10 項產品。 刪除此產品。

一旦刪除最後一個產品 GridView*應該*自動移至第八個頁面，以及這類功能使用預設分頁顯示。 使用自訂分頁時，不過，在刪除最後一個產品的最後一頁之後, GridView 只從畫面消失完全。 精確原因*為什麼*發生這種情況都是位元超出本教學課程的範圍，請參閱 <<c4> [ 刪除自訂分頁的 GridView 的最後一筆記錄的最後一頁](http://scottonwriting.net/sowblog/posts/7326.aspx)和來源的低層級的詳細資料這個問題。 在摘要它 s，因為下列一連串步驟，按一下 [刪除] 按鈕時執行的 GridView:

1. 刪除記錄
2. 取得適當的記錄，以顯示所指定`PageIndex`和 `PageSize`
3. 檢查以確定`PageIndex`不超過資料來源; 中的資料頁面的數目，如果它存在，會自動遞減 GridView 的`PageIndex`屬性
4. 將適當的頁面資料的繫結至 GridView 使用在步驟 2 中取得的記錄

問題的起因是該中的步驟 2`PageIndex`時擷取要顯示的記錄仍在使用`PageIndex`已只會刪除其唯一記錄的最後一頁。 因此，在步驟 2*沒有*會傳回記錄，因為該最後一頁的資料不再包含任何記錄。 然後，在步驟 3 中 GridView 實現，其`PageIndex`屬性大於資料來源中的頁面總數 （自我們 ve 已刪除最後一個記錄中的最後一頁），並因此遞減其`PageIndex`屬性。 步驟 4 中 GridView 會嘗試將本身繫結至在步驟 2; 擷取的資料不過，在步驟 2 中未傳回任何記錄，因此導致空的 GridView。 使用預設分頁時，此問題不 t 介面因為在步驟 2 中*所有*從資料來源擷取記錄。

若要修正此問題，我們有兩個選項。 第一個是建立事件處理常式 GridView s`RowDeleted`決定只刪除網頁中顯示的多少筆記錄的事件處理常式。 如果發生只有一筆記錄，然後將它刪除的記錄必須已被的最後一個，我們要遞減的 GridView s `PageIndex`。 當然，我們只想要更新`PageIndex`如果刪除作業實際上成功，這可藉由確保`e.Exception`屬性是`null`。

這種方式運作，因為它會更新`PageIndex`在步驟 1 之後但在之前步驟 2。 因此，在步驟 2 中，會傳回一組適當的記錄。 若要達成此目的，使用下列程式碼：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample11.vb)]

一種解決方法是建立事件處理常式 ObjectDataSource s`RowDeleted`事件，並設定`AffectedRows`屬性設為 1 的值。 在 GridView 更新之前刪除的記錄，在步驟 1 中 （但然後再重新擷取步驟 2 中的資料），其`PageIndex`如果一或多個資料列作業所影響的屬性。 不過， `AffectedRows` objectdatasource 未設定屬性，因此會省略此步驟。 具有執行此步驟的其中一種方式是手動設定`AffectedRows`如果順利完成刪除作業的屬性。 這可以使用下列程式碼來完成：


[!code-vb[Main](efficiently-paging-through-large-amounts-of-data-vb/samples/sample12.vb)]

這些事件處理常式的兩個程式碼可以在程式碼後置類別的`EfficientPaging.aspx`範例。

## <a name="comparing-the-performance-of-default-and-custom-paging"></a>比較效能的預設和自訂分頁

因為自訂分頁只會擷取所需的記錄，則傳回預設分頁*所有*的檢視，每個頁面記錄它 s 清除自訂分頁會比預設分頁更有效率。 但是就是如何更有效率的作法是自訂分頁嗎？ 從預設分頁移到自訂分頁，可以看到哪一種效能提升？

不幸的是，有 s 不適合大小所有這裡回答。 提升的效能取決於許多因素，最明顯正在透過正在呼叫的記錄和負載數目的兩個放在 web 伺服器和資料庫伺服器之間的資料庫伺服器和通訊通道。 用於具有少數的數十個記錄的小型資料表，效能差異會造成影響。 對於大型資料表，具有數千部數十萬個資料列，來使用的效能差異是，嚴重。

我，發行項[ASP.NET 2.0 與 SQL Server 2005 中的自訂分頁](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)，包含執行至展現中時包含的資料庫資料表進行分頁這兩種分頁技術之間的效能差異某些效能測試50,000 筆記錄。 我可以在這些測試中檢查這兩個執行 SQL Server 層級查詢的時間 (使用[SQL Profiler](https://msdn.microsoft.com/library/ms173757.aspx)) 和 ASP.NET 頁面使用[ASP.NET 的追蹤功能](https://msdn.microsoft.com/library/y13fw6we.aspx)。 請注意，這些測試是在單一的作用中使用者，我開發方塊上執行，因此科學而不會模擬一般網站的負載模式。 不論如何，結果會說明執行時間的預設和自訂分頁中的相對差異使用夠大的資料量時。


|  | **Avg.持續時間 （秒）** | **讀取** |
| --- | --- | --- |
| **預設分頁 SQL 程式碼剖析工具** | 1.411 | 383 |
| **自訂分頁 SQL 程式碼剖析工具** | 0.002 | 29 |
| **預設分頁 ASP.NET 追蹤** | 2.379 | *N/A* |
| **自訂分頁 ASP.NET 追蹤** | 0.029 | *N/A* |


如您所見，擷取資料的特定頁面需要較少讀取 354 平均，並在大部分的時間內完成。 ASP.NET 頁面中，在自訂頁面無法轉譯的 1/100 接近<sup>th</sup>所花費時間時使用的預設分頁。 請參閱[我文章](http://aspnet.4guysfromrolla.com/articles/031506-1.aspx)如需詳細資訊，對這些結果，以及程式碼和資料庫，您可以下載重現您自己的環境中的這些測試。

## <a name="summary"></a>總結

預設分頁是很簡單，在資料 Web 控制項 s 智慧標籤中實作只核取 啟用分頁的核取方塊，但是這類簡化代價效能。 使用預設分頁時，當使用者要求資料的任何頁面*所有*傳回記錄，即使只有極少數的它們可能會顯示。 為了打擊這樣的效能負荷，ObjectDataSource 提供替代分頁選項自訂分頁。

雖然預設分頁 s 效能問題，只需要顯示，那些記錄中擷取自訂分頁做了改善它 s 更為複雜，若要實作自訂分頁。 首先，您必須，用來正確 （且有效地） 存取要求的記錄的特定子集是撰寫查詢。 這可以透過完成許多種;我們檢查本教學課程中的一個是使用新的 SQL Server 2005 的`ROW_NUMBER()`為 rank 函數結果，並再返回只會產生其次序落在指定範圍內。 此外，我們需要加入方法來判斷正在透過呼叫的記錄總數。 在建立之後這些 DAL 和 BLL 方法，我們也需要設定 ObjectDataSource，讓它可以決定總記錄數需分頁處理透過以及可以正確啟動的資料列索引和最大資料列將值傳遞至 BLL。

在實作自訂分頁並需要幾個步驟並不幾乎只要預設分頁時，自訂分頁時，必須夠大的資料量進行分頁。 檢查結果顯示、 自訂分頁可以剖析從 ASP.NET 頁面呈現時間的秒數，而且可以由一或多個的數量級淡化資料庫伺服器上的負載。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](paging-and-sorting-report-data-vb.md)
> [下一頁](sorting-custom-paged-data-vb.md)
