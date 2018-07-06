---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
ms.date: 02/15/2013
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 88652a33d5367241fec4d442e9deb15d88a096c9
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37817619"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新
====================
藉由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

在本教學課程中，您讓資料庫變更和相關程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署至測試、 預備及生產環境。

本教學課程先示範如何更新由 Code First 移轉，資料庫並稍後它顯示如何使用 dbDacFx 提供者更新資料庫。

提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>使用 Code First 移轉來部署資料庫更新

在本節中，您新增的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。 然後，您會更新，以顯示新的資料行顯示講師資料的頁面。 最後，您將變更部署到測試、 預備及生產環境。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>將資料行新增至應用程式資料庫中的資料表

1. 在  *ContosoUniversity.DAL*專案中，開啟*Person.cs* ，並在結尾新增下列屬性`Person`（應該會有兩個左右大括號後面） 的類別：

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    接下來，更新`Seed`方法，讓它提供新的資料行的值。 開啟*Migrations\Configuration.cs*並取代程式碼區塊開頭`var instructors = new List<Instructor>`其中包括出生日期資訊的下列程式碼區塊：

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. 建置方案，然後再開啟**Package Manager Console**視窗。 請確定 ContosoUniversity.DAL 仍已選取作為**預設專案**。
3. 在  **Package Manager Console**視窗中，選取**ContosoUniversity.DAL**做為**預設專案**，然後輸入下列命令：

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。 `Up`方法會建立資料行，當您在實作變更，而`Down`方法會刪除資料行，您會回復時變更。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. 建置方案，，然後輸入下列命令，在**Package Manager Console**視窗 （請確定 ContosoUniversity.DAL 專案仍為已選取）：

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework 執行`Up`方法，然後執行`Seed`方法。

### <a name="display-the-new-column-in-the-instructors-page"></a>Instructors 頁面中顯示新的資料行

1. 在 [ContosoUniversity] 專案中，開啟*Instructors.aspx*並加入新的 [範本] 欄位顯示的出生日期。 請將它加入的項目指派的雇用日期及 office 之間：

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    （如果程式碼縮排不同步，您可以按 CTRL-K 然後按 CTRL-D 會自動重新格式化檔案。）
2. 執行應用程式，然後按一下**講師**連結。

    當頁面載入時，您就會看到它有新的出生日期欄位。

    ![Instructors 頁面與出生日期](deploying-a-database-update/_static/image2.png)
3. 關閉瀏覽器。

### <a name="deploy-the-database-update"></a>部署資料庫更新

1. 在 [**方案總管] 中**選取 ContosoUniversity 專案。
2. 在  **Web 單鍵發佈**工具列上，按一下**測試**發行設定檔，然後按一下**發佈 Web**。 (如果已停用工具列，選取 [ContosoUniversity 專案中的**方案總管] 中**。)

    Visual Studio 會部署更新的應用程式和瀏覽器開啟至首頁。
3. 執行**講師**頁面，確認已成功部署更新。

    當應用程式嘗試存取此頁面的資料庫時，Code First 會更新資料庫結構描述和執行`Seed`方法。 當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。
4. 在**Web 單鍵發佈**工具列上，按一下**預備**發行設定檔，然後按一下**發佈 Web**。
5. 執行**講師**來確認更新已成功部署至預備環境中的頁面。
6. 在**Web 單鍵發佈**工具列上，按一下**生產**發行設定檔，然後按一下**發佈 Web**。
7. 執行**講師**來確認更新已成功部署的生產環境中的頁面。

    包含資料庫變更的實際生產應用程式更新為您通常也會需要應用程式離線部署期間使用*應用程式\_offline.htm*，如您在上一個教學課程中看到。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>將資料庫更新部署所使用的 dbDacFx 提供者

在本節中，您會新增*註解*資料行*使用者*成員資格資料庫中資料表，並建立頁面，可讓您顯示和編輯每個使用者的註解。 然後您將變更部署到測試、 預備及生產環境。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>將資料行新增至成員資格資料庫中的資料表

1. 在 Visual Studio 中開啟**SQL Server 物件總管**。
2. 依序展開 **(localdb) \v11.0**，展開**資料庫**，展開**aspnet ContosoUniversity** (不**aspnet-ContosoUniversity-生產**)然後展開**資料表**。

    如果您沒有看到 **(localdb) \v11.0**下方**SQL Server**節點，以滑鼠右鍵按一下**SQL Server**節點，然後按一下**加入 SQL Server**。 在**連接到伺服器** 對話方塊中輸入 *(localdb) \v11.0*做為**伺服器名稱**，然後按一下**Connect**。

    如果您沒有看到**aspnet ContosoUniversity**，請執行專案，並使用登入*admin*認證 (密碼*devpwd*)，然後重新整理**SQL Server 物件總管**視窗。
3. 以滑鼠右鍵按一下**使用者**資料表，然後按一下**檢視表設計工具**。

    ![SSOX 檢視表設計工具](deploying-a-database-update/_static/image3.png)
4. 在設計工具中，加入*註解*資料行並使它成為 *& lt;languagekeyword>nvarchar(128)</languagekeyword>* 而且可以是 null，然後按一下**更新**。

    ![加入註解的資料行](deploying-a-database-update/_static/image4.png)
5. 在 **預覽資料庫更新**方塊中，按一下**更新資料庫**。

    ![預覽資料庫更新](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>建立頁面，即可顯示和編輯新的資料行

1. 在**方案總管] 中**，以滑鼠右鍵按一下**帳戶**資料夾中 [ContosoUniversity] 專案中，按一下 [**新增**，然後按一下**新項目**.
2. 建立新**Web 表單使用主版頁面**並將它命名*UserInfo.aspx*。 接受預設值*Site.Master*主版頁面檔案。
3. 複製到下列標記`MainContent``Content`項目 (3 的最後一個`Content`項目):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 以滑鼠右鍵按一下*UserInfo.aspx*頁面，然後按一下**瀏覽器中的檢視**。
5. 登入您*系統管理員*使用者認證 (密碼*devpwd*) 並將一些註解新增至使用者驗證頁面是否正常運作。

    ![使用者資訊頁面](deploying-a-database-update/_static/image6.png)
6. 關閉瀏覽器。

## <a name="deploy-the-database-update"></a>部署資料庫更新

若要部署使用 dbDacFx 提供者，您只需要選取**更新資料庫**發行設定檔中的選項。 不過，初始的部署使用此選項時您也設定一些其他的 SQL 指令碼來執行： 這些是仍在設定檔中，您必須防止它們執行一次。

1. 開啟**發佈 Web**精靈，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。
2. 選取 **測試**設定檔。
3. 按一下 [**設定**] 索引標籤。
4. 底下**DefaultConnection**，選取**更新資料庫**。
5. 停用您設定要執行的初始部署的其他指令碼：

    1. 按一下 **設定資料庫更新**。
    2. 在 [**設定資料庫更新**] 對話方塊中，清除核取方塊旁*Grant.sql*並*aspnet-資料-dev.sql*。
    3. 按一下 [ **關閉**]。
6. 按一下 [**預覽**] 索引標籤。
7. 底下**資料庫**和右邊**DefaultConnection**，按一下 **預覽資料庫**連結。

    ![資料庫預覽](deploying-a-database-update/_static/image7.png)

    預覽視窗會顯示指令碼，將會執行目的地資料庫進行比對來源資料庫結構描述的資料庫結構描述中。 指令碼中包含 ALTER TABLE 命令，將新的資料行。
8. 關閉**Database 預覽**對話方塊，然後再按一下**發佈**。

    Visual Studio 會部署更新的應用程式和瀏覽器開啟至首頁。
9. 執行使用者資訊頁面 (新增*Account/UserInfo.aspx*首頁 url) 來確認更新已成功部署。 您必須輸入登入*系統管理員*並*devpwd*。

    資料表中的資料不會部署預設的情況下，而且您未設定資料部署指令碼執行，因此找不到您在開發中加入註解。 您可以在預備環境中確認變更已部署到資料庫，而且網頁正確運作，現在加入新的註解。
10. 請遵循相同的程序，將部署至預備和生產環境。

    別忘了停用額外的指令碼。 相較於測試設定檔的唯一差異是，您將會停用在預備環境中的只有一個指令碼和實際執行設定檔，因為它們已設定成只能在執行*aspnet-prod-data.sql*。

    預備和生產環境的認證是系統管理員和 prodpwd。

    包含資料庫變更的實際生產應用程式更新為您通常也會需要應用程式離線部署期間上傳*應用程式\_offline.htm*之前發行後將其刪除之後，如在您所見[先前的教學課程](deploying-a-code-update.md)。

## <a name="summary"></a>總結

您現在已部署應用程式更新包含使用 Code First 移轉和 dbDacFx 提供者的資料庫變更。

![Instructors 頁面與出生日期](deploying-a-database-update/_static/image8.png)

![使用者資訊頁面](deploying-a-database-update/_static/image9.png)

下一個教學課程會示範如何使用命令列執行部署。

> [!div class="step-by-step"]
> [上一頁](deploying-a-code-update.md)
> [下一頁](command-line-deployment.md)
