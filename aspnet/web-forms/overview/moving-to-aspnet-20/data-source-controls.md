---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: 資料來源控制項 |Microsoft Docs
author: microsoft
description: DataGrid 控制項在 ASP.NET 1.x 標示為 Web 應用程式中的資料存取的一大進步。 不過，它不是易於使用，可能是...
ms.author: riande
ms.date: 02/20/2005
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: ba00024e93beba6eab226dd0d381d8734061e095
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41827241"
---
<a name="data-source-controls"></a>資料來源控制項
====================
by [Microsoft](https://github.com/microsoft)

> DataGrid 控制項在 ASP.NET 1.x 標示為 Web 應用程式中的資料存取的一大進步。 不過，它不是因為可能是使用者的易記。 此外，它仍會需要相當多的程式碼，以從它取得太多有用的功能。 滿足下列條件是所有資料存取工作領域能夠一路順風 1.x 中的模型。


DataGrid 控制項在 ASP.NET 1.x 標示為 Web 應用程式中的資料存取的一大進步。 不過，它不是因為可能是使用者的易記。 此外，它仍會需要相當多的程式碼，以從它取得太多有用的功能。 滿足下列條件是所有資料存取工作領域能夠一路順風 1.x 中的模型。

ASP.NET 2.0 可應付此部分資料來源控制項。 在 ASP.NET 2.0 中的資料來源控制項提供開發人員宣告式模型來擷取資料，顯示資料，並編輯資料。 資料來源控制項的用途是提供一致的這些資料來源無關的資料繫結控制項的資料表示法。 在 ASP.NET 2.0 中的資料來源控制項的核心是 DataSourceControl 抽象類別。 DataSourceControl 類別提供基底實作 IDataSource 介面和 IListSource 介面，後者可讓您指派為資料繫結控制項 （透過新的 [DataSourceId] 屬性的資料來源的資料來源控制項稍後討論），並公開資料投影為清單。 每個資料來源控制項的資料清單會公開為 DataSourceView 物件。 DataSourceView 執行個體的存取是 IDataSource 介面所提供。 比方說，GetViewNames 方法會傳回相關聯的特定資料來源控制項，可讓您列舉 DataSourceViews ICollection 和 GetView 方法可讓您依名稱存取特定的 DataSourceView 執行個體。

資料來源控制項有沒有使用者介面。 它們會實作做為伺服器控制項，讓它們可以支援宣告式語法，並使他們將可以存取頁面狀態，如有需要。 資料來源控制項不呈現到用戶端的任何 HTML 標記。

> [!NOTE]
> 因為您稍後所見，那里要也快取使用資料來源控制項所取得的優點。


## <a name="storing-connection-strings"></a>將連接字串儲存

我們看看如何設定資料來源控制項之前，我們應該涵蓋在 ASP.NET 2.0 中有關連接字串的新功能。 ASP.NET 2.0 導入了新的區段，可讓您輕鬆地儲存連接字串，可以在執行階段以動態方式讀取組態檔中。 &lt;ConnectionStrings&gt;  區段可以讓您輕鬆地將連接字串儲存。

下列程式碼片段會新增新的連接字串。

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> 如同&lt;appSettings&gt;區段中， &lt;connectionStrings&gt;之外的區段會出現&lt;system.web&gt;組態檔中的區段。


若要使用此連接字串，您可以使用下列語法，設定伺服器控制項的 ConnectionString 屬性時。

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt;區段也會加密，因此不會公開機密資訊。 在更新版本的模組，將會說明這項功能。

## <a name="caching-data-sources"></a>快取的資料來源

每個 DataSourceControl 會提供四個屬性來設定快取;EnableCaching、 CacheDuration、 CacheExpirationPolicy 和 CacheKeyDependency。

## <a name="enablecaching"></a>EnableCaching

EnableCaching 是布林值屬性，決定快取已啟用資料來源控制項。

## <a name="cacheduration-property"></a>CacheDuration 屬性

CacheDuration 屬性會設定快取會維持有效狀態的秒數。 將此屬性設定為**0**會導致要保持有效，直到明確失效的快取。

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy 屬性

CacheExpirationPolicy 屬性可以設定為**絕對**或是**滑動**。 請將它設定為絕對最大快取的資料的時間量是 CacheDuration 屬性所指定的秒數表示。 藉由將它設定為 滑動，每個作業執行時，會重設到期時間。

## <a name="cachekeydependency-property"></a>CacheKeyDependency 屬性

如果 CacheKeyDependency 屬性指定的字串值，ASP.NET 會設定新的快取相依性，該字串為基礎。 這可讓您明確地只變更或移除 CacheKeyDependency 確認快取。

**重要**： 如果已啟用模擬，且存取的資料來源及/或資料的內容，根據用戶端身分識別，則建議 EnableCaching 設定為 False，則快取停用。 如果已啟用快取在此案例中，而且非原本要求資料之使用者的使用者發出的要求，不會強制執行資料來源的授權。 從快取，將只會提供資料。

## <a name="the-sqldatasource-control"></a>SqlDataSource 控制項

SqlDataSource 控制項可讓開發人員存取支援 ADO.NET 的任何關聯式資料庫中儲存的資料。 它可用來存取 SQL Server 資料庫、 System.Data.OleDb 提供者、 System.Data.Odbc 提供者或 System.Data.OracleClient 提供者來存取 Oracle System.Data.SqlClient 提供者。 因此，SqlDataSource 當然不只用於存取 SQL Server 資料庫中的資料。

若要使用 SqlDataSource，，只要提供 ConnectionString 屬性的值和指定的 SQL 命令或預存程序。 SqlDataSource 控制項負責使用基礎 ADO.NET 架構。 它會開啟連接、 查詢資料來源或執行預存程序、 傳回的資料，，然後關閉您的連接。

> [!NOTE]
> 因為 DataSourceControl 類別會自動關閉您的連線，它應該減少產生的資料庫連線流失的客戶來電數目。


下列程式碼片段中，將繫結 DropDownList 控制項至 SqlDataSource 控制項使用的連接字串儲存在如上所示的組態檔中。

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

如以上所述，SqlDataSource DataSourceMode 屬性會指定資料來源的模式。 在上述範例中，DataSourceMode 設為 DataReader。 在此情況下，SqlDataSource 會傳回 IDataReader 物件使用順向且唯讀資料指標。 傳回物件的指定的型別是由使用提供者控制。 在此情況下，我使用 System.Data.SqlClient 的提供者中所指定&lt;connectionStrings&gt; web.config 檔案區段。 因此，會傳回的物件將會是類型 SqlDataReader。 藉由指定 DataSourceMode 值的資料集，資料可以儲存在伺服器上的資料集。 此模式可讓您新增功能，例如排序、 分頁等等。如果我已經資料繫結到 GridView 控制項 SqlDataSource，我會選擇資料集模式。 不過，在下拉式清單中，DataReader 模式是正確的選擇。

> [!NOTE]
> 當快取 SqlDataSource 或 AccessDataSource DataSourceMode 屬性必須設定資料集。 如果您啟用使用 DataReader DataSourceMode 的快取，則會發生例外狀況。


## <a name="sqldatasource-properties"></a>SqlDataSource 屬性

以下是一些 SqlDataSource 控制項的屬性。

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

布林值，指定是否其中一個參數為 null，是否要取消選取的命令。 預設值為 true。

### <a name="conflictdetection"></a>ConflictDetection

在其中多個使用者可能會更新資料來源在同一時間的情況下，ConflictDetection 屬性會決定 SqlDataSource 控制項的行為。 這個屬性評估為其中一個 ConflictOptions 列舉型別的值。 這些值為**CompareAllValues**並**OverwriteChanges**。 如果設為 OverwriteChanges，將資料寫入至資料來源的最後一個人會覆寫任何先前的變更。 不過，如果 ConflictDetection 屬性設定為 CompareAllValues，參數會建立 SelectCommand 所傳回的資料行，且參數也會建立來保存每個允許以 sqldatasource 進行的資料行中的原始值判斷值已變更，因為 SelectCommand 執行。

### <a name="deletecommand"></a>DeleteCommand

設定或取得從資料庫刪除資料列時使用的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="deletecommandtype"></a>DeleteCommandType

設定或取得 delete 命令的類型是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="deleteparameters"></a>DeleteParameters

傳回 DeleteCommand SqlDataSourceView 相關聯的物件使用 SqlDataSource 控制項所使用的參數。

### <a name="oldvaluesparameterformatstring"></a>OldValuesParameterFormatString

這個屬性用來指定在其中 ConflictDetection 屬性設定為 CompareAllValues 的情況下的原始值參數的格式。 預設值是{0}表示原始值的參數會與原始的參數同名。 換句話說，如果 EmployeeID 欄位名稱，原始的值，參數會是@EmployeeID。

### <a name="selectcommand"></a>SelectCommand

設定或取得用來從資料庫擷取資料的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="selectcommandtype"></a>SelectCommandType

設定或取得的 select 命令，類型是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="selectparameters"></a>SelectParameters

傳回 SelectCommand SqlDataSourceView 相關聯的物件使用 SqlDataSource 控制項所使用的參數。

### <a name="sortparametername"></a>SortParameterName

取得或設定預存程序參數，以供排序資料擷取時資料來源控制項的名稱。 只有在 SelectCommandType 設為預存程序時，才有效。

### <a name="sqlcachedependency"></a>SqlCacheDependency

以分號分隔的字串，指定的資料庫和 SQL Server 快取相依性中使用的資料表。 （更新版本的模組中會討論 SQL 快取相依性）。

### <a name="updatecommand"></a>UpdateCommand

設定或取得更新資料庫中的資料時使用的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="updatecommandtype"></a>UpdateCommandType

設定或取得更新命令的類型是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="updateparameters"></a>UpdateParameters

傳回的 UpdateCommand SqlDataSourceView 相關聯的物件使用 SqlDataSource 控制項所使用的參數。

## <a name="the-accessdatasource-control"></a>AccessDataSource 控制項

AccessDataSource 控制項衍生自 SqlDataSource 類別，以及用來資料繫結至 Microsoft Access 資料庫。 AccessDataSource 控制項上的 ConnectionString 屬性是唯讀屬性。 而不是使用上的 ConnectionString 屬性，資料檔屬性用來指向 Access 資料庫，如下所示。

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource 將的基底 SqlDataSource ProviderName 永遠設 System.Data.OleDb，並連接至使用 Microsoft.Jet.OLEDB.4.0 OLE DB 提供者的資料庫。 您無法使用 AccessDataSource 控制項連接到受密碼保護 Access 資料庫。 如果您有連接到受密碼保護資料庫，您應該使用 SqlDataSource 控制項。

> [!NOTE]
> 儲存在網站內的 access 資料庫應該放在應用程式\_資料目錄。 ASP.NET 不允許所要瀏覽此目錄中的檔案。 您將需要應用程式的讀取和寫入權限授與處理序帳戶\_使用 Access 資料庫時，資料目錄。


## <a name="the-xmldatasource-control"></a>XmlDataSource 控制項

XmlDataSource 用來進行資料繫結的 XML 資料的資料繫結控制項。 您可以繫結至 XML 檔案中使用的資料檔的屬性，或您可以繫結使用的資料屬性的 XML 字串。 XmlDataSource 會公開為可繫結欄位的 XML 屬性。 在您要繫結至不會表示為屬性的值的情況下，您必須使用 XSL 轉換。 您也可以使用篩選的 XML 資料的 XPath 運算式。

請考慮下列的 XML 檔案：

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

請注意，XmlDataSource 使用 XPath 屬性*人/* 若要篩選只&lt;人員&gt;節點。 DropDownList 然後資料繫結至使用 DataTextField 屬性 LastName 屬性。

XmlDataSource 控制項主要是用來資料繫結至唯讀的 XML 資料中，您可編輯 XML 資料檔。 請注意，在此情況下，自動插入、 更新和刪除的 XML 檔案中的資訊不會自動與其他資料來源控制項所顯示的一樣。 相反地，您必須撰寫程式碼來手動編輯使用下列方法的 XmlDataSource 控制項的資料。

### <a name="getxmldocument"></a>GetXmlDocument

擷取 XmlDocument 物件，其中包含 XmlDataSource 所擷取的 XML 程式碼。

### <a name="save"></a>儲存

將記憶體中 XmlDocument 儲存回資料來源。

請務必了解下列兩項條件符合時，將只會運作 Save 方法：

1. XmlDataSource 使用繫結至記憶體中 XML 資料繫結至 XML 檔案而非資料屬性的資料檔屬性。
2. 任何轉換，並透過 轉換 或 TransformFile 屬性不指定。

也請注意 Save 方法可以產生非預期的結果時同時呼叫多個使用者。

## <a name="the-objectdatasource-control"></a>ObjectDataSource 控制項

截至目前為止，我們已涵蓋的資料來源控制項是絕佳的兩層式應用程式，其中的資料來源控制項與外界溝通的直接資料存放區。 不過，許多真實世界應用程式是多層式應用程式資料來源控制項可能需要進行通訊的通訊與資料層商務物件。 在這些情況下，ObjectDataSource 會填滿帳單得很好。 ObjectDataSource 的運作方式與來源物件搭配使用。 ObjectDataSource 控制項將會建立來源物件、 呼叫指定的方法和處置的物件執行個體全部都在範圍內的單一要求的執行個體，如果將物件的執行個體方法，而不是靜態方法 (Visual Basic 中為 Shared)。 因此，您的物件必須是無狀態。 也就是您的物件應該取得和釋放所有必要的資源，在單一要求的範圍內。 您可以控制處理 ObjectCreating 事件的 ObjectDataSource 控制項來源物件建立的方式。 您可以建立的來源物件，執行個體，，然後將該執行個體的 ObjectDataSourceEventArgs 類別的 ObjectInstance 屬性。 ObjectDataSource 控制項將使用建立 ObjectCreating 事件，而不是建立在它自己的執行個體中的執行個體。

如果 ObjectDataSource 控制項來源物件會公開的公用靜態方法 (Visual Basic 中為 Shared) 可以呼叫以擷取和修改資料，ObjectDataSource 控制項就會直接呼叫這些方法。 如果 ObjectDataSource 控制項必須建立來源物件的執行個體，才能進行方法呼叫，該物件必須包含不接受任何參數的公用建構函式。 ObjectDataSource 控制項來源物件的新執行個體建立時，會呼叫這個建構函式。

如果來源物件不包含不含參數的公用建構函式，您可以建立將由 ObjectCreating 事件中的 ObjectDataSource 控制項來源物件的執行個體。

## <a name="specifying-object-methods"></a>指定物件的方法

ObjectDataSource 控制項來源物件可以包含任意數目的方法，用於選取、 插入、 更新或刪除資料。 使用 ObjectDataSource 控制項的 SelectMethod、 InsertMethod、 UpdateMethod、 或 DeleteMethod 屬性所識別，ObjectDataSource 控制項根據方法的名稱，會呼叫這些方法。 來源物件也可以包含選擇性 SelectCount 方法，以使用 SelectCountMethod; 屬性，傳回的資料來源的物件總數計數的 ObjectDataSource 控制項識別。 ObjectDataSource 控制項選取的方法呼叫時，要擷取的資料來源使用的記錄總數分頁之後，會呼叫 SelectCount; 方法。

## <a name="lab-using-data-source-controls"></a>使用資料來源控制項的實驗室

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>練習 1-使用 SqlDataSource 控制項顯示的資料

下列練習中使用 SqlDataSource 控制項連接至 Northwind 資料庫。 這裡假設您的 SQL Server 2000 執行個體上有 Northwind 資料庫的存取權。

1. 建立新的 ASP.NET 網站。
2. 加入新的 web.config 檔案。

    1. 以滑鼠右鍵按一下方案總管] 中的專案，然後按一下 [加入新項目。
    2. 從範本清單中選擇 Web 組態檔，並按一下 新增。
3. 編輯&lt;connectionStrings&gt;區段，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. 切換到程式碼檢視，並新增的 ConnectionString 屬性，以及 SelectCommand 屬性，來&lt;asp: SqlDataSource&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. 從 [設計] 檢視中，加入新的 GridView 控制項。
6. 從 GridView 工作 功能表中選擇資料來源 下拉式清單中，選擇 SqlDataSource1。
7. 以滑鼠右鍵按一下 Default.aspx，並從功能表選擇 在瀏覽器中檢視。 當系統提示您儲存時，請按一下 [是]。
8. GridView 會顯示來自 [產品] 資料表的資料。

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>練習 2-使用 SqlDataSource 控制項的編輯資料

下列練習示範如何為資料繫結 DropDownList 控制項使用的宣告式語法和可讓您編輯 DropDownList 控制項中顯示的資料。

1. 在 [設計] 檢視中，刪除從 Default.aspx 的 GridView 控制項。 

    **重要**： 保留 SqlDataSource 控制項在頁面上。
2. 將 DropDownList 控制項新增至 Default.aspx 中。
3. 切換至來源檢視。
4. DataSourceId、 DataTextField 和 DataValueField 將屬性新增至&lt;asp: DropDownList&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. 儲存 Default.aspx，並在瀏覽器中檢視它。 請注意，DropDownList 包含所有的產品，從 Northwind 資料庫。
6. 關閉瀏覽器。
7. 在 Default.aspx 的來源檢視中，加入新的 TextBox 控制項 DropDownList 控制項下方。 將文字方塊的 ID 屬性變更為 txtProductName。
8. 在 TextBox 控制項中，加入新的按鈕控制項。 將按鈕的 ID 屬性變更為 btnUpdate 和 Text 屬性**更新產品名稱**。
9. 在 Default.aspx 來源檢視，UpdateCommand 屬性和加入兩個新 UpdateParameters SqlDataSource 標記，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > 請注意，有兩個更新參數 （「 產品名稱 」 和 「 ProductID 」） 加入此程式碼中。 這些參數會對應到 txtProductName 文字方塊的 Text 屬性和 ddlProducts DropDownList SelectedValue 屬性。
10. 切換至 [設計] 檢視，並按兩下按鈕控制項加入事件處理常式。
11. 將下列程式碼新增至 btnUpdate\_按一下程式碼： 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. 以滑鼠右鍵按一下 Default.aspx，然後選擇在瀏覽器中檢視它。 當系統提示您儲存所有變更，請按一下 [是]。
13. ASP.NET 2.0 部份類別允許在執行階段編譯。 您不需要建置應用程式以查看程式碼變更，才會生效。
14. 從 DropDownList 中選取產品。
15. 在文字方塊中輸入所選產品的新名稱，然後按一下 [更新] 按鈕。
16. 在資料庫中更新的產品名稱。

## <a name="exercise-3-using-the-objectdatasource-control"></a>練習 3 使用 ObjectDataSource 控制項

本練習將示範如何使用 ObjectDataSource 控制項與來源物件與 Northwind 資料庫互動。

1. 以滑鼠右鍵按一下方案總管 中的專案，然後按一下加入新項目。
2. 在 [範本] 清單中選取 Web 表單。 將名稱變更為 object.aspx 並按一下 [新增]。
3. 以滑鼠右鍵按一下方案總管 中的專案，然後按一下加入新項目。
4. 在 [範本] 清單中選取類別。 將類別名稱變更為 NorthwindData.cs 並按一下 [新增]。
5. 按一下 [是] 會將類別新增至應用程式出現提示時\_程式碼資料夾。
6. NorthwindData.cs 檔案中加入下列程式碼： 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. 將下列程式碼新增至 object.aspx 的來源檢視中： 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. 儲存所有檔案，然後瀏覽 object.aspx。
9. 檢視詳細資料、 編輯員工、 加入員工和刪除員工互動的介面。
