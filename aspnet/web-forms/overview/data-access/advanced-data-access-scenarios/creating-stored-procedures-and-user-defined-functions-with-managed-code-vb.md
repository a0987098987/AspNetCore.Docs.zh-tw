---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
title: "建立預存程序和使用者定義函數與 Managed 程式碼 (VB) |Microsoft 文件"
author: rick-anderson
description: "Microsoft SQL Server 2005 整合與.NET Common Language Runtime 可讓開發人員建立透過 managed 程式碼的資料庫物件。 本教學課程中..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: 8be9a51b-ea6b-46c7-bfa2-476d9b14c24c
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-vb
msc.type: authoredcontent
ms.openlocfilehash: efec52c4085c24b1d6227a86f7c435ca657e493c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-vb"></a>建立預存程序和使用者定義函數以 Managed 程式碼 (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_VB.zip)或[下載 PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/datatutorial75vb1.pdf)

> Microsoft SQL Server 2005 整合與.NET Common Language Runtime 可讓開發人員建立透過 managed 程式碼的資料庫物件。 本教學課程示範如何建立 managed 預存程序和 managed 使用者定義函式，以您的 Visual Basic 或 C# 程式碼。 我們也會看到這些版本的 Visual Studio 如何讓您偵錯這類 managed 的資料庫物件。


## <a name="introduction"></a>簡介

如 Microsoft 的 SQL Server 2005 的資料庫使用[Transact-Structured 查詢語言 (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL)插入、 修改和擷取資料。 大多數的資料庫系統包含群組的 SQL 陳述式，然後以單一、 可重複使用單位必須執行一系列的建構。 預存程序是一個範例。 另一個是*使用者定義函數*(Udf)，我們將在步驟 9 中的更詳細地檢查的建構。

基本上，SQL 被針對使用的資料集。 `SELECT`， `UPDATE`，和`DELETE`陳述式原本就是適用於對應資料表中的所有記錄和只受限於其`WHERE`子句。 尚未有許多設計使用一次一筆記錄，並處理純量資料處理的語言功能。 [`CURSOR`s](http://www.sqlteam.com/item.asp?ItemID=553)在 if 透過下列其中一次讓一組記錄。 字串操作函數，例如`LEFT`， `CHARINDEX`，和`PATINDEX`使用純量資料。 SQL 也包含控制流程陳述式，例如`IF`和`WHILE`。

Microsoft SQL Server 2005 之前, 預存程序和 Udf 只可定義為 T-SQL 陳述式的集合。 SQL Server 2005，不過，設計來提供與整合[Common Language Runtime (CLR)](https://msdn.microsoft.com/en-us/netframework/aa497266.aspx)，也就是所有的.NET 組件所使用的執行階段。 因此，預存程序和 Udf 的 SQL Server 2005 資料庫中可以建立使用 managed 程式碼。 也就是說，您可以建立預存程序或 UDF 為 Visual Basic 類別中的方法。 這可讓這些預存程序和 Udf 利用.NET Framework 中，以及您自己自訂的類別的功能。

在此教學課程中我們將檢查如何建立 managed 預存程序和使用者定義函式，以及如何將它們整合到我們的 Northwind 資料庫。 Let s 開始 ！

> [!NOTE]
> Managed 的資料庫物件會提供一些優於與其 SQL 對應項目。 語言很豐富，並熟悉度及重複使用現有的程式碼和邏輯的功能是將主要優點。 但 managed 的資料庫物件不可能使用不會涉及多程序邏輯的資料集時做比較沒有效率。 如需優點使用 managed 程式碼與 T-SQL 的更完整討論，請參閱[優點的使用 Managed 程式碼建立資料庫物件](https://msdn.microsoft.com/en-us/library/k2e1fb36(VS.80).aspx)。


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>步驟 1： 移動 Northwind 資料庫`App_Data`

所有教學課程到目前為止已使用 Microsoft SQL Server 2005 Express 的 Edition 資料庫檔案中的 web 應用程式 s`App_Data`資料夾。 將資料庫中的放置`App_Data`簡化發佈和執行這些教學課程的所有檔案位在一個目錄內所以所需的任何其他組態步驟，以測試本教學課程。

此教學課程中，不過，o d e s Northwind 將資料庫移出`App_Data`明確地將它登錄的 SQL Server 2005 Express Edition 資料庫執行個體。 雖然我們可以在此教學課程中的資料庫與執行步驟`App_Data`資料夾中，數個步驟會簡單許多藉由明確地註冊資料庫與 SQL Server 2005 Express Edition 資料庫執行個體。

下載此教學課程中有兩個資料庫檔案-`NORTHWND.MDF`和`NORTHWND_log.LDF`-資料夾中放置`DataFiles`。 如果您要遵照以及您自己的實作，教學課程，請關閉 Visual Studio，並移動`NORTHWND.MDF`和`NORTHWND_log.LDF`檔案從網站的`App_Data`外部網站資料夾的資料夾。 一旦將資料庫檔案已移至另一個資料夾我們必須向 Northwind 資料庫的 SQL Server 2005 Express Edition 資料庫執行個體。 這可以從 SQL Server Management Studio 完成。 如果您有非-Express Edition 的 SQL Server 2005 電腦上安裝然後您應該已有安裝 Management Studio。 如果您只在電腦上的 SQL Server 2005 Express Edition，則請花一點時間來下載並安裝[Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

啟動 SQL Server Management Studio。 如圖 1 所示，Management Studio 一開始會詢問您要連接哪些伺服器。 Localhost\SQLExpress 輸入伺服器名稱、 在驗證] 下拉式清單中，選擇 [Windows 驗證並按一下 [連線]。


![連接到適當的資料庫執行個體](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image1.png)

**圖 1**： 連接到適當的資料庫執行個體


一旦您已連線，[物件總管] 視窗會列出 SQL Server 2005 Express Edition 資料庫執行個體，包括資料庫、 安全性資訊、 管理選項及其他等等的相關資訊。

我們需要將資料庫附加到 Northwind 在`DataFiles`資料夾 （或任何您可能會將其移動） 到 SQL Server 2005 Express Edition 資料庫執行個體。 以滑鼠右鍵按一下 [資料庫] 資料夾，並從內容功能表選擇 [附加] 選項。 這會顯示 [附加資料庫] 對話方塊。 按一下 [新增] 按鈕，向下鑽研至適當`NORTHWND.MDF`檔案，然後按一下 [確定]。 此時您的畫面看起來應該類似於圖 2。


[![連接到適當的資料庫執行個體](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image2.png)

**圖 2**： 連接到適當的資料庫執行個體 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image4.png))


> [!NOTE]
> 當連接到 SQL Server 2005 Express Edition 執行個體，透過 Management Studio 附加資料庫 對話方塊中不允許您向下鑽研到使用者設定檔的目錄，例如 我的文件。 因此，請確定將`NORTHWND.MDF`和`NORTHWND_log.LDF`非使用者設定檔的目錄中的檔案。


按一下 [確定] 按鈕，將資料庫附加到。 [附加資料庫] 對話方塊會關閉，而 [物件總管] 現在應該會列出剛才附加的資料庫。 有可能是資料庫的名稱，例如 Northwind `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`。 重新命名至 Northwind 資料庫上按一下滑鼠右鍵，然後選擇 重新命名的資料庫。


![重新命名為 Northwind 的資料庫](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image5.png)

**圖 3**： 重新命名為 Northwind 的資料庫


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>步驟 2: Visual Studio 中建立新的方案和 SQL Server 專案

SQL Server 2005 中建立 managed 預存程序或 Udf 我們會撰寫的預存程序和 UDF 邏輯做為類別中的 Visual Basic 程式碼。 一旦在撰寫程式碼，我們必須將此類別編譯為組件 (`.dll`檔案)、 註冊組件與 SQL Server 資料庫，然後再建立預存程序或 UDF 物件中的資料庫，指向對應中的方法組件。 這些步驟可以所有手動執行。 我們可以建立程式碼中的任何文字編輯器、 從命令列使用 Visual Basic 編譯器編譯 (`vbc.exe`)，向資料庫使用[ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/en-us/library/ms189524.aspx)命令或從 Management Studio，並加入儲存程序或 UDF 物件透過類似的方式。 幸運的是，專業人員和小組的系統版本的 Visual Studio 包含可自動化這些工作的 SQL Server 專案類型。 在此教學課程中我們將逐步引導使用 SQL Server 專案類型來建立 managed 預存程序和 UDF。

> [!NOTE]
> 如果您使用 Visual Web Developer 或 Standard 版本的 Visual Studio，然後您必須改為使用手動方法。 步驟 13 提供手動執行這些步驟的詳細的指示。 建議您閱讀 12 中的步驟 2，再讀取步驟 13，因為這些步驟包括重要必須套用不論您使用 Visual Studio 的哪些版本的 SQL Server 組態指示。


開啟 Visual Studio 以開始。 從 [檔案] 功能表中，選擇 [新增專案] 以顯示 [新增專案] 對話方塊 （請參閱圖 4）。 向下鑽研至資料庫專案類型，以及從右邊列出的範本，然後選擇 建立新的 SQL Server 專案。 我已選擇要此專案的名稱，`ManagedDatabaseConstructs`並放在名為方案內`Tutorial75`。


[![建立新的 SQL Server 專案](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image6.png)

**圖 4**： 建立新的 SQL Server 專案 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image8.png))


按一下 [確定] 按鈕，在建立方案和 SQL Server 專案的 [新增專案] 對話方塊中。

SQL Server 專案繫結至特定資料庫。 因此，建立新的 SQL Server 專案之後我們會立即要求您指定這項資訊。 圖 5 顯示已指向我們註冊回在步驟 1 中的 SQL Server 2005 Express Edition 資料庫執行個體中的 Northwind 資料庫填寫 [新的資料庫參考] 對話方塊。


![將 SQL Server 專案與 Northwind 資料庫產生關聯](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image9.png)

**圖 5**： 將 SQL Server 專案與 Northwind 資料庫產生關聯


若要偵錯 managed 預存程序和 Udf 我們將建立此專案中，我們需要啟用 SQL/CLR 偵錯連接的支援。 每當一樣 （圖 5），將 SQL Server 專案產生關聯新的資料庫時，Visual Studio 會詢問您是否我們想要讓連接上的 SQL/CLR 偵錯 （請參閱圖 6）。 按一下 [是]。


![啟用 SQL/CLR 偵錯](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image10.png)

**圖 6**： 啟用 SQL/CLR 偵錯


此時新的 SQL Server 專案有已加入至方案。 它包含名為的資料夾`Test Scripts`名為的檔案與`Test.sql`，用於偵錯專案中建立的 managed 的資料庫物件。 我們將探討步驟 12 中的偵錯。

我們現在可以將新的 managed 預存程序和 Udf，加入此專案中，但我們還是會讓之前先包含我們現有的 web 應用程式方案中。 從 [檔案] 功能表選取 [加入] 選項，然後選擇現有的網站。 瀏覽至適當的網站資料夾，然後按一下 [確定]。 如圖 7 所示，這將會更新以包含兩個專案的方案： 在網站和`ManagedDatabaseConstructs`SQL Server 專案。


![[方案總管] 現在包含兩個專案](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image11.png)

**圖 7**: [方案總管] 現在包含兩個專案


`NORTHWNDConnectionString`值`Web.config`目前參考`NORTHWND.MDF`檔案`App_Data`資料夾。 由於我們移除了這個資料庫`App_Data`及明確註冊 SQL Server 2005 Express Edition 資料庫執行個體中，我們要跟著更新`NORTHWNDConnectionString`值。 開啟`Web.config`檔案中的網站和變更`NORTHWNDConnectionString`值，讓連接字串： `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`。 此變更後，您`<connectionStrings>`一節中`Web.config`看起來應該類似下列：


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample1.xml)]

> [!NOTE]
> 中所述[前述教學課程](debugging-stored-procedures-vb.md)，當偵錯 SQL Server 物件，從用戶端應用程式，例如 ASP.NET 網站，我們需要停用連接共用。 如上所示的連接字串會停用連線共用 ( `Pooling=false` )。 如果您不打算在偵錯 managed 預存程序和 Udf 從 ASP.NET 網站上，啟用連接共用。


## <a name="step-3-creating-a-managed-stored-procedure"></a>步驟 3： 建立 Managed 預存程序

若要將受管理的預存程序新增至 Northwind 資料庫，我們需要建立，預存程序做為 SQL Server 專案中的方法。 從 方案總管 中，以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案名稱，然後選擇 加入新項目。 這會顯示加入新項目 對話方塊，列出可以加入至專案的 managed 的資料庫物件的類型。 如圖 8 所示，這包括預存程序和使用者定義函數和其他項目。

可讓 s，將只會傳回所有已中止的產品的預存程序來開始。 將新的預存程序檔案`GetDiscontinuedProducts.vb`。


[![加入新的預存程序，以名為 GetDiscontinuedProducts.vb](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image12.png)

**圖 8**： 加入新預存程序命名為`GetDiscontinuedProducts.vb`([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image14.png))


這會建立新的 Visual Basic 類別檔具有下列內容：


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample2.vb)]

請注意，預存程序實作為`Shared`方法內`Partial`類別檔案命名為`StoredProcedures`。 此外，`GetDiscontinuedProducts`方法以裝飾[`SqlProcedure`屬性](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlprocedureattribute.aspx)，將會標示為預存程序的方法。

下列程式碼會建立`SqlCommand`物件，然後設定其`CommandText`至`SELECT`查詢會傳回所有資料行從`Products`其資料表用於產品`Discontinued`欄位等於 1。 然後執行命令，並將結果傳送回用戶端應用程式。 將此程式碼加入`GetDiscontinuedProducts`方法。


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample3.vb)]

所有的 managed 的資料庫物件都具有存取權[`SqlContext`物件](https://msdn.microsoft.com/en-us/library/ms131108.aspx)，代表呼叫端的內容。 `SqlContext`提供存取[`SqlPipe`物件](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.aspx)透過其[`Pipe`屬性](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx)。 這`SqlPipe`物件可用於用渡船 SQL Server 資料庫與呼叫的應用程式的資訊。 正如其名， [ `ExecuteAndSend`方法](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx)執行傳入的`SqlCommand`物件並將結果傳回用戶端應用程式。

> [!NOTE]
> Managed 的資料庫物件是最適合用來預存程序和 Udf 會使用程序邏輯，而不是集合為基礎的邏輯。 程序邏輯牽涉到使用以逐列為基礎的資料集，或使用純量資料。 `GetDiscontinuedProducts`方法剛才所建立，不過，牽涉到任何程序的邏輯。 因此，它會在理想情況下會實作為 T-SQL 預存程序。 它會實為受管理的預存程序，來示範的步驟建立及部署所需的受管理的預存程序。


## <a name="step-4-deploying-the-managed-stored-procedure"></a>步驟 4： 部署 Managed 預存程序

使用這個完整的程式碼，我們已經準備好將它部署到 Northwind 資料庫項目。 部署 SQL Server 專案的程式碼編譯成組件、 使用資料庫時，註冊組件和建立對應的物件在資料庫中，將它們連結至組件中的適當方法。 明確的 [部署] 選項所執行的工作集合更精確地拼寫步驟 13。 以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案名稱，在 [方案總管] 中的，選擇 [部署] 選項。 不過，部署會失敗，發生下列錯誤: '外部' 附近的語法不正確。 您可能需要目前資料庫的相容性層級設定為較高的值，以啟用這項功能。 請參閱 < 預存程序說明`sp_dbcmptlevel`。

當您嘗試與 Northwind 資料庫中登錄組件時，就會發生這個錯誤訊息。 若要使用 SQL Server 2005 資料庫中註冊組件，資料庫 s 相容性層級必須設為 90。 根據預設，新的 SQL Server 2005 資料庫有相容性層級 90。 不過，使用 Microsoft SQL Server 2000 建立的資料庫都有預設相容性層級為 80。 因為 Northwind 資料庫一開始的 Microsoft SQL Server 2000 資料庫，其相容性層級使用目前設為 80，因此需要增加至 90，才能註冊 managed 的資料庫物件。

若要更新資料庫 s 相容性層級，在 Management Studio 中開啟新查詢視窗並輸入：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample4.sql)]

按一下 [執行] 圖示，以執行上述查詢中。


[![更新 Northwind 資料庫 s 相容性層級](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image15.png)

**圖 9**： 更新 Northwind 資料庫 s 相容性層級 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image17.png))


在更新之後的相容性層級，重新部署 SQL Server 專案。 此時不會發生錯誤應該完成的部署。

返回 SQL Server Management Studio，以滑鼠右鍵按一下 物件總管 中的 Northwind 資料庫並選擇 重新整理。 接下來，向下鑽研至 [可程式性] 資料夾，然後展開 [組件] 資料夾。 如圖 10 所示，Northwind 資料庫現在會包含所產生的組件`ManagedDatabaseConstructs`專案。


![ManagedDatabaseConstructs 組件會立即向 Northwind 資料庫](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image18.png)

**圖 10**:`ManagedDatabaseConstructs`組件會立即向 Northwind 資料庫


也依序展開 [預存程序] 資料夾。 您會看到名為預存程序的那里`GetDiscontinuedProducts`。 這個預存程序所建立的部署程序和指向`GetDiscontinuedProducts`方法中的`ManagedDatabaseConstructs`組件。 當`GetDiscontinuedProducts`執行預存程序時，它，接著會執行`GetDiscontinuedProducts`方法。 由於這是受管理的預存程序無法編輯透過 Management Studio (因此預存程序名稱旁邊的鎖定圖示)。


![GetDiscontinuedProducts 預存程序會列在預存程序資料夾](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image19.png)

**圖 11**:`GetDiscontinuedProducts`預存程序會列在預存程序資料夾


仍有一個更多的障礙，我們必須克服之前，我們就可以呼叫受管理的預存程序： 若要避免執行 managed 程式碼會將資料庫設定。 驗證，請開啟 新增查詢視窗中，執行`GetDiscontinuedProducts`預存程序。 您會收到下列錯誤訊息： 已停用的.NET Framework 中的使用者程式碼執行。 啟用 clr 已啟用組態選項。

若要檢查的 Northwind 資料庫的組態資訊、 輸入並執行命令`exec sp_configure`在查詢視窗中。 這會顯示 clr 已啟用設定目前設定為 0。


[![Clr 已啟用的設定為目前設定 0](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image20.png)

**圖 12**: clr 已啟用的設定為目前設定 0 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image22.png))


請注意，圖 12 中的每個組態設定的一起列出的四個值： 最小值和最大值的組態和執行的值。 若要更新 clr 已啟用設定的設定值，執行下列命令：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample5.sql)]

如果您重新執行`exec sp_configure`您會看到上述陳述式更新 clr 已啟用設定 s 的設定值，設為 1，但仍執行的值設定為 0。 我們要執行這項組態變更才會生效[`RECONFIGURE`命令](https://msdn.microsoft.com/en-us/library/ms176069.aspx)，它會將執行的值設定為目前的組態值。 只要輸入`RECONFIGURE`在查詢視窗中，按一下工具列中的 [執行] 圖示。 如果您執行`exec sp_configure`現在您應該看到值為 1，clr 已啟用設定 s 組態中，執行值。

Clr 已啟用設定完成後，我們已準備好執行 managed`GetDiscontinuedProducts`預存程序。 在查詢視窗中輸入並執行命令`exec` `GetDiscontinuedProducts`。 叫用預存程序會導致對應的 managed 程式碼中`GetDiscontinuedProducts`方法才能執行。 此程式碼發出`SELECT`查詢以傳回已停用，並傳回呼叫的應用程式，也就是 SQL Server Management Studio，這個執行個體中的這項資料的所有產品。 Management Studio 接收這些結果，並顯示在 [結果] 視窗。


[![預存程序會傳回所有 GetDiscontinuedProducts 停產的產品](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image23.png)

**圖 13**:`GetDiscontinuedProducts`預存程序傳回所有已停止的產品 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>步驟 5： 建立受管理的預存程序會接受輸入的參數

許多查詢和預存程序，我們建立了在整個這些教學課程使用*參數*。 例如，在[建立新預存程序型別資料集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程中我們建立了名為預存程序`GetProductsByCategoryID`可接受名為的輸入的參數`@CategoryID`。 預存程序則會傳回所有產品的`CategoryID`欄位符合所提供的值`@CategoryID`參數。

若要建立受管理的預存程序可接受輸入的參數，只需 s 方法定義中指定這些參數。 為了說明這點，讓 s 加入另一個 managed 預存程序`ManagedDatabaseConstructs`專案名為`GetProductsWithPriceLessThan`。 這個 managed 預存程序會接受輸入的參數指定價格，而且會傳回所有產品的`UnitPrice`欄位小於參數 s 的值。

若要將新的預存程序加入至專案，以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案名稱，然後選擇 加入新的預存程序。 將檔案命名為 `GetProductsWithPriceLessThan.vb`。 如我們所見在步驟 3 中，這將使用名為的方法來建立新的 Visual Basic 類別檔`GetProductsWithPriceLessThan`置於`Partial`類別`StoredProcedures`。

更新`GetProductsWithPriceLessThan`方法的定義，讓它接受[ `SqlMoney` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.aspx)名為輸入的參數`price`撰寫的程式碼來執行並傳回查詢結果：


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample6.vb)]

`GetProductsWithPriceLessThan`方法的定義和程式碼很類似的定義和程式碼`GetDiscontinuedProducts`步驟 3 中建立的方法。 唯一差異是`GetProductsWithPriceLessThan`方法會接受做為輸入參數 (`price`)、 `SqlCommand` s 查詢包含參數 (`@MaxPrice`)，並將參數加入`SqlCommand`s`Parameters`集合是和指派值給`price`變數。

加入此程式碼之後, 重新部署 SQL Server 專案。 接下來，返回 SQL Server Management Studio 並重新整理的預存程序資料夾。 您應該會看到新的項目， `GetProductsWithPriceLessThan`。 從查詢視窗中，輸入並執行命令`exec GetProductsWithPriceLessThan 25`，此圖 14 顯示所有產品小於 $25，都將清單。


[![顯示產品下 $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image26.png)

**圖 14**： 會顯示產品下 $25 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>步驟 6： 從資料存取層中呼叫 Managed 預存程序

此時我們新增了`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`managed 預存程序`ManagedDatabaseConstructs`專案，並向他們 Northwind SQL Server 資料庫。 我們也叫用這些 managed 預存程序從 SQL Server Management Studio （請參閱圖 13 與 14）。 為了讓我們的 ASP.NET 應用程式使用這些 managed 預存程序，不過，我們需要將它們新增到資料存取和商務邏輯層架構中。 在此步驟中，我們將會新增兩個新的方法來`ProductsTableAdapter`中`NorthwindWithSprocs`具類型資料集，其中一開始是在[建立新預存程序型別資料集 s Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。 在步驟 7 中，我們將會對應方法加入 BLL 中。

開啟`NorthwindWithSprocs`型別資料集，在 Visual Studio 和加入新的方法來開始`ProductsTableAdapter`名為`GetDiscontinuedProducts`。 若要將新方法加入至 TableAdapter，以滑鼠右鍵按一下設計工具中的 TableAdapter 的名稱並從內容功能表中選擇 加入查詢選項。

> [!NOTE]
> 因為我們將從 Northwind 資料庫的移`App_Data`資料夾中的 SQL Server 2005 Express Edition 資料庫執行個體，請務必在 Web.config 中對應的連接字串會更新來反映這項變更。 我們將討論在步驟 2 中更新`NORTHWNDConnectionString`值`Web.config`。 如果您忘記執行這項更新，您會看到錯誤訊息無法加入查詢。 找不到連接`NORTHWNDConnectionString`物件`Web.config`中時嘗試將新方法加入至 TableAdapter 的對話方塊。 若要解決這個錯誤，請按一下 [確定]，然後前往`Web.config`並更新`NORTHWNDConnectionString`值在步驟 2 中所述。 然後再次嘗試重新將方法加入至 TableAdapter。 這次它應該會運作不會發生錯誤。


加入新的方法會啟動 TableAdapter 查詢組態精靈中，我們在過去的教學課程中多次使用。 第一個步驟會詢問我們指定 TableAdapter 存取資料庫的方式： 透過臨機操作 SQL 陳述式，或透過新的或現有的預存程序。 因為我們已經建立並註冊`GetDiscontinuedProducts`managed 預存程序，與資料庫中，選擇使用現有預存程序選項，然後按 [下一步]。


[![選擇 使用現有的預存程序選項](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image29.png)

**圖 15**： 選擇使用現有的預存程序選項 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image31.png))


下一個畫面會提示我們方法會叫用預存程序。 選擇`GetDiscontinuedProducts`managed 預存程序，從下拉式清單，然後按 [下一步]。


[![選取 GetDiscontinuedProducts Managed 預存程序](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image32.png)

**圖 16**： 選取`GetDiscontinuedProducts`Managed 預存程序 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image34.png))


系統會要求我們指定預存程序傳回資料列、 單一值，或執行任何動作。 因為`GetDiscontinuedProducts`傳回集停產的產品資料列中，選擇第一個選項 （表格式資料），按一下 [下一步]。


[![選取 表格式資料選項](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image35.png)

**圖 17**： 選取 表格式資料選項 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image37.png))


最後一個精靈畫面可讓我們指定使用的資料存取模式和結果方法的名稱。 保留選取的核取方塊和名稱方法`FillByDiscontinued`和`GetDiscontinuedProducts`。 按一下 [完成] 5d; 以完成精靈。


[![名稱方法 FillByDiscontinued 和 GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image38.png)

**圖 18**: Name 方法`FillByDiscontinued`和`GetDiscontinuedProducts`([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image40.png))


重複上述步驟，建立方法，名為`FillByPriceLessThan`和`GetProductsWithPriceLessThan`中`ProductsTableAdapter`如`GetProductsWithPriceLessThan`managed 預存程序。

圖 19 顯示 DataSet 設計工具的螢幕擷取畫面之後將方法加入至`ProductsTableAdapter`如`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`managed 預存程序。


[![ProductsTableAdapter 包含在此步驟中加入的新方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image41.png)

**圖 19**:`ProductsTableAdapter`包含在此步驟中的新方法加入 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>步驟 7： 加入商務邏輯層中的對應方法

既然我們已更新資料的存取層，以包含受管理的預存程序，以加入在步驟 4 和 5 函式呼叫的方法，我們需要對應的方法加入商務邏輯層。 加入下列兩種方法可以`ProductsBLLWithSprocs`類別：


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample7.vb)]

這兩種方法只要呼叫對應的 DAL 方法，並傳回`ProductsDataTable`執行個體。 `DataObjectMethodAttribute`標記上述每個方法會導致 ObjectDataSource 的設定資料來源精靈的 [選取] 索引標籤中的下拉式清單中包含這些方法。

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>步驟 8： 叫用 Managed 預存程序從展示層

使用商務邏輯和資料存取層級予以增加以包含用於呼叫支援`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`managed 預存程序，我們現在可以顯示這些預存程序透過 ASP.NET 網頁的結果。

開啟`ManagedFunctionsAndSprocs.aspx`頁面`AdvancedDAL`資料夾，並從 [工具箱] 拖曳至設計工具拖曳至 GridView。 設定 GridView s`ID`屬性`DiscontinuedProducts`和其智慧標籤上，從繫結到名為新 ObjectDataSource `DiscontinuedProductsDataSource`。 設定提取資料的來源 ObjectDataSource`ProductsBLLWithSprocs`類別的`GetDiscontinuedProducts`方法。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image44.png)

**圖 20**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image46.png))


[![從下拉式清單中選取索引標籤中選擇 GetDiscontinuedProducts 方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image47.png)

**圖 21**： 選擇`GetDiscontinuedProducts`方法，從下拉式清單中選取索引標籤 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image49.png))


由於此方格會用來只顯示產品資訊中，設定下拉式清單中更新、 插入和刪除索引標籤，以 （無），以及然後按一下 [完成]。

正在完成精靈，在 Visual Studio 會自動加入 BoundField 或 CheckBoxField 中每個資料欄位`ProductsDataTable`。 請花一點時間移除所有的這些欄位，除了`ProductName`和`Discontinued`速率 並按一下您的 GridView，ObjectDataSource s 宣告式標記看起來應該類似下列：


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample8.aspx)]

請花一點時間來檢視此頁面，透過瀏覽器。 當瀏覽頁面時，ObjectDataSource 呼叫`ProductsBLLWithSprocs`類別的`GetDiscontinuedProducts`方法。 如我們所見步驟 7 中，這個方法會呼叫向下到 DAL s`ProductsDataTable`類別 s`GetDiscontinuedProducts`方法，會叫用`GetDiscontinuedProducts`預存程序。 Managed 預存程序，而我們在步驟 3 中，傳回不再生產的產品建立的程式碼會執行這個預存程序。

Managed 預存程序所傳回的結果封裝成`ProductsDataTable`dal 然後傳回給 BLL，然後將其還原為展示層，它們會在此繫結至 GridView，而且會顯示。 如預期般，方格會列出已停用這些產品。


[![列出已停止的產品](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image50.png)

**圖 22**： 列出已停止的產品 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image52.png))


進一步的作法，將文字方塊和其他的 GridView 加入頁面。 有顯示產品低於金額藉由呼叫在 textbox 中輸入此 GridView`ProductsBLLWithSprocs`類別的`GetProductsWithPriceLessThan`方法。

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>步驟 9： 建立和呼叫 T-SQL Udf

使用者定義函數或 Udf，為資料庫物件精確地模擬的程式設計語言中的函式的語意。 在 Visual Basic 中的函式，例如 Udf 可以包含多個輸入參數和傳回特定類型的值。 UDF 可以傳回純量資料的字串、 整數、 和等位或表格式資料。 可讓 s 快速看看這兩種 Udf，開頭 UDF，以傳回純量資料類型。

下列的 UDF 計算特定產品的清查的估計的值。 它是由三個輸入參數層中考慮`UnitPrice`， `UnitsInStock`，和`Discontinued`傳回值的類型和值為特定的產品- `money`。 計算存貨預估的值乘以`UnitPrice`由`UnitsInStock`。 已停止的項目，這個值會變成一半。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample9.sql)]

一旦此 UDF 已加入至資料庫，可以是找到透過 Management Studio，展開可程式性 資料夾中，則函式，然後純量值函式。 它可以用於`SELECT`查詢就像這樣：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample10.sql)]

我加入`udf_ComputeInventoryValue`UDF Northwind 資料庫。圖 23 示範上述輸出`SELECT`時透過 Management Studio 檢視查詢。 也請注意 UDF 會列在 物件總管 中的純量值函式 資料夾底下。


[![列出每個產品的庫存值](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image53.png)

**圖 23**： 庫存值列出的每個產品 s ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image55.png))


Udf 也可以傳回表格式資料。 例如，我們可以建立 UDF，以傳回屬於特定類別的產品：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample11.sql)]

`udf_GetProductsByCategoryID` UDF 接受`@CategoryID`輸入的參數，並傳回指定的結果`SELECT`查詢。 在建立之後，可以參考此 UDF `FROM` (或`JOIN`) 子句`SELECT`查詢。 下列範例會傳回`ProductID`， `ProductName`，和`CategoryID`飲料的每個值。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample12.sql)]

我加入`udf_GetProductsByCategoryID`UDF Northwind 資料庫。圖 24 示範上述輸出`SELECT`時透過 Management Studio 檢視查詢。 傳回表格式資料的 Udf 可以找到物件總管 中的 s 資料表值函式資料夾中。


[![每個飲料列出 ProductID、 ProductName 和 CategoryID](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image56.png)

**圖 24**: `ProductID`， `ProductName`，和`CategoryID`列出每個飲料 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image58.png))


> [!NOTE]
> 如需有關建立和使用 Udf 的詳細資訊，請參閱[使用者定義函數的簡介](http://www.sqlteam.com/item.asp?ItemID=1955)。 也請參閱[優點和 Drawbacks of User-Defined 函式](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)。


## <a name="step-10-creating-a-managed-udf"></a>步驟 10： 建立受管理的 UDF

`udf_ComputeInventoryValue`和`udf_GetProductsByCategoryID`在上述範例中建立的 Udf 是 T-SQL 的資料庫物件。 SQL Server 2005 也支援受管理的 Udf 會加入到`ManagedDatabaseConstructs`專案就像 managed 預存程序，從步驟 3 和 5。 此步驟中，可讓實作 s `udf_ComputeInventoryValue` managed 程式碼中的 UDF。

若要加入至 managed 的 UDF`ManagedDatabaseConstructs`專案中，以滑鼠右鍵按一下方案總管] 中的專案名稱並選擇 [加入新項目。 從 [加入新項目] 對話方塊中選取的使用者定義範本並將新的 UDF 檔案`udf_ComputeInventoryValue_Managed.vb`。


[![ManagedDatabaseConstructs 專案中加入新的 Managed 的 UDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image59.png)

**圖 25**： 新增到新管理 UDF`ManagedDatabaseConstructs`專案 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image61.png))


使用者定義函式樣板建立`Partial`名為類別`UserDefinedFunctions`與方法，其名稱為類別 s 的檔案名稱相同 (`udf_ComputeInventoryValue_Managed`，這個執行個體中)。 這個方法使用裝飾[`SqlFunction`屬性](https://msdn.microsoft.com/en-us/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)的旗標的為受管理的 UDF 的方法。


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample13.vb)]

`udf_ComputeInventoryValue`目前方法會傳回[`SqlString`物件](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlstring.aspx)而且不接受任何輸入參數。 我們需要更新方法定義，以便讓它接受三個輸入參數- `UnitPrice`， `UnitsInStock`，和`Discontinued`-並傳回`SqlMoney`物件。 計算存貨值的邏輯中都有相同的 T-SQL `udf_ComputeInventoryValue` UDF。


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample14.vb)]

請注意，UDF 方法 s 輸入的參數是其對應的 SQL 型別的：`SqlMoney`如`UnitPrice` 欄位中， [ `SqlInt16` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlint16.aspx)如`UnitsInStock`，和[ `SqlBoolean` ](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlboolean.aspx)如`Discontinued`。 這些資料類型會反映在定義的型別`Products`資料表：`UnitPrice`資料行是類型`money`、`UnitsInStock`類型的資料行`smallint`，而`Discontinued`類型的資料行`bit`。

程式碼會啟動建立`SqlMoney`名為執行個體`inventoryValue`，指派值為 0。 `Products`資料表可讓資料庫`NULL`值`UnitsInPrice`和`UnitsInStock`資料行。 因此，我們需要先檢查這些值是否包含，請參閱`NULL`s，我們不要透過`SqlMoney`物件 s [ `IsNull`屬性](https://msdn.microsoft.com/en-us/library/system.data.sqltypes.sqlmoney.isnull.aspx)。 如果兩個`UnitPrice`和`UnitsInStock`包含非`NULL`值，那麼我們計算`inventoryValue`是兩個產品。 如果`Discontinued`為 true，則我們一半的值。

> [!NOTE]
> `SqlMoney`物件只允許兩個`SqlMoney`一起相乘的執行個體。 不允許`SqlMoney`先乘以常值的浮點數的執行個體。 因此，以一半`inventoryValue`我們將它乘以新`SqlMoney`值 0.5 的執行個體。


## <a name="step-11-deploying-the-managed-udf"></a>步驟 11： 部署受管理的 UDF

現在，已建立受管理的 UDF，我們已準備好將它部署到 Northwind 資料庫。 如我們所見步驟 4 中，會部署在 SQL Server 專案中的受管理的物件 [方案總管] 中的專案名稱上按一下滑鼠右鍵，然後從內容功能表選擇 [部署] 選項。

一旦部署專案之後，返回 SQL Server Management Studio，並重新整理的純量值函式的資料夾。 您現在應該會看到兩個項目：

- `dbo.udf_ComputeInventoryValue`-在步驟 9 中，建立 T-SQL UDF 和
- `dbo.udf ComputeInventoryValue_Managed`-已剛剛所部署的步驟 10 中建立受管理的 UDF。

若要測試此受管理的 UDF，執行下列查詢從 Management Studio 中：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample15.sql)]

此命令會使用 managed `udf ComputeInventoryValue_Managed` UDF，而不是 T-SQL `udf_ComputeInventoryValue` UDF，但是輸出都相同。 若要查看 UDF 的輸出的螢幕擷取畫面圖 23 回頭參考。

## <a name="step-12-debugging-the-managed-database-objects"></a>步驟 12： 偵錯 Managed 的資料庫物件

在[偵錯預存程序](debugging-stored-procedures-vb.md)的偵錯 SQL Server 透過 Visual Studio 的教學課程中我們將討論三個選項： 直接資料庫偵錯、 應用程式偵錯，以及從 SQL Server 專案的偵錯。 從用戶端應用程式，以及直接從 SQL Server 專案，則 managed 的資料庫物件無法透過直接資料庫偵錯，偵錯，但可偵錯。 為了讓偵錯能夠運作，不過，SQL Server 2005 資料庫必須允許 SQL/CLR 偵錯。 最初建立時，請記得， `ManagedDatabaseConstructs` Visual Studio 要求我們是否要啟用 SQL/CLR 偵錯 （請參閱圖 6 中步驟 2） 的專案。 以滑鼠右鍵按一下 [伺服器總管] 視窗的資料庫，就可以修改此設定。


![請確定資料庫允許 SQL/CLR 偵錯](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image62.png)

**圖 26**： 確保資料庫允許 SQL/CLR 偵錯


假設我們想要偵錯`GetProductsWithPriceLessThan`managed 預存程序。 我們會開始，以設定中斷點的程式碼內`GetProductsWithPriceLessThan`方法。


[![GetProductsWithPriceLessThan 方法中設定中斷點](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image63.png)

**圖 27**： 中設定中斷點`GetProductsWithPriceLessThan`方法 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image65.png))


可讓 s 先來看偵錯 managed 的資料庫物件從 SQL Server 專案。 因為我們的方案包含兩個專案- `ManagedDatabaseConstructs` SQL Server 專案，以及我們的網站-若要從 SQL Server 專案，我們要指示來啟動 Visual Studio 偵錯`ManagedDatabaseConstructs`我們開始偵錯時，SQL Server 專案。 以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案方案總管] 中，選擇 [設定為啟始專案選項，從內容功能表。

當`ManagedDatabaseConstructs`專案啟動偵錯工具在執行中的 SQL 陳述式從`Test.sql`檔案，位於`Test Scripts`資料夾。 例如，若要測試`GetProductsWithPriceLessThan`managed 預存程序，取代現有`Test.sql`檔案使用下列陳述式，會叫用的內容`GetProductsWithPriceLessThan`managed 預存程序中傳遞`@CategoryID`14.95 的值：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample16.sql)]

一旦您已輸入到上述指令碼`Test.sql`、 開始偵錯移至 偵錯 功能表，然後選擇 開始偵錯，或按 F5 或綠色播放工具列中的圖示。 這會建置方案中的專案中，Northwind 資料庫中，部署 managed 的資料庫物件，然後執行`Test.sql`指令碼。 此時，會叫用中斷點，我們可以逐步執行`GetProductsWithPriceLessThan`方法，檢查輸入參數的值等等。


[![叫用中斷點 GetProductsWithPriceLessThan 方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image66.png)

**圖 28**: 中斷點`GetProductsWithPriceLessThan`方法叫用 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image68.png))


為了要透過用戶端應用程式進行偵錯 SQL 資料庫物件，請務必為設定資料庫，以支援應用程式偵錯。 以滑鼠右鍵按一下伺服器總管 中的資料庫，並確定已核取的應用程式偵錯選項。 此外，我們需要設定 ASP.NET 應用程式與 SQL 偵錯工具整合，以及停用連接共用。 這些步驟已在步驟 2 中詳細討論[偵錯預存程序](debugging-stored-procedures-vb.md)教學課程。

一旦您已設定的 ASP.NET 應用程式和資料庫，將 ASP.NET 網站設定為啟始專案，並開始偵錯。 如果您瀏覽的頁面，會呼叫其中一個具有中斷點的受管理物件，將會暫停，應用程式和控制項將會開啟一段偵錯工具，其中您可以逐步執行程式碼中圖 28 所示。

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>步驟 13： 手動編譯和部署 Managed 資料庫物件

SQL Server 專案，讓您輕鬆建立、 編譯及部署 managed 的資料庫物件。 不幸的是，SQL Server 專案中才有 Visual Studio Professional 和小組的系統版本。 如果您使用 Visual Web Developer 或 Standard 版本的 Visual Studio，並想要使用 managed 的資料庫物件，您必須手動建立，並將其部署。 這包含四個步驟：

1. 建立檔案，其中包含 managed 的資料庫物件的原始程式碼
2. 編譯為組件中物件
3. 使用 SQL Server 2005 資料庫時，註冊此組件和
4. 指向組件中的適當方法的 SQL Server 中建立資料庫物件。

說明這些工作，讓建立新的受管理的預存程序傳回的產品，其`UnitPrice`大於指定的值。 建立名為您電腦上的新檔案`GetProductsWithPriceGreaterThan.vb`和輸入檔案中的下列程式碼 （您可以使用 Visual Studio 中，[記事本] 或任何文字編輯器來完成這項作業）：


[!code-vb[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample17.vb)]

此程式碼是幾乎完全相同的`GetProductsWithPriceLessThan`步驟 5 中建立的方法。 唯一差異是方法名稱，`WHERE`子句，而且查詢中使用的參數名稱。 回到`GetProductsWithPriceLessThan`方法，`WHERE`子句讀取： `WHERE UnitPrice < @MaxPrice`。 這裡的`GetProductsWithPriceGreaterThan`，我們使用： `WHERE UnitPrice > @MinPrice` 。

我們現在需要此類別編譯為組件。 從命令列中，瀏覽至您的儲存所在的目錄`GetProductsWithPriceGreaterThan.vb`檔，並以 C# 編譯器 (`csc.exe`) 來編譯為組件的類別檔案：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample18.cmd)]

如果包含 v 資料夾`bc.exe`中不在系統 s `PATH`，您必須完整參考路徑， `%WINDOWS%\Microsoft.NET\Framework\version\`，就像這樣：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.vb 編譯為組件](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image69.png)

**圖 29**： 編譯`GetProductsWithPriceGreaterThan.vb`到組件 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image71.png))


`/t`旗標會指定 Visual Basic 類別檔應該進行編譯成 DLL （而不是可執行檔）。 `/out`旗標會指定產生的組件的名稱。

> [!NOTE]
> 而不是編譯`GetProductsWithPriceGreaterThan.vb`類別檔案，從命令列，或者，您無法使用[Visual Basic Express 版](https://msdn.microsoft.com/vstudio/express/vb/)或在 Visual Studio Standard 版中建立個別的類別庫專案。 S ren 因此 Lauritsen 請提供程式碼具有這類的 Visual Basic Express 版專案`GetProductsWithPriceGreaterThan`管理預存程序的預存程序和兩個，並在步驟 3、 5 和 10 中建立的 UDF。 S ren s 專案也包含要加入對應的資料庫物件所需的 T-SQL 命令。


程式碼編譯的組件時，我們已經準備好註冊 SQL Server 2005 資料庫內組件項目。 這可透過 T-SQL，使用命令來執行`CREATE ASSEMBLY`，或透過 SQL Server Management Studio。 使用 Management Studio 讓 s 焦點。

從 Management Studio 中，展開 Northwind 資料庫中的 可程式性 資料夾。 組件及其子資料夾的其中一個。 若要手動將新的組件加入至資料庫，以滑鼠右鍵按一下組件 資料夾並從內容功能表中選擇新的組件。 這個新組件對話方塊 （請參閱圖 30） 會顯示。 按一下 瀏覽按鈕，選取`ManuallyCreatedDBObjects.dll`我們只編譯，，然後按一下 確定 以加入資料庫中的組件的組件。 您不應該看到`ManuallyCreatedDBObjects.dll`物件總管 中的組件。


[![ManuallyCreatedDBObjects.dll 組件加入至資料庫](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image72.png)

**圖 30**： 新增`ManuallyCreatedDBObjects.dll`到資料庫的組件 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image74.png))


![在 [物件總管] 中列為 ManuallyCreatedDBObjects.dll](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image75.png)

**圖 31**:`ManuallyCreatedDBObjects.dll`會列在 [物件總管]


雖然我們已加入組件到 Northwind 資料庫，我們還沒有關聯的預存程序`GetProductsWithPriceGreaterThan`組件中的方法。 若要完成這項作業，開啟新查詢視窗並執行下列指令碼：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample20.sql)]

這名為 Northwind 資料庫中建立新的預存程序`GetProductsWithPriceGreaterThan`並受管理的方法與關聯`GetProductsWithPriceGreaterThan`(這是在類別中`StoredProcedures`，這是組件中`ManuallyCreatedDBObjects`)。

在執行上述指令碼之後, 重新整理物件總管 中的 預存程序 資料夾。 您應該會看到新的預存程序項目- `GetProductsWithPriceGreaterThan` -具有鎖定圖示旁邊。 若要測試此預存程序，請輸入，並在查詢視窗中執行下列指令碼：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/samples/sample21.sql)]

如圖 32 所示，上述命令會顯示使用這些產品的資訊`UnitPrice`大於 $24.95。


[![在 [物件總管] 中列為 ManuallyCreatedDBObjects.dll](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image76.png)

**圖 32**:`ManuallyCreatedDBObjects.dll`會列在 [物件總管] ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb/_static/image78.png))


## <a name="summary"></a>總結

Microsoft SQL Server 2005 提供整合與 Common Language Runtime (CLR)，可讓 managed 程式碼建立的資料庫物件。 先前，這些資料庫物件無法只會建立使用 T-SQL，但現在我們可以建立使用.NET 程式設計語言，例如 Visual Basic 這些物件。 在此教學課程中我們建立兩個預存程序以及管理受管理的使用者定義函式。

建立、 編譯和部署 managed 的資料庫物件，可協助 visual Studio 的 SQL Server 專案類型。 此外，它會提供豐富的偵錯支援。 不過，SQL Server 專案類型才可用在 Visual Studio Professional 和小組的系統版本。 對於使用 Visual Web Developer 或 Standard 版本的 Visual Studio、 建立、 編譯和部署步驟必須手動執行，就如上文步驟 13。

祝您程式設計 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [使用者定義函數的優缺點](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [在 Managed 程式碼中建立 SQL Server 2005 物件](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [建立觸發程序使用 SQL Server 2005 中的 Managed 程式碼](http://www.15seconds.com/issue/041006.htm)
- [如何： 建立和執行 CLR SQL Server 預存程序](https://msdn.microsoft.com/en-us/library/5czye81z(VS.80).aspx)
- [如何： 建立和執行 CLR SQL Server 使用者定義函式](https://msdn.microsoft.com/en-us/library/w2kae45k(VS.80).aspx)
- [如何： 編輯`Test.sql`執行 SQL 物件的指令碼](https://msdn.microsoft.com/en-us/library/ms233682(VS.80).aspx)
- [簡介使用者定義函數](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Managed 程式碼和 SQL Server 2005 （影片）](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [TRANSACT-SQL 參考](https://msdn.microsoft.com/en-us/library/aa299742(SQL.80).aspx)
- [逐步解說： 在 Managed 程式碼中建立預存程序](https://msdn.microsoft.com/en-us/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

許多有用的檢閱者已檢閱本教學課程系列。 在此教學課程的前導檢閱者已 S ren 因此 Lauritsen。 除了檢閱這個發行項，S ren 也建立了此手動編譯 managed 的資料庫物件的發行項的下載中包含 Visual C# Express 版專案。 檢閱我即將推出的 MSDN 文件有興趣嗎？ 如果是這樣，卸除我一行[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

>[!div class="step-by-step"]
[上一步](debugging-stored-procedures-vb.md)
