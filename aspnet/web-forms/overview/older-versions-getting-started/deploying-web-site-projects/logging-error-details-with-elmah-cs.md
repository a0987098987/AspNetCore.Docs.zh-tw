---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
title: 記錄錯誤的詳細資料，使用 ELMAH (C#) |Microsoft Docs
author: rick-anderson
description: 錯誤記錄模組和處理常式 (ELMAH) 提供了另一種方法來記錄在生產環境中的執行階段錯誤。 ELMAH 是免費、 開放原始碼的錯誤...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/09/2009
ms.topic: article
ms.assetid: 11f6fe44-64ef-4a38-a3b4-35c7bb992352
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/logging-error-details-with-elmah-cs
msc.type: authoredcontent
ms.openlocfilehash: 0b9b91b54f50ddd86e102fea3b0dfd5505e3f594
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388473"
---
<a name="logging-error-details-with-elmah-c"></a>使用 ELMAH (C#) 中記錄錯誤的詳細資料
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/1/0/C/10CC829F-A808-4302-97D3-59989B8F9C01/ASPNET_Hosting_Tutorial_14_CS.zip)或[下載 PDF](http://download.microsoft.com/download/5/C/5/5C57DB8C-5DEA-4B3A-92CA-4405544D313B/aspnet_tutorial14_ELMAH_cs.pdf)

> 錯誤記錄模組和處理常式 (ELMAH) 提供了另一種方法來記錄在生產環境中的執行階段錯誤。 ELMAH 是免費、 開放原始碼錯誤記錄程式庫，其中包含的功能，例如錯誤篩選及 RSS 摘要，以檢視錯誤記錄檔，從網頁，或將其下載為逗號分隔的檔案的能力。 本教學課程將逐步引導下載及設定 ELMAH。


## <a name="introduction"></a>簡介

[前述教學課程](logging-error-details-with-asp-net-health-monitoring-cs.md)檢查 ASP。NET 的狀況監控系統，可提供現成的方塊程式庫記錄各種 Web 事件。 許多開發人員使用健全狀況監視記錄和電子郵件的未處理例外狀況詳細資料。 不過，有幾個與此系統的痛點。 首先是沒有任何一種使用者介面，以檢視記錄的事件的相關資訊。 如果您想要查看摘要的 10 名的最後一個錯誤，或檢視發生過去一週的錯誤詳細資料，您必須透過資料庫的其中一個專欄、 審視您的電子郵件收件匣，或建置可顯示資訊的網頁`aspnet_WebEvent_Events`資料表。

另一個痛苦點都著重於健全狀況監視的複雜度。 因為健康情況監視可以用來記錄在眾多不同的事件，而且有各種選項來指示如何及何時會記錄事件，正確設定狀況監控系統可以是繁重的工作。 最後，還有相容性問題。 因為健康情況監視第一次加入至.NET Framework 2.0 版中，它不適用於舊版的 web 應用程式建置使用 ASP.NET 版本 1.x。 此外，`SqlWebEventProvider`類別，我們使用在先前的教學課程，以記錄至資料庫的錯誤詳細資料，僅適用於 Microsoft SQL Server 資料庫。 您必須建立自訂記錄提供者類別，您需要將錯誤記錄到替代資料存放區，例如 XML 檔案或 Oracle 資料庫。

狀況監控系統的替代方式是錯誤記錄模組和處理常式 (ELMAH)，所建立的免費、 開放原始碼的錯誤記錄系統[Atif Aziz](http://www.raboof.com/)。 兩個系統之間最顯著的差異是 ELAMH 的能夠顯示的錯誤和從網頁和特定錯誤的詳細資料，透過 rss 摘要清單。 ELMAH 很容易設定比健全狀況監視，因為它只會記錄錯誤。 此外，ELMAH 會包含支援 ASP.NET 1.x，ASP.NET 2.0 和 ASP.NET 3.5 應用程式，並隨附於各種不同的記錄來源提供者。

本教學課程會逐步新增 ELMAH ASP.NET 應用程式所需的步驟。 讓我們開始吧 ！

> [!NOTE]
> 健全狀況監視系統和 ELMAH 兩者都有自己的集合的優缺點。 我建議您試用這兩個系統，並決定哪一個最能符合您的需求。


## <a name="adding-elmah-to-an-aspnet-web-application"></a>ASP.NET Web 應用程式中加入 ELMAH

整合新的或現有的 ASP.NET 應用程式中的 ELMAH 是簡單明瞭的程序，會在 5 分鐘。 簡單的說，它會包含四個簡單步驟：

1. 下載 ELMAH，並加入`Elmah.dll`web 應用程式時，組件
2. 註冊 ELMAH 的 HTTP 模組和處理常式中的`Web.config`，
3. 指定 ELMAH 的組態選項，以及
4. 如有需要請建立錯誤記錄檔來源基礎結構。

讓我們一一介紹這些四個步驟中，一次。

### <a name="step-1-downloading-the-elmah-project-files-and-addingelmahdllto-your-web-application"></a>步驟 1： 下載 ELMAH 專案檔案，並新增`Elmah.dll`您 Web 應用程式

ELMAH 1.0 BETA 3 (建置 10617)，在撰寫時，時間的最新版本會包含在本教學課程中下載。 或者，您可以瀏覽[ELMAH 網站](https://code.google.com/p/elmah/)來取得最新版本，或下載的原始程式碼。 擷取您的桌面上的資料夾，ELMAH 下載，並找出 ELMAH 的組件檔 (`Elmah.dll`)。

> [!NOTE]
> `Elmah.dll`檔案會位於下載`Bin`具有不同的.NET Framework 版本和發行和偵錯組建的子資料夾的資料夾。 使用適當的 framework 版本的發行組建。 比方說，如果您要建置的 ASP.NET 3.5 web 應用程式，請將複製`Elmah.dll`檔案從`Bin\net-3.5\Release`資料夾。


接下來，開啟 Visual Studio，並加入專案中的組件，以滑鼠右鍵按一下方案總管] 和 [選擇加入的參考，從內容功能表中的網站名稱。 這會顯示 [加入參考] 對話方塊。 瀏覽至 瀏覽 索引標籤，然後選擇 `Elmah.dll`檔案。 這個動作會將`Elmah.dll`檔案，以 web 應用程式的`Bin`資料夾。

> [!NOTE]
> Web 應用程式專案 (WAP) 型別不會顯示`Bin`方案總管 中的資料夾。 相反地，它會列出這些項目參考 資料夾下。


`Elmah.dll`組件包括 ELMAH 系統所使用的類別。 這些類別可分為三種類別：

- **HTTP 模組**-HTTP 模組是一個類別來定義事件處理常式`HttpApplication`事件，例如`Error`事件。 ELMAH 包含多個 HTTP 模組，三個最密切關聯的項目要： 

    - `ErrorLogModule` -記錄到記錄檔來源的未處理例外狀況。
    - `ErrorMailModule` -傳送電子郵件訊息的未處理的例外狀況詳細資料。
    - `ErrorFilterModule` -適用於開發人員指定的篩選條件來判斷何種例外狀況的記錄，以及是會被忽略。
- **HTTP 處理常式**-HTTP 處理常式是負責產生要求的特定型別的標記的類別。 ELMAH 包含 web 網頁、 RSS 摘要，或以逗號分隔的檔案 (CSV) 轉譯錯誤詳細資料的 HTTP 處理常式。
- **錯誤記錄檔來源**-根據預設，ELMAH 可以將記憶體，到 Microsoft SQL Server 資料庫，到 Microsoft Access 資料庫，Oracle 資料庫，錯誤記錄至 SQLite 資料庫，或 Vista DB 資料庫的 XML 檔案。 狀況監控系統，例如 ELMAH 的架構是使用提供者模型，這表示您可以用來建立，並將完美地整合您自己自訂的記錄檔來源提供者，視所建立的。

### <a name="step-2-registering-elmahs-http-module-and-handler"></a>步驟 2： 註冊 ELMAH 的 HTTP 模組和處理常式

雖然`Elmah.dll`檔案包含 HTTP 模組和處理常式需要自動記錄未處理的例外狀況，並顯示從網頁上的錯誤詳細資料，這些必須先明確註冊 web 應用程式的組態。 `ErrorLogModule` HTTP 模組，一旦註冊之後，訂閱`HttpApplication`的`Error`事件。 引發這個事件是每當`ErrorLogModule`例外狀況的詳細資訊記錄至指定的記錄檔來源。 我們會了解如何在下一步 區段中，定義的記錄檔來源提供者 」"設定 ELMAH。 `ErrorLogPageFactory` HTTP 處理常式 factory 會負責產生標記，檢視錯誤記錄檔，從網頁上時。

註冊 HTTP 模組和處理常式的特定語法，取決於正在強化功能的站台的 web 伺服器。 ASP.NET 程式開發伺服器和 Microsoft 的 iis 6.0 和更早版本，HTTP 模組和處理常式會在註冊`<httpModules>`並`<httpHandlers>`區段，其中會出現在`<system.web>`項目。 如果您使用 IIS 7.0，則他們需要在中註冊`<system.webServer>`項目的`<modules>`和`<handlers>`區段。 幸運的是，您可以在其中定義的 HTTP 模組和中的處理常式*兩者*放置不論所使用的 web 伺服器。 此選項是最方便的其中一個，因為它可讓相同的設定，用於開發和生產環境，不論所使用的 web 伺服器。

開始註冊`ErrorLogModule`HTTP 模組和`ErrorLogPageFactory`中的 HTTP 處理常式`<httpModules>`並`<httpHandlers>`一節中`<system.web>`。 如果您的組態已定義這兩個元素則只包含`<add>`ELMAH 的 HTTP 模組和處理常式的項目。

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample1.xml)]

接下來，註冊 ELMAH 的 HTTP 模組，並在處理常式`<system.webServer>`項目。 同樣地，如果這個項目已不存在於您的組態再將它新增。

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample2.xml)]

根據預設，IIS 7 抱怨 HTTP 模組和處理常式會在註冊`<system.web>`一節。 `validateIntegratedModeConfiguration`屬性中`<validation>`元素會指示 IIS 7，以隱藏這類的錯誤訊息。

請注意，註冊的語法`ErrorLogPageFactory`包含 HTTP 處理常式`path`屬性設為`elmah.axd`。 這個屬性會通知 web 應用程式，如果名為的頁面要求到達`elmah.axd`則要求應該由處理`ErrorLogPageFactory`HTTP 處理常式。 我們會看到`ErrorLogPageFactory`稍後在本教學課程的作用中的 HTTP 處理常式。

### <a name="step-3-configuring-elmah"></a>步驟 3： 設定 ELMAH

會尋找其網站中的組態選項的 ELMAH`Web.config`檔案中名為自訂組態區段`<elmah>`。 若要使用中的自訂區段`Web.config`首先必須定義在`<configSections>`項目。 開啟`Web.config`檔案，並新增下列標記來`<configSections>`:

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample3.xml)]

> [!NOTE]
> 如果您要設定 ELMAH ASP.NET 1.x 應用程式然後移除`requirePermission="false"`屬性從`<section>`上述項目。


上述語法會註冊自訂`<elmah>`區段，其各小節： `<security>`， `<errorLog>`， `<errorMail>`，和`<errorFilter>`。

接下來，新增`<elmah>`一節`Web.config`。 此區段應該會出現在相同的層級`<system.web>`項目。 內部`<elmah>`一節新增`<security>`和`<errorLog>`章節就像這樣：

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample4.xml)]

`<security>`一節的`allowRemoteAccess`屬性會指出是否允許遠端存取。 如果此值設定為 0，然後錯誤記錄 web 頁面只能檢視在本機。 如果這個屬性設定為 1 遠端和本機的訪客的已啟用錯誤記錄檔的網頁。 現在，讓我們來停用錯誤記錄 web 頁面遠端的訪客。 稍後我們有機會這麼做的安全性考量的討論之後，我們會允許遠端存取。

`<errorLog>`區段會定義錯誤記錄檔來源的指定位置記錄的錯誤詳細資料，類似於`<providers>`狀況監控系統中的一節。 上述語法會指定`SqlErrorLog`錯誤記錄檔來源，錯誤記錄到 Microsoft SQL Server 資料庫所指定的類別`connectionStringName`屬性值。

> [!NOTE]
> ELMAH 隨附其他錯誤記錄檔提供者可將錯誤記錄到 XML 檔案、 Microsoft Access 資料庫、 Oracle 資料庫和其他資料存放區。 請參閱範例`Web.config`會包含有關如何使用這些替代錯誤記錄檔提供者的 ELMAH 下載的檔案。


### <a name="step-4-creating-the-error-log-source-infrastructure"></a>步驟 4： 建立錯誤記錄檔來源基礎結構

ELMAH 的`SqlErrorLog`提供者記錄到指定的 Microsoft SQL Server 資料庫的錯誤詳細資料。 `SqlErrorLog`提供者必須要有此資料庫有一個名為資料表`ELMAH_Error`和三個預存程序： `ELMAH_GetErrorsXml`， `ELMAH_GetErrorXml`，和`ELMAH_LogError`。 ELMAH 下載包含名為的檔案`SQLServer.sql`在`db`資料夾，其中包含用於建立這個資料表，而且這些 T-SQL 預存程序。 您必須在您要使用的資料庫上執行這些陳述式`SqlErrorLog`提供者。

**數字 1**並**2**後所需的資料庫物件，顯示在 Visual Studio 中的 資料庫總管`SqlErrorLog`已新增提供者。

[![](logging-error-details-with-elmah-cs/_static/image2.png)](logging-error-details-with-elmah-cs/_static/image1.png)

**圖 1**:`SqlErrorLog`提供者會記錄錯誤`ELMAH_Error`資料表

[![](logging-error-details-with-elmah-cs/_static/image4.png)](logging-error-details-with-elmah-cs/_static/image3.png)

**圖 2**:`SqlErrorLog`提供者會使用三個預存程序

## <a name="elmah-in-action"></a>作用中的 ELMAH

此時我們已新增至註冊的 web 應用程式的 ELMAH `ErrorLogModule` HTTP 模組和`ErrorLogPageFactory`HTTP 處理常式中，指定中的 ELMAH 的設定選項`Web.config`，並新增所需的資料庫物件`SqlErrorLog`錯誤記錄檔提供者。 現在，我們已準備好要查看作用中的 ELMAH ！ 瀏覽的書籍評論網站並瀏覽的頁面，產生執行階段錯誤，例如`Genre.aspx?ID=foo`，或不存在的頁面，例如`NoSuchPage.aspx`。 瀏覽這些網頁時所看到取決於`<customErrors>`設定和是否您造訪在本機或遠端。 (請參閱上一步[*顯示自訂錯誤網頁*教學課程](displaying-a-custom-error-page-cs.md)溫習本主題。)

ELMAH 並不會影響內容顯示給使用者時就會發生未處理的例外狀況;此外，它只會記錄其詳細資料。 此錯誤記錄檔是可從網站上`elmah.axd`從您的網站，例如`http://localhost/BookReviews/elmah.axd`。 (此檔案並未實際存在專案中，但要求的傳入`elmah.axd`執行階段會將它分派至`ErrorLogPageFactory`HTTP 處理常式，它會產生傳送回瀏覽器的標記。)

> [!NOTE]
> 您也可以使用`elmah.axd`頁面，即可指示 ELMAH，以產生測試時發生錯誤。 瀏覽`elmah.axd/test`(像是`http://localhost/BookReviews/elmah.axd/test`) 會導致擲回例外狀況類型的 ELMAH `Elmah.TestException`，其中包含錯誤訊息: 「 這是您可以放心忽略測試例外狀況 」。


**圖 3**瀏覽時顯示錯誤記錄檔`elmah.axd`從開發環境。

[![](logging-error-details-with-elmah-cs/_static/image6.png)](logging-error-details-with-elmah-cs/_static/image5.png)

**圖 3**:`Elmah.axd`顯示網頁的錯誤記錄檔  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image7.png))

錯誤記錄檔**圖 3**包含六個錯誤項目。 每個項目包含的 HTTP 狀態碼 （404 或 500，這些錯誤）、 類型、 描述、 發生錯誤時，登入的使用者名稱和日期和時間。 按一下 詳細資料 連結會顯示包含錯誤詳細資料黃色死亡畫面所示相同的錯誤訊息的頁面 (請參閱**圖 4**) 以及在發生錯誤時的伺服器變數的值 (請參閱**圖 5**)。 您也可以檢視在其中儲存錯誤詳細資料，其中包含 HTTP POST 標頭中的其他資訊，例如值的原始 XML。

[![](logging-error-details-with-elmah-cs/_static/image9.png)](logging-error-details-with-elmah-cs/_static/image8.png)

**圖 4**： 檢視錯誤詳細資料 YSOD  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image10.png))

[![](logging-error-details-with-elmah-cs/_static/image12.png)](logging-error-details-with-elmah-cs/_static/image11.png)

**圖 5**： 瀏覽伺服器變數集合，在發生錯誤時的值  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image13.png))

部署至生產環境網站的 ELMAH 需要：

- 複製`Elmah.dll`檔案`Bin`上生產環境中，資料夾
- 複製的 ELMAH 特定組態設定`Web.config`上生產環境中，使用的檔案和
- 將錯誤記錄檔來源基礎結構新增至生產環境資料庫。

我們探討了可將檔案從開發複製到先前的教學課程中的生產環境的技術。 可能是在生產資料庫上取得的錯誤記錄檔來源基礎結構的最簡單方式是使用 SQL Server Management Studio 來連線到生產資料庫，然後執行`SqlServer.sql`指令碼檔案，它會建立所需的資料表，並儲存程序。

### <a name="viewing-the-error-details-page-on-production"></a>在生產環境中檢視錯誤詳細資料頁面

之後到生產環境中部署您的網站，請移至生產網站，並產生未處理的例外狀況。 開發環境中，ELMAH 就不會影響處理的例外狀況發生時顯示錯誤頁面相反地，它只會記錄錯誤。 如果您嘗試瀏覽錯誤記錄檔頁面 (`elmah.axd`) 從生產環境中，會顯示與所示的 禁止 頁面**圖 6**。

[![](logging-error-details-with-elmah-cs/_static/image15.png)](logging-error-details-with-elmah-cs/_static/image14.png)

**圖 6**： 根據預設，遠端的訪客無法檢視錯誤記錄檔的 Web 網頁  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image16.png))

請記得，ELMAH 組態中`<security>`一節我們將設定`allowRemoteAccess`屬性為 0，以檢視錯誤記錄檔時，禁止遠端使用者。 請務必禁止從檢視錯誤記錄檔的匿名訪客，安全性弱點或其他機密資訊，可能會顯示錯誤詳細資料。 如果您決定將這個屬性設定為 1，並啟用遠端錯誤記錄檔的存取，則務必要鎖定`elmah.axd`路徑，因此只有獲得授權的訪客具有存取權限。 這可藉由新增`<location>`項目`Web.config`檔案。

下列組態會允許系統管理員角色，才能存取錯誤記錄檔的網頁只有使用者︰

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample5.xml)]

> [!NOTE]
> 在已加入系統管理員角色和三個使用者，在系統中，Scott、 Jisun 和 Alice- [*設定網站，會使用應用程式服務*教學課程](configuring-a-website-that-uses-application-services-cs.md)。 使用者 Scott 和 Jisun 是系統管理員角色的成員。 如需有關驗證和授權的詳細資訊，請參閱我[網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)。


遠端使用者現在可以檢視錯誤記錄檔，在實際執行環境回頭**數字 3**， **4**，並**5**的錯誤記錄 web 頁面的螢幕擷取畫面。 不過，如果匿名或非系統管理員的使用者嘗試檢視錯誤記錄檔頁面它們會自動重新導向至登入頁面 (`Login.aspx`)，作為**圖 7**顯示。

[![](logging-error-details-with-elmah-cs/_static/image18.png)](logging-error-details-with-elmah-cs/_static/image17.png)

**圖 7**： 未經授權的使用者會自動重新導向至登入頁面  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image19.png))

### <a name="programmatically-logging-errors"></a>以程式設計方式記錄錯誤

ELMAH 的`ErrorLogModule`HTTP 模組會自動記錄未處理例外狀況至指定的記錄檔來源。 或者，您可以記錄錯誤，而不必使用引發未處理例外狀況`ErrorSignal`類別和其`Raise`方法。 `Raise`方法會傳遞`Exception`物件，並加以記錄，如同該例外狀況已擲回，且已達到 ASP.NET 執行階段，而不需要處理。 差異，不過，是要求會繼續執行正常之後`Raise`已呼叫方法，而擲回未處理的例外狀況中斷要求的正常執行，並讓 ASP.NET 執行階段顯示設定錯誤頁面。

`ErrorSignal`類別會在其中一些動作，可能會失敗，但其失敗不是重大的整體的作業正在執行的情況下很有用。 比方說，網站可能包含一個表單，會採用使用者的輸入，將它儲存在資料庫中，然後傳送電子郵件通知使用者他們已處理的資訊。 如果成功，資訊儲存到資料庫，但卻發生錯誤時傳送電子郵件訊息，就應該發生什麼情況？ 其中一個選項是擲回例外狀況，並將使用者傳送至錯誤頁面。 不過，這可能會混淆使用者以為未儲存它們所輸入的資訊。 另一種方法是將電子郵件相關的錯誤，記錄，但不是會改變以任何方式的使用者體驗。 這正是`ErrorSignal`類別就很有用。

[!code-csharp[Main](logging-error-details-with-elmah-cs/samples/sample6.cs)]

## <a name="error-notification-via-email"></a>透過電子郵件的錯誤通知

將錯誤記錄至資料庫，以及 ELMAH 也可以設定電子郵件給指定的收件者的錯誤詳細資料。 此功能會由`ErrorMailModule`HTTP 模組; 因此，您必須註冊此 HTTP 模組中`Web.config`才能傳送錯誤詳細資料，透過電子郵件。

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample7.xml)]

接下來，指定在錯誤電子郵件的相關資訊`<elmah>`項目的`<errorMail>`區段中，指出電子郵件的寄件者和收件者、 主旨、 電子郵件是否以非同步方式傳送。

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample8.xml)]

使用上述設定後，每當發生執行階段錯誤 ELMAH 會將傳送電子郵件給support@example.com錯誤詳細資料。 ELMAH 的錯誤的電子郵件包含錯誤詳細資料網頁上，也就是錯誤訊息、 堆疊追蹤和伺服器變數中顯示的相同資訊 (請參閱上一步**數字 4**並**5**)。 錯誤的電子郵件也包含例外狀況詳細資料黃色死亡畫面內容做為附件 (`YSOD.html`)。

**圖 8**顯示 ELMAH 的錯誤電子郵件，請瀏覽產生`Genre.aspx?ID=foo`。 雖然**圖 8**顯示只有錯誤訊息和堆疊追蹤，伺服器變數會包含進一步向下電子郵件的主體中。

[![](logging-error-details-with-elmah-cs/_static/image21.png)](logging-error-details-with-elmah-cs/_static/image20.png)

**圖 8**： 您可以設定要傳送錯誤詳細資料，透過電子郵件的 ELMAH  
([按一下以檢視完整大小的影像](logging-error-details-with-elmah-cs/_static/image22.png))

## <a name="only-logging-errors-of-interest"></a>只記錄感興趣的錯誤

根據預設，ELMAH 會記錄每個未處理的例外狀況，包括 404 和其他 HTTP 錯誤的詳細資料。 您可以指示要略過這些或其他類型的錯誤使用錯誤篩選的 ELMAH。 篩選的邏輯的執行方式的 ELMAH `ErrorFilterModule` HTTP 模組，您必須在中註冊`Web.config`若要使用篩選的邏輯。 中指定的規則篩選`<errorFilter>`一節。

下列標記指示 ELMAH 記錄 404 錯誤。

[!code-xml[Main](logging-error-details-with-elmah-cs/samples/sample9.xml)]

> [!NOTE]
> 忘了，若要使用錯誤篩選，您必須註冊`ErrorFilterModule`HTTP 模組。


`<equal>`內的項目`<test>`區段稱為判斷提示。 判斷提示會評估為 true 時的錯誤會篩選從 ELMAH 的記錄檔。 其他判斷提示已推出，包括： `<greater>`， `<greater-or-equal>`， `<not-equal>`， `<lesser>`， `<lesser-or-equal>`，依此類推。 您也可以結合使用的判斷提示`<and>`和`<or>`布林運算子。 不僅如此，您甚至可以包含簡單的 JavaScript 運算式，做為判斷提示，或以 C# 或 Visual Basic 中撰寫您自己的判斷提示。

如需有關 ELMAH 的錯誤篩選功能的詳細資訊，請參閱[錯誤篩選區段](https://code.google.com/p/elmah/wiki/ErrorFiltering)中[ELMAH wiki](https://code.google.com/p/elmah/w/list)。

## <a name="summary"></a>總結

ELMAH 提供簡單但功能強大的機制，在 ASP.NET web 應用程式中記錄錯誤。 Microsoft 的健全狀況監視系統，例如 ELMAH 可以將錯誤記錄到資料庫，以開發人員透過電子郵件傳送錯誤詳細資料。 與不同狀況監控系統，ELMAH 會包含預設支援更大範圍的錯誤記錄檔資料存放區，包括： Microsoft SQL Server、 Microsoft Access、 Oracle、 XML 檔案，以及其他許多選項。 此外，ELMAH 會提供內建的機制，來檢視錯誤記錄檔和詳細資料網頁、 從特定的錯誤`elmah.axd`。 `elmah.axd`頁面也可以轉譯為 RSS 摘要或逗號分隔值 (CSV) 檔案，您可以閱讀使用 Microsoft Excel 的錯誤資訊。 您也可以篩選錯誤指示 ELMAH，從記錄檔使用宣告式或以程式設計方式判斷提示。 ELMAH 適用於 ASP.NET 版本 1.x 的應用程式。

每個已部署的應用程式應該有一些機制來自動記錄未處理的例外狀況，並將通知傳送給開發小組。 是否有所使用健康情況監視] 或 [ELMAH 是次要資料庫。 換句話說，則怪罪更您是使用健康情況監視或 ELMAH;評估這兩個系統，然後選擇最適合您的需求。 本質上重要的是一些機制，成為在生產環境中記錄未處理的例外狀況的地方。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [ELMAH-錯誤記錄模組和處理常式](http://dotnetslackers.com/articles/aspnet/ErrorLoggingModulesAndHandlers.aspx)
- [ELMAH 專案頁面](https://code.google.com/p/elmah/)（來源的程式碼、 範例、 wiki）
- [插入 ELMAH 到 Web 應用程式攔截未處理的例外狀況](http://screencastaday.com/ScreenCasts/43_Plugging_Elmah_into_Web_Application_to_Catch_Unhandled_Exceptions.aspx)（影片）
- [安全性錯誤記錄檔頁面](https://code.google.com/p/elmah/wiki/SecuringErrorLogPages)
- [使用 HTTP 模組和處理常式來建立可外掛的 ASP.NET 元件](https://msdn.microsoft.com/library/aa479332.aspx)
- [網站安全性教學課程](../../older-versions-security/introduction/security-basics-and-asp-net-support-cs.md)

> [!div class="step-by-step"]
> [上一頁](logging-error-details-with-asp-net-health-monitoring-cs.md)
> [下一頁](precompiling-your-website-cs.md)
