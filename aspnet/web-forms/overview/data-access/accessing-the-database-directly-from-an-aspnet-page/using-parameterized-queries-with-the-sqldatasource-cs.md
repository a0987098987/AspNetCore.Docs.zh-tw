---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
title: 使用參數化的查詢以 sqldatasource 進行 (C#) |Microsoft Docs
author: rick-anderson
description: 在本教學課程中，我們會繼續我們看看 SqlDataSource 控制項，並了解如何定義參數化的查詢。 參數可以指定這兩個宣告...
ms.author: riande
ms.date: 02/20/2007
ms.assetid: 9128aaac-afe2-449f-84b2-bb1d035083c4
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/using-parameterized-queries-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 654c0ce5520a206e5e8e2fd20bed92ac1075bfe9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834632"
---
<a name="using-parameterized-queries-with-the-sqldatasource-c"></a>使用參數化的查詢以 sqldatasource 進行 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_48_CS.exe)或[下載 PDF](using-parameterized-queries-with-the-sqldatasource-cs/_static/datatutorial48cs1.pdf)

> 在本教學課程中，我們會繼續我們看看 SqlDataSource 控制項，並了解如何定義參數化的查詢。 參數會同時以宣告方式或以程式設計的方式，可以指定，而且可以從數個位置，例如查詢字串、 工作階段狀態，其他控制項和多個提取。


## <a name="introduction"></a>簡介

在上一個教學課程中，我們了解如何直接從資料庫擷取資料時，用於 SqlDataSource 控制項的內容。 使用 「 設定資料來源精靈 」，我們可以選擇資料庫，然後將： 挑選要從資料表或檢視中，傳回的資料行輸入自訂的 SQL 陳述式：或使用預存程序。 從資料表或檢視表選取資料行，或輸入自訂的 SQL 陳述式，SqlDataSource 控制項是否 s`SelectCommand`屬性會被指派所產生的特定 SQL`SELECT`陳述式，而且這是此`SELECT`陳述式時執行SqlDataSource 的`Select()`叫用方法時 (以程式設計方式或自動從資料 Web 控制項)。

SQL`SELECT`陳述式中缺少的上一個教學課程的示範使用`WHERE`子句。 在 `SELECT`陳述式，`WHERE`子句可以用來限制傳回的結果。 例如，若要顯示的成本大於 $50.00 的產品名稱，我們可以使用下列查詢：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample1.sql)]

一般而言，將使用中的值`WHERE`子句會決定某些外部來源，例如查詢字串值、 工作階段變數或從網頁上的 Web 控制項的使用者輸入。 在理想情況下，使用指定這類輸入*參數*。 使用 Microsoft SQL Server，參數會表示使用`@parameterName`，如下所示：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample2.sql)]

SqlDataSource 支援參數化的查詢，這兩者`SELECT`陳述式並`INSERT`， `UPDATE`，和`DELETE`陳述式。 此外，參數值可以自動從各種來源的查詢字串，工作階段狀態，在頁面上，控制項和等或可以透過程式設計方式指定。 在本教學課程中，我們會看到如何如何以指定參數值以宣告方式和以程式設計方式定義參數化的查詢。

> [!NOTE]
> 在上一個教學課程中，我們會比較 ObjectDataSource 透過以 sqldatasource 進行，而且要注意的是其概念的相似之處的第一次 46 教學課程已我們選擇的工具。 參數也擴充這些相似之處。 ObjectDataSource 的參數對應到商務邏輯層中的方法的輸入參數。 使用 SqlDataSource，參數會定義直接在 SQL 查詢。 這兩個控制項都有參數的集合，這些他們`Select()`， `Insert()`， `Update()`，和`Delete()`方法以及兩者都可以從預先定義的來源 （查詢字串值、 工作階段變數等等填入這些參數值) 或以程式設計的方式指派。


## <a name="creating-a-parameterized-query"></a>建立參數型查詢

SqlDataSource 控制項 s 設定資料來源精靈提供三種途徑來定義要擷取資料庫中的記錄執行的命令：

- 方法是選擇從現有的資料表或檢視中，資料行
- 輸入自訂的 SQL 陳述式，或
- 選擇預存程序

當從現有的資料表或檢視表，為參數的資料行中挑選`WHERE`子句必須指定透過 加入`WHERE`子句對話方塊。 當建立自訂的 SQL 陳述式，不過，您可以輸入的參數，直接將`WHERE`子句 (使用`@parameterName`表示每個參數)。 A[預存程序](http://www.awprofessional.com/articles/article.asp?p=25288&amp;rl=1)組成一或多個 SQL 陳述式，而且這些陳述式可以參數化。 SQL 陳述式中使用的參數不過，必須傳遞做為輸入參數的預存程序。

因為建立參數化的查詢取決於如何 SqlDataSource 的`SelectCommand`已指定，可讓 s 看所有三種方法。 若要開始，開啟`ParameterizedQueries.aspx`頁面中`SqlDataSource`資料夾中，將 SqlDataSource 控制項從工具箱拖曳至設計工具中，並設定其`ID`至`Products25BucksAndUnderDataSource`。 接下來，按一下 設定資料來源連結，從控制項 s 智慧標籤。 選取要使用的資料庫 (`NORTHWINDConnectionString`)，按一下 [下一步]。

## <a name="step-1-adding-awhereclause-when-picking-the-columns-from-a-table-or-view"></a>步驟 1： 加入`WHERE`子句時挑選資料行從資料表或檢視表

選取時要傳回從 SqlDataSource 控制項使用資料庫的資料，請設定資料來源精靈可讓我們只要挑選要傳回從現有的資料表或檢視 （請參閱 圖 1） 的資料行。 Sql 自動進行建置`SELECT`陳述式，也就是傳送至資料庫時 SqlDataSource 的`Select()`叫用方法。 如同我們在上一個教學課程，從下拉式清單中選取 [產品] 資料表，並檢查`ProductID`， `ProductName`，和`UnitPrice`資料行。


[![挑選要從資料表或檢視傳回的資料行](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image1.png)

**圖 1**： 從資料表或檢視表傳回挑選資料行 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.png))


若要包含`WHERE`中的子句`SELECT`陳述式中，按一下`WHERE`按鈕，會顯示 新增`WHERE`子句對話方塊 （請參閱 圖 2）。 若要加入參數來限制所傳回的結果`SELECT`查詢中，第一次選擇 篩選資料的資料行。 接下來，選擇要用於篩選的運算子 (=、 &lt;， &lt;=，&gt;等等)。 最後，選擇來源參數 s 的值，例如從查詢字串或工作階段狀態。 設定參數之後, 按一下 [新增] 按鈕，將它包含在`SELECT`查詢。

針對此範例，可讓 s 只傳回這些結果其中`UnitPrice`值是否小於或等於 $25.00。 因此，挑選`UnitPrice`的資料行的下拉式清單和&lt;= 從 [運算子] 下拉式清單。 使用硬式編碼的參數值 （例如 $25.00) 時，或以程式設計方式指定為參數值，選取 無從 來源 下拉式清單。 接下來，在 25.00 上的 [值] 文字方塊中輸入硬式編碼的參數值，並按一下 [新增] 按鈕以完成程序。


[![限制傳回的結果加入 WHERE 子句對話方塊](using-parameterized-queries-with-the-sqldatasource-cs/_static/image2.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.png)

**圖 2**： 限制從 新增傳回的結果`WHERE`子句對話方塊 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.png))


加入參數之後, 按一下 [確定] 以返回 [設定資料來源精靈]。 `SELECT`現在應該包含陳述式底部的精靈`WHERE`子句的參數，名為`@UnitPrice`:


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample3.sql)]

> [!NOTE]
> 如果您指定多個條件`WHERE`從 新增子句`WHERE`子句對話方塊中，「 精靈 」 來聯結它們與`AND`運算子。 如果您需要包含`OR`中`WHERE`子句 (例如`WHERE UnitPrice <= @UnitPrice OR Discontinued = 1`)，您需要建置`SELECT`透過自訂的 SQL 陳述式螢幕的陳述式。


完成設定的 SqlDataSource （按 下一步，然後完成），然後檢查 SqlDataSource s 宣告式標記。 標記現在包含`<SelectParameters>`集合，其中詳細說明中的參數的來源`SelectCommand`。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample4.aspx)]

當 SqlDataSource s`Select()`叫用方法時，`UnitPrice`參數值 (25.00) 套用至`@UnitPrice`中的參數`SelectCommand`再傳送至資料庫。 最後結果就是，只有在小於或等於 $25.00 從傳回的產品`Products`資料表。 若要確認這點，將 GridView 加入頁面中，將它繫結到此資料來源，然後檢視該頁面，透過瀏覽器。 您應該只會看到列出的小於或等於 $25.00，如 圖 3 會確認這些產品。


[![這些產品的小於或等於 $25.00 才會顯示](using-parameterized-queries-with-the-sqldatasource-cs/_static/image3.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.png)

**圖 3**： 顯示只有那些產品的小於或等於 $25.00 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.png))


## <a name="step-2-adding-parameters-to-a-custom-sql-statement"></a>步驟 2： 將參數加入至自訂的 SQL 陳述式

新增自訂的 SQL 陳述式時您可以輸入`WHERE`子句明確或查詢產生器的篩選器儲存格中指定值。 為了示範這點，讓其價格均不超過特定閾值 GridView 中顯示只是這些產品的 s。 新增文字方塊中的，若要開始`ParameterizedQueries.aspx`頁面，即可從使用者收集此臨界值。 設定文字方塊 s`ID`屬性設`MaxPrice`。 將 Button Web 控制項，並設定其`Text`屬性，以顯示比對的產品。

接下來，拖曳到頁面的 GridView，並從它的智慧標籤中，選擇建立新的 SqlDataSource 名為`ProductsFilteredByPriceDataSource`。 設定資料來源 精靈中，從繼續進行 指定自訂 SQL 陳述式或預存程序畫面 （請參閱 圖 4），並輸入下列查詢：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample5.sql)]

輸入之後的查詢 （手動或透過 查詢產生器），請按一下 下一步。


[![只傳回的產品小於或等於參數值](using-parameterized-queries-with-the-sqldatasource-cs/_static/image4.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.png)

**圖 4**： 傳回只有那些產品的小於或等於參數值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.png))


因為查詢包含參數，請在精靈中的下一個畫面會提示我們輸入參數值的來源。 從參數來源下拉式清單中選擇控制項和`MaxPrice`(TextBox 控制項的`ID`值) 從 [ControlID] 下拉式清單。 您也可以輸入要在其中使用者未輸入任何文字的情況下使用選擇性的預設值`MaxPrice`文字方塊中。 目前，請勿輸入預設值。


[![MaxPrice 文字方塊的 Text 屬性做為參數來源](using-parameterized-queries-with-the-sqldatasource-cs/_static/image5.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.png)

**圖 5**: `MaxPrice` TextBox s`Text`屬性做為參數的來源 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.png))


接下來，按一下以完成設定資料來源精靈，然後完成。 GridView、 文字方塊、 按鈕和 SqlDataSource 的宣告式標記如下：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample6.aspx)]

請注意，SqlDataSource s 內的參數`<SelectParameters>`一節`ControlParameter`，其中包含其他屬性，例如`ControlID`和`PropertyName`。 當 SqlDataSource s`Select()`叫用方法時，`ControlParameter`抓取指定的 Web 控制項屬性的值，並將它指派給對應的參數，在`SelectCommand`。 在此範例中， `MaxPrice` s 文字屬性做為`@MaxPrice`參數值。

花點時間檢視此頁面，透過瀏覽器。 第一次造訪網頁時，或每當`MaxPrice`文字方塊缺少任何記錄會顯示在 GridView 的值。


[![沒有記錄會顯示當 MaxPrice 文字方塊是空的](using-parameterized-queries-with-the-sqldatasource-cs/_static/image6.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.png)

**圖 6**： 不記錄會顯示當`MaxPrice`文字方塊是空的 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.png))


會顯示任何產品的原因是因為根據預設，參數值為空字串會轉換成資料庫`NULL`值。 因為比較`[UnitPrice] <= NULL`一律評估為 False，未傳回任何結果。

文字方塊中輸入值，例如 5.00，，然後按一下 [顯示比對的產品] 按鈕。 在回傳時，SqlDataSource 會通知 GridView 在於其中一個參數的來源已變更。 因此，GridView 重新繫結至 SqlDataSource，顯示這些產品小於或等於 5.00 美元。


[![顯示產品的小於或等於 $5.00](using-parameterized-queries-with-the-sqldatasource-cs/_static/image7.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.png)

**圖 7**： 顯示產品的小於或等於 $5.00 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.png))


## <a name="initially-displaying-all-products"></a>一開始顯示 所有產品

而不是第一次載入頁面時，請顯示任何產品，我們可能想要顯示*所有*產品。 其中一種方式列出所有的產品時`MaxPrice`文字方塊是空的是將參數 s 的預設值設定為一些個人瘋狂高的值，例如 1000000，因為它不太可能，Northwind Traders 將曾有清查單價 s 超過 $1,000,000。 不過，這種方法是 shortsighted，並在其他情況下可能無法運作。

在先前的教學課程-[宣告式的參數](../basic-reporting/declarative-parameters-cs.md)並[主版/詳細篩選使用 DropDownList](../masterdetail/master-detail-filtering-with-a-dropdownlist-cs.md)我們所面臨了類似的問題。 我們的解決方案是要將此邏輯放在商務邏輯層。 具體來說，BLL 檢查傳入的值，如果它是`NULL`或部分保留值、 呼叫路由至 DAL 方法，以傳回所有記錄。 如果傳入的值是正常的篩選值，已呼叫來執行使用參數化 SQL 陳述式的 DAL 方法`WHERE`子句與所提供的值。

不幸的是，我們會略過架構使用 SqlDataSource 時。 相反地，我們需要自訂 SQL 陳述式，以智慧方式擷取所有記錄，如果`@MaximumPrice`參數是`NULL`或保留的值。 針對此練習，可讓 s 就因此，如果`@MaximumPrice`參數等於`-1.0`，然後*所有*的記錄會傳回 (`-1.0`擔任保留的值，因為沒有產品可以有負`UnitPrice`值)。 若要這麼做，我們可以使用下列 SQL 陳述式：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample7.sql)]

這`WHERE`子句會傳回*所有*記錄如果`@MaximumPrice`參數等於`-1.0`。 如果參數值不是`-1.0`，只有那些產品其`UnitPrice`小於或等於`@MaximumPrice`傳回參數值。 藉由設定的預設值`@MaximumPrice`參數來`-1.0`，在第一個頁面載入 (或每當`MaxPrice`文字方塊是空的)，`@MaximumPrice`值為`-1.0`，並且會顯示所有產品。


[![現在所有產品都會顯示當 MaxPrice 文字方塊是空的](using-parameterized-queries-with-the-sqldatasource-cs/_static/image8.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.png)

**圖 8**： 顯示時，現在所有產品都都`MaxPrice`文字方塊是空的 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image16.png))


有幾個需要注意的事項来注意這種方法。 首先，了解參數的資料型別由其推斷 s SQL 查詢中的使用方式。 如果您變更`WHERE`子句，從`@MaximumPrice = -1.0`到`@MaximumPrice = -1`，執行階段會將參數視為整數。 如果您隨後嘗試指派`MaxPrice`的十進位值 （例如 5.00) 文字方塊中，會發生錯誤，因為它無法將 5.00 轉換成整數。 若要解決此問題，是請確定您使用`@MaximumPrice = -1.0`中`WHERE`子句或更好的是，設定`ControlParameter`物件的`Type`為十進位數的屬性。

其次，藉由新增`OR @MaximumPrice = -1.0`要`WHERE`子句，查詢引擎無法使用索引上`UnitPrice`（假設其存在），進而導致資料表掃描。 如果沒有夠大的數字中的記錄，這會影響效能`Products`資料表。 好的做法是將此邏輯移至 預存程序所在`IF`陳述式會執行`SELECT`從查詢`Products`資料表而不需要`WHERE`子句時要傳回需要的所有記錄，或其中一個其`WHERE`子句只包含`UnitPrice`準則，以便可以使用索引。

## <a name="step-3-creating-and-using-parameterized-stored-procedures"></a>步驟 3： 建立和使用參數化預存程序

預存程序可以包含一組可用的輸入參數的預存程序中定義的 SQL 陳述式中。 時設定 SqlDataSource 使用接受輸入的參數的預存程序，可以使用相同的技術與特定 SQL 陳述式一樣指定這些參數值。

為了說明使用以 sqldatasource 進行的預存程序，可讓 s，請建立名為 Northwind 資料庫中的新的預存程序`GetProductsByCategory`，它會接受名為的參數`@CategoryID`，並傳回所有產品的資料行其`CategoryID`資料行比對`@CategoryID`。 若要建立預存程序，請移至 [伺服器總管] 中，並向下切入至`NORTHWND.MDF`資料庫。 （如果您不要看到 [伺服器總管] 中，使其移至 [檢視] 功能表，然後選取 [伺服器總管] 選項。）

從`NORTHWND.MDF`資料庫、 預存程序 資料夾上按一下滑鼠右鍵、 選擇加入新預存程序，並輸入下列語法：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample8.sql)]

按一下儲存圖示 （或 Ctrl + S） 以儲存預存程序。 您可以用滑鼠右鍵按一下從預存程序] 資料夾，然後選擇 [執行測試的預存程序。 這會提示您輸入的預存程序的參數 (`@CategoryID`，這個執行個體中) 之後的結果會顯示在 [輸出] 視窗。


[![GetProductsByCategory 預存程序執行時未使用@CategoryID為 1](using-parameterized-queries-with-the-sqldatasource-cs/_static/image9.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image17.png)

**圖 9**:`GetProductsByCategory`預存程序執行時未使用`@CategoryID`為 1 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image18.png))


可讓使用此預存程序顯示在 GridView 中的飲料類別目錄中的所有產品的 s。 加入至頁面的新 GridView 和繫結至名為新 SqlDataSource `BeverageProductsDataSource`。 繼續指定自訂 SQL 陳述式或預存程序 畫面，選取預存程序選項按鈕，然後挑選`GetProductsByCategory`預存程序，從下拉式清單。


[![選取 GetProductsByCategory 預存程序，從下拉式清單](using-parameterized-queries-with-the-sqldatasource-cs/_static/image10.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image19.png)

**圖 10**： 選取 `GetProductsByCategory`預存程序，從下拉式清單 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image20.png))


由於預存程序會接受輸入的參數 (`@CategoryID`)，按一下 [下一步] 會提示我們輸入來指定此參數的值的來源。 飲料`CategoryID`為 1，因此保留無參數來源下拉式清單，然後在 [預設值] 文字方塊中輸入 1。


[![使用硬式編碼的值為 1，以傳回 「 飲料 」 分類中的產品](using-parameterized-queries-with-the-sqldatasource-cs/_static/image11.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image21.png)

**圖 11**： 使用 Hard-Coded 值為 1，傳回 「 飲料 」 分類中的產品 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image22.png))


下列所示宣告式標記，使用預存程序，SqlDataSource s 時`SelectCommand`屬性設定為預存程序名稱和[`SelectCommandType`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommandtype.aspx)設定為`StoredProcedure`，這表示`SelectCommand`是預存程序，而不是特定 SQL 陳述式的名稱。


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample9.aspx)]

測試瀏覽器頁面。 顯示只屬於 「 飲料 」 分類的產品，雖然*所有*產品的欄位會顯示自`GetProductsByCategory`預存程序會傳回所有的資料行`Products`資料表。 當然，我們可以限制或自訂從 GridView 編輯資料行對話方塊 GridView 中顯示的欄位。


[![會顯示所有飲料的](using-parameterized-queries-with-the-sqldatasource-cs/_static/image12.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image23.png)

**圖 12**： 會顯示所有飲料的 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image24.png))


## <a name="step-4-programmatically-invoking-a-sqldatasource-sselectstatement"></a>步驟 4： 以程式設計方式叫用 SqlDataSource 的`Select()`陳述式

這些範例我們目前為止在上一個教學課程和本教學課程中所見的 ve 有直接繫結 SqlDataSource 控制項至 GridView。 SqlDataSource 控制項的資料，不過，可以透過程式設計方式存取和列舉程式碼中。 這項功能在您要查詢資料檢查，但不要需要顯示它是特別有用的。 而不需要撰寫所有的未定案 ADO.NET 程式碼，以連接到資料庫、 指定的命令，並擷取結果，您可以讓 SqlDataSource 處理這個單調的程式碼。

若要說明使用 SqlDataSource 的資料以程式設計的方式，假設您的老闆已接近您並要求建立網頁，顯示的隨機選取的類別和其相關聯的產品名稱。 也就是說，當使用者瀏覽此頁面，我們想要隨機選擇 從類別`Categories`資料表中，顯示類別目錄名稱，並接著列出屬於該類別目錄的產品。

若要完成這項作業中，我們必須擷取隨機的類別目錄，從兩個的 SqlDataSource 控制項一`Categories`資料表並使用另一個取得分類的產品。 我們將建置 SqlDataSource 擷取隨機的類別目錄記錄在此步驟中;步驟 5 會查看製作 SqlDataSource 擷取類別的產品。

首先新增至 SqlDataSource`ParameterizedQueries.aspx`並設定其`ID`至`RandomCategoryDataSource`。 因此，它會使用下列 SQL 查詢，請設定它：


[!code-sql[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample10.sql)]

`ORDER BY NEWID()` 傳回以隨機順序排序的記錄 (請參閱[Using`NEWID()`隨機排序記錄](http://www.sqlteam.com/item.asp?ItemID=8747))。 `SELECT TOP 1` 從結果集傳回第一筆記錄。 將放在一起，則此查詢會傳回`CategoryID`和`CategoryName`從單一的隨機選取的類別目錄的資料行值。

若要顯示類別目錄 s`CategoryName`值，將標籤 Web 控制項加入頁面，設定其`ID`屬性設`CategoryNameLabel`，並清除其`Text`屬性。 若要以程式設計方式從 SqlDataSource 控制項擷取資料，我們要叫用其`Select()`方法。 [ `Select()`方法](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.select.aspx)預期單一輸入的參數為類型[ `DataSourceSelectArguments` ](https://msdn.microsoft.com/library/system.web.ui.datasourceselectarguments.aspx)，指定如何將資料應該每來就訊息再傳回。 這可以包括有關排序和篩選資料，並正由 Web 控制項的排序，或從 SqlDataSource 控制項的資料進行分頁時的資料。 我們的範例，不過，我們不修改才能被傳回，t 需要資料，因此會傳遞在`DataSourceSelectArguments.Empty`物件。

`Select()`方法會傳回該物件會實作`IEnumerable`。 精確的型別傳回 SqlDataSource 控制項 s 的值而定[`DataSourceMode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)。 如先前的教學課程中所述，這個屬性可以設定的其中一個值`DataSet`或`DataReader`。 如果設定為`DataSet`，則`Select()`方法會傳回[DataView](https://msdn.microsoft.com/library/01s96x0z.aspx)物件; 如果設為`DataReader`，它會傳回該物件會實作[ `IDataReader` ](https://msdn.microsoft.com/library/system.data.idatareader.aspx)。 由於`RandomCategoryDataSource`SqlDataSource 有其`DataSourceMode`屬性設定為`DataSet`（預設值），我們會使用 DataView 物件。

下列程式碼說明如何擷取的記錄，從`RandomCategoryDataSource`DataView 為 SqlDataSource 以及如何讀取`CategoryName`從第一個 DataView 的資料列的資料行值：


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample11.cs)]

`randomCategoryView[0]` 傳回第一個`DataRowView`DataView 中。 `randomCategoryView[0]["CategoryName"]` 傳回的值`CategoryName`此第一個資料列中的資料行。 請注意，DataView 鬆散型別。 若要參考特定資料行值，我們需要的資料行的名稱傳遞為字串 (在此情況下 CategoryName)。 [圖 13] 顯示中顯示的訊息`CategoryNameLabel`檢視頁面時。 當然，顯示的實際類別名稱會隨機選取`RandomCategoryDataSource`SqlDataSource 在每次造訪 [（包括回傳）] 頁面上的。


[![名稱會顯示在隨機選取的類別 s](using-parameterized-queries-with-the-sqldatasource-cs/_static/image13.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image25.png)

**圖 13**： 顯示名稱的隨機選取的類別 s ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image26.png))


> [!NOTE]
> 如果 SqlDataSource 控制項 s`DataSourceMode`屬性已設定為`DataReader`，傳回的值從`Select()`方法將需要轉型為`IDataReader`。 若要讀取`CategoryName`從第一個資料列中，我們的 d 資料行值使用類似的程式碼：


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample12.cs)]

使用 SqlDataSource 隨機選取一個類別，我們準備好將 GridView 列出類別的產品。

> [!NOTE]
> 而不是使用標籤 Web 控制項來顯示類別 s 的名稱，我們可能已加入 FormView 或 DetailsView 頁面上，繫結至 SqlDataSource。 不過，使用標籤，讓我們將探討如何以程式設計方式叫用 SqlDataSource 的`Select()`陳述式，並使用其程式碼所產生的資料。


## <a name="step-5-assigning-parameter-values-programmatically"></a>步驟 5： 以程式設計的方式指派參數值

所有範例中我們目前為止所見，在本教學課程已使用硬式編碼的參數值或一個來自其中一個預先定義的參數來源 （ 頁面上，等等的 Web 控制項中的 查詢字串值）。 不過，SqlDataSource 控制項的參數也可以設定以程式設計的方式。 若要完成我們目前的範例中，我們需要 SqlDataSource 傳回所有屬於指定分類的產品。 將會有此 SqlDataSource`CategoryID`參數，其值必須設為基礎`CategoryID`所傳回的資料行值`RandomCategoryDataSource`SqlDataSource 中的`Page_Load`事件處理常式。

啟動新增至頁面的 GridView，並將它繫結至名為新 SqlDataSource `ProductsByCategoryDataSource`。 我們未在步驟 3 中的更像，請設定 SqlDataSource，讓它會叫用`GetProductsByCategory`預存程序。 保留參數來源下拉式清單設定為 [無]，但不是會輸入預設值，因為我們會以程式設計方式設定此預設值。


[![未指定參數的來源或預設值](using-parameterized-queries-with-the-sqldatasource-cs/_static/image14.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image27.png)

**圖 14**： 未指定參數的來源或預設值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image28.png))


完成 SqlDataSource 精靈之後，產生的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample13.aspx)]

我們可以指派`DefaultValue`的`CategoryID`參數以程式設計方式在`Page_Load`事件處理常式：


[!code-csharp[Main](using-parameterized-queries-with-the-sqldatasource-cs/samples/sample14.cs)]

此步驟中，此頁面包含的 GridView 會顯示與隨機選取的類別相關聯的產品。


[![未指定參數的來源或預設值](using-parameterized-queries-with-the-sqldatasource-cs/_static/image15.gif)](using-parameterized-queries-with-the-sqldatasource-cs/_static/image29.png)

**圖 15**： 未指定參數的來源或預設值 ([按一下以檢視完整大小的影像](using-parameterized-queries-with-the-sqldatasource-cs/_static/image30.png))


## <a name="summary"></a>總結

SqlDataSource 可讓網頁程式開發人員定義參數化的查詢，其參數值可以是硬式編碼，從預先定義的參數來源提取或以程式設計方式指派。 在本教學課程中，我們看到如何製作特定 SQL 查詢和預存程序的設定資料來源精靈的參數化的查詢。 我們也會探討使用硬式編碼參數的來源，Web 控制項做為參數來源，並以程式設計方式指定的參數值。

例如使用 ObjectDataSource、 SqlDataSource 也提供功能來修改其基礎資料。 在下一個教學課程中我們將探討如何定義`INSERT`， `UPDATE`，和`DELETE`以 sqldatasource 進行的陳述式。 加入這些陳述式之後, 我們可以利用內建的插入、 編輯和刪除功能的 GridView、 DetailsView 和 FormView 控制項本身。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Scott 西與克萊德、 Randell Schmidt 和 Ken Pespisa。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](querying-data-with-the-sqldatasource-control-cs.md)
> [下一頁](inserting-updating-and-deleting-data-with-the-sqldatasource-cs.md)
