---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
title: 建立預存程序和使用者定義函數的 Managed 程式碼 (C#) |Microsoft Docs
author: rick-anderson
description: Microsoft SQL Server 2005 整合了.NET Common Language Runtime 可讓開發人員建立透過 managed 程式碼的資料庫物件。 本教學課程...
ms.author: riande
ms.date: 08/03/2007
ms.assetid: 213eea41-1ab4-4371-8b24-1a1a66c515de
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/creating-stored-procedures-and-user-defined-functions-with-managed-code-cs
msc.type: authoredcontent
ms.openlocfilehash: fb4a867d5868e8000fcd10130401a9e169b6f49f
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833867"
---
<a name="creating-stored-procedures-and-user-defined-functions-with-managed-code-c"></a>建立預存程序和使用者定義函式，以 Managed 程式碼 (C#)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_75_CS.zip)或[下載 PDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/datatutorial75cs1.pdf)

> Microsoft SQL Server 2005 整合了.NET Common Language Runtime 可讓開發人員建立透過 managed 程式碼的資料庫物件。 本教學課程會示範如何建立 managed 預存程序，並管理您的 Visual Basic 或 C# 程式碼的使用者定義函數。 我們也會看到這些版本的 Visual Studio 如何讓您偵錯這類的 managed 的資料庫物件。


## <a name="introduction"></a>簡介

使用如 Microsoft 的 SQL Server 2005 的資料庫[Transact-Structured 查詢語言 (T-SQL)](http://en.wikipedia.org/wiki/Transact-SQL)插入、 修改和擷取資料。 大部分的資料庫系統包含群組的 SQL 陳述式，則可以執行為單一、 可重複使用單位的一系列的建構。 預存程序是一個範例。 另一個是*使用者定義函式*(Udf)，我們將在步驟 9 中的更詳細地檢查的建構。

基本上，SQL 被設計用於處理的資料集。 `SELECT`， `UPDATE`，並`DELETE`陳述式本質上套用至對應資料表中的所有記錄，並只受限於其`WHERE`子句。 尚未有許多設計使用一次一筆記錄，並處理純量資料的語言功能。 [`CURSOR` s](http://www.sqlteam.com/item.asp?ItemID=553)提供一組記錄，透過一次執行迴圈。 字串操作函式，例如`LEFT`， `CHARINDEX`，和`PATINDEX`搭配純量資料。 SQL 也會包含控制流程陳述式，例如`IF`和`WHILE`。

Microsoft SQL Server 2005 之前, 預存程序和 Udf 可以只定義為 T-SQL 陳述式的集合。 SQL Server 2005，不過，用意是要提供與整合[Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)，這是所有的.NET 組件所使用的執行階段。 因此，預存程序和 Udf，SQL Server 2005 資料庫中的可以建立使用 managed 程式碼。 也就是說，您可以建立預存程序或 UDF 為 C# 類別的方法。 這可讓這些預存程序和 Udf，以利用.NET Framework 中，並從您自己的自訂類別的功能。

在此教學課程中，我們會檢驗如何建立 managed 預存程序和使用者定義函式，以及如何將它們整合到我們的 Northwind 資料庫。 讓 s 開始 ！

> [!NOTE]
> Managed 的資料庫物件會提供一些優於其 SQL 對應項目。 語言豐富功能和熟悉度及重複使用現有的程式碼和邏輯的能力是主要的優點。 但 managed 的資料庫物件有可能使用不涉及多程序邏輯的資料集時為比較沒有效率。 如需優點使用 T-SQL 與 managed 程式碼的更完整討論，請參閱[優點的使用 Managed 程式碼建立資料庫物件](https://msdn.microsoft.com/library/k2e1fb36(VS.80).aspx)。


## <a name="step-1-moving-the-northwind-database-out-ofappdata"></a>步驟 1︰ 移出的 Northwind 資料庫`App_Data`

我們的教學課程的所有目前為止已使用 Microsoft SQL Server 2005 Express 的 Edition 資料庫檔案中之 web 應用程式`App_Data`資料夾。 將資料庫中的放置`App_Data`簡化發佈和執行這些教學課程，以及所有的檔案是位於一個目錄所不需的任何其他組態步驟，以測試本教學課程。

本教學課程中，不過，讓的 Northwind 將資料庫移出`App_Data`並明確地向 SQL Server 2005 Express Edition 資料庫執行個體。 雖然我們可以執行本教學課程中使用的資料庫中的步驟`App_Data`資料夾中，數個步驟會更簡單藉由明確地向 SQL Server 2005 Express Edition 資料庫執行個體中的資料庫。

本教學課程中下載有兩個資料庫檔案-`NORTHWND.MDF`並`NORTHWND_log.LDF`放在名為的資料夾中- `DataFiles`。 如果您密切注意您自己的實作，教學課程，請關閉 Visual Studio，並移`NORTHWND.MDF`並`NORTHWND_log.LDF`檔案，從網站的`App_Data`至外部網站資料夾的資料夾。 一旦將資料庫檔案已移到另一個資料夾我們必須向 Northwind 資料庫的 SQL Server 2005 Express Edition 資料庫執行個體。 這可從 SQL Server Management Studio。 如果您有非-Express Edition 的 SQL Server 2005 安裝在您的電腦上則您可能已經有安裝 Management Studio。 如果您只在您的電腦上有 SQL Server 2005 Express Edition，請花一點時間下載並安裝[Microsoft SQL Server Management Studio Express](https://www.microsoft.com/downloads/details.aspx?displaylang=en&amp;FamilyID=C243A5AE-4BD1-4E3D-94B8-5A0F62BF7796)。

啟動 SQL Server Management Studio。 如 [圖 1] 所示，Management Studio 一開始會詢問您要連接到哪一部伺服器。 Localhost\SQLExpress 輸入伺服器名稱，在驗證 下拉式清單中，選擇 Windows 驗證，按一下 連接。


![連接到適當的資料庫執行個體](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image1.png)

**圖 1**： 連接到適當的資料庫執行個體


一旦您已連線，[物件總管] 視窗會列出 SQL Server 2005 Express Edition 資料庫執行個體，包括資料庫、 安全性資訊、 管理選項等相關資訊。

我們要附加 Northwind 資料庫中的`DataFiles`資料夾 （或只要您可能會將其移動） 到 SQL Server 2005 Express Edition 資料庫執行個體。 以滑鼠右鍵按一下 [資料庫] 資料夾，並從內容功能表中選擇 [附加] 選項。 這會顯示 [附加資料庫] 對話方塊。 按一下 [新增] 按鈕，向下鑽研至適當`NORTHWND.MDF`檔案，然後按一下 [確定]。 此時您的畫面應該看起來類似 圖 2。


[![連接到適當的資料庫執行個體](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image3.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image2.png)

**圖 2**： 連接到適當的資料庫執行個體 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image4.png))


> [!NOTE]
> 當連接到 SQL Server 2005 Express Edition 執行個體，透過 Management Studio 附加資料庫 對話方塊中不允許您向下切入至 使用者設定檔的目錄，例如 我的文件。 因此，請確定放`NORTHWND.MDF`和`NORTHWND_log.LDF`非使用者設定檔目錄中的檔案。


按一下 [確定] 按鈕，以附加資料庫。 [附加資料庫] 對話方塊會關閉，[物件總管] 現在應該會列出剛才附加的資料庫。 您想要的資料庫有名稱，例如 Northwind `9FE54661B32FDD967F51D71D0D5145CC_LINE ARTICLES\DATATUTORIALS\VOLUME 3\CSHARP\73\ASPNET_DATA_TUTORIAL_75_CS\APP_DATA\NORTHWND.MDF`。 重新命名資料庫以 Northwind 資料庫上按一下滑鼠右鍵，然後選擇 重新命名。


![重新命名資料庫以 Northwind](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image5.png)

**圖 3**： 重新命名資料庫以 Northwind


## <a name="step-2-creating-a-new-solution-and-sql-server-project-in-visual-studio"></a>步驟 2： 在 Visual Studio 中建立新的方案和 SQL Server 專案

若要建立 SQL Server 2005 中的 受管理的預存程序或 Udf 我們會為類別中的 C# 程式碼撰寫的預存程序和 UDF 邏輯。 一旦已撰寫的程式碼，我們需要此類別編譯為組件 (`.dll`檔案) 使用 SQL Server 資料庫時，註冊組件，然後建立指向的相對應的方法，在資料庫中的 預存程序或 UDF 物件組件。 這些步驟可以所有手動執行。 我們可以在任何文字編輯器建立的程式碼，從命令列使用 C# 編譯器編譯它 ([`csc.exe`](https://msdn.microsoft.com/library/ms379563(vs.80).aspx))，向使用[ `CREATE ASSEMBLY` ](https://msdn.microsoft.com/library/ms189524.aspx)命令，或從管理Studio 中，並加入預存程序或 UDF 物件，可透過類似的方法。 幸運的是，Visual Studio Professional 和 Team 系統版本包含 SQL Server 專案類型，用來自動化這些工作。 在本教學課程中我們將逐步引導使用 SQL Server 專案類型來建立 managed 預存程序和 UDF。

> [!NOTE]
> 如果您使用 Visual Web Developer 或 Standard edition 的 Visual Studio，則您必須改為使用手動方式。 步驟 13 中提供了以手動方式執行這些步驟的詳細的指示。 建議您閱讀 12 中的步驟 2，再閱讀步驟 13，因為這些步驟包括重要必須套用不論您使用哪個版本的 Visual Studio 的 SQL Server 組態指示。


首先開啟 Visual Studio。 從 檔案 功能表中，選擇 新增專案 以顯示 新增專案 對話方塊 （請參閱 圖 4）。 向下鑽研至資料庫專案類型，並從列於右側的範本，然後選擇 建立新的 SQL Server 專案。 我選擇此專案的名稱，`ManagedDatabaseConstructs`並放置在名為方案內`Tutorial75`。


[![建立新的 SQL Server 專案](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image7.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image6.png)

**圖 4**： 建立新的 SQL Server 專案 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image8.png))


按一下 [新增專案] 對話方塊中，建立方案和 SQL Server 專案中的 [確定] 按鈕。

SQL Server 專案繫結至特定的資料庫。 因此，建立新的 SQL Server 專案之後我們會立即要求您指定這項資訊。 圖 5 顯示具有指向我們註冊 SQL Server 2005 Express Edition 資料庫執行個體，在 步驟 1 中的 Northwind 資料庫填妥 新增資料庫參考 對話方塊。


![與 Northwind 資料庫產生關聯的 SQL Server 專案](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image9.png)

**圖 5**： 關聯 Northwind 資料庫的 SQL Server 專案


若要偵錯 managed 預存程序和 Udf，我們將建立在此專案中，我們要啟用 SQL/CLR 偵錯連接的支援。 每當 SQL Server 專案與新的資料庫產生關聯，（如同我們在 圖 5），Visual Studio 會詢問您是否我們想要啟用在連接上的 SQL/CLR 偵錯 （請參閱 圖 6）。 按一下 [是]。


![啟用 SQL/CLR 偵錯](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image10.png)

**圖 6**： 啟用 SQL/CLR 偵錯


此時新的 SQL Server 專案有已加入至方案。 它包含名為的資料夾`Test Scripts`檔案，名為`Test.sql`，後者用來偵錯 managed 的資料庫物件，在專案中建立。 我們將探討在步驟 12 中偵錯。

我們現在可以將新的 managed 預存程序和 Udf，加入此專案中，但我們還是會讓之前先包含我們現有的 web 應用程式方案中。 從 [檔案] 功能表選取 [新增] 選項，並選擇現有的網站。 瀏覽至適當的網站資料夾，然後按一下 [確定]。 如 [圖 7] 所示，這將會更新以包含兩個專案的解決方案： 網站和`ManagedDatabaseConstructs`SQL Server 專案。


![[方案總管] 現在包含兩個專案](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image11.png)

**圖 7**: 方案總管 現在包含兩個專案


`NORTHWNDConnectionString`中的值`Web.config`目前參考`NORTHWND.MDF`檔案中`App_Data`資料夾。 由於我們移除了此資料庫，從`App_Data`明確註冊它，並在 SQL Server 2005 Express Edition 資料庫執行個體中，我們必須跟著更新`NORTHWNDConnectionString`值。 開啟`Web.config`檔案中的網站和變更`NORTHWNDConnectionString`值，以便讀取連接字串： `Data Source=localhost\SQLExpress;Initial Catalog=Northwind;Integrated Security=True`。 這項變更之後, 您`<connectionStrings>`一節中`Web.config`看起來應該如下所示：


[!code-xml[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample1.xml)]

> [!NOTE]
> 中所述[前述教學課程](debugging-stored-procedures-cs.md)時偵錯用戶端應用程式中，從 SQL Server 物件，例如 ASP.NET 網站，我們必須停用連接共用。 如上所示的連接字串會停用連線共用 ( `Pooling=false` )。 如果您不打算從 ASP.NET 網站，偵錯 managed 預存程序和 Udf，啟用連接共用。


## <a name="step-3-creating-a-managed-stored-procedure"></a>步驟 3： 建立 Managed 預存程序

若要加入 Northwind 資料庫中的受管理的預存程序，我們需要建立預存程序，為 SQL Server 專案中的方法。 從 方案總管 中，以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案名稱，然後選擇 加入新項目。 這會顯示 [加入新項目] 對話方塊，列出可以加入至專案的 managed 的資料庫物件的類型。 如 [圖 8] 所示，這包括預存程序和使用者定義函式，其他項目。

可讓 s，將只會傳回所有已不再提供的產品的預存程序來開始。 將新的預存程序檔案`GetDiscontinuedProducts.cs`。


[![加入新的預存程序，以名為 GetDiscontinuedProducts.cs](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image13.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image12.png)

**圖 8**： 新增新預存程序名為`GetDiscontinuedProducts.cs`([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image14.png))


這會建立新的 C# 類別檔案，使用下列內容：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample2.cs)]

請注意，預存程序會實作成`static`方法內`partial`類別檔案命名為`StoredProcedures`。 此外，`GetDiscontinuedProducts`方法以裝飾`SqlProcedure attribute`，將會標示為預存程序的方法。

下列程式碼會建立`SqlCommand`物件以及設定其`CommandText`要`SELECT`會傳回所有的資料行的查詢`Products`其資料表適用於產品`Discontinued`欄位等於 1。 然後執行命令，並將結果傳送回用戶端應用程式。 新增下列程式碼`GetDiscontinuedProducts`方法。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample3.cs)]

所有的受管理的資料庫物件都具有存取權[`SqlContext`物件](https://msdn.microsoft.com/library/ms131108.aspx)表示呼叫端的內容。 `SqlContext`提供存取權[`SqlPipe`物件](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.aspx)透過其[`Pipe`屬性](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlcontext.pipe.aspx)。 這`SqlPipe`物件用來在兩者之間互相傳遞 SQL Server 資料庫與呼叫的應用程式之間的資訊。 正如其名， [ `ExecuteAndSend`方法](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlpipe.executeandsend.aspx)傳入執行`SqlCommand`物件並將結果傳回用戶端應用程式。

> [!NOTE]
> Managed 的資料庫物件是最適合用於預存程序和 Udf，使用程序邏輯，而不是以集合為基礎的邏輯。 程序邏輯牽涉到使用逐列為基礎的資料集，或使用純量資料。 `GetDiscontinuedProducts`我們剛剛建立，不過，方法牽涉到的任何程序的邏輯。 因此，它會在理想情況下會實作為 T-SQL 預存程序。 它會實作為受管理的預存程序來建立及部署所需的步驟會示範 managed 預存程序。


## <a name="step-4-deploying-the-managed-stored-procedure"></a>步驟 4： 部署 Managed 預存程序

使用這個完整的程式碼，我們已準備好將它部署到 Northwind 資料庫項目。 部署 SQL Server 專案的程式碼編譯成組件、 使用資料庫時，註冊組件和建立對應的物件在資料庫中，將它們連結至組件中適當的方法。 確切的 [部署] 選項所執行的工作集更精確地拼寫步驟 13。 以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案在 [方案總管] 中的名稱，然後選擇 [部署] 選項。 不過，部署會失敗，發生下列錯誤: 「 外部 」 附近的語法不正確。 您可能需要目前資料庫的相容性層級設定為較高的值，以啟用這項功能。 請參閱預存程序的說明`sp_dbcmptlevel`。

當您嘗試使用 Northwind 資料庫中註冊組件時，就會發生此錯誤訊息。 若要使用 SQL Server 2005 資料庫中註冊組件，資料庫 s 相容性層級必須設定為 90。 根據預設，新的 SQL Server 2005 資料庫有相容性層級 90。 不過，使用 Microsoft SQL Server 2000 建立的資料庫會有的預設相容性層級為 80。 因為 Northwind 資料庫一開始是 Microsoft SQL Server 2000 資料庫，其相容性層級目前設為 80，並因此需要增加為 90，才能註冊 managed 的資料庫物件。

若要更新資料庫 s 相容性層級，在 Management Studio 中開啟新的查詢視窗，並輸入：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample4.sql)]

按一下 [執行] 圖示，以執行上述查詢中。


[![更新 Northwind 資料庫 s 相容性層級](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image16.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image15.png)

**圖 9**： 更新 Northwind 資料庫 s 相容性層級 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image17.png))


在更新之後的相容性層級，重新部署 SQL Server 專案。 此時不會發生錯誤應該完成的部署。

返回 SQL Server Management Studio，Northwind 資料庫的 物件總管 中，以滑鼠右鍵按一下並選擇 重新整理。 接下來，向下切入至 [可程式性] 資料夾，然後再展開 [組件] 資料夾。 如 [圖 10] 所示，Northwind 資料庫現在會包含所產生的組件`ManagedDatabaseConstructs`專案。


![ManagedDatabaseConstructs 組件是現在已向 Northwind 資料庫](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image18.png)

**圖 10**:`ManagedDatabaseConstructs`組件是現在已向 Northwind 資料庫


也依序展開 [預存程序] 資料夾。 您會看到名為預存程序`GetDiscontinuedProducts`。 這個預存程序所建立的部署程序和指向`GetDiscontinuedProducts`方法中的`ManagedDatabaseConstructs`組件。 當`GetDiscontinuedProducts`預存程序執行時，它，接著執行`GetDiscontinuedProducts`方法。 由於這是受管理的預存程序無法編輯透過 Management Studio (因此預存程序名稱旁邊的鎖定圖示)。


![GetDiscontinuedProducts 預存程序會列在預存程序 資料夾](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image19.png)

**[圖 11**:`GetDiscontinuedProducts`預存程序會列在預存程序] 資料夾


還有一個更多的障礙，我們必須克服前我們可以呼叫 managed 預存程序： 將資料庫設定以防止執行 managed 程式碼。 驗證，請開啟新的查詢視窗並執行`GetDiscontinuedProducts`預存程序。 您會收到下列錯誤訊息： 已停用的.NET Framework 中的使用者程式碼的執行。 啟用 clr 已啟用組態選項。

若要檢查的 Northwind 資料庫的組態資訊、 輸入並執行命令`exec sp_configure`在查詢視窗中。 這會顯示設定 clr 已啟用目前設定為 0。


[![Clr 已啟用 設定為 0 是 目前設定](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image21.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image20.png)

**圖 12**: clr 已啟用] 設定為 0 是 [目前設定 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image22.png))


請注意，圖 12 中的每個組態設定一起列出的四個值： 最小值和最大值的組態和執行的值。 若要更新 [clr 已啟用] 設定的設定值，執行下列命令：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample5.sql)]

如果您重新執行`exec sp_configure`您會看到上述陳述式更新 clr 已啟用設定的設定值，設為 1，但仍執行的值設定為 0。 我們需要執行這項設定變更才會生效[`RECONFIGURE`命令](https://msdn.microsoft.com/library/ms176069.aspx)，它會將執行的值設定為目前的組態值。 只要輸入`RECONFIGURE`在查詢視窗中，按一下工具列中的 [執行] 圖示。 如果您執行`exec sp_configure`現在，您應該看到的值為 1，clr 已啟用設定的組態，然後執行值。

Clr 已啟用設定完成後，我們已準備好執行 managed`GetDiscontinuedProducts`預存程序。 在查詢視窗中輸入，然後執行命令`exec` `GetDiscontinuedProducts`。 叫用預存程序會導致對應的 managed 程式碼中`GetDiscontinuedProducts`方法來執行。 此程式碼發出`SELECT`查詢以傳回已停用，並傳回呼叫的應用程式，也就是 SQL Server Management Studio 這個執行個體中的這項資料的所有產品。 Management Studio 接收這些結果，並在 [結果] 視窗中加以顯示。


[![預存程序會傳回所有 GetDiscontinuedProducts 停產的產品](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image24.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image23.png)

**圖 13**:`GetDiscontinuedProducts`預存程序傳回所有已停止的產品 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image25.png))


## <a name="step-5-creating-managed-stored-procedures-that-accept-input-parameters"></a>步驟 5： 建立 Managed 預存程序會接受輸入的參數

許多查詢和預存程序，我們建立了在這些教學課程中使用*參數*。 例如，在[建立新預存程序的輸入資料集 Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程中我們建立名為預存程序`GetProductsByCategoryID`，接受名為輸入的參數`@CategoryID`。 預存程序，則傳回所有產品之`CategoryID`欄位比對所提供的值`@CategoryID`參數。

若要建立受管理的預存程序可接受輸入的參數，只需 s 方法定義中指定這些參數。 為了說明這點，可讓 s 新增另一個 managed 預存程序`ManagedDatabaseConstructs`專案，命名為`GetProductsWithPriceLessThan`。 此受管理的預存程序會接受輸入的參數指定的價格，而且會傳回所有產品的`UnitPrice`欄位小於參數 s 的值。

將新的預存程序加入至專案，以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案名稱，然後選擇 加入新的預存程序。 將檔案命名為 `GetProductsWithPriceLessThan.cs`。 如我們在步驟 3 中所見的這會建立新的 C# 類別檔案命名的方法`GetProductsWithPriceLessThan`置於`partial`類別`StoredProcedures`。

更新`GetProductsWithPriceLessThan`s 方法定義，使其接受[ `SqlMoney` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.aspx)名為輸入的參數`price`和撰寫程式碼執行，並傳回查詢結果：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample6.cs)]

`GetProductsWithPriceLessThan` s 方法定義和程式碼十分相似的定義和程式碼`GetDiscontinuedProducts`步驟 3 中建立的方法。 唯一的差異是，`GetProductsWithPriceLessThan`方法會接受做為輸入參數 (`price`)，則`SqlCommand`s 查詢包含參數 (`@MaxPrice`)，並將參數新增至`SqlCommand`s`Parameters`集合和將值指派給`price`變數。

新增此程式碼之後, 重新部署 SQL Server 專案。 接下來，回到 SQL Server Management Studio，並重新整理的預存程序資料夾。 您應該會看到新的項目， `GetProductsWithPriceLessThan`。 從 [查詢] 視窗中，輸入，然後執行命令`exec GetProductsWithPriceLessThan 25`，也將列出所有產品小於 $25，如 [圖 14] 所示。


[![顯示產品下 $25](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image27.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image26.png)

**圖 14**： 顯示產品下 $25 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image28.png))


## <a name="step-6-calling-the-managed-stored-procedure-from-the-data-access-layer"></a>步驟 6： 從資料存取層呼叫 Managed 預存程序

此時，我們已新增`GetDiscontinuedProducts`並`GetProductsWithPriceLessThan`managed 預存程序`ManagedDatabaseConstructs`專案，並已向它們 Northwind SQL Server 資料庫。 我們也叫用這些 managed 預存程序從 SQL Server Management Studio （請參閱 圖 13 與 14，s）。 為了讓我們的 ASP.NET 應用程式使用這些 managed 預存程序，不過，我們需要將它們新增到架構中的商務邏輯層與資料存取。 在此步驟中，我們會加入兩種新方法`ProductsTableAdapter`中`NorthwindWithSprocs`類型的 DataSet，最初在中建立[建立新預存程序的輸入資料集 Tableadapter](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程。 在步驟 7 中，我們會將對應的方法加入 BLL。

開啟`NorthwindWithSprocs`型別資料集，在 Visual Studio，並藉由新增新的方法來開始`ProductsTableAdapter`名為`GetDiscontinuedProducts`。 若要加入 TableAdapter 的新方法，以滑鼠右鍵按一下設計工具中的 TableAdapter 的名稱，並從內容功能表中選擇 加入查詢選項。

> [!NOTE]
> 因為我們已從 Northwind 資料庫`App_Data`資料夾中的 SQL Server 2005 Express Edition 資料庫執行個體，請務必在 Web.config 中對應的連接字串更新，以反映這項變更。 我們將討論在步驟 2 中更新`NORTHWNDConnectionString`中的值`Web.config`。 如果您忘記執行這項更新，您會看到錯誤訊息無法加入查詢。 找不到連接`NORTHWNDConnectionString`物件`Web.config`在對話方塊中，當嘗試將新方法加入至 TableAdapter。 若要解決這個錯誤，請按一下 [確定]，然後移至`Web.config`並更新`NORTHWNDConnectionString`值在步驟 2 中所述。 然後再次嘗試重新將方法加入至 TableAdapter。 這次它應該會運作不會發生錯誤。


加入新的方法，就會啟動我們在過去的教學課程中多次使用 [TableAdapter 查詢組態精靈]。 第一個步驟會要求我們指定 TableAdapter 存取資料庫的方式： 透過臨機操作 SQL 陳述式，或透過新的或現有的預存程序。 因為我們已建立並註冊`GetDiscontinuedProducts`managed 預存程序，與資料庫中，選擇 使用現有預存程序選項，然後按 下一步。


[![選擇 使用現有的預存程序選項](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image30.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image29.png)

**圖 15**： 選擇 使用現有預存程序選項 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image31.png))


下一個畫面會提示我們輸入方法會叫用預存程序。 選擇`GetDiscontinuedProducts`managed 預存程序，從下拉式清單，然後按 [下一步]。


[![選取 GetDiscontinuedProducts Managed 預存程序](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image33.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image32.png)

**圖 16**： 選取  `GetDiscontinuedProducts` Managed 預存程序 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image34.png))


系統會要求我們指定預存程序傳回資料列、 單一值，或執行任何動作。 因為`GetDiscontinuedProducts`傳回集合的停產的產品資料列中，選擇第一個選項 （表格式資料），並按一下 [下一步]。


[![選取 表格式資料選項](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image36.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image35.png)

**圖 17**： 選取表格式的 [資料] 選項 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image37.png))


最後一個精靈畫面可讓我們指定使用的資料存取模式，以及產生方法的名稱。 保留核取方塊已核取和名稱的方法`FillByDiscontinued`和`GetDiscontinuedProducts`。 按一下 完成 以完成精靈。


[![名稱方法 FillByDiscontinued 和 GetDiscontinuedProducts](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image39.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image38.png)

**圖 18**: Name 方法`FillByDiscontinued`並`GetDiscontinuedProducts`([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image40.png))


重複上述步驟來建立名為的方法`FillByPriceLessThan`並`GetProductsWithPriceLessThan`中`ProductsTableAdapter`如`GetProductsWithPriceLessThan`managed 預存程序。

[圖 19] 顯示的 DataSet 設計工具的螢幕擷取畫面之後將方法加入至`ProductsTableAdapter`for`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`managed 預存程序。


[![ProductsTableAdapter 包含在此步驟中新增新方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image42.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image41.png)

**圖 19**:`ProductsTableAdapter`包含在此步驟中的 新的方法加入 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image43.png))


## <a name="step-7-adding-corresponding-methods-to-the-business-logic-layer"></a>步驟 7： 將相對應的方法加入商務邏輯層

既然我們已更新資料的存取層，來包含方法呼叫加入在步驟 4 和 5 中的 managed 預存程序，我們需要對應的方法加入商務邏輯層。 新增下列兩個方法來`ProductsBLLWithSprocs`類別：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample7.cs)]

這兩種方法只會呼叫對應的 DAL 方法，並傳回`ProductsDataTable`執行個體。 `DataObjectMethodAttribute`上述每個方法的標記會導致 ObjectDataSource s 設定資料來源精靈 的 選取 索引標籤中的下拉式清單中包含這些方法。

## <a name="step-8-invoking-the-managed-stored-procedures-from-the-presentation-layer"></a>步驟 8： 叫用 Managed 預存程序從展示層

使用商務邏輯和資料存取層級增強，以包括呼叫的支援`GetDiscontinuedProducts`和`GetProductsWithPriceLessThan`managed 預存程序，我們現在可以顯示這些預存程序結果，透過 ASP.NET 網頁。

開啟`ManagedFunctionsAndSprocs.aspx`頁面中`AdvancedDAL`資料夾，並從 [工具箱] 拖曳至設計工具拖曳至 GridView。 設定 GridView s`ID`屬性，以`DiscontinuedProducts`和它的智慧標籤，從繫結至名為新 ObjectDataSource `DiscontinuedProductsDataSource`。 設定提取其資料從 ObjectDataSource`ProductsBLLWithSprocs`類別的`GetDiscontinuedProducts`方法。


[![設定使用 ProductsBLLWithSprocs 類別 ObjectDataSource](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image45.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image44.png)

**圖 20**： 設定要使用 ObjectDataSource`ProductsBLLWithSprocs`類別 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image46.png))


[![從下拉式清單中選取的索引標籤中選擇 GetDiscontinuedProducts 方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image48.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image47.png)

**圖 21**： 選擇`GetDiscontinuedProducts`方法，從下拉式清單中選取索引標籤中 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image49.png))


因為此方格會用來只顯示產品資訊，設定下拉式清單中更新、 插入和刪除索引標籤為 （無） 以及然後按一下 完成。

在完成精靈，Visual Studio 會自動將新增 BoundField 或 CheckBoxField 中每個資料欄位`ProductsDataTable`。 花點時間，移除所有的這些欄位除外`ProductName`和`Discontinued`，而此時您的 GridView 和 ObjectDataSource s 宣告式標記看起來應該如下所示：


[!code-aspx[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample8.aspx)]

請花一點時間才能檢視此頁面，透過瀏覽器。 當瀏覽頁面時，ObjectDataSource 會呼叫`ProductsBLLWithSprocs`類別的`GetDiscontinuedProducts`方法。 如我們所見步驟 7 中，這個方法會呼叫向下到 DAL`ProductsDataTable`類別 s`GetDiscontinuedProducts`方法，它會叫用`GetDiscontinuedProducts`預存程序。 這個預存程序是 managed，則預存程序，並執行我們在步驟 3 中，傳回已停用的產品中建立的程式碼。

Managed 預存程序所傳回的結果封裝成`ProductsDataTable`dal 後又返回的 BLL，然後將它們傳回展示層，它們會繫結至 GridView 和顯示的位置。 如預期般，方格會列出已不再提供這些產品。


[![列出已停止的產品](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image51.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image50.png)

**圖 22**： 列出已停止的產品 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image52.png))


進一步練習時，將文字方塊和另一個 GridView，加入頁面。 有顯示產品低於金額在 TextBox 輸入藉由呼叫這個 GridView`ProductsBLLWithSprocs`類別的`GetProductsWithPriceLessThan`方法。

## <a name="step-9-creating-and-calling-t-sql-udfs"></a>步驟 9： 建立和呼叫 T-SQL Udf

使用者定義函數或 Udf，是資料庫物件精確地模擬的程式設計語言中的函式的語意。 像在 C# 函式，Udf 可以包含多個輸入參數，並傳回特定類型的值。 UDF 可以傳回純量資料-字串、 整數、 和等位或表格式資料。 可讓 s 快速看看這兩種類型的 Udf，開頭為 UDF，以傳回純量資料類型。

下列的 UDF 計算特定產品的清查的估計的值。 它是藉由在三個輸入參數-採取`UnitPrice`， `UnitsInStock`，並`Discontinued`傳回值的型別和值特定產品- `money`。 它會計算存貨估計的值乘以`UnitPrice`由`UnitsInStock`。 已停止的項目，這個值會變成一半。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample9.sql)]

此 UDF 新增至資料庫之後, 就可以找到透過 Management Studio 藉由展開可程式性 資料夾中，則函式，然後純量值函式。 它可以用於`SELECT`查詢就像這樣：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample10.sql)]

我已新增`udf_ComputeInventoryValue`UDF Northwind 資料庫中;[圖 23] 顯示上述輸出`SELECT`時透過 Management Studio 檢視查詢。 也請注意，UDF 會列在 物件總管 中的純量值函式 資料夾底下。


[![列出每項產品的庫存值](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image54.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image53.png)

**圖 23**： 列出庫存值的每個產品 s ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image55.png))


Udf 也可以傳回表格式資料。 比方說，我們可以建立 UDF，以傳回屬於特定分類的產品：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample11.sql)]

`udf_GetProductsByCategoryID`接受 UDF`@CategoryID`輸入的參數，並傳回指定之結果`SELECT`查詢。 建立之後，可以在中參考此 UDF `FROM` (或`JOIN`) 子句`SELECT`查詢。 下列範例會傳回`ProductID`， `ProductName`，和`CategoryID`各種飲料的值。


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample12.sql)]

我已新增`udf_GetProductsByCategoryID`UDF Northwind 資料庫中;[圖 24] 顯示上述輸出`SELECT`時透過 Management Studio 檢視查詢。 傳回表格式資料的 Udf 可以在物件總管 中的 s 資料表值函式資料夾中找到。


[![ProductID、 ProductName、 及 CategoryID 列出每個飲料](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image57.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image56.png)

**圖 24**: `ProductID`， `ProductName`，以及`CategoryID`列出每個飲料 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image58.png))


> [!NOTE]
> 如需有關建立和使用 Udf 的詳細資訊，請參閱[使用者定義函式簡介](http://www.sqlteam.com/item.asp?ItemID=1955)。 也瞧瞧[優點和 Drawbacks of User-Defined 函式](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)。


## <a name="step-10-creating-a-managed-udf"></a>步驟 10： 建立 Managed 的 UDF

`udf_ComputeInventoryValue`和`udf_GetProductsByCategoryID`上述範例中建立的 Udf 是 T-SQL 的資料庫物件。 SQL Server 2005 也支援受管理的 Udf，可以加入至`ManagedDatabaseConstructs`專案就像 managed 預存程序，從步驟 3 和 5。 此步驟中，可讓實作 s `udf_ComputeInventoryValue` managed 程式碼中的 UDF。

若要將 managed 的 UDF 以便`ManagedDatabaseConstructs`專案、 方案總管 中的專案名稱上按一下滑鼠右鍵，然後選擇要加入新項目。 從 [加入新項目] 對話方塊中選取使用者定義的範本，並將新的 UDF 檔案命名`udf_ComputeInventoryValue_Managed.cs`。


[![ManagedDatabaseConstructs 專案中加入新的 Managed 的 UDF](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image60.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image59.png)

**圖 25**： 新增新 Managed UDF`ManagedDatabaseConstructs`專案 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image61.png))


使用者定義函式範本會建立`partial`名為類別`UserDefinedFunctions`方法，其名稱是做為類別 s 的檔案名稱相同 (`udf_ComputeInventoryValue_Managed`，這個執行個體中)。 這個方法使用裝飾[`SqlFunction`屬性](https://msdn.microsoft.com/library/microsoft.sqlserver.server.sqlfunctionattribute.aspx)的旗標為 managed UDF 方法。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample13.cs)]

`udf_ComputeInventoryValue`方法目前會傳回[`SqlString`物件](https://msdn.microsoft.com/library/system.data.sqltypes.sqlstring.aspx)，不接受任何輸入參數。 我們要更新的方法定義，讓它接受三個輸入參數- `UnitPrice`， `UnitsInStock`，並`Discontinued`-並傳回`SqlMoney`物件。 計算存貨值的邏輯是完全在 T-SQL 中相同`udf_ComputeInventoryValue`UDF。


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample14.cs)]

請注意，UDF 方法 s 的輸入的參數是其對應的 SQL 型別： `SqlMoney` for`UnitPrice`欄位中， [ `SqlInt16` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlint16.aspx)如`UnitsInStock`，和[ `SqlBoolean` ](https://msdn.microsoft.com/library/system.data.sqltypes.sqlboolean.aspx)針對`Discontinued`。 這些資料型別反映中定義的類型`Products`資料表： `UnitPrice` sloupec je typu `money`，則`UnitsInStock`類型的資料行`smallint`，而`Discontinued`類型的資料行`bit`。

程式碼一開始會建立`SqlMoney`執行個體名為`inventoryValue`指派值為 0。 `Products`資料庫的資料表允許`NULL`中的值`UnitsInPrice`和`UnitsInStock`資料行。 因此，我們需要先檢查以查看這些值是否包含`NULL`s，我們透過`SqlMoney`物件 s [ `IsNull`屬性](https://msdn.microsoft.com/library/system.data.sqltypes.sqlmoney.isnull.aspx)。 如果兩個`UnitPrice`並`UnitsInStock`包含非`NULL`值，那麼我們計算`inventoryValue`是兩個產品。 然後，如果`Discontinued`為 true，則我們可以藉此值。

> [!NOTE]
> `SqlMoney`物件只允許兩個`SqlMoney`一起相乘的執行個體。 它不允許`SqlMoney`先乘以常值的浮點數的執行個體。 因此，到一半`inventoryValue`我們將它乘以新`SqlMoney`執行個體，其值為 0.5。


## <a name="step-11-deploying-the-managed-udf"></a>步驟 11： 部署受管理的 UDF

現在，建立受管理的 UDF 之後，我們已準備好將它部署到 Northwind 資料庫。 如我們所見在步驟 4 中，會部署在 SQL Server 專案中的受管理的物件 [方案總管] 中的專案名稱上按一下滑鼠右鍵，並從操作功能表選擇 [部署] 選項。

一旦您已部署專案，請返回 SQL Server Management Studio，並重新整理純量值函式資料夾。 現在，您應該會看到兩個項目：

- `dbo.udf_ComputeInventoryValue` -步驟 9 中建立的 T-SQL UDF 和
- `dbo.udf ComputeInventoryValue_Managed` -只是已部署的步驟 10 中建立的受管理的 UDF。

若要測試此受管理的 UDF，請執行下列查詢從 Management Studio 中：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample15.sql)]

此命令會使用 managed`udf ComputeInventoryValue_Managed`而不是 T-SQL 的 UDF `udf_ComputeInventoryValue` UDF，但輸出會相同。 若要查看 UDF 的輸出的螢幕擷取畫面的 圖 23 回頭參考。

## <a name="step-12-debugging-the-managed-database-objects"></a>步驟 12： 偵錯 Managed 的資料庫物件

在 [偵錯預存程序](debugging-stored-procedures-cs.md)進行偵錯透過 Visual Studio 的 SQL Server 的教學課程中我們將討論三個選項： 直接資料庫偵錯、 應用程式偵錯，以及從 SQL Server 專案的偵錯。 從用戶端應用程式，以及直接從 SQL Server 專案，請管理的資料庫物件無法透過直接資料庫偵錯，偵錯，但可以進行偵錯。 為了讓偵錯能夠運作，不過，SQL Server 2005 資料庫必須允許 SQL/CLR 偵錯。 請注意，當我們第一次建立`ManagedDatabaseConstructs`Visual Studio 要求我們是否要啟用 SQL/CLR 偵錯 （請參閱 圖 6 在步驟 2） 的專案。 以滑鼠右鍵按一下 [伺服器總管] 視窗的資料庫，就可以修改此設定。


![請確定資料庫允許 SQL/CLR 偵錯](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image62.png)

**圖 26**： 請確定資料庫允許 SQL/CLR 偵錯


假設我們想要偵錯`GetProductsWithPriceLessThan`managed 預存程序。 藉由設定中斷點的程式碼中，我們會開始`GetProductsWithPriceLessThan`方法。


[![GetProductsWithPriceLessThan 方法中設定中斷點](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image64.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image63.png)

**圖 27**： 在設定的中斷點`GetProductsWithPriceLessThan`方法 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image65.png))


可讓 s 第一次查看偵錯 managed 的資料庫物件，從 SQL Server 專案。 因為我們的解決方案包含兩個專案- `ManagedDatabaseConstructs` SQL Server 專案，以及我們的網站-若要從 SQL Server 專案，我們必須指示來啟動 Visual Studio 偵錯`ManagedDatabaseConstructs`我們開始偵錯時，SQL Server 專案。 以滑鼠右鍵按一下`ManagedDatabaseConstructs`專案在 方案總管，然後選擇 設定為啟始專案選項，從內容功能表。

當`ManagedDatabaseConstructs`對執行中的 SQL 陳述式的偵錯工具啟動專案`Test.sql`檔案，位於`Test Scripts`資料夾。 例如，若要測試`GetProductsWithPriceLessThan`managed 預存程序，取代現有`Test.sql`檔案內容，使用下列陳述式，會叫用`GetProductsWithPriceLessThan`managed 預存程序傳入`@CategoryID`14.95 的值：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample16.sql)]

一旦您已輸入到上述指令碼`Test.sql`、 開始偵錯移至 偵錯 功能表並選擇 開始偵錯，或按 F5 或綠色工具列中 播放 圖示。 這會建置專案的方案中，Northwind 資料庫中，部署 managed 的資料庫物件，然後執行`Test.sql`指令碼。 此時，會叫用中斷點，而且我們可以逐步執行`GetProductsWithPriceLessThan`方法，檢查輸入參數的值等等。


[![叫用中斷點，在 GetProductsWithPriceLessThan 方法](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image67.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image66.png)

**圖 28**： 在中斷點`GetProductsWithPriceLessThan`已叫用方法 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image68.png))


為了要透過用戶端應用程式進行偵錯的 SQL 資料庫物件，因此務必資料庫設定來支援應用程式偵錯。 以滑鼠右鍵按一下伺服器總管] 中的資料庫，並確定已核取 [應用程式偵錯的選項。 此外，我們需要設定 ASP.NET 應用程式與 SQL 偵錯工具整合，並在停用連接共用。 下列步驟所述的步驟 2 中的詳細資料[偵錯預存程序](debugging-stored-procedures-cs.md)教學課程。

一旦您已設定的 ASP.NET 應用程式和資料庫，將 ASP.NET 網站設定為啟始專案，並開始偵錯。 如果您造訪的頁面，會呼叫具有中斷點的受管理物件的其中一個時，應用程式將會中止，控制項將會開啟偵錯工具，其中您可以逐步執行程式碼如圖 28 所示。

## <a name="step-13-manually-compiling-and-deploying-managed-database-objects"></a>步驟 13： 手動編譯和部署 Managed 資料庫物件

SQL Server 專案，讓您輕鬆建立、 編譯及部署 managed 的資料庫物件。 不幸的是，SQL Server 專案中才有的 Visual Studio Professional 和 Team 系統版本。 如果您使用 Visual Web Developer 或 Standard Edition 的 Visual Studio，並想要使用 managed 的資料庫物件，您必須以手動方式建立，並將其部署。 這牽涉到四個步驟：

1. 建立此檔案包含 managed 的資料庫物件的原始程式碼
2. 編譯為組件中物件
3. 使用 SQL Server 2005 資料庫時，註冊組件和
4. 指向組件中的適當方法的 SQL Server 中建立資料庫物件。

說明這些工作，讓建立新的受管理的預存程序會傳回這些產品的`UnitPrice`大於指定的值。 建立名為您電腦上的新檔案`GetProductsWithPriceGreaterThan.cs`和輸入檔案中的下列程式碼 （您可以使用 Visual Studio 中，[記事本] 或任何文字編輯器來完成這項作業）：


[!code-csharp[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample17.cs)]

此程式碼是幾乎完全相同的`GetProductsWithPriceLessThan`步驟 5 中建立的方法。 唯一的差異是方法名稱，`WHERE`子句，而且查詢中使用的參數名稱。 回到`GetProductsWithPriceLessThan`方法中，`WHERE`讀取的子句： `WHERE UnitPrice < @MaxPrice`。 此處，請在`GetProductsWithPriceGreaterThan`，我們使用： `WHERE UnitPrice > @MinPrice` 。

我們現在需要此類別編譯為組件。 從命令列中，瀏覽至您的儲存位置的目錄`GetProductsWithPriceGreaterThan.cs`檔案，並使用 C# 編譯器 (`csc.exe`) 來編譯為組件的類別檔案：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample18.cmd)]

如果包含的資料夾`csc.exe`中不在系統 s `PATH`，您必須完整參考它的路徑， `%WINDOWS%\Microsoft.NET\Framework\version\`，就像這樣：


[!code-console[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample19.cmd)]


[![GetProductsWithPriceGreaterThan.cs 編譯為組件](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image70.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image69.png)

**圖 29**： 編譯`GetProductsWithPriceGreaterThan.cs`成組件 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image71.png))


`/t`旗標會指定 C# 類別檔案，應編譯成 DLL （而不是可執行檔）。 `/out`旗標會指定產生的組件的名稱。

> [!NOTE]
> 而不是編譯`GetProductsWithPriceGreaterThan.cs`從命令列，您也可以使用的類別檔案[Visual C# Express 版](https://msdn.microsoft.com/vstudio/express/visualcsharp/)或 Visual Studio Standard Edition 中建立個別的類別庫專案。 S ren Jacob Lauritsen 請提供程式碼具有這類的 Visual C# Express 版專案`GetProductsWithPriceGreaterThan`預存程序和兩個 managed 預存程序，並在步驟 3、 5 和 10 中建立的 UDF。 S ren s 專案也包含新增對應的資料庫物件所需的 T-SQL 命令。


程式碼編譯成組件時，我們已準備好註冊 SQL Server 2005 資料庫內組件項目。 這可透過 T-SQL，使用命令來執行`CREATE ASSEMBLY`，或透過 SQL Server Management Studio。 使用 Management Studio 讓 s 焦點。

從 Management Studio 中，展開 Northwind 資料庫中的 可程式性 資料夾。 及其子資料夾是組件。 手動將新的組件新增至資料庫中，以滑鼠右鍵按一下組件 資料夾，並從操作功能表中選擇新的組件。 新組件對話方塊 （請參閱圖 30） 此顯示。 按一下 瀏覽按鈕，選取`ManuallyCreatedDBObjects.dll`我們剛所編譯，，，然後按一下 確定 以加入資料庫中的組件的組件。 您不應該看見`ManuallyCreatedDBObjects.dll`物件總管 中的組件。


[![將 ManuallyCreatedDBObjects.dll 組件加入至資料庫](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image73.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image72.png)

**圖 30**： 新增`ManuallyCreatedDBObjects.dll`資料庫的組件 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image74.png))


![ManuallyCreatedDBObjects.dll 會列在 [物件總管]](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image75.png)

**圖 31**:`ManuallyCreatedDBObjects.dll`會列在 [物件總管]


雖然我們已加入組件至 Northwind 資料庫，我們尚未建立關聯的預存程序`GetProductsWithPriceGreaterThan`組件中的方法。 若要這麼做，開啟新的查詢視窗並執行下列指令碼：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample20.sql)]

這會在名為 Northwind 資料庫中建立新的預存程序`GetProductsWithPriceGreaterThan`，並產生關聯的 managed 方法`GetProductsWithPriceGreaterThan`(這是在類別中`StoredProcedures`，這是組件中`ManuallyCreatedDBObjects`)。

執行上述指令碼之後, 重新整理物件總管 中的 預存程序 資料夾。 您應該會看到新的預存程序項目- `GetProductsWithPriceGreaterThan` -具有鎖定圖示旁邊。 若要測試此預存程序，請輸入，並在查詢視窗中執行下列指令碼：


[!code-sql[Main](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/samples/sample21.sql)]

如圖 32 所示，上述命令會顯示與這些產品的資訊`UnitPrice`大於美金 24.95。


[![ManuallyCreatedDBObjects.dll 會列在 [物件總管]](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image77.png)](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image76.png)

**圖 32**:`ManuallyCreatedDBObjects.dll`會列在 [物件總管] 中 ([按一下以檢視完整大小的影像](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs/_static/image78.png))


## <a name="summary"></a>總結

Microsoft SQL Server 2005 提供整合與 Common Language Runtime (CLR)，可使用 managed 程式碼建立的資料庫物件。 先前，這些資料庫物件只能建立使用 T-SQL，但現在我們可以在其中建立這些物件使用的.NET 程式設計語言，例如 C#。 在本教學課程，我們建立兩個受管理預存程序和 managed 使用者定義函式。

Visual Studio 的 SQL Server 專案類型可協助建立、 編譯和部署 managed 的資料庫物件。 此外，它提供豐富的偵錯支援。 不過，SQL Server 專案類型中才有的 Visual Studio Professional 和 Team 系統版本。 對於使用 Visual Web Developer 或 Standard Edition 的 Visual Studio、 建立、 編譯和部署步驟必須手動執行，如我們所見步驟 13。

快樂地寫程式 ！

## <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [優點和缺點的使用者定義函式](http://www.samspublishing.com/articles/article.asp?p=31724&amp;rl=1)
- [在 Managed 程式碼中建立 SQL Server 2005 物件](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [建立觸發程序使用 SQL Server 2005 中的 Managed 程式碼](http://www.15seconds.com/issue/041006.htm)
- [如何： 建立和執行 CLR 的 SQL Server 預存程序](https://msdn.microsoft.com/library/5czye81z(VS.80).aspx)
- [如何： 建立和執行 CLR 的 SQL Server 使用者定義函式](https://msdn.microsoft.com/library/w2kae45k(VS.80).aspx)
- [如何： 編輯`Test.sql`執行 SQL 物件的指令碼](https://msdn.microsoft.com/library/ms233682(VS.80).aspx)
- [簡介使用者定義函式](http://www.sqlteam.com/item.asp?ItemID=1955)
- [Managed 程式碼和 SQL Server 2005 （影片）](https://channel9.msdn.com/Showpost.aspx?postid=142413)
- [TRANSACT-SQL 參考](https://msdn.microsoft.com/library/aa299742(SQL.80).aspx)
- [逐步解說： 在 Managed 程式碼建立預存程序](https://msdn.microsoft.com/library/zxsa8hkf(VS.80).aspx)

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於 [http://ScottOnWriting.NET](http://ScottOnWriting.NET)。

## <a name="special-thanks-to"></a>特別感謝

本教學課程系列是由許多實用的檢閱者檢閱。 本教學課程中的潛在客戶檢閱者已 S ren Jacob Lauritsen。 除了檢閱此文件、 S ren 也建立了包含在此發行項的下載手動編譯 managed 的資料庫物件中的 Visual C# Express 版專案。 有興趣檢閱我即將推出的 MSDN 文章嗎？ 如果是這樣，psychic 在[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)

> [!div class="step-by-step"]
> [上一頁](debugging-stored-procedures-cs.md)
> [下一頁](creating-new-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)
