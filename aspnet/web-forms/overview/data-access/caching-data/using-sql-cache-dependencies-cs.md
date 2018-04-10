---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: 使用 SQL 快取相依性 (C#) |Microsoft 文件
author: rick-anderson
description: 若要讓指定的一段時間之後過期的快取的資料是最簡單的快取策略。 但這個簡單的方法是指，快取的資料 maintai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: 8660fd979b5f18ed0182c8ae1e671f362f5dec7e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="using-sql-cache-dependencies-c"></a>使用 SQL 快取相依性 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip)或[下載 PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> 若要讓指定的一段時間之後過期的快取的資料是最簡單的快取策略。 但這個簡單的方式表示快取的資料會維護與過時的資料保留太長或太快過期的目前資料中產生其基礎資料來源沒有關聯。 更好的方法是使用 SqlCacheDependency 類別，讓資料維持快取，直到其基礎資料的 SQL 資料庫中已修改。 本教學課程會說明方法。


## <a name="introduction"></a>簡介

檢查快取技術[快取資料與 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)和[架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程會使用以時間為基礎的到期之後指定收回快取中的資料一段時間。 這個方法是以平衡效能提升的快取資料失效針對最簡單的方式。 選取的時間到期*x*幾秒鐘的網頁開發人員 concedes 來享受只能快取的效能優點*x*秒，但是可以將很容易，她資料將不會過時時間超過最大值*x*秒。 當然，對於靜態資料， *x*可擴充的 web 應用程式存留期為已檢查[快取的資料，在應用程式啟動](caching-data-at-application-startup-cs.md)教學課程。

當快取資料庫資料、 以時間為基礎的到期通常會選擇易於使用，但通常是不適當的解決方案。 在理想情況下，直到資料庫; 中已修改基礎資料的資料庫資料會維持快取則只會收回快取。 此方法達到最大快取的效能優勢，並將過時資料的持續時間降到最低。 不過，為了享受有這些優點必須知道基礎資料庫資料已修改，且對應的項目從快取的收回的備妥某些系統。 在 ASP.NET 2.0 之前網頁開發人員所負責實作此系統。

ASP.NET 2.0 提供[`SqlCacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx)和可以收回必要的基礎結構，以判斷當變更發生在資料庫中，因此對應的快取項目。 有兩種技術，以判斷基礎資料變更時： 告知和輪詢。 討論告知和輪詢之間的差異之後, 我們將建立基礎結構需要支援輪詢，然後瀏覽如何使用`SqlCacheDependency`類別中宣告式和以程式設計方式的案例。

## <a name="understanding-notification-and-polling"></a>了解告知和輪詢

有兩種技術，可用來判斷資料庫中的資料時已經修改： 告知和輪詢。 使用通知，資料庫會自動發出警示 ASP.NET 執行階段，因為自從上次執行查詢已變更的特定查詢結果時，此時會收回快取與查詢相關聯的項目。 輪詢，與資料庫伺服器會維護當上次已更新特定資料表的相關資訊。 ASP.NET 執行階段會定期輪詢資料庫，請檢查資料表已變更後已進入快取。 其資料已被修改這些資料表有收回及其相關聯的快取項目。

通知選項需要較少的安裝程式，比輪詢，而且是更細微的因為它會追蹤在查詢層級，而不是在資料表層級的變更。 不幸的是，通知中才有完整版本的 Microsoft SQL Server 2005 （亦即，非 Express 版本）。 不過，輪詢選項可以用於所有版本的 Microsoft SQL Server 7.0 2005年。 由於這些教學課程使用的 SQL Server 2005 Express 版本，我們將著重於設定和使用輪詢選項。 請參閱進一步讀取章節進一步上 SQL Server 2005 的通知功能的資源本教學課程結尾處。

輪詢，資料庫必須設定要包含名為的資料表`AspNet_SqlCacheTablesForChangeNotification`具有三個資料行- `tableName`， `notificationCreated`，和`changeId`。 此資料表包含每個資料表都會有可能需要在 web 應用程式中的 SQL 快取相依性中使用的資料列。 `tableName`資料行指定名稱的資料表時`notificationCreated`表示的日期和時間的資料列已加入至資料表。 `changeId`資料行是類型`int`且初始值為 0。 其值會隨著每項修改資料表。

除了`AspNet_SqlCacheTablesForChangeNotification`資料表中，資料庫也需要在每個資料表可能會出現在觸發程序中包含 SQL 快取相依性。 這些觸發程序執行時插入、 更新或刪除資料列，並遞增資料表 s`changeId`值`AspNet_SqlCacheTablesForChangeNotification`。

ASP.NET 執行階段會追蹤目前`changeId`資料表快取資料使用時`SqlCacheDependency`物件。 定期檢查資料庫和任何`SqlCacheDependency`物件`changeId`不同於資料庫中的值會收回自不同`changeId`值會指出已變更資料表因為資料已快取。

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>步驟 1： 瀏覽`aspnet_regsql.exe`命令列程式

輪詢方法資料庫必須與安裝程式包含上述的基礎結構： 預先定義的資料表 (`AspNet_SqlCacheTablesForChangeNotification`)，少數幾個預存程序和觸發程序在每個可用於 web 中的 SQL 快取相依性的資料表應用程式。 透過命令列程式，則可以建立這些資料表、 預存程序和觸發程序`aspnet_regsql.exe`，找到`$WINDOWS$\Microsoft.NET\Framework\version`資料夾。 若要建立`AspNet_SqlCacheTablesForChangeNotification`資料表和相關聯的預存程序，從命令列執行下列命令：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> 若要執行這些命令必須在指定的資料庫登入[ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx)和[ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx)角色。 若要檢查 T-SQL 傳送到資料庫`aspnet_regsql.exe`命令列程式，請參閱[此部落格文章](http://scottonwriting.net/sowblog/posts/10709.aspx)。


例如，若要輪詢的基礎結構新增到 Microsoft SQL Server 資料庫中名為`pubs`名為資料庫伺服器上`ScottsServer`使用 Windows 驗證，請導覽至適當的目錄，並且從命令列中，輸入：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

加入資料庫層級基礎結構之後，我們需要將 SQL 快取相依性中使用這些資料表中新增觸發程序。 使用`aspnet_regsql.exe`命令列程式一次，但指定資料表名稱使用`-t`切換，而不是使用`-ed`切換使用`-et`，就像這樣：


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

若要新增至觸發程序`authors`和`titles`資料表上`pubs`資料庫`ScottsServer`，使用：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

此教學課程中加入觸發程序來`Products`， `Categories`，和`Suppliers`資料表。 我們會探討在步驟 3 中的特定命令列語法。

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>步驟 2： 參考中的 Microsoft SQL Server 2005 Express Edition 資料庫`App_Data`

`aspnet_regsql.exe`命令列程式需要的資料庫和伺服器名稱，才能新增必要的輪詢基礎結構。 什麼是 Microsoft SQL Server 2005 Express 資料庫所在的資料庫和伺服器名稱，但`App_Data`資料夾嗎？ 而不必探索的資料庫和伺服器名稱是什麼，我已找到最簡單的方式是附加至資料庫`localhost\SQLExpress`資料庫執行個體，並重新命名資料使用[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)。 如果您有一個在電腦上安裝 SQL Server 2005 的完整版本，然後您可能已經在電腦上安裝 SQL Server Management Studio。 如果您只有 Express 版本，您可以下載免費[Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

關閉 Visual Studio 來啟動。 接下來，開啟 SQL Server Management Studio 並選擇連接到`localhost\SQLExpress`伺服器使用 Windows 驗證。


![附加至 localhost\SQLExpress 伺服器](using-sql-cache-dependencies-cs/_static/image1.gif)

**圖 1**： 將附加至`localhost\SQLExpress`伺服器


連接到伺服器之後，Management Studio 會顯示伺服器，並具有資料庫、 安全性和其他等等的子資料夾。 以滑鼠右鍵按一下 [資料庫] 資料夾，然後選擇 [附加] 選項。 這會顯示 [附加資料庫] 對話方塊 （請參閱圖 2）。 按一下 [新增] 按鈕，然後選取`NORTHWND.MDF`資料庫資料夾，在您的 web 應用程式 s`App_Data`資料夾。


[![附加 NORTHWND。從 [App_Data] 資料夾的 MDF 資料庫](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**圖 2**： 附加`NORTHWND.MDF`資料庫從`App_Data`資料夾 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image2.png))


這會將資料庫加入 [資料庫] 資料夾。 資料庫名稱可能是資料庫檔案的完整路徑或完整路徑前面加上[GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)。 若要避免此時間較長的資料庫名稱 中輸入，使用 aspnet 時\_regsql.exe 命令列工具，附加資料庫到更人類看得好記的名稱，以滑鼠右鍵按一下資料庫只重新命名，然後選擇重新命名。 我已重新命名為 DataTutorials 的我的資料庫。


![將附加的資料庫重新命名為更人類看得好記的名稱](using-sql-cache-dependencies-cs/_static/image3.gif)

**圖 3**： 將附加的資料庫重新命名為更人類看得好記的名稱


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>步驟 3： 加入至 Northwind 資料庫輪詢基礎結構

既然我們已附加`NORTHWND.MDF`資料庫從`App_Data`資料夾中，我們準備好要加入的輪詢基礎結構。 假設您已重新命名為 DataTutorials 的資料庫，執行下列四個命令：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

之後執行這些四個命令，Management Studio 中的資料庫名稱上按一下滑鼠右鍵，移至 工作 子功能表，然後選擇 卸離。 然後關閉 Management Studio 並重新開啟 Visual Studio。

一旦已重新開啟 Visual Studio，切入到透過 [伺服器總管] 中的資料庫。 請注意新的資料表 (`AspNet_SqlCacheTablesForChangeNotification`)，新的預存程序和觸發程序上`Products`， `Categories`，和`Suppliers`資料表。


![Database 現在包含必要的輪詢基礎結構](using-sql-cache-dependencies-cs/_static/image4.gif)

**圖 4**: Database 現在包含必要的輪詢基礎結構


## <a name="step-4-configuring-the-polling-service"></a>步驟 4： 設定輪詢服務

之後在資料庫中建立所需的資料表、 觸發程序和預存程序，最後一個步驟是設定輪詢服務，這由透過`Web.config`藉由以毫秒為單位指定要使用與輪詢頻率的資料庫。 下列標記會輪詢一次每秒鐘 Northwind 資料庫。


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name`值`<add>`項目 (NorthwindDB) 與特定資料庫有關聯的人類看得懂的名稱。 當使用 SQL 快取相依性，我們需要參考快取的資料為基礎的資料表以及此處定義的資料庫名稱。 我們會看到如何使用`SqlCacheDependency`類別以程式設計方式將 SQL 快取相依性與快取的步驟 6 中的資料。

一旦建立 SQL 快取相依性，輪詢系統將會連接到資料庫中定義`<databases>`項目每個`pollTime`毫秒，並執行`AspNet_SqlCachePollingStoredProcedure`預存程序。 這個預存程序-已加入這些備份在步驟 3 中使用`aspnet_regsql.exe`命令列工具-傳回`tableName`和`changeId`中每個記錄的值`AspNet_SqlCacheTablesForChangeNotification`。 過期的 SQL 快取相依性會從快取收回。

`pollTime`設定導入了效能與資料過時之間權衡取捨。 小型`pollTime`值會增加資料庫中，要求的數目，但更快速地收回從快取資料失效。 較大`pollTime`值減少資料庫要求的數目，但是會增加後端資料變更時，何時收回相關的快取項目之間的延遲。 幸運的，資料庫要求從簡單的輕量型表格傳回幾個資料列 s 執行簡單的預存程序。 執行試驗不同但`pollTime`值，以尋找適合的平衡資料庫應用程式存取及資料失效。 最小`pollTime`允許值為 500。

> [!NOTE]
> 上述範例中提供了單一`pollTime`值`<sqlCacheDependency>`項目，但是您可以選擇性地指定`pollTime`值`<add>`項目。 如果您有多個指定的資料庫，並想要自訂每個資料庫的輪詢頻率，這非常有用。


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>步驟 5： 以宣告方式使用 SQL 快取相依性

在步驟 1 到 4 我們討論了如何安裝必要的資料庫基礎結構，以及設定輪詢系統。 與這個基礎結構，我們現在可以新增項目資料快取以使用以程式設計方式或宣告式技術相關聯 SQL 快取相依性。 在此步驟中，我們將檢驗如何以宣告方式使用的 SQL 快取相依性。 步驟 6 中我們將探討以程式設計的方式。

[快取資料與 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)教學課程中，瀏覽 ObjectDataSource 宣告式快取功能。 只需要設定`EnableCaching`屬性`true`和`CacheDuration`某些時間間隔的屬性，ObjectDataSource 會自動快取的資料從其基礎的物件傳回指定的間隔。 ObjectDataSource 也可以使用一或多個 SQL 快取相依性。

若要示範如何以宣告方式使用 SQL 快取相依性，開啟`SqlCacheDependencies.aspx`頁面`Caching`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳 GridView。 設定 GridView s`ID`至`ProductsDeclarative`和從其智慧標籤上，選擇 繫結至名為新 ObjectDataSource `ProductsDataSourceDeclarative`。


[![建立名為 ProductsDataSourceDeclarative 新 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**圖 5**： 建立新的 ObjectDataSource 具名`ProductsDataSourceDeclarative`([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image4.png))


設定用於 ObjectDataSource`ProductsBLL`類別並設定下拉式清單中選取索引標籤，以`GetProducts()`。 在 更新 索引標籤中，選擇 `UpdateProduct`以三個輸入參數的多載`productName`， `unitPrice`，和`productID`。 在 INSERT 和 DELETE 的索引標籤中設定為 （無） 的下拉式清單。


[![使用具有三個輸入參數的 UpdateProduct 多載](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**圖 6**： 使用 UpdateProduct 多載具有三個輸入參數 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image6.png))


[![下拉式清單 （無） 插入和刪除索引標籤](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**圖 7**： 插入和刪除的索引標籤設定為 （無） 下拉式清單 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 和 CheckBoxFields GridView 中每個資料欄位。 移除所有的欄位，但`ProductName`， `CategoryName`，和`UnitPrice`，並適當地格式化這些欄位。 從 GridView s 智慧標籤，選取啟用分頁、 啟用排序及啟用編輯核取方塊。 Visual Studio 會將 ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`。 為了讓正常運作的 GridView 的編輯功能，請移除此屬性完全宣告式語法或設回其預設值`{0}`。

最後，會加入標籤 Web 控制項上方的 GridView 和設定其`ID`屬性`ODSEvents`及其`EnableViewState`屬性`false`。 進行這些變更之後，頁面 s 的宣告式標記看起來應該如下所示。 請注意，我們所做的美觀的自訂項目不是為了示範 SQL 快取相依性功能的 GridView 欄位的數字。


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

接下來，建立事件處理常式 ObjectDataSource s`Selecting`事件並在它中加入下列程式碼：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

請記得，ObjectDataSource 的`Selecting`只有在從其基礎物件擷取資料時，就會引發事件。 如果 ObjectDataSource 從自己的快取存取資料，不會引發這個事件。

現在，請造訪此頁面，透過瀏覽器。 因為我們尚未來實作任何快取，頁面上，進行排序，或編輯的格線頁，每次發生應該會顯示為文字、 Selecting 事件引發，如圖 8 所示。


[![ObjectDataSource 的 Selecting 事件引發的每個分頁 GridView，編輯、 時間或排序](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**圖 8**: ObjectDataSource s`Selecting`事件引發每次被分頁 GridView、 編輯或排序 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image10.png))


中[快取資料與 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)教學課程中，設定`EnableCaching`屬性`true`ObjectDataSource 所指定的持續期間快取其資料會導致其`CacheDuration`屬性。 ObjectDataSource 還有[`SqlCacheDependency`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)，這樣會將一個或多個 SQL 快取相依性加入快取的資料使用模式：


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

其中*databaseName*中所指定的資料庫名稱`name`屬性`<add>`中的項目`Web.config`，和*tableName*是資料庫資料表的名稱。 比方說，若要建立會無限期地快取資料 ObjectDataSource 根據 Northwind s 針對 SQL 快取相依性`Products`資料表中，設定 ObjectDataSource s`EnableCaching`屬性`true`及其`SqlCacheDependency`屬性NorthwindDB:Products。

> [!NOTE]
> 您可以使用 SQL 快取相依性*和*以時間為基礎的到期設定`EnableCaching`至`true`，`CacheDuration`時間間隔，以和`SqlCacheDependency`資料庫和資料表名稱。 當達到時間為基礎的到期日期或輪詢系統資訊已變更基礎資料庫資料，何者先發生 ObjectDataSource 會收回其資料。


在 GridView`SqlCacheDependencies.aspx`顯示兩個資料表-`Products`和`Categories`(產品 s`CategoryName`欄位會擷取透過`JOIN`上`Categories`)。 因此，我們想要指定兩個 SQL 快取相依性： NorthwindDB:Products;NorthwindDB:Categories。


[![設定以支援使用 SQL 快取相依性產品和分類快取 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**圖 9**： 上設定支援快取使用 SQL 快取相依性 ObjectDataSource`Products`和`Categories`([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image12.png))


設定支援快取 ObjectDataSource 之後, 重新檢查透過瀏覽器頁面。 同樣地，引發文字 Selecting 事件應該會出現在第一次頁面開啟，但是應該消失分頁、 排序，或按一下 [編輯] 或 [取消] 5d; 按鈕時。 這是因為將資料載入 ObjectDataSource s 快取後，仍有直到`Products`或`Categories`修改資料表，或透過 GridView 的資料更新資料。

方格中進行分頁，您會看到缺乏 Selecting 事件引發之後的文字，開啟新的瀏覽器視窗，並瀏覽至基本教學課程 」 中編輯、 插入和刪除區段 (`~/EditInsertDelete/Basics.aspx`)。 更新的產品價格的名稱。 然後，從第一個瀏覽器視窗中，以檢視資料的不同頁面、 排序方格中，或按一下資料列 s [編輯] 按鈕。 此時，引發在 Selecting 事件應該再次出現，因為基礎資料庫資料已修改 （請參閱圖 10）。 如果文字未出現，請稍候片刻，然後再試。 請記住輪詢服務正在檢查對`Products`資料表每個`pollTime`毫秒，因此沒有基礎資料更新時，與時收回快取的資料之間的延遲。


[![修改 [產品] 資料表會收回快取的產品資料](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**圖 10**： 修改 Products 資料表收回快取產品資料 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>步驟 6： 以程式設計方式使用`SqlCacheDependency`類別

[架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程看過的架構，而不是緊密結合 ObjectDataSource 與快取中使用不同的快取層的優點。 在該教學課程中我們建立`ProductsCL`類別來示範以程式設計方式使用資料快取。 若要利用快取層中的 SQL 快取相依性，使用`SqlCacheDependency`類別。

輪詢系統，`SqlCacheDependency`物件必須是特定的資料庫和資料表組相關聯。 例如，下列程式碼中，會建立`SqlCacheDependency`物件會根據 Northwind 資料庫的`Products`資料表：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

兩個輸入參數來`SqlCacheDependency`s 建構函式會分別為資料庫和資料表名稱。 例如 ObjectDataSource s`SqlCacheDependency`屬性所使用的資料庫名稱是在指定的值相同`name`屬性`<add>`中的項目`Web.config`。 資料表名稱是實際的資料庫資料表名稱。

使`SqlCacheDependency`與資料快取中加入項目，使用其中一種`Insert`接受相依性的方法多載。 下列程式碼加入*值*無限期的持續期間內的資料快取，但將它與`SqlCacheDependency`上`Products`資料表。 簡單地說，*值*會保留在快取中，直到收回因為記憶體條件約束，或因為輪詢系統偵測到`Products`快取以來，資料表已變更。


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

快取層 s`ProductsCL`類別目前會快取的資料從`Products`資料表使用 60 秒的時間基礎到期。 可讓此類別的更新，使其改為使用 SQL 快取相依性的 s。 `ProductsCL`類別的`AddCacheItem`方法，這個方法會負責將資料加入至快取，目前包含下列程式碼：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

更新這個程式碼以使用`SqlCacheDependency`物件而非`MasterCacheKeyArray`快取相依性：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

若要測試這項功能，加入 GridView 頁面下方現有`ProductsDeclarative`GridView。 設定這個新的 GridView s`ID`至`ProductsProgrammatic`和透過其智慧標籤上，繫結到名為新 ObjectDataSource `ProductsDataSourceProgrammatic`。 設定用於 ObjectDataSource`ProductsCL`類別，在 選取的下拉式清單和更新索引標籤，以設定`GetProducts`和`UpdateProduct`分別。


[![設定使用 ProductsCL 類別 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**圖 11**： 設定要使用 ObjectDataSource`ProductsCL`類別 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image16.png))


[![從選取的索引標籤的下拉式清單中選取 GetProducts 方法](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**圖 12**： 選取`GetProducts`方法，從下拉式清單選取索引標籤 s ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image18.png))


[![從更新索引標籤的下拉式清單中選擇 UpdateProduct 方法](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**圖 13**： 從更新索引標籤的下拉式清單中選擇 UpdateProduct 方法 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image20.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 和 CheckBoxFields GridView 中每個資料欄位。 Like 與第一個 GridView 加入至此頁面，移除所有的欄位，但`ProductName`， `CategoryName`，和`UnitPrice`，並適當地格式化這些欄位。 從 GridView s 智慧標籤，選取啟用分頁、 啟用排序及啟用編輯核取方塊。 如同`ProductsDataSourceDeclarative`ObjectDataSource，Visual Studio 會將設定`ProductsDataSourceProgrammatic`ObjectDataSource s`OldValuesParameterFormatString`屬性`original_{0}`。 為了讓 GridView 的編輯功能，若要正常運作，請將此屬性設定回`{0}`（或完全移除屬性指派的宣告式語法）。

完成這些工作之後，產生 GridView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

快取相依性，在快取層來測試 SQL 中設定中斷點`ProductCL`類別的`AddCacheItem`方法，然後開始偵錯。 當您第一次瀏覽`SqlCacheDependencies.aspx`，當做資料第一次要求，然後放入快取，應叫用中斷點。 接下來，移至另一個在 GridView 的頁面，或其中一個資料行排序。 這會導致在 GridView 重新查詢資料，但應該後，快取中找到的資料`Products`尚未修改資料庫資料表。 如果資料是重複找不到快取中，請確定您的電腦上沒有足夠的記憶體，並再試一次。

之後的幾個頁面的 GridView 進行分頁，開啟第二個瀏覽器視窗並瀏覽至基本教學課程 」 中編輯、 插入和刪除區段 (`~/EditInsertDelete/Basics.aspx`)。 更新 Products 資料表的記錄，然後，從第一個瀏覽器視窗中，檢視新的頁面或按一下其中一個排序的標頭。

在此案例中您會看到其中一個操作： 其中一個中斷點會被叫用，表示快取的資料已變更資料庫; 因為收回或者，將不會叫用中斷點，也就是說，`SqlCacheDependencies.aspx`現在會顯示過時資料。 如果未遇到中斷點時，可能是因為輪詢服務時有不尚未引發後的資料已變更。 請記住輪詢服務正在檢查對`Products`資料表每個`pollTime`毫秒，因此沒有基礎資料更新時，與時收回快取的資料之間的延遲。

> [!NOTE]
> 這項延遲是比較可能會編輯透過在 GridView 的產品時，會出現`SqlCacheDependencies.aspx`。 在[架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程中，我們加入`MasterCacheKeyArray`快取相依性，確保透過正在編輯的資料`ProductsCL`類別的`UpdateProduct`方法已從快取收回。 不過，我們取代此快取相依性時修改`AddCacheItem`稍早在此步驟中的方法，因此`ProductsCL`類別會持續輪詢系統資訊的變更之前顯示的快取的資料`Products`資料表。 我們會看到如何重新引入`MasterCacheKeyArray`快取在步驟 7 中的相依性。


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>步驟 7： 將多個相依性關聯的快取的項目

請記得，`MasterCacheKeyArray`快取相依性用來確保*所有*內它相關聯的任何單一項目更新時從快取收回產品相關資料。 例如，`GetProductsByCategoryID(categoryID)`方法快取`ProductsDataTables`每個執行個體唯一*categoryID*值。 如果其中一個物件會將它收回，`MasterCacheKeyArray`快取相依性可確保，其他人也會移除。 沒有此快取相依性，快取的資料修改時，可能會存在其他快取的產品資料可能過期。 因此，它很重要，我們會維護 s`MasterCacheKeyArray`使用 SQL 快取相依性時，快取相依性。 不過，資料快取 s`Insert`方法只允許單一相依性物件。

此外，當使用 SQL 快取相依性我們需要將做為相依性的多個資料庫資料表產生關聯。 例如，`ProductsDataTable`快取中`ProductsCL`類別包含每個產品類別目錄] 和 [供應商名稱，但`AddCacheItem`方法只使用相依性上`Products`。 在此情況下，如果使用者更新的類別目錄或供應商名稱快取的產品資料會保留在快取，並已過期。 因此，我們想要讓資料快取的產品相依於不只`Products`資料表，但在`Categories`和`Suppliers`也會針對資料表。

[ `AggregateCacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx)提供一種方法將多個相依性與快取項目產生關聯。 藉由建立啟動`AggregateCacheDependency`執行個體。 接下來，加入相依性使用一組`AggregateCacheDependency`s`Add`方法。 當插入項目資料快取之後，傳入`AggregateCacheDependency`執行個體。 當*任何*的`AggregateCacheDependency`s 執行個體的相依性變更，將會收回快取的項目。

下圖顯示更新的程式碼，如`ProductsCL`類別的`AddCacheItem`方法。 此方法會建立`MasterCacheKeyArray`快取相依性，連同`SqlCacheDependency`物件`Products`， `Categories`，和`Suppliers`資料表。 所有結合成一個`AggregateCacheDependency`名為物件`aggregateDependencies`，接著會傳遞至`Insert`方法。


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

測試這個新的程式碼時。現在會變更為`Products`， `Categories`，或`Suppliers`資料表會導致快取的資料被收回。 此外，`ProductsCL`類別 s`UpdateProduct`方法會呼叫編輯 GridView 透過產品時，收回`MasterCacheKeyArray`快取相依性，這會導致的快取`ProductsDataTable`收回和要在下一次重新擷取資料要求。

> [!NOTE]
> SQL 快取相依性也可用以[輸出快取](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)。 如需這項功能的示範，請參閱：[使用 ASP.NET 輸出快取與 SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)。


## <a name="summary"></a>總結

當快取資料庫的資料，資料將在理想情況下保留在快取修改之前，都會在資料庫中。 ASP.NET 2.0，可建立及宣告式和程式設計案例中使用 SQL 快取相依性。 其中一個這種方法的挑戰是探索已修改的資料。 Microsoft SQL Server 2005 的完整版本提供通知功能，可以在查詢結果變更時警示應用程式。 Express Edition 的 SQL Server 2005 和 SQL Server 的舊版本，必須改為使用輪詢系統。 幸運的是，設定必要的輪詢基礎結構就相當簡單。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [Microsoft SQL Server 2005 中使用查詢通知](https://msdn.microsoft.com/library/ms175110.aspx)
- [建立查詢通知](https://msdn.microsoft.com/library/ms188669.aspx)
- [在 ASP.NET 中使用快取`SqlCacheDependency`類別](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server 註冊工具 (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [概觀 `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 此教學課程中的前導檢閱者已馬可 Rangel、 本文菲和 Hilton Giesenow。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-at-application-startup-cs.md)
> [下一頁](caching-data-with-the-objectdatasource-vb.md)
