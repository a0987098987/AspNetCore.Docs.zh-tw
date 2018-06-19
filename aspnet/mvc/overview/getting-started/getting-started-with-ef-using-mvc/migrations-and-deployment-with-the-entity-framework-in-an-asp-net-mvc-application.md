---
uid: mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
title: Code First 移轉，以及使用的 Entity Framework 中的 ASP.NET MVC 應用程式部署 |Microsoft 文件
author: tdykstra
description: Contoso 大學範例 web 應用程式示範如何建立 ASP.NET MVC 5 應用程式使用 Entity Framework 6 Code First 和 Visual Studio...
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/07/2014
ms.topic: article
ms.assetid: d4dfc435-bda6-4621-9762-9ba270f8de4e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/getting-started/getting-started-with-ef-using-mvc/migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application
msc.type: authoredcontent
ms.openlocfilehash: 04d393edca0469df140f06a7d083a48aa8f84b65
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879552"
---
<a name="code-first-migrations-and-deployment-with-the-entity-framework-in-an-aspnet-mvc-application"></a>Code First 移轉，以及使用的 Entity Framework 中的 ASP.NET MVC 應用程式部署
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載完成的專案](http://code.msdn.microsoft.com/ASPNET-MVC-Application-b01a9fe8)或[下載 PDF](http://download.microsoft.com/download/0/F/B/0FBFAA46-2BFD-478F-8E56-7BF3C672DF9D/Getting%20Started%20with%20Entity%20Framework%206%20Code%20First%20using%20MVC%205.pdf)

> Contoso 大學範例 web 應用程式示範如何建立使用 Entity Framework 6 Code First 和 Visual Studio 2013 的 ASP.NET MVC 5 應用程式。 如需教學課程系列的資訊，請參閱[本系列的第一個教學課程](creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)。

到目前為止已被本機執行應用程式在 IIS Express 在開發電腦上。 若要讓實際的應用程式供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 在本教學課程中，您將部署在 Azure 中雲端 Contoso 大學應用程式。

本教學課程包含下列各節：

- 啟用 Code First 移轉。 移轉功能可讓您變更資料模型，並將變更部署到實際執行，藉由更新資料庫結構描述，而不需要卸除並重新建立資料庫。
- 部署至 Azure。 這個步驟是選擇性的;您可以繼續，其餘教學課程，而不需要部署專案。

我們建議您部署中，使用持續整合程序與原始檔控制，但本教學課程並未涵蓋這些主題。 如需詳細資訊，請參閱[原始檔控制](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/source-control)和[連續整合](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/continuous-integration-and-continuous-delivery)章節[建置真實世界雲端應用程式與 Azure](xref:aspnet/overview/developing-apps-with-windows-azure/building-real-world-cloud-apps-with-windows-azure/introduction)電子書。

## <a name="enable-code-first-migrations"></a>啟用 Code First 移轉

在開發新的應用程式時，您的資料模型會頻繁變更，而每次模型變更時，模型會與資料庫不同步。 您已設定 Entity Framework 自動卸除並重新建立每次變更資料模型資料庫。 當您新增、 移除或變更實體類別或變更您`DbContext`類別，在下次執行應用程式它自動刪除現有的資料庫，然後建立一個新符合模型，並設測試資料。

在您將應用程式部署到生產環境之前，都可以使用上述方法讓資料庫與資料模型保持同步。 當應用程式在生產環境中執行時，通常正在儲存您想要保留，且您不想遺失的所有項目每次您進行變更，例如加入新的資料行的資料。 [Code First 移轉](https://msdn.microsoft.com/data/jj591621)功能藉由啟用程式碼第一次更新資料庫結構描述，而不是卸除並重新建立資料庫來解決這個問題。 在本教學課程中，您要部署應用程式，並準備，您將會啟用移轉。

1. 停用您稍早標記為註解或刪除設定的初始設定式`contexts`您新增至應用程式 Web.config 檔案的項目。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample1.xml?highlight=2,6)]
2. 應用程式中而且*Web.config*檔案中，將連接字串中的資料庫名稱變更為 ContosoUniversity2。

    [!code-xml[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample2.xml?highlight=2)]

    這項變更會設定專案，以讓第一次移轉建立新的資料庫。 這不是必要，但您稍後就會看到的原因是個不錯的主意。
3. 從**工具**功能表上，按一下 **程式庫套件管理員**然後**Package Manager Console**。

    ![Selecting_Package_Manager_Console](https://asp.net/media/4336350/1pm.png)
4. 在`PM>`提示字元輸入下列命令：

    `enable-migrations`  
    `add-migration InitialCreate`

    ![enable-migrations 命令](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image1.png)

    `enable-migrations`命令會建立*移轉*資料夾 ContosoUniversity 專案中，它會將該資料夾中放*configuration.cs 中*設定移轉，您可以編輯的檔案。

    (如果您錯過上述步驟，以引導您變更資料庫名稱時，移轉會尋找現有的資料庫，並自動執行`add-migration`命令。 [確定]，它只是表示部署資料庫之前，您將不會執行測試移轉程式碼。 稍後當您執行`update-database`命令，因為資料庫已經存在，所以會發生任何事。)

    ![Migrations 資料夾](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image2.png)

    初始設定式類別更早版本，您所看到的一樣`Configuration`類別包含`Seed`方法。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample3.cs)]

    目的[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法是，您可以插入或更新之後的程式碼第一次建立或更新資料庫的測試資料。 建立資料庫時，每當資料庫結構描述資料模型變更之後更新時，會呼叫方法。

### <a name="set-up-the-seed-method"></a>設定種子方法

當您卸除並重新建立資料庫的每一個資料模型變更，您使用初始設定式類別的`Seed`插入測試資料，因為每個模型變更之後卸除資料庫的方法和測試的所有資料都會遺失。 Code First 移轉，資料會保留在資料庫變更後的測試，包括中的測試資料[種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法通常不是必要。 事實上，您不想`Seed`方法插入的測試資料，如果您將使用移轉將資料庫部署到生產環境，因為`Seed`方法會在生產環境中執行。 在此情況下您想`Seed`插入資料庫，您需要的資料在生產環境中的方法。 例如，您可能想要包括在實際的部門名稱的資料庫`Department`資料表在生產環境中可以使用應用程式時。

此教學課程中，您將會使用移轉部署，但您`Seed`方法將還是插入測試資料，以便更輕鬆地查看應用程式的功能而不需要以手動方式插入大量資料的運作方式。

1. 取代內容*configuration.cs 中*以下列程式碼，將測試資料載入至新的資料庫檔案。 

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample4.cs)]

    [種子](https://msdn.microsoft.com/library/hh829453(v=vs.103).aspx)方法會採用做為輸入參數，為資料庫物件和方法中的程式碼會使用該物件加入資料庫中的新實體。 每個實體類型，程式碼會建立新實體的集合，將它們加入至適當[DbSet](https://msdn.microsoft.com/library/system.data.entity.dbset(v=vs.103).aspx)屬性，然後按一下 儲存至資料庫的變更。 不需要呼叫[SaveChanges](https://msdn.microsoft.com/library/system.data.entity.dbcontext.savechanges(v=VS.103).aspx)方法之後每個實體群組，做為執行這項，但這麼做可協助您找出問題的來源，如果程式碼寫入資料庫時發生例外狀況。

    某些插入資料的陳述式使用[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法，以執行 「 更新插入 」 作業。 因為`Seed`方法會執行，每次您執行`update-database`命令時，通常每個移轉之後，因為您嘗試新增的資料列，就會有第一個移轉建立資料庫之後插入資料。 「 更新插入 」 作業會防止您嘗試要插入的資料列已經存在，但它會發生的錯誤***會覆寫***資料您測試應用程式時所做的任何變更。 測試資料表中的資料部分可能不會想才會發生： 在某些情況下測試時變更資料時要您的資料庫更新後要保持的變更。 在此情況下您要執行條件式的 insert 作業： 插入資料列，只有當其不存在。 Seed 方法會使用這兩種方法。

    第一個參數傳遞至[AddOrUpdate](https://msdn.microsoft.com/library/system.data.entity.migrations.idbsetextensions.addorupdate(v=vs.103).aspx)方法會指定用來檢查資料列是否已經存在的屬性。 您提供的測試學生資料`LastName`因為每個清單中的最後一個名稱是唯一的可以針對此用途使用屬性：

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample5.cs)]

    此程式碼會假設這些姓氏是唯一。 如果您手動新增一位學生重複最後一個名稱，您會得到下列例外狀況下一次執行移轉。

    序列包含一個以上的項目

    如需如何處理重複的資料，例如兩個名為"Alexander Carson"的學生的資訊，請參閱[植入和偵錯 Entity Framework (EF) Db](https://blogs.msdn.com/b/rickandy/archive/2013/02/12/seeding-and-debugging-entity-framework-ef-dbs.aspx) Rick Anderson 部落格上。 如需有關`AddOrUpdate`方法，請參閱[小心以 EF 4.3 AddOrUpdate 方法](http://thedatafarm.com/blog/data-access/take-care-with-ef-4-3-addorupdate-method/)Julie Lerman 部落格上。

    建立的程式碼`Enrollment`實體會假設您擁有`ID`中的實體中的值`students`集合中，雖然您不會建立集合的程式碼中設定該屬性。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample6.cs?highlight=2)]

    您可以使用`ID`這裡屬性因為`ID`設定值，當您呼叫`SaveChanges`如`students`集合。 EF 自動時，取得索引鍵值插入資料庫中，實體，便會更新`ID`在記憶體中的實體屬性。

    每個加入的程式碼`Enrollment`實體`Enrollments`不會使用實體集`AddOrUpdate`方法。 它會檢查是否實體已經存在，並將實體，如果不存在。 這種方法將會保留您對使用應用程式 UI 來註冊等級變更。 此程式碼迴圈的每個成員`Enrollment`[清單](https://msdn.microsoft.com/library/6sh2ey19.aspx)，如果資料庫中找不到註冊，它會將註冊加入資料庫。 第一次更新資料庫，資料庫將是空的因此它會將加入每個註冊。

    [!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample7.cs)]
2. 建置專案。

### <a name="execute-the-first-migration"></a>執行第一次移轉

當您執行`add-migration`命令，移轉作業產生的程式碼會從頭開始建立資料庫。 此程式碼也是*移轉*資料夾中名為的檔案*&lt;時間戳記&gt;\_InitialCreate.cs*。 `Up`方法`InitialCreate`類別會建立對應至資料模型實體集的資料庫資料表和`Down`方法會刪除它們。

[!code-csharp[Main](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/samples/sample8.cs)]

Migrations 會呼叫 `Up` 方法，以實作移轉所需的資料模型變更。 當您輸入命令以復原更新時，Migrations 會呼叫 `Down` 方法。

這是您輸入時所建立的初始移轉`add-migration InitialCreate`命令。 參數 (`InitialCreate`在範例中) 用於檔案名稱，而且可以是您所要的任何; 您通常會選擇的單字或片語來摘要列出所要完成移轉中的作業。 例如，您可能會命名稍後移轉&quot;AddDepartmentTable&quot;。

如果在您建立初始移轉時資料庫已經存在，系統會產生資料庫建立程式碼，但不需要執行，因為資料庫已經符合資料模型。 當您將應用程式部署到資料庫尚未存在的其他環境中時，即會執行這個程式碼以建立您的資料庫；建議您先進行測試。 這就是為什麼稍早要您變更連接字串中資料庫名稱的原因，這樣一來，移轉即可從頭建立一個資料庫。

1. 在**Package Manager Console**視窗中，輸入下列命令：

    `update-database`

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image3.png)

    `update-database`命令執行`Up`方法來建立資料庫然後執行`Seed`方法來擴展資料庫。 相同的程序會自動執行在生產環境中部署應用程式之後, 您會發現下一節。
2. 使用**伺服器總管**檢查資料庫，您可以如同在第一個教學課程中，並執行應用程式確認運作一切仍然正常相同和以前一樣。

## <a name="deploy-to-azure"></a>部署至 Azure

到目前為止已被本機執行應用程式在 IIS Express 在開發電腦上。 若要使其可供其他人透過網際網路使用，您必須將它部署至 web 主控提供者。 本章節的教學課程中您會將它部署至 Azure。 這個區段是選擇性的。您可以略過此步驟並繼續進行下列教學課程中，或您可以調整這一節的指示不同的主控提供者您選擇。

### <a name="using-code-first-migrations-to-deploy-the-database"></a>使用 Code First 移轉，將資料庫部署

若要將資料庫部署中，您將使用 Code First 移轉。 當您建立發行設定檔讓您從 Visual Studio 部署的設定時，您會選取核取方塊標示為**更新資料庫**。 此設定會自動設定應用程式部署程序*Web.config*檔案在目的地伺服器上，所以第一個程式碼會使用`MigrateDatabaseToLatestVersion`初始設定式類別。

Visual Studio 不執行任何動作與資料庫在部署程序時，它會將您的專案複製到目的地伺服器。 當您執行部署的應用程式，且它存取資料庫第一次部署之後時，Code First 會檢查資料庫是否符合資料模型。 如果不相符，第一個程式碼會自動建立資料庫 （如果尚未存在） 或更新至最新版本的資料庫結構描述 （如果資料庫存在，但不符合模型）。 如果應用程式實作移轉`Seed`方法，在方法執行之後建立資料庫，或在更新結構描述。

您的移轉`Seed`方法會將測試資料。 如果您已部署至生產環境，您必須變更`Seed`方法，使它只會插入您想要插入至您的生產資料庫的資料。 例如，您目前的資料模型中您可以開發資料庫中有實際的課程，但虛構學生。 您可以撰寫`Seed`方法來載入兩者在開發中，並再標記為註解虛構的學生在部署到生產環境之前。 您可以撰寫或`Seed`載入只課程中，並使用應用程式的 UI 手動輸入虛構的學生在測試資料庫中的方法。

### <a name="get-an-azure-account"></a>取得 Azure 帳戶

您將需要 Azure 帳戶。 如果您還沒有其中一個，但您沒有 Visual Studio 訂用帳戶，您可以[啟用訂用帳戶權益](https://azure.microsoft.com/pricing/member-offers/msdn-benefits-details/)。 否則，您可以建立免費的試用帳戶只需要幾分鐘的時間。 如需詳細資訊，請參閱[Azure 免費試用](https://azure.microsoft.com/free/)。

### <a name="create-a-web-site-and-a-sql-database-in-azure"></a>在 Azure 中建立的網站和 SQL 資料庫

在 Azure web 應用程式將會執行共用裝載環境中，這表示它與其他 Azure 用戶端共用的虛擬機器 (Vm) 上執行。 共用裝載環境是開始在雲端中的低成本方法。 更新版本中，如果您的 web 流量的增加，應用程式可以調整以符合需要專用的 Vm 上執行。 若要深入了解定價選項 Azure 應用程式服務，請參閱文件上[Azure 文件](https://azure.microsoft.com/pricing/details/app-service/)

您要將資料庫部署到 Azure SQL Database。 SQL Database 是建立在 SQL Server 技術以雲端為基礎的關聯式資料庫服務。 工具和 SQL Server 所使用的應用程式也使用 SQL 資料庫。

1. 在[Azure 管理入口網站](https://portal.azure.com)，按一下 **新增**在左側的索引標籤上，按一下**查看所有**中新刀鋒視窗中，然後按一下**Web 應用程式與 SQL**中**Web**區段，最後**建立**。

    ![在管理入口網站中的新按鈕](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/CreateWeb-Sql.png)

   **新 Web 應用程式與 SQL-建立**精靈 隨即開啟。

2. 在刀鋒視窗中，輸入字串中的**應用程式名稱**方塊，以使用唯一的 URL 做為您的應用程式。 將會包含哪些您在此處輸入再加上 Azure 應用程式服務的預設網域的完整 URL (。 名稱是.azurewebsites.net)。 如果**應用程式名稱**已經存在，精靈會將通知您有紅色*應用程式名稱不是使用*訊息。 如果**應用程式名稱**為可用，就會出現綠色的核取記號。

    ![建立具有管理入口網站中的資料庫連結](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-WebApp.png)

3. 在**訂用帳戶**下拉式清單中，請選擇 Azure 訂用帳戶在您想在其中**App Service**位於。

4. 在**資源群組**文字方塊中，選擇資源群組，或另外新建一個。 此設定指定您的網站會在執行哪一個資料中心。 如需資源群組的詳細資訊，請閱讀文件上[Azure 文件](https://docs.microsoft.com/azure/azure-resource-manager/resource-group-overview#resource-groups)。
5. 建立新**應用程式服務方案**按一下*應用程式服務 區段*，**建立新**，並填入**App Service 方案**（可以是相同的名稱應用程式服務）**位置**，和**定價層**（沒有可用的選項）。

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-AppService.png)
6. 按一下**SQL Database**，然後選擇 *新建*或選取現有的資料庫

    ![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Create-Database.png)

7. 在**名稱**方塊中，輸入您的資料庫的名稱。
8. 按一下**目標伺服器**方塊中，選取**建立新的伺服器**。 或者，如果您先前建立的伺服器，您可以從可用的伺服器清單中選取該伺服器。
9. 選擇**定價層**區段中，選擇*免費*。 如需其他資源，可以隨時向上資料庫。 若要了解需 Azure SQL 定價的詳細資訊，請閱讀文件上[Azure 文件](https://azure.microsoft.com/pricing/details/sql-database/)。
10. 修改[定序](https://docs.microsoft.com/sql/relational-databases/collations/collation-and-unicode-support)視。
11. 輸入系統管理員**SQL 系統管理員使用者名稱**和**SQL 系統管理員密碼**。 如果您選取**新 SQL Database 伺服器**，您不輸入現有的名稱和密碼，您輸入新名稱和密碼，您現在定義存取資料庫時使用。 如果您選取您先前建立的伺服器，您會輸入該伺服器的認證。
12. 使用 Application Insights 的應用程式服務，可以啟用遙測收集。 組態設定少與 application Insights 收集重要事件、 例外狀況、 相依性、 要求，以及追蹤資訊。 若要深入了解 Application Insights，開始[Azure 文件](https://azure.microsoft.com/services/application-insights/)。
13. 按一下**建立**底部的刀鋒視窗，指出您已完成。
  
    在管理入口網站會回到儀表板 頁面上，而**通知**刀鋒視窗頂端的頁面顯示建立網站。 （通常小於一分鐘），一段時間之後會有部署成功的通知。 在左側導覽列中的新**App Service**會出現在*應用程式服務*區段和新**SQL Database**會出現在*SQL 資料庫* > 一節。

### <a name="deploy-the-application-to-azure"></a>部署至 Azure 應用程式

1. 在 Visual Studio 中的專案上按一下滑鼠右鍵**方案總管 中**選取**發行**從內容功能表。
  
    ![在專案內容功能表中發行](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image10.png)
2. 在**設定檔** 索引標籤**發行 Web**精靈 中，按一下  **Microsoft Azure App Service**。
  
    ![匯入發行設定](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-ChooseTarget.png)
3. 如果您未在 Visual Studio 中先前加入您 Azure 訂用帳戶，請在螢幕上執行步驟。 這些步驟會啟用 Visual Studio 以連接到您的 Azure 訂閱所以清單**應用程式服務**將包含您的網站。
 
4. 選取**訂用帳戶**您加入應用程式服務，然後**App Service 方案**資料夾應用程式服務的一部分，最後再**App Service**本身後面**確定**。

    ![選取應用程式服務](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-AppService.png)
5. 在設定設定檔之後，**連接** 索引標籤會顯示。 按一下**驗證連線**確定設定正確無誤

    ![驗證連接](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Connection.png)
6. 當已驗證的連接時，旁邊顯示綠色的核取記號**驗證連線** 按鈕。 按 [ **下一步**]。
  
    ![已成功驗證的連接](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-SettingsValidated.png)
7. 開啟**遠端的連接字串**底下的下拉式清單**SchoolContext**並選取您所建立之資料庫的連接字串。
8. 選取**更新資料庫**。

    ![設定 索引標籤](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Settings.png)

    此設定會自動設定應用程式部署程序*Web.config*檔案在目的地伺服器上，所以第一個程式碼會使用`MigrateDatabaseToLatestVersion`初始設定式類別。
9. 按 [ **下一步**]。
10. 在**預覽**索引標籤上，按一下 **啟動預覽**。
  
    ![在 [預覽] 索引標籤中的 StartPreview 按鈕](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Preview.png)
  
    索引標籤會顯示將會複製到伺服器的檔案清單。 顯示預覽並不需要發行應用程式但要注意的有用的函式。 在此情況下，您不需要進行任何動作顯示的檔案清單。 下次當您部署此應用程式，這份清單中會變更的檔案。
    ![StartPreview 檔案輸出](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-PreviewLoaded.png)

11. 按一下 [發行] 。
    Visual Studio 會開始將檔案複製到 Azure 伺服器的程序。
12. **輸出**視窗會顯示所執行的部署動作，並回報成功完成部署。
  
    ![報告成功部署的 [輸出] 視窗](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-BuildOutput.png)
13. 部署成功之後，預設瀏覽器會自動開啟至已部署的網站的 URL。
    您所建立的應用程式現在在雲端中執行。 
  
    ![Students_index_page_with_paging](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Publish-Site.png)

此時您*SchoolContext*資料庫已建立 Azure SQL Database 中因為您選取**執行 Code First 移轉 （在應用程式啟動時執行）**。 *Web.config*已部署的網站中檔案已經變更，讓[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)初始設定式會執行第一次讀取或寫入資料 （這發生在資料庫中的程式碼當您選取**學生** 索引標籤):

![](https://asp.net/media/4367421/mig.png)

部署程序也會建立新的連接字串 *(SchoolContext\_DatabasePublish*) 的 Code First 移轉，以用於更新資料庫結構描述和植入資料庫。

![Database_Publish 連接字串](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/image26.png)

您可以找到您自己的電腦上的 Web.config 檔案的部署的版本*ContosoUniversity\obj\Release\Package\PackageTmp\Web.config*。您可以存取已部署*Web.config*檔案本身可以使用 FTP。 如需指示，請參閱[使用 Visual Studio 的 ASP.NET Web 部署： 部署程式碼更新](xref:web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-code-update)。 遵循開頭 「 若要使用 FTP 工具，您需要三件事： FTP URL、 使用者名稱和密碼。 」

> [!NOTE]
> Web 應用程式不會實作安全性，以便尋找此 URL 的任何人都可以變更資料。 如需有關如何保護網站的指示，請參閱[成員資格、 OAuth、 與 SQL Database 的安全的 ASP.NET MVC 應用程式部署至 Azure](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。 您可以防止其他人使用 Azure 管理入口網站使用站台或**伺服器總管**停止站台的 Visual Studio 中。


![](migrations-and-deployment-with-the-entity-framework-in-an-asp-net-mvc-application/_static/Stop-Service.png)

## <a name="advanced-migrations-scenarios"></a>進階的移轉案例

如果您本教學課程中所示的自動執行移轉以部署資料庫，而且您要部署到多部伺服器執行的 web 站台，您可以取得多部伺服器嘗試在相同時間執行移轉。 移轉是不可部分完成的因此如果兩部伺服器會嘗試執行相同的移轉，其中將會成功，其他將會失敗 （假設作業無法完成兩次）。 在案例中，如果您想要避免這些問題，您可以手動呼叫移轉並設定自己的程式碼，讓它只會發生一次。 如需詳細資訊，請參閱[執行，並從程式碼的指令碼移轉](http://romiller.com/2012/02/09/running-scripting-migrations-from-code/)Rowan Miller 部落格上並[Migrate.exe](https://msdn.microsoft.com/data/jj618307) （適用於從命令列執行移轉） MSDN 上。

其他的移轉案例的相關資訊，請參閱[移轉錄影畫面數列](https://blogs.msdn.com/b/adonet/archive/2014/03/12/migrations-screencast-series.aspx)。

## <a name="code-first-initializers"></a>程式碼的第一個初始設定式

在 [部署] 區段中看到[MigrateDatabaseToLatestVersion](https://msdn.microsoft.com/library/hh829476(v=vs.103).aspx)所使用的初始設定式。 程式碼第一次也提供其他初始設定式，包括[CreateDatabaseIfNotExists](https://msdn.microsoft.com/library/gg679221(v=vs.103).aspx) （預設）、 [DropCreateDatabaseIfModelChanges](https://msdn.microsoft.com/library/gg679604(v=VS.103).aspx) （用您稍早） 和[DropCreateDatabaseAlways](https://msdn.microsoft.com/library/gg679506(v=VS.103).aspx)。 `DropCreateAlways`初始設定式可用於設定單元測試的條件。 您也可以撰寫您自己的初始設定式，而且您可以呼叫初始設定式明確若不想等到應用程式讀取或寫入資料庫。 本教學課程以 2013 年 11 月日次您只可以使用 Create and DropCreate 初始設定式之前啟用移轉。 Entity Framework 團隊正在進行移轉，以及在這些初始設定式。

如需初始設定式的詳細資訊，請參閱[Entity Framework Code First 中了解資料庫初始設定式](http://www.codeguru.com/csharp/article.php/c19999/Understanding-Database-Initializers-in-Entity-Framework-Code-First.htm)和活頁簿第 6 章[程式設計 Entity Framework: Code First](http://shop.oreilly.com/product/0636920022220.do) Julie Lerman 由和 Rowan Miller。

## <a name="summary"></a>總結

在本教學課程中，您已經看到如何啟用移轉和部署應用程式。 在下一個教學課程中開始，您將查看更進階的主題，方法是展開的資料模型。

請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。 您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。

Entity Framework 中的其他資源連結位於[ASP.NET 資料存取-建議資源](xref:whitepapers/aspnet-data-access-content-map)。

> [!div class="step-by-step"]
> [上一頁](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/connection-resiliency-and-command-interception-with-the-entity-framework-in-an-asp-net-mvc-application)
> [下一頁](xref:mvc/overview/getting-started/getting-started-with-ef-using-mvc/creating-a-more-complex-data-model-for-an-asp-net-mvc-application)
