---
uid: web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
title: 換行 (VB) 在交易內的資料庫修改 |Microsoft Docs
author: rick-anderson
description: 本教學課程會探討更新、 刪除和插入的資料批次的四個中的第一個。 在本教學課程中我們了解如何讓資料庫交易...
ms.author: aspnetcontent
ms.date: 06/26/2007
ms.assetid: 7d821db5-6cbb-4b38-af14-198f9155fc82
msc.legacyurl: /web-forms/overview/data-access/working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb
msc.type: authoredcontent
ms.openlocfilehash: 877174bad08970eed0cab52d0f1d8a521f7d2cc0
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840741"
---
<a name="wrapping-database-modifications-within-a-transaction-vb"></a>資料庫修改包裝在交易 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_63_VB.zip)或[下載 PDF](wrapping-database-modifications-within-a-transaction-vb/_static/datatutorial63vb1.pdf)

> 本教學課程會探討更新、 刪除和插入的資料批次的四個中的第一個。 在本教學課程中我們了解資料庫交易如何允許做為不可部分完成的作業，可確保，所有步驟都成功或失敗的所有步驟執行的批次修改。


## <a name="introduction"></a>簡介

如我們所見開頭[概觀的插入、 更新和刪除資料](../editing-inserting-and-deleting-data/an-overview-of-inserting-updating-and-deleting-data-vb.md)教學課程中，GridView 提供內建支援的資料列層級編輯和刪除。 按幾下滑鼠就可以，只要您是內容與編輯和刪除以每個資料列的基礎，而不需要撰寫一行程式碼，建立豐富的資料修改介面。 不過，在某些情況下這還不足夠，而且我們需要提供使用者編輯或刪除批次的記錄功能。

比方說，最網頁型電子郵件用戶端使用方格列出每個訊息，每個資料列包含一個核取方塊，以及 （主旨、 寄件者等） 的電子郵件的資訊。 這個介面允許使用者刪除多個訊息，以檢查它們，然後按一下 [刪除選取的訊息] 按鈕。 批次編輯介面是理想的情況下，使用者經常編輯許多不同的記錄方式。 而不是強迫使用者按一下 編輯其變更，並必須修改每一筆記錄，然後按一下 更新、 批次編輯介面呈現具有其編輯介面的每個資料列。 使用者可以快速修改的需要變更的資料列集，並按一下 [全部更新] 按鈕，以儲存這些變更。 在這套教學課程中，我們將檢驗如何建立用於插入、 編輯和刪除的資料批次的介面。

執行批次作業時它來判斷是否應該可以針對某些成功的批次中的作業，而其他重要失敗。 請考慮批次刪除介面-如果已成功，刪除第一個選取的記錄，但第二個失敗，說，因為外部索引鍵條件約束違規，應該發生什麼情況？ 應該先記錄的刪除復原，或它是否可接受的第一筆記錄，保留已刪除？

如果您想在批次作業被視為[不可部分完成的作業](http://en.wikipedia.org/wiki/Atomic_operation)，其中一個位置是所有步驟成功或所有步驟失敗，則資料存取層必須予以增加以包含支援[資料庫交易](http://en.wikipedia.org/wiki/Database_transaction)。 資料庫交易保證不可部份完成性整組`INSERT`， `UPDATE`，和`DELETE`陳述式交易的傘狀下執行，而是大部分的所有最新的資料庫系統所支援功能。

在本教學課程中我們將探討如何將 DAL 使用資料庫交易。 後續的教學課程會檢查批次插入、 更新和刪除介面實作的 web 網頁。 讓 s 開始 ！

> [!NOTE]
> 修改批次交易中的資料時，不一定需要不可部分完成性。 在某些情況下，可能會接受有一些成功的資料修改，而且相同的批次中的其他項目失敗，例如當刪除從 web 型電子郵件用戶端的一組電子郵件。 如果沒有 s 刪除資料庫錯誤中途處理，它可能可以接受這些處理不會發生錯誤的記錄保留已刪除的 s。 在此情況下，DAL 不必修改以支援資料庫的交易。 有其他批次作業的情況下，不過，不可部分完成性很重要。 當客戶會將她的資金從一個銀行帳戶移至另一個時，必須執行兩項作業： 資金必須扣除的第一個帳戶，然後新增第二個。 雖然銀行可能不介意成功的第一個步驟，但第二個步驟失敗，也因此會不滿其客戶。 建議您完成本教學課程和實作以支援資料庫的交易，即使您不打算運用在批次的插入、 更新和刪除我們將在下列三個教學課程中建置的介面 DAL 的增強功能。


## <a name="an-overview-of-transactions"></a>交易的概觀

大部分的資料庫包含對*交易*，可讓多個資料庫命令，以便分組到單一工作邏輯單位。 資料庫命令構成交易保證是不可部分完成，這表示可能是所有的命令將會失敗，或所有將會成功。

一般情況下，交易是透過使用下列模式的 SQL 陳述式的方式實作：

1. 表示開始的交易。
2. 執行 SQL 陳述式構成交易。
3. 如果其中一種從步驟 2 中，復原交易的陳述式錯誤。
4. 如果所有步驟 2 中的陳述式完成不會發生錯誤時，會認可交易。

SQL 陳述式，用來建立、 認可及回復時撰寫 SQL 指令碼或建立預存程序，交易也可以手動輸入或透過程式設計方式表示，使用 ADO.NET 或中的類別[ `System.Transactions`命名空間](https://msdn.microsoft.com/library/system.transactions.aspx)。 在本教學課程中我們只會檢查管理使用 ADO.NET 的交易。 在未來的教學課程中我們將探討如何使用預存程序在資料存取層中，在這段中，我們將探討 SQL 陳述式來建立、 回復，並認可交易。 在此同時，請參閱[SQL Server 預存程序中的 管理交易](http://www.4guysfromrolla.com/webtech/080305-1.shtml)如需詳細資訊。

> [!NOTE]
> [ `TransactionScope`類別](https://msdn.microsoft.com/library/system.transactions.transactionscope.aspx)在`System.Transactions`命名空間可讓開發人員以程式設計方式將一系列的陳述式包裝在交易範圍內，且包含牽涉到多個複雜交易的支援例如，兩個不同的資料庫或甚至異質性類型的資料存放區，例如 Microsoft SQL Server 資料庫、 Oracle 資料庫和 Web 服務的來源。 我決定要使用 ADO.NET 交易，而不是本教學課程的 ve`TransactionScope`類別因為 ADO.NET 是更特定的資料庫交易和在許多情況下，是最少需要大量的資源。 此外，在特定狀況下`TransactionScope`類別會使用 Microsoft Distributed Transaction Coordinator (MSDTC)。 組態、 實作與效能問題周圍的 MSDTC 使得而不是特製化和進階的主題和這些教學課程的範圍之外。


當使用 SqlClient 提供者，在 ADO.NET 中，會透過呼叫初始化交易[`SqlConnection`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.aspx)s [ `BeginTransaction`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection.begintransaction.aspx)，以傳回[ `SqlTransaction`物件](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.aspx)。 結構的交易都會放在資料修改陳述式`try...catch`區塊。 如果中的陳述式中發生錯誤`try`封鎖，請執行傳輸至`catch`區塊，交易可以回復透過`SqlTransaction`物件 s [ `Rollback`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.rollback.aspx)。 如果所有的陳述式順利完成，呼叫`SqlTransaction`物件 s [ `Commit`方法](https://msdn.microsoft.com/library/system.data.sqlclient.sqltransaction.commit.aspx)結尾`try`區塊認可交易。 下列程式碼片段會示範這個模式。 請參閱[交易與維護資料庫一致性](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)如其他的語法和範例使用 ADO.NET 中的交易。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample1.vb)]

根據預設，在具類型資料集 Tableadapter 不使用交易。 若要提供支援，我們需要擴充的 TableAdapter 類別，以包含額外的方法，使用上述的模式來執行一系列的資料修改陳述式的範圍內的交易的交易。 在步驟 2 中，我們將了解如何新增這些方法會使用部分類別。

## <a name="step-1-creating-the-working-with-batched-data-web-pages"></a>步驟 1： 建立工作與批次的資料 Web Pages

我們開始探索如何擴充為支援資料庫交易的 DAL 之前，讓先花點時間來建立 ASP.NET web pages，我們必須在本教學課程及遵循的三個 s。 藉由新增名為的新資料夾啟動`BatchData`，然後新增下列的 asp.net WEB 網頁、 關聯與每個頁面`Site.master`主版頁面。

- `Default.aspx`
- `Transactions.aspx`
- `BatchUpdate.aspx`
- `BatchDelete.aspx`
- `BatchInsert.aspx`


![加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程](wrapping-database-modifications-within-a-transaction-vb/_static/image1.gif)

**圖 1**： 加入 ASP.NET 網頁，如 SqlDataSource 與相關的教學課程


如同其他的資料夾中，`Default.aspx`會使用`SectionLevelTutorialListing.ascx`列出的教學課程 > 一節中的使用者控制項。 因此，新增此使用者控制項`Default.aspx`從拖曳到頁面的設計 檢視中的 方案總管 中拖曳。


[![將 SectionLevelTutorialListing.ascx 使用者控制項新增至 Default.aspx](wrapping-database-modifications-within-a-transaction-vb/_static/image2.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image1.png)

**圖 2**： 新增`SectionLevelTutorialListing.ascx`使用者控制項`Default.aspx`([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image2.png))


最後，將這些四個頁面新增項目為`Web.sitemap`檔案。 具體來說，自訂之後新增下列標記網站地圖`<siteMapNode>`:


[!code-xml[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample2.xml)]

在更新之後`Web.sitemap`，花點時間檢視教學課程網站，透過瀏覽器。 在左側功能表現在包含批次的資料教學課程使用的項目。


![網站導覽現在包含批次的資料教學課程使用的項目](wrapping-database-modifications-within-a-transaction-vb/_static/image3.gif)

**圖 3**： 網站地圖現在包含批次的資料教學課程使用的項目


## <a name="step-2-updating-the-data-access-layer-to-support-database-transactions"></a>步驟 2： 更新資料的存取層，來支援資料庫的交易

當上一步中第一個教學課程中，我們討論過[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)，在我們的 DAL 中具類型資料集包含 Datatable 和 Tableadapter。 Tableadapter 會提供功能，以從資料庫的資料讀入 Datatable，Datatable 等等所做的變更更新資料庫時，Datatable 會保留資料。 您應該記得 TableAdapters 提供兩種模式，來更新資料，我稱為批次更新和 DB Direct。 使用批次更新模式時，會將 TableAdapter 傳遞資料集、 DataTable 或 Datarow 的集合。 這項資料會列舉和每個插入、 修改或刪除的資料列， `InsertCommand`， `UpdateCommand`，或`DeleteCommand`執行。 使用 DB 直接模式時，TableAdapter 會改為傳遞所需的插入、 更新或刪除單一資料錄的資料行的值。 DB 直接模式方法，然後使用這些傳入的值來執行適當`InsertCommand`， `UpdateCommand`，或`DeleteCommand`陳述式。

不論所使用的更新模式，Tableadapter 所自動產生的方法不會使用交易。 依預設每個插入、 更新或刪除由 TableAdapter 會被視為單一的個別作業。 例如，假設資料庫直接模式用到 BLL 中某些程式碼，來插入資料庫中的 10 筆記錄。 此程式碼會呼叫 tableadapter`Insert`方法十倍。 如果前五個插入成功，但卻造成例外狀況的第六個前, 五個插入的記錄會保留在資料庫中。 同樣地，如果批次更新模式用來執行插入、 更新和刪除以插入，修改和刪除的 DataTable 中的資料列如果第一個的一些修改成功，但更新版本時發生錯誤，這些較早的修改，完成會保留在資料庫中。

在某些情況下我們想要確保所做的修改一系列不可部分完成性。 若要這麼做將執行的新方法加入，我們必須以手動方式擴充 TableAdapter `InsertCommand`， `UpdateCommand`，和`DeleteCommand`s 在交易的保護傘。 在[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)我們探討了使用[部分類別](http://en.wikipedia.org/wiki/Partial_type)擴充功能的輸入資料集內的 Datatable。 這項技術也可以搭配 TableAdapters。

輸入資料集`Northwind.xsd`位於`App_Code`資料夾的`DAL`子資料夾。 建立的子資料夾中`DAL`名為資料夾`TransactionSupport`並新增新的類別檔案，名為`ProductsTableAdapter.TransactionSupport.vb`（請參閱 圖 4）。 這個檔案會保留部分實作`ProductsTableAdapter`包含執行使用交易的資料修改的方法。


![新增名為適用的 TransactionSupport 的資料夾和名為 ProductsTableAdapter.TransactionSupport.vb 類別檔案](wrapping-database-modifications-within-a-transaction-vb/_static/image4.gif)

**圖 4**： 新增名為資料夾`TransactionSupport`和名為的類別檔案 `ProductsTableAdapter.TransactionSupport.vb`


輸入下列程式碼插入`ProductsTableAdapter.TransactionSupport.vb`檔案：


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample3.vb)]

`Partial`在類別宣告中的關鍵字指示編譯器中新增的成員會加入至`ProductsTableAdapter`類別中`NorthwindTableAdapters`命名空間。 請注意`Imports System.Data.SqlClient`陳述式在檔案頂端。 TableAdapter 已設定為使用 SqlClient 提供者，因為在內部使用[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter.aspx)對資料庫發出命令的物件。 因此，我們需要使用`SqlTransaction`類別來開始交易，然後將它回復或認可它。 如果您使用 Microsoft SQL Server 以外的資料存放區，您必須使用適當的提供者。

這些方法會提供啟動，復原所需的建置組塊，並認可交易。 標示`Public`，讓他們能夠使用從`ProductsTableAdapter`、 DAL 中的另一個類別或結構，例如 BLL 中的另一層。 `BeginTransaction` 開啟內部 tableadapter `SqlConnection` （如有需要）、 開始的交易，並將其指派給`Transaction`屬性，並將交易附加至內部`SqlDataAdapter`s`SqlCommand`物件。 `CommitTransaction` 並`RollbackTransaction`呼叫`Transaction`物件 s`Commit`並`Rollback`方法，分別，再關閉內部`Connection`物件。

## <a name="step-3-adding-methods-to-update-and-delete-data-under-the-umbrella-of-a-transaction"></a>步驟 3： 加入方法來更新及刪除資料在交易的傘

利用這些完整的方法，我們重新準備好將方法加入至`ProductsDataTable`或執行一系列的命令在交易的傘狀 BLL。 下列方法會使用批次更新模式，來更新`ProductsDataTable`執行個體使用的交易。 交易開始時先呼叫`BeginTransaction`方法，然後再使用`Try...Catch`發出資料修改陳述式區塊。 如果在呼叫`Adapter`物件 s`Update`方法會產生例外狀況，執行就會傳送到`catch`交易將回復的區塊並重新擲回的例外狀況。 請記得，`Update`方法會實作列舉所提供的資料列的批次更新模式`ProductsDataTable`並執行必要`InsertCommand`， `UpdateCommand`，和`DeleteCommand`s。 如果任一這些命令會導致錯誤，會回復交易，並復原先前交易 s 存留期期間所做的變更。 應該`Update`陳述式完成而沒有錯誤，整個認可交易。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample4.vb)]

新增`UpdateWithTransaction`方法，以`ProductsTableAdapter`類別中的部分類別透過`ProductsTableAdapter.TransactionSupport.vb`。 或者，您可以將此方法加入到商業邏輯層的`ProductsBLL`做一些次要的語法變更的類別。 亦即 keyword`Me`中`Me.BeginTransaction()`， `Me.CommitTransaction()`，和`Me.RollbackTransaction()`需要換成`Adapter`(請記得，`Adapter`中的屬性名稱`ProductsBLL`型別的`ProductsTableAdapter`)。

`UpdateWithTransaction`方法會使用批次更新模式中，但一系列的 DB 直接呼叫也可用在交易中，如下所示的方法範圍內。 `DeleteProductsWithTransaction`方法接受做為輸入`List(Of T)`型別的`Integer`，這是`ProductID`要刪除。 方法會起始交易，透過呼叫`BeginTransaction`，然後在`Try`區塊中，逐一查看提供的清單呼叫 DB 直接模式`Delete`方法，每個`ProductID`值。 如果呼叫的任何`Delete`失敗，控制權會轉移到`Catch`區塊，其中會回復交易並重新擲回的例外狀況。 如果所有呼叫`Delete`成功，則會認可交易。 將下列方法來新增`ProductsBLL`類別。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample5.vb)]

## <a name="applying-transactions-across-multiple-tableadapters"></a>將交易套用至多個 TableAdapters

交易相關的程式碼檢查在本教學課程可讓多個陳述式針對`ProductsTableAdapter`被視為不可部分完成的作業。 但如果需要以不可分割方式執行多個修改不同的資料庫資料表？ 比方說，當刪除類別目錄，我們可能會先想要重新指派其目前的產品，其他類別目錄。 應該執行這兩個步驟重新指派的產品，並刪除分類，成為不可部分完成的作業。 但是`ProductsTableAdapter`包含修改的方法`Products`資料表並`CategoriesTableAdapter`包含修改的方法`Categories`資料表。 那麼要如何交易可以包含這兩個 Tableadapter？

其中一個選項是將方法加入`CategoriesTableAdapter`名為`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`和呼叫預存程序，同時重新指派的產品，並刪除的預存程序中定義的交易範圍內的類別，該方法。 我們將探討如何開始、 認可和回復交易，預存程序在未來的教學課程。

另一個選項是建立協助程式類別中包含的 DAL`DeleteCategoryAndReassignProducts(categoryIDtoDelete, reassignToCategoryID)`方法。 這個方法會建立的執行個體`CategoriesTableAdapter`而`ProductsTableAdapter`，然後設定這些兩個 Tableadapter`Connection`屬性，以相同`SqlConnection`執行個體。 在該時間點，其中之一的兩個 Tableadapter 會起始呼叫交易`BeginTransaction`。 重新指派的產品和刪除分類的 Tableadapter 方法會在叫用`Try...Catch`與交易認可或復原回所需的區塊。

## <a name="step-4-adding-theupdatewithtransactionmethod-to-the-business-logic-layer"></a>步驟 4： 加入`UpdateWithTransaction`方法商業邏輯層

在我們加入在步驟 3`UpdateWithTransaction`方法，以`ProductsTableAdapter`DAL 中。 我們應該 BLL 中新增一個對應的方法。 雖然展示層可以呼叫 DAL 以叫用的直接下`UpdateWithTransaction`方法，這些教學課程有 strived 定義隔離從展示層 DAL 的多層式的架構。 因此，它 behooves 我們繼續這種方法。

開啟`ProductsBLL`類別檔案，並新增名為`UpdateWithTransaction`直接呼叫到對應的 DAL 方法。 現在應該在兩個新方法`ProductsBLL`: `UpdateWithTransaction`，這只是加入和`DeleteProductsWithTransaction`，即其中加入在步驟 3 中。


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample6.vb)]

> [!NOTE]
> 這些方法不包含`DataObjectMethodAttribute`屬性中的大部分其他方法指派`ProductsBLL`類別，因為我們將會叫用這些方法直接從 ASP.NET 頁面的程式碼後置類別。 請記得，`DataObjectMethodAttribute`用哪些方法應該會出現在 s 中的 ObjectDataSource 精靈和哪些 （SELECT、 UPDATE、 INSERT 或 DELETE） 索引標籤下設定資料來源加上旗標。 因為 GridView 不足，無法編輯或刪除批次的任何內建支援，我們必須以程式設計方式叫用這些方法，而不是使用無程式碼宣告式方法。


## <a name="step-5-atomically-updating-database-data-from-the-presentation-layer"></a>步驟 5： 以不可分割方式更新從展示層的資料庫資料

為了說明交易已更新記錄的批次時的效果，讓 s 建立列出 GridView 中的所有產品，並包含按鈕網頁的使用者介面控制項，按一下時，重新指派產品`CategoryID`值。 以便有效指派的第一次的數個產品類別目錄重新指派會特別的是，進行`CategoryID`指派不存在的值，有些則是故意`CategoryID`值。 如果我們嘗試更新資料庫中一項產品其`CategoryID`不符合現有分類的`CategoryID`、 foreign key 條件約束違規會發生，而且會引發例外狀況。 我們會看到在此範例是，當使用交易從外部索引鍵條件約束違規所引發的例外狀況會導致先前有效`CategoryID`回復變更。 當不使用交易，不過，會保持在初始的類別已修改。

首先開啟`Transactions.aspx`頁面中`BatchData`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳的 GridView。 設定其`ID`要`Products`和它的智慧標籤，從繫結至名為新 ObjectDataSource `ProductsDataSource`。 設定提取其資料從 ObjectDataSource`ProductsBLL`類別的`GetProducts`方法。 會是唯讀的 GridView，因此設定下拉式清單中更新、 插入和刪除索引標籤為 （無），並按一下 完成。


[![設定為使用 ProductsBLL 類別的 GetProducts 方法的 ObjectDataSource](wrapping-database-modifications-within-a-transaction-vb/_static/image5.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image3.png)

**圖 5**： 設定要使用 ObjectDataSource`ProductsBLL`類別 s`GetProducts`方法 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image4.png))


[![設定下拉式清單中更新、 插入和刪除 （無） 索引標籤](wrapping-database-modifications-within-a-transaction-vb/_static/image6.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image5.png)

**圖 6**： 設定下拉式清單中更新、 插入和刪除索引標籤為 （無） ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image6.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 及其產品資料欄位。 移除所有的這些欄位除外`ProductID`， `ProductName`， `CategoryID`，和`CategoryName`，並重新命名`ProductName`並`CategoryName`BoundFields`HeaderText`屬性，以 Product 和 Category，分別。 從智慧標籤，勾選 啟用分頁選項。 進行這些修改後, GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample7.aspx)]

接下來，新增上述 GridView 的三個按鈕 Web 控制項。 設定 s Text 屬性的第一個按鈕來重新整理方格、 s 修改類別 （與交易） 的第二個和第三個 s 修改類別 （而不需要交易）。


[!code-aspx[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample8.aspx)]

此時在 Visual Studio 中的 設計 檢視看起來應該類似螢幕擷取畫面的 圖 7 所示。


[![此頁面包含的 GridView 和三個按鈕 Web 控制項](wrapping-database-modifications-within-a-transaction-vb/_static/image7.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image7.png)

**圖 7**: 頁面包含的 GridView 和三個按鈕 Web 控制項 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image8.png))


建立事件處理常式每三個按鈕的`Click`事件，並使用下列程式碼：


[!code-vb[Main](wrapping-database-modifications-within-a-transaction-vb/samples/sample9.vb)]

重新整理按鈕 s`Click`事件處理常式只會重新繫結至 GridView 資料藉由呼叫`Products`GridView 的`DataBind`方法。

第二個事件處理常式會重新指派產品`CategoryID`s，並使用 BLL，讓資料庫的新交易方法更新在交易的保護傘。 請注意，每個產品 s`CategoryID`任意設為相同的值做為其`ProductID`。 這將會正常運作的前幾項產品，因為這些產品`ProductID`剛好對應至有效的值`CategoryID`s。 但是一次`ProductID`s 開始變得太大，此巧合重疊`ProductID`s 和`CategoryID`s 不再適用。

第三個`Click`事件處理常式更新產品`CategoryID`中的相同方式，但會將更新傳送到資料庫時使用`ProductsTableAdapter`預設`Update`方法。 這`Update`方法不會包裝在交易內的命令系列中，因此第一個遇到的外部索引鍵條件約束違規錯誤之前，進行這些變更會保存。

為了示範此行為，請瀏覽此頁面，透過瀏覽器。 一開始您應該看到的第一頁的資料，如 圖 8 所示。 接下來，按一下 [修改類別 （與交易）] 按鈕。 這會導致回傳，並嘗試更新的所有產品`CategoryID`值，但是會造成外部索引鍵條件約束違規 （請參閱 圖 9）。


[![產品都會顯示在可分頁的 GridView](wrapping-database-modifications-within-a-transaction-vb/_static/image8.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image9.png)

**圖 8**: 產品都會顯示在可分頁的 GridView ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image10.png))


[![重新指派分類結果中的外部索引鍵條件約束違規](wrapping-database-modifications-within-a-transaction-vb/_static/image9.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image11.png)

**圖 9**： 重新指派分類會導致外部索引鍵條件約束違規 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image12.png))


現在按下 s 的瀏覽器的 [上一頁] 按鈕，然後按一下 [重新整理方格] 按鈕。 重新整理資料時您應該看到完全相同的輸出，如 圖 8 所示。 也就是說，即使雖然部分產品`CategoryID`s 已變更合法的值，並更新資料庫中，已將交易回復時，發生外部索引鍵條件約束違規。

現在，請嘗試按一下 [修改類別 （而不需要交易）] 按鈕。 這會導致相同的外部索引鍵條件約束違規錯誤 （請參閱 圖 9），但這次這些產品的`CategoryID`值已變更為合法值將不會回復。 叫用您的瀏覽器 s 上一步 按鈕，然後重新整理方格 按鈕。 如 [圖 10] 所示，`CategoryID`已重新指派的前八個產品。 比方說，在 圖 8 中，變更必須`CategoryID`為 1，但在圖 10 it s 已重新指派為 2。


[![某些產品 CategoryID 值未更新而其他人已](wrapping-database-modifications-within-a-transaction-vb/_static/image10.gif)](wrapping-database-modifications-within-a-transaction-vb/_static/image13.png)

**圖 10**： 某些產品`CategoryID`值未更新而其他人已 ([按一下以檢視完整大小的影像](wrapping-database-modifications-within-a-transaction-vb/_static/image14.png))


## <a name="summary"></a>總結

根據預設，TableAdapter 的方法不會換行的資料庫執行陳述式，在交易範圍內，但我們可以很輕鬆地加入將建立的方法、 commit 和 rollback 交易。 在本教學課程中，我們建立在三個這類方法`ProductsTableAdapter`類別： `BeginTransaction`， `CommitTransaction`，和`RollbackTransaction`。 我們了解如何使用這些方法，連同`Try...Catch`進行一系列的資料修改陳述式不可部分完成的區塊。 我們建立了特別的是，`UpdateWithTransaction`方法中的`ProductsTableAdapter`，執行必要的修改提供的資料列的情況下，這在使用批次更新模式`ProductsDataTable`。 我們也新增了`DeleteProductsWithTransaction`方法，以`ProductsBLL`類別中 BLL，bll 接受`List`的`ProductID`值做為其輸入，並呼叫 DB 直接模式方法`Delete`每個`ProductID`。 這兩個方法所建立的交易，並執行中的資料修改陳述式會啟動`Try...Catch`區塊。 如果發生例外狀況，會回復交易，否則會認可。

步驟 5 所示交易式批次更新，會忽略使用交易的批次更新和的效果。 在接下來三個教學課程中，我們將在本教學課程中所配置的基礎建置，並建立執行批次更新、 刪除及插入的使用者介面。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [維護資料庫與交易的一致性](http://aspnet.4guysfromrolla.com/articles/072705-1.aspx)
- [管理交易，在 SQL Server 預存程序](http://www.4guysfromrolla.com/webtech/080305-1.shtml)
- [交易變得更容易： `System.Transactions`](https://blogs.msdn.com/florinlazar/archive/2004/07/23/192239.aspx)
- [TransactionScope 和 Dataadapter](http://andyclymer.blogspot.com/2007/01/transactionscope-and-dataadapters.html)
- [在.NET 中使用 Oracle 資料庫交易](http://www.oracle.com/technology/pub/articles/price_dbtrans_dotnet.html)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Dave Gardner、 Hilton Giesenow 和 Teresa murphy 徹底檢驗了。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](batch-inserting-cs.md)
> [下一頁](batch-updating-vb.md)
