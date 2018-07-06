---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
title: 設定生產環境 Web 應用程式使用生產資料庫 (VB) |Microsoft Docs
author: rick-anderson
description: 先前的教學課程所述，並不常見的開發和生產環境之間不同的設定資訊。 這是 es...
ms.author: aspnetcontent
ms.date: 04/23/2009
ms.assetid: a64a7aa0-6608-449e-83bf-1ef8cceee504
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-vb
msc.type: authoredcontent
ms.openlocfilehash: 952bdafd06143ce24e9e8beb0d6e6343400ffdc6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37840696"
---
<a name="configuring-the-production-web-application-to-use-the-production-database-vb"></a>設定生產環境 Web 應用程式使用生產資料庫 (VB)
====================
藉由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_VB.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_vb.pdf)

> 先前的教學課程所述，並不常見的開發和生產環境之間不同的設定資訊。 這是資料驅動 web 應用程式，特別有用，因為開發和生產環境之間不同的資料庫連接字串。 本教學課程將探討設定生產環境，以納入更多詳細資料中的適當的連接字串的方式。


## <a name="introduction"></a>簡介

資料驅動 web 應用程式通常會使用不同的資料庫，在開發比生產環境中。 對於 web 主機提供者所裝載，並在本機開發的應用程式，開發資料庫通常位於開發人員電腦上時在虛擬主機公司的設備的資料庫伺服器上裝載生產資料庫中。 資料驅動 web 應用程式部署需要將開發資料庫複製到生產環境資料庫伺服器。 在上一個教學課程中，我們討論過如何完成此步驟。

Web 應用程式使用中的資訊*連接字串*建立與資料庫的連線。 連接字串中，通常會儲存在`Web.config`，指定資料庫伺服器名稱、 資料庫、 安全性內容中，以及其他資訊的名稱。 因為 web 應用程式所使用的資料庫，取決於是否正在開發或生產環境中執行 web 應用程式，連接字串必須不同的兩種環境中。

您不常見的開發和生產環境之間不同的設定資訊。 *常見組態差異之間開發和生產環境*教學課程所討論的技術上維護個別的組態資訊，這兩種環境，以及簡短討論之間資料庫連接字串。 本教學課程將探討設定生產環境，以納入更多詳細資料中的適當的連接字串的方式。

## <a name="examining-the-connection-string-information"></a>檢查連接字串資訊

書籍評論的 web 應用程式所使用的連接字串儲存在應用程式的組態檔， `Web.config`。 `Web.config` 包含為了儲存連接字串，恰如其名為的特殊區段[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)。 `Web.config`檔案的書籍評論網站有一個名為此區段中定義的連接字串`ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample1.xml)]

連接字串-資料來源 =。 \SQLEXPRESS;AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated Security = True;使用者執行個體 = True-包含數個選項和值，以分隔的一個分號和每個選項的選項/值組與號分隔的值。 此連接字串中使用的四個選項如下：

- `Data Source` -指定資料庫伺服器和資料庫伺服器執行個體名稱的位置 （如果有的話）。 值， `.\SQLEXPRESS`，是一個範例沒有資料庫伺服器和執行個體名稱。 時間期限可讓您指定資料庫伺服器是在應用程式的同一部電腦上執行個體名稱是`SQLEXPRESS`。
- `AttachDbFilename` -指定資料庫檔案的位置。 值包含預留位置`|DataDirectory|`，這是解析為 s 的應用程式的完整路徑`App_Data`在執行階段資料夾。
- `Integrated Security` -布林值，指出是否要連接到資料庫 (false) 或目前的 Windows 帳戶認證 (true) 時，使用指定的使用者名稱/密碼。
- `User Instance` -指出是否允許本機電腦上的非系統管理使用者連接和連接到 SQL Server Express Edition 資料庫的 SQL Server Express Edition 的特定組態選項。 請參閱[SQL Server Express 使用者執行個體](https://msdn.microsoft.com/library/ms254504.aspx)如需有關這項設定。
  

允許的連接字串選項取決於您所連接的資料庫和[ADO.NET](http://ADO.NET)所使用的資料庫提供者。 例如，連接字串連接到 Microsoft SQL Server 資料庫與建立用來連接到 Oracle 資料庫。 同樣地，連接到 Microsoft SQL Server 資料庫，使用 SqlClient 提供者會使用不同的連接字串比時使用的 OLE DB 提供者。

您可以手動使用像是站台來建置的資料庫連接字串[ConnectionStrings.com](http://www.connectionstrings.com/)為的資源有哪些選項。 不過，您更輕鬆的方法是將資料庫新增至 Visual Studio 中的 [伺服器總管]，然後擷取 [屬性] 視窗的連接字串。 可讓 s 建構生產環境資料庫伺服器的連接字串使用後者這種技術。

開啟 Visual Studio，然後瀏覽至 [伺服器總管] 視窗 （在 Visual Web Developer 中，此視窗稱為資料庫總管）。 以滑鼠右鍵按一下 [資料連線] 選項，並從內容功能表中選擇 [加入連接] 選項。 這會顯示在 圖 1 所示的精靈。 選擇適當的資料來源，然後按一下 [繼續]。


[![若要將新的資料庫新增至 [伺服器總管] 中選擇](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image1.jpg) 

**圖 1**： 選擇將新的資料庫新增至 [伺服器總管] ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image3.jpg))


接下來，指定 各種資料庫連接資訊 （請參閱 圖 2）。 當您註冊您的 web 主機服務的公司，他們應該如何提供資訊連線至資料庫伺服器名稱、 資料庫名稱、 使用者名稱和密碼，用來連接到資料庫，資料庫等等。 輸入這項資訊之後按一下 [確定] 完成這個精靈，並將資料庫加入至 [伺服器總管] 中。


[![指定的資料庫連線資訊](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image4.jpg) 

**圖 2**： 指定資料庫連接資訊 ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image6.jpg))


生產環境資料庫現在應該會列出在 [伺服器總管] 中。 從 [伺服器總管] 中選取資料庫，請移至 [屬性] 視窗。 您在這裡可以找到名為資料庫的連接字串與連接字串的屬性。 假設您在生產和 SqlClient 提供者上使用 Microsoft SQL Server 資料庫連接字串看起來應該如下所示：

<strong>資料來源 =<em>serverName</em>;初始目錄 =<em>databaseName</em>;Persist Security Info = True;使用者識別碼 =<em>username</em>;密碼 =*密碼</strong>*

其中*serverName*， *databaseName*， *username*，以及*密碼*與資料庫伺服器名稱，資料庫的值名稱和使用者名稱和密碼提供給您的 web 主機公司。

## <a name="deploying-the-book-reviews-web-application"></a>書籍評論 Web 應用程式部署

前述教學課程會逐步執行將開發資料庫複製到生產環境中，但未不瀏覽資料導向應用程式部署。 目前生產環境包含的資料庫，但使用活頁簿會檢閱應用程式的版本與靜態的評論。 我們需要新的資料導向應用程式部署至實際執行伺服器，以及更新的組態資訊。

請花一點時間，將資料導向應用程式從開發環境到生產環境。 在先前的教學課程中，此程序所涵蓋的詳細資料。 如果您需要複習一下，請參閱*部署您的網站使用 FTP 用戶端*或*部署您的網站使用的 Visual Studio*教學課程。 您必須確定生產環境資料庫連接字串是用在生產環境中，這表示替代`Web.config`檔案必須部署。 具體來說，這修改`Web.config`檔案的`<connectionStrings>`元素必須包含生產環境資料庫連接字串，並看起來應該如下所示：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample2.xml)]

請注意，連接字串中`<connectionStrings>`元素的名稱相同 (`ReviewsConnectionString`)，但現在也包含生產環境資料庫連接字串，而不是開發資料庫連接字串。

除非您有更正式的部署工作流程，請以手動方式修改`Web.config`之前部署 （記得要還原成使用開發資料庫的連接字串中使用生產環境資料庫連接字串的檔案之後） 或維護個別`Web.config`時都會上傳到生產環境部署程序的一部分的實際執行環境組態資訊的檔案。

> [!NOTE]
> 如果您不小心將部署`Web.config`檔案，其中包含開發資料庫連接字串，則會有錯誤時在生產環境應用程式會嘗試連接到資料庫。 此錯誤記錄成資訊清單`SqlException`reporting 伺服器找不到或無法存取的訊息。


一旦網站部署至生產環境，請瀏覽生產網站，透過瀏覽器。 您應該查看，並在本機執行資料驅動應用程式時享有與相同的使用者體驗。 當然您造訪的網站上實際執行時的站台是由提供生產環境資料庫伺服器上，而瀏覽的網站，在開發環境中使用的資料庫，在開發過程中。 [圖 3] 所示*教導您自己 ASP.NET 3.5 24 小時內*檢閱從在實際執行環境 （請注意在瀏覽器的網址列中的 URL） 網站的頁面。


[![資料驅動應用程式是現在可在生產環境 ！](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image7.jpg) 

**圖 3**: The Data-Driven 應用程式是現在可在生產環境 ！ ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>將連接字串儲存在個別的組態檔

維護個別的組態資訊，在開發和生產環境的常見技術是有兩個版本`Web.config`： 一個用於開發環境，一個用於生產環境。 在部署階段適當`Web.config`版本可以複製到實際執行環境。 在理想情況下，此程序會自動化的部署工作流程。

而不是維護兩個不同`Web.config`檔案，您可以選擇性地提供更細微的差異。 項目構成`Web.config`接著會在參考的外部組態檔中可以定義檔案`Web.config`檔案。 簡單的說，您可以有一個`Web.config`檔案參考 databaseConnectionStrings.config 檔案，其中將包含的連接字串，這兩種環境應用程式使用，而且絕對是唯一的每個環境。 我發現分隔成不同的檔案不同的組態資訊，提供一個更有條理且更簡單`Web.config`檔案和更清晰地顯示開發和生產環境中設定的差異。

若要使用這項技術，啟動 建立新的資料夾中名為 web 應用程式`ConfigSections`。 接下來，加入這個新的資料夾，名為 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 的兩個檔案。接下來，複製`<connectionStrings>`項目從`Web.config`成 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 的檔案，然後修改中的連接字串databaseConnectionStrings.production.config 檔案，使它指定生產環境資料庫連接字串。 比方說，databaseConnectionStrings.dev.config 檔案應包含剛`<connectionStrings>`參考開發資料庫的連接字串的項目：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample3.xml)]

同樣地，databaseConnectionStrings.production.config 檔案應該只包含`<connectionStrings>`項目，但是具有生產環境資料庫連接字串。

建立一份 databaseConnectionStrings.dev.config 檔案並將它命名為 databaseConnectionStrings.config。

> [!NOTE]
> 您可以命名組態檔以外的 databaseConnectionStrings.config，視 d，例如`connectionStrings.config`或`dbInfo.config`。 不過，請務必將檔案命名為與`.config`延伸模組做為`.config`檔案，根據預設，不提供 ASP.NET 引擎。 如果您其他項目名稱的檔案，例如`connectionStrings.txt`，使用者無法以其瀏覽器指向[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)並檢視檔案的內容 ！


此時`ConfigSections`資料夾應包含三個檔案 （請參閱 圖 4）。 DatabaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 檔案分別包含開發和生產環境中，連接字串。 DatabaseConnectionStrings.config 檔案包含將 web 應用程式在執行階段所使用的連接字串資訊。 因此，databaseConnectionStrings.config 檔案應該相同 databaseConnectionStrings.dev.config 檔案，在開發環境中，而在實際執行上 databaseConnectionStrings.config 檔案應該相同databaseConnectionStrings.production.config。


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image10.jpg) 

**圖 4**: ConfigSections ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-vb/_static/image12.jpg))


我們現在需要指示`Web.config`databaseConnectionStrings.config 檔案用於其連接字串存放區。 開啟 `Web.config` 並使用下列內容取代現有的 `<connectionStrings>` 元素：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-vb/samples/sample4.xml)]

`configSource`屬性會指定實體的路徑，相對於`Web.config`檔案。 如果外部`.config`檔案位於相同的目錄`Web.config`然後將此屬性設定為的檔案名稱`.config`檔案。 如果它在子目錄中，在此情況下使用 databaseConnectionStrings.config，指定使用反斜線分隔的資料夾和檔案名稱，例如 ConfigSections\databaseConnectionStrings.config 的子資料夾。

此項修改，開發和生產環境包含相同`Web.config`檔案。 現在唯一的差別是 databaseConnectionStrings.config 檔案。 DatabaseConnectionStrings.production.config 檔案複製到生產環境，並將它重新命名為 databaseConnectionStrings.config。如果未來有生產環境資料庫連接字串變更您必須對 databaseConnectionStrings.production.config 檔案，然後將該檔案上傳到生產環境，其重新命名為 databaseConnectionStrings.config。

> [!NOTE]
> 您可以指定任何的資訊`Web.config`項目，在個別的檔案，然後使用`configSource`屬性，來參照該檔案從`Web.config`。


## <a name="summary"></a>總結

資料導向應用程式通常會在開發和生產環境中使用不同的資料庫。 因此，儲存在 web 應用程式的組態中的資料庫連接字串必須是每個環境的唯一的。 在本教學課程中，我們討論過如何判斷生產環境資料庫連接字串和維護的兩種環境中唯一的連接字串資訊的方式。

快樂地寫程式 ！

### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [連接字串和組態檔](https://msdn.microsoft.com/library/ms254494.aspx)
- [資料庫組態字串資訊 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [將設定移出 Web.config 檔案](http://www.asp101.com/tips/index.asp?id=154)
- [技術文件&lt;connectionStrings&gt;項目](https://msdn.microsoft.com/library/bf7sd233.aspx)

> [!div class="step-by-step"]
> [上一頁](deploying-a-database-vb.md)
> [下一頁](configuring-a-website-that-uses-application-services-vb.md)
