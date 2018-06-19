---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: 查詢資料與 SqlDataSource 控制項 (VB) |Microsoft 文件
author: rick-anderson
description: 在先前的教學課程中，我們會使用 ObjectDataSource 控制項將完全分開資料存取層中的展示層。 從這個老師...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: 6f886ca85a2a4dea5daeff109370bedc1a3f7265
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30876562"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>查詢資料與 SqlDataSource 控制項 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe)或[下載 PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> 在先前的教學課程中，我們會使用 ObjectDataSource 控制項將完全分開資料存取層中的展示層。 我們從開始本教學課程，了解 SqlDataSource 控制項如何用於簡單的應用程式不需要這類嚴格隔離的簡報和資料存取。


## <a name="introduction"></a>簡介

所有的教學課程，我們檢驗到目前為止我們已經使用簡報、 商務邏輯和資料存取層級所組成的階層式的架構。 資料存取層 (DAL) 第一個教學課程中製作 ([建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)) 和商務邏輯層，在第二個 ([建立商務邏輯層](../introduction/creating-a-business-logic-layer-vb.md))。 從開始[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程，我們可了解如何使用 ASP.NET 2.0 s 新 ObjectDataSource 控制項以宣告方式與從展示層架構的介面。

雖然教學課程到目前為止的所有已使用架構來處理資料，它也可存取、 插入、 更新和刪除資料庫資料直接從 ASP.NET 網頁，略過架構。 如此一來直接在網頁中放置的特定資料庫查詢和商務邏輯。 充分的大型或複雜應用程式的設計、 實作和使用階層式的架構是非常重要的成功、 可更新性，與應用程式的可維護性。 開發強固的架構，不過，可能就不需要建立非常簡單、 一次性應用程式時。

ASP.NET 2.0 提供五個內建資料來源控制項[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)， [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)，和[Treeview](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)。 SqlDataSource 可以用於存取和修改資料直接從關聯式資料庫，包括 Microsoft SQL Server、 Microsoft Access、 Oracle、 MySQL 和其他項目。 在本教學課程和三個 下一步，我們將檢驗如何使用 SqlDataSource 控制項時，瀏覽如何查詢和篩選資料庫資料，以及如何使用以 SqlDataSource 插入、 更新和刪除資料。


![ASP.NET 2.0 包含五個內建資料來源控制項](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**圖 1**: ASP.NET 2.0 包含五個內建資料來源控制項


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>比較 ObjectDataSource 與 SqlDataSource

就概念而言，ObjectDataSource 和 SqlDataSource 控制項是只是資料的 proxy。 中所述[顯示資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程中，ObjectDataSource 的屬性會指出物件類型，提供資料和選取，叫用方法插入、 更新和刪除資料從基礎的物件類型。 ObjectDataSource 的屬性已設定後，資料例如 GridView、 DetailsView，或是 DataList Web 控制項可以繫結至控制項，使用 ObjectDataSource s `Select()`， `Insert()`， `Delete()`，和`Update()`方法基礎結構進行互動。

SqlDataSource 提供相同的功能，但會針對關聯式資料庫，而不是物件程式庫。 使用 SqlDataSource，我們必須指定資料庫連接字串和的特定 SQL 查詢，或預存程序執行插入、 更新、 刪除和擷取資料。 SqlDataSource s `Select()`， `Insert()`， `Update()`，和`Delete()`方法叫用時，連接到指定的資料庫，並發出適當的 SQL 查詢。 如下列圖表所示，這些方法可以執行 grunt 連接到資料庫、 發出查詢，並傳回結果。


![SqlDataSource 做為資料庫的 Proxy](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**圖 2**: SqlDataSource 做為資料庫的 Proxy


> [!NOTE]
> 在此教學課程中我們將焦點放在從資料庫擷取資料。 在[插入、 更新和拒資料與 SqlDataSource 控制項](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)教學課程，我們會看到如何設定支援插入、 更新和刪除 SqlDataSource。


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource 和 AccessDataSource 控制項

SqlDataSource 控制項，除了 ASP.NET 2.0 也包含 AccessDataSource 控制項。 這兩個不同控制項會導致許多開發人員熟悉 ASP.NET 2.0 懷疑 AccessDataSource 控制項為了專門與 Microsoft Access 使用 SqlDataSource 控制項設計以獨佔方式使用 Microsoft SQL Server。 雖然 AccessDataSource 設計來搭配 Microsoft Access，SqlDataSource 控制項適用於*任何*可以透過.NET 存取關聯式資料庫。 這包括任何 OleDb 或 ODBC 相容資料存放區，例如 Microsoft SQL Server、 Microsoft Access、 Oracle、 Informix、 MySQL 和 PostgreSQL，還有許多其他。

AccessDataSource 和 SqlDataSource 控制項之間的唯一差異是指定資料庫連接資訊的方式。 AccessDataSource 控制項需要檔案路徑來存取資料庫檔案。 SqlDataSource，相反地，需要完整的連接字串。

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>步驟 1： 建立 SqlDataSource 網頁

我們可以開始探索如何直接使用資料庫資料使用 SqlDataSource 控制項之前，可讓 s 先花點時間我們網站專案中，我們需要針對此教學課程中，下一步的三個建立的 ASP.NET 網頁。 加入新的資料夾，名為啟動`SqlDataSource`。 接下來，將下列 ASP.NET 網頁新增至該資料夾中，務必要建立關聯的每一頁`Site.master`主版頁面：

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![如需 SqlDataSource 相關教學課程加入 ASP.NET 網頁](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**圖 3**： 加入 ASP.NET 網頁的 SqlDataSource 相關教學課程


如同在其他資料夾，`Default.aspx`中`SqlDataSource`資料夾會列出這些教學課程 > 一節中。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**圖 4**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


最後，將這些四個頁面加入做為項目至`Web.sitemap`檔案。 具體來說，加入下列標記之後新增自訂按鈕的 DataList 和中繼器`<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入和刪除教學課程的項目。


![站台地圖現在包含項目為 SqlDataSource 教學課程](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**圖 5**： 網站導覽現在包含項目為 SqlDataSource 教學課程


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>步驟 2： 加入和設定 SqlDataSource 控制項

先開啟`Querying.aspx`頁面`SqlDataSource`資料夾，然後切換到設計檢視。 拖曳 SqlDataSource 控制項從工具箱拖曳至設計工具和設定其`ID`至`ProductsDataSource`。 如同 ObjectDataSource SqlDataSource 會不會產生轉譯的輸出，因此會顯示為設計介面上的灰色方塊。 若要設定 SqlDataSource，按一下 從 SqlDataSource s 智慧標籤的 設定資料來源 連結。


![按一下 設定從 SqlDataSource s 智慧標籤的資料來源連結](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**圖 6**： 按一下 設定從 SqlDataSource s 智慧標籤的資料來源連結


這會顯示 SqlDataSource 控制項 s 設定資料來源精靈。 雖然精靈 s 中的步驟不同 ObjectDataSource 控制項 s，最終目的是相同，以提供有關如何擷取、 插入、 更新和刪除資料來源中的資料的詳細資料。 對於 SqlDataSource，這必須指定要使用的基礎資料庫，並提供特定 SQL 陳述式或預存程序。

第一個步驟中，精靈會提示我們的資料庫。 下拉式清單包含在 web 應用程式 s 中找不到這些資料庫`App_Data`資料夾和那些已新增至 [伺服器總管] 中的 [資料連線] 節點。 因為我們我們已加入的連接字串`NORTHWIND.MDF`資料庫`App_Data`s 受測專案的資料夾`Web.config`檔案，下拉式清單包含該連接字串中，參考`NORTHWINDConnectionString`。 從下拉式清單中選擇此項目，然後按一下 [下一步]。


![從下拉式清單中選擇 NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**圖 7**： 選擇`NORTHWINDConnectionString`從下拉式清單


選擇資料庫之後, 在精靈會要求查詢傳回的資料。 我們可以指定的資料行的資料表或檢視，以傳回，或可以輸入自訂的 SQL 陳述式或指定預存程序。 您可以切換透過指定此選項，自訂的 SQL 陳述式或預存程序和指定的資料行從資料表或檢視的選項按鈕。

> [!NOTE]
> 這個範例，可讓 s 使用資料表或檢視表選項的指定資料行。 我們會回到精靈，稍後在本教學課程中，瀏覽 指定自訂 SQL 陳述式或預存程序選項。


圖 8 選取指定的資料行從資料表或檢視表的選項按鈕時，顯示 設定 Select 陳述式螢幕。 下拉式清單包含資料表和檢視表，在 Northwind 資料庫中，以選取的資料表或檢視表 s 資料行下方的核取方塊清單中顯示的集合。 對於此範例中，傳回 o d e s `ProductID`， `ProductName`，和`UnitPrice`中的資料行`Products`資料表。 如圖 8 所示，讓這些選取項目精靈顯示產生的 SQL 陳述式之後`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`。


![傳回從 Products 資料表的資料](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**圖 8**： 傳回資料來源`Products`資料表


一旦設定精靈，以傳回`ProductID`， `ProductName`，和`UnitPrice`中的資料行`Products`資料表中，按一下 [下一步] 按鈕。 這個最後的畫面提供檢查先前步驟中設定的查詢結果的機會。 按一下 [測試查詢] 按鈕執行設定`SELECT`陳述式，並顯示在方格中的結果。


![按一下 [測試查詢] 按鈕，檢閱您的 SELECT 查詢](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**圖 9**： 按一下 [測試查詢] 按鈕，檢閱您`SELECT`查詢


若要完成精靈後，按一下 [完成]。

Like 與 ObjectDataSource SqlDataSource 的精靈只是將值指派給控制項 s 的屬性，也就是[ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx)和[ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx)屬性。 完成精靈之後，您 SqlDataSource 控制項 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString`屬性提供有關如何連接到資料庫的資訊。 這個屬性會指定完整的硬式編碼的連接字串值或連接字串中可以指向`Web.config`。 若要參考在 Web.config 中的連接字串值，請使用語法`<%$ expressionPrefix:expressionValue %>`。 一般而言， *expressionPrefix*是 ConnectionStrings 和*expressionValue*是中的連接字串名稱`Web.config` [ `<connectionStrings>`區段](https://msdn.microsoft.com/library/bf7sd233.aspx)。 不過，可以使用語法來參考`<appSettings>`項目或從資源檔的內容。 請參閱[ASP.NET 運算式概觀](https://msdn.microsoft.com/library/d5bd1tad.aspx)如需此語法的詳細資訊。

`SelectCommand`屬性會指定特定 SQL 陳述式或預存程序傳回資料所執行。

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>步驟 3： 加入資料 Web 控制項並繫結至 SqlDataSource

SqlDataSource 設定之後, 可以繫結至 Web 控制項，例如 GridView 或 DetailsView 資料。 本教學課程，可讓 s GridView 中顯示資料。 從 工具箱 拖曳到頁面的 GridView，並將它繫結`ProductsDataSource`SqlDataSource GridView s 智慧標籤在下拉式清單中選擇資料來源。


[![加入 GridView，並將它繫結至 SqlDataSource 控制項](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**圖 10**： 加入 GridView，並將它繫結至 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


一旦您已選取 SqlDataSource 控制項，從下拉式清單中的 GridView s 智慧標籤，Visual Studio 會自動加入 BoundField 或 CheckBoxField 至 GridView 每個資料來源控制項所傳回的資料行。 因為 SqlDataSource 傳回三個資料庫資料行`ProductID`， `ProductName`，和`UnitPrice`GridView 中有三個欄位。

請花一點時間設定 GridView 的三 BoundFields。 變更`ProductName`欄位 s`HeaderText`產品名稱的屬性和`UnitPrice`欄位 s 價格。 也格式化`UnitPrice`欄位做為貨幣。 進行這些修改後, GridView s 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

請瀏覽此頁面，透過瀏覽器。 圖 11 顯示，如 GridView 列出每個產品 s `ProductID`， `ProductName`，和`UnitPrice`值。


[![在 GridView 會顯示每個產品的 ProductID、 ProductName、 和 UnitPrice 值](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**圖 11**: GridView 會顯示每個產品 s `ProductID`， `ProductName`，和`UnitPrice`值 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


在 GridView 時瀏覽頁面時叫用其資料來源控制項的`Select()`方法。 當我們使用 ObjectDataSource 控制項時，這稱為`ProductsBLL`類別的`GetProducts()`方法。 使用 SqlDataSource，不過，`Select()`方法會連線到指定的資料庫和問題`SelectCommand`(`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`，在此範例中)。 SqlDataSource 傳回它 GridView 然後列舉，建立在傳回的每個資料庫記錄的 GridView 的資料列的結果。

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>內建資料 Web 控制項的功能和 SqlDataSource 控制項

一般而言，功能的固有資料 Web 控制項分頁、 排序、 編輯、 刪除、 插入和等等特有資料 Web 控制項，且不依存上使用的資料來源控制項。 也就是說，GridView 可以利用內建的分頁、 排序、 編輯和刪除是否 ObjectDataSource 或 SqlDataSource 繫結。 不過，某些資料 Web 控制項功能，則機密正在使用之資料來源控制項，或對此資料來源控制項的設定。

例如，在[有效率地透過大型量的資料分頁](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)教學課程中我們將討論如何，根據預設，資料 Web 的分頁邏輯控制這個傳回*所有*來自基礎的記錄資料來源，並接著會顯示目前頁面索引和每頁顯示的記錄數目的記錄中的適當子集合。 分頁夠大的結果集時，高效率不佳此模型。 幸運的是，ObjectDataSource 可以設定為支援自訂分頁時，它會傳回要顯示的記錄中的精確子集合。 SqlDataSource 控制項，不過，缺乏實作自訂分頁屬性。

使用 SqlDataSource，就會發生另一個微妙的地方使用分頁和排序。 根據預設，從 SqlDataSource 傳回的資料分頁或透過 GridView 中排序。 若要示範這點，請檢查 GridView 的 s 智慧標記中啟用分頁，並啟用排序選項`Querying.aspx`並確認其運作正常。

排序和分頁運作，因為 SqlDataSource 插入鬆散型別資料集擷取的資料庫資料。 從資料集都可以確定查詢所傳回的基本層面實作分頁的記錄總數。 此外，資料集的結果可以透過 DataView 進行排序。 這些功能會自動使用 SqlDataSource GridView 要求分頁或排序資料。

SqlDataSource 可以設定為傳回 DataReader 而不是資料集，藉由變更其[`DataSourceMode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)從`DataSet`（預設） 至`DataReader`。 使用 DataReader 可能被慣用的情況下將 SqlDataSource 的結果傳遞到現有的程式碼預期 DataReader 時。 此外，由於 Datareader 是相當簡單的資料集物件，它們會提供更佳的效能。 如果您進行這項變更，不過，兩者都不可以排序資料的 Web 控制項，也因為 SqlDataSource 無法確定查詢所傳回的多少筆記錄，也不會 DataReader 的頁面會提供任何技術排序傳回的資料。

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>步驟 4： 使用自訂的 SQL 陳述式或預存程序

在設定 SqlDataSource 控制項時，用來傳回資料的查詢可以指定兩種方法之一，做為自訂的 SQL 陳述式或預存程序，或從現有的資料表或檢視的資料行。 在我們所檢查的步驟 2 中選取的資料行`Products`資料表。 可讓 s 查看使用自訂的 SQL 陳述式。

將另一個的 GridView 控制項加入`Querying.aspx`頁面上，然後選擇 建立新的資料來源，從下拉式清單中的智慧標籤。 接下來，表示該資料提取它會從資料庫這將會建立新的 SqlDataSource 控制項。 將控制項`ProductsWithCategoryInfoDataSource`。


![建立名為 ProductsWithCategoryInfoDataSource 新 SqlDataSource 控制項](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**圖 12**： 建立新的 SqlDataSource 控制項，名為 `ProductsWithCategoryInfoDataSource`


下一個畫面會詢問您指定的資料庫。 我們在上一步濆迶 7 選取`NORTHWINDConnectionString`從下拉式清單，然後按一下 [下一步]。 在 設定 Select 陳述式畫面中，自訂 SQL 陳述式或預存程序 選項按鈕，選擇 指定，按一下 下一步。 這會顯示定義自訂陳述式或預存程序螢幕，可提供選取、 更新、 插入和刪除標示為索引標籤。 每個索引標籤中，您可以在文字方塊中輸入自訂的 SQL 陳述式，或從下拉式清單中選擇 預存程序。 在此教學課程中我們將探討輸入自訂的 SQL 陳述式。下一個教學課程包含的範例會使用預存程序。


![輸入自訂的 SQL 陳述式，或請選擇 預存程序](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**圖 13**： 輸入自訂的 SQL 陳述式，或請選擇 預存程序


自訂 SQL 陳述式可以手動輸入的文字方塊中，或可依序按一下 [查詢產生器] 按鈕以圖形方式建構。 從 查詢產生器或文字方塊中，使用下列查詢來傳回`ProductID`和`ProductName`欄位從`Products`資料表使用`JOIN`擷取產品 s`CategoryName`從`Categories`資料表：


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![您可以以圖形方式建構查詢使用查詢產生器](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**圖 14**： 您可以以圖形方式建構查詢中使用查詢產生器


指定查詢之後, 按一下 [下一步] 繼續進行測試查詢螢幕。 按一下 [完成] 來完成 SqlDataSource 精靈。

完成精靈之後，GridView 必須加入至顯示它的三個 BoundFields `ProductID`， `ProductName`，和`CategoryName`查詢所傳回的與產生的下列宣告式標記中的資料行：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![在 GridView 會顯示每個產品的識別碼，名稱，以及相關聯的類別名稱](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**圖 15**: GridView 會顯示每個產品的識別碼、 名稱和相關聯的類別名稱 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>總結

在本教學課程中，我們看到如何查詢及使用 SqlDataSource 控制項顯示資料。 ObjectDataSource，像是 SqlDataSource 可做為 proxy、 提供宣告式方法來存取資料。 其屬性會指定要連接到資料庫和 SQL`SELECT`執行查詢; 它們可以透過 屬性 視窗或使用 設定資料來源精靈來指定。

`SELECT`我們檢查本教學課程中的查詢範例則會從指定的查詢傳回的所有記錄。 SqlDataSource 控制項，不過，可以包含`WHERE`具有參數的值被指派以程式設計方式或自動取自指定的來源子句。 我們將檢驗如何建立及使用參數化的查詢在下一個教學課程 ！

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取關聯式資料庫資料](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource 控制項概觀](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [SqlDataSource 控制項的 ASP.NET 快速入門教學課程：](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config`<connectionStrings>`項目](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [資料庫連接字串參考](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Susan Connery、 Bernadette Leigh 和 David Suru。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [下一頁](using-parameterized-queries-with-the-sqldatasource-vb.md)
