---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
title: 使用 SqlDataSource 控制項 (VB) 來查詢資料 |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中，我們會使用 ObjectDataSource 控制項將完全分開的資料存取層中的展示層。 開始此的老師...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: b12f752d-3502-40a4-b695-fc7b7d08cfd3
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/querying-data-with-the-sqldatasource-control-vb
msc.type: authoredcontent
ms.openlocfilehash: d7ec182325d609877ac0d3603a5124b5fbe5a229
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37365707"
---
<a name="querying-data-with-the-sqldatasource-control-vb"></a>使用 SqlDataSource 控制項 (VB) 來查詢資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_47_VB.exe)或[下載 PDF](querying-data-with-the-sqldatasource-control-vb/_static/datatutorial47vb1.pdf)

> 在先前的教學課程中，我們會使用 ObjectDataSource 控制項將完全分開的資料存取層中的展示層。 我們開始本教學課程，了解，SqlDataSource 控制項如何用於簡單的應用程式不需要這類嚴格的分隔的簡報和資料存取。


## <a name="introduction"></a>簡介

所有教學課程，我們檢查到目前為止已使用簡報、 商務邏輯和資料存取層級所組成的階層式的架構。 在第一個教學課程中所製作的資料存取層 (DAL) ([建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)) 和商務邏輯層，在第二個 ([建立商業邏輯層](../introduction/creating-a-business-logic-layer-vb.md))。 開頭[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程中，我們了解如何使用 ASP.NET 2.0 s 新 ObjectDataSource 控制項以宣告方式與從展示層架構的介面。

所有教學課程到目前為止已使用架構來處理資料，而它也可存取、 插入、 更新和刪除資料庫的資料直接從 ASP.NET 頁面，並略過架構。 這樣會將特定的資料庫查詢和商務邏輯放直接在網頁中。 夠大型或複雜的應用程式，設計、 實作和使用階層式的架構是非常重要的成功、 可更新性和可維護性應用程式。 開發強固的架構，不過，可能就不需要建立非常簡單的一次性應用程式時。

ASP.NET 2.0 提供五個內建的資料來源控制項[SqlDataSource](https://msdn.microsoft.com/library/dz12d98w%28vs.80%29.aspx)， [AccessDataSource](https://msdn.microsoft.com/library/8e5545e1.aspx)， [ObjectDataSource](https://msdn.microsoft.com/library/9a4kyhcx.aspx)， [XmlDataSource](https://msdn.microsoft.com/library/e8d8587a%28en-US,VS.80%29.aspx)，並[SiteMapDataSource](https://msdn.microsoft.com/library/5ex9t96x%28en-US,VS.80%29.aspx)。 SqlDataSource 可用來存取和修改資料直接從關聯式資料庫，包括 Microsoft SQL Server、 Microsoft Access、 Oracle、 MySQL 和其他項目。 在本教學課程 和 下一步 的第三，我們將檢驗如何使用 SqlDataSource 控制項時，探索如何查詢和篩選資料庫資料，以及如何使用以 SqlDataSource 插入、 更新和刪除資料。


![ASP.NET 2.0 包含五個內建的資料來源控制項](querying-data-with-the-sqldatasource-control-vb/_static/image1.gif)

**圖 1**: ASP.NET 2.0 包含五個內建的資料來源控制項


## <a name="comparing-the-objectdatasource-and-sqldatasource"></a>比較 ObjectDataSource 和 SqlDataSource

就概念而言，ObjectDataSource 和 SqlDataSource 控制項都只是資料的 proxy。 中所述[顯示的資料與 ObjectDataSource](../basic-reporting/displaying-data-with-the-objectdatasource-vb.md)教學課程中，ObjectDataSource 的屬性會指出物件類型，提供資料和方法來叫用來選取、 插入、 更新和刪除資料從基礎的物件型別。 資料 Web 控制項，例如 GridView、 DetailsView 或 DataList ObjectDataSource 的屬性已設定之後，可以繫結至控制項，使用 ObjectDataSource s `Select()`， `Insert()`， `Delete()`，和`Update()`方法如果需要互動的基礎架構。

SqlDataSource 提供相同的功能，但運作對關聯式資料庫，而不是物件程式庫。 使用 SqlDataSource，我們將必須指定資料庫連接字串和特定 SQL 查詢或預存程序，執行插入、 更新、 刪除和擷取資料。 SqlDataSource s `Select()`， `Insert()`， `Update()`，和`Delete()`方法，叫用時，連接到指定的資料庫，並發出適當的 SQL 查詢。 下圖所示，這些方法進行的連接至資料庫、 發出查詢，並傳回結果。


![SqlDataSource 做為資料庫的 Proxy](querying-data-with-the-sqldatasource-control-vb/_static/image2.gif)

**圖 2**: SqlDataSource 做為資料庫的 Proxy


> [!NOTE]
> 在本教學課程中我們將焦點放在從資料庫擷取資料。 在 [插入、 更新和正在刪除資料，使用 SqlDataSource 控制項](inserting-updating-and-deleting-data-with-the-sqldatasource-vb.md)教學課程中，我們會看到如何設定支援插入、 更新和刪除 SqlDataSource。


## <a name="the-sqldatasource-and-accessdatasource-controls"></a>SqlDataSource 和 AccessDataSource 控制項

除了 SqlDataSource 控制項，ASP.NET 2.0 也包含 AccessDataSource 控制項。 這兩個不同控制項會導致許多開發人員為有疑問 AccessDataSource 控制項是以獨佔方式使用 Microsoft Access 使用 SqlDataSource 控制項以獨佔方式使用 Microsoft SQL Server 設計的 ASP.NET 2.0 新。 雖然 AccessDataSource 設計來搭配 Microsoft Access 中，使用適用於 SqlDataSource 控制項*任何*關聯式資料庫，您可以透過.NET。 這包括任何 OleDb 或 ODBC 相容資料存放區，例如 Microsoft SQL Server、 Microsoft Access、 Oracle、 Informix、 MySQL 和 PostgreSQL，還有其他更多。

AccessDataSource 和 SqlDataSource 控制項之間的唯一差異是指定的資料庫連線資訊的方式。 AccessDataSource 控制項必須只 Access 資料庫檔案的檔案路徑。 SqlDataSource，相反地，需要完整的連接字串。

## <a name="step-1-creating-the-sqldatasource-web-pages"></a>步驟 1： 建立 SqlDataSource 網頁

我們開始探索如何直接與資料庫資料使用 SqlDataSource 控制項之前，讓先花點時間，我們需要針對本教學課程和下列三個我們網站專案中建立 ASP.NET 網頁的 s。 藉由新增新的資料夾，名為啟動`SqlDataSource`。 接下來，新增到該資料夾，並確認其關聯與每個頁面的 下列 ASP.NET 網頁`Site.master`主版頁面：

- `Default.aspx`
- `Querying.aspx`
- `ParameterizedQueries.aspx`
- `InsertUpdateDelete.aspx`
- `OptimisticConcurrency.aspx`


![加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程](querying-data-with-the-sqldatasource-control-vb/_static/image3.gif)

**圖 3**： 加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程


在其他資料夾，例如`Default.aspx`在`SqlDataSource`資料夾會列出其一節中的教學課程。 請記得，`SectionLevelTutorialListing.ascx`使用者控制項提供這項功能。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](querying-data-with-the-sqldatasource-control-vb/_static/image5.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image4.gif)

**圖 4**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image6.gif))


最後，將這些四個頁面新增項目為`Web.sitemap`檔案。 具體來說，加入下列標記之後新增自訂按鈕 DataList 與重複項`<siteMapNode>`:


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample1.sql)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含編輯、 插入及刪除教學課程的項目。


![網站導覽現在包含項目，如 SqlDataSource 教學課程](querying-data-with-the-sqldatasource-control-vb/_static/image7.gif)

**圖 5**： 網站地圖現在包含項目，如 SqlDataSource 教學課程


## <a name="step-2-adding-and-configuring-the-sqldatasource-control"></a>步驟 2： 加入和設定 SqlDataSource 控制項

首先開啟`Querying.aspx`頁面中`SqlDataSource`資料夾，然後切換到設計檢視。 將 SqlDataSource 控制項從工具箱拖曳至設計工具和設定其`ID`至`ProductsDataSource`。 如同 ObjectDataSource、 SqlDataSource 不會產生任何呈現的輸出，並因此會顯示為設計介面上的灰色方塊。 若要設定 SqlDataSource，按一下 從 SqlDataSource s 智慧標籤的 設定資料來源 連結。


![按一下 設定從 SqlDataSource s 智慧標籤的資料來源連結](querying-data-with-the-sqldatasource-control-vb/_static/image8.gif)

**圖 6**： 按一下 設定從 SqlDataSource s 智慧標籤的資料來源連結


這會顯示 [SqlDataSource 控制項 s 設定資料來源精靈]。 雖然精靈 s 中的步驟有不同從 ObjectDataSource 控制項 s，但最終目的是相同，以提供有關如何擷取、 插入、 更新和刪除資料來源中的資料的詳細資料。 如 SqlDataSource 這需要指定要使用的基礎資料庫，並提供特定 SQL 陳述式或預存程序。

第一個步驟中，精靈會提示我們輸入的資料庫。 下拉式清單包含在 s 中的 web 應用程式中找到這些資料庫`App_Data`資料夾以及已加入至伺服器總管 中的 資料連線 節點。 因為我們已已經新增的連接字串`NORTHWIND.MDF`資料庫中`App_Data`s 專案的資料夾`Web.config`檔案，下拉式清單包含該連接字串中，參考`NORTHWINDConnectionString`。 從下拉式清單中選擇此項目，然後按一下 [下一步]。


![從下拉式清單中選擇 NORTHWINDConnectionString](querying-data-with-the-sqldatasource-control-vb/_static/image9.gif)

**圖 7**： 選擇`NORTHWINDConnectionString`從下拉式清單


選擇之後的資料庫，精靈會要求查詢傳回的資料。 我們可以指定資料表或檢視，以傳回，或可以輸入自訂的 SQL 陳述式或指定預存程序的資料行。 您可以切換到 指定此選項，自訂的 SQL 陳述式或預存程序和資料表中的指定資料行，或檢視的選項按鈕。

> [!NOTE]
> 這個範例來說，可讓使用資料表或檢視表選項指定的資料行的 s。 我們將會回到精靈，稍後在本教學課程，並探索指定的自訂 SQL 陳述式或預存程序選項。


圖 8 顯示 設定 Select 陳述式螢幕時指定的資料行從資料表或檢視表選項按鈕已選取。 下拉式清單包含資料表和檢視表，在 Northwind 資料庫中，使用選取的資料表或檢視表 s 資料行下方的核取方塊清單中顯示的集合。 對於此範例中，可讓 s 會傳回`ProductID`， `ProductName`，並`UnitPrice`中的資料行`Products`資料表。 如 [圖 8 所示，讓這些選取項目精靈] 會顯示產生的 SQL 陳述式之後`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`。


![從 Products 資料表傳回資料](querying-data-with-the-sqldatasource-control-vb/_static/image10.gif)

**圖 8**： 傳回資料來源`Products`資料表


一旦您已設定要傳回精靈`ProductID`， `ProductName`，並`UnitPrice`中的資料行`Products`資料表中，按一下 [下一步] 按鈕。 此最後一個畫面會提供要檢查的上一個步驟所設定的查詢結果的機會。 按一下 [測試查詢] 按鈕執行設定`SELECT`陳述式，並顯示在方格中的結果。


![按一下 [測試查詢] 按鈕，檢閱您選取的查詢](querying-data-with-the-sqldatasource-control-vb/_static/image11.gif)

**圖 9**： 按一下 測試查詢 按鈕，檢閱您`SELECT`查詢


若要完成精靈，按一下 [完成]。

使用 ObjectDataSource、 SqlDataSource 的精靈只是將值指派給控制項 s 的屬性，也就是像[ `ConnectionString` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.connectionstring.aspx)並[ `SelectCommand` ](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.selectcommand.aspx)屬性。 完成精靈之後，您 SqlDataSource 控制項 s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample2.aspx)]

`ConnectionString`屬性會提供有關如何連接到資料庫的資訊。 此屬性指派或可以是完整的硬式編碼的連接字串值中的連接字串可以指向`Web.config`。 若要參考的 Web.config 中的連接字串值，請使用語法`<%$ expressionPrefix:expressionValue %>`。 通常*expressionPrefix*是 ConnectionStrings 並*expressionValue*中的連接字串的名稱`Web.config` [ `<connectionStrings>`區段](https://msdn.microsoft.com/library/bf7sd233.aspx)。 不過，可以使用語法來參考`<appSettings>`項目或從資源檔的內容。 請參閱[ASP.NET 運算式概觀](https://msdn.microsoft.com/library/d5bd1tad.aspx)如需此語法的詳細資訊。

`SelectCommand`屬性會指定特定 SQL 陳述式或預存程序會一直執行到傳回的資料。

## <a name="step-3-adding-a-data-web-control-and-binding-it-to-the-sqldatasource"></a>步驟 3： 加入資料 Web 控制項及繫結至 SqlDataSource

設定 SqlDataSource 之後, 可以繫結至資料 Web 控制項，例如 GridView 或 DetailsView。 本教學課程中，可讓 s GridView 中顯示的資料。 從 工具箱 拖曳到頁面的 GridView，並繫結至`ProductsDataSource`SqlDataSource 資料來源清單中選擇下拉式清單中的 GridView s 智慧標籤。


[![新增 GridView，並將它繫結至 SqlDataSource 控制項](querying-data-with-the-sqldatasource-control-vb/_static/image13.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image12.gif)

**圖 10**： 新增 GridView，並將它繫結至 SqlDataSource 控制項 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image14.gif))


一旦您已選取 SqlDataSource 控制項，從下拉式清單中的 GridView s 智慧標籤，Visual Studio 會自動加入 BoundField 或 CheckBoxField 至 GridView 針對每個資料來源控制項所傳回的資料行。 由於 SqlDataSource 會傳回三個資料庫資料行`ProductID`， `ProductName`，和`UnitPrice`GridView 裡有三個欄位。

請花一點時間設定 GridView 三 BoundFields。 變更`ProductName`欄位 s`HeaderText`產品名稱的屬性和`UnitPrice`價格欄位 s。 也會格式化`UnitPrice`欄位做為貨幣。 進行這些修改之後, 您 GridView s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample3.aspx)]

請瀏覽此頁面，透過瀏覽器。 GridView 如 [圖 11] 所示，列出每項產品 s `ProductID`， `ProductName`，和`UnitPrice`值。


[![GridView 會顯示每個產品的 ProductID、 ProductName、 以及 UnitPrice 值](querying-data-with-the-sqldatasource-control-vb/_static/image16.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image15.gif)

**圖 11**: GridView 會顯示每個產品 s `ProductID`， `ProductName`，以及`UnitPrice`的值 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image17.gif))


當瀏覽的頁面 GridView 會叫用其資料來源控制項的`Select()`方法。 當我們使用 ObjectDataSource 控制項時，這叫做`ProductsBLL`類別的`GetProducts()`方法。 使用 SqlDataSource，不過，`Select()`方法會連線到指定的資料庫和問題`SelectCommand`(`SELECT [ProductID], [ProductName], [UnitPrice] FROM [Products]`，在此範例中)。 SqlDataSource 傳回它 GridView 然後列舉，傳回每個資料庫記錄的 GridView 裡建立一個資料列的結果。

## <a name="the-built-in-data-web-control-features-and-the-sqldatasource-control"></a>內建資料 Web 控制項功能和 SqlDataSource 控制項

一般而言，資料 Web 控制項分頁、 排序、 編輯、 固有的功能刪除、 插入及等等特有資料 Web 控制項，並不依賴使用資料來源控制項。 也就是 GridView 可以利用內建的分頁、 排序、 編輯和刪除是否 ObjectDataSource 或 SqlDataSource 繫結。 不過，某些資料 Web 控制項的功能是敏感性資料來源控制項所使用或資料來源控制項的組態。

例如，在[有效率地透過大型數量的資料分頁](../paging-and-sorting/efficiently-paging-through-large-amounts-of-data-vb.md)我們討論過，根據預設，分頁邏輯資料 Web 控制項如何這個傳回的教學課程*所有*記錄的基礎資料來源，並接著會顯示指定目前的頁面索引和每頁顯示的記錄數目的記錄中的適當子集合。 逐頁查看夠大的結果集時，此模型的效率。 幸運的是，ObjectDataSource 可以設定為支援自訂分頁時，它會傳回只顯示記錄的精確子集。 SqlDataSource 控制項，不過，不會實作自訂分頁的屬性。

使用 SqlDataSource，就會發生另一個微妙的地方使用分頁和排序。 根據預設，可以是分頁或排序 GridView 透過從 SqlDataSource 傳回的資料。 若要示範這種情況，請檢查在 GridView 的 s 智慧標籤中啟用分頁，並啟用排序選項`Querying.aspx`並確認其運作如預期般運作。

排序和分頁作用，因為 SqlDataSource 至鬆散型別資料集擷取的資料庫資料。 從資料集，可以確定查詢所傳回的一個關鍵要素，以實作分頁的記錄總數。 此外，資料集的結果可以透過 DataView 進行排序。 這些功能會自動使用 SqlDataSource GridView 要求分頁或排序資料時。

可以設定 SqlDataSource 傳回 DataReader 而不是資料集，藉由變更其[`DataSourceMode`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.sqldatasource.datasourcemode.aspx)從`DataSet`（預設值） 到`DataReader`。 使用 DataReader 可能會偏好的情況下將 SqlDataSource 的結果傳遞至預期 DataReader 的現有程式碼時。 此外，由於 DataReaders 是相當簡單的物件與資料集，其可提供較佳的效能。 如果您進行這項變更，不過，都可以排序資料 Web 控制項，也因為 SqlDataSource 無法確定查詢中，傳回多少筆記錄，也不會 DataReader 的頁面會提供任何技術來排序傳回的資料。

## <a name="step-4-using-a-custom-sql-statement-or-stored-procedure"></a>步驟 4： 使用自訂 SQL 陳述式或預存程序

當設定 SqlDataSource 控制項，用來傳回資料的查詢可以指定兩種方法之一，為自訂的 SQL 陳述式或預存程序，或從現有的資料表或檢視的資料行。 在步驟 2 中，我們檢查選取的資料行`Products`資料表。 可讓 s 探討使用自訂的 SQL 陳述式。

新增另一個 GridView 控制項`Querying.aspx`頁面上，然後選擇 建立新的資料來源，從下拉式清單中的智慧標籤。 接下來，表示該資料就會提取從資料庫這會建立新的 SqlDataSource 控制項。 將控制項`ProductsWithCategoryInfoDataSource`。


![建立新的 SqlDataSource 控制項，名為 ProductsWithCategoryInfoDataSource](querying-data-with-the-sqldatasource-control-vb/_static/image18.gif)

**圖 12**： 建立新的 SqlDataSource 控制項，名為 `ProductsWithCategoryInfoDataSource`


下一個畫面會要求我們指定的資料庫。 我們在上一步圖 7 中選取`NORTHWINDConnectionString`從下拉式清單，然後按一下 下一步。 在 [設定 Select 陳述式] 畫面中，選擇指定自訂 SQL 陳述式或預存程序選項按鈕，按 [下一步]。 這會顯示定義自訂陳述式或預存程序 」 畫面，可提供標示為 SELECT、 UPDATE、 INSERT 和 DELETE 的索引標籤。 在每個索引標籤中，您可以在文字方塊中輸入自訂的 SQL 陳述式或從下拉式清單中選擇 預存程序。 在本教學課程會探討輸入自訂的 SQL 陳述式：下一個教學課程包含使用預存程序的範例。


![輸入自訂的 SQL 陳述式，或選取 預存程序](querying-data-with-the-sqldatasource-control-vb/_static/image19.gif)

**圖 13**： 輸入自訂的 SQL 陳述式，或選取 預存程序


自訂 SQL 陳述式可以是以手動方式在 textbox 輸入，或可以按一下 [查詢產生器] 按鈕以圖形方式建構。 從 查詢產生器或文字方塊中，使用下列查詢來傳回`ProductID`並`ProductName`欄位從`Products`資料表使用`JOIN`擷取產品 s`CategoryName`從`Categories`資料表：


[!code-sql[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample4.sql)]


![您可以以圖形方式建構查詢使用 查詢產生器](querying-data-with-the-sqldatasource-control-vb/_static/image20.gif)

**圖 14**： 您可以以圖形方式建構查詢中使用查詢產生器


指定查詢之後, 按一下 [下一步] 繼續進行測試查詢畫面。 按一下 完成 以完成 SqlDataSource 精靈。

完成精靈之後，GridView 必須加入至顯示它的三個 BoundFields `ProductID`， `ProductName`，和`CategoryName`查詢所傳回的並導致下列的宣告式標記的資料行：


[!code-aspx[Main](querying-data-with-the-sqldatasource-control-vb/samples/sample5.aspx)]


[![GridView 會顯示每個產品的識別碼、 名稱和相關聯的類別名稱](querying-data-with-the-sqldatasource-control-vb/_static/image22.gif)](querying-data-with-the-sqldatasource-control-vb/_static/image21.gif)

**圖 15**: GridView 會顯示每個產品識別碼、 名稱和相關聯的類別名稱 ([按一下以檢視完整大小的影像](querying-data-with-the-sqldatasource-control-vb/_static/image23.gif))


## <a name="summary"></a>總結

在本教學課程中，我們了解如何查詢，並使用 SqlDataSource 控制項顯示資料的內容。 ObjectDataSource，例如 SqlDataSource 可做為 proxy、 提供宣告式的方法，用來存取資料。 其屬性會指定要連接到資料庫和 SQL`SELECT`執行查詢，透過 [屬性] 視窗，或使用 [設定資料來源精靈] 可以指定它們。

`SELECT`在本教學課程中，我們檢查的查詢範例則會從指定的查詢傳回的所有記錄。 SqlDataSource 控制項，不過，可以包含`WHERE`具有參數的值會指派以程式設計方式或自動提取指定之來源子句。 我們將檢驗如何建立，並在下一個教學課程中使用參數化的查詢 ！

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [存取關聯式資料庫資料](http://aspnet.4guysfromrolla.com/articles/022206-1.aspx)
- [SqlDataSource 控制項概觀](https://msdn.microsoft.com/library/dz12d98w.aspx)
- [SqlDataSource 控制項的 ASP.NET 快速入門教學課程：](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/ctrlref/data/sqldatasource.aspx)
- [Web.config`<connectionStrings>`項目](https://msdn.microsoft.com/library/bf7sd233.aspx)
- [資料庫連接字串參考](http://www.connectionstrings.com/)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Susan Connery、 Bernadette Leigh 和 David Suru。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
> [下一頁](using-parameterized-queries-with-the-sqldatasource-vb.md)
