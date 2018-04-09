---
uid: web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
title: 偵錯預存程序 (C#) |Microsoft 文件
author: rick-anderson
description: Visual Studio Professional 和 Team System 的版本可讓您設定中斷點，然後逐步執行至 SQL Server 中的預存程序進行偵錯預存...
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/03/2007
ms.topic: article
ms.assetid: c655c324-2ffa-4c21-8265-a254d79a693d
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/data-access/advanced-data-access-scenarios/debugging-stored-procedures-cs
msc.type: authoredcontent
ms.openlocfilehash: 52bb409798dae550c664b78521f0fb4793464833
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="debugging-stored-procedures-c"></a>偵錯預存程序 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/3/9/f/39f92b37-e92e-4ab3-909e-b4ef23d01aa3/ASPNET_Data_Tutorial_74_CS.zip)或[下載 PDF](debugging-stored-procedures-cs/_static/datatutorial74cs1.pdf)

> Visual Studio Professional 和 Team System 的版本可讓您設定中斷點，然後逐步執行至 SQL Server 中的預存程序進行偵錯預存程序，只要偵錯應用程式程式碼。 本教學課程會示範直接資料庫偵錯，以及應用程式偵錯預存程序。


## <a name="introduction"></a>簡介

Visual Studio 提供豐富的偵錯經驗。 有幾個按鍵或按下滑鼠，它可以將中斷點停止執行程式並檢查其狀態和控制流程 s。 偵錯應用程式程式碼，以及 Visual Studio 會提供支援 SQL Server 中的從預存程序，偵錯。 就像可以在 ASP.NET 程式碼後置類別或商務邏輯層類別的程式碼中設定中斷點以便太它們可放置在預存程序內。

在本教學課程，我們將探討逐步執行預存程序從 Visual Studio 中的 [伺服器總管] 以及如何設定中斷點會叫用預存程序呼叫時從執行的 ASP.NET 應用程式。

> [!NOTE]
> 不幸的是，預存程序只能是逐步執行和偵錯透過 Visual Studio 的專業人員和小組的系統版本。 如果您使用 Visual Web Developer 或 standard 版本的 Visual Studio，您可以隨意地讀取沿著我們逐步偵錯預存程序所需的步驟，但您無法將複寫您的電腦上的這些步驟。


## <a name="sql-server-debugging-concepts"></a>SQL Server 偵錯概念

Microsoft SQL Server 2005 設計來提供與整合[Common Language Runtime (CLR)](https://msdn.microsoft.com/netframework/aa497266.aspx)，也就是所有的.NET 組件所使用的執行階段。 因此，SQL Server 2005 支援 managed 的資料庫物件。 也就是說，您可以建立資料庫物件，例如預存程序和使用者定義函數 (Udf) 做為 C# 類別的方法。 這可讓這些預存程序和 Udf 利用.NET Framework 中，以及您自己自訂的類別的功能。 當然，SQL Server 2005 也會提供支援的 T-SQL 和資料庫物件。

SQL Server 2005 提供 T-SQL 和 managed 的資料庫物件偵錯的支援。 不過，這些物件只可透過 Visual Studio 2005 Professional 和小組的系統版本偵錯。 在本教學課程中，我們將檢查偵錯的 T-SQL 和資料庫物件。 後續的教學課程會查看偵錯 managed 的資料庫物件。

[概觀的 T-SQL 和 CLR 偵錯 SQL Server 2005 中](https://blogs.msdn.com/sqlclr/archive/2006/06/29/651644.aspx)部落格文章： 從[SQL Server 2005 CLR 整合小組](https://blogs.msdn.com/sqlclr/default.aspx)會反白顯示偵錯 Visual Studio 中的 SQL Server 2005 物件的三種方式：

- **指示資料庫偵錯 (DDD)** -從伺服器總管，我們可以逐步執行任何 T-SQL 的資料庫物件，例如預存程序和 Udf。 我們將檢驗 DDD 在步驟 1 中。
- **應用程式偵錯**-我們可以將資料庫物件中的中斷點，然後再執行 我們的 ASP.NET 應用程式。 當執行資料庫物件時，會叫用中斷點，控制偵錯工具。 請注意，應用程式偵錯我們無法逐步執行的資料庫物件從應用程式程式碼。 我們必須明確地設定中斷點這些預存程序或 Udf 我們想要停止偵錯工具。 偵錯應用程式會檢查在步驟 2 中啟動。
- **從 SQL Server 專案，偵錯**-Visual Studio Professional 和小組的系統版本包括通常用來建立 managed 的資料庫物件的 SQL Server 專案類型。 我們將使用 SQL Server 專案來檢視和下一個教學課程中的偵錯其內容。

Visual Studio 可以偵錯在本機和遠端的 SQL Server 執行個體上的預存程序。 本機的 SQL Server 執行個體是安裝在與 Visual Studio 在同一部電腦上。 如果在開發電腦上找不到您使用的 SQL Server 資料庫，則它會被視為遠端執行個體。 如需這些教學課程我們已在使用本機 SQL Server 執行個體。 偵錯在遠端的 SQL server 執行個體上的預存程序需要更多的組態步驟，比當偵錯在本機執行個體上的預存程序。

如果您使用本機的 SQL Server 執行個體，您可以從步驟 1 開始，逐步進行本教學課程結尾。 如果您使用遠端 SQL Server 執行個體，不過，您將必須先確定當您偵錯會記錄到遠端執行個體上有 SQL Server 登入的 Windows 使用者帳戶在開發電腦。 Moveover，此資料庫登入，用於從執行的 ASP.NET 應用程式連接到資料庫的資料庫登入必須是成員的`sysadmin`角色。 如需有關設定 Visual Studio 和 SQL Server 遠端執行個體進行偵錯本教學課程結尾處中查看上遠端執行個體 > 一節 T-SQL 偵錯資料庫物件。

最後，了解偵錯支援 T-SQL 的資料庫物件不為做為偵錯的.NET 應用程式支援豐富的功能。 例如，中斷點條件和篩選條件不支援，偵錯視窗的子集只可以使用，您無法使用 [編輯後繼續]、 即時運算視窗呈現毫無用處，等等。 請參閱[偵錯工具命令和功能的限制](https://msdn.microsoft.com/library/ms165035(VS.80).aspx)如需詳細資訊。

## <a name="step-1-directly-stepping-into-a-stored-procedure"></a>步驟 1： 直接逐步執行預存程序

Visual Studio 輕鬆地直接偵錯資料庫物件。 將 s 看看如何使用直接資料庫偵錯 (DDD) 功能來逐步執行`Products_SelectByCategoryID`Northwind 資料庫中預存程序。 正如其名，`Products_SelectByCategoryID`傳回產品資訊的特定某類; 中建立[使用現有預存程序型別資料集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程。 首先移至 [伺服器總管]，然後展開 Northwind 資料庫節點。 接下來，向下鑽研至預存程序資料夾、 以滑鼠右鍵按一下`Products_SelectByCategoryID`預存程序，並從內容功能表中選擇 逐步執行預存程序選項。 這會啟動偵錯工具。

因為`Products_SelectByCategoryID`預存程序必須有`@CategoryID`輸入的參數，我們會要求您提供此值。 輸入 1，將會傳回飲料的相關資訊。


![使用的值 1@CategoryID參數](debugging-stored-procedures-cs/_static/image1.png)

**圖 1**： 使用的值 1`@CategoryID`參數


在提供的值之後`@CategoryID`參數，預存程序時執行。 不過，而不是執行到完成為止，偵錯工具暫停執行的第一個陳述式。 請注意界黃色箭頭，指出預存程序中的目前位置。 您可以檢視和編輯參數值透過監看式視窗或將滑鼠停留在預存程序的參數名稱。


[![偵錯工具已停止執行的預存程序的第一個陳述式上](debugging-stored-procedures-cs/_static/image3.png)](debugging-stored-procedures-cs/_static/image2.png)

**圖 2**： 偵錯工具已停止執行預存程序的第一個陳述式 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image4.png))


若要逐步執行預存程序一個陳述式一次，請按一下工具列中的 [不進入函數] 按鈕或按 F10 鍵。 `Products_SelectByCategoryID`預存程序包含單一`SELECT`陳述式，因此按下 F10 會不進入單一陳述式，並完成執行預存程序。 預存程序完成之後，它的輸出會出現在 [輸出] 視窗和偵錯工具將會終止。

> [!NOTE]
> T-SQL 偵錯，就會發生在陳述式層級;您無法逐步執行`SELECT`陳述式。


## <a name="step-2-configuring-the-website-for-application-debugging"></a>步驟 2： 設定應用程式偵錯的網站

偵錯直接從 [伺服器總管] 中的預存程序是很方便，在許多情況下很多想要呼叫從我們的 ASP.NET 應用程式時，偵錯預存程序。 我們可以將中斷點加入至 Visual Studio 中的預存程序，然後開始偵錯 ASP.NET 應用程式。 從應用程式叫用具有中斷點的預存程序時，執行將會在中斷點暫止，我們可以檢視和變更預存程序的參數值並逐步執行陳述式中，，如同我們在步驟 1。

我們可以開始偵錯應用程式從呼叫的預存程序之前，我們需要 ASP.NET web 應用程式與 SQL Server 偵錯工具整合的指示。 以滑鼠右鍵按一下方案總管 中的網站名稱開始 (`ASPNET_Data_Tutorial_74_CS`)。 從內容功能表中選擇 [屬性頁] 選項選取左邊的啟動選項項目，請檢查 SQL Server 中的核取 [偵錯工具] 區段 （請參閱圖 3）。


[![檢查應用程式的屬性頁面中的 SQL Server 核取方塊](debugging-stored-procedures-cs/_static/image6.png)](debugging-stored-procedures-cs/_static/image5.png)

**圖 3**： 選取 SQL 伺服器核取方塊，在應用程式屬性頁 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image7.png))


此外，我們需要更新資料庫連接字串，由應用程式使用，因此已停用連線共用。 資料庫的連接關閉時，對應`SqlConnection`物件會放置在可用的連接集區。 在建立資料庫的連接時，可用的連接物件可以擷取此集區中而不需要重新建立，並建立新的連接。 這種共用的連線物件是效能增強功能，並預設會啟用。 不過，當偵錯我們想要關閉連接共用，因為偵錯基礎結構會不正確地重新建立時使用取自集區的連接。

若要停用的連線集區，更新`NORTHWNDConnectionString`中`Web.config`，使其包含設定`Pooling=false`。


[!code-xml[Main](debugging-stored-procedures-cs/samples/sample1.xml)]

> [!NOTE]
> 當您完成後，透過 ASP.NET 應用程式偵錯 SQL Server 務必要重新啟用連接共用，藉由移除`Pooling`從連接字串設定 (或將它設定為`Pooling=true`)。


此時 ASP.NET 應用程式已設定為允許 Visual Studio 偵錯 web 應用程式透過叫用時，SQL Server 資料庫物件。 現在只剩是將中斷點加入至預存程序，並開始偵錯 ！

## <a name="step-3-adding-a-breakpoint-and-debugging"></a>步驟 3： 新增中斷點和偵錯

開啟`Products_SelectByCategoryID`預存程序，並設定中斷點的開頭`SELECT`陳述式的適當位置界中按一下，或將游標放在開頭`SELECT`陳述式，並按 f9 鍵。 如圖 4 所示，中斷點會顯示為邊界中的紅色圓圈。


[![Products_SelectByCategoryID 中設定中斷點預存程序](debugging-stored-procedures-cs/_static/image9.png)](debugging-stored-procedures-cs/_static/image8.png)

**圖 4**： 中設定中斷點`Products_SelectByCategoryID`預存程序 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image10.png))


為了要透過用戶端應用程式進行偵錯 SQL 資料庫物件，請務必為設定資料庫，以支援應用程式偵錯。 當您第一次設定中斷點時，這項設定應該自動電源已開啟，但請仔細檢查審慎的作法是。 以滑鼠右鍵按一下`NORTHWND.MDF`伺服器總管中的節點。 操作功能表應該包含核取的功能表項目命名為應用程式偵錯。


![請確定已啟用應用程式偵錯選項](debugging-stored-procedures-cs/_static/image11.png)

**圖 5**： 請確定已啟用應用程式偵錯選項


中斷點設定和已啟用的應用程式偵錯選項，我們就可以開始偵錯預存程序從 ASP.NET 應用程式呼叫時項目。 啟動偵錯工具移至 [偵錯] 功能表並選擇開始偵錯，請按 F5，或按一下綠色播放工具列中的圖示。 這將會啟動偵錯工具，並啟動網站。

`Products_SelectByCategoryID`預存程序中建立[使用現有預存程序型別資料集 s Tableadapter](using-existing-stored-procedures-for-the-typed-dataset-s-tableadapters-cs.md)教學課程。 其對應的網頁 (`~/AdvancedDAL/ExistingSprocs.aspx`) 包含的 GridView 會顯示此預存程序所傳回的結果。 請瀏覽此頁面，透過瀏覽器。 當到達 頁面上，在中斷點`Products_SelectByCategoryID`會被叫用預存程序和控制傳回到 Visual Studio。 就像在步驟 1 中，您可以逐步執行的預存程序 s 陳述式和檢視和修改參數值。


[![ExistingSprocs.aspx 頁面一開始會顯示飲料](debugging-stored-procedures-cs/_static/image13.png)](debugging-stored-procedures-cs/_static/image12.png)

**圖 6**:`ExistingSprocs.aspx`頁面一開始會顯示飲料 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image14.png))


[![預存程序 s 已到達中斷點](debugging-stored-procedures-cs/_static/image16.png)](debugging-stored-procedures-cs/_static/image15.png)

**圖 7**： 已到達中斷點的預存程序 s ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image17.png))


圖 7 所示的值中的 [監看式] 視窗`@CategoryID`參數是 1。 這是因為`ExistingSprocs.aspx`頁面一開始會顯示產品中具有 「 飲料 」 分類`CategoryID`值為 1。 從下拉式清單中選擇不同的類別。 這樣會導致回傳，並重新執行`Products_SelectByCategoryID`預存程序。 叫用中斷點時一次，但這次`@CategoryID`參數 s 的值會反映下拉式清單中選取項目的`CategoryID`。


[![從下拉式清單中選擇不同的類別](debugging-stored-procedures-cs/_static/image19.png)](debugging-stored-procedures-cs/_static/image18.png)

**圖 8**： 從下拉式清單中選擇不同的分類 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image20.png))


[![@CategoryID參數會反映從網頁上所選取的類別](debugging-stored-procedures-cs/_static/image22.png)](debugging-stored-procedures-cs/_static/image21.png)

**圖 9**:`@CategoryID`參數會反映從網頁上的類別目錄選取 ([按一下以檢視完整大小的影像](debugging-stored-procedures-cs/_static/image23.png))


> [!NOTE]
> 如果在中斷點`Products_SelectByCategoryID`預存程序不叫用造訪時`ExistingSprocs.aspx`頁面上，請確定 SQL Server 核取方塊已核取 ASP.NET 應用程式的屬性頁面 偵錯工具 部分中，已經過連接共用已停用，而且已啟用資料庫 s 應用程式偵錯選項。 如果您重新仍然發生問題，重新啟動 Visual Studio 並再試一次。


## <a name="debugging-t-sql-database-objects-on-remote-instances"></a>偵錯遠端執行個體上的 T-SQL 和資料庫物件

在 Visual Studio 的同一部機器上的 SQL Server 資料庫執行個體時，偵錯資料庫物件，透過 Visual Studio 就相當簡單。 不過，如果 SQL Server 和 Visual Studio 位於不同機器上某些周密的組態設定則需要取得項目的正常運作。 有兩個核心工作，我們正面臨著：

- 確認用來透過 ADO.NET 資料庫連接的登入屬於`sysadmin`角色。
- 請確定 Visual Studio 用來開發電腦上的 Windows 使用者帳戶是有效的 SQL Server 登入帳戶屬於`sysadmin`角色。

第一個步驟是相當直接。 首先，識別用來從 ASP.NET 應用程式連接到資料庫，然後，從 SQL Server Management Studio，加入至該登入帳戶的使用者帳戶`sysadmin`角色。

第二項工作需要您使用偵錯應用程式的 Windows 使用者帳戶是有效的登入遠端資料庫上。 但是，機率是您登入您的工作站使用的 Windows 帳戶不是有效的登入 SQL Server 上。 而不是將您的特定登入帳戶加入至 SQL Server，就是比較好的選擇將某些 Windows 使用者帳戶指定為 SQL Server 偵錯帳戶。 然後，若要偵錯的遠端 SQL Server 執行個體的資料庫物件，您會執行 Visual Studio 中使用該 Windows 登入帳戶的認證。

範例應該有助於釐清項目。 假設是名為的 Windows 帳戶`SQLDebug`Windows 網域內。 此帳戶會想要新增至遠端 SQL Server 執行個體做為有效的登入，且成員的身分`sysadmin`角色。 然後偵錯遠端 SQL Server 執行個體，從 Visual Studio，我們將需要執行 Visual Studio，做為`SQLDebug`使用者。 無法做到這超出我們工作站上，記錄中記錄為`SQLDebug`，，然後啟動 Visual Studio 中，但更簡單的方法會是我們工作站使用自己的認證來登入，然後使用`runas.exe`啟動 Visual Studio，做為`SQLDebug`使用者。 `runas.exe` 可讓執行不同的使用者帳戶冒充特定應用程式。 若要啟動 Visual Studio，做為`SQLDebug`，您可以輸入下列陳述式，在命令列：


[!code-console[Main](debugging-stored-procedures-cs/samples/sample2.cmd)]

如需這個程序的更詳細說明，請參閱[William R Vaughn](http://betav.com/BLOG/billva/) s *（英文) s Visual Studio 和 SQL Server，第七個 Edition 指引*以及[How To： 設定 SQL Server 權限偵錯](https://msdn.microsoft.com/library/w1bhybwz(VS.80).aspx)。

> [!NOTE]
> 如果您在開發電腦正在執行 Windows XP Service Pack 2，您必須設定網際網路連線防火牆，允許遠端偵錯。 [如何以： 啟用 SQL Server 2005 偵錯](https://msdn.microsoft.com/library/s0fk6z6e(VS.80).aspx)文章附註這牽涉到兩個步驟: （a） 在 Visual Studio 主機電腦上，您必須加入`Devenv.exe`例外狀況清單並開啟 TCP 135 通訊埠，且 （b） 上遠端 (SQL) 電腦中，您必須開啟TCP 135 通訊埠及新增`sqlservr.exe`至例外清單。 如果您的網域原則要求透過 IPSec 完成網路通訊，您必須開啟 UDP 4500 和 UDP 500 的連接埠。


## <a name="summary"></a>總結

除了提供.NET 應用程式程式碼偵錯支援，Visual Studio 也會提供各種不同的 SQL Server 2005 的偵錯選項。 本教學課程中我們探討兩種： 直接資料庫偵錯和應用程式偵錯。 直接偵錯的 T-SQL 和資料庫物件，找出透過 伺服器總管 中的物件，然後在其上以滑鼠右鍵按一下並選擇 逐步執行。 這樣會啟動偵錯工具，並在資料庫物件，此時，您可以逐步執行的物件 s 陳述式與檢視和修改參數值的第一個陳述式時，中止。 在步驟 1 中使用這種方法來逐步執行`Products_SelectByCategoryID`預存程序。

應用程式偵錯允許直接在資料庫物件內設定中斷點。 從用戶端應用程式 （例如 ASP.NET web 應用程式） 叫用中斷點的資料庫物件時，程式會中止時偵錯工具會接管。 應用程式偵錯是很有用，因為它會更清楚地顯示應用程式的動作會導致叫用特定資料庫物件。 不過，它需要更多的組態與比直接資料庫偵錯的設定。

也可透過 SQL Server 專案偵錯資料庫物件。 我們將探討使用 SQL Server 專案，以及如何使用它們來建立及偵錯 managed 資料庫物件中下一個教學課程。

祝您程式設計 ！

## <a name="about-the-author"></a>關於作者

[Scott Mitchell](http://www.4guysfromrolla.com/ScottMitchell.shtml)，作者的七個 ASP/ASP.NET 書籍和的創辦[4GuysFromRolla.com](http://www.4guysfromrolla.com)，已從 1998 年使用 Microsoft Web 技術。 Scott 可做為獨立顧問、 訓練和寫入器。 他最新的活頁簿[ *Sam 教導您自己 ASP.NET 2.0 24 小時內*](https://www.amazon.com/exec/obidos/ASIN/0672327384/4guysfromrollaco)。 他可以在達到[ mitchell@4GuysFromRolla.com。](mailto:mitchell@4GuysFromRolla.com)或透過他的部落格，這可以在找到[ http://ScottOnWriting.NET ](http://ScottOnWriting.NET)。

> [!div class="step-by-step"]
> [上一頁](protecting-connection-strings-and-other-configuration-information-cs.md)
> [下一頁](creating-stored-procedures-and-user-defined-functions-with-managed-code-cs.md)
