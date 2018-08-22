---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server Compact 資料庫-12 個 2 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: riande
ms.date: 11/17/2011
ms.assetid: c3c76516-4c48-4153-bd03-d70e3a3edbb0
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12
msc.type: authoredcontent
ms.openlocfilehash: 378bcc038335ee852cd1a6c6e545eb72c6e0c78b
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41824756"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-sql-server-compact-databases---2-of-12"></a>使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server Compact 資料庫-2 / 12
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。 如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。 在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。
> 
> 顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。


## <a name="overview"></a>總覽

本教學課程會示範如何設定兩個 SQL Server Compact 資料庫和 database engine 的部署。

資料庫存取 Contoso 大學應用程式需要下列軟體必須與應用程式部署，因為它不會包含在.NET Framework:

- [SQL Server Compact](https://www.microsoft.com/sqlserver/en/us/editions/compact.aspx) （資料庫引擎）。
- [ASP.NET Universal Providers](http://www.hanselman.com/blog/IntroducingSystemWebProvidersASPNETUniversalProvidersForSessionMembershipRolesAndUserProfileOnSQLCompactAndSQLAzure.aspx) （可讓 ASP.NET 成員資格系統，才能使用 SQL Server Compact）
- [Entity Framework 5.0](https://msdn.microsoft.com/library/gg696172(d=lightweight,v=vs.103).aspx)（程式碼進行移轉作業的第一個）。

資料庫結構，以及一些 （並非全部） 的應用程式的兩個中的資料也必須部署資料庫。 一般而言，當您開發應用程式時，您輸入的測試資料到資料庫，您不想要部署至實際網站。 不過，您也可以輸入一些要部署的實際執行資料。 在本教學課程中，您將設定 Contoso 大學專案以便必要的軟體和正確的資料時，會包含您所部署。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。

## <a name="sql-server-compact-versus-sql-server-express"></a>與 SQL Server Express 的 SQL Server Compact

範例應用程式會使用 SQL Server Compact 4.0。 此資料庫引擎是相當新的選項的網站，舊版的 SQL Server Compact 在 web 裝載環境中無法運作。 SQL Server Compact 提供幾個優點，相較於使用 SQL Server Express 進行開發和部署完整的 SQL server 的較常見的情況。 根據您所選擇的裝載提供者，SQL Server Compact 可能更便宜，若要部署，因為某些提供者會收取額外的支援完整的 SQL Server 資料庫。 沒有額外的免費 SQL Server Compact 的因為您可以將資料庫引擎本身部署為 web 應用程式的一部分。

不過，您也應該了解其限制。 SQL Server Compact 不支援預存程序、 觸發程序、 檢視或複寫。 (如需不支援的 SQL Server Compact 的 SQL Server 功能的完整清單，請參閱 <<c0> [ 差異之間 SQL Server Compact 和 SQL Server](https://msdn.microsoft.com/library/bb896140.aspx)。)此外，部分的工具，您可以使用結構描述和 SQL Server Express 和 SQL Server 資料庫中的資料操作功能不適用於 SQL Server Compact。 比方說，您無法使用 SQL Server Management Studio 或 SQL Server Data Tools 在 Visual Studio 中使用 SQL Server Compact 資料庫。 您需要使用 SQL Server Compact 資料庫的其他選項：

- 您可以使用伺服器總管 中，在 Visual Studio 中的 SQL Server Compact 提供有限的資料庫操作功能。
- 您可以使用的資料庫操作功能[WebMatrix](https://www.microsoft.com/web/webmatrix/)，其具有較多的伺服器總管 功能。
- 您可以使用相對的全功能的協力廠商，或開啟原始碼工具，例如[SQL Server Compact 工具箱](https://github.com/ErikEJ/SqlCeToolbox)並[SQL Compact 的資料和結構描述指令碼的公用程式](https://github.com/ErikEJ/SqlCeToolbox)。
- 您可以撰寫和執行您自己的 DDL （資料定義語言） 指令碼，來管理資料庫結構描述。

您可以開始使用 SQL Server Compact，然後再升級 稍後隨著您需求的演變。 在本系列稍後的教學課程會示範如何從 SQL Server Compact 移轉，SQL Server express 和 SQL Server。 不過，如果您要建立新的應用程式，並預期在不久的將來需要 SQL Server，最好是可能以 SQL Server 或 SQL Server Express 開始著手。

## <a name="configuring-the-sql-server-compact-database-engine-for-deployment"></a>設定部署的 SQL Server Compact 資料庫引擎

Contoso 大學應用程式中的資料存取所需的軟體已安裝下列 NuGet 封裝：

- [SqlServerCompact](http://nuget.org/List/Packages/SqlServerCompact)
- [System.Web.Providers](http://nuget.org/List/Packages/System.Web.Providers) （ASP.NET 通用提供者）
- [EntityFramework](http://nuget.org/List/Packages/EntityFramework)
- [EntityFramework.SqlServerCompact](http://nuget.org/List/Packages/EntityFramework.sqlservercompact)

連結會指向目前的版本，這些封裝，它可能會比在您下載本教學課程中將入門專案中安裝的項目。 部署至主機服務提供者，請確定您使用 Entity Framework 5.0 或更新版本。 舊版的 Code First 移轉需要完全信任，並將在許多主機服務提供者中度信任執行應用程式。 如需度信任的詳細資訊，請參閱[部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)教學課程。

NuGet 套件安裝通常會處理所需的一切若要部署此軟體與應用程式。 在某些情況下，這牽涉到的工作，例如變更 Web.config 檔案，並新增您建置方案時執行的 PowerShell 指令碼。 **如果您想要新增的任何一項功能 （例如 SQL Server Compact 和 Entity Framework） 的支援，而不需使用 NuGet，請確定您知道的 NuGet 套件安裝的功能，讓您可以手動執行相同的工作。**

沒有一個例外狀況，NuGet 不會處理您只需要以確保成功部署的所有項目。 SqlServerCompact NuGet 套件新增至您複製的原生組件的專案的建置後指令碼*x86*並*amd64*專案下的子資料夾*bin*資料夾中，但指令碼不在專案中包含這些資料夾。 如此一來，Web Deploy 將不將它們複製到目標網站除非您以手動方式在專案中包含。 （此行為起因預設部署組態，另一個選項，您將不會使用這些教學課程中，是要變更的設定來控制此行為。 設定，您可以變更是**執行應用程式所需的檔案**下方**部署的項目**上**封裝/發行 Web**  索引標籤的**專案屬性**視窗。 不通常建議變更此設定，因為它可能會導致許多的多個檔案部署到生產環境所需那里比。 如需替代方式的詳細資訊，請參閱[3f25044b4ce2"&gt;configuring Project Properties&lt](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)教學課程。)

建置專案，然後在**方案總管**按一下 **顯示所有檔案**如果您還沒有這麼。 您也可能必須按一下 **重新整理**。

![Solution_Explorer_Show_All_Files](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image1.png)

依序展開**bin**資料夾，以查看**amd64**並**x86**資料夾，然後選取這些資料夾、 按一下滑鼠右鍵，然後選取**加入至專案**.

![amd64_and_x86_in_Solution_Explorer.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image2.png)

資料夾圖示變更為顯示資料夾已包含在專案中。

![Solution_Explorer_amd64_included.png](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image3.png)

## <a name="configuring-code-first-migrations-for-application-database-deployment"></a>設定部署應用程式資料庫的 Code First 移轉

當您部署應用程式資料庫時，通常您不只要部署您的開發資料庫，所有的資料到生產環境，因為大部分的資料可能有僅用於測試目的。 比方說，在測試資料庫中的學生名稱是虛構的。 相反地，您通常無法部署資料庫結構中沒有資料的根本。 測試資料庫中的資料部分可能實際資料，而且時，必須有使用者使用應用程式會開始。 例如，您的資料庫可能包含有效的等級值或實際的部門名稱的資料表。

若要模擬這個常見的案例，您將設定您想要在生產環境中的資料插入資料庫的程式碼的第一個移轉種子方法。 這個種子方法不會插入測試資料，因為它會在生產環境中執行 Code First 在生產環境中建立資料庫之後。

在舊版的 Code First 移轉發行之前，很常見的種子方法，將測試資料以及因為資料庫已完全刪除並從頭開始重新建立在開發期間每次模型變更。 使用 Code First 移轉，測試資料之後，會保留資料庫的變更，因此不需要測試資料納入種子方法。 您所下載的專案會使用前的移轉方法包括所有資料的初始設定式類別的識別值種子方法中。 在本教學課程中，您將停用初始設定式類別，並啟用移轉。 然後您將更新種子方法，在 移轉設定類別，以便將它插入您想要在生產環境中要插入的資料。

下圖說明應用程式資料庫的結構描述：

[![School_database_diagram](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image4.png)

這些教學課程中，您將會假設`Student`和`Enrollment`站台第一次部署時，資料表應該是空的。 其他資料表包含對應用程式上線時預先載入的資料。

因為您將使用 Code First 移轉，您不再需要使用**DropCreateDatabaseIfModelChanges** Code First 初始設定式。 此初始設定式的程式碼檔案中不 SchoolInitializer.cs ContosoUniversity.DAL 專案中。 中的設定**appSettings** Web.config 檔案的項目會使得每次應用程式會嘗試將第一次存取資料庫時執行此初始設定式：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample1.xml?highlight=3)]

開啟應用程式 Web.config 檔案，並移除指定的 appSettings 元素的 Code First 初始設定式類別的項目。 AppSettings 項目現在看起來像這樣：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample2.xml)]

> [!NOTE]
> 指定初始設定式類別的另一種方式是藉由呼叫這麼`Database.SetInitializer`中`Application_Start`方法中的*Global.asax*檔案。 如果您要啟用移轉的專案會使用該方法來指定初始設定式中，移除該程式碼行。


接下來，啟用 Code First 移轉。

第一個步驟是確定 ContosoUniversity 專案設為啟始專案。 在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後選取**設定為啟始專案**。 在 啟始專案，若要尋找的資料庫連接字串看起來 code First 移轉。

從**工具**功能表上，按一下**程式庫套件管理員**，然後**Package Manager Console**。

![Selecting_Package_Manager_Console](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image6.png)

在頂端**Package Manager Console**視窗中選取做為預設專案，然後在 ContosoUniversity.DAL`PM>`提示字元中輸入 「 啟用移轉 」。

![enable-migrations_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image7.png)

此命令會建立*Configuration.cs*在新的檔案*移轉*ContosoUniversity.DAL 專案中的資料夾。

![Migrations_folder_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image8.png)

因為 「 啟用移轉 」 命令必須執行包含的第一個程式碼的內容類別的專案中，您可以選取 DAL 專案。 類別庫專案中該類別時，Code First Migrations 會尋找方案的啟始專案中的資料庫連接字串。 在 ContosoUniversity 解決方案中，web 專案已設定為啟始專案。 （如果您不想指定為啟始專案在 Visual Studio 已連接字串的專案，您可以指定啟始專案中的 PowerShell 命令。 若要查看啟用移轉命令的命令語法，您可以輸入命令"取得說明啟用-移轉 」）。

開啟 Configuration.cs 檔案，並取代中的註解`Seed`為下列程式碼的方法：

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample3.cs)]

若要參考`List`已在其下的紅色曲線，因為您不需要`using`其命名空間陳述式尚未。 以滑鼠右鍵按一下其中一個的執行個體`List`，按一下 **解決**，然後按一下**using System.Collections.Generic**。

![解決與使用陳述式](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image9.png)

此功能表選取項目新增下列程式碼`using`陳述式會靠近頂端的檔案。

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample4.cs)]

> [!NOTE]
> 將程式碼加入`Seed`方法是，您可以將固定的資料插入資料庫的眾多方法之一。 替代方式是將程式碼加入`Up`和`Down`移轉的每個類別的方法。 `Up`和`Down`方法包含實作資料庫變更的程式碼。 您會看到範例在[部署資料庫更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教學課程。
> 
> 您也可以撰寫使用執行 SQL 陳述式的程式碼`Sql`方法。 比方說，如果您已將預算資料行加入將 Department 資料表，而且想要移轉過程中初始化所有部門預算 $ 1，000.00，您可以新增的程式碼的下一行`Up`的移轉方法：
> 
> `Sql("UPDATE Department SET Budget = 1000");`
> 
> 顯示本教學課程會使用此範例`AddOrUpdate`方法中的`Seed`方法的 Code First 移轉`Configuration`類別。 程式碼會呼叫第一個移轉`Seed`方法之後每個移轉，並且此方法會更新具有已插入，或將其插入，如果它們尚不存在的資料列。 `AddOrUpdate`方法可能不到您的案例的最佳選擇。 如需詳細資訊，請參閱 <<c0> [ 請小心使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 的部落格上。


按 CTRL-SHIFT-B 來建置專案。

下一個步驟是建立`DbMigration`類別初始移轉。 您想要建立新的資料庫，因此您必須刪除的資料庫已經存在，此移轉。 SQL Server Compact 資料庫中包含 *.sdf*中的檔案*應用程式\_資料*資料夾。 在 **方案總管**，展開*應用程式\_資料*在 ContosoUniversity 專案若要查看兩個 SQL Server Compact 資料庫中，這由 *.sdf*檔案。

以滑鼠右鍵按一下*School.sdf*檔案，然後按一下**刪除**。

![sdf_files_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image10.png)

在 [ **Package Manager Console** ] 視窗中，輸入命令"add-migration Initial 」 來建立初始移轉並將它命名為 「 初始 」。

![add-migration_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image11.png)

Code First 移轉會建立另一個類別檔案中的*移轉*資料夾，然後此類別包含程式碼會建立資料庫結構描述。

在  **Package Manager Console**，輸入命令"更新資料庫內 」 來建立資料庫和執行**種子**方法。

![update-database_command](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image12.png)

(如果您收到錯誤，指出資料表已經存在，而且無法建立時，可能是因為您已刪除的資料庫之後，您在執行之前，執行應用程式`update-database`。 在此情況下，刪除*School.sdf*檔案一次，然後再試一次`update-database`命令。)

執行應用程式。 現在學生頁面是空的但是 Instructors 頁面包含講師。 這是您會得到在生產環境中部署應用程式之後。

![Empty_Students_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image13.png)

![Instructors_page_after_initial_migration](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image14.png)

專案現在已準備好部署*學校*資料庫。

## <a name="creating-a-membership-database-for-deployment"></a>建立用於部署的成員資格資料庫

Contoso 大學應用程式會使用 ASP.NET 成員資格系統和表單驗證來驗證和授權使用者。 其中一個頁面是僅供系統管理員存取。 若要查看此頁面，執行應用程式，然後選取**更新信用額度**下方的飛出視窗功能表從**課程**。 應用程式會顯示**登入**頁面上，因為只有系統管理員已獲授權使用**更新信用額度**頁面。

[![Log_in_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image16.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image15.png)

"Admin"使用密碼"Pa$ w0rd 」 身分登入 （請注意數字零來取代"w0rd 」 中的字母"o"）。 登入之後,**更新信用額度**頁面隨即顯示。

[![Update_Credits_page](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image17.png)

當您第一次部署站台時，常會排除大部分或所有您建立用於測試的使用者帳戶。 在此情況下，您會將部署的系統管理員帳戶並沒有使用者帳戶。 而不是以手動方式刪除測試帳戶，您將建立新的成員資格資料庫具有只有一位系統管理員使用者帳戶，您需要在生產環境中。

> [!NOTE]
> 成員資格資料庫會儲存帳戶密碼的雜湊。 若要部署到另一個從一部電腦帳戶，您必須確定雜湊的常式不產生在目的地伺服器上的不同雜湊，比在來源電腦上。 它們會產生相同的雜湊當您使用 ASP.NET Universal Providers，只要您不要變更預設的演算法。 預設的演算法為 HMACSHA256，而且在指定**驗證**屬性 **[machineKey](https://msdn.microsoft.com/library/w8h3skw9.aspx)** Web.config 檔案中的項目。


成員資格資料庫不由 Code First 移轉，維護，而且沒有任何自動的初始設定式 （因為沒有 School 資料庫） 植入測試帳戶的資料庫。 因此，若要提供的測試資料之前建立一個新會建立一份測試資料庫。

在 [**方案總管] 中**，重新命名*aspnet.sdf*中的檔案*應用程式\_資料*資料夾*aspnet Dev.sdf*。 (不製作複本，只是重新命名，您將在稍後建立新的資料庫。)

在 **方案總管 中**，請確定已選取 web 專案 （不 ContosoUniversity.DAL 中的 ContosoUniversity）。 然後在**專案**功能表上，選取**ASP.NET 組態**到執行**Web Site Administration Tool**(WAT)。

選取 [**安全性**] 索引標籤。

[![WAT_Security_tab](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image19.png)

按一下 **建立或管理角色**並加入**管理員**角色。

[![WAT_Create_New_Role](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image21.png)

瀏覽回到**安全性**索引標籤上，按一下**Create User**，並新增系統管理員身分的使用者"admin"。 在您按一下之前**Create User**按鈕**Create User**頁面上，確定您選取**管理員**核取方塊。 在本教學課程所使用的密碼為"Pa$ w0rd"，而您可以輸入任何電子郵件地址。

[![WAT_Create_User](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image23.png)

關閉瀏覽器。 在 **方案總管**，按一下 重新整理 按鈕，以查看新*aspnet.sdf*檔案。

![New_aspnet.sdf_in_Solution_Explorer](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image25.png)

以滑鼠右鍵按一下**aspnet.sdf** ，然後選取**加入至專案**。

## <a name="distinguishing-development-from-production-databases"></a>從生產資料庫的特殊開發

在本節中，您將重新命名資料庫，讓開發版本學校 Dev.sdf 而且 aspnet Dev.sdf 和生產環境版本是學校 Prod.sdf aspnet Prod.sdf。 這並非必要，但這麼做因此有助於讓您取得資料庫的測試和實際執行版本混淆。

中**方案總管**，按一下**重新整理**，展開 應用程式\_若要查看您稍早建立的 School 資料庫，以滑鼠右鍵按一下它，然後選取 資料 資料夾**加入至專案**.

![Including_School.sdf_in_project](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/_static/image26.png)

重新命名*aspnet.sdf*要*aspnet Prod.sdf*。

重新命名*School.sdf*要*學校 Dev.sdf*。

當您不想使用的 Visual Studio 中，會在執行應用程式時 *-Prod*資料庫檔案的版本中，您想要 *-Dev*版本。 因此您必須變更 Web.config 檔案中的連接字串，使其指向 *-Dev*與資料庫的版本。 （您還沒有建立 School Prod.sdf 檔案，但這是因為 Code First 會建立該資料庫在該處執行您的應用程式的生產第一個階段中的 [確定]）。

開啟應用程式的 Web.config 檔案，並找到可用的連接字串：

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample5.xml)]

變更"aspnet-Dev.sdf"，"aspnet.sdf 」，並將"School.sdf"變更為 「 學校 Dev.sdf 」:

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12/samples/sample6.xml?highlight=4-5)]

SQL Server Compact 資料庫引擎和這兩個資料庫已準備好進行部署。 在下列教學課程，您將設定自動*Web.config*檔的設定，必須在開發、 測試和生產環境中不同的轉換。 （必須變更的設定，包括連接字串，但您將設定這些變更之後當您建立的發行設定檔）。

## <a name="more-information"></a>更多資訊

如需 NuGet 的詳細資訊，請參閱[nuget 管理專案程式庫](https://msdn.microsoft.com/magazine/hh547106.aspx)並[NuGet 文件](http://docs.nuget.org/docs/start-here/overview)。 如果您不想要使用 NuGet，您必須了解如何分析以判斷其用途在安裝時 NuGet 套件。 (例如，它可能會設定*Web.config*轉換設定 PowerShell 指令碼，來執行在建置時間等等。)若要深入了解 NuGet 的運作方式，請參閱特別[建立及發佈套件](http://docs.nuget.org/docs/creating-packages/creating-and-publishing-a-package)並[組態檔和原始程式碼轉換](http://docs.nuget.org/docs/creating-packages/configuration-file-and-source-code-transformations)。

> [!div class="step-by-step"]
> [上一頁](deployment-to-a-hosting-provider-introduction-1-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-web-config-file-transformations-3-of-12.md)
