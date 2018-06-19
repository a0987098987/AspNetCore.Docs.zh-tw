---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 12 個 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: 25a829f1d3c730c7bb3b174f075ce8163999e482
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892091"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 12 個
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何從 SQL Server Compact 移轉到 SQL Server。 其中一個原因，您可能想要這樣做，是利用 SQL Server Compact 不支援，例如預存程序、 觸發程序、 檢視或複寫的 SQL Server 功能。 如需有關 SQL Server Compact 和 SQL Server 之間的差異的詳細資訊，請參閱[部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express 與完整的 SQL Server 進行開發

一旦您已經決定要升級到 SQL Server，您可能想要在開發和測試環境中使用 SQL Server 或 SQL Server Express。 在工具支援和 database engine 功能的差異，除了有 SQL Server Compact 和其他版本的 SQL Server 之間的差異提供者實作。 這些差異可能造成相同的程式碼來產生不同的結果。 因此，如果您決定要保留 SQL Server Compact 當做開發資料庫，您應該徹底測試您的站台 SQL Server 或 SQL Server Express 中每個部署到生產環境之前在測試環境中。

不同於 SQL Server Compact，SQL Server Express 是基本上相同的資料庫引擎，使用相同的.NET 提供者為 SQL Server 完整版。 當您測試以 SQL Server Express 時，您可以取得相同的結果，您將會與 SQL Server 的信心。 您可以使用 SQL Server Express，您可以使用 SQL Server 的大部分相同的資料庫工具 (值得注意的例外狀況，正在[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))，並且也支援 SQL Server 的其他功能，例如預存程序、 檢視表、 觸發程序，和複寫。 （您通常必須在生產網站，但是使用 SQL Server 完整版。 SQL Server Express 可以在共用代管環境中執行，但它不適用於該作業，且許多主控提供者不支援它。）

如果您使用 Visual Studio 2012，您通常選擇 SQL Server Express LocalDB 開發環境的因為這是預設會隨 Visual Studio 安裝的內容。 不過，LocalDB 無法用在 IIS 中，因此對於您的測試環境，您必須使用 SQL Server 或 SQL Server Express。

### <a name="combining-databases-versus-keeping-them-separate"></a>結合相對於其個別的資料庫

Contoso 大學應用程式有兩個 SQL Server Compact 資料庫： 成員資格資料庫 (*aspnet.sdf*) 及應用程式資料庫 (*School.sdf*)。 當您移轉時，您可以在兩個不同的資料庫或單一資料庫，移轉這些資料庫。 您可以將它們結合為簡化資料庫應用程式資料庫和成員資格資料庫之間的聯結。 主控方案可能也會提供將它們結合的原因。 例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。 這是使用 Cytanium Lite 裝載帳戶用於此教學課程，可讓單一 SQL Server 資料庫的情況。

在本教學課程中，您會將兩個資料庫移轉這種方式：

- 移轉至在開發環境中的兩個 LocalDB 資料庫。
- 移轉至測試環境中的兩個 SQL Server Express 資料庫。
- 移轉至單一結合完整 SQL Server 資料庫在生產環境中。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="installing-sql-server-express"></a>安裝 SQL Server Express

根據預設，Visual Studio 2010 中，使用自動安裝 SQL Server Express，但根據預設並未安裝使用 Visual Studio 2012。 若要安裝 SQL Server 2012 Express，請按一下下列連結

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

選擇*繁體中文/x64/SQLEXPR\_x64\_ENU.exe*或*繁體中文/x86/SQLEXPR\_x86\_ENU.exe*，並在安裝精靈接受預設值設定。 如需有關安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>針對測試環境中建立 SQL Server Express 資料庫

下一個步驟是建立的 ASP.NET 成員資格和 School 資料庫。

從**檢視**功能表中選取**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接**選取**建立新的 SQL Server 資料庫**。

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

在**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**方塊和 「 aspnet-測試 」 中**新的資料庫名稱**方塊，然後按一下  **確定**。

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

請遵循相同的程序來建立新的 SQL Server Express School 資料庫名為 「 學校測試 」。

（您要新增 「 測試 」 至這些資料庫的名稱，因此稍後您將建立額外的執行個體的每個資料庫的開發環境中，您必須能夠區分這兩組的資料庫。）

**伺服器總管**現在會顯示兩個新的資料庫。

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>建立新資料庫的授與指令碼

當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區認證存取資料庫。 不過，根據預設，應用程式集區識別沒有權限來開啟資料庫。 因此，您必須執行指令碼授與該權限。 本節中，您會建立指令碼，您將執行更新版本，才能確定應用程式在執行於 IIS 時，可以開啟資料庫。

在方案的*SolutionFiles*資料夾中建立[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，建立新的 SQL 檔案命名為*Grant.sql*。 將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> 此指令碼被為了運作與 SQL Server 2008 和 Windows 7 中的 IIS 設定，因為它們在本教學課程中指定。 如果您使用不同版本的 SQL Server 或 Windows，或如果您設定 IIS 在您的電腦上以不同的方式，可能必須使用這個指令碼的變更。 如需有關 SQL Server 指令碼的詳細資訊，請參閱[SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。


> [!NOTE] 
> 
> **安全性注意事項**此指令碼會為 db\_使用者在執行階段，您將必須在生產環境中存取資料庫的擁有者權限。 在某些情況下，您可以指定具有完整的資料庫結構描述更新權限只部署，而且指定的執行階段不同的使用者具有權限僅讀取和寫入資料的使用者。 如需詳細資訊，請參閱**檢閱自動 Web.config 變更的程式碼 Code First 移轉**中[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)。


## <a name="configuring-database-deployment-for-the-test-environment"></a>設定資料庫部署在測試環境

接下來，您會設定 Visual Studio，好讓它將會執行下列工作的每個資料庫：

- 產生 SQL 指令碼的目的地資料庫中建立來源資料庫的結構 （資料表、 資料行、 條件約束等）。
- 產生 SQL 指令碼，將來源資料庫的資料插入至目的地資料庫中的資料表。
- 執行產生的指令碼，以及建立目的地資料庫中授與指令碼。

開啟**專案屬性**視窗，然後選取**封裝/發行 SQL**  索引標籤。

請確定**作用中 （發行）** 或**發行**中選取**組態**下拉式清單。

按一下**啟用此頁面**。

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**封裝/發行 SQL**  索引標籤通常停用，因為它會指定舊版的部署方法。 大部分情況下，您應該設定中的資料庫部署**發行 Web**精靈。 從 SQL Server Compact 移轉至 SQL Server 或 SQL Server Express 是特殊案例的這個方法是不錯的選擇。

按一下**匯入從 Web.config**。

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio 中的連接字串尋找*Web.config*檔案，尋找一個成員資格資料庫，一個用於 School 資料庫，並將對應至每個連接字串中的資料列**資料庫項目**資料表。 找到的連接字串不會針對現有的 SQL Server Compact 資料庫，而且下一個步驟來設定如何及在何處部署這些資料庫。

您輸入的資料庫中的部署設定**資料庫項目詳細資料**節**資料庫項目**資料表。 顯示在設定**資料庫項目詳細資料**兩者中的資料列屬於區段**資料庫項目**選取資料表，如下圖所示。

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>設定成員資格資料庫的部署設定

選取**DefaultConnection 部署**中的資料列**資料庫項目**資料表中，以設定將套用至成員資格資料庫的設定。

在**目的地資料庫的連接字串**，輸入指向新的 SQL Server Express 的成員資格資料庫的連接字串。 您可以取得連接字串，您需要從**伺服器總管**。 在**伺服器總管**，依序展開**資料連接**選取**aspnetTest**資料庫，然後從**屬性**視窗複製**連接字串**值。

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

此複製生產相同的連接字串：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

複製並貼上到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

請確定**提取資料和/或從現有的資料庫結構描述**已選取。 這是什麼情況會讓自動產生並在目的地資料庫中執行的 SQL 指令碼。

**來源資料庫的連接字串**值擷取自*Web.config*檔案和開發 SQL Server Compact 資料庫的點。 這是來源資料庫將會用來產生將稍後在目的地資料庫中執行的指令碼。 因為您想要部署之資料庫的生產環境版本時，變更"aspnet Dev.sdf"為"aspnet Prod.sdf"。

變更**資料庫指令碼選項**從**僅限結構描述**至**結構描述和資料**，因為您想要複製資料 （使用者帳戶和角色），以及資料庫結構。

若要設定部署到執行您稍早建立的授與指令碼，您必須將它們加入**資料庫指令碼**> 一節。 按一下**新增指令碼**，然後在**加入 SQL 指令碼**對話方塊方塊中，瀏覽至您儲存授與指令碼的資料夾 （這是您的方案檔所在的資料夾）。 選取的檔名為*Grant.sql*，然後按一下**開啟**。

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

設定**DefaultConnection 部署**中的資料列**資料庫項目**現在看起來像下圖：

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 資料庫進行的部署設定

接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定部署 School 資料庫。

您可以使用您先前用來取得新的 SQL Server Express 資料庫的連接字串相同的方法。 複製到這個連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

請確定**提取資料和/或從現有的資料庫結構描述**已選取。

**來源資料庫的連接字串**值擷取自*Web.config*檔案和開發 SQL Server Compact 資料庫的點。 將"學校 Dev.sdf"變更為"學校-Prod.sdf 「 部署資料庫的生產環境版本。 (您的應用程式中永遠不會建立 School Prod.sdf 檔案\_資料資料夾，讓您將應用程式從測試環境複製該檔案\_稍後 ContosoUniversity 專案資料夾中的 [資料] 資料夾。)

變更**資料庫指令碼選項**至**結構描述和資料**。

您也想要執行指令碼授與讀取和寫入這個資料庫的權限的應用程式集區識別，因此請加入*Grant.sql*成員資格資料庫的指令碼檔案。

當您完成時，設定**SchoolContext 部署**中的資料列**資料庫項目**看起來像下圖：

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

變更儲存到**封裝/發行 SQL**  索引標籤。

複製*學校 Prod.sdf*檔案從*c:\inetpub\wwwroot\ContosoUniversity\App\_資料*資料夾*應用程式\_資料*資料夾中ContosoUniversity 專案。

### <a name="specifying-transacted-mode-for-the-grant-script"></a>指定授與指令碼的交易的模式

部署程序會產生指令碼部署資料庫結構描述和資料。 根據預設，這些指令碼會在交易中執行。 不過，自訂的指令碼 （例如授與指令碼） 根據預設不需要在交易中執行。 如果在部署程序中混合交易模式，您可能會收到逾時錯誤，在部署期間執行的指令碼時。 在本節中，您可以編輯專案檔以便設定在交易中執行的自訂指令碼。

在**方案總管 中**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**卸載專案**。

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

然後，再次以滑鼠右鍵按一下專案並選取**編輯 ContosoUniversity.csproj**。

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

在 Visual Studio 編輯器中顯示的專案檔的 XML 內容。 請注意，有數個`PropertyGroup`項目。 (在圖中，內容`PropertyGroup`略過項目。)

![專案檔案編輯器 視窗](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

第一個，沒有任何`Condition`屬性，不論套用設定組建組態。 一個`PropertyGroup`項目只適用於偵錯組建組態 (請注意`Condition`屬性)、 一個僅適用於發行組建組態，且其中一個只適用於測試的組建組態。 內`PropertyGroup`發行組建組態項目，您會看到`PublishDatabaseSettings`上輸入包含設定項目**封裝/發行 SQL**  索引標籤。沒有`Object`授與指令碼的每一個對應的項目指定 （請注意這兩個執行個體的 「 Grant.sql"中）。 根據預設，`Transacted`屬性`Source`每個授與指令碼的項目是`False`。

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

值變更`Transacted`屬性`Source`元素`True`。

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

儲存並關閉專案檔，然後再以滑鼠右鍵按一下中的專案**方案總管 中**選取**重新載入專案**。

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>為連接字串設定 Web.Config 轉換

您輸入新的 SQL Express 資料庫的連接字串**封裝/發行 SQL**  索引標籤的 Web Deploy 只用於在部署期間更新目的地資料庫。 您仍然必須設定*Web.config*轉換，以便連接字串中已部署*Web.config*檔案指向新的 SQL Server Express 資料庫。 (當您使用**封裝/發行 SQL**索引標籤上，您無法設定連接字串中的發行設定檔。)

開啟*Web.Test.config*和取代`connectionStrings`具有項目`connectionStrings`在下列範例中的項目。 （請確定您僅複製 connectionStrings 項目，而不周圍的程式碼所提供的內容如下所示）。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

此程式碼會造成`connectionString`和`providerName`每個屬性`add`要被取代的項目中已部署*Web.config*檔案。 這些連接字串不是您在輸入項目完全相同**封裝/發行 SQL**  索引標籤。設定"MultipleActiveResultSets = True"因為有需要的 Entity Framework 和通用的提供者已新增至它們。

## <a name="installing-sql-server-compact"></a>安裝 SQL Server Compact

SqlServerCompact NuGet 套件會提供 SQL Server Compact 資料庫引擎 Contoso 大學應用程式的組件。 但現在它不是應用程式，但 Web Deploy 必須能夠讀取 SQL Server Compact 資料庫中，若要建立 SQL Server 資料庫中執行指令碼。 若要啟用 Web Deploy 讀取 SQL Server Compact 資料庫，請安裝 SQL Server Compact 在開發電腦上使用下列連結： [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)。

## <a name="deploying-to-the-test-environment"></a>部署到測試環境

若要發行到測試環境，您必須建立發行設定檔設定為使用**封裝/發行 SQL**資料庫發行，而不發行設定檔資料庫設定 索引標籤。

首先，刪除現有的測試設定檔。

在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發行**。

選取**設定檔** 索引標籤。

按一下**管理設定檔**。

選取**測試**，按一下 **移除**，然後按一下 **關閉**。

關閉**發行 Web**精靈，以儲存這項變更。

接下來，建立新的測試設定檔，並使用它來發行專案。

在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發行**。

選取**設定檔** 索引標籤。

選取**&lt;新...&gt;** 從下拉式清單，並輸入 「 測試 」 做為設定檔名稱。

在**服務 URL**方塊中，輸入*localhost*。

在**網站/應用程式**方塊中，輸入*預設網站/ContosoUniversity*。

在**目的地 URL**方塊中，輸入`http://localhost/ContosoUniversity/`。

按 [ **下一步**]。

**設定** 索引標籤會警告您，**封裝/發行 SQL**已設定 索引標籤，且它會讓您覆寫它們，即可啟用新的資料庫發行改進功能。 此部署，您不想覆寫**封裝/發行 SQL**索引標籤上設定，因此只要按一下**下一步**。

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

上的訊息**預覽**索引標籤會指出**不選取要發行的任何資料庫**，但這僅表示在發行設定檔中未設定資料庫發行。

按一下 [發行] 。

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio 將應用程式部署和測試環境中開啟網站的首頁瀏覽器。 執行講師頁面，以查看它會顯示您稍早所見相同的資料。 執行**新增學生**頁面上，加入新的學生，然後檢視中新的學生**學生**頁面。 這可確認您可以更新資料庫。 選取**更新信用額度**頁面 （您必須登入），並確定已部署的成員資格資料庫，您可以存取它。

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>建立 SQL Server 資料庫的生產環境

現在您已部署到測試環境，您已準備好設定部署至生產環境。 您可以開始與測試環境中，建立要部署到資料庫。 如您從 概觀 Cytanium Lite 主控方案將只允許單一的 SQL Server 資料庫，所以您將設定只能有一個資料庫，而不是兩個。 所有資料表和資料從成員資格和學校 SQL Server Compact 資料庫將會部署至生產環境中的一個 SQL Server 資料庫。

移至的 Cytanium 控制項面板[ http://panel.cytanium.com ](http://panel.cytanium.com)。上按住滑鼠**資料庫**，然後按一下 **SQL Server 2008**。

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

在**SQL Server 2008**頁面上，按一下**Create Database**。

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

將資料庫"學校 」，然後按一下**儲存**。 （[] 頁面上會自動加入前置詞"contosou"，因此有效的名稱將會是"contosouSchool"。）

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

在相同頁面上，按一下  **Create User**。 Cytanium 的伺服器而不是使用 Windows 整合式的安全性，以及讓應用程式集區識別，開啟您的資料庫，您會建立具有權限，開啟您的資料庫使用者。 您會將使用者的認證加入到生產環境中的連接字串*Web.config*檔案。 在此步驟中，您會建立這些認證。

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

必要的欄位中填入**SQL 使用者屬性**頁面：

- 請輸入"ContosoUniversityUser"做為名稱。
- 輸入的密碼。
- 選取**contosouSchool**做為預設資料庫。
- 選取**contosouSchool**核取方塊。

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>設定資料庫的生產環境的部署

現在您已準備好設定中的資料庫部署設定**封裝/發行 SQL**索引標籤上，您可以如同先前測試環境。

開啟**專案屬性**視窗中，選取**封裝/發行 SQL**索引標籤，並確定**作用中 （發行）** 或**發行**是中已選取**組態**下拉式清單。

當您設定每個資料庫的部署設定時，您的生產和測試環境中所執行的主要差別是您如何設定連接字串中。 針對測試環境中，您輸入不同的目的地資料庫的連接字串，但實際執行環境目的地連接字串將會使用相同的兩個資料庫。 這是因為您要部署這兩個資料庫在生產環境中的一個資料庫。

### <a name="configuring-deployment-settings-for-the-membership-database"></a>設定成員資格資料庫的部署設定

若要設定將套用至成員資格資料庫的設定，請選取**DefaultConnection 部署**中的資料列**資料庫項目**資料表。

在**目的地資料庫的連接字串**，輸入指向您剛建立的新實際執行 SQL Server 資料庫的連接字串。 您可以從 歡迎使用電子郵件，以取得連接字串。 電子郵件的相關部分包含下列的範例連接字串：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

取代三個變數之後，您需要的連接字串看起來像本範例中：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

複製並貼上到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

請確定**提取資料和/或從現有的資料庫結構描述**仍然選取，而且**資料庫指令碼選項**仍然是**結構描述和資料**。

在**資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 資料庫進行的部署設定

接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定 School 資料庫。

複製到相同的連接字串**目的地資料庫的連接字串**您複製到該欄位的成員資格資料庫。

請確定**提取資料和/或從現有的資料庫結構描述**仍然選取，而且**資料庫指令碼選項**仍然是**結構描述和資料**。

在**資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。

變更儲存到**封裝/發行 SQL**  索引標籤。

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>設定 Web.Config 轉換的生產資料庫的連接字串

接下來，您將會建立*Web.config*轉換，以便連接字串中已部署*Web.config*以指向新的生產資料庫的檔案。 您輸入的連接字串**封裝/發行 SQL** Web deploy，若要使用的索引標籤是一個應用程式需要使用時，除了新增相同`MultipleResultSets`選項。

開啟*Web.Production.config*和取代`connectionStrings`具有項目`connectionStrings`項目，如下列範例所示。 (只複製`connectionStrings`元素中，不提供給顯示內容周圍的標籤。)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

有時您會看到告知您一律加密連接字串中的建議*Web.config*檔案。 這可能是適用於您已部署到您自己的公司網路上的伺服器。 當您部署至共用裝載環境中時，不過，您所信任的主控提供者，安全性作法並不是必要或適合加密連接字串。

## <a name="deploying-to-the-production-environment"></a>部署到生產環境

您現在已經準備好要部署到生產環境。 Web Deploy 會讀取您的專案中的 SQL Server Compact 資料庫*應用程式\_資料*資料夾並重新建立所有的資料表和實際執行 SQL Server 資料庫中的資料。 若要使用發行**封裝/發行 Web**索引標籤設定，您必須建立實際執行新的發行設定檔。

首先，刪除現有的實際執行設定檔，因為您稍早所執行的測試設定檔。

在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發行**。

選取**設定檔** 索引標籤。

按一下**管理設定檔**。

選取**生產**，按一下 **移除**，然後按一下 **關閉**。

關閉**發行 Web**精靈，以儲存這項變更。

接著，建立新的實際執行設定檔，並使用它來將專案發行。

在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發行**。

選取**設定檔** 索引標籤。

按一下**匯入**，並選取您之前下載的.publishsettings 檔案。

在**連接**索引標籤上，變更**目的地 URL**暫存 URL 正確，在此範例是http://contosouniversity.com.vserver01.cytanium.com。

將設定檔重新命名為生產環境。 (選取**設定檔**索引標籤上，按一下 **管理設定檔**若要這樣做)。

關閉**發行 Web**精靈，以儲存您的變更。

在實際應用中所在資料庫更新生產環境中，您會執行兩個額外的步驟現在發行之前：

1. 上傳*應用程式\_offline.htm*，如下所示[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。
2. 使用**檔案管理員**Cytanium 控制台複製功能*aspnet Prod.sdf*和*學校 Prod.sdf*檔案從生產網站到*應用程式\_資料*ContosoUniversity 專案的資料夾。 這可確保您要部署至新的 SQL Server 資料庫的資料包括您的生產網站所做的最新更新。

在**Web 一個按一下 Publish**工具列上，請確定**生產**設定檔已選取，然後按一下**發行**。

如果您上傳<em>應用程式\_offline.htm</em>在發行之前，您必須使用<strong>檔案管理員</strong>Cytanium 控制台中刪除公用程式<em>應用程式\_離線。</em>htm 才能進行測試。 您也可以同時刪除<em>.sdf</em>檔案從<em>應用程式\_資料</em>資料夾。

您現在可以開啟瀏覽器並移至您的公用網站 URL，測試應用程式部署到測試環境後的相同方式。

## <a name="switching-to-sql-server-express-localdb-in-development"></a>切換至 SQL Server Express LocalDB 中開發

已概觀中所述，它通常最好是在您測試和生產環境中使用的開發中使用相同的資料庫引擎。 （請記住，在開發中使用 SQL Server Express 的優點是資料庫的作用相同開發、 測試和實際執行環境中）。本節中您會設定 ContosoUniversity 專案從 Visual Studio 執行應用程式時，使用 SQL Server Express LocalDB。

若要執行這項移轉最簡單的方式是讓 Code First 和成員資格系統為您建立這兩個新的開發資料庫。 使用這個方法來移轉需要三個步驟：

1. 變更連接字串以指定新的 SQL Express LocalDB 資料庫。
2. 執行網站管理工具來建立系統管理員使用者。 這會建立成員資格資料庫。
3. 使用 Code First 移轉 update-database 命令來建立並植入的應用程式資料庫。

### <a name="updating-connection-strings-in-the-webconfig-file"></a>更新 Web.config 檔案中的連接字串

開啟*Web.config*檔案，並將`connectionStrings`具有下列程式碼項目：

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>建立成員資格資料庫

在**方案總管] 中**選取 ContosoUniversity 專案，然後按一下 [ **ASP.NET 組態**中**專案**功能表。

選取 [安全性] 索引標籤。

按一下**建立或管理角色**，然後再建立**管理員**角色。

返回 [安全性] 索引標籤。

按一下**建立使用者**，然後選取**管理員**核取方塊，並建立使用者，名為系統管理員。

關閉**網站管理工具**。

### <a name="creating-the-school-database"></a>建立 School 資料庫

開啟 [封裝管理員主控台] 視窗。

在**預設專案**下拉式清單中，選取 ContosoUniversity.DAL 專案。

輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First 移轉適用於初始移轉來建立資料庫，然後再套用 AddBirthDate 移轉，然後執行 Seed 方法。

按 Ctrl + F5，以執行站台。 如您所做的測試和實際執行環境，執行**新增學生**頁面上，加入新的學生，然後檢視中新的學生**學生**頁面。 這可確認已建立 School 資料庫，並初始化，以及您具有讀取和寫入權限。

選取**更新信用額度**頁面上，並確認已部署的成員資格資料庫，而且您有存取權限登入。 如果您沒有不會移轉您的使用者帳戶，建立系統管理員帳戶，然後選取**更新信用額度**頁面上，確認它是否有效。

## <a name="cleaning-up-sql-server-compact-files"></a>清除 SQL Server Compact 檔案

您不再需要的檔案和已加入來支援 SQL Server Compact 的 NuGet 封裝。 如果您想 （此步驟不需要），您可以清除不必要的檔案和參考。

在**方案總管 中**，刪除 *.sdf*檔案從*應用程式\_資料*資料夾和*amd64*和*x86*資料夾從*bin*資料夾。

在**方案總管 中**方案 （不是其中一個專案），以滑鼠右鍵按一下，然後按**管理方案的 NuGet 套件**。

在左窗格中**管理 NuGet 封裝**對話方塊中，選取**安裝封裝**。

選取**EntityFramework.SqlServerCompact**封裝，然後按一下 **管理**。

在**選取專案**對話方塊中，選取兩個專案。 若要解除安裝這兩個專案中的封裝，請清除這兩個核取方塊，然後按一下 **確定**。

在詢問您是否也解除安裝相依封裝對話方塊中，按一下 [否]。 下列其中一種是 Entity Framework 封裝，您必須保留。

請遵循相同的程序來解除安裝**SqlServerCompact**封裝。 (封裝必須先解除安裝這個順序因為**EntityFramework.SqlServerCompact**套件相依於**SqlServerCompact**封裝。)

您現在成功移轉至 SQL Server Express 和 SQL Server 完整版。 在下一個教學課程中您要進行另一個資料庫的變更，而且您會看到如何部署資料庫變更，當您測試與實際的資料庫使用 SQL Server Express 和 SQL Server 完整版。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
