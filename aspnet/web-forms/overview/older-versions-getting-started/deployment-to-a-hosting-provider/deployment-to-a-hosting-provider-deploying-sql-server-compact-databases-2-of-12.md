---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server Compact 資料庫-2 / 12 |Microsoft 文件
author: tdykstra
description: 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 7e2d430bd8e07ed7d97d11a00c61d90beeac005f
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30890079"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server Compact 資料庫-2 / 12
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。 如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。 數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何設定兩個 SQL Server Compact 資料庫和 database engine 的部署。

進行資料庫存取 Contoso 大學應用程式需要下列軟體必須與應用程式部署，因為它不會包含在.NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) （資料庫引擎）。
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （可讓 ASP.NET 成員資格系統，若要使用 SQL Server Compact）
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)（程式碼移轉的第一個）。

資料庫結構以及某些 （並非全部） 的應用程式的兩個資料也必須部署資料庫。 一般而言，當您開發應用程式時，您輸入測試資料的資料庫，您不想要部署到即時網站。 不過，您也可能會輸入您要部署的實際執行資料。 在本教學課程中，您會設定 Contoso 大學專案，讓所需的軟體和正確的資料都包含在部署時。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="sql-server-compact-versus-sql-server-express"></a>SQL Server Compact 和 SQL Server Express

範例應用程式會使用 SQL Server Compact 4.0。 此資料庫引擎是網站; 相對較新的選項舊版的 SQL Server Compact 在 web 主機環境中無法運作。 SQL Server Compact 提供相較於以 SQL Server Express 開發及部署到 SQL Server 完整版的更常見的案例的一些優點。 根據您選擇的主控提供者，SQL Server Compact 可能成本較低，若要部署，因為某些提供者會收取額外支援完整的 SQL Server 資料庫。 沒有額外的免費的 SQL Server Compact，因為您可以將資料庫引擎本身部署 web 應用程式的一部分。

不過，您也應該注意其限制。 SQL Server Compact 不支援預存程序、 觸發程序、 檢視或複寫。 (如 SQL Server compact 不支援的 SQL Server 功能的完整清單，請參閱[差異之間 SQL Server Compact 和 SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)。)此外，有些工具可用來管理結構描述和 SQL Server Express 和 SQL Server 資料庫中的資料不適用與 SQL Server Compact。 比方說，您無法使用 SQL Server Management Studio 或 SQL Server Data Tools Visual Studio 中的 SQL Server Compact 資料庫。 您需要使用 SQL Server Compact 資料庫的其他選項：

- 您可以使用伺服器總管，在 Visual Studio 中的 SQL Server Compact 提供有限的資料庫管理功能。
- 您可以使用的資料庫操作功能[WebMatrix](https://www.microsoft.com/web/webmatrix/)，其中包含 伺服器總管較多的功能。
- 您可以使用相對的全功能的協力廠商，或開啟原始碼工具，例如[SQL Server Compact 工具箱](https://github.com/ErikEJ/SqlCeToolbox)和[SQL Compact 資料和結構描述指令碼公用程式](https://github.com/ErikEJ/SqlCeToolbox)。
- 您可以撰寫和執行您自己的 DDL （資料定義語言） 指令碼來管理資料庫結構描述。

您可以開始使用 SQL Server Compact，稍後當您的需求不同時升級。 在這一系列之後的教學課程會示範如何以 SQL Server Express 和 SQL server，從 SQL Server Compact 移轉。 不過，如果您正在建立新的應用程式，並預期在不久的將來需要 SQL Server，最好是可能使用 SQL Server 或 SQL Server Express 來啟動。

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>部署設定 SQL Server Compact 資料庫引擎

Contoso 大學應用程式中的資料存取所需的軟體已安裝下列 NuGet 套件加入：

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) （ASP.NET 通用提供者）
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

連結會指向目前的版本，這些封裝，可能會比您下載此教學課程的起始專案中安裝的內容。 以部署至主機服務提供者，請確定您使用 Entity Framework 5.0 或更新版本。 舊版的 Code First 移轉需要完全信任 」，並將許多主控提供者在中度信任中執行應用程式。 如需度信任的詳細資訊，請參閱[部署至 IIS 做為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。

NuGet 套件安裝通常會負責您需將此軟體與應用程式部署的所有項目。 在某些情況下，這牽涉到的工作，例如變更 Web.config 檔案並增加每次建置方案時執行的 PowerShell 指令碼。 **如果您想要新增支援這些功能 （例如 SQL Server Compact 和 Entity Framework），而不需使用 NuGet，請確定您知道 NuGet 套件安裝的用途，讓您以手動方式執行相同的工作。**

沒有一個例外狀況，NuGet 不接受的您必須執行以確保成功部署的所有項目。 SqlServerCompact NuGet 封裝新增至您複製的原生組件的專案建置後指令碼*x86*和*amd64*專案下的子資料夾*bin*資料夾中，但指令碼不在專案中包含這些資料夾。 如此一來，Web Deploy 將不將它們複製到目標網站除非您手動在專案中包含。 （這個行為會產生從預設的部署組態; 另一個選項，您將不會使用這些教學課程中，若要變更的設定來控制此行為。 您可以變更的設定是**只有執行應用程式所需的檔案**下**要部署的項目**上**封裝/發行 Web**  索引標籤**專案屬性**視窗。 不通常建議變更此設定，因為它可能會導致許多的多個檔案部署到生產環境不時所需那里。 如需替代方式的詳細資訊，請參閱[設定專案屬性](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)教學課程。)

建置專案，然後在**方案總管 中**按一下**顯示所有檔案**如果您有不這樣做。 您也可能需要按一下**重新整理**。

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

展開**bin**資料夾，以查看**amd64**和**x86**資料夾，然後選取那些資料夾、 按一下滑鼠右鍵，然後選取**加入至專案**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

資料夾圖示變更為顯示資料夾已包含在專案中。

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>設定 Code First 移轉，以部署應用程式資料庫

當您部署應用程式資料庫時，通常您不只會部署您開發的資料庫使用的所有資料至生產環境，因為大部分的資料可能有僅用於測試目的。 例如，測試資料庫中的學生名稱是虛構。 相反地，您通常無法部署資料庫結構與任何資料完全。 測試資料庫中的資料部分可能實際資料，而且必須有使用者使用應用程式會開始時。 例如，您的資料庫可能有包含有效的等級值或實際的部門名稱的資料表。

若要模擬這個常見案例中，您需要設定您想要有在生產環境中的資料插入資料庫的程式碼第一次移轉種子方法。 這個種子方法不會插入測試資料，因為它會在生產環境中執行程式碼第一次在生產環境中建立資料庫之後。

在舊版的 Code First 移轉發行之前，減小種子方法插入測試資料的同時，因為資料庫已完全刪除並從頭開始重新建立每一次在開發期間的模型變更。 使用 Code First 移轉，測試資料後，要保留資料庫的變更，因此不需要在 Seed 方法中包含測試資料。 您已經下載的專案使用種子類別的方法初始設定式中包括的所有資料前的移轉的方法。 在本教學課程將停用初始設定式類別，並啟用移轉。 然後您要更新的移轉組態類別中的種子方法，讓它插入您想要在生產環境中要插入的資料。

下圖說明應用程式資料庫的結構描述：

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

如需這些教學課程中，您會假設`Student`和`Enrollment`網站第一次部署時，資料表應該是空的。 其他資料表包含資料，必須預先載入應用程式上線。

因為您會使用 Code First 移轉，您不再需要使用**DropCreateDatabaseIfModelChanges** Code First 初始設定式。 此初始設定式的程式碼檔案中不 SchoolInitializer.cs ContosoUniversity.DAL 專案中。 中的設定**appSettings** Web.config 檔案的項目會造成此初始設定式來執行應用程式第一次存取資料庫時：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

開啟應用程式 Web.config 檔案，並移除項目，指定第一個程式碼的初始設定式類別，從 appSettings 項目。 AppSettings 項目現在看起來像這樣：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> 指定初始設定式類別的另一種方式是藉由呼叫來進行`Database.SetInitializer`中`Application_Start`方法中的*Global.asax*檔案。 如果您要啟用移轉的專案中，會使用該方法指定初始設定式，移除該程式碼行。


接下來，啟動 Code First 移轉。

第一個步驟是確定 ContosoUniversity 專案已設定為啟始專案。 在**方案總管 中**，ContosoUniversity 專案上按一下滑鼠右鍵，然後選取**設定為啟始專案**。 Code First 移轉會在啟始專案，若要尋找的資料庫連接字串中尋找。

從**工具**功能表上，按一下 **程式庫套件管理員**然後**Package Manager Console**。

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

在頂端**Package Manager Console**視窗選取 ContosoUniversity.DAL 做為預設專案，然後在`PM>`提示字元中輸入 「 啟用移轉 」。

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

此命令會建立*configuration.cs 中*檔案中的新*移轉*ContosoUniversity.DAL 專案資料夾中的。

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

因為 「 啟用移轉 」 命令必須執行包含的第一個程式碼的內容類別的專案中，您可以選取 DAL 專案。 該類別是在類別庫專案，Code First 移轉會尋找資料庫連接字串中方案的啟始專案。 在 ContosoUniversity 解決方案中，web 專案已設定為啟始專案。 （如果您不想指定為啟始專案，Visual Studio 中具有的連線字串的專案，您可以指定的啟始專案中的 PowerShell 命令。 若要查看 enable-migrations 命令的命令語法，您可以輸入命令"取得說明讓移轉 」）。

開啟 configuration.cs 中檔案和取代中的註解`Seed`方法取代下列程式碼：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

若要參考`List`有其下的紅色曲線，因為您沒有`using`其命名空間陳述式尚未。 以滑鼠右鍵按一下其中一個執行個體的`List`按一下**解決**，然後按一下  **using System.Collections.Generic**。

![解決與使用陳述式](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

此功能表選取項目加入下列程式碼，`using`陳述式頂端附近的檔案。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> 加入程式碼以`Seed`方法是，您可以將固定的資料插入資料庫的許多方法之一。 替代方式是加入程式碼以`Up`和`Down`每個移轉類別的方法。 `Up`和`Down`方法包含實作資料庫變更的程式碼。 您會看到它們中的範例[部署資料庫更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教學課程。
> 
> 您也可以撰寫使用執行 SQL 陳述式的程式碼`Sql`方法。 例如，如果您已將 Department 資料表來加入預算資料行，而且想要移轉過程中初始化所有部門預算 $ 1，000.00，您可以加入下的一行程式碼`Up`移轉方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> 此範例顯示針對本教學課程使用`AddOrUpdate`方法中的`Seed`方法的 Code First 移轉`Configuration`類別。 Code First 移轉呼叫`Seed`方法，每個移轉之後，而且這個方法會更新資料列已經插入，或將它們插入如果它們尚不存在。 `AddOrUpdate`方法可能不是您的案例的最佳選擇。 如需詳細資訊，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 部落格上。


按 CTRL-SHIFT-B 以建置專案。

下一個步驟是建立`DbMigration`初始移轉的類別。 您想要這項移轉，來建立新的資料庫，因此您必須刪除現有的資料庫。 SQL Server Compact 資料庫中所包含 *.sdf*檔案*應用程式\_資料*資料夾。 在**方案總管 中**，依序展開*應用程式\_資料*ContosoUniversity 專案中查看的兩個 SQL Server Compact 資料庫，這由 *.sdf*檔案。

以滑鼠右鍵按一下*School.sdf*檔案，然後按一下 **刪除**。

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

在**Package Manager Console**視窗中，輸入 「 新增移轉初始"命令以建立初始移轉並將它命名為 「 初始 」。

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First 移轉建立的另一個類別檔案*移轉*資料夾中，且此類別包含程式碼會建立資料庫結構描述。

在**Package Manager Console**，輸入命令 [更新的資料庫] 來建立資料庫和執行**種子**方法。

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(如果您收到錯誤，指出資料表已經存在，而且無法建立，它可能是因為刪除資料庫之後，您在執行之前，執行該應用程式`update-database`。 In that case，刪除*School.sdf*檔案一次，然後重試`update-database`命令。)

執行應用程式。 現在學生頁面是空的但講師頁面包含講師。 這是功能之後，您會取得在生產環境中部署應用程式。

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

專案已經準備好要部署*學校*資料庫。

## <a name="creating-a-membership-database-for-deployment"></a>正在建立部署的成員資格資料庫

Contoso 大學應用程式使用 ASP.NET 成員資格系統和表單驗證來驗證和授權使用者。 其中一個它的頁面是只能由系統管理員存取。 若要查看此頁面，執行應用程式，然後選取**更新信用額度**從彈出式視窗功能表底下**課程**。 應用程式會顯示**登入**頁面上，因為只有系統管理員已獲授權使用**更新信用額度**頁面。

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

"Admin"使用"Pa$ w0rd"的密碼以登入 （請注意數字零來取代"w0rd"中的字母"o"）。 您登入之後,**更新信用額度**頁面隨即顯示。

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

當您第一次部署站台時，因此常會排除大部分或所有您針對測試建立的使用者帳戶。 在此情況下，您要部署的系統管理員帳戶並沒有使用者帳戶。 而不是以手動方式刪除測試帳戶，您將建立新的成員資格資料庫，其只有一位系統管理員使用者帳戶，您需要在生產環境中。

> [!NOTE]
> 成員資格資料庫會儲存帳戶密碼的雜湊。 若要部署到另一部電腦帳戶，您必須確定雜湊常式不產生目的地伺服器上的不同雜湊，比在來源電腦上。 則會產生相同雜湊當您使用 ASP.NET Universal Providers，只要您不要變更預設的演算法。 預設的演算法為 HMACSHA256，而且在指定**驗證**屬性**[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Web.config 檔案中的項目。


成員資格資料庫不由 Code First 移轉，維護，而且沒有任何自動的初始設定式植入的資料庫測試帳戶，（因為沒有 School 資料庫）。 因此，要保留測試資料使用之前建立一個新會建立一份測試資料庫。

在**方案總管 中**，重新命名*aspnet.sdf*檔案*應用程式\_資料*資料夾*aspnet Dev.sdf*。 (不要製作複本，只要將它重新命名，您將在不久後建立新的資料庫。)

在**方案總管] 中**，請確定已選取 [web 專案 （ContosoUniversity、 不 ContosoUniversity.DAL）。 接著在**專案**功能表上，選取**ASP.NET 組態**執行**網站管理工具**(WAT)。

選取**安全性** 索引標籤。

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

按一下**建立或管理角色**並加入**管理員**角色。

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

瀏覽回**安全性**索引標籤上，按一下  **Create User**，並加入做為管理員的使用者"admin"。 您按一下前**Create User**按鈕**Create User**頁面上，確定您選取**管理員**核取方塊。 本教學課程中使用的密碼為"Pa$ w0rd"，而且您可以輸入任何電子郵件地址。

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

關閉瀏覽器。 在**方案總管 中**，按一下 重新整理 按鈕，以查看新*aspnet.sdf*檔案。

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

以滑鼠右鍵按一下**aspnet.sdf**選取**加入至專案**。

## <a name="distinguishing-development-from-production-databases"></a>區別從實際執行資料庫開發

在本節中，您將會重新命名資料庫使開發版本被學校 Dev.sdf 和 aspnet Dev.sdf 與實際執行版本的學校 Prod.sdf 和 aspnet Prod.sdf。 這並非必要，但這樣做，將有助於防止取得與資料庫的測試和實際執行版本混淆。

在**方案總管 中**，按一下 **重新整理**，展開 應用程式\_Data 資料夾，請參閱您稍早建立的 School 資料庫，以滑鼠右鍵按一下它並選取**加入至專案**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

重新命名*aspnet.sdf*至*aspnet Prod.sdf*。

重新命名*School.sdf*至*學校 Dev.sdf*。

當您不想使用的 Visual Studio 中執行應用程式時 *-Prod*資料庫檔案的版本中，您想要使用 *-開發人員*版本。 因此您必須變更 Web.config 檔案中的連接字串，使其指向 *-開發人員*與資料庫的版本。 （您尚未建立 School Prod.sdf 檔案，沒關係，因為第一個程式碼會建立該資料庫在該處執行您的應用程式的實際執行第一個時間。）

開啟應用程式的 Web.config 檔案，並找到的連接字串：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

變更"aspnet-Dev.sdf"，"aspnet.sdf 」，並將"School.sdf"變更"學校 Dev.sdf":

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact 資料庫引擎和這兩個資料庫現在會是隨時可供部署項目。 下列教學課程中您設定自動*Web.config*檔案必須在開發、 測試和實際執行環境中不同的設定值的轉換。 （必須變更的設定，其中包括連接字串中，但您將會建立這些變更稍後當您建立的發行設定檔）。

## <a name="more-information"></a>更多資訊

如需 NuGet 的詳細資訊，請參閱[管理專案程式庫與 NuGet](https://msdn.microsoft.com/magazine/hh547106.aspx)和[NuGet 文件](http://docs.nuget.org/docs/start-here/overview)。 如果您不想要使用 NuGet，您必須了解如何分析 NuGet 封裝來判斷其用途在安裝時。 (例如，它可能會設定*Web.config*轉換設定為在建置時間等等時執行的 PowerShell 指令碼。)若要了解有關 NuGet 的運作方式的詳細資訊，請參閱特別[建立和發佈封裝](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)和[組態檔和來源的程式碼轉換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
