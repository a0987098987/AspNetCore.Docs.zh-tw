---
uid: web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
title: "使用 Visual Studio 的 ASP.NET Web 部署： 準備將資料庫部署 |Microsoft 文件"
author: tdykstra
description: "此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: ae4def81-fa37-4883-a13e-d9896cbf6c36
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/preparing-databases
msc.type: authoredcontent
ms.openlocfilehash: caa79725ede320c4bd3e87ac246966c57175eb8e
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="aspnet-web-deployment-using-visual-studio-preparing-for-database-deployment"></a>使用 Visual Studio 的 ASP.NET Web 部署： 準備部署資料庫
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何取得專案供部署資料庫。 資料庫結構以及某些 （並非全部） 的資料，應用程式的兩個資料庫必須部署到測試、 預備及實際執行環境。

一般而言，當您開發應用程式時，您輸入測試資料的資料庫，您不想要部署到即時網站。 不過，您可能也有一些要部署的實際執行資料。 在此教學課程中，將 Contoso 大學專案設定和準備的 SQL 指令碼，以便在部署時，會包含正確的資料。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="sql-server-express-localdb"></a>SQL Server Express LocalDB

範例應用程式會使用 SQL Server Express LocalDB。 SQL Server Express 是免費的 SQL Server 版本。 它通常用在開發期間因為它根據相同的資料庫引擎的 SQL Server 完整版本。 您可以測試以 SQL Server Express，及保證，應用程式會將實際執行環境，SQL Server 版本而異的功能有一些例外狀況相同行為。

LocalDB 是一種特殊的執行模式的 SQL Server Express，可讓您能夠使用資料庫，做為*.mdf*檔案。 一般而言，LocalDB 資料庫檔案會保留在*應用程式\_資料*web 專案的資料夾。 在 SQL Server Express 使用者執行個體功能也可讓您能夠使用*.mdf*檔案，但使用者執行個體功能已被取代; 因此，建議使用的 LocalDB *.mdf*檔案。

一般 SQL Server Express 不用於生產環境 web 應用程式。 LocalDB 尤其不建議用於生產環境 web 應用程式因為它不是使用 IIS。

在 Visual Studio 2012 中，使用 Visual Studio 的預設會安裝 LocalDB。 在 Visual Studio 2010 和舊版中，SQL Server Express （而不要使用 LocalDB) 預設會安裝 Visual studio;也就是為什麼您當做安裝的必要條件的其中一個[此系列的第一個教學課程](introduction.md)。

如需有關 SQL Server 版本的詳細資訊，包括 LocalDB，請參閱下列資源[使用 SQL Server 資料庫](../../../../whitepapers/aspnet-data-access-content-map.md#sqlserver)。

## <a name="entity-framework-and-universal-providers"></a>Entity Framework 和通用的提供者

進行資料庫存取 Contoso 大學應用程式需要下列軟體必須與應用程式部署，因為它不會包含在.NET Framework:

- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （啟用 ASP.NET 成員資格系統，才能使用 Azure SQL Database）
- [Entity Framework](https://msdn.microsoft.com/library/gg696172.aspx)

此軟體包含在 NuGet 封裝，因為專案已設定，以便必要的組件會部署專案。 （連結指向這些封裝，可能會比功能會安裝在您下載此教學課程的起始專案的目前版本。）

如果您要部署到協力廠商主機服務提供者而不是 Azure，請確定您使用 Entity Framework 5.0 或更新版本。 舊版的 Code First 移轉需要完全信任，和大部分的主機服務提供者中度信任中執行應用程式。 如需度信任的詳細資訊，請參閱[部署至 IIS 做為測試環境](deploying-to-iis.md)教學課程。

## <a name="configure-code-first-migrations-for-application-database-deployment"></a>應用程式資料庫部署設定 Code First 移轉

Code First，管理 Contoso 大學應用程式資料庫，您將使用 Code First 移轉來部署。 如需使用 Code First 移轉的資料庫部署的概觀，請參閱[此系列的第一個教學課程](introduction.md)。

當您部署應用程式資料庫時，通常您不只會部署您開發的資料庫使用的所有資料至生產環境，因為大部分的資料可能有僅用於測試目的。 例如，測試資料庫中的學生名稱是虛構。 相反地，您通常無法部署資料庫結構與任何資料完全。 測試資料庫中的資料部分可能實際資料，而且必須有使用者使用應用程式會開始時。 例如，您的資料庫可能有包含有效的等級值或實際的部門名稱的資料表。

若要模擬這個常見案例中，您需要設定 Code First 移轉`Seed`只有您想要有在生產環境中的資料插入資料庫的方法。 這`Seed`方法不應該插入測試資料，因為它會在生產環境中執行程式碼第一次在生產環境中建立資料庫之後。

在舊版的 Code First 移轉發行之前，它是很常見的`Seed`方法來插入測試資料此外，因為資料庫已完全刪除並從頭開始重新建立每一次在開發期間的模型變更。 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料`Seed`方法不是必要。 您所下載的專案使用的方法中的所有資料，其中包括`Seed`初始設定式類別的方法。 本教學課程中您將會停用該初始設定式的類別和`enable Migrations. Then you'll update the `種子 ' 方法中的移轉組態類別，因此它會插入只有您想要在生產環境中要插入的資料。

下圖說明應用程式資料庫的結構描述：

[![School_database_diagram](preparing-databases/_static/image2.png)](preparing-databases/_static/image1.png)

如需這些教學課程中，您會假設`Student`和`Enrollment`網站第一次部署時，資料表應該是空的。 其他資料表包含資料，必須預先載入應用程式上線。

### <a name="disable-the-initializer"></a>停用初始設定式

因為您會使用 Code First 移轉，您不必使用`DropCreateDatabaseIfModelChanges`Code First 初始設定式。 這個初始設定式的程式碼位於*SchoolInitializer.cs* ContosoUniversity.DAL 專案中的檔案。 中的設定`appSettings`元素*Web.config*檔案造成此初始設定式來執行應用程式第一次存取資料庫時：

[!code-xml[Main](preparing-databases/samples/sample1.xml?highlight=3)]

開啟應用程式*Web.config*檔案並移除或註解`add`項目，指定第一個程式碼的初始設定式類別。 `appSettings`項目看起來像這樣：

[!code-xml[Main](preparing-databases/samples/sample2.xml)]

> [!NOTE]
> 指定初始設定式類別的另一種方式是藉由呼叫來進行`Database.SetInitializer`中`Application_Start`方法中的*Global.asax*檔案。 如果您要啟用移轉的專案中，會使用該方法指定初始設定式，移除該程式碼行。


> [!NOTE]
> 如果您使用 Visual Studio 2013，加入在步驟 2 和 3 之間的下列步驟: (a) 中 PMC 輸入 「 更新套件 entityframework-版本 6.1.1"以取得目前的 EF 版本。 然後 (b) 建置專案來取得組建錯誤清單並加以修正。 刪除不再存在，以滑鼠右鍵按一下，然後按一下 [解決]，加入 using 陳述式所需要之位置的命名空間的 using 陳述式，並將出現的 System.Data.EntityState 變更 System.Data.Entity.EntityState。


### <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

1. 請確定 ContosoUniversity 專案 (不 ContosoUniversity.DAL) 設定為啟始專案。 在**方案總管 中**，ContosoUniversity 專案上按一下滑鼠右鍵，然後選取**設定為啟始專案**。 Code First 移轉會在啟始專案，若要尋找的資料庫連接字串中尋找。
2. 從**工具**功能表上，按一下 **程式庫套件管理員**(或**NuGet 套件管理員**)，然後**Package Manager Console**。

    ![Selecting_Package_Manager_Console](preparing-databases/_static/image3.png)
3. 在頂端**Package Manager Console**視窗選取 ContosoUniversity.DAL 做為預設專案，然後在`PM>`提示字元中輸入 「 啟用移轉 」。

    ![enable-migrations 命令](preparing-databases/_static/image4.png)

    (如果您收到錯誤指出*啟用移轉*無法辨識命令中，輸入命令*更新套件 EntityFramework-重新安裝*並再試一次。)

    此命令會建立*移轉*資料夾 ContosoUniversity.DAL 專案，並將該資料夾中兩個檔案： *configuration.cs 中*檔案可讓您設定移轉，以及*InitialCreate.cs*第一個移轉建立資料庫的檔案。

    ![Migrations 資料夾](preparing-databases/_static/image5.png)

    您選取中的 DAL 專案**預設專案**的下拉式清單**Package Manager Console**因為`enable-migrations`必須包含程式碼第一個專案中執行命令內容類別。 該類別是在類別庫專案，Code First 移轉會尋找資料庫連接字串中方案的啟始專案。 在 ContosoUniversity 解決方案中，web 專案已設定為啟始專案。 如果您不想指定為啟始專案，Visual Studio 中具有的連線字串的專案，您可以在 PowerShell 命令中指定啟始專案。 若要查看命令語法，請輸入命令`get-help enable-migrations`。

    `enable-migrations`命令會自動建立第一次移轉，因為資料庫已經存在。 替代方法是能夠建立資料庫的移轉。 若要這樣做，請使用**伺服器總管**或**SQL Server 物件總管**刪除 ContosoUniversity 資料庫，才能啟用移轉。 啟用移轉之後，第一次移轉手動建立輸入 「 新增移轉 InitialCreate"命令。 您可以輸入命令 [更新的資料庫]，然後建立資料庫。

### <a name="set-up-the-seed-method"></a>設定種子方法

此教學課程中您會加入固定的資料加上程式碼`Seed`方法的 Code First 移轉`Configuration`類別。 Code First 移轉呼叫`Seed`每個移轉之後的方法。

因為`Seed`方法會執行每個移轉之後，已經有資料的資料表中第一次移轉後。 若要處理這種情況下，您將使用`AddOrUpdate`方法，以更新已插入，或將其插入，如果它們尚不存在的資料列。 `AddOrUpdate`方法可能不是您的案例的最佳選擇。 如需詳細資訊，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 部落格上。

1. 開啟*configuration.cs 中*檔案，然後取代中的註解`Seed`方法取代下列程式碼：

    [!code-csharp[Main](preparing-databases/samples/sample3.cs)]
2. 若要參考`List`有其下的紅色曲線，因為您沒有`using`其命名空間陳述式尚未。 以滑鼠右鍵按一下其中一個執行個體的`List`按一下**解決**，然後按一下  **using System.Collections.Generic**。

    ![解決與使用陳述式](preparing-databases/_static/image6.png)

    此功能表選取項目加入下列程式碼，`using`陳述式頂端附近的檔案。

    [!code-csharp[Main](preparing-databases/samples/sample4.cs)]
3. 按 CTRL-SHIFT-B 以建置專案。

專案已經準備好要部署*ContosoUniversity*資料庫。 部署應用程式中，第一次執行，並巡覽至頁面可存取資料庫之後, 第一個程式碼會建立資料庫，並執行此程序`Seed`方法。

> [!NOTE]
> 加入程式碼以`Seed`方法是，您可以將固定的資料插入資料庫的許多方法之一。 替代方式是加入程式碼以`Up`和`Down`每個移轉類別的方法。 `Up`和`Down`方法包含實作資料庫變更的程式碼。 您會看到它們中的範例[部署資料庫更新](deploying-a-database-update.md)教學課程。
> 
> 您也可以撰寫使用執行 SQL 陳述式的程式碼`Sql`方法。 例如，如果您已將 Department 資料表來加入預算資料行，而且想要移轉過程中初始化所有部門預算 $ 1，000.00，您可以加入下的一行程式碼`Up`移轉方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`


## <a name="create-scripts-for-membership-database-deployment"></a>建立成員資格資料庫部署的指令碼

Contoso 大學應用程式使用 ASP.NET 成員資格系統和表單驗證來驗證和授權使用者。 **更新信用額度**頁面是只有屬於系統管理員角色的使用者能夠存取。

執行應用程式，然後按一下**課程**，然後按一下 **更新信用額度**。

![按一下 更新參與名單](preparing-databases/_static/image7.png)

**登入**頁面隨即出現，因為**更新信用額度**頁面需要系統管理權限。

輸入*admin*的使用者名稱和*devpwd*做為密碼並按**登入**。

![登入頁面](preparing-databases/_static/image8.png)

**更新信用額度**頁面隨即出現。

![更新信用額度頁面](preparing-databases/_static/image9.png)

使用者和角色的資訊是位在*aspnet ContosoUniversity*所指定的資料庫**DefaultConnection**連接字串中的*Web.config*檔案。

此資料庫不是由 Entity Framework Code First，管理，因此您無法使用移轉將其部署。 您將使用 dbDacFx 提供者來部署資料庫結構描述中，而且您將設定執行指令碼會將初始資料插入資料庫資料表的發行設定檔。

> [!NOTE]
> Visual Studio 2013 已引進新的 ASP.NET 成員資格系統，（現在稱為 ASP.NET Identity）。 新的系統可讓您將應用程式和成員資格資料表放在相同的資料庫，您可以使用 Code First 移轉來部署。 範例應用程式會使用舊版的 ASP.NET 成員資格系統，無法使用 Code First 移轉來部署。 此成員資格資料庫部署的程序也適用於您的應用程式要部署的 SQL Server 資料庫，不由 Entity Framework Code First 中建立的任何其他案例。


此處，通常不想在您在開發的生產環境中相同的資料。 當您第一次部署站台時，因此常會排除大部分或所有您針對測試建立的使用者帳戶。 因此，會下載的專案有兩個成員資格資料庫： *aspnet ContosoUniversity.mdf*與開發使用者和*aspnet-ContosoUniversity-Prod.mdf*與生產使用者。 此教學課程中的使用者名稱是這兩個資料庫中的相同： *admin*和*nonadmin*。 這兩個使用者有密碼*devpwd*開發資料庫中並*prodpwd*生產資料庫中。

您會將開發使用者部署到測試環境和生產環境使用者預備和生產環境。 若要這樣做，在本教學課程中，一個用於開發，一個用於生產環境中，您將建立兩個 SQL 指令碼並在之後的教學課程中，您會設定發行程序，並執行。

> [!NOTE]
> 成員資格資料庫會儲存帳戶密碼的雜湊。 若要部署到另一部電腦帳戶，您必須確定雜湊常式不產生目的地伺服器上的不同雜湊，比在來源電腦上。 則會產生相同雜湊當您使用 ASP.NET Universal Providers，只要您不要變更預設的演算法。 預設的演算法為 HMACSHA256，而且在指定**驗證**屬性 **[machineKey](https://msdn.microsoft.com/library/system.web.configuration.machinekeysection.aspx)**  Web.config 檔案中的項目。


使用 SQL Server Management Studio (SSMS)，或使用協力廠商工具，您可以手動建立資料的部署指令碼。 本教學課程的這個其餘部分將說明如何執行在 SSMS 中，但如果您不想要安裝和使用 SSMS 您可以從專案的完整版取得的指令碼，並略過，您將其儲存在方案資料夾中的區段。

若要安裝 SSMS，安裝從[Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)按一下[ENU\x64\SQLManagementStudio\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLManagementStudio_x64_ENU.exe)或[ENU\x86\SQLManagementStudio\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLManagementStudio_x86_ENU.exe)。 如果您選擇錯誤的其中一個用於您的系統，它將無法安裝，而且您可以嘗試另一個。

（請注意，這是 600 mb 下載項目。 可能需要很長的時間安裝且您需要重新啟動您的電腦。)

在 SQL Server 安裝中心] 的第一個頁面上，按一下 [**新的 SQL Server 獨立安裝或將功能加入現有安裝**，並遵循指示，接受預設選項。

### <a name="create-the-development-database-script"></a>建立開發資料庫指令碼

1. 執行 SSMS。
2. 在**連接到伺服器**對話方塊方塊中，輸入*(localdb) \v11.0*為**伺服器名稱**，保留**驗證**設**Windows 驗證**，然後按一下 **連接**。

    ![SSMS 連接到伺服器](preparing-databases/_static/image10.png)
3. 在**物件總管] 中**視窗中，展開**資料庫**，以滑鼠右鍵按一下**aspnet ContosoUniversity**，按一下 [**工作**，然後按一下**產生指令碼**。

    ![SSMS 產生指令碼](preparing-databases/_static/image11.png)
4. 在**產生和發佈指令碼**對話方塊中，按一下 **設定指令碼編寫選項**。

    您可以略過**選擇物件**步驟，因為預設值是**編寫整個資料庫及所有資料庫物件的指令碼**，且您想要。
5. 按一下 [ **進階**]。

    ![SSMS 指令碼編寫選項](preparing-databases/_static/image12.png)
6. 在**進階指令碼編寫選項**對話方塊中，向下捲動至**要指令碼的資料類型**，然後按一下**僅限資料**下拉式清單中的選項。
7. 變更**編寫 USE DATABASE 的指令碼**至**False**。 使用陳述式不是有效的 Azure SQL Database，並不所需的測試環境中的部署到 SQL Server Express。

    ![SSMS 資料僅限指令碼，沒有使用陳述式](preparing-databases/_static/image13.png)
8. 按一下 [確定 **Deploying Office Solutions**]。
9. 在**產生和發佈指令碼**對話方塊中，**檔案名稱**方塊會指定將會建立指令碼。 將路徑變更您的方案資料夾 （具有 ContosoUniversity.sln 檔案的資料夾） 和檔案名稱， *aspnet-資料-dev.sql*。
10. 按一下**下一步**移至**摘要**索引標籤，然後再按一下**下一步**以建立指令碼。

    ![SSMS 指令碼建立](preparing-databases/_static/image14.png)
11. 按一下 [ **完成**]。

### <a name="create-the-production-database-script"></a>建立實際執行資料庫指令碼

您尚未執行專案與生產資料庫，因為它不是 LocalDB 執行個體尚未附加。 因此，您必須先附加資料庫。

1. 在 SSMS**物件總管 中**，以滑鼠右鍵按一下**資料庫**按一下**附加**。

    ![SSMS 附加](preparing-databases/_static/image15.png)
- 在**附加資料庫**對話方塊中，按一下 **新增**然後瀏覽至*aspnet-ContosoUniversity-Prod.mdf*檔案*應用程式\_資料*資料夾。

    ![要附加的 SSMS 將.mdf 檔案](preparing-databases/_static/image16.png)
- 按一下 [確定 **Deploying Office Solutions**]。
- 請遵循相同的程序，您先前用來建立實際執行檔案的指令碼。 指令碼檔案名稱*aspnet-資料-prod.sql*。

## <a name="summary"></a>總結

這兩個資料庫現在隨時可供部署，而且您有兩個資料部署指令碼在您的方案資料夾中。

![資料部署指令碼](preparing-databases/_static/image17.png)

下列教學課程中，您設定會影響部署的專案設定，而且您設定自動*Web.config*檔案必須已部署的應用程式中不同的設定值的轉換。

## <a name="more-information"></a>更多資訊

如需 NuGet 的詳細資訊，請參閱[管理專案程式庫與 NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx)和[NuGet 文件](http://docs.nuget.org/docs/start-here/overview)。 如果您不想要使用 NuGet，您必須了解如何分析 NuGet 封裝來判斷其用途在安裝時。 (例如，它可能會設定*Web.config*轉換設定為在建置時間等等時執行的 PowerShell 指令碼。)若要了解有關 NuGet 的運作方式的詳細資訊，請參閱[建立和發佈封裝](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)和[組態檔和來源的程式碼轉換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

>[!div class="step-by-step"]
[上一頁](introduction.md)
[下一頁](web-config-transformations.md)
