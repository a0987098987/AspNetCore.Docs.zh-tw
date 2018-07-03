---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: 使用 Visual Studio 的 ASP.NET Web 部署： 準備資料庫部署 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: 21b4a924115daff6ee79ce045330c748b58246ee
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37388530"
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 準備資料庫部署
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何取得專案準備好進行資料庫部署。 資料庫結構，以及一些 （並非全部） 的資料在應用程式的兩個資料庫必須部署至測試、 預備及生產環境。

一般而言，當您開發應用程式時，您輸入的測試資料到資料庫，您不想要部署至實際網站。 不過，您可能也有一些要部署的實際執行資料。 在本教學課程中，您將設定 Contoso 大學專案及準備 SQL 指令碼，以便在部署時，會包含正確的資料。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

範例應用程式會使用 SQL Server Express LocalDB。 SQL Server Express 是免費的 SQL Server 版本。 它通常用在開發期間因為它以完整的 SQL Server 版本相同的資料庫引擎為基礎。 您可以使用 SQL Server Express 進行測試，並確保應用程式的行為相同的實際執行環境，有一些例外狀況之功能的 SQL Server 版本而有所不同。

LocalDB 是一種特殊的執行模式的 SQL Server Express，可讓您使用資料庫作為 *.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用 *.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。

SQL Server Express 不是常用的生產 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是在搭配 IIS 運作。

在 Visual Studio 2012 中，使用 Visual Studio 的預設會安裝 LocalDB。 在 Visual Studio 2010 和舊版中，在預設情況下使用 Visual Studio，安裝 SQL Server Express （不含 LocalDB)也就是為什麼您安裝它做為其中一個的先決條件[這個系列的第一個教學課程](introduction.md)。

如需有關 SQL Server 版本的詳細資訊，包括 LocalDB，請參閱下列資源[使用 SQL Server 資料庫](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver)。

## <a name="entity-framework-and-universal-providers"></a>Entity Framework 和通用提供者

資料庫存取 Contoso 大學應用程式需要下列軟體必須與應用程式部署，因為它不會包含在.NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （可讓 ASP.NET 成員資格系統，才能使用 Azure SQL Database）
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

因為此軟體包含在 NuGet 套件中，專案已設定，以便必要的組件會部署專案。 （連結指向這些套件，可能會比在您下載本教學課程中將入門專案中安裝的項目目前的版本。）

如果您要部署而不是 Azure 的協力廠商裝載提供者，請確定您使用 Entity Framework 5.0 或更新版本。 舊版的 Code First 移轉需要完全信任，和大部分的主機服務提供者會在中度信任執行您的應用程式。 如需度信任的詳細資訊，請參閱[部署至 IIS 作為測試環境](deploying-to-iis.md)教學課程。

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>針對應用程式的資料庫部署設定 Code First 移轉

Contoso 大學應用程式資料庫由 Code First，您將使用 Code First 移轉來部署。 如需使用 Code First 移轉的資料庫部署的概觀，請參閱 <<c0> [ 這個系列的第一個教學課程](introduction.md)。

當您部署應用程式資料庫時，通常您不只要部署您的開發資料庫，所有的資料到生產環境，因為大部分的資料可能有僅用於測試目的。 比方說，在測試資料庫中的學生名稱是虛構的。 相反地，您通常無法部署資料庫結構中沒有資料的根本。 測試資料庫中的資料部分可能實際資料，而且時，必須有使用者使用應用程式會開始。 例如，您的資料庫可能包含有效的等級值或實際的部門名稱的資料表。

若要模擬這個常見的案例，您將設定 Code First 移轉`Seed`只有您想要在生產環境中的資料插入資料庫的方法。 這`Seed`方法不應插入測試資料，因為它會在生產環境中執行 Code First 在生產環境中建立資料庫之後。

在舊版的 Code First 移轉發行之前，它是很常見的`Seed`方法來插入測試資料也，因為資料庫已完全刪除並從頭開始重新建立在開發期間每次模型變更。 使用 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料`Seed`方法不需要。 您所下載的專案會使用的方法包括中的所有資料`Seed`初始設定式類別的方法。 在本教學課程中您將會停用該初始設定式類別和`enable Migrations. Then you'll update the `種子 ' 方法中的移轉設定類別，使它會插入只有您想要在生產環境中要插入的資料。

下圖說明應用程式資料庫的結構描述：

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

這些教學課程中，您將會假設`Student`和`Enrollment`站台第一次部署時，資料表應該是空的。 其他資料表包含對應用程式上線時預先載入的資料。

### <a name="disable-the-initializer"></a>停用初始設定式

因為您將使用 Code First 移轉，您不必使用`DropCreateDatabaseIfModelChanges`Code First 初始設定式。 此初始設定式的程式碼位於*SchoolInitializer.cs* ContosoUniversity.DAL 專案檔中的。 中的設定`appSettings`項目*Web.config*檔案會導致每次應用程式會嘗試將第一次存取資料庫時執行此初始設定式：

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

開啟應用程式*Web.config*檔案並移除或註解化`add`指定 Code First 初始設定式類別的項目。 `appSettings`元素現在看起來像這樣：

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> 指定初始設定式類別的另一種方式是藉由呼叫這麼`Database.SetInitializer`中`Application_Start`方法中的*Global.asax*檔案。 如果您要啟用移轉的專案會使用該方法來指定初始設定式中，移除該程式碼行。


> [!NOTE]
> 如果您使用 Visual Studio 2013，新增下列的步驟，在步驟 2 和 3 之間: (a) 在 PMC 輸入 「 更新套件 entityframework-6.1.1 版 」 來取得目前的 EF 版本。 然後 (b) 建置專案，以取得組建錯誤清單並加以修正。 刪除不再存在，以滑鼠右鍵按一下，按一下 加入 using 陳述式需要它們的地方，解析的命名空間的 using 陳述式，並將出現的 System.Data.EntityState 變更 System.Data.Entity.EntityState。


### <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

1. 請確定 ContosoUniversity 專案 (不 ContosoUniversity.DAL) 設定為啟始專案。 在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後選取**設定為啟始專案**。 在 啟始專案，若要尋找的資料庫連接字串看起來 code First 移轉。
2. 從**工具**功能表上，按一下**程式庫套件管理員**(或**NuGet 套件管理員**)，然後**Package Manager Console**。

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. 在頂端**Package Manager Console**視窗中選取做為預設專案，然後在 ContosoUniversity.DAL`PM>`提示字元中輸入 「 啟用移轉 」。

    ![啟用移轉命令](preparing-databases/_static/image4.png)

    (如果您收到錯誤指出*啟用移轉*無法辨識命令中，輸入命令*更新套件 EntityFramework-重新安裝*並再試一次。)

    此命令會建立*移轉*ContosoUniversity.DAL 專案中，它的資料夾放在該資料夾中兩個檔案： *Configuration.cs*檔案可供您設定移轉，以及*InitialCreate.cs*第一個移轉建立資料庫的檔案。

    ![Migrations 資料夾](preparing-databases/_static/image5.png)

    您選取中的 DAL 專案**預設專案**下拉式清單中的**Package Manager Console**因為`enable-migrations`必須包含程式碼第一個專案中執行命令內容類別。 類別庫專案中該類別時，Code First Migrations 會尋找方案的啟始專案中的資料庫連接字串。 在 ContosoUniversity 解決方案中，web 專案已設定為啟始專案。 如果您不想要指定具有連接字串為啟始專案在 Visual Studio 中的專案，您可以在 PowerShell 命令中指定啟始專案。 若要查看命令語法，請輸入命令`get-help enable-migrations`。

    `enable-migrations`命令會自動建立第一次移轉，因為資料庫已經存在。 替代方法是能夠建立資料庫的移轉。 若要這樣做，請使用**伺服器總管**或是**SQL Server 物件總管**來刪除 ContosoUniversity 資料庫，才能啟用移轉。 啟用移轉後，建立第一次移轉手動輸入此命令"為新增移轉 InitialCreate"。 您接著可以藉由輸入命令 「 更新-資料庫 」 建立資料庫。

### <a name="set-up-the-seed-method"></a>設定種子方法

本教學課程中您會加入固定的資料，加上程式碼`Seed`方法的 Code First 移轉`Configuration`類別。 程式碼會呼叫第一個移轉`Seed`方法之後每個移轉。

因為`Seed`方法會執行每個移轉之後，已經有資料的資料表中第一次移轉後。 若要處理這種情況，您將使用`AddOrUpdate`方法來更新具有已插入，或將其插入，如果它們尚不存在的資料列。 `AddOrUpdate`方法可能不到您的案例的最佳選擇。 如需詳細資訊，請參閱 <<c0> [ 請小心使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 的部落格上。

1. 開啟*Configuration.cs*檔案，並取代中的註解`Seed`為下列程式碼的方法：

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. 若要參考`List`已在其下的紅色曲線，因為您不需要`using`其命名空間陳述式尚未。 以滑鼠右鍵按一下其中一個的執行個體`List`，按一下 **解決**，然後按一下**using System.Collections.Generic**。

    ![解決與使用陳述式](preparing-databases/_static/image6.png)

    此功能表選取項目新增下列程式碼`using`陳述式會靠近頂端的檔案。

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. 按 CTRL-SHIFT-B 來建置專案。

專案現在已準備好部署*ContosoUniversity*資料庫。 在您部署應用程式中，第一次您執行它，並巡覽至頁面存取資料庫之後, 第一個程式碼會建立資料庫，並執行此程序`Seed`方法。

> [!NOTE]
> 將程式碼加入`Seed`方法是，您可以將固定的資料插入資料庫的眾多方法之一。 替代方式是將程式碼加入`Up`和`Down`移轉的每個類別的方法。 `Up`和`Down`方法包含實作資料庫變更的程式碼。 您會看到範例在[部署資料庫更新](deploying-a-database-update.md)教學課程。
> 
> 您也可以撰寫使用執行 SQL 陳述式的程式碼`Sql`方法。 比方說，如果您已將預算資料行加入將 Department 資料表，而且想要移轉過程中初始化所有部門預算 $ 1，000.00，您可以新增的程式碼的下一行`Up`的移轉方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>建立成員資格資料庫部署的指令碼

Contoso 大學應用程式會使用 ASP.NET 成員資格系統和表單驗證來驗證和授權使用者。 **更新信用額度**頁面可供存取，只有以系統管理員角色中的使用者。

執行應用程式，然後按一下**課程**，然後按一下**更新信用額度**。

![按一下 更新信用額度](preparing-databases/_static/image7.png)

**登入**頁面隨即出現，因為**更新信用額度**頁面需要系統管理權限。

請輸入*系統管理員*作為使用者名稱並*devpwd*作為密碼，然後按一下**登入**。

![登入頁面](preparing-databases/_static/image8.png)

**更新信用額度**頁面隨即出現。

![更新學分數頁面](preparing-databases/_static/image9.png)

使用者和角色的資訊位於*aspnet ContosoUniversity*所指定的資料庫**DefaultConnection**中的連接字串*Web.config*檔案。

此資料庫不是由 Entity Framework Code First，管理，因此您無法使用移轉來部署它。 您將使用的 dbDacFx 提供者來部署資料庫結構描述，而且您會設定執行指令碼會將初始資料插入資料庫資料表的發行設定檔。

> [!NOTE]
> 使用 Visual Studio 2013 引進了新的 ASP.NET 成員資格系統，（現在稱為 「 ASP.NET 身分識別 」）。 新的系統可讓您將應用程式和成員資格資料表放在相同的資料庫，您可以使用 Code First 移轉來部署兩者。 範例應用程式會使用舊版的 ASP.NET 成員資格系統，無法部署使用 Code First 移轉。 此成員資格資料庫部署的程序也套用至您的應用程式要部署 SQL Server 資料庫不是由 Entity Framework Code First 建立的其他案例。


這裡，您通常不想您在開發的生產環境中相同的資料。 當您第一次部署站台時，常會排除大部分或所有您建立用於測試的使用者帳戶。 因此，所下載的專案有兩個成員資格資料庫： *aspnet ContosoUniversity.mdf*與開發使用者並*aspnet 集 ContosoUniversity-Prod.mdf*與生產環境的使用者。 本教學課程中的使用者名稱會是這兩個資料庫中的相同：*系統管理員*並*nonadmin*。 這兩個使用者有密碼*devpwd*開發資料庫中並*prodpwd*實際執行資料庫中。

您會將開發使用者部署到測試環境和生產使用者，來預備和生產環境。 若要這樣做，您將建立兩個 SQL 指令碼，在本教學課程中，一個用於開發，一個用於生產環境，並在稍後的教學課程中，您將設定發行程序，並執行。

> [!NOTE]
> 成員資格資料庫會儲存帳戶密碼的雜湊。 若要部署到另一個從一部電腦帳戶，您必須確定雜湊的常式不產生在目的地伺服器上的不同雜湊，比在來源電腦上。 它們會產生相同的雜湊當您使用 ASP.NET Universal Providers，只要您不要變更預設的演算法。 預設的演算法為 HMACSHA256，而且在指定**驗證**屬性**[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)** Web.config 檔案中的項目。


使用 SQL Server Management Studio (SSMS)，或使用協力廠商工具，您可以手動建立資料部署指令碼。 本教學課程的其餘部分將說明如何在 SSMS 中，但如果您不想安裝和使用 SSMS 您可以從專案的完整版取得的指令碼，並略過的區段，您將它們儲存在方案資料夾。

若要安裝 SSMS，請安裝從[Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)依序按一下[ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe)或[ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)。 如果您選擇了錯誤，它會為您的系統無法安裝，而且您可以嘗試另一個。

（請注意，這是 600 mb 下載。 可能需要很長的時間安裝且您需要重新啟動您的電腦。)

在 SQL Server 安裝中心 的第一個頁面上，按一下**新的 SQL Server 獨立安裝或將功能加入到現有安裝**，並遵循指示接受預設選項。

### <a name="create-the-development-database-script"></a>建立開發資料庫指令碼

1. SSMS 中執行。
2. 在 **連接到伺服器**對話方塊方塊中，輸入 *(localdb) \v11.0*做為**伺服器名稱**，保留**驗證**設**Windows 驗證**，然後按一下**Connect**。

    ![SSMS 連線到伺服器](preparing-databases/_static/image10.png)
3. 中**物件總管 中** 視窗中，展開**資料庫**，以滑鼠右鍵按一下**aspnet ContosoUniversity**，按一下 **工作**，然後按一下**產生指令碼**。

    ![SSMS 產生指令碼](preparing-databases/_static/image11.png)
4. 在 [**產生和發佈指令碼**] 對話方塊中，按一下**設定指令碼編寫選項**。

    您可以略過**選擇物件**步驟，因為預設值是**編寫整個資料庫和所有資料庫物件的指令碼**，這是您想要。
5. 按一下 [ **進階**]。

    ![SSMS 指令碼編寫選項](preparing-databases/_static/image12.png)
6. 在**進階指令碼編寫選項** 對話方塊中，向下捲動至**要指令碼的資料類型**，然後按一下**僅限資料**下拉式清單中的選項。
7. 變更**指令碼 USE DATABASE**要**False**。 使用陳述式不是有效的 Azure SQL Database，並不需要在測試環境中的部署至 SQL Server Express。

    ![SSMS 資料僅限指令碼，不使用陳述式](preparing-databases/_static/image13.png)
8. 按一下 [確定 **Deploying Office Solutions**]。
9. 在 [**產生和發佈指令碼**] 對話方塊中，**檔案名稱**方塊可讓您指定要建立指令碼。 將路徑變更為您的方案資料夾 （ContosoUniversity.sln 檔案的資料夾） 和檔案名稱以*aspnet-資料-dev.sql*。
10. 按一下 **下一步**以前往**摘要**索引標籤，然後再按**下一步**建立指令碼。

    ![建立 SSMS 指令碼](preparing-databases/_static/image14.png)
11. 按一下 [ **完成**]。

### <a name="create-the-production-database-script"></a>建立實際執行資料庫指令碼

您還沒有執行專案與生產資料庫，因為它未連結尚未至 LocalDB 執行個體。 因此，您必須先附加資料庫。

1. 在 SSMS**物件總管**，以滑鼠右鍵按一下**資料庫**然後按一下**附加**。

    ![SSMS 附加](preparing-databases/_static/image15.png)
2. 在**附加資料庫** 對話方塊中，按一下**新增**，然後瀏覽*aspnet 集 ContosoUniversity-Prod.mdf*中的檔案*應用程式\_資料*資料夾。

     ![要附加的 SSMS 將.mdf 檔案](preparing-databases/_static/image16.png)
3. 按一下 [確定 **Deploying Office Solutions**]。
4. 請遵循您稍早用來建立實際執行檔案的指令碼相同的程序。 將檔案命名為指令碼*aspnet-資料-prod.sql*。

## <a name="summary"></a>總結

這兩個資料庫現在已準備好進行部署，您的方案資料夾中有兩個資料部署指令碼。

![資料部署指令碼](preparing-databases/_static/image17.png)

在下列教學課程中，您設定會影響部署的專案設定，而且設定好自動*Web.config*檔的設定，必須在已部署的應用程式中不同的轉換。

## <a name="more-information"></a>更多資訊

如需 NuGet 的詳細資訊，請參閱[nuget 管理專案程式庫](https://msdn.microsoft.com/magazine/hh547106.aspx)並[NuGet 文件](http://docs.nuget.org/docs/start-here/overview)。 如果您不想要使用 NuGet，您必須了解如何分析以判斷其用途在安裝時 NuGet 套件。 (例如，它可能會設定*Web.config*轉換設定 PowerShell 指令碼，來執行在建置時間等等。)若要深入了解 NuGet 的運作方式，請參閱[建立及發佈套件](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)並[組態檔和原始程式碼轉換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

> [!div class="step-by-step"]
> [上一頁](introduction.md)
> [下一頁](web-config-transformations.md)
