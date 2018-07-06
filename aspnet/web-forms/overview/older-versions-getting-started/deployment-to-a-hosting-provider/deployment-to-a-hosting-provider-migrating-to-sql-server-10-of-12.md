---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 小時，共 12 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827353"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 小時，共 12
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何從 SQL Server Compact 移轉至 SQL Server。 您可能想要這麼做的其中一個原因是利用 SQL Server Compact 不支援，例如預存程序、 觸發程序、 檢視或複寫的 SQL Server 功能。 如需有關 SQL Server Compact 和 SQL Server 之間的差異的詳細資訊，請參閱 <<c0> [ 部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。

### <a name="sql-server-express-versus-full-sql-server-for-development"></a>SQL Server Express 與完整的 SQL Server 進行開發

一旦您已經決定要升級到 SQL Server，您可能想要用於開發和測試環境中的 SQL Server 或 SQL Server Express。 除了 database engine 功能和工具支援中的差異，還有 SQL Server Compact 和 SQL Server 的其他版本之間的差異提供者實作。 這些差異可能會導致相同的程式碼來產生不同的結果。 因此，如果您決定要保留 SQL Server Compact 為您的開發資料庫，您應該徹底測試您的網站在 SQL Server 或 SQL Server Express 在測試環境中每個部署至生產環境之前。

不同於 SQL Server Compact，SQL Server Express 是基本上相同的資料庫引擎，使用相同的.NET 提供者與完整的 SQL Server。 當您使用 SQL Server Express 進行測試時，您可以確信取得相同的結果，因為您將使用 SQL Server。 您可以使用相同的資料庫工具大部分都與 SQL Server Express，您可以使用 SQL Server (值得注意的例外狀況，正在[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))，而且它支援 SQL Server 的其他功能，例如預存程序、 檢視、 觸發程序，和複寫。 （您通常必須在生產網站中，不過使用完整的 SQL Server。 SQL Server Express 可以執行在共用主控環境中，但原先的設計並非如此，而且許多主機服務提供者不支援。）

如果您使用 Visual Studio 2012，您通常選擇 SQL Server Express LocalDB 的開發環境，因為這是使用 Visual Studio 的預設安裝的項目。 不過，LocalDB 不適用於 IIS，因此您必須針對您的測試環境使用 SQL Server 或 SQL Server Express。

### <a name="combining-databases-versus-keeping-them-separate"></a>結合相對於它們個別的資料庫

Contoso 大學應用程式有兩個 SQL Server Compact 資料庫： 成員資格資料庫 (*aspnet.sdf*) 和應用程式資料庫 (*School.sdf*)。 當您移轉時，您可以在兩個不同的資料庫或單一資料庫，移轉這些資料庫。 您可以將它們結合為簡化您的應用程式資料庫和成員資格資料庫之間的資料庫聯結。 主控方案可能也會提供將它們結合的原因。 例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。 這種情況使用 Cytanium Lite 裝載用於本教學課程中，可讓單一 SQL Server 資料庫的帳戶。

在本教學課程中，您會將兩個資料庫移轉這種方式：

- 將移轉到開發環境中的兩個 LocalDB 資料庫。
- 將移轉至測試環境中的兩個 SQL Server Express 資料庫。
- 將移轉至一個結合完整 SQL Server 資料庫的生產環境中。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="installing-sql-server-express"></a>安裝 SQL Server Express

根據預設，Visual Studio 2010 中，會自動安裝 SQL Server Express，但預設並未安裝 Visual Studio 2012。 若要安裝 SQL Server 2012 Express，請按一下下列連結

- [SQL Server Express 2012](https://www.microsoft.com/download/details.aspx?id=29062)

選擇*繁體中文/x64/SQLEXPR\_x64\_ENU.exe*或是*繁體中文/x86/SQLEXPR\_x86\_ENU.exe*，並在 [安裝精靈] 中，接受預設值設定。 如需安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a>針對測試環境中建立 SQL Server Express 資料庫

下一個步驟是建立的 ASP.NET 成員資格和 School 資料庫。

從**檢視**功能表中，選取**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接**，然後選取**建立新的 SQL Server 資料庫**。

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

中**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**] 方塊中，「 aspnet-測試 」 中**新的資料庫名稱**，然後按一下 [ **[確定]**。

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

請遵循相同的程序，以建立新的 SQL Server Express School 資料庫，名為"學校-Test"。

（您要新增 「 測試 」 這些資料庫名稱，因為稍後您將在其中建立開發環境中，每個資料庫的其他執行個體，您需要能夠區分這兩組的資料庫。）

**伺服器總管**現在會顯示兩個新的資料庫。

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a>建立新的資料庫授與指令碼

當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區的認證存取資料庫。 不過，根據預設，應用程式集區身分識別沒有權限來開啟資料庫。 因此，您必須執行的指令碼，以授與該權限。 在本節中，您會建立指令碼，您將會執行更新版本，以確定應用程式在執行於 IIS 時，可以開啟資料庫。

在方案的*SolutionFiles*中所建立的資料夾[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，建立新的 SQL 檔案命名為*Grant.sql*。 將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> 此指令碼可配合搭配 SQL Server 2008 與 Windows 7 中的 IIS 設定，因為它們會指定在本教學課程。 如果您使用不同版本的 SQL Server 或 Windows，或如果您將 IIS 設定您的電腦上以不同的方式，可能必須使用這個指令碼的變更。 如需有關 SQL Server 指令碼的詳細資訊，請參閱 < [SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。


> [!NOTE] 
> 
> **安全性注意事項**此指令碼會為 db\_使用者在執行階段，也就是您會有在生產環境中存取資料庫的擁有者權限。 在某些情況下，您可能想要指定具有完整的資料庫結構描述更新的權限只部署，並指定執行階段不同的使用者具有權限才能讀取和寫入資料的使用者。 如需詳細資訊，請參閱 <<c0>  **檢閱自動 Web.config 變更的 Code First Migrations**中[部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)。


## <a name="configuring-database-deployment-for-the-test-environment"></a>設定測試環境的資料庫部署

接下來，您將設定 Visual Studio，使它將會執行下列工作的每個資料庫：

- 產生 SQL 指令碼會建立目的地資料庫中的來源資料庫的結構 （資料表、 資料行、 條件約束等）。
- 產生 SQL 指令碼，將來源資料庫的資料插入至目的地資料庫中的資料表。
- 執行產生的指令碼，並建立目的地資料庫中授與指令碼。

開啟**專案屬性**視窗，然後選取**封裝/發行 SQL**  索引標籤。

請確定**主動 （發行）** 或**Release**中選取**組態**下拉式清單。

按一下 **啟用此頁面**。

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

**封裝/發行 SQL**  索引標籤通常停用，因為它會指定舊版的部署方法。 大部分的情況下，您應該設定中的資料庫部署**發佈 Web**精靈。 從 SQL Server Compact 移轉至 SQL Server 或 SQL Server Express 是特殊案例，這個方法會是不錯的選擇。

按一下 **從 Web.config 匯入**。

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

Visual Studio 中的連接字串會尋找*Web.config*檔案中，尋找另一個用於成員資格資料庫，並使用另一個用於 School 資料庫，並將對應至每個連接字串中的資料列**資料庫項目**資料表。 找到的連接字串會針對現有的 SQL Server Compact 資料庫，而且下一個步驟會設定方式和位置來部署這些資料庫。

輸入中的資料庫部署設定**資料庫項目詳細資料**一節**資料庫項目**資料表。 在顯示的設定**資料庫項目詳細資料**兩者中的資料列屬於 區段**資料庫項目**選取資料表，如下圖所示。

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a>成員資格資料庫進行的部署設定

選取  **DefaultConnection 部署**中的資料列**資料庫項目**資料表中，以設定將套用至成員資格資料庫的設定。

在 **目的地資料庫的連接字串**，輸入指向新的 SQL Server Express 成員資格資料庫的連接字串。 您可以取得您需要從連接字串**伺服器總管**。 在**伺服器總管**，展開**資料連接**，然後選取**aspnetTest**資料庫，然後從**屬性**視窗複製**連接字串**值。

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

相同的連接字串在此處重現：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

複製並貼到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

請確定**提取資料及/或從現有的資料庫結構描述**已選取。 這是什麼情況導致自動產生並在目的地資料庫中執行的 SQL 指令碼。

**來源資料庫的連接字串**的值取自*Web.config*檔案，並指向開發 SQL Server Compact 資料庫。 這是來源資料庫將會使用產生指令碼，將稍後在目的地資料庫中執行。 因為您想要部署之資料庫的生產版本，則會將"aspnet Dev.sdf"變更"aspnet Prod.sdf"。

變更**資料庫的指令碼選項**從**僅限結構描述**來**結構描述和資料**，因為您想要複製您的資料 （使用者帳戶和角色），以及資料庫結構。

若要設定執行授與指令碼您稍早建立的部署，您必須將它們新增至**資料庫指令碼**一節。 按一下 **新增指令碼**，然後在**新增 SQL 指令碼**對話方塊方塊中，瀏覽至您儲存授與指令碼的資料夾 （這是包含您的方案檔案的資料夾）。 選取名為的檔案*Grant.sql*，然後按一下**開啟**。

[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)

設定**DefaultConnection 部署**中的資料列**資料庫項目**現在看起來像下圖：

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 資料庫的部署設定

接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定 School 資料庫的部署設定。

您可以使用您稍早用來取得新的 SQL Server Express 資料庫的連接字串的相同方法。 複製此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

請確定**提取資料及/或從現有的資料庫結構描述**已選取。

**來源資料庫的連接字串**的值取自*Web.config*檔案，並指向開發 SQL Server Compact 資料庫。 「 學校 Dev.sdf 」 變成 「 學校-Prod.sdf 「 部署資料庫的實際執行版本。 (您在應用程式永遠不會建立學校 Prod.sdf 檔案\_Data 資料夾，因此您也會將該檔案從測試環境中，複製到應用程式\_稍後 ContosoUniversity 專案資料夾中的 [資料] 資料夾。)

變更**資料庫的指令碼選項**要**結構描述和資料**。

您也想要執行的指令碼，以授與讀取和寫入此資料庫的權限應用程式集區身分識別，因此請新增*Grant.sql*成員資格資料庫的指令碼檔案。

當您完成時，設定**SchoolContext 部署**中的資料列**資料庫項目**看起來像下圖：

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

變更儲存到**封裝/發行 SQL**  索引標籤。

複製*學校 Prod.sdf*檔案*c:\inetpub\wwwroot\ContosoUniversity\App\_Data*資料夾*應用程式\_資料*資料夾中ContosoUniversity 專案中。

### <a name="specifying-transacted-mode-for-the-grant-script"></a>指定授與指令碼的交易的模式

部署程序會產生指令碼來部署資料庫結構描述和資料。 根據預設，這些指令碼會在交易中執行。 不過，自訂的指令碼 （例如授與指令碼中），預設不執行在交易中。 如果在部署程序中混合交易模式，您可能會收到逾時錯誤，在部署期間執行的指令碼時。 在本節中，您可以編輯專案檔才能設定自訂的指令碼，來在交易中執行。

在 **方案總管**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**卸載專案**。

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

然後再次以滑鼠右鍵按一下專案，並選取**編輯 ContosoUniversity.csproj**。

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

Visual Studio 編輯器會顯示您的專案檔的 XML 內容。 請注意，有數個`PropertyGroup`項目。 (在圖中，內容`PropertyGroup`略過項目。)

![專案檔編輯器 視窗](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

第一個不含任何`Condition`屬性，不論套用設定組建組態。 一`PropertyGroup`項目僅適用於偵錯組建組態 (請注意`Condition`屬性)、 一個僅適用於發行組建組態，而且其中一個只適用於測試的組建組態。 內`PropertyGroup`發行 組建組態項目，您會看到`PublishDatabaseSettings`上輸入包含設定項目**封裝/發行 SQL**  索引標籤。沒有`Object`授與指令碼的每個對應的項目指定 （請注意這兩個執行個體的 「 Grant.sql"中）。 根據預設，`Transacted`的屬性`Source`每個授與指令碼的項目是`False`。

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

值變更`Transacted`的屬性`Source`項目`True`。

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

儲存並關閉專案檔，然後在專案上按一下滑鼠右鍵**方案總管**，然後選取**重新載入專案**。

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a>設定連接字串的 Web.Config 轉換

連接字串，針對新的 SQL Express 資料庫，是您在上輸入**封裝/發行 SQL**  索引標籤用於 Web deploy 只在部署期間更新目的地資料庫。 您仍然必須設定*Web.config*轉換，以便在已部署的連接字串*Web.config*檔案指向新的 SQL Server Express 資料庫。 (當您使用**封裝/發行 SQL**索引標籤上，您無法在發行設定檔中設定連接字串。)

開啟*Web.Test.config* ，並取代`connectionStrings`項目`connectionStrings`在下列範例中的項目。 （請確定您僅能複製 connectionStrings 項目，而不周圍的程式碼，以提供內容如下所示）。

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

此程式碼會造成`connectionString`並`providerName`每個屬性`add`要被取代的項目中已部署*Web.config*檔案。 這些連接字串不是您在輸入項目完全相同**封裝/發行 SQL**  索引標籤。設定"MultipleActiveResultSets = True 」 是因為 Entity Framework 和通用提供者必須已加入它們。

## <a name="installing-sql-server-compact"></a>安裝 SQL Server Compact

SqlServerCompact NuGet 套件提供 SQL Server Compact 資料庫引擎的 Contoso 大學應用程式的組件。 但現在它不是應用程式，但 Web Deploy，必須要能夠讀取 SQL Server Compact 資料庫中，若要建立 SQL Server 資料庫中執行的指令碼。 若要啟用 Web Deploy 讀取 SQL Server Compact 資料庫，請安裝 SQL Server Compact 在開發電腦上使用下列連結： [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)。

## <a name="deploying-to-the-test-environment"></a>部署到測試環境

若要發行到測試環境，您必須建立已設定為使用的發行設定檔**封裝/發行 SQL**的資料庫發行的發行設定檔資料庫設定 索引標籤。

首先，刪除現有的 「 測試 」 設定檔。

在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。

選取 [**設定檔**] 索引標籤。

按一下 **管理設定檔**。

選取 **測試**，按一下**移除**，然後按一下**關閉**。

關閉**發佈 Web** 」 精靈來儲存這項變更。

接下來，建立新的 「 測試 」 設定檔，並使用它來發行專案。

在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。

選取 [**設定檔**] 索引標籤。

選取  **&lt;新...&gt;** 從下拉式清單，然後輸入"Test"作為設定檔名稱。

在 **服務 URL**方塊中，輸入*localhost*。

在 **網站/應用程式**方塊中，輸入*Default Web Site/ContosoUniversity*。

在 **目的地 URL**方塊中，輸入`http://localhost/ContosoUniversity/`。

按 [ **下一步**]。

**設定**索引標籤會警告您，**封裝/發行 SQL**已設定 索引標籤，並且讓您覆寫它們，即可啟用新的資料庫發行改進的機會。 針對此部署，您不想覆寫**封裝/發行 SQL**索引標籤上設定，因此只要按一下**下一步**。

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

上的訊息**Preview**  索引標籤表示**沒有任何資料庫選取要發行**，但這只表示，發行設定檔中未設定資料庫發行。

按一下 [發行] 。

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

Visual Studio 部署應用程式和瀏覽器開啟至網站的 [首頁] 頁面中的測試環境。 執行 Instructors 頁面，以查看它會顯示您稍早所見的相同資料。 執行**新增的學生**頁面上，加入新的學生，然後檢視 在新的學生**學生**頁面。 這會確認您可以更新資料庫。 選取 **更新信用額度**（您必須登入） 的頁面確認已部署的成員資格資料庫，而且您可以存取它。

## <a name="creating-a-sql-server-database-for-the-production-environment"></a>針對生產環境中建立 SQL Server 資料庫

既然您已部署至測試環境，您可以設定部署至生產環境就緒。 就如同針對測試環境中，建立將部署到資料庫。 從概觀，您應該還記得，Cytanium Lite 主控方案將只允許單一的 SQL Server 資料庫，因此您會將只有一個資料庫，而不是兩個設定。 所有的資料表和資料的成員資格和學校 SQL Server Compact 資料庫將會部署到生產環境中的一個 SQL Server 資料庫。

移至在 Cytanium 控制台中[ http://panel.cytanium.com ](http://panel.cytanium.com)。 按住滑鼠**資料庫**，然後按一下**SQL Server 2008**。

[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)

在  **SQL Server 2008**頁面上，按一下**Create Database**。

[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)

命名資料庫 [學校]，然後按一下**儲存**。 （頁面會自動新增前置詞"contosou 」，因此有效的名稱會是"contosouSchool 」）。

[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)

在相同頁面上，按一下**Create User**。 在 Cytanium 的伺服器，而不是使用整合式的 Windows 安全性，並讓應用程式集區識別，開啟您的資料庫，您將建立具有權限開啟您的資料庫的使用者。 將使用者的認證加入到生產環境中的連接字串*Web.config*檔案。 在此步驟中，您會建立這些認證。

[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)

中的必要欄位填滿**SQL 使用者屬性**頁面：

- 請輸入"ContosoUniversityUser 」 作為名稱。
- 輸入密碼。
- 選取  **contosouSchool**作為預設的資料庫。
- 選取  **contosouSchool**核取方塊。

[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)

## <a name="configuring-database-deployment-for-the-production-environment"></a>設定生產環境的資料庫部署

現在您已準備好進行設定中的資料庫部署設定**封裝/發行 SQL**索引標籤，如先前所做為測試環境。

開啟**專案屬性**視窗中，選取**封裝/發行 SQL**索引標籤，並確定**作用 （發行）** 或是**版本**是中已選取**組態**下拉式清單。

當您設定每個資料庫的部署設定時，您針對生產和測試環境中所執行的主要差別是在您設定連接字串的方式。 為測試環境中，您輸入不同的目的地資料庫的連接字串，但生產環境的目的地連接字串會適用於這兩個資料庫。 這是因為您要將這兩個資料庫部署到生產環境中的一個資料庫。

### <a name="configuring-deployment-settings-for-the-membership-database"></a>成員資格資料庫進行的部署設定

若要設定將套用至成員資格資料庫的設定，請選取**DefaultConnection 部署**中的資料列**資料庫項目**資料表。

在 **目的地資料庫的連接字串**，輸入指向您剛才建立的新實際執行 SQL Server 資料庫的連接字串。 您可以從您的歡迎電子郵件，以取得連接字串。 電子郵件的相關部分包含下列的範例連接字串：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

取代的三個變數之後，您需要的連接字串看起來像此範例中：

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

複製並貼到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。

請確定**提取資料及/或從現有的資料庫結構描述**是仍選取，而**資料庫的指令碼選項**仍**結構描述和資料**。

在 **資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a>School 資料庫的部署設定

接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定 School 資料庫。

複製到相同的連接字串**目的地資料庫的連接字串**您複製到該欄位的成員資格資料庫。

請確定**提取資料及/或從現有的資料庫結構描述**是仍選取，而**資料庫的指令碼選項**仍**結構描述和資料**。

在 **資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。

變更儲存到**封裝/發行 SQL**  索引標籤。

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a>設定 Web.Config 轉換到生產環境資料庫的連接字串

接下來，您會設定*Web.config*轉換，以便在已部署的連接字串*Web.config*檔以指向新的生產環境資料庫。 您輸入的連接字串**封裝/發行 SQL** Web deploy 使用的索引標籤是應用程式需要使用時，除了新增一個相同`MultipleResultSets`選項。

開啟*Web.Production.config* ，並取代`connectionStrings`項目`connectionStrings`項目，如下列範例所示。 (只複製`connectionStrings`項目，不可說明內容周圍的標記。)

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

您有時會看到建議，告訴您永遠加密中的連接字串*Web.config*檔案。 這可能是適當，如果您已部署到您自己的公司網路上的伺服器。 當您要部署到共用裝載環境中時，不過，您將信任的主控提供者，安全性作法，並不是必要或實際加密連接字串。

## <a name="deploying-to-the-production-environment"></a>部署到生產環境

現在您已準備好要部署到生產環境。 Web Deploy 會讀取您的專案中的 SQL Server Compact 資料庫*應用程式\_資料*資料夾，並重新建立所有的資料表和實際執行 SQL Server 資料庫中的資料。 若要使用發行**封裝/發行 Web**  索引標籤上設定，您必須建立新的發行設定檔，用於生產環境。

首先，刪除現有的生產環境設定檔，如同您先前的測試設定檔。

在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。

選取 [**設定檔**] 索引標籤。

按一下 **管理設定檔**。

選取 **生產**，按一下**移除**，然後按一下**關閉**。

關閉**發佈 Web** 」 精靈來儲存這項變更。

接下來，建立新的實際執行設定檔，並使用它來發行專案。

在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。

選取 [**設定檔**] 索引標籤。

按一下 **匯入**，並選取您稍早下載的.publishsettings 檔案。

在上**連接**索引標籤中變更**目的地 URL**暫存 URL 正確，在此範例是http://contosouniversity.com.vserver01.cytanium.com。

將設定檔的重新命名為實際執行環境。 (選取**設定檔**索引標籤，然後按一下**管理設定檔**若要這麼做)。

關閉**發佈 Web** 」 精靈來儲存您的變更。

在實際的應用程式，在其資料庫已更新在生產環境中，您必須執行兩個額外步驟現在發行之前：

1. 上傳*應用程式\_offline.htm*，如下所示[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。
2. 使用**檔案管理員**Cytanium 控制台的複製功能*aspnet Prod.sdf*並*學校 Prod.sdf*檔案從生產網站到*應用程式\_資料*ContosoUniversity 專案的資料夾。 這可確保您要部署到新的 SQL Server 資料庫的資料，包括最新生產環境網站所做的更新。

在**Web 單鍵發佈**工具列上，請確定**生產**設定檔已選取，然後按一下**發行**。

如果您上傳<em>應用程式\_offline.htm</em>發佈之前，您必須使用<strong>檔案管理員</strong>Cytanium 控制項台中刪除公用程式<em>應用程式\_離線。</em>在測試之前 htm。 您也可以在此同時刪除<em>.sdf</em>從檔案<em>應用程式\_資料</em>資料夾。

您現在可以開啟瀏覽器並移至您的公用網站 URL，以測試應用程式的相同方式部署到測試環境之後。

## <a name="switching-to-sql-server-express-localdb-in-development"></a>切換至 SQL Server Express LocalDB 中開發

如概觀中所述，它通常最好是在您用於測試和生產環境的開發中使用相同的資料庫引擎。 （請記得在開發中使用 SQL Server Express 的優點是資料庫的適用相同開發、 測試和生產環境中）。在本節中您將設定 ContosoUniversity 專案從 Visual Studio 執行應用程式時，使用 SQL Server Express LocalDB。

若要執行此移轉的最簡單方式是讓 Code First 和成員資格系統為您建立這兩個新的開發資料庫。 使用此方法來移轉需要三個步驟：

1. 變更連接字串，以指定新的 SQL Express LocalDB 資料庫。
2. 執行 Web Site Administration Tool，若要建立的系統管理員使用者。 這會建立成員資格資料庫。
3. 您可以使用 Code First 移轉更新資料庫命令來建立並植入的應用程式資料庫。

### <a name="updating-connection-strings-in-the-webconfig-file"></a>正在更新 Web.config 檔案中的連接字串

開啟*Web.config*檔案，並將`connectionStrings`為下列程式碼的項目：

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a>建立成員資格資料庫

在 **方案總管**，選取 ContosoUniversity 專案，然後按一下  **ASP.NET 組態**中**專案**功能表。

選取 [安全性] 索引標籤。

按一下 **建立或管理角色**，然後建立**管理員**角色。

返回 [安全性] 索引標籤。

按一下 **建立使用者**，然後選取**管理員**核取方塊，並建立名為管理員的使用者

關閉**Web Site Administration Tool**。

### <a name="creating-the-school-database"></a>建立 School 資料庫

開啟 [套件管理員主控台] 視窗。

在 **預設專案**下拉式清單中，選取 ContosoUniversity.DAL 專案。

輸入下列命令：

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

Code First 移轉適用於建立資料庫，然後套用 AddBirthDate 移轉時，在初始移轉，然後它會執行 Seed 方法。

按下 ctrl+f5 來執行站台。 當您對測試與生產環境時，執行**新增的學生**頁面上，加入新的學生，然後檢視 在新的學生**學生**頁面。 這會確認已建立 School 資料庫，並初始化，您具有讀取和寫入權限。

選取 **更新信用額度**頁面上，並確認已部署的成員資格資料庫，而且您有存取權限登入。 如果您沒有不會移轉您的使用者帳戶，建立系統管理員帳戶，然後選取**更新信用額度**頁面，確認它是否有效。

## <a name="cleaning-up-sql-server-compact-files"></a>清除 SQL Server Compact 檔案

您不再需要的檔案和 NuGet 套件包含支援 SQL Server Compact。 如果您想要 （此步驟不需要），您可以清除不必要的檔案和參考。

在 [**方案總管] 中**，刪除 *.sdf*從檔案*應用程式\_資料*資料夾和*amd64*和*x86*資料夾，從*bin*資料夾。

在 **方案總管**解決方案 （不是其中一個專案），以滑鼠右鍵按一下，然後按一下 **管理方案的 NuGet 套件**。

在左窗格中**管理 NuGet 套件**對話方塊中，選取**已安裝的套件**。

選取  **EntityFramework.SqlServerCompact**封裝，然後按一下**管理**。

在 [**選取專案**] 對話方塊中，選取這兩個專案。 若要解除安裝這兩個專案中的封裝，請清除這兩個核取方塊，然後按一下 **確定**。

在詢問您是否也解除安裝相依封裝對話方塊中，按一下 [否]。 其中之一是，您必須讓 Entity Framework 套件。

遵循相同的程序來解除安裝**SqlServerCompact**封裝。 (封裝必須解除安裝，依此順序因為**EntityFramework.SqlServerCompact**套件需倚賴**SqlServerCompact**封裝。)

您現在已成功移轉至 SQL Server Express 和 SQL Server 完整版。 在下一個教學課程中您要進行另一個資料庫變更，而且您將了解如何部署資料庫變更，當您測試和生產環境的資料庫使用 SQL Server Express 和 SQL Server 完整版。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)
