---
uid: web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
title: "插入、 更新和刪除資料與 SqlDataSource (C#) |Microsoft 文件"
author: rick-anderson
description: "在先前的教學課程中我們學到如何 ObjectDataSource 控制項允許的插入、 更新和刪除的資料。 SqlDataSource 控制項支援 t..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2007
ms.topic: article
ms.assetid: a526f0ec-779e-4a2b-a476-6604090d25ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/accessing-the-database-directly-from-an-aspnet-page/inserting-updating-and-deleting-data-with-the-sqldatasource-cs
msc.type: authoredcontent
ms.openlocfilehash: 3b080046df49ff6d4c83063d782962c5fcb33e94
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="inserting-updating-and-deleting-data-with-the-sqldatasource-c"></a>插入、 更新和刪除資料與 SqlDataSource (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載範例應用程式](http://download.microsoft.com/download/4/a/7/4a7a3b18-d80e-4014-8e53-a6a2427f0d93/ASPNET_Data_Tutorial_49_CS.exe)或[下載 PDF](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/datatutorial49cs1.pdf)

> 在先前的教學課程中我們學到如何 ObjectDataSource 控制項允許的插入、 更新和刪除的資料。 SqlDataSource 控制項支援相同的作業，方法是不同，但本教學課程示範如何設定 SqlDataSource 插入、 更新和刪除資料。


## <a name="introduction"></a>簡介

中所述[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)GridView 控制項提供內建的更新和刪除功能的 DetailsView 和 FormView 控制項包含插入時支援連同編輯和刪除功能。 這些資料修改功能直接插入資料來源控制項，而不需要撰寫程式碼行。 [概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)檢查使用 ObjectDataSource 以利於進行插入、 更新和刪除使用 GridView、 DetailsView 和在 FormView 的控制項。 或者，您可以使用 SqlDataSource ObjectDataSource 取代。

請記得，以支援插入、 更新和刪除與 ObjectDataSource 我們需要指定物件層方法，來叫用執行插入、 更新或刪除動作。 使用 SqlDataSource，我們需要提供`INSERT`， `UPDATE`，和`DELETE`SQL 陳述式 （或預存程序） 執行。 我們會看到此教學課程中，可以手動建立這些陳述式，或可以自動產生 SqlDataSource s 設定資料來源精靈。

> [!NOTE]
> 因為我們已討論過插入、 編輯和刪除功能的 GridView 中 DetailsView，並控制 FormView，本教學課程著重於設定成支援這些作業的 SqlDataSource 控制項。 如果您需要複習實作 GridView、 DetailsView 和 FormView，回復成編輯、 插入和刪除資料的教學課程中的這些功能，從[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)。


## <a name="step-1-specifyinginsertupdate-anddeletestatements"></a>步驟 1： 指定`INSERT`，`UPDATE`，和`DELETE`陳述式

當我們從 SqlDataSource 控制項，我們需要擷取資料所示過去兩個教學課程，我們設定兩個屬性：

1. `ConnectionString`指定哪些資料庫要傳送的查詢和
2. `SelectCommand`指定的特定 SQL 陳述式或預存程序傳回結果時所執行的名稱。

如`SelectCommand`具有參數的值、 參數值會指定透過 SqlDataSource 的`SelectParameters`集合並且可包含硬式編碼值，一般參數的來源值 (querystring 欄位、 工作階段變數 Web 控制項的值，並等等），或以程式設計的方式指派。 當 SqlDataSource 控制項 s`Select()`叫用方法以程式設計方式或自動資料 Web 控制項中的資料。 在建立資料庫的連接、 參數值指派給查詢和命令會關閉傳送至資料庫。 然後傳回結果為資料集或 DataReader，控制項 s 的值而定`DataSourceMode`屬性。

選取的資料，以及 SqlDataSource 控制項可用來插入、 更新和刪除資料，藉由提供`INSERT`， `UPDATE`，和`DELETE`SQL 陳述式中相同的方式。 只需指派`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性`INSERT`， `UPDATE`，和`DELETE`来執行的 SQL 陳述式。 如果陳述式具有參數 （如他們最一定會），將其併入`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

一次`InsertCommand`， `UpdateCommand`，或`DeleteCommand`已經指定的值，對應資料 Web 控制項 s 智慧標籤中的啟用插入、 啟用編輯，或啟用刪除選項將會變成可用。 為了說明這點，讓 s 舉例從`Querying.aspx`頁面中建立[查詢資料與 SqlDataSource 控制項](querying-data-with-the-sqldatasource-control-cs.md)教學課程和它包含刪除功能強化。

先開啟`InsertUpdateDelete.aspx`和`Querying.aspx`頁面`SqlDataSource`資料夾。 從設計工具上`Querying.aspx`頁面上，選取第一個範例的 SqlDataSource 和 GridView (`ProductsDataSource`和`GridView1`控制項)。 選取後兩個控制項，移至 [編輯] 功能表並選擇 [複製] （或只按 Ctrl + C）。 接下來，請移至活動設計工具的`InsertUpdateDelete.aspx`並貼上的控制項中。 您已在兩個控制項轉換成之後`InsertUpdateDelete.aspx`，測試出瀏覽器中。 您應該會看到的值`ProductID`， `ProductName`，和`UnitPrice`中記錄的所有資料行`Products`資料庫資料表。


[![所有的產品未列出，請依產品識別碼](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image1.png)

**圖 1**： 所有產品未列出，請依`ProductID`([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.png))


## <a name="adding-the-sqldatasource-sdeletecommandanddeleteparametersproperties"></a>新增 SqlDataSource s`DeleteCommand`和`DeleteParameters`屬性

現在我們有只會傳回所有記錄的 SqlDataSource`Products`資料表並將此資料呈現的 GridView。 我們的目標是要延伸此範例，以允許使用者刪除產品，透過 GridView。 若要完成此我們需要指定值的 SqlDataSource 控制項 s`DeleteCommand`和`DeleteParameters`屬性，然後設定支援刪除 GridView。

`DeleteCommand`和`DeleteParameters`數種方式可以指定的屬性：

- 透過宣告式語法
- 從設計工具中的 [屬性] 視窗
- 從指定自訂的 SQL 陳述式或預存程序在設定資料來源精靈畫面
- 透過指定資料行的資料表中的 [檢視] 畫面中設定資料來源精靈中 [進階] 按鈕，它會實際自動產生`DELETE`中使用的 SQL 陳述式和參數集合`DeleteCommand`和`DeleteParameters`屬性

我們將檢驗如何以自動`DELETE`步驟 2 中建立的陳述式。 現在，讓 s 使用設計工具中，在 [屬性] 視窗，雖然 「 設定資料來源精靈 」 或 「 宣告式語法選項一樣運作。

從設計工具中`InsertUpdateDelete.aspx`，按一下`ProductsDataSource`SqlDataSource 然後開啟 [屬性] 視窗 （從 [檢視] 功能表中，選擇 [屬性] 視窗中，或只要點按 F4）。 選取 DeleteQuery 屬性，將會出現一組的省略符號。


![從 [屬性] 視窗中選取 DeleteQuery 屬性](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image2.gif)

**圖 2**： 從 [屬性] 視窗中選取 DeleteQuery 屬性


> [!NOTE]
> SqlDataSource 規定 t 具有 DeleteQuery 屬性。 相反地，DeleteQuery 是組合`DeleteCommand`和`DeleteParameters`屬性和時才會列出在 [屬性] 視窗中檢視透過設計工具視窗。 如果您正在查看來源檢視中的 [屬性] 視窗，您會看到`DeleteCommand`屬性改為。


按一下省略符號，以顯示命令及參數編輯器對話方塊 DeleteQuery 屬性方塊 （請參閱圖 3）。 您可以從這個對話方塊中指定`DELETE`SQL 陳述式指定的參數。 輸入下列查詢`DELETE`命令文字方塊中 (請以手動方式或使用查詢產生器中，如果您想要的話):

[!code-sql[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample1.sql)]

接下來，按一下 [重新整理參數] 按鈕以加入`@ProductID`參數至以下參數的清單。


[![從 [屬性] 視窗中選取 DeleteQuery 屬性](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image3.png)

**圖 3**： 從 [屬性] 視窗中選取 DeleteQuery 屬性 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.png))


請勿*不*提供這個參數 （將保留在 [無] 來源其參數） 的值。 一旦我們將刪除的支援至 GridView，GridView 會自動提供這個參數值，使用的值及其`DataKeys`其 [刪除] 按鈕已按下的資料列的集合。

> [!NOTE]
> 中使用的參數名稱`DELETE`查詢*必須*的名稱與相同`DataKeyNames`GridView、 DetailsView 或在 FormView 中的值。 也就是中的參數`DELETE`陳述式故意名為`@ProductID`(而不是，例如， `@ID`)，因為 Products 資料表 （以及在 GridView 的 DataKeyNames 值） 中的主索引鍵資料行名稱是`ProductID`。


如果參數名稱和`DataKeyNames`值並不相符，GridView 無法自動將參數值從`DataKeys`集合。

在命令及參數編輯器對話方塊中輸入的刪除相關的資訊之後按一下 [確定]，並移至來源檢視，以檢查產生的宣告式標記：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample2.aspx)]

請注意新增`DeleteCommand`屬性以及`<DeleteParameters>`區段和名為的參數物件`productID`。

## <a name="configuring-the-gridview-for-deleting"></a>設定 刪除的 GridView

與`DeleteCommand`加入屬性，GridView s 智慧標籤現在會包含啟用刪除的選項。 請繼續並選取此核取方塊。 中所述[概觀的插入、 更新和刪除](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)，這會導致將與 CommandField GridView 其`ShowDeleteButton`屬性設定為`true`。 如圖 4 所示，當透過瀏覽器瀏覽頁面時包含 [刪除] 按鈕。 刪除某些產品中，測試出此頁面。


[![每個 GridView 資料列現在包含刪除按鈕](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image4.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.png)

**圖 4**： 現在包含每個 GridView 資料列的 [刪除] 按鈕 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.png))


按一下 [刪除] 按鈕後, 回傳，就會發生 GridView 指派`ProductID`參數值的`DataKeys`集合值的資料列的 [刪除] 按鈕已按下，並叫用 SqlDataSource 的`Delete()`方法。 SqlDataSource 控制項連接至資料庫，然後執行`DELETE`陳述式。 在 GridView 然後重新繫結至 SqlDataSource 取回並顯示目前的產品集 （其中不包含只刪除記錄）。

> [!NOTE]
> 因為 GridView 會使用其`DataKeys`集合來填入 SqlDataSource 參數，它 s 重要的 GridView s`DataKeyNames`屬性設定為的資料行組成的主索引鍵，SqlDataSource 的`SelectCommand`傳回這些資料行。 此外，它很重要的參數名稱在 SqlDataSource s s`DeleteCommand`設`@ProductID`。 如果`DataKeyNames`屬性未設定或參數的名稱不是`@ProductsID`，按一下 [刪除] 按鈕將會導致回傳，但是獲利的 t 實際上會刪除任何記錄。


圖 5 以圖形方式描繪此互動。 回頭參考[檢查插入、 更新和刪除與事件相關聯](../editing-inserting-and-deleting-data/examining-the-events-associated-with-inserting-updating-and-deleting-cs.md)教學課程的插入、 更新和刪除資料的 Web 控制項相關聯的事件鏈結的更詳細討論。


![按一下 [刪除] 按鈕，在 GridView 中的叫用 SqlDataSource s delete （） 方法](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image5.gif)

**圖 5**： 按一下 [刪除] 按鈕，在 GridView 中的叫用 SqlDataSource 的`Delete()`方法


## <a name="step-2-automatically-generating-theinsertupdate-anddeletestatements"></a>步驟 2： 自動產生`INSERT`，`UPDATE`，和`DELETE`陳述式

步驟 1 檢查，為`INSERT`， `UPDATE`，和`DELETE`可以透過 [屬性] 視窗或控制項 s 宣告式語法指定 SQL 陳述式。 不過，這個方法會要求，我們以手動方式寫出的 SQL 陳述式以手動方式，它可以是 monotonous 而且容易產生錯誤。 幸運的是，設定資料來源精靈會提供此選項可讓`INSERT`， `UPDATE`，和`DELETE`時使用的資料表中的 [檢視] 畫面上的指定資料行自動產生的陳述式。

可讓 s 瀏覽這個自動產生選項。 在 DetailsView 加入設計師中`InsertUpdateDelete.aspx`並設定其`ID`屬性`ManageProducts`。 接下來，從 DetailsView s 智慧標籤，選擇 建立新的資料來源並建立名為 SqlDataSource `ManageProductsDataSource`。


[![建立名為 ManageProductsDataSource 新 SqlDataSource](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image6.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.png)

**圖 6**： 建立新的 SqlDataSource 具名`ManageProductsDataSource`([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.png))


在設定資料來源精靈中，選擇使用`NORTHWINDConnectionString`連接字串，然後按一下 [下一步]。 從 設定 Select 陳述式畫面中，將指定的資料行從選取資料表或檢視表選項按鈕，並挑選`Products`下拉式清單中的資料表。 選取`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`核取方塊清單中的資料行。


[![使用 [產品] 資料表，傳回 ProductID、 ProductName、 UnitPrice 和已停止的資料行](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image7.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.png)

**圖 7**： 使用`Products`資料表中，傳回`ProductID`， `ProductName`， `UnitPrice`，和`Discontinued`資料行 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image10.png))


若要自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式，根據選取的資料表和資料行，按一下 [進階] 按鈕，並檢查產生`INSERT`， `UPDATE`，和`DELETE`陳述式的核取方塊。


![檢查產生 INSERT、 UPDATE 和 DELETE 陳述式核取方塊](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image8.gif)

**圖 8**： 請檢查 產生`INSERT`， `UPDATE`，和`DELETE`陳述式核取方塊


產生`INSERT`， `UPDATE`，和`DELETE`只能可以核取陳述式的核取方塊，如果選取的資料表有主索引鍵，且主索引鍵資料行 （或） 資料行未包含在傳回的資料行清單中。 使用開放式並行存取核取方塊，它會變成可選取一次 產生`INSERT`， `UPDATE`，和`DELETE`陳述式的核取方塊已核取，將會增加`WHERE`子句在產生`UPDATE`和`DELETE`陳述式，以提供開放式並行存取控制。 現在，請保留此核取方塊未選取;在下一個教學課程中，我們將檢驗與 SqlDataSource 控制項的開放式並行存取。

檢查 產生後`INSERT`， `UPDATE`，和`DELETE`陳述式的核取方塊，按一下 確定 以回到 設定 Select 陳述式 畫面中，然後按一下 下一步，，然後在完成，以完成設定資料來源精靈。 正在完成精靈，在 Visual Studio 將會加入 BoundFields 的 DetailsView `ProductID`， `ProductName`，和`UnitPrice`資料行與針對 CheckBoxField`Discontinued`資料行。 從 DetailsView s 智慧標籤，檢查啟用分頁 選項，以便使用者瀏覽此頁面可以逐步執行的產品。 在 DetailsView s 也清除`Width`和`Height`屬性。

請注意智慧標籤已啟用插入、 啟用編輯和刪除啟用可用的選項。 這是因為 SqlDataSource 包含的值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`，如下列宣告式語法所示：

[!code-aspx[Main](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/samples/sample3.aspx)]

請注意如何 SqlDataSource 控制項已自動設定的值及其`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 中所參考的資料行集中`InsertCommand`和`UpdateCommand`屬性根據那些在`SELECT`陳述式。 也就是說，而不是讓*每個*產品中的資料行`InsertCommand`和`UpdateCommand`，有中指定這些資料行`SelectCommand`(較少`ProductID`，這省略，因為它 s [`IDENTITY`資料行](http://www.sqlteam.com/item.asp?ItemID=102)編輯時，就無法變更其值，這會自動指派插入時)。 此外，每個參數中`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性中的對應參數`InsertParameters`， `UpdateParameters`，和`DeleteParameters`集合。

若要開啟 DetailsView 的資料修改功能，請檢查啟用插入、 啟用編輯和啟用刪除其智慧標籤選項。 這會將與 CommandField 其`ShowInsertButton`， `ShowEditButton`，和`ShowDeleteButton`屬性設定為`true`。

請在瀏覽器中的瀏覽並記下編輯、 刪除和 DetailsView 中包含的新按鈕。 按一下 [編輯] 按鈕變成編輯模式，它會顯示每個 BoundField DetailsView 其`ReadOnly`屬性設定為`false`（預設值） 做為文字方塊中，並將 CheckBoxField 為核取方塊。


[![在 DetailsView s 預設編輯介面](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image9.gif)](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image11.png)

**圖 9**: DetailsView s 預設編輯介面 ([按一下以檢視完整大小的影像](inserting-updating-and-deleting-data-with-the-sqldatasource-cs/_static/image12.png))


同樣地，您可以刪除目前選取的產品，或將新的產品加入系統。 因為`InsertCommand`陳述式只能搭配`ProductName`， `UnitPrice`，和`Discontinued`資料行，其他資料行具有其中一個`NULL`或其指派於 insert 資料庫的預設值。 就像使用 ObjectDataSource，如果`InsertCommand`遺漏資料行不允許任何資料庫資料表`NULL`s 和不要有預設值，就會發生 SQL 錯誤時嘗試執行`INSERT`陳述式。

> [!NOTE]
> 在 DetailsView s 插入和編輯介面缺少任何類型的自訂或驗證。 若要加入驗證控制項或自訂介面，您需要將 BoundFields 轉換成 TemplateFields。 請參閱[新增驗證控制項的編輯和插入介面](../editing-inserting-and-deleting-data/adding-validation-controls-to-the-editing-and-inserting-interfaces-cs.md)和[自訂的資料修改介面](../editing-inserting-and-deleting-data/customizing-the-data-modification-interface-cs.md)教學課程，如需詳細資訊。


此外，請記住，用於更新和刪除，DetailsView 會使用目前的產品 s`DataKey`值時才存在`DataKeyNames`屬性設定。 如果編輯或刪除似乎沒有作用，請確認`DataKeyNames`屬性設定。

## <a name="limitations-of-automatically-generating-sql-statements"></a>自動產生 SQL 陳述式的限制

產生自`INSERT`， `UPDATE`，和`DELETE`挑選資料行從資料表中，針對較複雜查詢，您就必須自行撰寫時，才可用陳述式選項`INSERT`， `UPDATE`，和`DELETE`像我們在步驟 1 中的陳述式。 常見的是，SQL`SELECT`陳述式使用`JOIN`s 以回復的一或多個查閱資料表中的資料顯示用途 (例如將回`Categories`資料表的`CategoryName`欄位顯示產品資訊時)。 同時，我們可能會想要允許使用者編輯、 更新或插入資料至核心資料表 (`Products`，在此情況下)。

雖然`INSERT`， `UPDATE`，和`DELETE`陳述式可以手動輸入，請考慮下列節省時間的秘訣。 使它回到只會從提取資料一開始安裝 SqlDataSource`Products`資料表。 使用設定資料來源精靈的指定的資料行從資料表或檢視螢幕，以便您可以自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式。 完成精靈之後，然後選擇 設定從 屬性 視窗 SelectQuery （或者，或者，請回到設定資料來源精靈，但使用 指定自訂的 SQL 陳述式或預存程序選項）。 然後更新`SELECT`陳述式包含`JOIN`語法。 這項技術的節省時間的優點，自動產生的 SQL 陳述式，而且允許更多自訂`SELECT`陳述式。

自動產生的另一項限制`INSERT`， `UPDATE`，和`DELETE`陳述式是中的資料行`INSERT`和`UPDATE`陳述式會根據所傳回的資料行`SELECT`陳述式。 我們可能需要更新或插入更多或較少的欄位，不過。 比方說，在範例中步驟 2 中，可能要有`UnitPrice`BoundField 處於唯讀模式。 在此情況下，它應該不 t 會出現在`UpdateCommand`。 或者我們可能會想要在 GridView 中設定不會出現資料表欄位的值。 例如，當加入新資料錄我們可能會想`QuantityPerUnit`值設定為 TODO。

如果需要這類自訂項目，您需要以手動方式，讓它們，透過 [屬性] 視窗中，指定自訂的 SQL 陳述式或預存程序選項，在精靈中，或透過宣告式語法。

> [!NOTE]
> 當沒有資料中的對應欄位將參數加入 Web 控制項時，請記住，需要以某種方式的值指派給這些參數值。 這些值可以是： 硬式編碼中直接`InsertCommand`或`UpdateCommand`; 可來自於 querystring 中，工作階段狀態 （Web 控制項在頁面上，依此類推）; 某些預先定義的來源，或可以指派以程式設計的方式，如前述教學課程中我們了解。


## <a name="summary"></a>總結

為了讓資料可以將其插入的內建、 編輯和刪除功能的 Web 控制項，它們會繫結至資料來源控制項必須提供這類功能。 這表示，如 SqlDataSource `INSERT`， `UPDATE`，和`DELETE`SQL 陳述式都必須指派給`InsertCommand`， `UpdateCommand`，和`DeleteCommand`屬性。 這些屬性和對應的參數集合，可以手動加入或透過設定資料來源精靈 來自動產生。 在本教學課程，我們會檢查這兩種技術。

我們會檢查使用開放式並行存取，以在 ObjectDataSource[實作開放式並行存取](../editing-inserting-and-deleting-data/implementing-optimistic-concurrency-cs.md)教學課程。 SqlDataSource 控制項也支援開放式並行存取。 因為自動產生時，在步驟 2 中記下`INSERT`， `UPDATE`，和`DELETE`陳述式，精靈會提供使用開放式並行存取選項。 我們會看到下一個教學課程中，會使用開放式並行存取與 SqlDataSource 修改`WHERE`子句中的`UPDATE`和`DELETE`陳述式，以確保其他資料行的值不變更自上次資料 t頁面上顯示。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

>[!div class="step-by-step"]
[上一頁](using-parameterized-queries-with-the-sqldatasource-cs.md)
[下一頁](implementing-optimistic-concurrency-with-the-sqldatasource-cs.md)
