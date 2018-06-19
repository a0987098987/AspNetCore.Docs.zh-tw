---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 3020cfa19bf12f21c6d42a77ed257595431b4e86
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30892617"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a>使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新
====================
由[Tom Dykstra](https://github.com/tdykstra)

[下載入門專案](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。 數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。


## <a name="overview"></a>總覽

本教學課程中，您在資料庫變更及相關的程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署到測試、 預備及實際執行環境。

本教學課程先示範如何更新由 Code First 移轉，管理資料庫，然後稍後它示範如何使用 dbDacFx 提供者更新資料庫。

提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a>使用 Code First 移轉來部署資料庫更新

在本節中，您加入的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。 然後您可以更新，使其顯示新的資料行顯示講師資料的頁面。 最後，您將變更部署到測試、 預備及生產環境。

### <a name="add-a-column-to-a-table-in-the-application-database"></a>將資料行加入至應用程式資料庫中的資料表

1. 在*ContosoUniversity.DAL*專案中，開啟*Person.cs*和結尾加入下列屬性`Person`類別 （應該會有兩個左右大括號後面）：

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    接下來，更新`Seed`方法，使其提供新的資料行的值。 開啟*Migrations\Configuration.cs*取代開始的程式碼區塊和`var instructors = new List<Instructor>`與下列程式碼區塊其中包括出生日期資訊：

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. 建置方案，然後再開啟**Package Manager Console**視窗。 請確定 ContosoUniversity.DAL 仍然做為選取狀態**預設專案**。
3. 在**Package Manager Console**視窗中，選取**ContosoUniversity.DAL**為**預設專案**，然後輸入下列命令：

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。 `Up`方法會建立資料行，當您在實作變更，而`Down`方法刪除的資料行，您會回復時變更。

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. 建置方案，，然後輸入下列命令中的**Package Manager Console**視窗 （請確定已選取 ContosoUniversity.DAL 專案）：

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    Entity Framework 執行`Up`方法，然後執行`Seed`方法。

### <a name="display-the-new-column-in-the-instructors-page"></a>講師頁面中顯示新的資料行

1. 在 ContosoUniversity 專案中，開啟*Instructors.aspx*並加入新的範本欄位，以顯示出生日期。 雇用日期和 office 指派的項目之間，將它加入：

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    （如果程式碼縮排不同步，您可以按 CTRL-K 然後 CTRL-D 設為自動重新格式化檔案）。
2. 執行應用程式，然後按一下**講師**連結。

    當網頁載入時，您就會看到有新出生日期欄位。

    ![Birthdate 講師頁面](deploying-a-database-update/_static/image2.png)
3. 關閉瀏覽器。

### <a name="deploy-the-database-update"></a>將資料庫更新部署

1. 在**方案總管 中**選取 ContosoUniversity 專案。
2. 在**Web 一個按一下 Publish**工具列上，按一下**測試**發行設定檔，然後按一下 **發行 Web**。 (如果已停用工具列，ContosoUniversity 中選取專案**方案總管 中**。)

    Visual Studio 會部署更新的應用程式，並瀏覽器開啟至首頁。
3. 執行**講師**頁面，即可確認成功部署更新。

    當應用程式嘗試存取此頁面的資料庫時，Code First 更新資料庫結構描述並執行`Seed`方法。 當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。
4. 在**Web 一個按一下 Publish**工具列上，按一下**臨時**發行設定檔，然後按一下 **發行 Web**。
5. 執行**講師**暫存以確認更新已成功部署中的頁面。
6. 在**Web 一個按一下 Publish**工具列上，按一下**生產**發行設定檔，然後按一下 **發行 Web**。
7. 執行**講師**生產環境，以確認更新已成功部署中的頁面。

    包含資料庫變更的實際生產應用程式更新為您可以通常也會採用應用程式離線在部署期間使用*應用程式\_offline.htm*，如您在上一個教學課程中看到。

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a>將資料庫更新部署使用 dbDacFx 提供者

在本節中，您將*註解*欄*使用者*成員資格資料庫中資料表，並建立可讓您顯示和編輯每個使用者的註解的頁面。 然後您將變更部署到測試、 預備及生產環境。

### <a name="add-a-column-to-a-table-in-the-membership-database"></a>加入成員資格資料庫中的資料表資料行

1. 在 Visual Studio 中開啟**SQL Server 物件總管**。
2. 展開 **(localdb) \v11.0**，依序展開**資料庫**，依序展開**aspnet ContosoUniversity** (不**生產 ContosoUniversity aspnet 環境**)然後展開**資料表**。

    如果您沒有看到 **(localdb) \v11.0**下**SQL Server**  節點，以滑鼠右鍵按一下**SQL Server**節點，然後按一下**加入 SQL Server**。 在**連接到伺服器**對話方塊中輸入 *(localdb) \v11.0*為**伺服器名稱**，然後按一下 **連接**。

    如果您沒有看到**aspnet ContosoUniversity**、 執行專案，並使用登入*admin*認證 (密碼*devpwd*)，然後重新整理**SQL Server 物件總管**視窗。
3. 以滑鼠右鍵按一下**使用者**資料表，然後按一下 **檢視表設計工具**。

    ![SSOX 檢視表設計工具](deploying-a-database-update/_static/image3.png)
4. 在設計工具中，加入*註解*資料行，並將其*nvarchar （128)* 而且可以是 null，然後按一下 **更新**。

    ![加入註解的資料行](deploying-a-database-update/_static/image4.png)
5. 在**預覽資料庫更新**方塊中，按一下**更新資料庫**。

    ![預覽資料庫更新](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a>建立頁面，即可顯示和編輯新的資料行

1. 在**方案總管] 中**，以滑鼠右鍵按一下**帳戶**資料夾在 ContosoUniversity 專案中，按一下**新增**，然後按一下 [**新項目**.
2. 建立新**Web 表單使用主版頁面**並將其命名*UserInfo.aspx*。 接受預設值*Site.Master*主版頁面檔案。
3. 複製到下列標記`MainContent``Content`元素 (3 的最後一個`Content`項目):

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. 以滑鼠右鍵按一下*UserInfo.aspx*頁面上，按一下 **瀏覽器中的檢視**。
5. 登入您*admin*使用者認證 (密碼*devpwd*) 並將一些註解加入至使用者驗證頁面是否正常運作。

    ![使用者資訊頁面](deploying-a-database-update/_static/image6.png)
6. 關閉瀏覽器。

## <a name="deploy-the-database-update"></a>將資料庫更新部署

若要部署使用 dbDacFx 提供者，您只需要選取**更新資料庫**發行設定檔中的選項。 但是在最初部署使用此選項時您也設定一些其他的 SQL 指令碼，執行： 這些是仍在設定檔中，您必須防止它們執行一次。

1. 開啟**發行 Web** ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下精靈**發行**。
2. 選取**測試**設定檔。
3. 按一下**設定** 索引標籤。
4. 在下**DefaultConnection**，選取**更新資料庫**。
5. 停用您設定要執行的初始部署的其他指令碼：

    1. 按一下**設定資料庫更新**。
    2. 在**設定資料庫更新**對話方塊中，清除核取方塊旁的  *Grant.sql*和*aspnet-資料-dev.sql*。
    3. 按一下 [ **關閉**]。
6. 按一下**預覽** 索引標籤。
7. 在下**資料庫**和右邊的**DefaultConnection**，按一下 **預覽資料庫**連結。

    ![資料庫預覽](deploying-a-database-update/_static/image7.png)

    預覽視窗會顯示將會執行目的地資料庫，以便符合來源資料庫的結構描述的資料庫結構描述中的指令碼。 指令碼中包含 ALTER TABLE 命令，將新的資料行。
8. 關閉**Database 預覽**對話方塊，然後按一下**發行**。

    Visual Studio 會部署更新的應用程式，並瀏覽器開啟至首頁。
9. 執行使用者資訊頁面 (新增*Account/UserInfo.aspx*首頁 url) 以確認更新已成功部署。 您必須輸入登入*admin*和*devpwd*。

    根據預設，未部署資料表中的資料，您未設定資料部署指令碼執行，讓您將不會尋找您在開發中加入註解。 在您可以加入新的註解現在暫存以確認變更已部署到資料庫，且網頁正確運作。
10. 請遵循相同的程序，將部署到預備和生產環境。

    別忘了停用這些額外的指令碼。 相較於測試設定檔的唯一差異是，您將會停用在預備環境中的只有一個指令碼和實際執行設定檔，因為它們已設定為只執行*aspnet-prod-data.sql*。

    預備和生產環境的認證是系統管理員，並 prodpwd。

    包含資料庫變更的實際生產應用程式更新為您可以通常也會採用應用程式離線在部署期間上傳*應用程式\_offline.htm*之前發行後將其刪除之後，如同[上一個教學課程](deploying-a-code-update.md)。

## <a name="summary"></a>總結

您現在已部署包含使用 Code First 移轉和 dbDacFx 提供者的資料庫變更的應用程式更新。

![Birthdate 講師頁面](deploying-a-database-update/_static/image8.png)

![使用者資訊頁面](deploying-a-database-update/_static/image9.png)

下一個教學課程會示範如何使用命令列執行部署。

> [!div class="step-by-step"]
> [上一頁](deploying-a-code-update.md)
> [下一頁](command-line-deployment.md)
