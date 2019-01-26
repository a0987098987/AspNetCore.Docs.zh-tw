---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: 教學課程：ASP.NET MVC 應用程式中使用 EF 移轉並部署至 Azure
author: tdykstra
description: 在本教學課程中，您可以啟用 Code First 移轉，並部署至 Azure 中雲端應用程式。
ms.author: riande
ms.date: 01/16/2019
ms.topic: tutorial
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: dd6bf5d8eb8a05dad1d230ef40c9b863e2af7094
ms.sourcegitcommit: d5223cf6a2cf80b4f5dc54169b0e376d493d2d3a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2019
ms.locfileid: "54889908"
---
# <a name="tutorial-use-ef-migrations-in-an-aspnet-mvc-app-and-deploy-to-azure"></a>教學課程：ASP.NET MVC 應用程式中使用 EF 移轉並部署至 Azure

到目前為止 Contoso 大學範例 web 應用程式已經執行本機 IIS Express 中的開發電腦上。 若要讓實際的應用程式供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 在本教學課程中，您可以啟用 Code First 移轉，並部署至 Azure 中雲端應用程式：

- 啟用 Code First 移轉。 移轉功能可讓您變更資料模型，並將變更部署到生產環境，藉由更新資料庫結構描述，而不需卸除並重新建立資料庫。
- 部署到 Azure。 這個步驟是選擇性的;而不部署專案，您可以繼續進行其餘的教學課程。

我們建議您進行部署，使用與原始檔控制持續整合程序，但本教學課程並未涵蓋這些主題。 如需詳細資訊，請參閱 <<c0> [ 原始檔控制](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)並[連續整合](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)章[使用 Azure 建置真實世界雲端應用程式](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)。

在本教學課程中，您已：

> [!div class="checklist"]
> * 啟用 Code First 移轉
> * （選擇性） 的 Azure 中的應用程式部署

## <a name="prerequisites"></a>必要條件

- [連線恢復功能和命令攔截](connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application.md)

## <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

在開發新的應用程式時，您的資料模型會頻繁變更，而每次模型變更時，模型會與資料庫不同步。 您已設定 Entity Framework 自動卸除並重新建立每次變更資料模型資料庫。 當您新增、 移除或變更實體類別或變更您`DbContext`類別的下次您執行應用程式會自動刪除您現有的資料庫，會建立一個新的符合模型，並植入測試資料。

在您將應用程式部署到生產環境之前，都可以使用上述方法讓資料庫與資料模型保持同步。 當在生產環境中執行應用程式時，通常會儲存您想要保留，且您不希望遺失任何項目每次進行變更，例如加入新的資料行的資料。 [Code First 移轉](https://msdn.microsoft.com/data/jj591621)功能解決這個問題，藉由啟用 Code First 來更新資料庫結構描述，而不是卸除並重新建立資料庫。 在本教學課程中，您要部署的應用程式，並準備進行，您會啟用移轉。

1. 停用您稍早設定的註解化或刪除的初始設定式`contexts`您新增至應用程式 Web.config 檔案的項目。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. 應用程式中而且*Web.config*檔案中，將連接字串中資料庫的名稱變更為 ContosoUniversity2。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    這項變更會設定專案，以便在第一次移轉建立新的資料庫。 這不是必要，但您稍後所見，為什麼它是個不錯的主意。
3. 從 [工具] 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台]。

1. 在`PM>`提示字元輸入下列命令：

    ```text
    enable-migrations
    add-migration InitialCreate
    ```

    `enable-migrations`命令會建立*移轉*資料夾中 [ContosoUniversity] 專案，並將該資料夾中放*Configuration.cs*設定移轉，您可以編輯的檔案。

    (如果您錯過了上述步驟，將引導您變更資料庫名稱，移轉會尋找現有的資料庫，並自動執行`add-migration`命令。 這是好的它只是表示部署資料庫之前，您將不會執行移轉程式碼的測試。 稍後當您執行`update-database`命令不會有因為資料庫已經存在。)

    開啟*ContosoUniversity\Migrations\Configuration.cs*檔案。 如同您先前看到的初始設定式類別`Configuration`類別包含`Seed`方法。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    目的[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法是讓您插入或更新的測試資料，Code First 會建立或更新資料庫之後。 在建立資料庫時，以及每次變更資料模型之後，會更新資料庫結構描述，會呼叫方法。

### <a name="set-up-the-seed-method"></a>設定種子方法

當您卸除並重新建立資料庫中的每個資料模型變更時，您可以使用初始設定式類別`Seed`插入測試資料，因為每次模型變更之後卸除資料庫的方法，並測試的所有資料都會遺失。 使用 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法通常不是必要。 事實上，您不想`Seed`方法來插入測試資料，如果您將使用移轉將資料庫部署到生產環境，因為`Seed`方法會在生產環境中執行。 在此情況下，您想`Seed`插入資料庫，您需要的資料在生產環境中的方法。 例如，您可能想要包括在實際的部門名稱的資料庫`Department`資料表在生產環境中，您可以使用應用程式時。

本教學課程中，您將使用移轉來進行部署，但您`Seed`方法都會仍要插入的測試資料，以便讓您更輕鬆地查看應用程式的功能而不需要以手動方式插入大量資料的運作方式。

1. 內容取代*Configuration.cs*下列程式碼，將測試資料載入至新的資料庫檔案。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法會採用資料庫內容物件做為輸入參數，並在方法中的程式碼會使用該物件來將新實體新增至資料庫。 每個實體類型，程式碼會建立新實體的集合，將它們新增至適當[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)實體的每一個群組之後的方法，做為在這裡，完成，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

    某些插入資料的陳述式使用[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)執行"upsert"作業的方法。 因為`Seed`方法會執行，每次您執行`update-database`命令時，通常每個移轉之後，因為您嘗試新增的資料列會有第一個移轉建立資料庫之後插入資料。 "Upsert"作業可避免當您嘗試插入資料列已存在，但它會發生的錯誤***會覆寫***資料，您可能會在測試應用程式時所做的任何變更。 某些資料表中的測試資料與您可能不想發生這種情況： 在某些情況下當您將資料變更時測試您的要資料庫更新後要保持變更。 在此情況下您想要進行條件式的 insert 作業： 插入資料列，只有當不存在。 種子方法會使用這兩種方法。

    第一個參數傳遞給[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定要用來檢查資料列是否已經存在的屬性。 為您提供測試學生資料`LastName`屬性可以使用針對此目的，因為每個清單中的最後一個名稱是唯一的：

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    此程式碼會假設這些姓氏是唯一。 如果您以手動方式新增最後一個名稱重複的學生，您會收到下列例外狀況下一次您執行移轉：

    **序列包含一個以上的項目**

    如需如何處理重複的資料，例如兩個名為"Alexander Carson"的學生資訊，請參閱[植入及偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson 部落格上。 如需詳細資訊`AddOrUpdate`方法，請參閱 <<c2> [ 負責使用 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 的部落格上。

    建立的程式碼`Enrollment`實體會假設您已`ID`的實體中的值`students`集合，雖然您未設定該屬性在建立集合的程式碼。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    您可以使用`ID`以下屬性因為`ID`設定值，當您呼叫`SaveChanges`如`students`集合。 EF 會自動取得主索引鍵值，當它將實體插入資料庫，並更新`ID`在記憶體中實體的屬性。

    將每一個程式碼`Enrollment`實體`Enrollments`不會使用實體集`AddOrUpdate`方法。 它會檢查是否實體已經存在，而且如果不存在，插入實體。 這種方法將會保留您對使用應用程式 UI 的註冊級變更。 此程式碼迴圈的每個成員`Enrollment`[清單](https://msdn.microsoft.com/library/6sh2ey19.aspx)和資料庫中找不到註冊，它會將註冊加入資料庫。 第一次更新資料庫，資料庫將會是空的因此它會將新增每個註冊。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]

2. 建置專案。

### <a name="execute-the-first-migration"></a>執行第一次移轉

當您執行`add-migration`命令，移轉作業產生的程式碼，會從頭開始建立資料庫。 此程式碼也是*移轉*資料夾中名為的檔案*&lt;時間戳記&gt;\_InitialCreate.cs*。 `Up`方法`InitialCreate`類別會建立對應至資料模型實體集，資料庫資料表和`Down`方法會刪除它們。

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。 當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。

這是當您輸入時所建立的初始移轉`add-migration InitialCreate`命令。 參數 (`InitialCreate`在範例中) 用於檔案名稱，而且可以是任何您想要的結果，您通常會選擇的單字或片語，概括未移轉正在進行。 例如，您可能會在此名稱稍後進行移轉&quot;AddDepartmentTable&quot;。

如果在您建立初始移轉時資料庫已經存在，系統會產生資料庫建立程式碼，但不需要執行，因為資料庫已經符合資料模型。 當您將應用程式部署到資料庫尚未存在的其他環境中時，即會執行這個程式碼以建立您的資料庫；建議您先進行測試。 這就是為什麼您變更先前的連接字串中的資料庫名稱&mdash;以便移轉可以建立一個新從零開始。

1. 在 [ **Package Manager Console** ] 視窗中，輸入下列命令：

    `update-database`

    `update-database`命令會執行`Up`方法來建立資料庫然後執行`Seed`方法來填入資料庫。 相同的程序會自動執行在生產環境中之後部署應用程式，如您所見下一節。
2. 使用**伺服器總管**來檢查資料庫，如同在第一個教學課程中，並執行的應用程式，以確認所有項目仍可運作相同和以前一樣。

## <a name="deploy-to-azure"></a>部署到 Azure

到目前為止應用程式具有已在本機執行 IIS Express 中的開發電腦上。 若要使其可供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 在本節的教學課程中，您會將它部署至 Azure。 本節為選擇性;您可以略過這個，並繼續進行下列教學課程中，或者您可以調整的指示在本節中不同的主機服務提供者的您所選擇。

### <a name="use-code-first-migrations-to-deploy-the-database"></a>使用 Code First 移轉來部署資料庫

若要部署資料庫，您將使用 Code First 移轉。 當您建立發行設定檔，您用來設定從 Visual Studio 部署的設定時，您會選取核取方塊**更新資料庫**。 此設定會使部署程序會自動設定應用程式*Web.config*檔案在目的地伺服器上，讓 Code First 會使用`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不會執行任何與資料庫在部署程序時，它會將您的專案複製到目的地伺服器。 當您執行部署的應用程式並在部署後第一次存取資料庫時，Code First 會檢查資料庫是否符合資料模型。 如果不相符，Code First 會自動建立資料庫 （如果尚不存在） 或更新為最新版本的資料庫結構描述 （如果資料庫存在，但不符合模型）。 如果應用程式實作移轉`Seed`方法，在方法執行之後建立資料庫，或在更新結構描述。

移轉`Seed`方法會將測試資料。 如果您已部署至生產環境中，您必須變更`Seed`方法，讓它只會插入您想要插入到您的生產資料庫的資料。 比方說，您目前的資料模型中可能會想要在開發資料庫的實際課程但虛構的學生。 您可以撰寫`Seed`負載皆在開發中，並再標記為註解虛構的學生在部署至生產環境之前的方法。 或者您可以撰寫`Seed`方法來載入只有課程，並使用應用程式的 UI 手動輸入虛構的學生在測試資料庫。

### <a name="get-an-azure-account"></a>取得 Azure 帳戶

您將需要 Azure 帳戶。 如果您還沒有的話，但您沒有 Visual Studio 訂用帳戶，您可以[啟用您的訂用帳戶權益](https://azure.microsoft.com/pricing/member-offers/credit-for-visual-studio-subscribers/
)。 否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱 < [Azure 免費試用](https://azure.microsoft.com/free/)。

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>在 Azure 中建立網站和 SQL database

在 Azure web 應用程式會執行在共用主控環境中，這表示它會在與其他 Azure 用戶端所共用的虛擬機器 (Vm) 上執行。 共用主控環境是開始在雲端中的低成本方法。 稍後，如果您的 web 流量增加，應用程式可擴充至專用的 Vm 上執行依需求。 若要深入了解價格選項適用於 Azure App Service，請閱讀[App Service 定價](https://azure.microsoft.com/pricing/details/app-service/)。

您會將資料庫部署到 Azure SQL database。 SQL database 是建置在 SQL Server 技術的雲端型關聯式資料庫服務。 工具和使用 SQL Server 的應用程式也可以使用 SQL database。

1. 在[Azure 管理入口網站](https://portal.azure.com)，選擇**建立資源**左側索引標籤，然後選擇**查看所有**上**新增**窗格 （或* 刀鋒視窗*) 若要查看所有可用的資源。 選擇**Web 應用程式 + SQL**中**Web**一節**所有項目**刀鋒視窗。 最後，選擇**建立**。

    ![在 Azure 入口網站中建立資源](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/create-azure-resource.png)

   要建立新的表單**新 Web 應用程式 + SQL**資源隨即開啟。

2. 中，輸入字串**應用程式名稱**方塊，以使用唯一的 URL 做為您的應用程式。 完整的 URL 將包含您在此處輸入再加上 Azure App Service 的預設網域 (。 azurewebsites.net)。 如果**應用程式名稱**已被使用，精靈會通知您有紅色*應用程式名稱不提供*訊息。 如果**應用程式名稱**是可用，您會看到綠色的核取記號。

3. 在 **訂用帳戶**方塊中，選擇您想在其中的 Azure 訂用帳戶**App Service**存放。

4. 在 **資源群組**文字方塊中，選擇資源群組，或建立新的帳戶。 此設定會指定您的網站會以哪個資料中心。 如需資源群組的詳細資訊，請參閱[資源群組](/azure/azure-resource-manager/resource-group-overview#resource-groups)。

5. 建立新**App Service 方案**依序按一下*應用程式服務 區段*，**新建**，並填寫**App Service 方案**（可以是相同的名稱App Service 中)，**位置**，並**定價層**（沒有可用的選項）。

6. 按一下  **SQL Database**，然後選擇**建立新的資料庫**或選取現有的資料庫。

7. 在 **名稱**方塊中，輸入您的資料庫的名稱。
8. 按一下 **目標伺服器**方塊，然後按**建立新的伺服器**。 或者，如果您先前建立的伺服器，您就可以從可用伺服器清單選取該伺服器。
9. 選擇**定價層**區段中，選擇*免費*。 如果需要額外的資源，可以隨時相應資料庫。 若要進一步了解在 Azure SQL 定價，請參閱[Azure SQL Database 定價](https://azure.microsoft.com/pricing/details/sql-database/managed/)。
10. 修改[定序](/sql/relational-databases/collations/collation-and-unicode-support)視。
11. 輸入系統管理員**SQL 系統管理員使用者名稱**並**SQL 管理員密碼**。

   - 如果您選取**新的 SQL Database 伺服器**，定義新的名稱和密碼，稍後會用到存取資料庫時。
   - 如果您選取您先前建立的伺服器時，輸入該伺服器的認證。

12. 使用 Application Insights 的 App Service 時，可以啟用遙測收集。 使用小設定時，Application Insights 會收集重要的事件、 例外狀況、 相依性、 要求，以及追蹤資訊。 若要深入了解 Application Insights，請參閱[Azure 監視器](https://azure.microsoft.com/services/monitor/)。
13. 按一下 **建立**底部指出您已完成。

    在管理入口網站會回到儀表板 頁面中，而**通知**區域在頁面頂端會顯示正在建立網站。 一段時間之後 （通常少於一分鐘），還有部署成功的通知。 在左側導覽列中，新的 App Service 會出現在**應用程式服務**一節和新的 SQL 資料庫會出現在**SQL 資料庫**一節。

### <a name="deploy-the-app-to-azure"></a>將應用程式部署至 Azure

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管**，然後選取**發佈**從內容功能表。

2. 在上**挑選發行目標**頁面上，選擇**App Service** ，然後**選取現有**，然後選擇**發行**。

    ![挑選發行目標 頁面](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/publish-select-existing-azure-app-service.png)

3. 如果您先前尚未在 Visual Studio 中加入您 Azure 訂用帳戶，請在螢幕上執行步驟。 這些步驟可讓 Visual Studio 能夠連線到您 Azure 訂用帳戶因此的清單**應用程式服務**會包含您的網站。

4. 在  **App Service**頁面上，選取**訂用帳戶**新增至 App Service。 底下**檢視**，選取**資源群組**。 展開您新增應用程式服務的資源群組，然後選取 App Service。 選擇**確定**發行應用程式。

5. **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。

6. 部署成功時，預設瀏覽器會自動開啟至已部署的網站的 URL。

    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/cloud-app-browser.png)

    您的應用程式現在正在雲端中執行。

在此時*SchoolContext*資料庫具有 Azure SQL 資料庫中已建立，因為您選取**Execute Code First Migrations （在應用程式啟動時執行）**。 *Web.config*在已部署的網站上的檔案已變更，讓[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始設定式會執行您的程式碼中讀取或寫入資料的資料庫 （發生在第一次當您選取**學生** 索引標籤上):

![Web.config 檔案摘錄](https://asp.net/media/4367421/mig.png)

部署程序也會建立新的連接字串 *(SchoolContext\_DatabasePublish*) 對要用於更新資料庫結構描述和植入資料庫的 Code First 移轉。

![在 Web.config 檔案中的連接字串](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

您可以找到 Web.config 檔案在電腦上部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。您可以存取已部署*Web.config*檔案本身使用 FTP。 如需指示，請參閱[使用 Visual Studio 的 ASP.NET Web 部署：部署程式碼更新](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update)。 遵循指示以開始 「 若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。 」

> [!NOTE]
> Web 應用程式不會實作安全性，以便找到 URL 的任何人可以變更的資料。 如需有關如何保護網站上的指示，請參閱[將使用成員資格、 OAuth 和 SQL database 的安全 ASP.NET MVC 應用程式部署至 Azure](/aspnet/core/security/authorization/secure-data)。 您可以防止其他人使用網站服務使用 Azure 管理入口網站或**伺服器總管**Visual Studio 中。

![停止應用程式服務 功能表項目](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/server-explorer-stop-app-service.png)

## <a name="advanced-migrations-scenarios"></a>進階的移轉案例

如果您部署資料庫中執行本教學課程中所示的會自動移轉，而且您要部署到多部伺服器執行的 web 站台，您可以取得多部伺服器，嘗試在同一時間執行移轉。 移轉是不可部分完成的所以如果兩部伺服器會嘗試執行相同的移轉，其中將會成功，而且其他將會失敗 （假設作業無法完成兩次）。 在案例中，如果您想要避免這些問題，您可以手動呼叫移轉並設定自己的程式碼，讓它只會發生一次。 如需詳細資訊，請參閱 <<c0> [ 執行，並從程式碼的指令碼移轉](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/)Rowan Miller 部落格上並[Migrate.exe](/ef/ef6/modeling/code-first/migrations/migrate-exe) （適用於從命令列執行移轉）。

如需其他移轉案例的資訊，請參閱[移轉螢幕錄製影片系列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。

## <a name="code-first-initializers"></a>程式碼的第一個初始設定式

在 [部署] 區段中，您已看到[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)所使用的初始設定式。 程式碼第一次也提供其他初始設定式，包括[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （預設值）， [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) （其中您稍早使用） 和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始設定式可用於設定 單元測試的條件。 您也可以撰寫您自己的初始設定式，而您可以呼叫初始設定式明確地如果您不想等候，直到應用程式讀取或寫入資料庫。

如需初始設定式的詳細資訊，請參閱[了解在 Entity Framework Code First 資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)並在本書第 6 章[Programming Entity Framework:Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 和 Rowan Miller。

## <a name="get-the-code"></a>取得程式碼

[下載已完成的專案](https://webpifeed.blob.core.windows.net/webpifeed/Partners/ASP.NET%20MVC%20Application%20Using%20Entity%20Framework%20Code%20First.zip)

## <a name="additional-resources"></a>其他資源

其他 Entity Framework 資源連結可在[ASP.NET 資料存取-建議資源](xref:whitepapers/aspnet-data-access-content-map)。

## <a name="next-steps"></a>後續步驟

在本教學課程中，您已：

> [!div class="checklist"]
> * 啟用的 Code First 移轉
> * 在 Azure 中 （選擇性） 將應用程式部署

請前往下一篇文章，以了解如何建立 ASP.NET MVC 應用程式的更複雜的資料模型。
> [!div class="nextstepaction"]
> [建立更複雜的資料模型](creating-a-more-complex-data-model-for-an-asp-net-mvc-application.md)
