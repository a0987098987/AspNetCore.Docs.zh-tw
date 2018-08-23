---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
title: 設定資料存取層的連接和命令層級設定 (VB) |Microsoft Docs
author: rick-anderson
description: 在輸入資料集內 TableAdapters 自動處理連接到資料庫，發出命令，並填入具有結果的 DataTable...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: d57dfa2b-d627-45cb-b5b1-abbf3159d770
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-vb
msc.type: authoredcontent
ms.openlocfilehash: d44372ef3eaf7634d3bf3a82bd2c1eb1d710f786
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834242"
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-vb"></a>設定資料存取層的連接和命令層級設定 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_VB.zip)或[下載 PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/datatutorial72vb1.pdf)

> 在輸入資料集內 TableAdapters 自動處理連接到資料庫，發出命令，並填入具有結果的 DataTable。 不過，當我們想要處理這些詳細資料，並在本教學課程中我們了解如何存取在 TableAdapter 中的資料庫連接和命令層級設定，有些情況。


## <a name="introduction"></a>簡介

在整個教學課程系列中，我們有使用具類型資料集來實作資料存取層和商務物件的多層式架構。 中所述[第一個教學課程](../introduction/creating-a-data-access-layer-vb.md)，s DataTables 作為存放庫的資料而 TableAdapters 做為包裝函式與要擷取和修改基礎資料的資料庫進行通訊的具類型資料集。 Tableadapter 封裝使用之資料庫的複雜度，並讓我們不必撰寫程式碼以連接到資料庫、 發出命令，或將結果填入 DataTable。

有些的時候，不過，當我們需要的 TableAdapter burrow 撰寫直接與 ADO.NET 物件搭配運作的程式碼。 在 [資料庫修改包裝在交易](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)教學課程中，比方說，我們加入方法開始、 認可和回復 ADO.NET 交易的 TableAdapter。 這些方法使用的內部，以手動方式建立`SqlTransaction`物件已指派給 tableadapter`SqlCommand`物件。

在本教學課程中，我們將檢查如何存取在 TableAdapter 中的資料庫連接和命令層級設定。 特別是，我們會將功能新增至`ProductsTableAdapter`啟用存取基礎的連接字串，以及命令逾時設定。

## <a name="working-with-data-using-adonet"></a>有關使用 ADO.NET 的資料

Microsoft.NET Framework 包含了多種專門設計來處理資料的類別。 這些類別中找到[`System.Data`命名空間](https://msdn.microsoft.com/library/system.data.aspx)，稱為*ADO.NET*類別。 部分下 ADO.NET 傘狀類別會繫結至特定*資料提供者*。 您可以將資料提供者，視為可讓資訊的 ADO.NET 類別與基礎資料存放區之間的通訊通道。 有一般化的提供者，例如 OleDb 和 ODBC，以及專為特定資料庫系統的提供者。 比方說，雖然您可以連接到 Microsoft SQL Server 資料庫使用 OleDb 提供者，SqlClient 提供者為更有效率的設計和最佳化特別適用於 SQL Server。

當以程式設計方式存取資料時，會通常會使用下列模式：

1. 連接到資料庫。
2. 發出命令。
3. 針對`SELECT`查詢處理產生的記錄。

有個別的 ADO.NET 類別來執行每個步驟。 若要連接至資料庫，使用 SqlClient 提供者，例如，使用[`SqlConnection`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx)。 要發出`INSERT`， `UPDATE`， `DELETE`，或`SELECT`資料庫，請使用命令[`SqlCommand`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)。

除了[資料庫修改包裝在交易](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-vb.md)教學課程中，我們尚未將任何低階 ADO.NET 程式碼，自己因為 TableAdapters 自動產生的程式碼包含所需的功能連接到資料庫，發出命令，和擷取資料，填入 Datatable 中的該資料。 不過，可能的有時候我們會需要來自訂這些低層級的設定。 透過接下來的步驟中，我們將檢驗如何善用內部使用的 Tableadapter 的 ADO.NET 物件。

## <a name="step-1-examining-with-the-connection-property"></a>步驟 1： 檢查連接屬性

每個 TableAdapter 類別有`Connection`屬性，指定資料庫連接資訊。 這個屬性的資料型別和`ConnectionString`值取決於在 [TableAdapter 組態精靈] 所做的選擇。 您應該記得，我們先將 TableAdapter 加入具類型資料集時此精靈會要求我們資料庫來源 （請參閱 圖 1）。 在此步驟中第一個下拉式清單包含這些組態檔，以及任何其他資料庫在伺服器總管 中的資料連接中指定的資料庫。 如果我們想要使用的資料庫不存在於下拉式清單中，可以藉由按一下 [新增連接] 按鈕，並提供所需的連接資訊指定新的資料庫連接。


[![[TableAdapter 組態精靈] 的第一個步驟](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image1.png)

**圖 1**: [TableAdapter 組態精靈] 的第一個步驟 ([按一下以檢視完整大小的影像](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image3.png))


可讓 s 花點時間檢查 tableadapter 的程式碼`Connection`屬性。 如中所述[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，我們可以檢視自動產生的 TableAdapter 程式碼移至 [類別檢視] 視窗中，向下適當的類別，鑽研，然後按兩下成員名稱。

當手寫筆移至 檢視 功能表並選擇 類別檢視 （或按 Ctrl + Shift + C），瀏覽至 類別檢視 視窗。 從 [類別檢視] 視窗的上半部，向下切入至`NorthwindTableAdapters`命名空間，然後選取`ProductsTableAdapter`類別。 這會顯示`ProductsTableAdapter`的成員在底部 類別檢視中，如 圖 2 所示的下半部。 按兩下`Connection`屬性，以查看其程式碼。


![按兩下以檢視其自動產生的程式碼的 [類別] 檢視中的連接屬性](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image4.png)

**圖 2**： 按兩下以檢視其自動產生的程式碼的 類別 檢視中的連接屬性


Tableadapter`Connection`屬性和其他連接相關的程式碼如下所示：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample1.vb)]

TableAdapter 類別具現化時，成員變數`_connection`等於`Nothing`。 當`Connection`存取屬性時，它會先檢查是否`_connection`已具現化成員變數。 如果沒有，`InitConnection`方法會叫用，以具現化`_connection`並設定其`ConnectionString`TableAdapter 組態精靈 s 第一個步驟中指定的連接字串值的屬性。

`Connection`屬性也可以指派給`SqlConnection`物件。 這種方式建立關聯的新`SqlConnection`物件與每個 tableadapter`SqlCommand`物件。

## <a name="step-2-exposing-connection-level-settings"></a>步驟 2： 公開的連接層級設定

連接資訊應該維持封裝內的 TableAdapter，並無法存取應用程式架構中的其他圖層。 不過，可能有案例當 TableAdapter 的連接層級資訊必須可存取或可自訂的查詢、 使用者或 ASP.NET 網頁。

讓擴充的 s`ProductsTableAdapter`中`Northwind`要包含的資料集`ConnectionString`可以由商務邏輯層用來讀取或變更使用的 TableAdapter 的連接字串的屬性。

> [!NOTE]
> A*連接字串*是字串，指定資料庫連接資訊，例如要使用時，資料庫、 驗證認證和其他資料庫相關的設定位置的提供者。 如需各種不同的資料存放區和提供者所使用的連接字串模式的清單，請參閱 < [ConnectionStrings.com](http://www.connectionstrings.com/)。


中所述[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)教學課程中，輸入資料集的自動產生類別可加以擴充透過部分類別使用。 首先，建立名為專案中的 新的子資料夾`ConnectionAndCommandSettings`下方`~/App_Code/DAL`資料夾。


![新增名為 ConnectionAndCommandSettings 子資料夾](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image5.png)

**圖 3**： 新增名為的子資料夾 `ConnectionAndCommandSettings`


加入新的類別檔案，名為`ProductsTableAdapter.ConnectionAndCommandSettings.vb`並輸入下列程式碼：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample2.vb)]

此部分類別加入`Public`名為屬性`ConnectionString`到`ProductsTableAdapter`類別，可允許讀取或更新基礎連接的 tableadapter 的連接字串的任何一層。

建立使用此部分類別 （及儲存），開啟`ProductsBLL`類別。 請移至其中一個現有的方法，並輸入`Adapter`然後按下句號鍵，以顯示 IntelliSense。 您應該會看到新`ConnectionString`IntelliSense，這表示您可以透過程式設計方式讀取或調整此值從 BLL 中可用的屬性。

## <a name="exposing-the-entire-connection-object"></a>公開整個連線物件

這個部分的類別會公開基礎連接物件的一個屬性： `ConnectionString`。 如果您想要讓整個連線物件超過 TableAdapter 的範圍內，您也可以變更`Connection`屬性的保護層級。 我們在步驟 1 中檢查的自動產生程式碼所示範的 tableadapter`Connection`屬性標示為`Friend`，這表示，它只能存取相同的組件中的類別。 這可以變更，不過，透過 tableadapter`ConnectionModifier`屬性。

開啟`Northwind`資料集，按一下 `ProductsTableAdatper`在設計師中，並瀏覽至 屬性 視窗。 您會看到`ConnectionModifier`設為預設值， `Assembly`。 若要讓`Connection`外部輸入資料集 s 組件，變更才可以使用屬性`ConnectionModifier`屬性設`Public`。


[![您可以透過 ConnectionModifier 屬性設定連接屬性 s 存取範圍層級](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image6.png)

**圖 4**:`Connection`屬性存取範圍可以設定層級的 s 透過`ConnectionModifier`屬性 ([按一下以檢視完整大小的影像](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/_static/image8.png))


儲存的資料集，然後再傳回給`ProductsBLL`類別。 之前，請移至其中一個現有的方法，並輸入`Adapter`然後按下句號鍵，以顯示 IntelliSense。 此清單應包含`Connection`屬性，這表示，您可以現在以程式設計方式讀取或從 BLL 指派任何連接層級設定。

## <a name="step-3-examining-the-command-related-properties"></a>步驟 3： 檢查與命令相關的屬性

TableAdapter 所組成的主要查詢，根據預設，已自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式。 此主要查詢 s `INSERT`， `UPDATE`，並`DELETE`陳述式中的 TableAdapter s 程式碼實作為 ADO.NET 資料配接器物件透過`Adapter`屬性。 如同其`Connection`屬性，`Adapter`屬性的資料型別取決於使用的資料提供者。 由於這些教學課程使用 SqlClient 提供者`Adapter`屬性的類型是[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx)。

Tableadapter`Adapter`屬性有三個屬性的型別`SqlCommand`，它會使用問題`INSERT`， `UPDATE`，和`DELETE`陳述式：

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A`SqlCommand`物件會負責傳送至資料庫的特定查詢，其屬性，例如： [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx)，其中包含的特定 SQL 陳述式或預存程序來執行; 和[ `Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx)，這是集合的`SqlParameter`物件。 如我們所見年代[建立資料存取層](../introduction/creating-a-data-access-layer-vb.md)這些教學課程中，命令可以自訂物件，透過 [屬性] 視窗。

除了其主要的查詢，TableAdapter 還可以包含變動數目的方法，叫用時，將分派到資料庫指定的命令。 主要查詢的命令物件和所有其他方法的命令物件會儲存在 tableadapter`CommandCollection`屬性。

可讓 s 花點時間查看所產生的程式碼`ProductsTableAdapter`在`Northwind`這兩個屬性及其支援的成員變數和 helper 方法的資料集：


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample3.vb)]

程式碼`Adapter`並`CommandCollection`屬性精確地模擬的`Connection`屬性。 沒有保存的屬性所使用之物件的成員變數。 屬性`Get`存取子會開始檢查是否有相對應的成員變數`Nothing`。 如果是這樣，初始設定方法被呼叫建立成員變數的執行個體，並會將指派的核心命令相關的屬性。

## <a name="step-4-exposing-command-level-settings"></a>步驟 4︰ 公開命令層級設定

在理想情況下，命令層級資訊應該維持不封裝內的資料存取層。 應該架構的其他圖層會需要此資訊，不過，它就可以公開透過部分類別中，就像連接層級設定。

因為 TableAdapter 只有單一`Connection`屬性公開連接層級設定的程式碼是相當簡單。 TableAdapter 可以有多個命令物件，因為修改命令層級設定時，項目會稍微複雜`InsertCommand`， `UpdateCommand`，並`DeleteCommand`，以及命令中的物件數目可變`CommandCollection`屬性。 在更新命令層級設定時，這些設定需要傳播到所有的命令物件。

例如，假設花了異常的長時間執行的 TableAdapter 沒有特定的查詢。 當執行上述其中一個查詢中使用的 TableAdapter，我們可能會想要增加命令物件 s [ `CommandTimeout`屬性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)。 這個屬性會指定等待命令執行的秒數，且預設為 30。

若要允許`CommandTimeout`屬性，以調整 BLL，新增下列`Public`方法來`ProductsDataTable`步驟 2 中使用的部分類別檔建立 (`ProductsTableAdapter.ConnectionAndCommandSettings.vb`):


[!code-vb[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-vb/samples/sample4.vb)]

該 TableAdapter 的執行個體無法從 BLL 或展示層設定命令的所有問題的命令逾時叫用這個方法。

> [!NOTE]
> `Adapter`並`CommandCollection`屬性會標示為`Private`，這表示他們只能存取從 TableAdapter 中的程式碼。 不同於`Connection`屬性，這些存取修飾詞無法進行任何設定。 因此，如果您要公開到其他層架構中的命令層級屬性，您必須使用以上面所討論的部分類別方法`Public`方法或屬性，可讀取或寫入`Private`命令物件。


## <a name="summary"></a>總結

在輸入資料集內 TableAdapters 提供給封裝資料存取的詳細資料和複雜性。 使用 Tableadapter，我們就不必擔心撰寫 ADO.NET 程式碼，以連接到資料庫、 發出命令，或將結果填入 DataTable。 它是所有處理會自動為我們。

不過，有時候可能會當我們需要自訂的低層級的 ADO.NET 細節，例如變更連接字串或預設的連接或命令逾時值。 TableAdapter 已自動產生`Connection`， `Adapter`，並`CommandCollection`屬性，但這些都是`Friend`或`Private`，根據預設。 此內部的資訊可以由擴充 TableAdapter 使用部分類別來包含`Public`方法或屬性。 或者，tableadapter`Connection`屬性存取修飾詞可以設定透過 TableAdapter 的`ConnectionModifier`屬性。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 Burnadette Leigh，S ren Jacob Lauritsen Teresa Murphy 和 Hilton Geisenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](working-with-computed-columns-vb.md)
> [下一頁](protecting-connection-strings-and-other-configuration-information-vb.md)
