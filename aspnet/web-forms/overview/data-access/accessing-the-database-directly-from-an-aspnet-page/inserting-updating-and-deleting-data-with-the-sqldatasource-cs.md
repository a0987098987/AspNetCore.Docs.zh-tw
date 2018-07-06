---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: 插入、 更新和刪除資料以 sqldatasource 進行 (C#) |Microsoft Docs
author: rick-anderson
description: 在先前的教學課程中我們了解如何將 ObjectDataSource 控制項允許插入、 更新和刪除的資料。 SqlDataSource 控制項支援 t...
ms.author: aspnetcontent
ms.date: 02/20/2007
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: d2958cb9cb0dd5ed89988a969663022a920a3dca
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812843"
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>插入、 更新和刪除資料以 sqldatasource 進行 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe)或[下載 PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> 在先前的教學課程中我們了解如何將 ObjectDataSource 控制項允許插入、 更新和刪除的資料。 SqlDataSource 控制項支援相同的作業，方法是不同，但本教學課程示範如何設定 SqlDataSource 插入、 更新和刪除資料。


## <a name="introduction"></a>簡介

中所述[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)、 GridView 控制項提供內建的更新和刪除功能，雖然 DetailsView 和 FormView 控制項包含插入支援連同編輯和刪除功能。 這些資料修改功能直接插入資料來源控制項，而不需要撰寫的程式碼行。 [概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)檢查使用 ObjectDataSource 促進插入、 更新和刪除與 GridView、 DetailsView 和 FormView 控制項。 或者，SqlDataSource 可用以取代 ObjectDataSource。

請注意，若要支援插入、 更新和刪除使用 ObjectDataSource 我們需要指定物件層方法，要叫用來執行插入、 更新或刪除動作。 使用 SqlDataSource，我們需要提供`INSERT`， `UPDATE`，和`DELETE`SQL 陳述式 （或預存程序） 來執行。 如我們在本教學課程中所見，這些陳述式可以手動建立，或可以自動產生 SqlDataSource s 設定資料來源精靈。

> [!NOTE]
> 因為我們已討論過插入、 編輯和刪除功能的 GridView、 DetailsView 和 FormView 控制項，此教學課程著重於設定 SqlDataSource 控制項，以支援這些作業。 如果您需要實作 GridView、 DetailsView 和 FormView，回復成編輯、 插入，並刪除資料的教學課程中的這些功能是重溫一下，開頭[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)。


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>步驟 1： 指定`INSERT`，`UPDATE`，和`DELETE`陳述式

因為我們看到在過去兩個教學課程中，從 SqlDataSource 控制項，我們需要擷取資料的已設定兩個屬性：

1. `ConnectionString`指定哪些資料庫傳送到，查詢和
2. `SelectCommand`指定的特定 SQL 陳述式或預存程序傳回結果所執行的名稱。

針對`SelectCommand`參數的值，參數值會指定經由 SqlDataSource 的`SelectParameters`集合並且可包含硬式編碼值，一般參數的來源值 (查詢字串欄位，工作階段變數 Web 控制項的值，和等等），或以程式設計的方式指派。 當 SqlDataSource 控制項 s`Select()`叫用方法以程式設計方式或自動資料 Web 控制項從資料庫連線，參數值指派給查詢中，命令會關閉傳送至資料庫。 然後傳回結果做為資料集或 DataReader，控制項 s 的值而定`DataSourceMode`屬性。

選取資料，以及 SqlDataSource 控制項可用來插入、 更新和刪除資料，藉由提供`INSERT`， `UPDATE`，和`DELETE`SQL 陳述式中相同的方式。 只需指派`InsertCommand`， `UpdateCommand`，並`DeleteCommand`屬性`INSERT`， `UPDATE`，和`DELETE`来執行的 SQL 陳述式。 如果陳述式有參數 （以它們最永遠），將其併入`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

一次`InsertCommand`， `UpdateCommand`，或`DeleteCommand`已經指定的值、 相對應的資料 Web 控制項 s 智慧標籤的 [啟用插入、 啟用編輯，或啟用刪除] 選項會變成可用。 為了說明這點，可讓 s 看從範例`Querying.aspx`我們在中建立的頁面[SqlDataSource 控制項查詢資料](querying-data-with-the-sqldatasource-control-cs.md)教學課程，並加強其包含刪除功能。

首先開啟`InsertUpdateDelete.aspx`並`Querying.aspx`頁從`SqlDataSource`資料夾。 從設計工具上`Querying.aspx`頁面上，選取 從第一個範例中的 SqlDataSource 和 GridView (`ProductsDataSource`和`GridView1`控制項)。 選取兩個控制項之後, 請移至 [編輯] 功能表並選擇 [複製] （或只按 Ctrl + C）。 接下來，移至設計工具的`InsertUpdateDelete.aspx`並貼上的控制項中。 您已經移動兩個控制項，若要之後`InsertUpdateDelete.aspx`，測試的瀏覽器頁面。 您應該會看到的值`ProductID`， `ProductName`，並`UnitPrice`資料行中的記錄之所有`Products`資料庫資料表。


[![所有的產品未列出，請依 ProductID](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**圖 1**： 所有的產品未列出，請按照`ProductID`([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>新增 SqlDataSource s`DeleteCommand`和`DeleteParameters`屬性

目前我們只會傳回所有記錄的 SqlDataSource`Products`資料表和 GridView 呈現這項資料。 我們的目標是要擴充此範例，以允許使用者刪除透過 GridView 的產品。 若要這麼做我們需要指定 SqlDataSource 控制項 s 的值`DeleteCommand`和`DeleteParameters`屬性，然後設定 GridView，以支援刪除。

`DeleteCommand`和`DeleteParameters`數種方式可以指定屬性：

- 透過宣告式語法
- 從設計工具中的 [屬性] 視窗
- 從指定自訂的 SQL 陳述式或預存程序在設定資料來源精靈畫面
- 透過指定資料行，資料表中的設定資料來源精靈 中的 檢視 畫面中 進階 按鈕，這會實際自動產生`DELETE`中使用的 SQL 陳述式和參數集合`DeleteCommand`和`DeleteParameters`屬性

我們將檢驗如何自動`DELETE`步驟 2 中建立的陳述式。 現在先讓 s 使用 [屬性] 視窗，在設計師中，雖然 「 設定資料來源精靈 」 或宣告式語法選項一樣運作。

在設計工具`InsertUpdateDelete.aspx`，按一下`ProductsDataSource`SqlDataSource 然後開啟 [屬性] 視窗 （從 [檢視] 功能表中，選擇 [屬性] 視窗中，或只要點按 F4）。 選取 DeleteQuery 屬性，這會顯示一組的省略符號。


![從 [屬性] 視窗中選取 DeleteQuery 屬性](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**圖 2**： 從 屬性 視窗中選取 DeleteQuery 屬性


> [!NOTE]
> SqlDataSource 不 t 具有 DeleteQuery 屬性。 DeleteQuery 反而是組成`DeleteCommand`和`DeleteParameters`屬性和時才會列出在 [屬性] 視窗中檢視透過設計工具視窗。 如果您正在查看來源檢視中的 [屬性] 視窗，您會發現`DeleteCommand`屬性改為。


按一下省略符號，以顯示命令及參數編輯器對話方塊的 DeleteQuery 屬性方塊 （請參閱 圖 3）。 您可以從這個對話方塊中指定`DELETE`SQL 陳述式指定的參數。 輸入下列查詢`DELETE`命令 文字方塊 (可能是以手動方式或使用查詢產生器中，如果您偏好):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

接下來，按一下 [重新整理參數] 按鈕以新增`@ProductID`參數，以下列參數的清單。


[![從 [屬性] 視窗中選取 DeleteQuery 屬性](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**圖 3**： 從 [屬性] 視窗中選取 DeleteQuery 屬性 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


請勿*不*提供此參數 （將保留在無來源的參數） 的值。 一旦我們將刪除支援新增至 GridView，GridView 會自動提供這個參數值，使用的值及其`DataKeys`按下的 [刪除] 按鈕的資料列的集合。

> [!NOTE]
> 中使用的參數名稱`DELETE`查詢*必須*的名稱與相同`DataKeyNames`GridView、 DetailsView 或 FormView 中的值。 也就是中的參數`DELETE`稱為特意不到陳述式`@ProductID`(而不是，說`@ID`)，因為 [產品] 資料表 （以及在 gridview 裡的 DataKeyNames 值） 中的主索引鍵資料行名稱是`ProductID`。


如果參數名稱和`DataKeyNames`值不 t 相符項目，GridView 無法自動將參數值從`DataKeys`集合。

於 命令及參數編輯器對話方塊中輸入的刪除相關的資訊之後按一下 確定 並移至 來源 檢視來檢查產生的宣告式標記：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

請注意新增`DeleteCommand`屬性，以及`<DeleteParameters>`一節和名為的參數物件`productID`。

## <a name="configuring-the-gridview-for-deleting"></a>設定刪除的 GridView

使用`DeleteCommand`新增屬性，現在包含 GridView s 智慧標籤的 [啟用刪除] 選項。 請繼續進行，並選取此核取方塊。 中所述[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)，這會導致將與 CommandField GridView 其`ShowDeleteButton`屬性設定為`true`。 如圖 4 所示，當透過瀏覽器瀏覽網頁時就會包含 刪除 按鈕。 測試出這個頁面刪除某些產品。


[![每個 GridView 資料列現在包含 [刪除] 按鈕](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**圖 4**： 現在包含每個 GridView 資料列的 [刪除] 按鈕 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


按一下 [刪除] 按鈕，回傳，就會發生指派 GridView`ProductID`參數的值的`DataKeys`集合值的資料列的 [刪除] 按鈕已按下，並叫用 SqlDataSource 的`Delete()`方法。 SqlDataSource 控制項連接至資料庫，然後執行`DELETE`陳述式。 GridView 然後重新繫結與 sqldatasource 取回和顯示目前的產品集 （其中不會再包含只是刪除記錄）。

> [!NOTE]
> 因為 GridView 會使用其`DataKeys`集合來填入 SqlDataSource 參數，它不可或缺的 s，GridView s`DataKeyNames`屬性設定為資料行構成的主索引鍵，SqlDataSource 的`SelectCommand`傳回這些資料行。 此外，它重要的參數名稱中之 SqlDataSource`DeleteCommand`設為`@ProductID`。 如果`DataKeyNames`未設定屬性或參數名稱不是`@ProductsID`，按一下 [刪除] 按鈕將會導致回傳，但是獲利的 t 實際上會刪除任何記錄。


圖 5 以圖形方式描繪此類互動。 回頭[檢查與插入、 更新和刪除事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)的插入、 更新及刪除資料 Web 控制項相關聯的事件鏈結的詳細說明的教學課程。


![按一下 [刪除] 按鈕，在 gridview 裡會叫用 SqlDataSource s delete （） 方法](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**圖 5**： 按一下 GridView 裡的 刪除 按鈕會叫用 SqlDataSource 的`Delete()`方法


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>步驟 2： 自動產生`INSERT`，`UPDATE`，和`DELETE`陳述式

為步驟 1， `INSERT`， `UPDATE`，和`DELETE`可以透過 [屬性] 視窗或控制項 s 宣告式語法指定 SQL 陳述式。 不過，此方法需要，我們以手動方式寫出 SQL 陳述式以手動的方式，可以很單調且容易發生錯誤。 幸運的是，「 設定資料來源精靈 」 會提供選項，可讓`INSERT`， `UPDATE`，和`DELETE`使用資料表中的 [檢視] 畫面上的指定資料行時，會自動產生的陳述式。

可讓瀏覽這個自動產生選項的 s。 中的設計工具中加入 DetailsView`InsertUpdateDelete.aspx`並將其`ID`屬性設`ManageProducts`。 接下來，從 DetailsView s 智慧標籤，選擇 建立新的資料來源並建立名為 SqlDataSource `ManageProductsDataSource`。


[![建立名為 ManageProductsDataSource 新 SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**圖 6**： 建立新的 SqlDataSource 具名`ManageProductsDataSource`([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


從 [設定資料來源精靈] 中，選擇使用`NORTHWINDConnectionString`連接字串，並按一下 [下一步]。 從 [設定 Select 陳述式] 畫面中，將指定的資料行從選取的資料表或檢視表選項按鈕，並挑選`Products`從下拉式清單中的資料表。 選取  `ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`核取方塊清單中的資料行。


[![使用 [產品] 資料表，傳回 ProductID、 ProductName、 UnitPrice 和已停止的資料行](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**圖 7**： 使用`Products`資料表中，傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


若要自動產生`INSERT`， `UPDATE`，並`DELETE`陳述式，根據選取的資料表和資料行，按一下 [進階] 按鈕，並檢查產生`INSERT`， `UPDATE`，和`DELETE`陳述式的核取方塊。


![檢查產生 INSERT、 UPDATE 和 DELETE 陳述式 核取方塊](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**[圖 8**： 檢查產生`INSERT`， `UPDATE`，和`DELETE`陳述式] 核取方塊


產生`INSERT`， `UPDATE`，和`DELETE`只能可以核取陳述式的核取方塊，如果選取的資料表有主索引鍵和主索引鍵資料行 （或資料行） 都包含在傳回的資料行清單。 使用開放式並行存取核取方塊，這會成為可選取一次產生`INSERT`， `UPDATE`，並`DELETE`陳述式的核取方塊已核取，將會擴大`WHERE`子句中產生`UPDATE`和`DELETE`陳述式，以提供開放式並行存取控制。 現在，請核取此核取方塊;在下一個教學課程中，我們將檢驗使用 SqlDataSource 控制項的開放式並行存取。

檢查產生後`INSERT`， `UPDATE`，和`DELETE`陳述式的核取方塊，按一下 確定 以返回 設定 Select 陳述式 畫面中，然後按一下 下一步，最後再，以完成設定資料來源精靈。 在完成精靈，Visual Studio 會加入 BoundFields 的 DetailsView `ProductID`， `ProductName`，並`UnitPrice`資料行及其如`Discontinued`資料行。 從 DetailsView s 智慧標籤，請勾選 啟用分頁的選項，以便使用者瀏覽此頁面可以逐步執行產品。 也會清除 DetailsView s`Width`和`Height`屬性。

請注意智慧標籤已啟用插入、 啟用編輯和啟用刪除可用的選項。 這是因為 SqlDataSource 包含的值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`，如下列宣告式語法所示：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

請注意如何 SqlDataSource 控制項已自動設定的值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 中所參考的資料行集中`InsertCommand`並`UpdateCommand`屬性根據在`SELECT`陳述式。 亦即，而不會*每個*產品中的資料行`InsertCommand`並`UpdateCommand`中, 指定這些資料行`SelectCommand`(較少`ProductID`，這省略，因為它 s [`IDENTITY`資料行](http://www.sqlteam.com/item.asp?ItemID=102)編輯時，就無法變更其值，這會自動指派插入時)。 此外，每個參數中`InsertCommand`， `UpdateCommand`，並`DeleteCommand`屬性中的對應參數`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

若要開啟 DetailsView 的資料修改功能，請檢查啟用插入、 啟用編輯和它的智慧標籤中的 啟用刪除選項。 這會將新增使用 CommandField 及其`ShowInsertButton`， `ShowEditButton`，並`ShowDeleteButton`屬性設定為`true`。

瀏覽的頁面在瀏覽器，並注意編輯、 刪除和 DetailsView 中包含的新按鈕。 按一下 [編輯] 按鈕變成編輯模式，它會顯示每個 BoundField DetailsView 其`ReadOnly`屬性設定為`false`（預設值） 做為文字方塊中，及其為核取方塊。


[![在 DetailsView s 預設編輯介面](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**圖 9**: DetailsView s 預設編輯介面 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


同樣地，您可以刪除目前選取的產品，或將新的產品加入至系統。 由於`InsertCommand`陳述式只能搭配`ProductName`， `UnitPrice`，以及`Discontinued`資料行，其他資料行具有其中一個`NULL`或其資料庫在插入時所指派的預設值。 就像使用 ObjectDataSource，如果`InsertCommand`遺漏資料行不允許任何資料庫資料表`NULL`s 和不要有預設值，就會發生 SQL 錯誤時嘗試執行`INSERT`陳述式。

> [!NOTE]
> DetailsView s 中的插入和編輯介面沒有任何類型的自訂或驗證。 若要將驗證控制項加入或自訂介面，您需要將 BoundFields TemplateFields。 請參閱[將驗證控制項加入編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)並[自訂資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程，如需詳細資訊。


此外，請記住，更新和刪除，DetailsView 會使用目前的產品 s`DataKey`值時才存在`DataKeyNames`屬性設定。 如果編輯或刪除似乎沒有作用，請確定`DataKeyNames`屬性設定。

## <a name="limitations-of-automatically-generating-sql-statements"></a>自動產生 SQL 陳述式的限制

產生自`INSERT`， `UPDATE`，並`DELETE`挑選資料表中的資料行的較複雜查詢，您就必須自行撰寫時，才可用陳述式選項`INSERT`， `UPDATE`，和`DELETE`像我們在步驟 1 中的陳述式。 情況下，SQL`SELECT`陳述式會使用`JOIN`要回復的一或多個查閱資料表中的資料顯示用途 (例如帶回`Categories`表格的`CategoryName`欄位時顯示的產品資訊)。 在此同時，我們可能會想要允許使用者編輯、 更新或將資料插入核心資料表 (`Products`，在此情況下)。

雖然`INSERT`， `UPDATE`，和`DELETE`陳述式可以手動輸入，請考慮下列節省時間的提示。 一開始設定 SqlDataSource，進而讓取回資料只從`Products`資料表。 使用設定資料來源精靈的指定資料行，從資料表或檢視螢幕，讓您可以自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式。 然後完成精靈之後，選擇 設定從 屬性 視窗 SelectQuery （或者，或者，移至 設定資料來源精靈，但使用 指定自訂的 SQL 陳述式或預存程序選項的 上一步）。 然後更新`SELECT`陳述式包含`JOIN`語法。 這項技術提供自動產生 SQL 陳述式，節省時間的優點，而且允許更多自訂`SELECT`陳述式。

自動產生的另一項限制`INSERT`， `UPDATE`，並`DELETE`陳述式是中的資料行`INSERT`並`UPDATE`陳述式會根據所傳回的資料行`SELECT`陳述式。 我們可能需要更新或插入更多或較少的欄位，不過。 例如，在步驟 2 中範例中，或許我們想要有`UnitPrice`BoundField 處於唯讀模式。 在此情況下，它不成問題 t 會出現在`UpdateCommand`。 或者，我們可能想要設定 GridView 中不會出現 [資料表] 欄位的值。 例如，當加入新記錄我們可能會想`QuantityPerUnit`值設定為 待辦事項。

如果這類自訂需要，您需要以手動的方式，讓它們，透過 [屬性] 視窗中，指定自訂的 SQL 陳述式或在精靈中，或透過宣告式語法的預存程序選項。

> [!NOTE]
> 當加入的參數沒有對應的欄位，資料中的 Web 控制項時，請記住，必須以某種方式的值指派給這些參數值。 這些值可以是： 硬式編碼中直接`InsertCommand`或`UpdateCommand`; 可來自於某些預先定義的來源 （查詢字串、 工作階段狀態、 Web 控制項頁面上，等等）; 或如我們在先前的教學課程中所見可以以程式設計的方式指派。


## <a name="summary"></a>總結

為了讓資料 Web 控制項，以利用其內建的插入、 編輯和刪除功能，它們會繫結至資料來源控制項必須提供這類功能。 這表示，如 SqlDataSource `INSERT`， `UPDATE`，和`DELETE`SQL 陳述式必須指派給`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 這些屬性和對應的參數集合，則可以是以手動方式加入或透過設定資料來源精靈會自動產生。 在本教學課程中，我們檢查這兩種技巧。

使用 ObjectDataSource 中使用開放式並行存取檢查[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教學課程。 SqlDataSource 控制項也支援開放式並行存取。 自動產生時，在步驟 2 中所示`INSERT`， `UPDATE`，和`DELETE`陳述式，精靈會提供使用開放式並行存取選項。 我們將在下一個教學課程中所看到的會使用以 sqldatasource 進行的開放式並行存取修改`WHERE`中的子句`UPDATE`和`DELETE`陳述式，以確保其他資料行的值尚未變更自上次資料的 t在頁面上顯示。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](using-parameterized-queries-with-the-sqldatasource-cs.md)
> [下一頁](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
