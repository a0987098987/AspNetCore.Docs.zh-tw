---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
title: 包裝在交易中 (C#) 內資料庫修改 |Microsoft 文件
author: rick-anderson
description: 本教學課程是四個查看更新、 刪除和插入的資料批次中的第一個。 在此教學課程中我們了解資料庫交易如何允許...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/26/2007
ms.topic: article
ms.assetid: b45fede3-c53a-4ea1-824b-20200808dbae
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs
msc.type: authoredcontent
ms.openlocfilehash: a3f8ec2de7b9259e4bb83f4346bde8abfd643fb4
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30888951"
---
<a name="wrapping-database-modifications-within-a-transaction-c"></a>在交易中 (C#) 內文繞圖資料庫修改
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_CS.zip)或[下載 PDF](wrapping-database-modifications-within-a-transaction-cs/_static/datatutorial63cs1.pdf)

> 本教學課程是四個查看更新、 刪除和插入的資料批次中的第一個。 本教學課程中我們了解資料庫交易如何允許批次執行時，成為不可部分完成的作業，可確保，所有步驟都成功或失敗的所有步驟的修改。


## <a name="introduction"></a>簡介

如我們所見開頭[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-cs.md)教學課程中，GridView 提供內建支援的資料列層級編輯和刪除。 透過按幾下滑鼠就可能不需要撰寫一行程式碼，建立豐富的資料修改介面，因此只要您內容的編輯和刪除以每個資料列的基礎。 不過，在某些情況下這還不足夠，因此需要為使用者提供編輯或刪除記錄的批次的能力。

比方說，最網頁型電子郵件用戶端使用方格，列出每個訊息，每個資料列包含核取方塊，以及電子郵件中的 s 資訊 （例如主旨、 寄件者，等等）。 這個介面允許使用者刪除它們，然後按一下 [刪除選取的郵件] 按鈕的多個訊息。 理想情況下，使用者通常編輯多筆不同記錄批次編輯介面。 而不是強迫使用者按一下 編輯、 其變更，必須修改每一筆記錄，然後按一下 更新，批次編輯介面呈現其編輯介面與每個資料列。 使用者可以快速修改需要變更的資料列集，並按一下 [全部更新] 按鈕，以儲存這些變更。 在這組教學課程中，我們將檢驗如何建立介面的插入、 編輯和刪除批次的資料。

執行批次作業時它來判斷是否應該可以針對某些成功的批次中的作業時的其他重要 s 失敗。 請考慮刪除介面-採取的動作已成功刪除第一個選取的記錄，但第二個失敗，例如，因為外部索引鍵條件約束違規的緣故，如果批次？ 應該先記錄的刪除復原，或可以接受第一筆記錄，保留已刪除？

如果您想要當成處理的批次作業[不可部分完成的作業](http://en.wikipedia.org/wiki/Atomic_operation)，其中是的所有步驟成功或的所有步驟失敗，則必須予以增加以包含支援資料存取層一個[資料庫交易](http://en.wikipedia.org/wiki/Database_transaction)。 資料庫交易保證不可部份完成性的一組`INSERT`， `UPDATE`，和`DELETE`陳述式起交易之下執行，而且是大多數所有新型態的資料庫系統所支援的功能。

本教學課程中我們將探討如何擴充 DAL 使用資料庫交易。 後續的教學課程會檢查批次插入、 更新和刪除介面實作的 web 網頁。 Let s 開始 ！

> [!NOTE]
> 修改在批次交易中的資料時，不一定需要不可部份完成性。 在某些情況下，可能會接受已成功修改某些資料，有些是在相同批次失敗，例如當從網頁型電子郵件用戶端刪除一組電子郵件。 如果沒有 s 處理透過刪除資料庫錯誤中途島，它可能是可接受處理不會發生錯誤的記錄保留已刪除的 s。 在這種情況下，DAL 不必修改為支援資料庫的交易。 有其他批次作業的情況下，然而，不可部分完成性很重要。 當客戶會將她資金從銀行帳戶移至另一個時，必須執行兩項作業： 必須扣除的第一個帳戶，然後加入第二個款項。 銀行可能不介意成功的第一個步驟，但是第二個步驟失敗，客戶會理解會感到生氣。 我建議您在本教學課程工作，並實作至 DAL 支援資料庫的交易，即使您不打算使用它們在批次的插入、 更新和刪除我們會建置下列三個教學課程中的介面增強功能。


## <a name="an-overview-of-transactions"></a>交易的概觀

大部分資料庫都包含支援*交易*，可讓多個資料庫命令，以便將標準群組為單一邏輯工作單位。 資料庫命令組成交易保證都是不可部分完成，這表示，所有的命令將會失敗或所有將會成功。

一般情況下，交易是透過使用下列模式的 SQL 陳述式的方式實作：

1. 表示開始的交易。
2. 執行 SQL 陳述式組成的交易。
3. 如果其中一種從步驟 2 中，復原交易的陳述式錯誤。
4. 如果所有的步驟 2 中的陳述式完成不會發生錯誤時，會認可交易。

SQL 陳述式，用來建立、 認可和回復時撰寫 SQL 指令碼或建立預存程序，交易也可以手動輸入或透過程式設計表示使用 ADO.NET 或中的類別[ `System.Transactions`命名空間](https://msdn.microsoft.com/library/system.transactions.aspx)。 在此教學課程中我們只會檢查管理使用 ADO.NET 的交易。 在未來的教學課程中我們將探討如何使用預存程序中的資料存取層，此時，我們將探討 SQL 陳述式來建立、 回復，並認可交易。 在此同時，請參閱[管理 SQL Server 預存程序中的交易](http://www.4guysfromrolla.com/webtech/080305-1.shtml)如需詳細資訊。

> [!NOTE]
> [ `TransactionScope`類別](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx)中`System.Transactions`命名空間可讓開發人員以程式設計方式將一系列陳述式包裝在交易範圍內，而且包括牽涉到多個複雜交易支援來源，例如兩個不同的資料庫或甚至異質資料存放區，例如 Microsoft SQL Server 資料庫、 Oracle 資料庫，以及 Web 服務的類型。 我我們決定要使用 ADO.NET 交易，而不是本教學課程`TransactionScope`類別因為 ADO.NET 是更特定的資料庫交易和在許多情況下，是最少需要大量的資源。 此外，在某些狀況下`TransactionScope`類別會使用 Microsoft Distributed Transaction Coordinator (MSDTC)。 設定、 實作及效能問題周圍 MSDTC 使得特製化而進階主題和這些教學課程的範圍之外。


當使用 SqlClient 提供者在 ADO.NET 中，會透過呼叫初始化交易[`SqlConnection`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)s [ `BeginTransaction`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)，它會傳回[ `SqlTransaction`物件](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)。 結構的交易都放置在資料修改陳述式`try...catch`區塊。 如果發生錯誤的陳述式中`try`封鎖，請執行傳輸至`catch`區塊，其中交易可以回復透過`SqlTransaction`物件 s [ `Rollback`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)。 如果成功，所有的陳述式完成呼叫`SqlTransaction`物件 s [ `Commit`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx)結尾`try`區塊認可交易。 下列程式碼片段會示範這個模式。 請參閱[維護資料庫的一致性與交易](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)的其他語法和範例使用 ADO.NET 的交易。


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample1.cs)]

根據預設，具類型資料集內 Tableadapter 請勿使用交易。 若要提供交易支援，我們需要擴充的 TableAdapter 類別，包括使用上述的模式來執行一系列的資料修改陳述式的範圍內的交易的其他方法。 在步驟 2 中，我們會看到如何使用部分類別新增這些方法。

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>步驟 1： 建立工作批次的資料 Web 網頁

我們可以開始探索如何加強 DAL 支援資料庫的交易之前，可讓 s 先花點時間來建立 ASP.NET web pages，我們需要在此教學課程及遵循的三個。 加入新的資料夾，名為啟動`BatchData`然後加入下列的 asp.net WEB 網頁、 將相關聯的每一頁`Site.master`主版頁面。

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![如需 SqlDataSource 相關教學課程加入 ASP.NET 網頁](wrapping-database-modifications-within-a-transaction-cs/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁的 SqlDataSource 相關教學課程


如同其他資料夾，`Default.aspx`將使用`SectionLevelTutorialListing.ascx`列出的教學課程 > 一節中的使用者控制項。 因此，新增此使用者控制項加入`Default.aspx`拖曳到頁面的設計 檢視中的 方案總管 中。


[![將 SectionLevelTutorialListing.ascx 使用者控制項加入至 Default.aspx](wrapping-database-modifications-within-a-transaction-cs/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項加入`Default.aspx`([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image2.png))


最後，將這些四個頁面加入做為項目至`Web.sitemap`檔案。 具體來說，自訂之後加入下列標記網站導覽`<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample2.xml)]

在更新之後， `Web.sitemap`，花一點時間來檢視此教學課程網站，透過瀏覽器。 在左側功能表現在包含批次的資料教學課程使用的項目。


![批次的資料教學課程使用的站台地圖現在包含的項目](wrapping-database-modifications-within-a-transaction-cs/_static/image3.gif)

**圖 3**： 批次的資料教學課程使用的站台地圖現在包含的項目


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>步驟 2： 更新資料存取層來支援資料庫的交易

在 第一個教學課程中，我們所討論[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)，型別資料集，在我們的 DAL 組成 Datatable 和 Tableadapter。 Datatable 保存的資料，雖然 Tableadapter 提供的功能，從資料庫讀入 Datatable，Datatable 等所做的變更更新資料庫。 前文提過，Tableadapter 會提供兩種模式來更新資料，我稱為批次更新與 DB Direct。 批次更新模式中，使用 TableAdapter 會傳遞資料集、 資料表或資料行的集合。 這項資料會列舉和每個插入、 修改或刪除資料列， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`執行。 DB Direct 模式中，使用 TableAdapter 會改為傳遞所需的插入、 更新或刪除單一資料錄的資料行的值。 DB 直接模式方法再使用這些傳入的值來執行適當`InsertCommand`， `UpdateCommand`，或`DeleteCommand`陳述式。

不論使用的更新模式，Tableadapter 自動產生的方法不會使用交易。 依預設每個插入、 更新或刪除由 TableAdapter 會被視為單一的個別作業。 比方說，設想 DB Direct 模式使用 BLL 中某些程式碼，將十筆記錄插入資料庫。 此程式碼會呼叫 tableadapter`Insert`方法十倍。 如果前五個插入成功，但卻造成例外狀況的第六個的其中一個，插入的前五個記錄會保留在資料庫中。 同樣地，如果批次更新模式用來執行插入、 更新與刪除導引到插入經過修改，以及刪除 DataTable 中的資料列，如果第一個的一些修改成功，但更新版本時發生錯誤，這些更早的修改，完成會保留在資料庫中。

在某些情況下我們想要確保之間修改的一系列的不可部份完成性。 若要完成此我們必須加入執行的新方法，以手動方式擴增 TableAdapter `InsertCommand`， `UpdateCommand`，和`DeleteCommand`s 起的交易。 在[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)我們探討了使用[部分類別](http://en.wikipedia.org/wiki/Partial_type)以擴充輸入資料集內 Datatable 的功能。 這項技術也可以搭配 Tableadapter。

型別資料集`Northwind.xsd`位於`App_Code`資料夾的`DAL`子資料夾。 建立子資料夾中的`DAL`資料夾名為`TransactionSupport`並加入新的類別檔案命名為`ProductsTableAdapter.TransactionSupport.cs`（請參閱圖 4）。 這個檔案會保留部分實作`ProductsTableAdapter`包含用於執行資料修改使用交易的方法。


![加入名為 TransactionSupport 的資料夾和名為 ProductsTableAdapter.TransactionSupport.cs 類別檔案](wrapping-database-modifications-within-a-transaction-cs/_static/image4.gif)

**圖 4**： 新增名為資料夾`TransactionSupport`和名為的類別檔案 `ProductsTableAdapter.TransactionSupport.cs`


輸入下列程式碼`ProductsTableAdapter.TransactionSupport.cs`檔案：


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample3.cs)]

`partial`類別宣告中的關鍵字指示編譯器中新增的成員會加入至`ProductsTableAdapter`類別`NorthwindTableAdapters`命名空間。 請注意`using System.Data.SqlClient`在檔案最上方的陳述式。 由於 TableAdapter 已設定為使用 SqlClient 提供者，在內部使用[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx)其命令發出至資料庫的物件。 因此，我們需要使用`SqlTransaction`類別來開始交易，然後加以認可或回復它。 如果您使用 Microsoft SQL Server 以外的資料存放區，您必須使用適當的提供者。

這些方法提供啟動，復原所需的建置組塊，並認可交易。 它們就會標示`public`，好讓他們能夠從內使用`ProductsTableAdapter`、 從另一個類別中 DAL 或從另一層的架構，例如 BLL 中。 `BeginTransaction` 開啟內部 tableadapter `SqlConnection` （如有需要），開始的交易，並將其指派給`Transaction`屬性，並將交易附加至內部`SqlDataAdapter`s`SqlCommand`物件。 `CommitTransaction` 和`RollbackTransaction`呼叫`Transaction`物件 s`Commit`和`Rollback`方法，分別之前關閉內部`Connection`物件。

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>步驟 3： 加入方法來更新及刪除起交易資料

利用這些方法完成，我們重新準備好將方法加入至`ProductsDataTable`或 BLL 執行一系列的交易起命令。 下列方法會使用批次更新模式更新`ProductsDataTable`執行個體使用的交易。 它藉由呼叫會啟動交易`BeginTransaction`方法，然後使用`try...catch`發出資料修改陳述式區塊。 如果呼叫`Adapter`物件 s`Update`方法導致例外狀況，執行會傳輸至`catch`交易將回復的區塊，重新擲回的例外狀況。 請記得，`Update`方法會實作批次更新模式列舉所提供的資料列所`ProductsDataTable`並執行必要`InsertCommand`， `UpdateCommand`，和`DeleteCommand`s。 如果其中一個這些命令會產生錯誤，會復原交易，並復原先前交易的存留期間所做的變更。 應該`Update`陳述式完成不會發生錯誤，整個認可交易。


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample4.cs)]

新增`UpdateWithTransaction`方法`ProductsTableAdapter`類別中的部分類別透過`ProductsTableAdapter.TransactionSupport.cs`。 或者，您可以將此方法加入到商務邏輯層的`ProductsBLL`次要進行一些語法變更的類別。 關鍵字也就是，這在`this.BeginTransaction()`， `this.CommitTransaction()`，和`this.RollbackTransaction()`會想要取代`Adapter`(請記得，`Adapter`是中的屬性名稱`ProductsBLL`型別的`ProductsTableAdapter`)。

`UpdateWithTransaction`方法會使用批次更新模式中，但是一系列的 DB 直接呼叫也可以使用如下所示的方法，在交易範圍內。 `DeleteProductsWithTransaction`方法接受做為輸入`List<T>`型別的`int`，這是`ProductID`要刪除。 方法會起始交易，透過對呼叫`BeginTransaction`，然後在`try`封鎖，因此請逐一呼叫 DB Direct 模式提供的清單`Delete`每個方法`ProductID`值。 如果任何呼叫`Delete`失敗，控制權會轉移到`catch`區塊其中交易會復原並重新擲回的例外狀況。 如果所有呼叫`Delete`成功，則會認可交易。 將此方法加入`ProductsBLL`類別。


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample5.cs)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>套用交易跨多個 TableAdapters

檢查此教學課程中的交易相關程式碼可讓多個陳述式針對`ProductsTableAdapter`被視為不可部分完成的作業。 但多個不同的資料庫資料表的修改需要自動執行該怎麼辦？ 比方說，當刪除類別目錄，我們可能會先想要重新指派其目前的產品，其他類別目錄。 應該執行這兩個步驟重新指派的產品和刪除類別目錄，成為不可部分完成的作業。 但是`ProductsTableAdapter`包含只修改方法`Products`資料表和`CategoriesTableAdapter`包含只修改方法`Categories`資料表。 如何在交易可以包含這兩個 Tableadapter？

其中一個選項是將方法加入`CategoriesTableAdapter`名為`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`並使用該方法呼叫的預存程序，同時重新指派的產品，並刪除預存程序內所定義的交易範圍內的分類。 我們會探討如何開始、 認可和回復交易，預存程序在未來的教學課程。

另一個選項是建立協助程式類別中包含的 DAL`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`方法。 這個方法會建立的執行個體`CategoriesTableAdapter`和`ProductsTableAdapter`，然後設定這些兩個 Tableadapter`Connection`屬性至相同`SqlConnection`執行個體。 在該點，其中之一的兩個 Tableadapter 會起始交易呼叫`BeginTransaction`。 重新指派的產品和刪除類別目錄的 Tableadapter 方法會叫用`try...catch`與交易認可或回復回復為所需的區塊。

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>步驟 4： 加入`UpdateWithTransaction`商務邏輯層的方法

步驟 3 新增`UpdateWithTransaction`方法`ProductsTableAdapter`DAL 中。 我們應該新增 BLL 對應的方法。 雖然展示層無法直接向要叫用 DAL 呼叫`UpdateWithTransaction`這些教學課程有 strived 方法，來定義從展示層 DAL，以隔離的多層式的架構。 因此，它 behooves 我們繼續為此方法。

開啟`ProductsBLL`類別檔案，並新增名為`UpdateWithTransaction`只需呼叫到對應的 DAL 方法。 現在應該在兩個新方法`ProductsBLL`: `UpdateWithTransaction`，其中您剛才加入和`DeleteProductsWithTransaction`，已加入在步驟 3 中。


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample6.cs)]

> [!NOTE]
> 這些方法不包含`DataObjectMethodAttribute`屬性中的大多數其他方法來指派`ProductsBLL`類別，因為我們將會叫用這些方法直接從 ASP.NET 網頁程式碼後置類別。 請記得，`DataObjectMethodAttribute`用於哪種方法應該會出現在 ObjectDataSource 的設定資料來源精靈] 和 [索引標籤 （SELECT、 UPDATE、 INSERT 或 DELETE） 的旗標。 因為 GridView 缺少批次編輯或刪除任何內建支援，我們必須以程式設計方式叫用這些方法，而不是使用宣告式程式碼可用的方法。


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>步驟 5： 自動更新從展示層的資料庫資料

為了說明交易已更新記錄的批次時的效果，讓 s 建立列出 GridView 中的所有產品，並包含按鈕 Web 使用者介面控制項，當按下，重新指派產品`CategoryID`值。 類別目錄重新指派會特別的是，進度，如此第一個有幾項產品皆有效`CategoryID`指派不存在的值則是故意`CategoryID`值。 如果我們嘗試將與產品更新資料庫的`CategoryID`不符合現有分類的`CategoryID`，外部索引鍵條件約束違規會發生，且會引發例外狀況。 我們會看到在這個範例是，當使用異動從外部索引鍵條件約束違規引發的例外狀況會導致先前有效`CategoryID`回復變更。 當未使用的交易，不過，會保持初始類別目錄的修改。

先開啟`Transactions.aspx`頁面`BatchData`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 GridView。 設定其`ID`至`Products`和其智慧標籤上，從繫結到名為新 ObjectDataSource `ProductsDataSource`。 設定提取資料的來源 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 這將是唯讀的 GridView，因此設定下拉式清單中更新、 插入和刪除索引標籤，以 （無） 與按一下 [完成]。


[![圖 5： 設定為使用 ProductsBLL 類別的 GetProducts 方法 ObjectDataSource](wrapping-database-modifications-within-a-transaction-cs/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image3.png)

**圖 5**： 圖 5： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProducts`方法 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image4.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](wrapping-database-modifications-within-a-transaction-cs/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image5.png)

**圖 6**： 下拉式清單中設定更新、 插入和刪除的索引標籤 （無） ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image6.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 及其產品資料欄位。 移除所有的這些欄位，除了`ProductID`， `ProductName`， `CategoryID`，和`CategoryName`並重新命名`ProductName`和`CategoryName`BoundFields`HeaderText`屬性 Product 和 Category，分別。 從智慧標籤中，核取 [啟用分頁] 選項。 進行這些修改後, GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample7.aspx)]

接下來，加入三個按鈕 Web 控制項上方 GridView。 重新整理方格、 修改分類 （與交易） 的第二個 s 和修改類別 （不含交易） 的第一個 s 設 s 文字屬性的第一個按鈕。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample8.aspx)]

此時在 Visual Studio 中的 [設計] 檢視看起來應該類似螢幕擷取畫面圖 7 所示。


[![此頁面包含的 GridView 和三個按鈕 Web 控制項](wrapping-database-modifications-within-a-transaction-cs/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image7.png)

**圖 7**: 頁面包含的 GridView 和三個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image8.png))


建立三個按鈕 s 的每個事件處理常式`Click`事件，並使用下列程式碼：


[!code-csharp[Main](wrapping-database-modifications-within-a-transaction-cs/samples/sample9.cs)]

重新整理按鈕 s`Click`事件處理常式只會重新繫結至 GridView 的資料藉由呼叫`Products`GridView 的`DataBind`方法。

第二個事件處理常式會重新指派產品`CategoryID`s，並使用新的交易方法，從執行資料庫 BLL 更新起交易。 請注意，每個產品 s`CategoryID`任意設為相同的值做為其`ProductID`。 這將可以正常運作的第幾項產品，因為這些產品具有`ProductID`值時，會發生對應至有效`CategoryID`s。 但一次`ProductID`s 開始變得過大，此巧合重疊`ProductID`s 和`CategoryID`s 不再適用。

第三個`Click`事件處理常式更新產品`CategoryID`中相同的方式，但會將更新傳送到資料庫時使用`ProductsTableAdapter`s 預設`Update`方法。 這`Update`方法不會換一系列的命令在交易內，讓第一個遇到的外部索引鍵條件約束違規錯誤之前進行這些變更將會保存。

若要示範此行為，請造訪此頁透過瀏覽器。 一開始您應該看到第一頁資料，如圖 8 所示。 接下來，按一下 [修改類別 （與交易）] 按鈕。 這將導致回傳，並嘗試更新的所有產品`CategoryID`等值，但會導致外部索引鍵條件約束違規 （請參閱圖 9）。


[![中可分頁的 GridView 會顯示產品](wrapping-database-modifications-within-a-transaction-cs/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image9.png)

**圖 8**: 產品會顯示在分頁 GridView ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image10.png))


[![重新指派的分類會導致外部索引鍵條件約束違規](wrapping-database-modifications-within-a-transaction-cs/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image11.png)

**圖 9**： 重新分類中的結果，外部索引鍵條件約束違規 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image12.png))


現在叫用瀏覽器 s [上一頁] 按鈕，然後按一下 [重新整理格線] 按鈕。 在資料重新整理時您應該看到完全相同的輸出，如圖 8 所示。 也就是說，即使仍有某些產品`CategoryID`s 已變更合法的值，並更新資料庫中，已將交易回復時發生外部索引鍵條件約束違規。

現在請按一下 [修改類別 （不含交易）] 按鈕。 這會導致相同的 foreign key 條件約束違規錯誤 （請參閱圖 9），但這次這些產品的`CategoryID`值已變更為合法值將不會回復。 叫用瀏覽器 s 上一頁 按鈕，然後重新整理格線 按鈕。 如圖 10 所示，`CategoryID`前八個產品之已重新指派。 例如，在圖 8 中，變更必須`CategoryID`為 1，但在圖 10 it s 已重新指派為 2。


[![某些產品 CategoryID 值未更新時其他人已](wrapping-database-modifications-within-a-transaction-cs/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-cs/_static/image13.png)

**圖 10**： 某些產品`CategoryID`值未更新時其他人已 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-cs/_static/image14.png))


## <a name="summary"></a>總結

根據預設，TableAdapter 的方法不會換行的資料庫執行陳述式，在交易範圍內，但是我們可以輕鬆地加入方法，以建立、 認可和回復交易。 在本教學課程中，我們建立三個這類方法中的`ProductsTableAdapter`類別： `BeginTransaction`， `CommitTransaction`，和`RollbackTransaction`。 我們了解如何使用這些方法，連同`try...catch`進行一系列的資料修改陳述式不可部分完成的區塊。 特別是，我們建立`UpdateWithTransaction`方法中的`ProductsTableAdapter`，其用於執行必要的修改提供的資料列批次更新模式`ProductsDataTable`。 我們也加入`DeleteProductsWithTransaction`方法`ProductsBLL`類別中，可接受 BLL`List`的`ProductID`值做為其輸入，並呼叫 DB Direct 模式方法`Delete`每個`ProductID`。 這兩種方法啟動所建立的交易，並執行資料修改陳述式內`try...catch`區塊。 如果發生例外狀況，交易回復，否則便被認可。

步驟 5 所述的效果與帶走使用交易的批次更新的交易式批次更新。 接下來三個教學課程中我們會配置在本教學課程的基礎建置，以及建立使用者介面，執行批次更新、 刪除及插入。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [維護資料庫與交易的一致性](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [管理 SQL Server 中的交易預存程序](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [交易更容易： `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope 和 Dataadapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [在.NET 中使用 Oracle 資料庫交易](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已 Dave Gardner、 Hilton Giesenow 和本文菲。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [下一步](batch-updating-cs.md)
