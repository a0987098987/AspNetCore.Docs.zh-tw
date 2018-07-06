---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
title: 偵錯預存程序 (VB) |Microsoft Docs
author: rick-anderson
description: Visual Studio Professional 和 Team System 版本可讓您設定中斷點，或是 SQL Server 中的預存程序中的步驟使偵錯預存...
ms.author: aspnetcontent
ms.date: 08/03/2007
ms.assetid: 9ed8ccb5-5f31-4eb4-976d-cabf4b45ca09
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-vb
msc.type: authoredcontent
ms.openlocfilehash: a4a507205ff8e837f64e01acf84f6376006a87b9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37806647"
---
<a name="debugging-stored-procedures-vb"></a>偵錯預存程序 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_VB.zip)或[下載 PDF](debugging-stored-procedures-vb/_static/datatutorial74vb1.pdf)

> Visual Studio Professional 和 Team System 版本可讓您設定中斷點，並在 SQL Server 中的預存程序中的步驟使偵錯預存程序的偵錯應用程式程式碼一樣容易。 本教學課程會示範直接資料庫偵錯，以及應用程式偵錯預存程序。


## <a name="introduction"></a>簡介

Visual Studio 提供豐富的偵錯體驗。 有幾個按鍵輸入或按下滑鼠，就可以使用中斷點來停止執行程式，並檢查其狀態和控制流程。 偵錯應用程式程式碼，以及 Visual Studio 會提供偵錯預存程序內 SQL Server 的支援。 就像可以設定中斷點的程式碼的 ASP.NET 程式碼後置類別或商務邏輯層的類別，因此也可以將它們放在預存程序。

在本教學課程中，我們將探討逐步執行預存程序從伺服器總管 中，Visual Studio 內以及如何設定中斷點叫用預存程序呼叫時執行的 ASP.NET 應用程式。

> [!NOTE]
> 不幸的是，預存程序只能逐步執行和偵錯透過 Visual Studio Professional 和 Team 系統版本。 如果您使用 Visual Web Developer 或 standard 版本的 Visual Studio，您可以自由地閱讀沿著，因為我們會逐步偵錯預存程序，所需的步驟，但不是能複製您的電腦上的這些步驟。


## <a name="sql-server-debugging-concepts"></a>SQL Server 偵錯概念

Microsoft SQL Server 2005 已設計成提供與整合[Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)，這是所有的.NET 組件所使用的執行階段。 因此，SQL Server 2005 支援 managed 的資料庫物件。 也就是說，您可以建立預存程序、 使用者定義函數 (Udf) 之類的資料庫物件做為 Visual Basic 類別中的方法。 這可讓這些預存程序和 Udf，以利用.NET Framework 中，並從您自己的自訂類別的功能。 當然，SQL Server 2005 也會提供支援 T-SQL 的資料庫物件。

SQL Server 2005 提供 T-SQL 和 managed 的資料庫物件的偵錯支援。 不過，這些物件只可透過 Visual Studio 2005 Professional 和 Team 系統版本偵錯。 在本教學課程中，我們將檢查偵錯 T-SQL 的資料庫物件。 後續的教學課程會查看偵錯 managed 的資料庫物件。

[概觀的 T-SQL 和 CLR 偵錯在 SQL Server 2005](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx)部落格文章，從[SQL Server 2005 CLR 整合小組](https://blogs.msdn.com/sqlclr/default.aspx)會反白顯示 偵錯從 Visual Studio 的 SQL Server 2005 物件的三種方式：

- **直接資料庫偵錯設計 (DDD)** -從伺服器總管，我們可以逐步執行任何 T-SQL 的資料庫物件，例如預存程序和 Udf。 我們將檢視在步驟 1 中的 DDD。
- **應用程式偵錯**-我們可以將資料庫物件中的中斷點，然後再執行 ASP.NET 應用程式。 執行資料庫物件時，會叫用中斷點，並開啟偵錯工具的控制項。 請注意，應用程式偵錯我們無法逐步執行資料庫物件從應用程式的程式碼。 我們必須明確地設定中斷點這些預存程序或 Udf 我們想要停止偵錯工具。 應用程式偵錯，會檢查從步驟 2 開始。
- **從 SQL Server 專案偵錯**-Visual Studio Professional 和 Team 系統版本包含常用來建立 managed 的資料庫物件的 SQL Server 專案類型。 使用 SQL Server 專案，我們會檢驗和下一個教學課程中的偵錯其內容。

Visual Studio 可以偵錯在本機和遠端 SQL Server 執行個體上的預存程序。 本機的 SQL Server 執行個體是安裝在與 Visual Studio 相同的電腦上。 如果您使用 SQL Server 資料庫並不在您的開發電腦上，它就會視為遠端執行個體。 這些教學課程中我們使用本機 SQL Server 執行個體。 偵錯遠端的 SQL server 執行個體上的預存程序需要比當偵錯在本機執行個體上的預存程序的其他組態步驟。

如果您使用本機的 SQL Server 執行個體，您就可以開始進行步驟 1，並逐步完成本教學課程結尾。 如果您使用遠端 SQL Server 執行個體，不過，您將必須先確定當您偵錯時都會記錄到您的開發電腦的遠端執行個體上擁有 SQL Server 登入的 Windows 使用者帳戶。 Moveover，此資料庫登入和用來從執行的 ASP.NET 應用程式連接到資料庫的資料庫登入必須是成員`sysadmin`角色。 如需有關設定偵錯遠端執行個體的 [Visual Studio 和 SQL Server 的這個教學課程結尾處，在遠端執行個體] 區段中看到 T-SQL 偵錯資料庫物件。

最後，了解偵錯 T-SQL 的資料庫物件的支援不做為偵錯.NET 應用程式的支援豐富的功能。 例如，中斷點條件和篩選不支援，只可以使用偵錯視窗的子集，您無法使用 [編輯後繼續]、 即時運算視窗呈現沒有什麼用處，等等。 請參閱[偵錯工具命令和功能限制](https://msdn.microsoft.com/library/ms165035(VS.80).aspx)如需詳細資訊。

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>步驟 1： 直接逐步執行預存程序

Visual Studio 可讓您更輕鬆地直接偵錯資料庫物件。 讓 s 看看如何使用直接資料庫偵錯設計 (DDD) 功能來逐步執行`Products_SelectByCategoryID`Northwind 資料庫中預存程序。 正如其名，`Products_SelectByCategoryID`會傳回特定類別; 的產品資訊在其所建立[使用現有預存程序的輸入資料集 Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。 首先移至 [伺服器總管]，然後展開 Northwind 資料庫節點。 接下來，向下切入至 預存程序 資料夾，以滑鼠右鍵按一下`Products_SelectByCategoryID`預存程序，並從內容功能表中選擇 逐步執行預存程序選項。 這會啟動偵錯工具。

由於`Products_SelectByCategoryID`預存程序必須有`@CategoryID`輸入的參數，我們會要求您提供此值。 輸入 1，這會傳回飲料的相關資訊。


![使用值 1@CategoryID參數](debugging-stored-procedures-vb/_static/image1.png)

**圖 1**： 使用值 1`@CategoryID`參數


在提供的值之後`@CategoryID`參數，預存程序會執行。 而不是執行到完成為止，不過，偵錯工具暫止執行的第一個陳述式。 請注意界黃色箭頭，指出預存程序中的目前位置。 您可以檢視並編輯透過監看式視窗或藉由將滑鼠停留在預存程序的參數名稱的參數值。


[![偵錯工具已停止執行預存程序的第一個陳述式](debugging-stored-procedures-vb/_static/image3.png)](debugging-stored-procedures-vb/_static/image2.png)

**圖 2**： 使用偵錯工具已停止執行預存程序的第一個陳述式 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image4.png))


若要逐步執行一次的預存程序一個陳述式時，按一下工具列中的 [不進入函數] 按鈕或按 F10 鍵。 `Products_SelectByCategoryID`預存程序包含單一`SELECT`陳述式，因此按下 F10 將跳過單一陳述式，並完成預存程序的執行。 預存程序完成之後，其輸出會出現在 [輸出] 視窗和偵錯工具就會終止。

> [!NOTE]
> T-SQL 偵錯，就會發生在陳述式層級;您無法逐步執行`SELECT`陳述式。


## <a name="step-2-configuring-the-website-for-application-debugging"></a>步驟 2： 設定應用程式偵錯的網站

雖然很方便偵錯預存的程序，直接從 [伺服器總管] 中，在許多情況下我們會比較有興趣偵錯預存程序，從 ASP.NET 應用程式呼叫時。 我們可以將中斷點加入至 Visual Studio 中的預存程序，然後再啟動 偵錯 ASP.NET 應用程式。 從應用程式叫用具有中斷點的預存程序時，執行會在中斷點暫止，我們可以檢視及變更預存程序的參數值，並逐步執行它的陳述式，就像我們在步驟 1 中。

我們可以開始偵錯從應用程式呼叫的預存程序之前，我們必須指示 ASP.NET web 應用程式與 SQL Server 偵錯工具整合。 開始在 [方案總管] 中的網站名稱上按一下滑鼠右鍵 (`ASPNET_Data_Tutorial_74_VB`)。 從內容功能表中選擇 [屬性頁] 選項選取左側的 [啟動選項] 項目，請檢查 SQL Server 中的核取 [偵錯工具] 區段 （請參閱 [圖 3]）。


[![檢查應用程式的屬性頁中的 SQL Server 核取方塊](debugging-stored-procedures-vb/_static/image6.png)](debugging-stored-procedures-vb/_static/image5.png)

**[圖 3**： 請檢查應用程式的屬性頁中的 SQL Server] 核取方塊 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image7.png))


此外，我們需要更新資料庫連接字串，由應用程式使用，因此已停用連線共用。 關閉資料庫的連接時，對應`SqlConnection`物件放在可用的連線集區。 當建立資料庫的連接，可用的連接物件可以擷取此集區中而不必建立，並建立新的連接。 這種共用的連線物件是一項效能增強功能，並依預設會啟用。 不過，當偵錯我們想要關閉連接共用，因為偵錯的基礎結構不正確時重新建立使用取自集區的連線。

若要停用的連線集區，更新`NORTHWNDConnectionString`中`Web.config`使其包含設定`Pooling=false`。


[!code-xml[Main](debugging-stored-procedures-vb/samples/sample1.xml)]

> [!NOTE]
> 一旦您完成透過 ASP.NET 應用程式偵錯 SQL Server 會恢復連線集區移除，請務必`Pooling`從連接字串設定 (或將它設定為`Pooling=true`)。


此時的 ASP.NET 應用程式已設定為允許 Visual Studio 偵錯 web 應用程式透過叫用時，SQL Server 資料庫物件。 現在剩下的是要將中斷點加入至預存程序，並開始偵錯 ！

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>步驟 3： 新增中斷點和偵錯

開啟`Products_SelectByCategoryID`預存程序，並在開頭設定中斷點`SELECT`陳述式的適當位置界中按一下，或將游標放在開頭`SELECT`陳述式，然後按 F9。 圖 4 所示，中斷點會顯示為邊界中的紅色圓圈。


[![Products_SelectByCategoryID 中設定中斷點預存程序](debugging-stored-procedures-vb/_static/image9.png)](debugging-stored-procedures-vb/_static/image8.png)

**圖 4**： 在設定的中斷點`Products_SelectByCategoryID`預存程序 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image10.png))


為了要透過用戶端應用程式進行偵錯的 SQL 資料庫物件，因此務必資料庫設定來支援應用程式偵錯。 當您第一次設定中斷點時，這項設定應該會自動切換，但最好再次檢查。 以滑鼠右鍵按一下`NORTHWND.MDF`伺服器總管 中的節點。 操作功能表應包含已核取的功能表項目命名為應用程式偵錯。


![請確定已啟用 應用程式偵錯選項](debugging-stored-procedures-vb/_static/image11.png)

**圖 5**： 請確定已啟用 應用程式偵錯選項


中斷點設定和啟用的應用程式偵錯選項，我們已準備好進行偵錯時從 ASP.NET 應用程式呼叫預存程序項目。 開始偵錯工具偵錯] 功能表和工具列中選擇開始偵錯，按 f5 鍵，或按一下綠色 [播放圖示。 這會啟動偵錯工具，並啟動網站。

`Products_SelectByCategoryID`中所建立的預存程序[使用現有預存程序的輸入資料集 Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-vb.md)教學課程。 其對應的網頁 (`~/AdvancedDAL/ExistingSprocs.aspx`) 包含的 GridView 會顯示這個預存程序所傳回的結果。 請瀏覽此頁面，透過瀏覽器。 在達到 [] 頁面中的中斷點時`Products_SelectByCategoryID`就會叫用預存程序和控制傳回到 Visual Studio。 就像在步驟 1 中，您可以逐步執行預存程序 s 陳述式和檢視及修改參數值。


[![ExistingSprocs.aspx 頁面一開始會顯示飲料](debugging-stored-procedures-vb/_static/image13.png)](debugging-stored-procedures-vb/_static/image12.png)

**圖 6**:`ExistingSprocs.aspx`頁面一開始會顯示飲料 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image14.png))


[![預存程序 s 已到達中斷點](debugging-stored-procedures-vb/_static/image16.png)](debugging-stored-procedures-vb/_static/image15.png)

**圖 7**： 已到達中斷點的預存程序 s ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image17.png))


[圖 7] 所示，windows 7 中的 [監看式] 視窗為`@CategoryID`參數設為 1。 這是因為`ExistingSprocs.aspx`頁面中的飲料類別目錄，其具有一開始顯示產品`CategoryID`值為 1。 從下拉式清單中選擇不同的類別。 這樣會導致回傳，並重新執行`Products_SelectByCategoryID`預存程序。 同樣地，但這次叫用中斷點`@CategoryID`參數 s 的值會反映出選取的下拉式清單項目的`CategoryID`。


[![從下拉式清單中選擇不同的類別](debugging-stored-procedures-vb/_static/image19.png)](debugging-stored-procedures-vb/_static/image18.png)

**圖 8**： 從下拉式清單中選擇不同的類別 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image20.png))


[![@CategoryID參數會反映從網頁上所選取的類別](debugging-stored-procedures-vb/_static/image22.png)](debugging-stored-procedures-vb/_static/image21.png)

**圖 9**:`@CategoryID`參數會反映之 Web 網頁所選取的類別目錄 ([按一下以檢視完整大小的影像](debugging-stored-procedures-vb/_static/image23.png))


> [!NOTE]
> 如果中的中斷點`Products_SelectByCategoryID`預存程序不叫用瀏覽時，此`ExistingSprocs.aspx`頁面上，請確定 SQL Server 核取方塊已核取的 ASP.NET 應用程式的屬性頁面的 偵錯工具 區段中，已經過連接共用停用日及資料庫 s 應用程式偵錯的選項，會啟用。 如果您仍然遇到問題，重新啟動 Visual Studio，並再試一次。


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>偵錯遠端執行個體上的 T-SQL 的資料庫物件

在 Visual Studio 相同的電腦上的 SQL Server 資料庫執行個體時，偵錯資料庫物件，透過 Visual Studio 就相當簡單。 不過，如果 SQL Server 和 Visual Studio 位於不同的電腦上一些周密的組態設定被需要取得項目的正常運作。 有兩個我們所面臨的核心工作：

- 請確定用來透過 ADO.NET 資料庫連接的登入屬於`sysadmin`角色。
- 請確定 Visual Studio 用來在開發電腦上的 Windows 使用者帳戶是有效的 SQL Server 登入帳戶所屬`sysadmin`角色。

第一個步驟是相當簡單。 首先，識別用來從 ASP.NET 應用程式連接到資料庫，然後，從 SQL Server Management Studio 中，新增該登入帳戶的使用者帳戶`sysadmin`角色。

第二項工作需要您使用偵錯應用程式的 Windows 使用者帳戶是有效的登入遠端資料庫上。 不過，可能是您登入您的工作站使用的 Windows 帳戶不是有效的登入 SQL Server 上。 而加入 SQL Server 中的特定登入帳戶，不是較好的選擇就是將某些 Windows 使用者帳戶指定為 SQL Server 偵錯帳戶。 然後，若要偵錯的遠端 SQL Server 執行個體的資料庫物件，您會執行 Visual Studio 中使用該 Windows 登入帳戶的認證。

範例應該有助於釐清項目。 假設是名為的 Windows 帳戶`SQLDebug`Windows 網域內。 此帳戶必須為有效的登入和成員的身分新增至遠端 SQL Server 執行個體`sysadmin`角色。 然後，若要偵錯 Visual Studio 的遠端 SQL Server 執行個體，我們需要執行 Visual Studio`SQLDebug`使用者。 這可以由身分登入我們的工作站，登出`SQLDebug`，然後啟動 Visual Studio 中，但更簡單的方法會將登入我們的工作站使用我們自己的認證，然後使用`runas.exe`啟動 Visual Studio`SQLDebug`使用者。 `runas.exe` 可讓不同的使用者帳戶之下執行的特定應用程式。 若要啟動 Visual Studio 以`SQLDebug`，您可以輸入下列陳述式，在命令列：


[!code-console[Main](debugging-stored-procedures-vb/samples/sample2.cmd)]

如需此程序的更詳細說明，請參閱 < [William R.vaughn 所著](http://betav.com/BLOG/billva/)s *Hitchhiker Visual Studio 和 SQL Server，第七版指南*以及[How To： 設定 SQL Server 權限偵錯](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx)。

> [!NOTE]
> 如果您的開發電腦正在執行 Windows XP Service Pack 2，您必須設定網際網路連線防火牆以允許遠端偵錯。 [如何以： 啟用 SQL Server 2005 偵錯](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx)發行項資訊這牽涉到兩個步驟: （a） 的 Visual Studio 主機電腦上，您必須新增`Devenv.exe`至例外清單，開啟 TCP 135 通訊埠; 以及 （b） 的遠端 (SQL) 電腦上，您必須開啟TCP 135 通訊埠，並新增`sqlservr.exe`至例外清單。 如果您的網域原則需要透過 IPSec 完成網路通訊，您必須開啟 UDP 4500 與 UDP 500 通訊埠。


## <a name="summary"></a>總結

除了提供.NET 應用程式程式碼偵錯支援，Visual Studio 也提供各種不同的 SQL Server 2005 的偵錯選項。 在本教學課程中我們已經了解這些選項的兩個： 直接資料庫偵錯和應用程式偵錯。 直接偵錯 T-SQL 的資料庫物件，找出物件，可透過 伺服器總管 中按一下滑鼠右鍵，然後選擇 逐步執行。 這會啟動偵錯工具，並在資料庫物件中，此時，您可以逐步執行物件 s 陳述式和檢視及修改參數值的第一個陳述式上暫停。 我們可以在步驟 1 中使用這種方法來逐步執行`Products_SelectByCategoryID`預存程序。

應用程式偵錯，可讓要直接在資料庫物件內設定中斷點。 從用戶端應用程式 （例如 ASP.NET web 應用程式） 叫用中斷點的資料庫物件時，因為偵錯工具會於暫止程式。 應用程式偵錯很有用，因為它會更清楚地顯示應用程式的動作，讓要叫用特定資料庫物件。 不過，它需要更多組態和安裝程式比直接資料庫偵錯。

也可以透過 SQL Server 專案偵錯資料庫物件。 我們會探討使用 SQL Server 專案，以及如何使用它們來建立和偵錯 managed 資料庫物件中下一個教學課程。

快樂地寫程式 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP 書籍和的創辦人[4GuysFromRolla.com](http://www.4guysfromrolla.com)，自 1998 年從事 Microsoft Web 技術工作。 Scott 會擔任獨立的顧問、 培訓講師和作家。 他最新的著作是[ *Sams 教導您自己 ASP.NET 2.0 在 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在觸達[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com) 或透過他的部落格，這位於[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](protecting-connection-strings-and-other-configuration-information-vb.md)
> [下一頁](creating-stored-procedures-and-user-defined-functions-with-managed-code-vb.md)
