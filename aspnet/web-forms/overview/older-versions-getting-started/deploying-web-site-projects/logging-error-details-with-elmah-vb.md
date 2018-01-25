---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
title: "記錄錯誤的詳細資料與 ELMAH (VB) |Microsoft 文件"
author: rick-anderson
description: "錯誤記錄模組和處理常式 (ELMAH) 提供了另一個執行階段錯誤記錄在生產環境中的方法。 ELMAH 是免費的開放原始錯誤..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: a5f0439f-18b2-4c89-96ab-75b02c616f46
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-vb
msc.type: authoredcontent
ms.openlocfilehash: 41e1f8673b42571a9dcbdae668a30426fe90f42f
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="logging-error-details-with-elmah-vb"></a>記錄錯誤的詳細資料與 ELMAH (VB)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_VB.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_vb.pdf)

> 錯誤記錄模組和處理常式 (ELMAH) 提供了另一個執行階段錯誤記錄在生產環境中的方法。 ELMAH 是免費的開放原始錯誤記錄程式庫所包含的功能類似錯誤篩選和 RSS 摘要，以檢視錯誤記錄檔從網頁上，或下載為逗號分隔檔案的能力。 這個教學課程引導下載和設定 ELMAH。


## <a name="introduction"></a>簡介

[前述教學課程](logging-error-details-with-asp-net-health-monitoring-vb.md)檢查 ASP。網路的健全狀況監視系統，可提供現成的方塊程式庫錄製的 Web 事件。 許多開發人員使用健全狀況監視記錄和電子郵件的未處理例外狀況詳細資料。 不過，有幾個痛苦點與此系統。 第一次和最重要 」 表示無任何使用者介面，以檢視所記錄的事件的相關任何的資訊排序。 如果您想要查看摘要，以 10 的最後一個錯誤，或檢視上週發生了錯誤的詳細資料，您必須是透過資料庫查詢、 電子郵件收件匣，透過掃描或建立顯示資訊的 web 網頁`aspnet_WebEvent_Events`資料表。

另一個痛苦點中心健全狀況監視的複雜性。 可以用來記錄上的不同事件的健全狀況監視，因為有許多不同的選項來指示如何及何時會記錄事件，正確設定監視系統健全狀況是很繁重的工作。 最後，有相容性問題。 健全狀況監視已初次加入至.NET Framework 2.0 版中，因為它不適用於較舊的 web 應用程式使用 ASP.NET 版本建置的 1.x。 此外，`SqlWebEventProvider`類別，可使用資料庫的記錄檔錯誤詳細資料前述教學課程中，只適用於 Microsoft SQL Server 資料庫。 您必須建立自訂記錄提供者類別，您應該要替代的資料存放區，例如 XML 檔案或 Oracle 資料庫中記錄錯誤。

監視系統健全狀況的替代方式是錯誤記錄模組和處理常式 (ELMAH)，建立免費的開放原始碼的錯誤記錄系統[Atif Aziz](http://www.raboof.com/)。 兩個系統之間最顯著的差異是 ELAMH 的能夠顯示的錯誤，以及從網頁和特定錯誤的詳細資料 RSS 摘要清單。 ELMAH 很容易設定比健全狀況監視，因為它只會記錄錯誤。 此外，ELMAH 包含支援 ASP.NET 1.x、 ASP.NET 2.0 中，與 ASP.NET 3.5 應用程式，以及隨附於各種不同的記錄檔提供者。

本教學課程逐步解說中將 ELMAH 加入至 ASP.NET 應用程式所需的步驟。 讓我們開始吧 ！

> [!NOTE]
> 健全狀況監視系統和 ELMAH 兩者都有它們自己集合的優缺點。 建議您將再試一次這兩個系統，並決定哪一個最符合您的需求。


## <a name="adding-elmah-to-an-aspnet-web-application"></a>加入 ASP.NET Web 應用程式中的 ELMAH

整合新的或現有的 ASP.NET 應用程式中的 ELMAH 是簡單且直接的程序，需要在五分鐘的時間。 簡單地說，它牽涉到四個簡單步驟：

1. 下載 ELMAH 並加入`Elmah.dll`web 應用程式的組件
2. 註冊 ELMAH 的 HTTP 模組和處理常式中的`Web.config`，
3. 指定 ELMAH 的組態選項，並
4. 若有需要請建立錯誤記錄檔來源基礎結構。

讓我們逐步解說每一次下列四個步驟。

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>步驟 1： 下載 ELMAH 專案檔案，並加入`Elmah.dll`您 Web 應用程式

下載適用於本教學課程中隨附 ELMAH 1.0 BETA 3 (建置 10617)，在撰寫時，時間的最新版本。 或者，您可能瀏覽[ELMAH 網站](https://code.google.com/p/elmah/)取得最新版本，或下載的原始程式碼。 擷取 ELMAH 下載到您的桌面上的資料夾，並找出 ELMAH 組件檔案 (`Elmah.dll`)。

> [!NOTE]
> `Elmah.dll`檔案位於下載`Bin`有不同的.NET Framework 版本和發行和偵錯組建的子資料夾的資料夾。 使用 「 版本 」 組建之適當的 framework 版本。 比方說，如果您要建置的 ASP.NET 3.5 web 應用程式，將複製`Elmah.dll`檔案從`Bin\net-3.5\Release`資料夾。


接下來，開啟 Visual Studio，並將組件加入至專案，以滑鼠右鍵按一下方案總管 中選擇加入的參考，從內容功能表的網站名稱。 這會開啟 [加入參考] 對話方塊。 瀏覽至 瀏覽 索引標籤，然後選擇 `Elmah.dll`檔案。 這個動作會將`Elmah.dll`web 應用程式檔案`Bin`資料夾。

> [!NOTE]
> Web 應用程式專案 (WAP) 型別不會顯示`Bin`在 [方案總管] 中的資料夾。 相反地，它會列出這些項目參考 資料夾下。


`Elmah.dll`該組件包含 ELMAH 系統所使用的類別。 這些類別可分為三種類別：

- **HTTP 模組**-HTTP 模組是一個類別來定義事件處理常式`HttpApplication`事件，例如`Error`事件。 ELMAH 包含多個 HTTP 模組，三個最密切關聯的項目正在： 

    - `ErrorLogModule`-記錄到記錄檔來源的未處理例外狀況。
    - `ErrorMailModule`-傳送電子郵件訊息的未處理的例外狀況詳細資料。
    - `ErrorFilterModule`-適用於開發人員指定的篩選來判斷哪些例外狀況的記錄，以及是會被忽略。
- **HTTP 處理常式**-HTTP 處理常式是負責產生要求的特定類型的標記的類別。 ELMAH 包括網頁上、 RSS 摘要，或以逗號分隔的檔案 (CSV) 轉譯錯誤的詳細資料的 HTTP 處理常式。
- **錯誤記錄檔來源**-ELMAH 可以將 Microsoft SQL Server 資料庫，到 Microsoft Access 資料庫，Oracle 資料庫，要的記憶體，錯誤記錄到現成 SQLite 資料庫中，或 Vista DB 資料庫的 XML 檔案。 監視系統健全狀況，例如 ELMAH 的架構是使用提供者模型，這表示您可以用來建立，並密切整合您自己自訂的記錄檔來源提供者中，視所建立的。

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>步驟 2： 註冊 ELMAH 的 HTTP 模組和處理常式

雖然`Elmah.dll`檔包含 HTTP 模組和處理常式需要自動記錄未處理的例外狀況，並顯示從網頁上的錯誤詳細資料，這些必須先明確註冊 web 應用程式的組態中。 `ErrorLogModule` HTTP 模組，一旦註冊之後，訂閱`HttpApplication`的`Error`事件。 每當會引發這個事件`ErrorLogModule`例外狀況的詳細資訊記錄到指定的記錄檔來源。 我們會看到如何在下一節中定義的記錄檔來源提供者 」"設定 ELMAH。 `ErrorLogPageFactory` HTTP 處理常式 factory 會負責檢視錯誤記錄檔從網頁時，產生標記。

HTTP 模組和處理常式註冊的特定語法取決於正在強化功能的站台的網頁伺服器。 ASP.NET 程式開發伺服器和 Microsoft 的 iis 6.0 或更早版本，HTTP 模組和處理常式會在註冊`<httpModules>`和`<httpHandlers>`區段出現在`<system.web>`項目。 如果您使用的 IIS 7.0，則他們需要登記在`<system.webServer>`項目的`<modules>`和`<handlers>`區段。 幸運的是，您可以在其中定義的 HTTP 模組和處理常式中的*兩者*放置不論所使用的 web 伺服器。 這個選項就是大部分可攜式因為它可讓相同的設定，以用於開發和生產環境，不論所使用的 web 伺服器。

註冊啟動`ErrorLogModule`HTTP 模組和`ErrorLogPageFactory`HTTP 處理常式中`<httpModules>`和`<httpHandlers>`一節中`<system.web>`。 如果您的組態已定義這兩個元素然後只包含`<add>`ELMAH 的 HTTP 模組和處理常式項目。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample1.xml)]

接下來，註冊 ELMAH 的 HTTP 模組和處理常式中的`<system.webServer>`項目。 如往常一般，如有此項目已不存在於您的組態加入它。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample2.xml)]

根據預設，IIS 7 抱怨 HTTP 模組和處理常式會在註冊`<system.web>`> 一節。 `validateIntegratedModeConfiguration`屬性`<validation>`項目指示 IIS 7，以隱藏這類的錯誤訊息。

請注意，註冊語法`ErrorLogPageFactory`HTTP 處理常式包括`path`屬性設定為`elmah.axd`。 這個屬性會通知 web 應用程式，如果要求到達名為的頁面`elmah.axd`則要求應該由`ErrorLogPageFactory`HTTP 處理常式。 我們會看到`ErrorLogPageFactory`動作稍後在本教學課程中的 HTTP 處理常式。

### <a name="step-3-configuring-elmah"></a>步驟 3： 設定 ELMAH

會尋找其網站中的組態選項的 ELMAH`Web.config`名為的自訂組態區段中的檔案`<elmah>`。 若要使用的自訂區段`Web.config`必須先將定義在`<configSections>`項目。 開啟`Web.config`檔案，然後加入下列標記以`<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample3.xml)]

> [!NOTE]
> 如果您要設定 ELMAH ASP.NET 1.x 應用程式，則移除`requirePermission="false"`屬性從`<section>`上述項目。


上述語法註冊自訂`<elmah>`區段，其各小節： `<security>`， `<errorLog>`， `<errorMail>`，和`<errorFilter>`。

接下來，加入`<elmah>`區段`Web.config`。 本章節應該出現在相同的層級`<system.web>`項目。 內部`<elmah>`> 一節新增`<security>`和`<errorLog>`區段如下所示：

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample4.xml)]

`<security>`區段的`allowRemoteAccess`屬性會指出是否允許遠端存取。 如果此值設定為 0，然後錯誤記錄檔網頁只能檢視在本機。 如果這個屬性設定為 1 錯誤記錄檔的 web 網頁會啟用遠端和本機訪客。 現在，讓我們來停用錯誤記錄檔網頁遠端訪客。 稍後我們有機會討論這樣做的安全性考量之後，我們會允許遠端存取。

`<errorLog>`區段會定義錯誤記錄檔來源，其中會記錄錯誤的詳細資料，類似於哪一個指定`<providers>`監視系統健全狀況 > 一節。 上述語法指定`SqlErrorLog`類別做為將錯誤記錄到 Microsoft SQL Server 資料庫所指定錯誤記錄檔來源`connectionStringName`屬性值。

> [!NOTE]
> ELMAH 隨附其他錯誤記錄檔提供者可將錯誤記錄到 XML 檔案、 Microsoft Access 資料庫、 Oracle 資料庫和其他資料存放區。 請參閱範例`Web.config`會包含有關如何使用這些替代錯誤記錄檔提供者的 ELMAH 下載的檔案。


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>步驟 4： 建立錯誤記錄檔來源基礎結構

ELMAH 的`SqlErrorLog`提供者記錄到指定的 Microsoft SQL Server 資料庫的錯誤詳細資料。 `SqlErrorLog`提供者必須要有這個資料庫有一個名為資料表`ELMAH_Error`和三個預存程序： `ELMAH_GetErrorsXml`， `ELMAH_GetErrorXml`，和`ELMAH_LogError`。 ELMAH 下載包含名為`SQLServer.sql`中`db`資料夾，其中包含用於建立這個資料表，而且這些 T-SQL 預存程序。 您必須在您要使用的資料庫上執行這些陳述式`SqlErrorLog`提供者。

**數字 1**和**2** Visual Studio 中的 [資料庫總管] 中顯示所需的資料庫物件之後`SqlErrorLog`已加入提供者。

[![](logging-error-details-with-elmah-vb/_static/image2.png)](logging-error-details-with-elmah-vb/_static/image1.png)

**圖 1**:`SqlErrorLog`提供者會記錄錯誤`ELMAH_Error`資料表

[![](logging-error-details-with-elmah-vb/_static/image4.png)](logging-error-details-with-elmah-vb/_static/image3.png)

**圖 2**:`SqlErrorLog`提供者會使用三個預存程序

## <a name="elmah-in-action"></a>在動作中的 ELMAH

此時我們新增了 ELMAH web 應用程式、 註冊`ErrorLogModule`HTTP 模組和`ErrorLogPageFactory`HTTP 處理常式中，指定中的 ELMAH 的設定選項`Web.config`，並加入所需的資料庫物件`SqlErrorLog`錯誤記錄檔提供者。 我們現在已準備好在動作中看到 ELMAH ！ 瀏覽書籍檢閱網站，瀏覽的頁面，會產生執行階段錯誤，例如`Genre.aspx?ID=foo`，或不存在的頁面，例如`NoSuchPage.aspx`。 瀏覽這些網頁時看到的內容取決於`<customErrors>`組態和您所造訪在本機或遠端。 (會回頭[*顯示自訂錯誤網頁*教學課程](displaying-a-custom-error-page-vb.md)如本主題的重新整理程式。)

ELMAH 並不會影響哪些內容顯示給使用者時就會發生未處理的例外狀況。它只會記錄其詳細資料。 此錯誤記錄檔是可從網站上`elmah.axd`根目錄中的網站，例如`http://localhost/BookReviews/elmah.axd`。 (這個檔案不實際存在您的專案，但當要求進入的`elmah.axd`執行階段會將它分派至`ErrorLogPageFactory`HTTP 處理常式，會產生傳回給瀏覽器的標記。)

> [!NOTE]
> 您也可以使用`elmah.axd`頁面，即可指示 ELMAH 產生測試錯誤。 瀏覽`elmah.axd/test`(像是`http://localhost/BookReviews/elmah.axd/test`) 會導致擲回例外狀況類型的 ELMAH `Elmah.TestException`，後者有錯誤訊息: 「 這個 」 可以放心略過的測試例外狀況。


**圖 3**瀏覽時顯示錯誤記錄檔`elmah.axd`從開發環境。

[![](logging-error-details-with-elmah-vb/_static/image6.png)](logging-error-details-with-elmah-vb/_static/image5.png)

**圖 3**:`Elmah.axd`從網頁上顯示的錯誤  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image7.png))

錯誤記錄檔**圖 3**包含六個錯誤項目。 每個項目包括 HTTP 狀態碼 （404 或 500，這些錯誤）、 類型、 描述、 發生錯誤時，登入的使用者名稱和的日期和時間。 按一下 [詳細資料] 連結會顯示一個網頁，內含錯誤詳細資料黃色螢幕的死所示相同的錯誤訊息 (請參閱**圖 4**) 以及在發生錯誤時的伺服器變數的值 (請參閱**圖 5**)。 您也可以檢視在其中儲存錯誤詳細資料，其中包含 HTTP POST 標頭中的其他資訊，例如值的原始 XML。

[![](logging-error-details-with-elmah-vb/_static/image9.png)](logging-error-details-with-elmah-vb/_static/image8.png)

**圖 4**： 檢視錯誤詳細資料 YSOD  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image10.png))

[![](logging-error-details-with-elmah-vb/_static/image12.png)](logging-error-details-with-elmah-vb/_static/image11.png)

**圖 5**： 瀏覽伺服器變數集合，在發生錯誤時的值  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image13.png))

部署至生產環境網站的 ELMAH 牽涉到：

- 複製`Elmah.dll`檔案`Bin`上生產環境中，資料夾
- 複製 ELMAH 特定組態設定來`Web.config`用在生產環境中，檔案和
- 將錯誤記錄檔來源基礎結構加入至實際執行資料庫。

我們已瀏覽檔案複製到生產環境中先前的教學課程開發的技術。 可能是在生產資料庫上取得錯誤記錄檔來源基礎結構最簡單方式是使用 SQL Server Management Studio 連接到實際執行資料庫，然後執行`SqlServer.sql`指令碼檔案，它會建立所需的資料表，並儲存程序。

### <a name="viewing-the-error-details-page-on-production"></a>在生產環境中檢視錯誤詳細資料頁面

之後將您的網站部署至生產環境，請前往生產網站並產生未處理的例外狀況。 開發環境中，如同 ELMAH 會有任何作用時，發生未處理的例外狀況，顯示錯誤頁面相反地，則只會記錄錯誤。 如果您嘗試瀏覽錯誤記錄頁面 (`elmah.axd`) 從生產環境中，您將氣急敗壞 [禁止] 頁面中顯示與**圖 6**。

[![](logging-error-details-with-elmah-vb/_static/image15.png)](logging-error-details-with-elmah-vb/_static/image14.png)

**圖 6**： 根據預設，遠端的訪客無法檢視錯誤記錄檔的 Web 網頁  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image16.png))

請記得，ELMAH 組態中`<security>`> 一節中，我們設定`allowRemoteAccess`屬性設定為 0，檢視錯誤記錄檔時，防止遠端使用者。 請務必禁止匿名訪客檢視錯誤記錄檔，因為安全性弱點或其他機密資訊，可能會顯示錯誤詳細資料。 如果您決定要將這個屬性設定為 1，並啟用遠端存取錯誤記錄檔，則務必要鎖定`elmah.axd`路徑，因此只有授權的訪客具有存取權限。 這可藉由新增`<location>`元素`Web.config`檔案。

下列組態會允許系統管理員角色，來存取錯誤記錄檔網頁中只有使用者：

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample5.xml)]

> [!NOTE]
> 中已加入系統管理員角色和三個使用者在系統中，Scott、 Jisun 和 Alice- [*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-vb.md)。 使用者 Scott 和 Jisun 是 「 管理員 」 角色的成員。 如需有關驗證和授權的詳細資訊，請參閱我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


遠端使用者; 現在可以檢視錯誤記錄檔，在實際執行環境回頭參考**數字 3**， **4**，和**5**的錯誤記錄檔網頁的螢幕擷取畫面。 不過，如果匿名或非系統管理員的使用者嘗試檢視 [錯誤記錄] 頁面會會自動重新導向至登入頁面 (`Login.aspx`)，做為**圖 7**顯示。

[![](logging-error-details-with-elmah-vb/_static/image18.png)](logging-error-details-with-elmah-vb/_static/image17.png)

**圖 7**： 未經授權的使用者會自動重新導向至登入頁面  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image19.png))

### <a name="programmatically-logging-errors"></a>以程式設計方式記錄錯誤

ELMAH 的`ErrorLogModule`HTTP 模組自動記錄未處理例外狀況指定的記錄檔來源。 或者，您可以記錄錯誤，而不必使用引發未處理例外狀況`ErrorSignal`類別和其`Raise`方法。 `Raise`方法傳遞`Exception`物件，並已擲回該例外狀況，並已達 ASP.NET 執行階段，而不處理加以記錄。 差異，不過，會要求會繼續執行正常之後`Raise`已呼叫方法，而擲回、 未處理的例外狀況中斷要求的正常執行，並讓 ASP.NET 執行階段顯示設定錯誤頁面。

`ErrorSignal`類別會在沒有可能會失敗，某些動作，但其失敗不是重大的整體的作業正在執行的情況下很有用。 比方說，網站可能包含一個表單，會接受使用者輸入，將它儲存在資料庫中，然後傳送電子郵件通知他們使用者他們已處理的資訊。 如果成功，資訊儲存到資料庫，但卻發生錯誤時傳送電子郵件訊息，就應該發生什麼情況？ 其中一個選項，可擲回例外狀況，並傳送給使用者，對錯誤頁面。 不過，這可能會混淆使用者到他們所輸入的資訊未儲存的想法。 另一種方法就是記錄電子郵件相關的錯誤，但不是能更改任何方式中的使用者經驗。 這是 where`ErrorSignal`類別就很有用。

[!code-vb[Main](logging-error-details-with-elmah-vb/samples/sample6.vb)]

## <a name="error-notification-via-e-mail"></a>透過電子郵件錯誤通知

將錯誤記錄至資料庫，以及 ELMAH 也可以設定電子郵件給指定的收件者的錯誤詳細資料。 這項功能由`ErrorMailModule`HTTP 模組; 因此，您必須註冊此 HTTP 模組`Web.config`才能傳送錯誤詳細資料，透過電子郵件。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample7.xml)]

接下來，指定在錯誤電子郵件的相關資訊`<elmah>`項目的`<errorMail>`區段中，表示電子郵件的寄件者和收件者、 主旨、 電子郵件是否以非同步方式傳送。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample8.xml)]

使用就地上述的設定，每當發生執行階段錯誤 ELMAH 傳送電子郵件support@example.com與錯誤詳細資料。 ELMAH 的錯誤電子郵件包含錯誤詳細資料網頁上，也就是錯誤訊息、 堆疊追蹤和伺服器變數中顯示的相同資訊 (回頭參考**圖 4**和**5**)。 錯誤的電子郵件也包含例外狀況詳細資料黃色螢幕的死亡內容做為附件 (`YSOD.html`)。

**圖 8**顯示造訪所產生的 ELMAH 的錯誤電子郵件`Genre.aspx?ID=foo`。 雖然**圖 8**顯示只有錯誤訊息和堆疊追蹤，伺服器變數會包含進一步向下電子郵件的本文中。

[![](logging-error-details-with-elmah-vb/_static/image21.png)](logging-error-details-with-elmah-vb/_static/image20.png)

**圖 8**： 您可以設定要傳送錯誤詳細資料，透過電子郵件的 ELMAH  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-vb/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>只記錄您感興趣的錯誤

根據預設，ELMAH 會記錄每個未處理的例外狀況，包括 404 和其他 HTTP 錯誤的詳細資料。 您可以指示要略過這些或其他類型的錯誤使用錯誤篩選 ELMAH。 篩選的邏輯由 ELMAH 的`ErrorFilterModule`HTTP 模組，您必須註冊在`Web.config`才能使用篩選的邏輯。 中指定的規則篩選`<errorFilter>`> 一節。

下列標記會指示 ELMAH 不記錄 404 錯誤。

[!code-xml[Main](logging-error-details-with-elmah-vb/samples/sample9.xml)]

> [!NOTE]
> 不要忘記，才能使用錯誤的篩選必須註冊`ErrorFilterModule`HTTP 模組。


`<equal>`元素內`<test>`區段指判斷提示。 如果評估為 true 的判斷提示錯誤被篩選從 ELMAH 的記錄檔。 有可用的其他判斷提示，包括： `<greater>`， `<greater-or-equal>`， `<not-equal>`， `<lesser>`， `<lesser-or-equal>`，依此類推。 您也可以結合使用判斷提示`<and>`和`<or>`布林運算子。 您甚至可以包含簡單的 JavaScript 運算式，做為判斷提示，或在 C# 或 Visual Basic 中撰寫您自己的判斷提示。

如需有關篩選功能的 ELMAH 的錯誤的詳細資訊，請參閱[錯誤篩選區段](https://code.google.com/p/elmah/wiki/ErrorFiltering)中[ELMAH wiki](https://code.google.com/p/elmah/w/list)。

## <a name="summary"></a>總結

ELMAH 提供簡單，但功能強大的機制，ASP.NET web 應用程式中記錄錯誤。 Microsoft 的健全狀況監視系統，例如 ELMAH 錯誤記錄至資料庫，以開發人員透過電子郵件傳送錯誤詳細資料。 ELMAH 包含超出支援更大範圍的錯誤記錄檔資料存放區，包括與監視系統健全狀況，不同的是： Microsoft SQL Server、 Microsoft Access、 Oracle、 XML 檔案，以及其他幾個。 此外，ELMAH 提供內建的機制，可檢視錯誤記錄檔及網頁，從特定錯誤的詳細資訊`elmah.axd`。 `elmah.axd`頁面也可以轉譯為 RSS 摘要或以逗號分隔值檔案 (CSV)，您可以閱讀使用 Microsoft Excel 的錯誤資訊。 您也可以篩選錯誤指示 ELMAH，從記錄檔使用宣告式或以程式設計方式判斷提示。 且 ELMAH 可以搭配 ASP.NET 版本 1.x 應用程式。

每個已部署的應用程式應該擁有一些機制，自動記錄未處理例外狀況，並傳送通知給開發團隊。 是否這是使用的健全狀況監視或 ELMAH 是次要資料庫。 換句話說，它不會真的很重要更無論您使用健全狀況監視或 ELMAH;評估這兩個系統，然後選擇其中一個最符合您的需求。 基本上是重要的是一些機制，放在實際執行環境中記錄未處理的例外狀況的地方。

祝您程式設計 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ELMAH-錯誤記錄模組和處理常式](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH 專案頁面](https://code.google.com/p/elmah/)（來源的程式碼、 範例、 wiki）
- [插入 ELMAH 成 Web 應用程式以攔截未處理例外狀況](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx)（影片）
- [安全性錯誤記錄 頁面](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [使用 HTTP 模組和處理常式來建立隨插即用的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)
- [網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

>[!div class="step-by-step"]
[上一頁](logging-error-details-with-asp-net-health-monitoring-vb.md)
[下一頁](precompiling-your-website-vb.md)
