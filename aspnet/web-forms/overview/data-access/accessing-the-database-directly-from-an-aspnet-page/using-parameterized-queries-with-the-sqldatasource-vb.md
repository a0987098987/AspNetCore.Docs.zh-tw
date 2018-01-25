---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
title: "使用 SqlDataSource (VB) 中使用參數化的查詢 |Microsoft 文件"
author: rick-anderson
description: "在本教學課程中，我們會繼續我們查看 SqlDataSource 控制項，並了解如何定義參數化的查詢。 參數可以指定兩個 decla..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: e322f34c-83b7-41ea-ab65-ab1e0bdcc609
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-vb
msc.type: authoredcontent
ms.openlocfilehash: b1cda18620a970c45b05039dd380c393e3854889
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-parameterized-queries-with-the-sqldatasource-vb"></a>使用 SqlDataSource (VB) 中使用參數化的查詢
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_VB.exe)或[下載 PDF](using-parameterized-queries-with-the-sqldatasource-vb/_static/datatutorial48vb1.pdf)

> 在本教學課程中，我們會繼續我們查看 SqlDataSource 控制項，並了解如何定義參數化的查詢。 以宣告方式和以程式設計的方式，可以指定的參數，並且可以取自的位置，例如查詢字串、 工作階段狀態，其他控制項和更多的數字。


## <a name="introduction"></a>簡介

在上一個教學課程中我們可了解如何使用 SqlDataSource 控制項直接從資料庫擷取資料。 使用 「 設定資料來源精靈 」，我們可以選擇資料庫，然後將： 挑選要從資料表或檢視則傳回的資料行輸入自訂的 SQL 陳述式。或使用預存程序。 從資料表或檢視表選取資料行，或輸入自訂的 SQL 陳述式，SqlDataSource 控制項是否 s`SelectCommand`屬性會被指派結果的特定 SQL`SELECT`陳述式，而且這是此`SELECT`陳述式時執行SqlDataSource 的`Select()`叫用方法時 (以程式設計方式或自動從 Web 控制項的資料)。

SQL`SELECT`陳述式中先前的教學課程的示範缺乏使用`WHERE`子句。 在`SELECT`陳述式，`WHERE`子句可以用來限制傳回的結果。 例如，若要顯示的成本大於 $50.00 的產品名稱，我們可以使用下列查詢：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample1.sql)]

一般而言，使用中的值`WHERE`子句會決定某些外部來源，例如查詢字串值、 工作階段變數或頁面上的 Web 控制項的使用者輸入。 透過使用的理想上，指定此類輸入*參數*。 與 Microsoft SQL Server，參數會表示使用`@parameterName`，如下所示：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample2.sql)]

SqlDataSource 支援參數化的查詢，兩者`SELECT`陳述式和`INSERT`， `UPDATE`，和`DELETE`陳述式。 此外，參數值可以自動從各種來源的 querystring，工作階段狀態，在頁面上，控制項和等等或可以透過程式設計方式指定。 在本教學課程中，我們會看到如何定義參數化的查詢以及如何指定參數值以宣告方式和以程式設計的方式。

> [!NOTE]
> 在上一個教學課程中，我們會比較 ObjectDataSource 透過使用 SqlDataSource，注意其概念相似之處的第一次 46 教學課程已我們選擇的工具。 這些相似之處也會延伸至參數。 ObjectDataSource 的參數對應到商務邏輯層中的方法的輸入參數。 使用 SqlDataSource，參數會定義直接在 SQL 查詢。 這兩個控制項具有參數的集合，這些其`Select()`， `Insert()`， `Update()`，和`Delete()`方法，兩者均可以從預先定義的來源 （querystring 值、 工作階段變數等填入這些參數值) 或透過程式設計方式指定。


## <a name="creating-a-parameterized-query"></a>建立參數型查詢

SqlDataSource 控制項 s 設定資料來源精靈提供三種途徑，以定義要擷取資料庫中的記錄執行的命令：

- 選取的現有資料表或檢視表中的資料行
- 輸入自訂的 SQL 陳述式，或
- 選擇預存程序

當挑選現有的資料表或檢視表，為參數的資料行`WHERE`子句必須指定透過 「 新增`WHERE`子句對話方塊。 在建立自訂的 SQL 陳述式，不過，您可以輸入的參數，直接將`WHERE`子句 (使用`@parameterName`代表每個參數)。 A[預存程序](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)組成一或多個 SQL 陳述式，而且這些陳述式可以參數化。 SQL 陳述式中使用的參數不過，必須傳遞中做為輸入參數給預存程序。

因為建立參數化的查詢取決於如何 SqlDataSource 的`SelectCommand`已指定，讓 s 看看這三種方法。 若要開始使用，請開啟`ParameterizedQueries.aspx`頁面`SqlDataSource`資料夾中，將 SqlDataSource 控制項從工具箱拖曳至設計工具中，並設定其`ID`至`Products25BucksAndUnderDataSource`。 接下來，在按一下 設定資料來源連結控制項 s 智慧標籤。 選取要使用的資料庫 (`NORTHWINDConnectionString`)，按一下 [下一步]。

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>步驟 1： 加入`WHERE`子句時挑選資料行從資料表或檢視

選取要從 SqlDataSource 控制項使用資料庫所傳回的資料，設定資料來源精靈可讓我們只要挑選要傳回從現有的資料表或檢視 （請參閱圖 1） 的資料行。 自動執行組建 sql`SELECT`陳述式，這是什麼傳送至資料庫時 SqlDataSource 的`Select()`叫用方法。 如同上一個教學課程中，從下拉式清單中選取 [產品] 資料表，並檢查`ProductID`， `ProductName`，和`UnitPrice`資料行。


[![挑選要從資料表或檢視傳回的資料行](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image1.png)

**圖 1**： 挑選要從資料表或檢視傳回的資料行 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.png))


要包含`WHERE`中的子句`SELECT`陳述式中，按一下 `WHERE`按鈕，這會開啟 新增`WHERE`子句對話方塊 （請參閱圖 2）。 若要加入參數來限制所傳回的結果`SELECT`查詢，首先選擇篩選的資料行的資料。 接下來，選擇要用於篩選的運算子 (=、 &lt;， &lt;=、&gt;等等)。 最後，請選擇來源參數 s 的值，例如查詢字串或工作階段狀態。 之後設定參數，按一下 [新增] 按鈕，將它併入`SELECT`查詢。

例如，o d e s 只傳回這些結果其中`UnitPrice`值小於或等於 $25.00。 因此，挑選`UnitPrice`從資料行的下拉式清單和&lt;= 從 [運算子] 下拉式清單。 使用硬式編碼的參數值 （例如 $25.00) 時，或以程式設計方式指定為參數值，選取 無從 來源 下拉式清單。 接下來，25.00 中的 [值] 文字方塊中輸入硬式編碼的參數值，然後按一下 [新增] 按鈕以完成程序。


[![限制傳回的結果加入 WHERE 子句對話方塊](using-parameterized-queries-with-the-sqldatasource-vb/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.png)

**圖 2**： 限制從 新增傳回的結果`WHERE`子句對話方塊 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.png))


加入參數之後, 按一下 確定 以返回至 設定資料來源精靈。 `SELECT`現在應該包含在精靈的下方的陳述式`WHERE`具有名為參數的子句`@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample3.sql)]

> [!NOTE]
> 如果您指定多個條件中的`WHERE`從 [新增子句`WHERE`子句] 對話方塊中，精靈加入它們與`AND`運算子。 如果您需要包含`OR`中`WHERE`子句 (例如`WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`)，您需要建置`SELECT`陳述式利用自訂的 SQL 陳述式螢幕。


完成設定的 SqlDataSource （按 下一步，然後完成），然後檢查 SqlDataSource s 宣告式標記。 現在包含標記`<SelectParameters>`集合中的參數的來源會列出`SelectCommand`。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample4.aspx)]

當 SqlDataSource s`Select()`叫用方法時，`UnitPrice`參數值 (25.00) 套用至`@UnitPrice`中的參數`SelectCommand`再傳送至資料庫。 最後結果就是，只有那些產品從傳回小於或等於 $25.00`Products`資料表。 若要確認這個項目，將 GridView 加入頁面中，繫結到此資料來源，並檢視透過瀏覽器頁面。 您只能看到列出為確認圖 3 是小於或等於 $25.00，這些產品。


[![會顯示只有那些產品的小於或等於 $25.00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.png)

**圖 3**： 顯示只有那些產品的小於或等於 $25.00 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>步驟 2： 將參數加入至自訂的 SQL 陳述式

加入自訂的 SQL 陳述式時您可以輸入`WHERE`子句明確或在查詢產生器的篩選器儲存格中指定值。 若要示範這點，可讓顯示只是那些產品價格會低於特定臨界值的 GridView 中的 s。 藉由新增到文字方塊中啟動`ParameterizedQueries.aspx`頁面，即可從使用者收集此臨界值。 設定文字方塊中 s`ID`屬性`MaxPrice`。 加入按鈕 Web 控制項並設定其`Text`顯示比對產品的屬性。

下一步，拖曳到頁面的 GridView 和從其智慧標籤中，選擇建立新的 SqlDataSource 名為`ProductsFilteredByPriceDataSource`。 在設定資料來源精靈中，繼續進行 [指定自訂 SQL 陳述式或預存程序] 畫面 （請參閱圖 4），並輸入下列查詢：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample5.sql)]

（手動或透過查詢產生器），請輸入查詢之後, 按 下一步。


[![傳回小於或等於參數值，只有那些產品](using-parameterized-queries-with-the-sqldatasource-vb/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.png)

**圖 4**： 傳回只有那些產品的小於或等於參數值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.png))


因為查詢包含參數，請在精靈中的下一個畫面會提示我們參數值的來源。 從參數來源下拉式清單中選擇控制項和`MaxPrice`(TextBox 控制項的`ID`值) 從 [ControlID] 下拉式清單。 您也可以輸入要在使用者未輸入任何文字放到其中的情況下使用選擇性的預設值`MaxPrice`文字方塊。 時間之外，請勿輸入預設值。


[![MaxPrice 文字方塊中文字的屬性用做為參數來源](using-parameterized-queries-with-the-sqldatasource-vb/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.png)

**圖 5**:`MaxPrice`文字方塊 s`Text`屬性做為參數來源 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.png))


完成接下來，按一下 [設定資料來源精靈]，然後完成。 GridView、 文字方塊、 按鈕和 SqlDataSource 的宣告式標記如下：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample6.aspx)]

請注意，此參數在 SqlDataSource s 中的`<SelectParameters>`區段是`ControlParameter`，其中包括額外的屬性，例如`ControlID`和`PropertyName`。 當 SqlDataSource s`Select()`叫用方法時， `ControlParameter` grabs 示範從指定的 Web 控制項屬性的值，並將它指派給對應的參數中`SelectCommand`。 在此範例中， `MaxPrice` Text 屬性用做為`@MaxPrice`參數值。

花一分鐘，以檢視這個頁面，透過瀏覽器。 當第一次瀏覽頁面時，或每當`MaxPrice`文字方塊中缺少任何記錄會顯示在 GridView 的值。


[![沒有記錄會顯示當 MaxPrice 文字方塊是空的](using-parameterized-queries-with-the-sqldatasource-vb/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.png)

**圖 6**： 否記錄會顯示當`MaxPrice`文字方塊是空的 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.png))


會顯示任何產品的原因是因為依預設，參數值為空字串轉換成資料庫`NULL`值。 因為比較的`[UnitPrice] <= NULL`一律會評估為 False，就會傳回任何結果。

文字方塊中輸入值，例如 5.00，，然後按一下 [顯示比對的產品] 按鈕。 在回傳時，SqlDataSource 通知 GridView 的其中一個其參數的來源已變更。 因此，GridView 重新繫結至 SqlDataSource，顯示這些產品小於或等於 $5.00 美元。


[![顯示產品的小於或等於 $5.00](using-parameterized-queries-with-the-sqldatasource-vb/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.png)

**圖 7**： 顯示產品的小於或等於 $5.00 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.png))


## <a name="initially-displaying-all-products"></a>一開始顯示所有產品

而不是第一次載入頁面時顯示任何產品，我們可能會想要顯示*所有*產品。 一種方法來列出所有產品每當`MaxPrice`文字方塊是空的是將參數 s 的預設值設定為某個 insanely 高的值，例如 1000000，因為它不太可能，Northwind Traders 會曾有清查單價 s 超過 $1,000,000。 不過，這種方法是 shortsighted，而且在其他情況下可能無法運作。

在先前的教學課程-[宣告式的參數](../basic-reporting/declarative-parameters-vb.md)和[主要/詳細資料篩選與 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-vb.md)我們所面臨類似的問題。 我們的解決方案是將此邏輯放在商務邏輯層。 具體來說，BLL 檢查傳入的值，而如果它是`NULL`或部分保留值，請呼叫已傳送到傳回的所有記錄的 DAL 方法。 如果輸入的值是正常的篩選值，已進行呼叫以執行使用參數化 SQL 陳述式的 DAL 方法`WHERE`子句，但所提供的值。

不幸的是，我們會略過架構使用 SqlDataSource 時。 相反地，我們需要自訂 SQL 陳述式，以聰明的方式擷取所有記錄，如果`@MaximumPrice`參數是`NULL`或某些保留的值。 針對此練習，可讓 s 有它因此，如果`@MaximumPrice`參數等於`-1.0`，然後*所有*的記錄會傳回 (`-1.0`做為保留的值運作，因為沒有產品可以有負`UnitPrice`值)。 若要完成這項作業中，我們可以使用下列 SQL 陳述式：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample7.sql)]

這`WHERE`子句會傳回*所有*記錄如果`@MaximumPrice`參數等於`-1.0`。 如果參數值不是`-1.0`，只有那些產品的`UnitPrice`小於或等於`@MaximumPrice`傳回參數值。 藉由設定的預設值`@MaximumPrice`參數`-1.0`，第一個載入的頁面 (或每當`MaxPrice`文字方塊是空的)，`@MaximumPrice`的值將會`-1.0`，並且會顯示所有產品。


[![顯示當 MaxPrice 文字方塊現在所有產品都不是空的](using-parameterized-queries-with-the-sqldatasource-vb/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.png)

**圖 8**： 所有產品現在都會顯示當`MaxPrice`文字方塊是空的 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image16.png))


有幾個警告，請注意，這個方法。 首先，請了解參數 s 的資料類型由其推斷 s SQL 查詢中的使用方式。 如果您變更`WHERE`子句從`@MaximumPrice = -1.0`至`@MaximumPrice = -1`，執行階段視為整數參數。 如果您隨後嘗試指派`MaxPrice`（例如 5.00) 的十進位值的文字方塊中，會發生錯誤，因為它無法將 5.00 轉換成整數。 若要補救這種情況，請請確定您使用`@MaximumPrice = -1.0`中`WHERE`子句或更棒的是，設定`ControlParameter`物件的`Type`為十進位的屬性。

其次，藉由加入`OR @MaximumPrice = -1.0`至`WHERE`子句，查詢引擎無法使用索引上`UnitPrice`（假設其存在），因而導致資料表掃描。 如果沒有夠大的數字中的記錄，這會影響效能`Products`資料表。 更好的方法，可將此邏輯移到預存程序所在`IF`陳述式會執行`SELECT`查詢從`Products`資料表沒有`WHERE`子句時要傳回需要的所有記錄，或其中一個其`WHERE`子句只包含`UnitPrice`準則，以便可以使用索引。

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>步驟 3： 建立和使用參數化預存程序

預存程序可以包含一組可用的輸入參數的預存程序內所定義的 SQL 陳述式中。 設定時使用預存程序可接受輸入的參數 SqlDataSource，可以與特定 SQL 陳述式使用相同的技術指定這些參數值。

O d e s 為了說明使用 SqlDataSource 中預存程序，在名為 Northwind 資料庫中建立新的預存程序`GetProductsByCategory`，它會接受名為`@CategoryID`並傳回所有產品的資料行其`CategoryID`資料行比對`@CategoryID`。 若要建立預存程序，請移至 [伺服器總管] 中，並向下鑽研至`NORTHWND.MDF`資料庫。 （如果您不要看到 [伺服器總管] 中，使其移至 [檢視] 功能表，然後選取 [伺服器總管] 選項。）

從`NORTHWND.MDF`資料庫，預存程序資料夾上按一下滑鼠右鍵，選擇 加入新預存程序，並輸入下列語法：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample8.sql)]

按一下儲存圖示 （或 Ctrl + S） 來儲存預存程序。 您可以從預存程序資料夾上按一下滑鼠右鍵，然後選擇 執行測試的預存程序。 這會提示您輸入的預存程序的參數 (`@CategoryID`，這個執行個體中) 之後，結果會顯示在 [輸出] 視窗中。


[![GetProductsByCategory 預存程序執行時未使用@CategoryID的 1](using-parameterized-queries-with-the-sqldatasource-vb/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image17.png)

**圖 9**:`GetProductsByCategory`預存程序執行時未使用`@CategoryID`為 1 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image18.png))


可讓使用此預存程序在 GridView 中的飲料類別目錄中顯示所有產品的 s。 將新的 GridView 加入頁面和繫結到名為新 SqlDataSource `BeverageProductsDataSource`。 繼續指定自訂 SQL 陳述式或預存程序 畫面，選取預存程序選項按鈕，然後挑選`GetProductsByCategory`預存程序，從下拉式清單。


[![選取 GetProductsByCategory 預存程序，從下拉式清單](using-parameterized-queries-with-the-sqldatasource-vb/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image19.png)

**圖 10**： 選取`GetProductsByCategory`預存程序，從下拉式清單 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image20.png))


由於預存程序接受一個輸入的參數 (`@CategoryID`)，按一下 下一步提示我們指定這個參數 s 的值的來源。 飲料`CategoryID`為 1，因此保留無參數來源下拉式清單，然後在 [預設值] 文字方塊中輸入 1。


[![使用硬式編碼值為 1，以傳回 「 飲料 」 分類中的產品](using-parameterized-queries-with-the-sqldatasource-vb/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image21.png)

**圖 11**： 使用 Hard-Coded 值為 1，傳回 「 飲料 」 分類中的產品 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image22.png))


下列所示宣告式標記，當使用預存程序，SqlDataSource s`SelectCommand`屬性設定為預存程序名稱和[`SelectCommandType`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx)設`StoredProcedure`，這表示`SelectCommand`是預存程序，而不是特定 SQL 陳述式的名稱。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample9.aspx)]

測試瀏覽器中。 顯示屬於 「 飲料 」 分類的產品，雖然*所有*產品的欄位會顯示自`GetProductsByCategory`預存程序會傳回所有資料行從`Products`資料表。 當然，我們可以限制或自訂 GridView 從 GridView s 編輯資料行 對話方塊中顯示的欄位。


[![會顯示所有飲料的](using-parameterized-queries-with-the-sqldatasource-vb/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image23.png)

**圖 12**： 會顯示所有飲料 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>步驟 4： 以程式設計方式叫用 SqlDataSource 的`Select()`陳述式

範例我們目前為止所見上一個教學課程與本教學課程中我們有直接繫結 SqlDataSource 控制項至 GridView。 SqlDataSource 控制項的資料，不過，可以透過程式設計方式存取和列舉程式碼中。 當您需要查詢資料檢查，但不要需要加以顯示，這會特別有用。 而不需要撰寫所有未定案 ADO.NET 程式碼，以連接到資料庫中，指定的命令，並擷取結果，您可以讓 SqlDataSource 處理這個 monotonous 的程式碼。

若要說明使用 SqlDataSource 的資料以程式設計的方式，假設老闆已接近您使用的要求建立網頁，顯示隨機選取的類別和其相關聯的產品名稱。 也就是說，當使用者存取此頁面，我們想要隨機選擇的類別目錄`Categories`資料表，並顯示類別目錄名稱，然後再列出屬於該類別目錄的產品。

若要完成這項作業中，我們需要兩個 SqlDataSource 控制項一個抓取隨機類別從`Categories`資料表，而另一個取得 s 產品類別目錄。 我們會建置 SqlDataSource 擷取隨機分類資料錄，在此步驟。製作擷取類別的產品的 SqlDataSource 查看步驟 5。

啟動新增至 SqlDataSource`ParameterizedQueries.aspx`並設定其`ID`至`RandomCategoryDataSource`。 設定，讓它可以使用下列 SQL 查詢：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample10.sql)]

`ORDER BY NEWID()`傳回以隨機順序排序的記錄 (請參閱[使用`NEWID()`隨機排序記錄](http://www.sqlteam.com/item.asp?ItemID=8747))。 `SELECT TOP 1`從結果集傳回第一筆記錄。 放在一起，此查詢會傳回`CategoryID`和`CategoryName`從單一的隨機選取的分類資料行值。

若要顯示類別目錄 s`CategoryName`值、 標籤 Web 控制項加入網頁，請設定其`ID`屬性`CategoryNameLabel`，並以清除其`Text`屬性。 若要從 SqlDataSource 控制項，以程式設計方式擷取資料，我們要叫用其`Select()`方法。 [ `Select()`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx)單一輸入的參數的型別必須要有[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)，以指定如何將資料應該依現狀在傳回之前。 這可以包含指示排序及篩選資料，而且由 Web 控制項時排序，或從 SqlDataSource 控制項的資料進行分頁的資料。 我們的範例，我們不 t 需要資料在傳回之前修改，因此將會傳入`DataSourceSelectArguments.Empty`物件。

`Select()`方法會傳回該物件會實作`IEnumerable`。 SqlDataSource 控制項 s 的值而定的精確的類型傳回[`DataSourceMode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)。 上一個教學課程所述，這個屬性可以設定為值的其中一個`DataSet`或`DataReader`。 如果設定為`DataSet`、`Select()`方法會傳回[DataView](https://msdn.microsoft.com/library/01s96x0z.aspx)物件; 如果設為`DataReader`，它會傳回該物件會實作[ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx)。 因為`RandomCategoryDataSource`SqlDataSource 具有其`DataSourceMode`屬性設定為`DataSet`（預設值），我們會使用 DataView 物件。

下列程式碼說明如何擷取的記錄從`RandomCategoryDataSource`為 DataView SqlDataSource 以及如何讀取`CategoryName`從第一個 DataView 資料列的資料行值：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample11.vb)]

`randomCategoryView(0)`傳回第一個`DataRowView`在 DataView 中。 `randomCategoryView(0)("CategoryName")`傳回的值`CategoryName`此第一個資料列中的資料行。 請注意，在 DataView 鬆散型別。 若要參考特定資料行值，我們需要傳入資料行做為字串 （類別名稱，在此情況下）。 圖 13 顯示中顯示的訊息`CategoryNameLabel`檢視網頁時。 當然，顯示的實際類別目錄名稱會隨機選取`RandomCategoryDataSource`SqlDataSource 每個開啟的頁面 （包括回傳）。


[![名稱會顯示在隨機選取的類別 s](using-parameterized-queries-with-the-sqldatasource-vb/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image25.png)

**圖 13**: 隨機選取類別目錄名稱會顯示的 s ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image26.png))


> [!NOTE]
> 如果 SqlDataSource 控制項 s`DataSourceMode`屬性已設定為`DataReader`，傳回的值從`Select()`方法，需要轉換成`IDataReader`。 讀取`CategoryName`從第一個資料列中，我們 d 的資料行值使用類似的程式碼：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample12.vb)]

使用 SqlDataSource 隨機選取類別目錄時，我們將會列出類別的產品 GridView 準備好。

> [!NOTE]
> 而不是使用標籤 Web 控制項來顯示類別 s 的名稱，我們無法加入 FormView 或 DetailsView 的頁面上，繫結至 SqlDataSource。 使用標籤，不過，允許我們探討如何以程式設計方式叫用 SqlDataSource 的`Select()`陳述式和其產生的資料，在程式碼中使用工作。


## <a name="step-5-assigning-parameter-values-programmatically"></a>步驟 5： 以程式設計的方式指派參數值

所有範例我們目前為止所見，在此教學課程中我們已經使用硬式編碼的參數值或一個來自其中一個預先定義的參數來源 （querystring 值、 Web 上的控制項 頁面上，依此類推）。 不過，SqlDataSource 控制項的參數也可以設定以程式設計的方式。 若要完成目前範例中，我們需要傳回所有屬於指定的類別目錄的產品的 SqlDataSource。 將會有這個 SqlDataSource`CategoryID`參數必須設定其值為基礎`CategoryID`所傳回的資料行值`RandomCategoryDataSource`在 SqlDataSource`Page_Load`事件處理常式。

啟動頁面中加入 GridView 和繫結到名為新 SqlDataSource `ProductsByCategoryDataSource`。 我們未在步驟 3 中的像是，請設定 SqlDataSource，讓它會叫用`GetProductsByCategory`預存程序。 保留參數來源下拉式清單設定為 [無]，但不是會輸入預設值，因為我們會以程式設計方式設定這個預設值。


[![未指定參數的來源或預設值](using-parameterized-queries-with-the-sqldatasource-vb/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image27.png)

**圖 14**： 未指定參數的來源或預設值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image28.png))


完成 SqlDataSource 精靈之後，產生的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample13.aspx)]

我們可以指派`DefaultValue`的`CategoryID`參數以程式設計方式在`Page_Load`事件處理常式：


[!code-vb[Main](using-parameterized-queries-with-the-sqldatasource-vb/samples/sample14.vb)]

與此新增功能，此頁面包含的 GridView 會顯示與隨機選取的類別相關聯的產品。


[![未指定參數的來源或預設值](using-parameterized-queries-with-the-sqldatasource-vb/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-vb/_static/image29.png)

**圖 15**： 未指定參數的來源或預設值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-vb/_static/image30.png))


## <a name="summary"></a>總結

SqlDataSource 可讓網頁開發人員定義參數化的查詢，其參數值可以是硬式編碼，從預先定義的參數來源提取或以程式設計的方式指派。 在此教學課程中我們可了解如何製作特定 SQL 查詢和預存程序的設定資料來源精靈的參數化的查詢。 我們也會探討使用硬式編碼參數的來源，Web 控制項做為參數來源，並以程式設計方式指定參數值。

像 ObjectDataSource 與 SqlDataSource 也提供功能來修改其基礎資料。 在下一個教學課程中我們會探討如何定義`INSERT`， `UPDATE`，和`DELETE`SqlDataSource 陳述式。 一旦加這些陳述式之後，我們可以利用內建插入、 編輯和刪除固有 GridView、 DetailsView 和 FormView 控制項的功能。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Scott 西與克萊德 Randell Schmidt，Ken Pespisa。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](querying-data-with-the-sqldatasource-control-vb.md)
[下一頁](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)
