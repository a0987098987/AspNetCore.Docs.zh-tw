---
uid: web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
title: "設定生產環境 Web 應用程式使用實際執行資料庫 (C#) |Microsoft 文件"
author: rick-anderson
description: "如先前的教學課程所述，是很常見的開發和生產環境之間差異的組態資訊。 這是 es..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/23/2009
ms.topic: article
ms.assetid: 0177dabd-d888-449f-91b2-24190cf5e842
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deploying-web-site-projects/configuring-the-production-web-application-to-use-the-production-database-cs
msc.type: authoredcontent
ms.openlocfilehash: 21eac6a4d829795f02eeeca5f9870b1ab8132d08
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="configuring-the-production-web-application-to-use-the-production-database-c"></a>設定生產環境 Web 應用程式使用實際執行資料庫 (C#)
====================
由[Scott Mitchell](https://twitter.com/ScottOnWriting)

[下載程式碼](http://download.microsoft.com/download/E/6/F/E6FE3A1F-EE3A-4119-989A-33D1A9F6F6DD/ASPNET_Hosting_Tutorial_08_CS.zip)或[下載 PDF](http://download.microsoft.com/download/C/3/9/C391A649-B357-4A7B-BAA4-48C96871FEA6/aspnet_tutorial08_DBConfig_cs.pdf)

> 如先前的教學課程所述，是很常見的開發和生產環境之間差異的組態資訊。 因為開發和生產環境之間的資料庫連接字串不同，這是資料驅動的 web 應用程式特別有用。 本教學課程中，瀏覽方式來設定要在更多詳細資料中包含適當的連接字串的實際執行環境。


## <a name="introduction"></a>簡介

資料驅動的 web 應用程式通常會在生產環境中使用不同的資料庫，在比開發中。 裝載的 web 主機提供者，然後在本機開發應用程式，開發資料庫一般存在於開發人員電腦上實際執行資料庫裝載在虛擬主機公司的設備的資料庫伺服器上時。 資料驅動的 web 應用程式部署需要將開發資料庫複製到實際執行資料庫伺服器。 在上一個教學課程中我們探討了方式可以完成此步驟。

Web 應用程式使用中的資訊*連接字串*建立與資料庫連線。 連接字串中，通常會儲存在`Web.config`，指定資料庫伺服器名稱、 資料庫、 安全性內容，以及其他資訊的名稱。 Web 應用程式所使用的資料庫相依於是否正在開發或實際執行環境中執行 web 應用程式，因為連接字串必須與不同的兩種環境之間。

是很常見的開發和生產環境之間差異的組態資訊的位置。 *通用組態差異之間開發和生產*教學課程所討論的技術上維護個別的組態資訊，這兩種環境，以及簡短討論之間資料庫連接字串。 本教學課程中，瀏覽方式來設定要在更多詳細資料中包含適當的連接字串的實際執行環境。

## <a name="examining-the-connection-string-information"></a>檢查連接字串資訊

活頁簿檢閱 web 應用程式所使用的連接字串儲存在應用程式的組態檔， `Web.config`。 `Web.config`包含儲存連接字串，命名相似的特殊區段[ &lt;connectionStrings&gt;](https://msdn.microsoft.com/library/bf7sd233.aspx)。 `Web.config`檔案書籍檢閱網站有一個名為此區段中定義的連接字串`ReviewsConnectionString`:

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample1.xml)]

連接字串的資料來源 =。 \SQLEXPRESS。AttachDbFilename = |DataDirectory|\Reviews.mdf;Integrated 安全性 = True;使用者執行個體 = True-組成選項和值的數目，與選項/值組，以分號和每個選項分隔和等號分隔的值。 此連接字串中使用的四個選項如下：

- `Data Source`-指定資料庫伺服器和資料庫伺服器執行個體名稱的位置 （如果有的話）。 值， `.\SQLEXPRESS`，就是資料庫伺服器和執行個體名稱。 期間指定的資料庫伺服器位於與此應用程式; 相同的電腦執行個體名稱是`SQLEXPRESS`。
- `AttachDbFilename`-指定資料庫檔案的位置。 值包含預留位置`|DataDirectory|`，這是解析為 s 的應用程式的完整路徑`App_Data`在執行階段 資料夾。
- `Integrated Security`-布林值，指出是否要連接到資料庫 (false) 或目前 Windows 帳戶認證 (true) 時，使用指定的使用者名稱/密碼。
- `User Instance`-表示是否允許本機電腦上的非系統管理使用者連接和連接到 SQL Server Express Edition 資料庫的 SQL Server Express Edition 的特定組態選項。 請參閱[SQL Server Express 使用者執行個體](https://msdn.microsoft.com/library/ms254504.aspx)如需有關這項設定。
  

允許的連接字串選項取決於您要連接到資料庫以及所使用的 ADO.NET 資料庫提供者。 例如，連接字串連接到 Microsoft SQL Server 資料庫與不同，用來連接到 Oracle 資料庫。 同樣地，連接到 Microsoft SQL Server 資料庫使用 SqlClient 提供者會使用不同的連接字串比時使用的 OLE DB 提供者。

您可以使用像是站台手動建立資料庫連接字串[ConnectionStrings.com](http://www.connectionstrings.com/)為有哪些選項的資源。 不過，比較簡單的方式是將資料庫加入至 Visual Studio 中的 [伺服器總管]，然後抓取 [屬性] 視窗的連接字串。 可讓 s 建構生產資料庫伺服器的連接字串使用後者這種技術。

開啟 Visual Studio，然後瀏覽至 伺服器總管 視窗 （在 Visual Web Developer 中，此視窗稱為資料庫總管）。 以滑鼠右鍵按一下 [資料連線] 選項，並從內容功能表選擇 [加入連接] 選項。 這會顯示在圖 1 所示的精靈。 選擇適當的資料來源，然後按一下 [繼續]。


[![選擇將新的資料庫加入至伺服器總管](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image2.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image1.jpg) 

**圖 1**： 選擇將新的資料庫加入至 [伺服器總管] ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image3.jpg))


接下來，指定各種資料庫連接資訊 （請參閱圖 2）。 當您註冊您的 web 與控管的公司他們應該如何提供資訊連接到資料庫-資料庫伺服器名稱、 資料庫名稱、 使用者名稱和密碼，用來連接到資料庫，並以此類推。 輸入這項資訊之後按一下 [確定] 完成這個精靈，並將資料庫加入至 [伺服器總管] 中。


[![指定資料庫連接資訊](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image5.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image4.jpg) 

**圖 2**： 指定資料庫連接資訊 ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image6.jpg))


生產環境資料庫現在應該會顯示在 [伺服器總管] 中。 從 [伺服器總管] 中選取資料庫，並移至 [屬性] 視窗。 您將找到名為資料庫的連接字串與連接字串的屬性。 假設您在生產環境和 SqlClient 提供者上使用 Microsoft SQL Server 資料庫連接字串看起來應該如下所示：

**資料來源 =*serverName*;初始目錄 =*databaseName*;保存安全性資訊 = True;使用者 ID =*username*;密碼 = * 密碼***

其中*serverName*， *databaseName*， *username*，和*密碼*與資料庫伺服器名稱，資料庫的值名稱，以及使用者名稱和密碼提供給您的 web 主機公司。

## <a name="deploying-the-book-reviews-web-application"></a>活頁簿檢閱 Web 應用程式部署

前述教學課程會逐步執行將開發資料庫複製到生產環境，但沒有未瀏覽資料導向應用程式部署。 此時在實際執行環境包含的資料庫，但使用靜態檢閱書籍檢閱應用程式的版本。 我們需要新的資料驅動應用程式部署至實際執行伺服器，以及更新的組態資訊。

請花一點時間部署資料導向應用程式從開發環境至實際執行環境。 在先前的教學課程中，此程序所涵蓋的詳細資料。 如果您需要重新整理程式，請參閱*部署您使用 FTP 用戶端的網站*或*部署您的網站使用 Visual Studio*教學課程。 您必須確保生產資料庫的連接字串是用在實際執行環境中，這表示替代`Web.config`檔案必須部署。 具體而言，修改此設定`Web.config`檔案的`<connectionStrings>`項目必須包含的實際執行資料庫連接字串，而且看起來應該類似下列：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample2.xml)]

請注意，連接字串中`<connectionStrings>`元素的名稱相同 (`ReviewsConnectionString`)，但現在也包含生產資料庫的連接字串，而不是開發資料庫連接字串。

除非您有更正式的部署工作流程，請以手動方式修改`Web.config`部署 （記住還原成使用開發資料庫連接字串之前使用的實際執行資料庫連接字串的檔案之後） 或維護個別`Web.config`取得過程中上傳到生產環境的部署程序的實際執行環境組態資訊的檔案。

> [!NOTE]
> 如果您不小心將部署`Web.config`包含開發資料庫連接字串，則會有錯誤時在實際執行應用程式嘗試連接到資料庫的檔案。 這個錯誤，卻使用`SqlException`與報告伺服器找不到或無法存取的訊息。


一旦部署網站至生產環境，請瀏覽整個瀏覽器將生產站台。 您應看到及在本機執行資料驅動應用程式時享受與相同的使用者體驗。 當然您造訪的網站上實際執行時的站台是由所提供實際執行資料庫伺服器，而瀏覽的網站，在開發環境中使用的資料庫開發工作。 圖 3 顯示*教導您自己 ASP.NET 3.5 24 小時內*檢閱頁面上，從實際執行環境 （請注意在瀏覽器的網址列中的 URL） 中的網站。


[![資料驅動應用程式是現在提供在實際執行 ！](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image8.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image7.jpg) 

**圖 3**: The Data-Driven 應用程式是現在提供在實際執行 ！ ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image9.jpg))


### <a name="storing-connection-strings-in-a-separate-configuration-file"></a>將連接字串儲存在不同的組態檔

常用的技巧來維護個別的組態資訊的開發和生產環境上是同時擁有兩個版本`Web.config`： 一個用於開發環境，一個用於生產環境。 在部署階段適當`Web.config`版本可以複製到實際執行環境。 在理想情況下，此程序會自動部署工作流程的一部分。

而不是維護兩個不同`Web.config`檔案，您可以選擇性地提供更細微的差異。 構成項目`Web.config`然後中參考外部組態檔中可以定義檔案`Web.config`檔案。 簡單的說，您可以有一個`Web.config`檔案參考 databaseConnectionStrings.config 檔案，其中將包含連接字串的這兩種環境由應用程式，而且是唯一的每個環境。 我發現分隔到不同的檔案不同的組態資訊提供一個更有條理且更簡單`Web.config`檔案和更清楚地列出組態之間的差異開發和生產環境。

若要使用這項技術，啟動名為 web 應用程式中建立新資料夾`ConfigSections`。 接下來，將兩個檔案加入至名為 databaseConnectionStrings.dev.config databaseConnectionStrings.production.config 此新資料夾。接下來，複製`<connectionStrings>`項目從`Web.config`在 databaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 檔案，然後修改中的連接字串databaseConnectionStrings.production.config 檔案，使它指定生產資料庫的連接字串。 例如，databaseConnectionStrings.dev.config 檔案應該包含只`<connectionStrings>`參考開發資料庫的連接字串的項目：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample3.xml)]

同樣地，databaseConnectionStrings.production.config 檔案應該只包含`<connectionStrings>`項目，但有一個具有實際執行資料庫連線字串。

建立一份 databaseConnectionStrings.dev.config 檔案並將它命名為 databaseConnectionStrings.config。

> [!NOTE]
> 您可以命名組態檔以外的 databaseConnectionStrings.config，如您 d，例如`connectionStrings.config`或`dbInfo.config`。 不過，請務必將檔案與`.config`延伸做為`.config`檔案，根據預設，不提供 ASP.NET 引擎。 如果您將檔案命名其他項目，如`connectionStrings.txt`，使用者指出其瀏覽器[www.yoursite.com/ConfigSettings/connectionStrings.txt](http://www.yoursite.com/ConfigSettings/connectionStrings.txt)並檢視檔案的內容 ！


此時`ConfigSections`資料夾應包含三個檔案 （請參閱圖 4）。 DatabaseConnectionStrings.dev.config 和 databaseConnectionStrings.production.config 檔案分別包含開發和生產環境中，連接字串。 DatabaseConnectionStrings.config 檔案包含將 web 應用程式在執行階段所使用的連接字串資訊。 因此，databaseConnectionStrings.config 檔案應該相同 databaseConnectionStrings.dev.config 檔案，在開發環境中，而實際執行 databaseConnectionStrings.config 檔案應該相同databaseConnectionStrings.production.config。


[![ConfigSections](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image11.jpg)](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image10.jpg) 

**圖 4**: C ([按一下以檢視完整大小的影像](configuring-the-production-web-application-to-use-the-production-database-cs/_static/image12.jpg))


我們現在需要指示`Web.config`databaseConnectionStrings.config 檔案用於其連接字串存放區。 開啟`Web.config`並取代現有`<connectionStrings>`具有下列項目：

[!code-xml[Main](configuring-the-production-web-application-to-use-the-production-database-cs/samples/sample4.xml)]

`configSource`屬性指定的實體路徑，相對於`Web.config`檔案。 如果外部`.config`檔案位於相同的目錄`Web.config`然後將此屬性設定為的檔案名稱`.config`檔案。 如果它在子目錄中，使用 databaseConnectionStrings.config，案例指定使用反斜線分隔的資料夾和檔案名稱，例如 ConfigSections\databaseConnectionStrings.config 的子資料夾。

這項修改，以開發和生產環境包含相同`Web.config`檔案。 現在，唯一的差別是 databaseConnectionStrings.config 檔案。 DatabaseConnectionStrings.production.config 檔案複製到生產環境，並將它重新命名為 databaseConnectionStrings.config。如果日後有生產資料庫的連接字串變更您必須對 databaseConnectionStrings.production.config 檔案，然後將該檔案上傳到生產環境，databaseConnectionStrings.config 重新命名。

> [!NOTE]
> 您可以指定任何資訊`Web.config`個別的檔案和使用中的項目`configSource`屬性，來參照該檔案從`Web.config`。


## <a name="summary"></a>總結

資料驅動應用程式通常會在開發和生產環境中使用不同的資料庫。 因此，儲存在 web 應用程式的組態中的資料庫連接字串必須是唯一每個環境。 在此教學課程中我們討論了如何判斷實際執行資料庫連接字串和維護兩個環境中的唯一連接字串資訊的方式。

祝您程式設計 ！

#### <a name="further-reading"></a>進一步閱讀

如需有關在本教學課程所討論的主題的詳細資訊，請參閱下列資源：

- [連接字串和組態檔](https://msdn.microsoft.com/library/ms254494.aspx)
- [資料庫組態字串資訊 @ ConnectionStrings.com](http://www.connectionstrings.com/)
- [設定移出 Web.config 檔案](http://www.asp101.com/tips/index.asp?id=154)
- [技術文件&lt;connectionStrings&gt;項目](https://msdn.microsoft.com/library/bf7sd233.aspx)

>[!div class="step-by-step"]
[上一頁](deploying-a-database-cs.md)
[下一頁](configuring-a-website-that-uses-application-services-cs.md)
