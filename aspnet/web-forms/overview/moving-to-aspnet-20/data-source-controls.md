---
uid: web-forms/overview/moving-to-aspnet-20/data-source-controls
title: "資料來源控制項 |Microsoft 文件"
author: microsoft
description: "DataGrid 控制項 1.x 標記在 ASP.NET Web 應用程式中的資料存取的絕佳改進。 不過，它不是因為它有可能的易記..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2005
ms.topic: article
ms.assetid: 78fd0e92-f9c6-4e96-a5e9-0375b307a828
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/moving-to-aspnet-20/data-source-controls
msc.type: authoredcontent
ms.openlocfilehash: f40189796d3e25e9c337768cf04fdbfa293cdc2f
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="data-source-controls"></a>資料來源控制項
====================
由[Microsoft](https://github.com/microsoft)

> DataGrid 控制項 1.x 標記在 ASP.NET Web 應用程式中的資料存取的絕佳改進。 不過，它不是因為它有可能的易記。 您仍然需要大量的程式碼，以獲得更實用的功能。 就是這種 1.x 中的所有資料存取努力時中的模型。


DataGrid 控制項 1.x 標記在 ASP.NET Web 應用程式中的資料存取的絕佳改進。 不過，它不是因為它有可能的易記。 您仍然需要大量的程式碼，以獲得更實用的功能。 就是這種 1.x 中的所有資料存取努力時中的模型。

ASP.NET 2.0 可應付此部分資料來源控制項。 資料來源控制項，在 ASP.NET 2.0 提供開發人員用來擷取資料，顯示資料，並編輯資料的宣告式模型。 提供一致的資料繫結控制項，不論這些資料來源的資料呈現為資料來源控制項的用途。 在 ASP.NET 2.0 中的資料來源控制項的核心是 DataSourceControl 抽象類別。 DataSourceControl 類別提供的基底實作 IDataSource 介面以及 IListSource 介面，後者可讓您將資料來源控制項指派為資料來源的資料繫結控制項 （透過新的 DataSourceId 屬性稍後再討論），並公開為清單中的資料。 每個清單的資料來源控制項的資料會公開為 DataSourceView 物件。 IDataSource 介面可提供存取權的 DataSourceView 執行個體。 例如，GetViewNames 方法會傳回相關聯的特定資料來源控制項，可讓您列舉 DataSourceViews ICollection 和 GetView 方法可讓您依名稱存取特定的 DataSourceView 執行個體。

資料來源控制項有任何使用者介面。 做為伺服器控制項，讓它們可以支援宣告式語法，並讓他們能存取的頁面狀態視實作。 資料來源控制項不會呈現任何 HTML 標記用戶端。

> [!NOTE]
> 您稍後將會看到，那里要也快取使用資料來源控制項所取得的優勢。


## <a name="storing-connection-strings"></a>儲存連接字串

我們探討如何設定資料來源控制項之前，我們應該涵蓋有關連接字串的 ASP.NET 2.0 的新功能。 ASP.NET 2.0 導入了新的區段，可讓您輕鬆地儲存連接字串，可以在執行階段以動態方式讀取組態檔中。 &lt;ConnectionStrings&gt;  區段可以讓您輕鬆地儲存連接字串。

以下程式碼片段加入新的連接字串。

[!code-xml[Main](data-source-controls/samples/sample1.xml)]

> [!NOTE]
> 就像&lt;appSettings&gt;  區段中， &lt;connectionStrings&gt;區段會出現之外&lt;system.web&gt;組態檔中的區段。


若要使用此連接字串，您可以使用下列語法設定伺服器控制項的 ConnectionString 屬性時。

[!code-aspx[Main](data-source-controls/samples/sample2.aspx)]

&lt;ConnectionStrings&gt;區段也加密，如此不會公開機密資訊。 這項功能都將涵蓋於更新版本的模組。

## <a name="caching-data-sources"></a>快取的資料來源

每個 DataSourceControl 提供四個屬性來設定快取。EnableCaching、 CacheDuration、 CacheExpirationPolicy 和 CacheKeyDependency。

## <a name="enablecaching"></a>EnableCaching

EnableCaching 是布林值屬性，決定快取已啟用資料來源控制項。

## <a name="cacheduration-property"></a>CacheDuration 屬性

CacheDuration 屬性設定快取保持有效的秒的數。 此屬性設定為**0**會導致要保持有效，直到明確失效快取。

## <a name="cacheexpirationpolicy-property"></a>CacheExpirationPolicy 屬性

CacheExpirationPolicy 屬性可以設定為 **絕對**或**滑動**。 請將它設定為絕對最大的資料會快取的時間量是 CacheDuration 屬性所指定的秒數表示。 設定為 滑動，執行每個作業時，會重設到期時間。

## <a name="cachekeydependency-property"></a>CacheKeyDependency 屬性

如果 CacheKeyDependency 屬性指定的字串值，ASP.NET 會設定新的快取相依性，該字串為基礎。 這可讓您明確地只是變更或移除 CacheKeyDependency 確認快取。

**重要**： 如果已啟用模擬，且若要存取的資料來源及/或資料內容，根據用戶端身分識別，則建議的 EnableCaching 設為 False，停用快取。 如果在此案例中啟用快取，而且原先要求資料的使用者以外的使用者發出的要求，資料來源的授權不會強制執行。 從快取，系統將只會處理資料。

## <a name="the-sqldatasource-control"></a>SqlDataSource 控制項

SqlDataSource 控制項可讓開發人員存取儲存在任何支援 ADO.NET 的關聯式資料庫中的資料。 它可用來存取 SQL Server 資料庫、 System.Data.OleDb 提供者、 System.Data.Odbc 提供者或 System.Data.OracleClient 提供者來存取 Oracle System.Data.SqlClient 提供者。 因此，SqlDataSource 當然不只是用於存取 SQL Server 資料庫中的資料。

若要使用 SqlDataSource，，只要提供 ConnectionString 屬性的值和指定的 SQL 命令或預存程序。 SqlDataSource 控制項會負責基礎 ADO.NET 架構使用。 它會開啟連接、 查詢資料來源或執行預存程序、 傳回的資料，，然後關閉您的連接。

> [!NOTE]
> DataSourceControl 類別就會自動關閉您的連接，因為它應該減少產生的資料庫連接發生溢位客戶來電數目。


下列程式碼片段 DropDownList 將控制項繫結至 SqlDataSource 控制項使用的連接字串儲存在如上所示的組態檔中。

[!code-aspx[Main](data-source-controls/samples/sample3.aspx)]

如上所示，SqlDataSource DataSourceMode 屬性會指定資料來源的模式。 在上述範例中，將 datasourcemode 設為設定為 DataReader。 在此情況下，SqlDataSource 會傳回 IDataReader 物件使用順向且唯讀資料指標。 指定的型別的物件，則會傳回受到使用提供者。 我在此情況下，使用 System.Data.SqlClient 提供者中所指定&lt;connectionStrings&gt; web.config 檔案區段。 因此，會傳回的物件都屬於類型 SqlDataReader。 藉由指定 datasourcemode 設為資料集，資料會儲存在伺服器上的資料集。 此模式可讓您加入功能，例如排序、 分頁等等。如果已資料繫結至 GridView 控制項 SqlDataSource，我會選擇資料集模式。 不過，在 DropDownList，DataReader 模式是正確的選擇。

> [!NOTE]
> 當快取 SqlDataSource 或 AccessDataSource DataSourceMode 屬性必須設定資料集。 如果您啟用使用 DataSourceMode DataReader 快取，會發生例外狀況。


## <a name="sqldatasource-properties"></a>SqlDataSource 屬性

下列是一些 SqlDataSource 控制項的屬性。

### <a name="cancelselectonnullparameter"></a>CancelSelectOnNullParameter

布林值，指定是否其中一個參數為 null，是否要取消選取的命令。 根據預設，則為 true。

### <a name="conflictdetection"></a>ConflictDetection

其中多個使用者可能會更新資料來源在同一時間的情況下，ConflictDetection 屬性會決定 SqlDataSource 控制項的行為。 這個屬性評估為其中一個 ConflictOptions 列舉值。 這些值是**CompareAllValues**和**OverwriteChanges**。 如果設定為 OverwriteChanges，將資料寫入至資料來源的最後一個人覆寫任何先前的變更。 不過，如果 ConflictDetection 屬性設定為 CompareAllValues，SelectCommand 所傳回的資料行取得建立參數，也會建立參數中每個資料行，允許以 SqlDataSource 保留原始值判斷自 SelectCommand 上次執行所執行，不論是否已變更的值。

### <a name="deletecommand"></a>DeleteCommand

設定或取得從資料庫刪除資料列時使用的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="deletecommandtype"></a>DeleteCommandType

設定或取得的 delete 命令的類型是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="deleteparameters"></a>DeleteParameters

傳回與 SqlDataSource 控制項相關聯的 SqlDataSourceView 物件的 DeleteCommand 所使用的參數。

### <a name="oldvaluesparameterformatstring"></a>Where

這個屬性用來指定在其中 ConflictDetection 屬性設定為 CompareAllValues 的情況下的原始值參數的格式。 預設為 {0} 表示原始值參數會與原始參數同名。 換句話說，如果 EmployeeID 欄位名稱，則原始值參數為@EmployeeID。

### <a name="selectcommand"></a>SelectCommand

設定或取得用來從資料庫擷取資料的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="selectcommandtype"></a>SelectCommandType

設定或取得的 select 命令，類型是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="selectparameters"></a>SelectParameters

傳回與 SqlDataSource 控制項相關聯的 SqlDataSourceView 物件 SelectCommand 所使用的參數。

### <a name="sortparametername"></a>SortParameterName

取得或設定預存程序參數時所使用排序的資料擷取的資料來源控制項的名稱。 只有在 SelectCommandType 設為預存程序時才有效。

### <a name="sqlcachedependency"></a>SqlCacheDependency

以分號分隔的字串，指定資料庫和 SQL Server 的快取相依性中使用的資料表。 （在更新版本的模組中會討論 SQL 快取相依性）。

### <a name="updatecommand"></a>UpdateCommand

設定或取得更新資料庫中的資料時使用的 SQL 字串。 這可以是 SQL 查詢或預存程序名稱。

### <a name="updatecommandtype"></a>UpdateCommandType

設定或取得的更新命令類型可以是 SQL 查詢 （文字） 或預存程序 （預存程序）。

### <a name="updateparameters"></a>UpdateParameters

傳回與 SqlDataSource 控制項相關聯的 SqlDataSourceView 物件的 UpdateCommand 所使用的參數。

## <a name="the-accessdatasource-control"></a>AccessDataSource 控制項

AccessDataSource 控制項衍生自 SqlDataSource 類別，以及用來資料繫結至 Microsoft Access 資料庫。 AccessDataSource 控制項的 ConnectionString 屬性是唯讀屬性。 不使用 ConnectionString 屬性，DataFile 屬性用來指向 Access 資料庫，如下所示。

[!code-aspx[Main](data-source-controls/samples/sample4.aspx)]

AccessDataSource 一定會將設定的基底 SqlDataSource ProviderName System.Data.OleDb 至，並連接至使用 Microsoft.Jet.OLEDB.4.0 OLE DB 提供者的資料庫。 您無法使用 AccessDataSource 控制項來連接到受密碼保護 Access 資料庫。 如果您有連接到受密碼保護的資料庫，您應該使用 SqlDataSource 控制項。

> [!NOTE]
> 儲存在網站內的 access 資料庫應該放在應用程式\_資料目錄。 ASP.NET 不允許瀏覽這個目錄中的檔案。 您必須將應用程式的讀取和寫入權限授與處理序帳戶\_使用 Access 資料庫時的資料目錄。


## <a name="the-xmldatasource-control"></a>XmlDataSource 控制項

XmlDataSource 用來資料繫結的 XML 資料至資料繫結控制項。 您可以繫結至 XML 檔案使用 DataFile 屬性，或可以繫結使用的資料屬性的 XML 字串。 XmlDataSource 會公開為可繫結欄位的 XML 屬性。 在您要繫結到不會表示為屬性的值的情況下，您必須使用 XSL 轉換。 您也可以使用 XPath 運算式來篩選 XML 資料。

請考慮下列的 XML 檔案：

[!code-xml[Main](data-source-controls/samples/sample5.xml)]

請注意 XmlDataSource 會使用 XPath 屬性*人/*篩選以便只&lt;人員&gt;節點。 DropDownList 接著，此資料繫結至使用 DataTextField 屬性 LastName 屬性。

雖然 XmlDataSource 控制項主要用來資料繫結至唯讀的 XML 資料，便可編輯 XML 資料檔。 請注意，在這種情況下，自動插入、 更新和刪除的 XML 檔案中的資訊不會自動與其他資料來源控制項。 相反地，您必須撰寫程式碼以手動編輯的資料，使用下列 XmlDataSource 控制項的方法。

### <a name="getxmldocument"></a>GetXmlDocument

擷取 XmlDocument 物件，其中包含由 XmlDataSource 擷取的 XML 程式碼。

### <a name="save"></a>儲存

將記憶體中 XmlDocument 儲存回資料來源。

請務必了解 Save 方法才符合下列兩種情況時：

1. XmlDataSource 使用 DataFile 屬性繫結至記憶體中 XML 資料繫結至 XML 檔案而不是資料屬性。
2. 透過轉換或 TransformFile 屬性不指定任何轉換。

請注意 Save 方法可產生非預期的結果，當多位使用者同時呼叫。

## <a name="the-objectdatasource-control"></a>ObjectDataSource 控制項

我們已涵蓋這一點的資料來源控制項都是兩層式應用程式的絕佳選擇其中的資料來源控制項與外界溝通的直接資料存放區。 不過，許多真實世界應用程式是多層式應用程式資料來源控制項可能需要 business 物件的通訊與資料層進行通訊。 在這些情況下，ObjectDataSource 會填滿帳單正確地切割。 ObjectDataSource 可搭配使用與來源物件。 ObjectDataSource 控制項將會建立來源物件、 呼叫指定的方法和 dispose 的單一要求時，全部都在範圍內的物件執行個體的執行個體，如果您的物件執行個體方法，而非靜態方法 (在 Visual Basic 中是 Shared)。 因此，您必須是無狀態的物件。 也就是您的物件應該取得並發行單一要求的範圍內的所有必要的資源。 您可以控制所處理的 ObjectDataSource 控制項 ObjectCreating 事件建立的來源物件的方式。 您可以建立的來源物件，執行個體，並再將 ObjectDataSourceEventArgs 類別 ObjectInstance 屬性設定為該執行個體。 ObjectDataSource 控制項將會使用建立 ObjectCreating 事件，而不是建立自己的執行個體中的執行個體。

如果 ObjectDataSource 控制項來源物件公開的公用靜態方法 (在 Visual Basic 中是 Shared) 可以呼叫以擷取和修改資料，ObjectDataSource 控制項就會直接呼叫這些方法。 如果 ObjectDataSource 控制項必須以使方法呼叫建立來源物件的執行個體，則物件必須包含不採用任何參數的公用建構函式。 ObjectDataSource 控制項來源物件的新執行個體建立時，會呼叫這個建構函式。

如果來源物件未包含不含參數的公用建構函式，您可以建立將由 ObjectCreating 事件 ObjectDataSource 控制項來源物件的執行個體。

## <a name="specifying-object-methods"></a>指定物件方法

ObjectDataSource 控制項來源物件可以包含任何數目的方法，可用來選取、 插入、 更新或刪除資料。 識別使用 SelectMethod、 InsertMethod、 UpdateMethod 或 DeleteMethod ObjectDataSource 控制項的屬性，則 ObjectDataSource 控制項根據方法的名稱會呼叫這些方法。 來源物件也可以包含選擇性 SelectCount 方法，這識別 ObjectDataSource 控制項使用 SelectCountMethod 屬性，會傳回在資料來源的物件總數的計數。 ObjectDataSource 控制項 Select 方法呼叫時所要擷取在資料來源使用的記錄總數分頁之後，會呼叫 SelectCount 方法。

## <a name="lab-using-data-source-controls"></a>使用資料來源控制項的實驗室

## <a name="exercise-1---displaying-data-with-the-sqldatasource-control"></a>練習 1-SqlDataSource 控制項顯示資料

下列練習使用 SqlDataSource 控制項連接至 Northwind 資料庫。 它會假設您在 SQL Server 2000 執行個體上有 Northwind 資料庫的存取權。

1. 建立新的 ASP.NET 網站。
2. 加入新的 web.config 檔案。

    1. 以滑鼠右鍵按一下方案總管] 中的專案，然後按一下 [加入新項目。
    2. 從範本清單中選擇 Web 組態檔，並按一下 新增。
3. 編輯&lt;connectionStrings&gt;區段，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample6.aspx)]
4. 切換至程式碼 檢視，並加入 ConnectionString 屬性和 SelectCommand 屬性&lt;asp: SqlDataSource&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample7.aspx)]
5. 設計檢視中，加入新的 GridView 控制項。
6. 從 [GridView 工作] 功能表中選擇資料來源] 下拉式清單中，選擇 [SqlDataSource1。
7. 以滑鼠右鍵按一下 Default.aspx，並從功能表選擇 瀏覽器中檢視。 當系統提示您儲存時，請按一下 [是]。
8. 在 GridView 會顯示來自 [產品] 資料表的資料。

## <a name="exercise-2---editing-data-with-the-sqldatasource-control"></a>練習 2： 編輯資料與 SqlDataSource 控制項

下列練習示範如何為資料繫結 DropDownList 控制使用宣告式語法並可讓您編輯 DropDownList 控制項中顯示的資料。

1. 在設計檢視中，刪除從 Default.aspx 的 GridView 控制項。 

    **重要**： 離開 SqlDataSource 控制項在頁面上。
2. DropDownList 將控制項加入至 Default.aspx。
3. 切換至來源檢視。
4. 加入至的 DataSourceId、 DataTextField 和 DataValueField 屬性&lt;asp: DropDownList&gt;控制，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample8.aspx)]
5. 儲存 Default.aspx，並在瀏覽器中檢視它。 請注意 DropDownList 包含的所有產品，從 Northwind 資料庫。
6. 關閉瀏覽器。
7. 在 Default.aspx 的來源檢視中，加入新的 TextBox 控制項 DropDownList 控制項下方。 將文字方塊中的 ID 屬性變更為 txtProductName。
8. 在文字方塊控制項，加入新的按鈕控制項。 將按鈕的 ID 屬性變更為 btnUpdate 和 Text 屬性**更新產品名稱**。
9. 在 Default.aspx 來源檢視，UpdateCommand 屬性和加入兩個新 UpdateParameters SqlDataSource 標記，如下所示： 

    [!code-aspx[Main](data-source-controls/samples/sample9.aspx)]

    > [!NOTE]
    > 請注意，有兩個更新參數 （「 產品名稱 」 和 「 ProductID 」） 加入這個程式碼中。 這些參數會對應到 txtProductName 文字方塊中的文字屬性和 ddlProducts DropDownList SelectedValue 屬性。
10. 切換到設計檢視，然後按兩下按鈕控制項加入事件處理常式。
11. 將下列程式碼加入至 btnUpdate\_按一下程式碼： 

    [!code-csharp[Main](data-source-controls/samples/sample10.cs)]
12. 以滑鼠右鍵按一下 Default.aspx，然後選擇瀏覽器中檢視它。 當系統提示您儲存所有變更，請按一下 [是]。
13. ASP.NET 2.0 部分類別可讓執行階段編譯。 您不需要建置的應用程式，以查看程式碼變更，才會生效。
14. DropDownList 從選取的產品。
15. 在文字方塊中輸入所選產品的新名稱，然後按一下 [更新] 按鈕。
16. 在資料庫中更新的產品名稱。

## <a name="exercise-3-using-the-objectdatasource-control"></a>練習 3 使用 ObjectDataSource 控制項

此練習將示範如何使用 ObjectDataSource 控制項與來源物件來與 Northwind 資料庫互動。

1. 以滑鼠右鍵按一下方案總管] 中的專案，然後按一下 [加入新項目。
2. 在範本清單中選取 Web 表單。 將名稱變更為 object.aspx 並按一下 [新增]。
3. 以滑鼠右鍵按一下方案總管] 中的專案，然後按一下 [加入新項目。
4. 在範本清單中選取類別。 將類別的名稱變更為 NorthwindData.cs 並按一下 [新增]。
5. 按一下 [是] 會將類別新增至應用程式出現提示時\_程式碼資料夾。
6. NorthwindData.cs 檔案中加入下列程式碼： 

    [!code-csharp[Main](data-source-controls/samples/sample11.cs)]
7. Object.aspx 的來源檢視中加入下列程式碼： 

    [!code-aspx[Main](data-source-controls/samples/sample12.aspx)]
8. 儲存所有檔案，並瀏覽 object.aspx。
9. 檢視詳細資料、 編輯員工、 新增員工，以及刪除員工，互動的介面。
