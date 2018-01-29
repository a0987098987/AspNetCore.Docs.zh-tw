---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
title: "設定資料存取層連接和命令層級設定 (C#) |Microsoft 文件"
author: rick-anderson
description: "在輸入資料集內 Tableadapter 自動處理連接到資料庫中，發出命令，並填入具有結果的 DataTable..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: cd330dd9-6254-4305-9351-dd727384c83b
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/configuring-the-data-access-layer-s-connection-and-command-level-settings-cs
msc.type: authoredcontent
ms.openlocfilehash: be81bde63d66c3a7070f31be830f7d10ba3a5f8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-data-access-layers-connection--and-command-level-settings-c"></a>設定資料存取層連接和命令層級設定 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_72_CS.zip)或[下載 PDF](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/datatutorial72cs1.pdf)

> 在輸入資料集內 Tableadapter 自動處理連接到資料庫中，發出命令，並填入具有結果的 DataTable。 不過，當我們想要小心，這些詳細資料，以及在本教學課程中我們會了解如何存取資料庫連接和命令層級設定，在 TableAdapter 中沒有的情況。


## <a name="introduction"></a>簡介

在整個教學課程系列我們已實作的資料存取層和我們多層式架構商務物件使用型別資料集。 中所述[第一個教學課程](../introduction/creating-a-data-access-layer-cs.md)，s Datatable 會做為儲存機制的資料，而 Tableadapter 做為包裝函式與要擷取和修改基礎資料的資料庫進行通訊的型別資料集。 Tableadapter 封裝使用資料庫的複雜度，並將我們儲存不必撰寫程式碼以連接到資料庫、 發出命令，或填入 DataTable 的結果。

有的時間，不過，當我們需要 burrow TableAdapter 複製撰寫的程式碼直接使用的 ADO.NET 物件。 在[包裝在交易內的資料庫修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)教學課程中，例如，我們加入方法開始、 認可，以及回復 ADO.NET 交易 TableAdapter。 這些方法使用的內部，手動建立`SqlTransaction`物件指派給 tableadapter`SqlCommand`物件。

在本教學課程中，我們將檢查如何存取在 TableAdapter 中的資料庫連接和命令層級設定。 特別是，我們會將功能加入至`ProductsTableAdapter`啟用存取基礎的連接字串，以及命令逾時設定。

## <a name="working-with-data-using-adonet"></a>使用 ADO.NET 資料搭配使用

Microsoft.NET Framework 包含特別設計來處理資料的類別上的。 這些內找到的類別[`System.Data`命名空間](https://msdn.microsoft.com/library/system.data.aspx)，稱為*ADO.NET*類別。 某些 ADO.NET 概括性下類別會繫結至特定*資料提供者*。 您可以視為資料提供者可讓資訊 ADO.NET 類別與基礎資料存放區之間的通訊通道。 有一般化的提供者，例如 OleDb 和 ODBC，以及專為特定資料庫系統的提供者。 例如，雖然可以連接到 Microsoft SQL Server 資料庫使用 OleDb 提供者，SqlClient 提供者會更有效率設計並最佳化特別適用於 SQL Server。

當以程式設計方式存取資料時，下列的模式通常會：

- 連接到資料庫。
- 發出命令。
- 如`SELECT`查詢，使用產生的記錄。

有不同的 ADO.NET 類別來執行每個步驟。 若要連接至資料庫，使用 SqlClient 提供者，例如，使用[`SqlConnection`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlconnection(VS.80).aspx)。 要發出`INSERT`， `UPDATE`， `DELETE`，或`SELECT`命令到資料庫，使用[`SqlCommand`類別](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.aspx)。

除了[包裝在交易內的資料庫修改](../working-with-batched-data/wrapping-database-modifications-within-a-transaction-cs.md)教學課程中，我們已不需要撰寫任何低階 ADO.NET 程式碼自行因為 Tableadapter 自動產生程式碼包含所需的功能連接到資料庫、 發出命令、 擷取資料，並填入 Datatable 中的該資料。 不過，可能是我們需要來自訂這些低層級的設定。 在接下來的幾個步驟中，我們將檢查如何挖掘內部使用的 Tableadapter 的 ADO.NET 物件。

## <a name="step-1-examining-with-the-connection-property"></a>步驟 1： 檢查連接屬性

每個 TableAdapter 類別具有`Connection`屬性，指定資料庫連接資訊。 這個屬性的資料型別和`ConnectionString`值取決於在 TableAdapter 組態精靈所做的選擇。 前文提過，我們先將 TableAdapter 加入型別資料集時此精靈會詢問我們資料庫來源 （請參閱圖 1）。 在此步驟中第一個下拉式清單包含這些組態檔，以及在伺服器總管的資料連接中的任何其他資料庫中指定的資料庫。 如果下拉式清單中，我們想要使用的資料庫不存在，可以透過按一下 [新增連接] 按鈕，並提供所需的連接資訊指定新的資料庫連接。


[![[TableAdapter 組態精靈] 的第一個步驟](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image2.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image1.png)

**圖 1**: TableAdapter 組態精靈 的第一個步驟 ([按一下以檢視完整大小的影像](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image3.png))


可讓 s 花點時間 tableadapter 的程式碼檢查`Connection`屬性。 如中所述[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，我們可以檢視自動產生的 TableAdapter 程式碼移至 [類別檢視] 視窗中，向下適當的類別，鑽研，然後按兩下 成員名稱。

移至 檢視 功能表，然後選擇 類別檢視 （或輸入 Ctrl + Shift + C），請瀏覽 類別檢視 視窗。 從 [類別檢視] 視窗的上半部，向下鑽研至`NorthwindTableAdapters`命名空間並選取`ProductsTableAdapter`類別。 這會顯示`ProductsTableAdapter`的成員，在 類別檢視中，如圖 2 所示的下半部。 按兩下`Connection`屬性來查看其程式碼。


![按兩下要檢視其自動產生程式碼的類別檢視中的連接屬性](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image4.png)

**圖 2**： 按兩下要檢視其自動產生程式碼的類別檢視中的連接屬性


Tableadapter`Connection`屬性和其他連接相關的程式碼如下所示：


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample1.cs)]

TableAdapter 類別具現化時，成員變數`_connection`等於`null`。 當`Connection`存取屬性時，它會先檢查，看看是否`_connection`已具現化成員變數。 如果沒有，`InitConnection`叫用方法時，它會具現化`_connection`並設定其`ConnectionString`TableAdapter 組態精靈 s 第一個步驟中指定的連接字串值的屬性。

`Connection`屬性也可以指派給`SqlConnection`物件。 如此一來將新`SqlConnection`物件與每個 tableadapter`SqlCommand`物件。

## <a name="step-2-exposing-connection-level-settings"></a>步驟 2： 公開連接層級設定

連接資訊應該維持封裝內的 TableAdapter，並無法存取應用程式架構的其他圖層。 不過，可能有案例時必須是可存取或可自訂的查詢、 使用者或 ASP.NET 網頁的 TableAdapter 的連接層級資訊。

可讓擴充的 s`ProductsTableAdapter`中`Northwind`要包含的資料集`ConnectionString`可以由商務邏輯層用來讀取或變更使用 TableAdapter 的連接字串屬性。

> [!NOTE]
> A*連接字串*是字串，指定資料庫連接資訊，例如若要使用，資料庫、 驗證認證，以及其他資料庫相關設定的位置提供者。 如需使用的各種不同的資料存放區和提供者的連接字串模式的清單，請參閱[ConnectionStrings.com](http://www.connectionstrings.com/)。


中所述[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)教學課程中，可以透過使用部分類別擴充的型別資料集的自動產生類別。 首先，建立新的子資料夾中名為的專案`ConnectionAndCommandSettings`下方`~/App_Code/DAL`資料夾。


![加入名為 ConnectionAndCommandSettings 子資料夾](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image5.png)

**圖 3**： 加入子資料夾名為`ConnectionAndCommandSettings`


加入新的類別檔案命名為`ProductsTableAdapter.ConnectionAndCommandSettings.cs`並輸入下列程式碼：


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample2.cs)]

這個部分的類別會加入`public`屬性名為`ConnectionString`至`ProductsTableAdapter`類別，可讓讀取或更新基礎連接的 tableadapter 的連接字串的任何圖層。

建立使用此部分類別 （並儲存），開啟`ProductsBLL`類別。 移至其中一個現有的方法，在輸入`Adapter`然後按下以顯示 IntelliSense 期間的索引鍵。 您應該會看到新`ConnectionString`IntelliSense，這表示您可以透過程式設計方式讀取或調整此值從 BLL 中可用的屬性。

## <a name="exposing-the-entire-connection-object"></a>公開的整個連線物件

此部分的類別會公開基礎連接物件的一個屬性： `ConnectionString`。 如果您想要提供超過 TableAdapter 的範圍內的整個連線物件，您也可以變更`Connection`屬性的保護層級。 我們在步驟 1 中檢查的自動產生程式碼示範的 tableadapter`Connection`屬性標記為`internal`，也就是說，它只能存取由相同組件中的類別。 此項可能變更，不過，透過 tableadapter`ConnectionModifier`屬性。

開啟`Northwind`資料集，請按一下 `ProductsTableAdatper`在設計師中，並瀏覽至 屬性 視窗。 您會看到那里`ConnectionModifier`設為預設值， `Assembly`。 讓`Connection`屬性型別資料集 s 組件，變更之外使用`ConnectionModifier`屬性`Public`。


[![透過 ConnectionModifier 屬性可以設定連接屬性 s 存取範圍層級](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image7.png)](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image6.png)

**圖 4**:`Connection`屬性存取範圍可以設定層級的 s 透過`ConnectionModifier`屬性 ([按一下以檢視完整大小的影像](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/_static/image8.png))


儲存資料集，然後返回`ProductsBLL`類別。 之前，請移至其中一個現有的方法，並輸入`Adapter`然後按下以顯示 IntelliSense 期間的索引鍵。 清單應該包括`Connection`屬性，這表示您可以現在以程式設計方式讀取，或從 BLL 指派任何連接層級設定。

## <a name="step-3-examining-the-command-related-properties"></a>步驟 3： 檢查命令相關的屬性

TableAdapter 所組成的主要查詢，根據預設，已自動產生`INSERT`， `UPDATE`，和`DELETE`陳述式。 此主要查詢 s `INSERT`， `UPDATE`，和`DELETE`陳述式的 TableAdapter s 程式碼中實作以 ADO.NET 資料配接器物件透過`Adapter`屬性。 像使用其`Connection`屬性，`Adapter`屬性的資料類型取決於使用的資料提供者。 這些教學課程都使用 SqlClient 提供者，因為`Adapter`屬性屬於型別[ `SqlDataAdapter` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqldataadapter(VS.80).aspx)。

Tableadapter`Adapter`屬性有三個屬性的型別`SqlCommand`，它會使用問題`INSERT`， `UPDATE`，和`DELETE`陳述式：

- `InsertCommand`
- `UpdateCommand`
- `DeleteCommand`

A`SqlCommand`物件負責傳送至資料庫的特定查詢，其屬性，例如： [ `CommandText` ](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtext.aspx)，其中包含的特定 SQL 陳述式或預存程序，才可執行，並[ `Parameters`](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.parameters.aspx)，這是集合的`SqlParameter`物件。 如我們所見回[建立資料存取層](../introduction/creating-a-data-access-layer-cs.md)這些教學課程中，命令物件可以透過 [屬性] 視窗加以自訂。

除了其主要的查詢，TableAdapter 還可以包含多個方法，叫用時，會分派至資料庫指定的命令。 主要查詢的命令物件和所有其他方法的命令物件會儲存在 tableadapter`CommandCollection`屬性。

可讓 s 花點時間查看所產生的程式碼`ProductsTableAdapter`中`Northwind`這兩個屬性及其支援的成員變數和 helper 方法的資料集：


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample3.cs)]

程式碼`Adapter`和`CommandCollection`屬性慎的`Connection`屬性。 沒有保存物件內容所使用的成員變數。 屬性`get`開始，檢查是否對應的成員變數的存取子`null`。 如果是這樣，初始化呼叫執行方法時建立成員變數的執行個體，並指派核心與命令相關的屬性。

## <a name="step-4-exposing-command-level-settings"></a>步驟 4： 公開命令層級的設定

在理想情況下，命令層級資訊應該維持不在資料存取層中的封裝。 應該架構的其他圖層會需要此資訊，不過，它可以透過公開部分類別中，就像使用的連接層級設定。

因為 TableAdapter 只能有單一`Connection`屬性公開連接層級設定的程式碼是相當簡單。 TableAdapter 可以有多個命令物件，因為修改命令層級設定時事情就會稍微複雜`InsertCommand`， `UpdateCommand`，和`DeleteCommand`，以及變數命令中的物件數目`CommandCollection`屬性。 當更新命令層級設定，這些設定必須傳播到所有的命令物件。

例如，假設花費了異常長時間執行的 TableAdapter 中發生的特定查詢。 當使用 TableAdapter 執行這些查詢的其中一個，我們可能會想要增加命令物件 s [ `CommandTimeout`屬性](https://msdn.microsoft.com/library/system.data.sqlclient.sqlcommand.commandtimeout.aspx)。 這個屬性會指定無限等候命令執行的秒數，且預設值為 30。

若要允許`CommandTimeout`屬性，以調整 BLL，加入下列`public`方法`ProductsDataTable`使用部分類別檔案在步驟 2 中建立 (`ProductsTableAdapter.ConnectionAndCommandSettings.cs`):


[!code-csharp[Main](configuring-the-data-access-layer-s-connection-and-command-level-settings-cs/samples/sample4.cs)]

這個方法無法 TableAdapter 執行個體所叫用從 BLL 或展示層設定的所有命令問題的命令逾時之用。

> [!NOTE]
> `Adapter`和`CommandCollection`屬性標記為`private`，這表示他們只能存取從內 TableAdapter 的程式碼。 不同於`Connection`屬性，這些存取修飾詞不是可設定。 因此，如果您要公開命令層級屬性，以在架構中的其他圖層必須使用上述提供的部分類別方法`public`方法或屬性，可讀取或寫入`private`命令物件。


## <a name="summary"></a>總結

Tableadapter 在輸入資料集內提供的服務封裝資料存取的詳細資料和複雜性。 使用 Tableadapter，我們不必擔心撰寫 ADO.NET 程式碼，以連接到資料庫、 發出命令，或填入 DataTable 的結果。 它是所有處理會自動為我們。

不過，可能是我們需要自訂低階的 ADO.NET 特性，例如變更連接字串或預設連線或命令逾時值。 TableAdapter 已自動產生`Connection`， `Adapter`，和`CommandCollection`屬性，但這些都是`internal`或`private`，根據預設。 此內部的資訊可以由擴充 TableAdapter 使用部分類別包含`public`方法或屬性。 或者，tableadapter`Connection`透過 tableadapter 可以設定屬性的存取修飾詞`ConnectionModifier`屬性。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 導致檢閱者在此教學課程已 Burnadette Leigh，S ren 因此 Lauritsen 本文菲和 Hilton Geisenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一頁](working-with-computed-columns-cs.md)
[下一頁](protecting-connection-strings-and-other-configuration-information-cs.md)
