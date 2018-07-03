---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/15/2013
ms.topic: article
ms.assetid: 9cad0833-486a-4474-a7f3-7715542ec4ce
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-a-database-update
msc.type: authoredcontent
ms.openlocfilehash: 37b996452dfa619ba1276a1aba562ed7efc579b5
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37389185"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-a-database-update"></a><span data-ttu-id="f7ef5-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署資料庫更新</span><span class="sxs-lookup"><span data-stu-id="f7ef5-103">ASP.NET Web Deployment using Visual Studio: Deploying a Database Update</span></span>
====================
<span data-ttu-id="f7ef5-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="f7ef5-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="f7ef5-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="f7ef5-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="f7ef5-106">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="f7ef5-107">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="f7ef5-108">總覽</span><span class="sxs-lookup"><span data-stu-id="f7ef5-108">Overview</span></span>

<span data-ttu-id="f7ef5-109">在本教學課程中，您讓資料庫變更和相關程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署至測試、 預備及生產環境。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-109">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to the test, staging, and production environments.</span></span>

<span data-ttu-id="f7ef5-110">本教學課程先示範如何更新由 Code First 移轉，資料庫並稍後它顯示如何使用 dbDacFx 提供者更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-110">The tutorial first shows how to update a database that is managed by Code First Migrations, and then later it shows how to update a database by using the dbDacFx provider.</span></span>

<span data-ttu-id="f7ef5-111">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-111">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="deploy-a-database-update-by-using-code-first-migrations"></a><span data-ttu-id="f7ef5-112">使用 Code First 移轉來部署資料庫更新</span><span class="sxs-lookup"><span data-stu-id="f7ef5-112">Deploy a database update by using Code First Migrations</span></span>

<span data-ttu-id="f7ef5-113">在本節中，您新增的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-113">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="f7ef5-114">然後，您會更新，以顯示新的資料行顯示講師資料的頁面。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-114">Then you update the page that displays instructor data so that it displays the new column.</span></span> <span data-ttu-id="f7ef5-115">最後，您將變更部署到測試、 預備及生產環境。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-115">Finally, you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-application-database"></a><span data-ttu-id="f7ef5-116">將資料行新增至應用程式資料庫中的資料表</span><span class="sxs-lookup"><span data-stu-id="f7ef5-116">Add a column to a table in the application database</span></span>

1. <span data-ttu-id="f7ef5-117">在  *ContosoUniversity.DAL*專案中，開啟*Person.cs* ，並在結尾新增下列屬性`Person`（應該會有兩個左右大括號後面） 的類別：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-117">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample1.cs)]

    <span data-ttu-id="f7ef5-118">接下來，更新`Seed`方法，讓它提供新的資料行的值。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-118">Next, update the `Seed` method so that it provides a value for the new column.</span></span> <span data-ttu-id="f7ef5-119">開啟*Migrations\Configuration.cs*並取代程式碼區塊開頭`var instructors = new List<Instructor>`其中包括出生日期資訊的下列程式碼區塊：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

    [!code-csharp[Main](deploying-a-database-update/samples/sample2.cs)]
2. <span data-ttu-id="f7ef5-120">建置方案，然後再開啟**Package Manager Console**視窗。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-120">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="f7ef5-121">請確定 ContosoUniversity.DAL 仍已選取作為**預設專案**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-121">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>
3. <span data-ttu-id="f7ef5-122">在  **Package Manager Console**視窗中，選取**ContosoUniversity.DAL**做為**預設專案**，然後輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-122">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample3.ps1)]

    <span data-ttu-id="f7ef5-123">此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-123">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span> <span data-ttu-id="f7ef5-124">`Up`方法會建立資料行，當您在實作變更，而`Down`方法會刪除資料行，您會回復時變更。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-124">The `Up` method creates the column when you are implementing the change, and the `Down` method deletes the column when you are rolling back the change.</span></span>

    ![AddBirthDate_migration_code](deploying-a-database-update/_static/image1.png)
4. <span data-ttu-id="f7ef5-126">建置方案，，然後輸入下列命令，在**Package Manager Console**視窗 （請確定 ContosoUniversity.DAL 專案仍為已選取）：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-126">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

    [!code-powershell[Main](deploying-a-database-update/samples/sample4.ps1)]

    <span data-ttu-id="f7ef5-127">Entity Framework 執行`Up`方法，然後執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-127">The Entity Framework runs the `Up` method and then runs the `Seed` method.</span></span>

### <a name="display-the-new-column-in-the-instructors-page"></a><span data-ttu-id="f7ef5-128">Instructors 頁面中顯示新的資料行</span><span class="sxs-lookup"><span data-stu-id="f7ef5-128">Display the new column in the Instructors page</span></span>

1. <span data-ttu-id="f7ef5-129">在 [ContosoUniversity] 專案中，開啟*Instructors.aspx*並加入新的 [範本] 欄位顯示的出生日期。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-129">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="f7ef5-130">請將它加入的項目指派的雇用日期及 office 之間：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-130">Add it between the ones for hire date and office assignment:</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample5.aspx?highlight=9-17)]

    <span data-ttu-id="f7ef5-131">（如果程式碼縮排不同步，您可以按 CTRL-K 然後按 CTRL-D 會自動重新格式化檔案。）</span><span class="sxs-lookup"><span data-stu-id="f7ef5-131">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>
2. <span data-ttu-id="f7ef5-132">執行應用程式，然後按一下**講師**連結。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-132">Run the application and click the **Instructors** link.</span></span>

    <span data-ttu-id="f7ef5-133">當頁面載入時，您就會看到它有新的出生日期欄位。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-133">When the page loads, you see that it has the new birth date field.</span></span>

    ![Instructors 頁面與出生日期](deploying-a-database-update/_static/image2.png)
3. <span data-ttu-id="f7ef5-135">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-135">Close the browser.</span></span>

### <a name="deploy-the-database-update"></a><span data-ttu-id="f7ef5-136">部署資料庫更新</span><span class="sxs-lookup"><span data-stu-id="f7ef5-136">Deploy the database update</span></span>

1. <span data-ttu-id="f7ef5-137">在 [**方案總管] 中**選取 ContosoUniversity 專案。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-137">In **Solution Explorer** select the ContosoUniversity project.</span></span>
2. <span data-ttu-id="f7ef5-138">在  **Web 單鍵發佈**工具列上，按一下**測試**發行設定檔，然後按一下**發佈 Web**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-138">In the **Web One Click Publish** toolbar, click the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="f7ef5-139">(如果已停用工具列，選取 [ContosoUniversity 專案中的**方案總管] 中**。)</span><span class="sxs-lookup"><span data-stu-id="f7ef5-139">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

    <span data-ttu-id="f7ef5-140">Visual Studio 會部署更新的應用程式和瀏覽器開啟至首頁。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-140">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
3. <span data-ttu-id="f7ef5-141">執行**講師**頁面，確認已成功部署更新。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-141">Run the **Instructors** page to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="f7ef5-142">當應用程式嘗試存取此頁面的資料庫時，Code First 會更新資料庫結構描述和執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-142">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="f7ef5-143">當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-143">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>
4. <span data-ttu-id="f7ef5-144">在**Web 單鍵發佈**工具列上，按一下**預備**發行設定檔，然後按一下**發佈 Web**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-144">In the **Web One Click Publish** toolbar, click the **Staging** publish profile, and then click **Publish Web**.</span></span>
5. <span data-ttu-id="f7ef5-145">執行**講師**來確認更新已成功部署至預備環境中的頁面。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-145">Run the **Instructors** page in staging to verify that the update was successfully deployed.</span></span>
6. <span data-ttu-id="f7ef5-146">在**Web 單鍵發佈**工具列上，按一下**生產**發行設定檔，然後按一下**發佈 Web**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-146">In the **Web One Click Publish** toolbar, click the **Production** publish profile, and then click **Publish Web**.</span></span>
7. <span data-ttu-id="f7ef5-147">執行**講師**來確認更新已成功部署的生產環境中的頁面。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-147">Run the **Instructors** page in production to verify that the update was successfully deployed.</span></span>

    <span data-ttu-id="f7ef5-148">包含資料庫變更的實際生產應用程式更新為您通常也會需要應用程式離線部署期間使用*應用程式\_offline.htm*，如您在上一個教學課程中看到。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-148">For a real production application update that includes a database change you would also typically take the application offline during deployment by using *app\_offline.htm*, as you saw in the previous tutorial.</span></span>

## <a name="deploy-a-database-update-by-using-the-dbdacfx-provider"></a><span data-ttu-id="f7ef5-149">將資料庫更新部署所使用的 dbDacFx 提供者</span><span class="sxs-lookup"><span data-stu-id="f7ef5-149">Deploy a database update by using the dbDacFx provider</span></span>

<span data-ttu-id="f7ef5-150">在本節中，您會新增*註解*資料行*使用者*成員資格資料庫中資料表，並建立頁面，可讓您顯示和編輯每個使用者的註解。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-150">In this section, you add a *Comments* column to the *User* table in the membership database and create a page that lets you display and edit comments for each user.</span></span> <span data-ttu-id="f7ef5-151">然後您將變更部署到測試、 預備及生產環境。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-151">Then you deploy the changes to test, staging, and production.</span></span>

### <a name="add-a-column-to-a-table-in-the-membership-database"></a><span data-ttu-id="f7ef5-152">將資料行新增至成員資格資料庫中的資料表</span><span class="sxs-lookup"><span data-stu-id="f7ef5-152">Add a column to a table in the membership database</span></span>

1. <span data-ttu-id="f7ef5-153">在 Visual Studio 中開啟**SQL Server 物件總管**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-153">In Visual Studio, open **SQL Server Object Explorer**.</span></span>
2. <span data-ttu-id="f7ef5-154">依序展開 **(localdb) \v11.0**，展開**資料庫**，展開**aspnet ContosoUniversity** (不**aspnet-ContosoUniversity-生產**)然後展開**資料表**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-154">Expand **(localdb)\v11.0**, expand **Databases**, expand **aspnet-ContosoUniversity** (not **aspnet-ContosoUniversity-Prod**) and then expand **Tables**.</span></span>

    <span data-ttu-id="f7ef5-155">如果您沒有看到 **(localdb) \v11.0**下方**SQL Server**節點，以滑鼠右鍵按一下**SQL Server**節點，然後按一下**加入 SQL Server**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-155">If you don't see **(localdb)\v11.0** under the **SQL Server** node, right-click the **SQL Server** node and click **Add SQL Server**.</span></span> <span data-ttu-id="f7ef5-156">在**連接到伺服器** 對話方塊中輸入 *(localdb) \v11.0*做為**伺服器名稱**，然後按一下**Connect**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-156">In the **Connect to Server** dialog box enter *(localdb)\v11.0* as the **Server name**, and then click **Connect**.</span></span>

    <span data-ttu-id="f7ef5-157">如果您沒有看到**aspnet ContosoUniversity**，請執行專案，並使用登入*admin*認證 (密碼*devpwd*)，然後重新整理**SQL Server 物件總管**視窗。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-157">If you don't see **aspnet-ContosoUniversity**, run the project and log in using the *admin* credentials (password is *devpwd*), and then refresh the **SQL Server Object Explorer** window.</span></span>
3. <span data-ttu-id="f7ef5-158">以滑鼠右鍵按一下**使用者**資料表，然後按一下**檢視表設計工具**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-158">Right-click the **Users** table, and then click **View Designer**.</span></span>

    ![SSOX 檢視表設計工具](deploying-a-database-update/_static/image3.png)
4. <span data-ttu-id="f7ef5-160">在設計工具中，加入*註解*資料行並使它成為 *& lt;languagekeyword>nvarchar(128)</languagekeyword>* 而且可以是 null，然後按一下**更新**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-160">In the designer, add a *Comments* column and make it *nvarchar(128)* and nullable, and then click **Update**.</span></span>

    ![加入註解的資料行](deploying-a-database-update/_static/image4.png)
5. <span data-ttu-id="f7ef5-162">在 **預覽資料庫更新**方塊中，按一下**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-162">In the **Preview Database Updates** box, click **Update Database**.</span></span>

    ![預覽資料庫更新](deploying-a-database-update/_static/image5.png)

### <a name="create-a-page-to-display-and-edit-the-new-column"></a><span data-ttu-id="f7ef5-164">建立頁面，即可顯示和編輯新的資料行</span><span class="sxs-lookup"><span data-stu-id="f7ef5-164">Create a page to display and edit the new column</span></span>

1. <span data-ttu-id="f7ef5-165">在**方案總管] 中**，以滑鼠右鍵按一下**帳戶**資料夾中 [ContosoUniversity] 專案中，按一下 [**新增**，然後按一下**新項目**.</span><span class="sxs-lookup"><span data-stu-id="f7ef5-165">In **Solution Explorer**, right-click the **Account** folder in the ContosoUniversity project, click **Add**, and then click **New Item**.</span></span>
2. <span data-ttu-id="f7ef5-166">建立新**Web 表單使用主版頁面**並將它命名*UserInfo.aspx*。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-166">Create a new **Web Form Using Master Page** and name it *UserInfo.aspx*.</span></span> <span data-ttu-id="f7ef5-167">接受預設值*Site.Master*主版頁面檔案。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-167">Accept the default *Site.Master* file as the master page.</span></span>
3. <span data-ttu-id="f7ef5-168">複製到下列標記`MainContent``Content`項目 (3 的最後一個`Content`項目):</span><span class="sxs-lookup"><span data-stu-id="f7ef5-168">Copy the following markup into the `MainContent` `Content` element (the last of the 3 `Content` elements):</span></span>

    [!code-aspx[Main](deploying-a-database-update/samples/sample6.aspx)]
4. <span data-ttu-id="f7ef5-169">以滑鼠右鍵按一下*UserInfo.aspx*頁面，然後按一下**瀏覽器中的檢視**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-169">Right-click the *UserInfo.aspx* page and click **View in Browser**.</span></span>
5. <span data-ttu-id="f7ef5-170">登入您*系統管理員*使用者認證 (密碼*devpwd*) 並將一些註解新增至使用者驗證頁面是否正常運作。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-170">Log in with your *admin* user credentials (password is *devpwd*) and add some comments to a user to verify that the page works correctly.</span></span>

    ![使用者資訊頁面](deploying-a-database-update/_static/image6.png)
6. <span data-ttu-id="f7ef5-172">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-172">Close the browser.</span></span>

## <a name="deploy-the-database-update"></a><span data-ttu-id="f7ef5-173">部署資料庫更新</span><span class="sxs-lookup"><span data-stu-id="f7ef5-173">Deploy the database update</span></span>

<span data-ttu-id="f7ef5-174">若要部署使用 dbDacFx 提供者，您只需要選取**更新資料庫**發行設定檔中的選項。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-174">To deploy by using the dbDacFx provider, you just need to select the **Update database** option in the publish profile.</span></span> <span data-ttu-id="f7ef5-175">不過，初始的部署使用此選項時您也設定一些其他的 SQL 指令碼來執行： 這些是仍在設定檔中，您必須防止它們執行一次。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-175">However, for the initial deployment when you used this option you also configured some additional SQL scripts to run: those are still in the profile and you'll have to prevent them from running again.</span></span>

1. <span data-ttu-id="f7ef5-176">開啟**發佈 Web**精靈，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-176">Open the **Publish Web** wizard by right-clicking the ContosoUniversity project and clicking **Publish**.</span></span>
2. <span data-ttu-id="f7ef5-177">選取 **測試**設定檔。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-177">Select the **Test** profile.</span></span>
3. <span data-ttu-id="f7ef5-178">按一下 [**設定**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-178">Click the **Settings** tab.</span></span>
4. <span data-ttu-id="f7ef5-179">底下**DefaultConnection**，選取**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-179">Under **DefaultConnection**, select **Update database**.</span></span>
5. <span data-ttu-id="f7ef5-180">停用您設定要執行的初始部署的其他指令碼：</span><span class="sxs-lookup"><span data-stu-id="f7ef5-180">Disable the additional scripts that you configured to run for the initial deployment:</span></span>

    1. <span data-ttu-id="f7ef5-181">按一下 **設定資料庫更新**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-181">Click **Configure database updates**.</span></span>
    2. <span data-ttu-id="f7ef5-182">在 [**設定資料庫更新**] 對話方塊中，清除核取方塊旁*Grant.sql*並*aspnet-資料-dev.sql*。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-182">In the **Configure Database Updates** dialog box, clear the check boxes next to *Grant.sql* and *aspnet-data-dev.sql*.</span></span>
    3. <span data-ttu-id="f7ef5-183">按一下 [ **關閉**]。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-183">Click **Close**.</span></span>
6. <span data-ttu-id="f7ef5-184">按一下 [**預覽**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-184">Click the **Preview** tab.</span></span>
7. <span data-ttu-id="f7ef5-185">底下**資料庫**和右邊**DefaultConnection**，按一下 **預覽資料庫**連結。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-185">Under **Databases** and to the right of **DefaultConnection**, click the **Preview database** link.</span></span>

    ![資料庫預覽](deploying-a-database-update/_static/image7.png)

    <span data-ttu-id="f7ef5-187">預覽視窗會顯示指令碼，將會執行目的地資料庫進行比對來源資料庫結構描述的資料庫結構描述中。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-187">The preview window shows the script that will be run in the destination database to make that database schema match the schema of the source database.</span></span> <span data-ttu-id="f7ef5-188">指令碼中包含 ALTER TABLE 命令，將新的資料行。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-188">The script includes an ALTER TABLE command that adds the new column.</span></span>
8. <span data-ttu-id="f7ef5-189">關閉**Database 預覽**對話方塊，然後再按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-189">Close the **Database Preview** dialog box, and then click **Publish**.</span></span>

    <span data-ttu-id="f7ef5-190">Visual Studio 會部署更新的應用程式和瀏覽器開啟至首頁。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-190">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span>
9. <span data-ttu-id="f7ef5-191">執行使用者資訊頁面 (新增*Account/UserInfo.aspx*首頁 url) 來確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-191">Run the UserInfo page (add *Account/UserInfo.aspx* to the home page URL) to verify that the update was successfully deployed.</span></span> <span data-ttu-id="f7ef5-192">您必須輸入登入*系統管理員*並*devpwd*。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-192">You'll have to log in by entering *admin* and *devpwd*.</span></span>

    <span data-ttu-id="f7ef5-193">資料表中的資料不會部署預設的情況下，而且您未設定資料部署指令碼執行，因此找不到您在開發中加入註解。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-193">Data in tables is not deployed by default, and you didn't configure a data deployment script to run, so you won't find the comment that you added in development.</span></span> <span data-ttu-id="f7ef5-194">您可以在預備環境中確認變更已部署到資料庫，而且網頁正確運作，現在加入新的註解。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-194">You can add a new comment now in staging to verify that the change was deployed to the database and the page works correctly.</span></span>
10. <span data-ttu-id="f7ef5-195">請遵循相同的程序，將部署至預備和生產環境。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-195">Follow the same procedure to deploy to staging and production.</span></span>

    <span data-ttu-id="f7ef5-196">別忘了停用額外的指令碼。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-196">Don't forget to disable the extra scripts.</span></span> <span data-ttu-id="f7ef5-197">相較於測試設定檔的唯一差異是，您將會停用在預備環境中的只有一個指令碼和實際執行設定檔，因為它們已設定成只能在執行*aspnet-prod-data.sql*。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-197">The only difference compared to the Test profile is that you will disable only one script in the Staging and Production profiles because they were configured to run only *aspnet-prod-data.sql*.</span></span>

    <span data-ttu-id="f7ef5-198">預備和生產環境的認證是系統管理員和 prodpwd。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-198">The credentials for staging and production are admin and prodpwd.</span></span>

    <span data-ttu-id="f7ef5-199">包含資料庫變更的實際生產應用程式更新為您通常也會需要應用程式離線部署期間上傳*應用程式\_offline.htm*之前發行後將其刪除之後，如在您所見[先前的教學課程](deploying-a-code-update.md)。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-199">For a real production application update that includes a database change you would also typically take the application offline during deployment by uploading *app\_offline.htm* before publishing and deleting it afterward, as you saw in [the previous tutorial](deploying-a-code-update.md).</span></span>

## <a name="summary"></a><span data-ttu-id="f7ef5-200">總結</span><span class="sxs-lookup"><span data-stu-id="f7ef5-200">Summary</span></span>

<span data-ttu-id="f7ef5-201">您現在已部署應用程式更新包含使用 Code First 移轉和 dbDacFx 提供者的資料庫變更。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-201">You've now deployed an application update that included a database change using both Code First Migrations and the dbDacFx provider.</span></span>

![Instructors 頁面與出生日期](deploying-a-database-update/_static/image8.png)

![使用者資訊頁面](deploying-a-database-update/_static/image9.png)

<span data-ttu-id="f7ef5-204">下一個教學課程會示範如何使用命令列執行部署。</span><span class="sxs-lookup"><span data-stu-id="f7ef5-204">The next tutorial shows you how to execute deployments by using the command line.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="f7ef5-205">[上一頁](deploying-a-code-update.md)
> [下一頁](command-line-deployment.md)</span><span class="sxs-lookup"><span data-stu-id="f7ef5-205">[Previous](deploying-a-code-update.md)
[Next](command-line-deployment.md)</span></span>
