---
uid: web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
title: 使用 SQL 快取相依性 (C#) |Microsoft Docs
author: rick-anderson
description: 最簡單的快取策略是允許為指定的一段時間之後到期的快取的資料。 但這個簡單的方法表示，快取的資料 maintai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 05/30/2007
ms.topic: article
ms.assetid: 0e91842c-7f10-4aed-8c23-4ee3e2774014
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/data-access/caching-data/using-sql-cache-dependencies-cs
msc.type: authoredcontent
ms.openlocfilehash: 16766ce7d741a9bcaea7cc676ed87978ea9f9f68
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37385900"
---
<a name="using-sql-cache-dependencies-c"></a>使用 SQL 快取相依性 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_61_CS.zip)或[下載 PDF](using-sql-cache-dependencies-cs/_static/datatutorial61cs1.pdf)

> 最簡單的快取策略是允許為指定的一段時間之後到期的快取的資料。 但這個簡單的方式表示快取的資料會維護其基礎資料來源，導致過時的資料保留太長或太快過期的目前資料沒有關聯。 更好的方法是使用 SqlCacheDependency 類別，讓資料在其基礎資料已修改的 SQL database 直到快取。 本教學課程會說明方法。


## <a name="introduction"></a>簡介

在中，檢查快取技術[使用 ObjectDataSource 快取的資料](caching-data-with-the-objectdatasource-cs.md)並[架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程使用以時間為基礎的到期之後指定收回快取的資料一段時間。 這種方法是最簡單的方式，來平衡的資料過期針對快取的效能提升。 選取的時間到期*x*秒，為網頁開發人員 concedes 享受只快取的效能優勢*x*秒，但可以放手不管她的資料將永遠不會過時時間超過最大值*x*秒。 當然，對於靜態資料， *x*可擴充的 web 應用程式存留期已檢查中做[快取的資料，在應用程式啟動](caching-data-at-application-startup-cs.md)教學課程。

當快取資料庫的資料，以時間為基礎的到期通常會選擇以其方便使用，但通常是不適當的解決方案。 在理想情況下，直到資料庫中已修改基礎資料的資料庫資料會保持快取則只會收回快取。 這種方法達到最大快取的效能優勢，並且降到最低的持續時間的過時資料。 不過，為了享有這些權益有必須知道當基礎資料庫資料已經過修改，因此收回對應的項目從快取的位置中的某些系統。 ASP.NET 2.0 中，網頁程式開發人員之前，負責實作此系統。

ASP.NET 2.0 提供[`SqlCacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.sqlcachedependency.aspx)和必要的基礎結構，以判斷何時變更發生在資料庫中，以便對應快取項目可收回。 有兩種技術來判斷基礎資料已變更時： 告知和輪詢。 討論過告知和輪詢之間的差異之後, 我們會建立基礎結構需要支援輪詢，接著探討如何使用`SqlCacheDependency`類別中宣告式和以程式設計方式的案例。

## <a name="understanding-notification-and-polling"></a>了解告知和輪詢

有兩種技術，可用來判斷當已經修改資料庫中的資料： 告知和輪詢。 通知，資料庫會自動發出警示，ASP.NET 執行階段，當上一次執行查詢後已經變更的特定查詢的結果，此時會收回快取與查詢相關聯的項目。 輪詢，與資料庫伺服器會維護當上次已更新特定資料表的相關資訊。 ASP.NET 執行階段會定期輪詢要檢查資料表已變更的資料庫因為它們已進入快取。 其資料已修改這些資料表有收回其相關聯的快取項目。

通知選項需要較少的安裝程式比輪詢，而且會更輕微，因為它會追蹤在查詢層級，而不是在資料表層級的變更。 不幸的是，通知中才有完整的版本的 Microsoft SQL Server 2005 （亦即，非 Express 版本）。 不過，輪詢選項可用於所有版本的 Microsoft SQL Server 7.0 到 2005年。 由於這些教學課程使用 SQL Server 2005 Express edition，我們將著重在設定和使用輪詢選項。 在本教學課程中進一步在 SQL Server 2005 的通知功能的資源的結尾處，請參閱進一步閱讀 > 一節。

輪詢，資料庫必須設定要包含一個名為資料表`AspNet_SqlCacheTablesForChangeNotification`具有三個資料行- `tableName`， `notificationCreated`，和`changeId`。 此資料表包含有可能需要使用 web 應用程式中的 SQL 快取相依性中的資料的每個資料表的資料列。 `tableName`資料行指定名稱的資料表，而`notificationCreated`表示的日期和時間資料列已加入至資料表。 `changeId` Sloupec je typu`int`且初始值為 0。 其值會隨著每項修改資料表。

除了`AspNet_SqlCacheTablesForChangeNotification`資料表中，資料庫也必須包含觸發程序在每個資料表，可能會出現在 SQL 快取相依性。 這些觸發程序會執行插入、 更新或刪除資料列時，並遞增資料表 s`changeId`中的值`AspNet_SqlCacheTablesForChangeNotification`。

ASP.NET 執行階段會追蹤目前`changeId`資料表快取資料使用時`SqlCacheDependency`物件。 定期檢查資料庫和任何`SqlCacheDependency`物件，而其`changeId`不同於資料庫中的值自不同收回`changeId`值會指出已變更資料表的資料快取後。

## <a name="step-1-exploring-theaspnetregsqlexecommand-line-program"></a>步驟 1： 瀏覽`aspnet_regsql.exe`命令列程式

使用輪詢方法的資料庫必須是安裝程式，以包含上面所述的基礎結構： 預先定義的資料表 (`AspNet_SqlCacheTablesForChangeNotification`)，少數幾個預存程序和觸發程序在每個可用於在 web 中的 SQL 快取相依性的資料表應用程式。 透過命令列程式，則可以建立這些資料表、 預存程序和觸發程序`aspnet_regsql.exe`，其位在`$WINDOWS$\Microsoft.NET\Framework\version`資料夾。 若要建立`AspNet_SqlCacheTablesForChangeNotification`資料表和相關聯的預存程序，從命令列執行下列命令：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample1.cmd)]

> [!NOTE]
> 若要執行指定的資料庫登入必須是這些命令[ `db_securityadmin` ](https://msdn.microsoft.com/library/ms188685.aspx)並[ `db_ddladmin` ](https://msdn.microsoft.com/library/ms190667.aspx)角色。 若要檢查 T-SQL 傳送至資料庫`aspnet_regsql.exe`命令列程式，請參閱[此部落格文章](http://scottonwriting.net/sowblog/posts/10709.aspx)。


例如，若要將輪詢的基礎結構新增至 Microsoft SQL Server 資料庫名為`pubs`名為資料庫伺服器上`ScottsServer`使用 Windows 驗證，請導覽至適當的目錄，並且從命令列中，輸入：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample2.cmd)]

在新增資料庫層級基礎結構之後，我們需要加入將用於 SQL 快取相依性的資料表中的觸發程序。 使用 `aspnet_regsql.exe`命令列程式一次，但指定資料表名稱使用`-t`切換，而不是使用`-ed`切換使用`-et`，就像這樣：


[!code-html[Main](using-sql-cache-dependencies-cs/samples/sample3.html)]

若要新增至觸發程序`authors`和`titles`表格`pubs`上的資料庫`ScottsServer`，使用：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample4.cmd)]

本教學課程新增至觸發程序`Products`， `Categories`，和`Suppliers`資料表。 我們將探討在步驟 3 中的特定命令列語法。

## <a name="step-2-referencing-a-microsoft-sql-server-2005-express-edition-database-inappdata"></a>步驟 2： 參考中的 Microsoft SQL Server 2005 Express Edition 資料庫`App_Data`

`aspnet_regsql.exe`命令列程式需要的資料庫和伺服器名稱，才能新增必要的輪詢基礎結構。 什麼是 Microsoft SQL Server 2005 Express 資料庫所在的資料庫和伺服器名稱，但`App_Data`資料夾嗎？ 而不必探索的資料庫和伺服器名稱是什麼，我已找到的最簡單的方法是將附加的資料庫`localhost\SQLExpress`資料庫執行個體，並重新命名資料使用[SQL Server Management Studio](https://msdn.microsoft.com/library/ms174173.aspx)。 如果您有一個在電腦上安裝的 SQL Server 2005 的完整版本，則您可能已在電腦上安裝 SQL Server Management Studio。 如果您只有 Express edition，您可以下載免費[Microsoft SQL Server Management Studio Express Edition](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

開始關閉 Visual Studio。 接下來，開啟 SQL Server Management Studio，然後選擇 連接到`localhost\SQLExpress`伺服器使用 Windows 驗證。


![附加至 localhost\SQLExpress 伺服器](using-sql-cache-dependencies-cs/_static/image1.gif)

**圖 1**： 附加至`localhost\SQLExpress`伺服器


連接到伺服器之後，會顯示伺服器 Management Studio，並將其擁有的資料庫、 安全性和其他等等的子資料夾中。 以滑鼠右鍵按一下 [資料庫] 資料夾，然後選擇 [附加] 選項。 此時會出現 附加資料庫 對話方塊 （請參閱 圖 2）。 按一下 [新增] 按鈕，然後選取`NORTHWND.MDF`database 資料夾中您的 web 應用程式 s`App_Data`資料夾。


[![附加 NORTHWND。從 [App_Data] 資料夾的 MDF 資料庫](using-sql-cache-dependencies-cs/_static/image2.gif)](using-sql-cache-dependencies-cs/_static/image1.png)

**圖 2**： 附加`NORTHWND.MDF`從資料庫`App_Data`資料夾 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image2.png))


這會將資料庫新增至 [資料庫] 資料夾。 資料庫的名稱可能是資料庫檔案的完整路徑或完整路徑前面加上[GUID](http://en.wikipedia.org/wiki/Globally_Unique_Identifier)。 若要避免這個冗長的資料庫名稱中的類型時使用 aspnet\_regsql.exe 命令列工具，附加至更人類看得好記的名稱，以滑鼠右鍵按一下資料庫只資料庫重新命名，然後選擇重新命名。 我已重新命名為 DataTutorials 的我的資料庫。


![重新命名，附加的資料庫更人類易](using-sql-cache-dependencies-cs/_static/image3.gif)

**圖 3**： 重新命名，附加的資料庫更人類易


## <a name="step-3-adding-the-polling-infrastructure-to-the-northwind-database"></a>步驟 3： 加入 Northwind 資料庫中的輪詢基礎結構

既然我們已附加`NORTHWND.MDF`資料庫從`App_Data`資料夾中，我們準備好將輪詢基礎結構。 假設您已重新命名為 DataTutorials 的資料庫，請執行下列四個命令：


[!code-console[Main](using-sql-cache-dependencies-cs/samples/sample5.cmd)]

執行這些四個命令之後，Management Studio 中的資料庫名稱上按一下滑鼠右鍵，移至 工作 子功能表，然後選擇 卸離。 然後關閉 Management Studio，並重新開啟 Visual Studio。

一旦已重新開啟 Visual Studio，向下鑽研到透過伺服器總管資料庫。 請注意新的資料表 (`AspNet_SqlCacheTablesForChangeNotification`)，新的預存程序和觸發程序上`Products`， `Categories`，和`Suppliers`資料表。


![Database 現在包含必要的輪詢基礎結構](using-sql-cache-dependencies-cs/_static/image4.gif)

**圖 4**: Database 現在包含必要的輪詢基礎結構


## <a name="step-4-configuring-the-polling-service"></a>步驟 4： 設定輪詢服務

在資料庫中建立所需的資料表、 觸發程序和預存程序之後, 的最後一個步驟是設定的輪詢 」 服務，也就透過`Web.config`藉由指定資料庫，以使用與輪詢頻率，以毫秒為單位。 下列標記會輪詢一次每秒鐘 Northwind 資料庫。


[!code-xml[Main](using-sql-cache-dependencies-cs/samples/sample6.xml)]

`name`中的值`<add>`項目 (NorthwindDB) 與特定資料庫有關聯的人類看得懂的名稱。 當使用 SQL 快取相依性，我們必須參考此處定義以及資料表快取的資料為基礎的資料庫名稱。 我們將了解如何使用`SqlCacheDependency`類別來以程式設計方式建立與 SQL 快取相依性的關聯快取的步驟 6 中的資料。

一旦建立 SQL 快取相依性，輪詢系統將會連線到資料庫中定義`<databases>`項目每隔`pollTime`毫秒，然後執行`AspNet_SqlCachePollingStoredProcedure`預存程序。 這個預存程序-這加入備份在步驟 3 中使用`aspnet_regsql.exe`命令列工具-會傳回`tableName`並`changeId`中的每個記錄的值`AspNet_SqlCacheTablesForChangeNotification`。 過期的 SQL 快取相依性會從快取中收回。

`pollTime`設定導入了效能和資料過期之間有所取捨。 小型`pollTime`值增加到資料庫，要求的數目，但更快速地收回快取中的過時資料。 較大`pollTime`值可減少資料庫的要求數目，但是會增加後端的資料變更時，與相關的快取項目會收回時之間的延遲。 幸運的，資料庫要求會傳回多個資料列從簡單的輕量型表格 s 執行簡單的預存程序。 執行試驗不同，但`pollTime`值，以尋找適合的平衡資料庫存取和資料的過期，您的應用程式。 最小`pollTime`允許的值為 500。

> [!NOTE]
> 上述範例中提供單一`pollTime`中的值`<sqlCacheDependency>`項目，但是您可以選擇性地指定`pollTime`中的值`<add>`項目。 如果您有多個指定的資料庫，並想要自訂每個資料庫的輪詢頻率，這非常有用。


## <a name="step-5-declaratively-working-with-sql-cache-dependencies"></a>步驟 5： 以宣告方式使用 SQL 快取相依性

步驟 1 到 4 中，我們討論過如何安裝必要的資料庫基礎結構和設定輪詢系統。 使用此基礎結構就緒，我們現在可以將項目加入的資料快取搭配使用以程式設計方式或宣告式技術相關聯 SQL 快取相依性。 在此步驟中，我們將檢驗如何以宣告方式使用 SQL 快取相依性。 步驟 6 中我們將探討以程式設計的方式。

[快取的資料，使用 ObjectDataSource](caching-data-with-the-objectdatasource-cs.md)教學課程探索 ObjectDataSource 的宣告式快取功能。 只需要設定`EnableCaching`屬性，以`true`而`CacheDuration`某些時間間隔內的屬性，ObjectDataSource 會自動快取傳回來自其基礎的物件指定間隔內的資料。 ObjectDataSource 也可以使用一或多個 SQL 快取相依性。

若要示範如何以宣告方式使用 SQL 快取相依性，請開啟`SqlCacheDependencies.aspx`頁面中`Caching`資料夾，然後從 [工具箱] 拖曳至設計工具拖曳的 GridView。 設定 GridView s`ID`要`ProductsDeclarative`，並從它的智慧標籤，選擇 繫結至名為新 ObjectDataSource `ProductsDataSourceDeclarative`。


[![建立名為 ProductsDataSourceDeclarative 新 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image5.gif)](using-sql-cache-dependencies-cs/_static/image3.png)

**圖 5**： 建立新的 ObjectDataSource 具名`ProductsDataSourceDeclarative`([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image4.png))


設定要使用 ObjectDataSource`ProductsBLL`類別，並設定下拉式清單中選取的索引標籤，以`GetProducts()`。 在 [更新] 索引標籤中，選擇`UpdateProduct`三個輸入參數的多載`productName`， `unitPrice`，和`productID`。 在 INSERT 和 DELETE 的索引標籤中設定為 （無） 的下拉式清單。


[![UpdateProduct 多載使用三個輸入參數](using-sql-cache-dependencies-cs/_static/image6.gif)](using-sql-cache-dependencies-cs/_static/image5.png)

**圖 6**： 使用 UpdateProduct 多載具有三個輸入參數 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image6.png))


[![設定為 （無） 下拉式清單，以便進行插入和刪除索引標籤](using-sql-cache-dependencies-cs/_static/image7.gif)](using-sql-cache-dependencies-cs/_static/image7.png)

**圖 7**： 設定針對插入或刪除的索引標籤的下拉式清單中，為 （無） ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image8.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 和 CheckBoxFields GridView 裡的每個資料欄位。 移除所有的欄位，但`ProductName`， `CategoryName`，和`UnitPrice`，並視需要設定這些欄位的格式。 從 GridView s 智慧標籤，核取方塊啟用分頁、 啟用排序及啟用編輯。 Visual Studio 將會設定 ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`。 為了讓 GridView 的編輯功能，才能正常運作，移除這個屬性設回其預設值的宣告式語法從完全`{0}`。

最後，新增 標籤 Web 控制項上方的 GridView 和設定其`ID`屬性，以`ODSEvents`及其`EnableViewState`屬性設`false`。 進行這些變更之後，頁面 s 的宣告式標記看起來應該如下所示。 請注意，已進行的美觀的自訂項目不需要為了示範 SQL 快取相依性功能的 GridView 欄位的數目。


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample7.aspx)]

接下來，建立事件處理常式 ObjectDataSource s`Selecting`事件並在它中加入下列程式碼：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample8.cs)]

請記得，ObjectDataSource 的`Selecting`從其基礎物件擷取資料時，才，就會引發事件。 如果 ObjectDataSource 會從它自己的快取存取資料，是不會引發此事件。

現在，請瀏覽此頁面，透過瀏覽器。 因為我們尚未以實作任何快取，每次頁面、 排序或編輯的格線頁的 ve 應該會顯示文字、 選取事件引發，如 [圖 8] 所示。


[![每個分頁 GridView，編輯、 時間或排序，就會引發 ObjectDataSource 的選取事件](using-sql-cache-dependencies-cs/_static/image8.gif)](using-sql-cache-dependencies-cs/_static/image9.png)

**圖 8**: ObjectDataSource s`Selecting`事件會引發每次呼叫 GridView，、 編輯或排序 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image10.png))


如我們在中所見[使用 ObjectDataSource 快取的資料](caching-data-with-the-objectdatasource-cs.md)教學課程中，設定`EnableCaching`屬性設`true`會導致快取其資料，藉由指定的期間 ObjectDataSource 其`CacheDuration`屬性。 也有 ObjectDataSource [ `SqlCacheDependency`屬性](https://msdn.microsoft.com/library/system.web.ui.webcontrols.objectdatasource.sqlcachedependency.aspx)，其中將一個或多個 SQL 快取相依性新增至快取的資料使用模式：


[!code-css[Main](using-sql-cache-dependencies-cs/samples/sample9.css)]

何處*databaseName*中所指定的資料庫名稱`name`屬性`<add>`中的項目`Web.config`，和*tableName*是資料庫資料表的名稱。 比方說，若要建立快取資料無限期地 ObjectDataSource SQL 快取相依性為基礎針對 Northwind s`Products`資料表中，設定 ObjectDataSource s`EnableCaching`屬性設`true`及其`SqlCacheDependency`屬性NorthwindDB:Products。

> [!NOTE]
> 您可以使用 SQL 快取相依性*並*藉由設定以時間為基礎的到期日`EnableCaching`來`true`，`CacheDuration`時間間隔，以和`SqlCacheDependency`至資料庫和資料表名稱。 ObjectDataSource 會收回其資料，當時間為基礎的到期日為止或輪詢系統資訊已變更基礎資料庫資料，何者先發生。


在 GridView`SqlCacheDependencies.aspx`會顯示來自兩個資料表-資料`Products`並`Categories`(產品 s`CategoryName`透過擷取欄位`JOIN`上`Categories`)。 因此，我們需要指定兩個 SQL 快取相依性： NorthwindDB:Products;NorthwindDB:Categories。


[![設定來支援快取 使用 SQL 快取相依性產品和分類的 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image9.gif)](using-sql-cache-dependencies-cs/_static/image11.png)

**圖 9**： 設定上的 支援快取使用 SQL 快取相依性的 ObjectDataSource`Products`並`Categories`([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image12.png))


設定來支援快取 ObjectDataSource 之後, 重新瀏覽透過瀏覽器頁面。 同樣地，引發之文字選取事件應該會出現在第一次的頁面開啟，但應該會消失的分頁、 排序，或按一下 [編輯] 或 [取消] 按鈕時。 這是因為將資料載入 ObjectDataSource s 快取之後，它會保持有直到`Products`或`Categories`修改資料表，或透過 GridView 更新的資料。

逐頁查看方格，您會看到沒有選取事件引發之後的文字，開啟新的瀏覽器視窗，並瀏覽至 編輯、 插入及刪除區段的基本概念教學課程 (`~/EditInsertDelete/Basics.aspx`)。 更新的名稱或產品的價格。 然後，從第一個瀏覽器視窗中，檢視不同的資料頁、 排序方格中，或按一下資料列 s [編輯] 按鈕。 此時，請選取事件引發應該再次出現，因為基礎資料庫資料已修改 （請參閱 圖 10）。 如果文字不會出現，請稍候片刻並再試一次。 請記住，輪詢服務在檢查變更`Products`資料表每個`pollTime`毫秒，因此沒有更新基礎資料時，與時收回快取的資料之間的延遲。


[![修改 [產品] 資料表會收回快取的產品的資料](using-sql-cache-dependencies-cs/_static/image10.gif)](using-sql-cache-dependencies-cs/_static/image13.png)

**圖 10**： 修改 [產品] 資料表會收回快取產品的資料 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image14.png))


## <a name="step-6-programmatically-working-with-thesqlcachedependencyclass"></a>步驟 6： 以程式設計方式使用`SqlCacheDependency`類別

[架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程會探討使用個別的快取層，在架構中，而不是緊密結合使用 ObjectDataSource 快取的優點。 在本教學課程，我們建立`ProductsCL`類別示範以程式設計方式使用的資料快取。 若要利用快取層中的 SQL 快取相依性，使用`SqlCacheDependency`類別。

使用輪詢系統，`SqlCacheDependency`物件必須是特定的資料庫和資料表組相關聯。 例如，下列程式碼中，會建立`SqlCacheDependency`物件會根據 Northwind 資料庫的`Products`資料表：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample10.cs)]

這兩個輸入參數`SqlCacheDependency`s 建構函式分別為資料庫和資料表名稱。 使用 ObjectDataSource s`SqlCacheDependency`屬性所使用的資料庫名稱是在指定的值相同`name`屬性`<add>`中的項目`Web.config`。 資料表名稱是資料庫資料表的實際名稱。

若要建立關聯`SqlCacheDependency`加入至資料快取的項目，以使用其中一種`Insert`接受相依性的方法多載。 下列程式碼加入*值*無限期的持續時間的資料快取，但將它與`SqlCacheDependency`上`Products`資料表。 簡單來說，*值*將會保留在快取中，直到收回因為記憶體條件約束，或因為輪詢系統偵測到的`Products`資料表已變更，因為它已快取。


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample11.cs)]

快取層 s`ProductsCL`類別目前會快取的資料從`Products`資料表使用以時間為基礎的到期的 60 秒。 可讓此類別的更新，使其改為使用 SQL 快取相依性的 s。 `ProductsCL`類別的`AddCacheItem`方法，也就是負責將資料加入至快取，目前包含下列程式碼：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample12.cs)]

此程式碼更新成使用`SqlCacheDependency`物件，而不是`MasterCacheKeyArray`快取相依性：


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample13.cs)]

若要測試這項功能，新增至下方的現有頁面的 GridView `ProductsDeclarative` GridView。 設定這個新的 GridView s`ID`要`ProductsProgrammatic`和它的智慧標籤，透過繫結至名為新 ObjectDataSource `ProductsDataSourceProgrammatic`。 設定要使用 ObjectDataSource`ProductsCL`類別，在 選取的下拉式清單和更新索引標籤，以設定`GetProducts`和`UpdateProduct`分別。


[![設定使用 ProductsCL 類別 ObjectDataSource](using-sql-cache-dependencies-cs/_static/image11.gif)](using-sql-cache-dependencies-cs/_static/image15.png)

**圖 11**： 設定要使用 ObjectDataSource`ProductsCL`類別 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image16.png))


[![從選取的索引標籤的下拉式清單中選取 GetProducts 方法](using-sql-cache-dependencies-cs/_static/image12.gif)](using-sql-cache-dependencies-cs/_static/image17.png)

**圖 12**： 選取 `GetProducts`方法，從下拉式清單選取索引標籤 s ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image18.png))


[![從 [更新] 索引標籤 s] 下拉式清單選擇 UpdateProduct 方法](using-sql-cache-dependencies-cs/_static/image13.gif)](using-sql-cache-dependencies-cs/_static/image19.png)

**圖 13**： 從 [更新] 索引標籤的下拉式清單中選擇 UpdateProduct 方法 ([按一下以檢視完整大小的影像](using-sql-cache-dependencies-cs/_static/image20.png))


完成設定資料來源精靈之後，Visual Studio 會建立 BoundFields 和 CheckBoxFields GridView 裡的每個資料欄位。 像使用此頁面加入第一個 GridView，移除所有的欄位，但`ProductName`， `CategoryName`，和`UnitPrice`，並視需要設定這些欄位的格式。 從 GridView s 智慧標籤，核取方塊啟用分頁、 啟用排序及啟用編輯。 如同`ProductsDataSourceDeclarative`ObjectDataSource、 Visual Studio 將會設定`ProductsDataSourceProgrammatic`ObjectDataSource s`OldValuesParameterFormatString`屬性設`original_{0}`。 在 GridView 的編輯功能，若要正常運作，請將此屬性設定回`{0}`（或完全移除的宣告式語法的屬性指派）。

完成這些工作之後, 產生的 GridView 和 ObjectDataSource 的宣告式標記看起來應該如下所示：


[!code-aspx[Main](using-sql-cache-dependencies-cs/samples/sample14.aspx)]

若要測試 SQL 快取層中的快取相依性，請設定的中斷點`ProductCL`類別的`AddCacheItem`方法，然後開始偵錯。 當您第一次造訪`SqlCacheDependencies.aspx`，第一次要求和放入快取資料時，應叫用中斷點。 接下來，移至 GridView 內的另一個頁面，或其中一個資料行排序。 這會導致 GridView，以重新查詢資料，但資料應位於後，快取`Products`尚未修改資料庫資料表。 如果資料是重複找不到快取中，請確定您的電腦上有可用的記憶體不足的並再試一次。

之後的幾個頁面的 GridView 進行分頁，開啟 第二個瀏覽器視窗並瀏覽至 編輯、 插入及刪除區段的基本概念教學課程 (`~/EditInsertDelete/Basics.aspx`)。 更新 Products 資料表的記錄，然後，從第一個瀏覽器視窗中，檢視新的頁面或按一下其中一個排序的標頭。

在此案例中您會看到下列其中一種： 其中一個中斷點會被叫用，表示快取的資料已收回因為資料庫中的變更或者，將不會叫用中斷點，也就是說，`SqlCacheDependencies.aspx`現在會顯示過時資料。 如果未叫用中斷點時，它可能是因為 「 輪詢 」 服務不尚未引發資料已變更。 請記住，輪詢服務在檢查變更`Products`資料表每個`pollTime`毫秒，因此沒有更新基礎資料時，與時收回快取的資料之間的延遲。

> [!NOTE]
> 此延遲是更容易在編輯其中一個透過在 GridView 產品時顯示`SqlCacheDependencies.aspx`。 在 [架構中的快取資料](caching-data-in-the-architecture-cs.md)教學課程，我們已新增`MasterCacheKeyArray`快取相依性，以確保透過正在編輯的資料`ProductsCL`類別的`UpdateProduct`方法已從快取收回。 不過，我們取代此快取相依性時修改`AddCacheItem`稍早在此步驟中的方法，因此`ProductsCL`類別會繼續顯示快取的資料，直到輪詢系統資訊的變更`Products`資料表。 我們會了解如何重新引入`MasterCacheKeyArray`快取在步驟 7 中的相依性。


## <a name="step-7-associating-multiple-dependencies-with-a-cached-item"></a>步驟 7： 將多個相依性關聯的快取的項目

請記得，`MasterCacheKeyArray`快取相依性用來確保*所有*其內相關聯的任何單一項目更新時，產品相關的資料從快取收回。 例如，`GetProductsByCategoryID(categoryID)`方法的快取`ProductsDataTables`執行個體，每個唯一*categoryID*值。 如果其中一個物件收回，`MasterCacheKeyArray`快取相依性可確保其他人會一併移除。 沒有此快取相依性，當快取的資料遭到修改時可能會有的其他快取的產品資料可能過期。 因此，它很重要，我們會維護 s`MasterCacheKeyArray`快取相依性，當使用 SQL 快取相依性。 不過，資料快取 s`Insert`方法只允許單一的相依性物件。

此外，使用 SQL 快取相依性時我們需要建立做為相依性的多個資料庫資料表的關聯。 例如，`ProductsDataTable`快取中`ProductsCL`類別包含 category 與 supplier 每項產品的名稱，但`AddCacheItem`方法只使用相依性上`Products`。 在此情況下，如果使用者更新的分類或供應商的名稱快取的產品的資料會保留在快取，而且已經過期。 因此，我們想讓快取的產品的資料相依於不僅`Products`資料表，但是在`Categories`和`Suppliers`也會針對資料表。

[ `AggregateCacheDependency`類別](https://msdn.microsoft.com/library/system.web.caching.aggregatecachedependency.aspx)提供方法來將多個相依性關聯的快取項目。 開始建立`AggregateCacheDependency`執行個體。 接下來，新增相依性使用一組`AggregateCacheDependency`s`Add`方法。 當插入之項目的資料快取之後，傳入`AggregateCacheDependency`執行個體。 當*任何*的`AggregateCacheDependency`執行個體 s 的相依性變更，將會收回快取的項目。

下圖顯示更新的程式碼，如`ProductsCL`類別的`AddCacheItem`方法。 此方法會建立`MasterCacheKeyArray`快取相依性，連同`SqlCacheDependency`物件`Products`， `Categories`，和`Suppliers`資料表。 這些全部結合成一個`AggregateCacheDependency`名為物件`aggregateDependencies`，然後傳遞到`Insert`方法。


[!code-csharp[Main](using-sql-cache-dependencies-cs/samples/sample15.cs)]

測試出這個新的程式碼。現在會變更為`Products`， `Categories`，或`Suppliers`資料表會導致快取的資料被收回。 此外，`ProductsCL`類別 s`UpdateProduct`方法，稱為編輯透過 GridView 產品時，收回`MasterCacheKeyArray`快取相依性，這會導致快取`ProductsDataTable`被收回和下一步 重新擷取資料要求。

> [!NOTE]
> SQL 快取相依性也會搭配[輸出快取](https://quickstarts.asp.net/QuickStartv20/aspnet/doc/caching/output.aspx)。 如需這項功能的示範，請參閱：[使用 ASP.NET 輸出快取與 SQL Server](https://msdn.microsoft.com/library/e3w8402y(VS.80).aspx)。


## <a name="summary"></a>總結

當快取資料庫的資料，資料會在理想情況下保持快取中修改之前，都會在資料庫中。 使用 ASP.NET 2.0，可以建立並宣告與程式設計案例中使用 SQL 快取相依性。 使用此方法的挑戰之一是探索已修改資料時。 Microsoft SQL Server 2005 的完整版本提供通知功能，可在查詢結果變更時警示應用程式。 Express Edition 的 SQL Server 2005 和 SQL server 的較舊版本，必須改為使用輪詢系統。 幸運的是，設定必要的輪詢基礎結構就相當簡單。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [Microsoft SQL Server 2005 中使用查詢通知](https://msdn.microsoft.com/library/ms175110.aspx)
- [建立查詢通知](https://msdn.microsoft.com/library/ms188669.aspx)
- [在 ASP.NET 中使用快取`SqlCacheDependency`類別](https://msdn.microsoft.com/library/ms178604(VS.80).aspx)
- [ASP.NET SQL Server 註冊工具 (`aspnet_regsql.exe`)](https://msdn.microsoft.com/library/ms229862(vs.80).aspx)
- [概觀 `SqlCacheDependency`](http://www.aspnetresources.com/blog/sql_cache_depedency_overview.aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者是馬可 Rangel、 Teresa Murphy 和 Hilton Giesenow。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](caching-data-at-application-startup-cs.md)
> [下一頁](caching-data-with-the-objectdatasource-vb.md)
