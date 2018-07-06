---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 小時，共 12 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: 5e2bb092-cb22-4511-ad0a-22ae12dd99b3
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12
msc.type: authoredcontent
ms.openlocfilehash: c744bc54c8ce12820d1e1388ac7ab2db814ff031
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37816623"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-sql-server-database-update---11-of-12"></a><span data-ttu-id="bc65b-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署 SQL Server 資料庫更新-11 小時，共 12</span><span class="sxs-lookup"><span data-stu-id="bc65b-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a SQL Server Database Update - 11 of 12</span></span>
====================
<span data-ttu-id="bc65b-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="bc65b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="bc65b-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="bc65b-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="bc65b-106">這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="bc65b-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="bc65b-107">如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="bc65b-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="bc65b-108">在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="bc65b-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="bc65b-109">顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Windows Azure 網站的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="bc65b-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Windows Azure Web Sites, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="bc65b-110">總覽</span><span class="sxs-lookup"><span data-stu-id="bc65b-110">Overview</span></span>

<span data-ttu-id="bc65b-111">本教學課程會示範如何將資料庫更新部署至完整的 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="bc65b-111">This tutorial shows you how to deploy a database update to a full SQL Server database.</span></span> <span data-ttu-id="bc65b-112">因為 Code First 移轉更新資料庫的所有工作，程序是幾乎完全相同，您的 SQL Server Compact 中做過[部署資料庫更新](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="bc65b-112">Because Code First Migrations does all the work of updating the database, the process is almost identical to what you did for SQL Server Compact in the [Deploying a Database Update](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md) tutorial.</span></span>

<span data-ttu-id="bc65b-113">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="bc65b-113">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="bc65b-114">將新的資料行加入至資料表</span><span class="sxs-lookup"><span data-stu-id="bc65b-114">Adding a New Column to a Table</span></span>

<span data-ttu-id="bc65b-115">在本教學課程的這一節中，您要進行變更的資料庫和對應的程式碼變更，然後進行測試 Visual Studio 中以準備將它們部署到測試和生產環境。</span><span class="sxs-lookup"><span data-stu-id="bc65b-115">In this section of the tutorial you'll make a database change and corresponding code changes, then test them in Visual Studio in preparation for deploying them to the test and production environments.</span></span> <span data-ttu-id="bc65b-116">變更部署，涉及新增`OfficeHours`資料行`Instructor`實體，並顯示在新的資訊**講師**網頁。</span><span class="sxs-lookup"><span data-stu-id="bc65b-116">The change involves adding an `OfficeHours` column to the `Instructor` entity and displaying the new information in the **Instructors** web page.</span></span>

<span data-ttu-id="bc65b-117">在 ContosoUniversity.DAL 專案中，開啟*Instructor.cs*並新增下列屬性之間`HireDate`和`Courses`屬性：</span><span class="sxs-lookup"><span data-stu-id="bc65b-117">In the ContosoUniversity.DAL project, open *Instructor.cs* and add the following property between the `HireDate` and `Courses` properties:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample1.cs)]

<span data-ttu-id="bc65b-118">更新的初始設定式類別，使它植入測試資料的新資料行。</span><span class="sxs-lookup"><span data-stu-id="bc65b-118">Update the initializer class so that it seeds the new column with test data.</span></span> <span data-ttu-id="bc65b-119">開啟*Migrations\Configuration.cs*並取代程式碼區塊開頭`var instructors = new List<Instructor>`以下列程式碼區塊包含新的資料行：</span><span class="sxs-lookup"><span data-stu-id="bc65b-119">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes the new column:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample2.cs)]

<span data-ttu-id="bc65b-120">在 [ContosoUniversity] 專案中，開啟*Instructors.aspx* ，並將新的 [範本] 欄位加入結尾之前的上班`</Columns>`在第一個標記`GridView`控制項：</span><span class="sxs-lookup"><span data-stu-id="bc65b-120">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field for office hours just before the closing `</Columns>` tag in the first `GridView` control:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample3.aspx)]

<span data-ttu-id="bc65b-121">建置方案。</span><span class="sxs-lookup"><span data-stu-id="bc65b-121">Build the solution.</span></span>

<span data-ttu-id="bc65b-122">開啟**Package Manager Console**視窗中，並為選取 ContosoUniversity.DAL**預設專案**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-122">Open the **Package Manager Console** window, and select ContosoUniversity.DAL as the **Default project**.</span></span>

<span data-ttu-id="bc65b-123">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="bc65b-123">Enter the following commands:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample4.ps1)]

<span data-ttu-id="bc65b-124">執行應用程式，然後選取**講師**頁面。</span><span class="sxs-lookup"><span data-stu-id="bc65b-124">Run the application and select the **Instructors** page.</span></span> <span data-ttu-id="bc65b-125">頁面需要較長的時間比平常要載入，因為 Entity Framework 重新建立資料庫並植入測試資料。</span><span class="sxs-lookup"><span data-stu-id="bc65b-125">The page takes a little longer than usual to load, because the Entity Framework re-creates the database and seeds it with test data.</span></span>

<span data-ttu-id="bc65b-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="bc65b-126">[![Instructors_page_with_office_hours](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image1.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="bc65b-127">資料庫將更新部署至測試環境</span><span class="sxs-lookup"><span data-stu-id="bc65b-127">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="bc65b-128">當您使用 Code First 移轉時，則會將資料庫變更部署到 SQL Server 的方法是與 SQL Server Compact 時相同。</span><span class="sxs-lookup"><span data-stu-id="bc65b-128">When you use Code First Migrations, the method for deploying a database change to SQL Server is the same as for SQL Server Compact.</span></span> <span data-ttu-id="bc65b-129">不過，您必須變更測試發行設定檔，因為它仍然設定為從 SQL Server Compact 移轉至 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="bc65b-129">However, you have to change the Test publish profile because it is still set up to migrate from SQL Server Compact to SQL Server.</span></span>

<span data-ttu-id="bc65b-130">第一步是移除您在上一個教學課程中建立的連接字串轉換。</span><span class="sxs-lookup"><span data-stu-id="bc65b-130">The first step is to remove the connection string transformations that you created in the previous tutorial.</span></span> <span data-ttu-id="bc65b-131">不再需要用到這些因為您將發行設定檔中指定連接字串轉換設定之前一樣**封裝/發行 SQL**移轉至 SQL Server 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="bc65b-131">These are no longer needed because you'll specify connection string transformations in the publish profile, as you did before you configured the **Package/Publish SQL** tab for migration to SQL Server.</span></span>

<span data-ttu-id="bc65b-132">開啟*Web.Test.config*檔案，並移除`connectionStrings`項目。</span><span class="sxs-lookup"><span data-stu-id="bc65b-132">Open the *Web.Test.config* file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="bc65b-133">中的唯一剩餘的轉換*Web.Test.config*檔案的用途`Environment`中的值`appSettings`項目。</span><span class="sxs-lookup"><span data-stu-id="bc65b-133">The only remaining transformation in the *Web.Test.config* file is for the `Environment` value in the `appSettings` element.</span></span>

<span data-ttu-id="bc65b-134">現在您可以更新的發行設定檔和發行到測試環境。</span><span class="sxs-lookup"><span data-stu-id="bc65b-134">Now you can update the publish profile and publish to the test environment.</span></span>

<span data-ttu-id="bc65b-135">開啟**發佈 Web**精靈，然後再切換到**設定檔** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="bc65b-135">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="bc65b-136">選取 **測試**發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bc65b-136">Select the **Test** publish profile.</span></span>

<span data-ttu-id="bc65b-137">選取 [**設定**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="bc65b-137">Select the **Settings** tab.</span></span>

<span data-ttu-id="bc65b-138">按一下 **啟用新的資料庫發行改進**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-138">Click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="bc65b-139">在的連接字串中**SchoolContext**，輸入您在中使用的相同值*Web.Test.config*先前的教學課程中的轉換檔案：</span><span class="sxs-lookup"><span data-stu-id="bc65b-139">In the connection string box for **SchoolContext**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample5.cmd)]

<span data-ttu-id="bc65b-140">選取  **Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-140">Select **Execute Code First Migrations (runs on application start)**.</span></span> <span data-ttu-id="bc65b-141">(在您的 Visual Studio 版本，可能會標示為核取方塊**套用 Code First 移轉**。)</span><span class="sxs-lookup"><span data-stu-id="bc65b-141">(In your version of Visual Studio, the check box might be labeled **Apply Code First Migrations**.)</span></span>

<span data-ttu-id="bc65b-142">在的連接字串中**DefaultConnection**，輸入您在中使用的相同值*Web.Test.config*先前的教學課程中的轉換檔案：</span><span class="sxs-lookup"><span data-stu-id="bc65b-142">In the connection string box for **DefaultConnection**, enter the same value that you used in the *Web.Test.config* transformation file in the previous tutorial:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/samples/sample6.cmd)]

<span data-ttu-id="bc65b-143">離開**更新資料庫**清除。</span><span class="sxs-lookup"><span data-stu-id="bc65b-143">Leave **Update database** cleared.</span></span>

<span data-ttu-id="bc65b-144">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="bc65b-144">Click **Publish**.</span></span>

<span data-ttu-id="bc65b-145">Visual Studio 會部署到測試環境的程式碼變更和瀏覽器開啟至 Contoso 大學首頁上。</span><span class="sxs-lookup"><span data-stu-id="bc65b-145">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="bc65b-146">選取的講師頁面。</span><span class="sxs-lookup"><span data-stu-id="bc65b-146">Select the Instructors page.</span></span>

<span data-ttu-id="bc65b-147">當應用程式執行此頁面時，它會嘗試存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="bc65b-147">When the application runs this page, it tries to access the database.</span></span> <span data-ttu-id="bc65b-148">如果資料庫是最新狀態，並找到的最新的移轉具有尚未套用的資料，則會檢查 code First 移轉。</span><span class="sxs-lookup"><span data-stu-id="bc65b-148">Code First Migrations checks if the database is current, and finds that the latest migration has not been applied yet.</span></span> <span data-ttu-id="bc65b-149">Code First 移轉適用於最新的移轉，便會執行`Seed`方法，然後按一下  頁面會正常執行。</span><span class="sxs-lookup"><span data-stu-id="bc65b-149">Code First Migrations applies the latest migration, runs the `Seed` method, and then the page runs normally.</span></span> <span data-ttu-id="bc65b-150">您會看到新的辦公時間資料行，以植入的資料。</span><span class="sxs-lookup"><span data-stu-id="bc65b-150">You see the new Office Hours column with the seeded data.</span></span>

<span data-ttu-id="bc65b-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="bc65b-151">[![Instructors_page_with_OfficeHours_Test](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image3.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="bc65b-152">資料庫將更新部署至生產環境</span><span class="sxs-lookup"><span data-stu-id="bc65b-152">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="bc65b-153">您必須也變更生產環境的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bc65b-153">You have to change the publish profile for the production environment also.</span></span> <span data-ttu-id="bc65b-154">在此情況下，您會移除現有的設定檔，並建立一個新的匯入更新的.publishsettings 檔案。</span><span class="sxs-lookup"><span data-stu-id="bc65b-154">In this case you'll remove the existing profile and create a new one by importing an updated .publishsettings file.</span></span> <span data-ttu-id="bc65b-155">更新的檔案會包含在 Cytanium 的 SQL Server 資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="bc65b-155">The updated file will include the connection string for the SQL Server database at Cytanium.</span></span>

<span data-ttu-id="bc65b-156">如您所見，當您部署到測試環境，您不再需要連接字串中的轉換*Web.Production.config*轉換檔案。</span><span class="sxs-lookup"><span data-stu-id="bc65b-156">As you saw when you deployed to the test environment, you no longer need connection string transforms in the *Web.Production.config* transformation file.</span></span> <span data-ttu-id="bc65b-157">開啟了檔案，並移除`connectionStrings`項目。</span><span class="sxs-lookup"><span data-stu-id="bc65b-157">Open that file and remove the `connectionStrings` element.</span></span> <span data-ttu-id="bc65b-158">「 剩餘 」 轉換對於`Environment`中的值`appSettings`項目和`location`Elmah 錯誤報表，會限制存取的項目。</span><span class="sxs-lookup"><span data-stu-id="bc65b-158">The remaining transformations are for the `Environment` value in the `appSettings` element and the `location` element that restricts access to Elmah error reports.</span></span>

<span data-ttu-id="bc65b-159">建立新的發行設定檔，用於生產環境之前，下載更新的.publishsettings 檔案的相同方式稍早在[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="bc65b-159">Before you create a new publish profile for production, download an updated .publishsettings file the same way you did earlier in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span> <span data-ttu-id="bc65b-160">(在 Cytanium 控制面板中，按一下**Web Sites**，然後按一下**contosouniversity.com**網站。</span><span class="sxs-lookup"><span data-stu-id="bc65b-160">(In the Cytanium control panel, click **Web Sites**, and then click the **contosouniversity.com** website.</span></span> <span data-ttu-id="bc65b-161">選取**Web Publishing**索引標籤，然後再按一下**網站下載發行設定檔**。)您會執行此動作的原因是要挑選.publishsettings 檔案中的資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="bc65b-161">Select the **Web Publishing** tab, and then click **Download Publishing Profile for this web site**.) The reason you are doing this is to pick up the database connection string in the .publishsettings file.</span></span> <span data-ttu-id="bc65b-162">連接字串無法下載檔案，因為您仍使用 SQL Server Compact，並沒有 SQL Server 資料庫在 Cytanium 尚未建立的第一次使用。</span><span class="sxs-lookup"><span data-stu-id="bc65b-162">The connection string wasn't available the first time you downloaded the file, because you were still using SQL Server Compact and hadn't created the SQL Server database at Cytanium yet.</span></span>

<span data-ttu-id="bc65b-163">現在您可以更新的發行設定檔和發行到生產環境。</span><span class="sxs-lookup"><span data-stu-id="bc65b-163">Now you can update the publish profile and publish to the production environment.</span></span>

<span data-ttu-id="bc65b-164">開啟**發佈 Web**精靈，然後再切換到**設定檔** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="bc65b-164">Open the **Publish Web** wizard, and then switch to the **Profile** tab.</span></span>

<span data-ttu-id="bc65b-165">按一下 **管理設定檔**，然後再刪除實際執行設定檔。</span><span class="sxs-lookup"><span data-stu-id="bc65b-165">Click **Manage Profiles**, and then delete the Production profile.</span></span>

<span data-ttu-id="bc65b-166">關閉**發佈 Web** 」 精靈來儲存這項變更。</span><span class="sxs-lookup"><span data-stu-id="bc65b-166">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="bc65b-167">開啟**發佈 Web**再次強調，精靈，然後按一下**匯入**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-167">Open the **Publish Web** wizard again, and then click **Import**.</span></span>

<span data-ttu-id="bc65b-168">在 **連接**索引標籤中變更**目的地 URL**為適當的值，如果您使用暫存的 URL。</span><span class="sxs-lookup"><span data-stu-id="bc65b-168">On the **Connection** tab, change **Destination URL** to the appropriate value if you are using a temporary URL.</span></span>

<span data-ttu-id="bc65b-169">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="bc65b-169">Click **Next**.</span></span>

<span data-ttu-id="bc65b-170">在 **設定**索引標籤上，按一下**啟用新的資料庫發行改進**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-170">On the **Settings** tab, click **enable the new database publishing improvements**.</span></span>

<span data-ttu-id="bc65b-171">中的連接字串下拉式清單**SchoolContext**，選取 Cytanium 連接字串。</span><span class="sxs-lookup"><span data-stu-id="bc65b-171">In the connection string drop-down list for **SchoolContext**, select the Cytanium connection string.</span></span>

![Selecting_Cytanium_connection_string](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image5.png)

<span data-ttu-id="bc65b-173">選取 **執行 Code First 移轉 （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="bc65b-173">Select **Execute Code First migrations (runs on application start)**.</span></span>

<span data-ttu-id="bc65b-174">中的連接字串下拉式清單**DefaultConnection**，選取 Cytanium 連接字串。</span><span class="sxs-lookup"><span data-stu-id="bc65b-174">In the connection string drop-down list for **DefaultConnection**, select the Cytanium connection string.</span></span>

<span data-ttu-id="bc65b-175">選取 **設定檔**索引標籤上，按一下**管理設定檔**，並將設定檔從"contosouniversity.com-Web Deploy"重新命名為 「 生產 」。</span><span class="sxs-lookup"><span data-stu-id="bc65b-175">Select the **Profile** tab, click **Manage Profiles**, and rename the profile from "contosouniversity.com - Web Deploy" to "Production".</span></span>

<span data-ttu-id="bc65b-176">關閉的發行設定檔，以儲存變更，然後再重新開啟。</span><span class="sxs-lookup"><span data-stu-id="bc65b-176">Close the publish profile to save the change, then open it again.</span></span>

<span data-ttu-id="bc65b-177">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="bc65b-177">Click **Publish**.</span></span> <span data-ttu-id="bc65b-178">(用於實際生產網站，您會複製*應用程式\_offline.htm*到生產環境與 put 在您的專案資料夾，在發行之前，然後移除部署完成時。)</span><span class="sxs-lookup"><span data-stu-id="bc65b-178">(For a real production website, you would copy *app\_offline.htm* to production and put it in your project folder before publishing, then remove it when deployment is complete.)</span></span>

<span data-ttu-id="bc65b-179">Visual Studio 會部署到測試環境的程式碼變更和瀏覽器開啟至 Contoso 大學首頁上。</span><span class="sxs-lookup"><span data-stu-id="bc65b-179">Visual Studio deploys the code changes to the test environment and opens the browser to the Contoso University home page.</span></span>

<span data-ttu-id="bc65b-180">選取的講師頁面。</span><span class="sxs-lookup"><span data-stu-id="bc65b-180">Select the Instructors page.</span></span>

<span data-ttu-id="bc65b-181">Code First 移轉更新資料庫中的測試環境相同的方式。</span><span class="sxs-lookup"><span data-stu-id="bc65b-181">Code First Migrations updates the database the same way it did in the Test environment.</span></span> <span data-ttu-id="bc65b-182">您會看到新的辦公時間資料行，以植入的資料。</span><span class="sxs-lookup"><span data-stu-id="bc65b-182">You see the new Office Hours column with the seeded data.</span></span>

![Instructors_page_with_OfficeHours_Prod](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12/_static/image6.png)

<span data-ttu-id="bc65b-184">現在您已成功部署應用程式更新包含資料庫變更，使用 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="bc65b-184">You have now successfully deployed an application update that included a database change, using a SQL Server database.</span></span>

## <a name="more-information"></a><span data-ttu-id="bc65b-185">更多資訊</span><span class="sxs-lookup"><span data-stu-id="bc65b-185">More Information</span></span>

<span data-ttu-id="bc65b-186">如此即完成本系列的教學課程將部署到協力廠商裝載提供者的 ASP.NET web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="bc65b-186">This completes this series of tutorials on deploying an ASP.NET web application to a third-party hosting provider.</span></span> <span data-ttu-id="bc65b-187">如需任何這些教學課程所涵蓋的主題的詳細資訊，請參閱[ASP.NET 部署內容對應](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx)MSDN 網站上。</span><span class="sxs-lookup"><span data-stu-id="bc65b-187">For more information about any of the topics covered in these tutorials, see the [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521(v=vs.110).aspx) on the MSDN web site.</span></span>

## <a name="acknowledgements"></a><span data-ttu-id="bc65b-188">謝誌</span><span class="sxs-lookup"><span data-stu-id="bc65b-188">Acknowledgements</span></span>

<span data-ttu-id="bc65b-189">我想要感謝下列人士提出重大貢獻的內容，本教學課程系列：</span><span class="sxs-lookup"><span data-stu-id="bc65b-189">I would like to thank the following people who made significant contributions to the content of this tutorial series:</span></span>

- [<span data-ttu-id="bc65b-190">Alberto Poblacion、 MVP &amp; MCT，西班牙</span><span class="sxs-lookup"><span data-stu-id="bc65b-190">Alberto Poblacion, MVP &amp; MCT, Spain</span></span>](https://mvp.support.microsoft.com/profile/Alberto)
- <span data-ttu-id="bc65b-191">Jarod Ferguson，資料平台開發 MVP、 美國。</span><span class="sxs-lookup"><span data-stu-id="bc65b-191">Jarod Ferguson, Data Platform Development MVP, United States</span></span>
- <span data-ttu-id="bc65b-192">Harsh Mittal，Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc65b-192">Harsh Mittal, Microsoft</span></span>
- [<span data-ttu-id="bc65b-193">Kristina Olson Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc65b-193">Kristina Olson, Microsoft</span></span>](https://blogs.iis.net/krolson/default.aspx)
- [<span data-ttu-id="bc65b-194">Mike 主教 Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc65b-194">Mike Pope, Microsoft</span></span>](http://www.mikepope.com/blog/DisplayBlog.aspx)
- <span data-ttu-id="bc65b-195">Mohit Srivastava Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc65b-195">Mohit Srivastava, Microsoft</span></span>
- [<span data-ttu-id="bc65b-196">Raffaele Rialdi 義大利</span><span class="sxs-lookup"><span data-stu-id="bc65b-196">Raffaele Rialdi, Italy</span></span>](http://www.iamraf.net/)
- [<span data-ttu-id="bc65b-197">Rick Anderson Microsoft</span><span class="sxs-lookup"><span data-stu-id="bc65b-197">Rick Anderson, Microsoft</span></span>](https://blogs.msdn.com/b/rickandy/)
- <span data-ttu-id="bc65b-198">[Sayed Hashimi，Microsoft](http://sedodream.com/default.aspx)(twitter: [ @sayedihashimi ](http://twitter.com/sayedihashimi))</span><span class="sxs-lookup"><span data-stu-id="bc65b-198">[Sayed Hashimi, Microsoft](http://sedodream.com/default.aspx)(twitter: [@sayedihashimi](http://twitter.com/sayedihashimi))</span></span>
- <span data-ttu-id="bc65b-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [ @shanselman ](http://twitter.com/shanselman))</span><span class="sxs-lookup"><span data-stu-id="bc65b-199">[Scott Hanselman](http://www.hanselman.com/blog/) (twitter: [@shanselman](http://twitter.com/shanselman))</span></span>
- <span data-ttu-id="bc65b-200">[Scott Hunter、 Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [ @coolcsh ](http://twitter.com/coolcsh))</span><span class="sxs-lookup"><span data-stu-id="bc65b-200">[Scott Hunter, Microsoft](https://blogs.msdn.com/b/scothu/) (twitter: [@coolcsh](http://twitter.com/coolcsh))</span></span>
- [<span data-ttu-id="bc65b-201">Srđan Božović，塞爾維亞</span><span class="sxs-lookup"><span data-stu-id="bc65b-201">Srđan Božović, Serbia</span></span>](http://msforge.net/blogs/zmajcek/)
- <span data-ttu-id="bc65b-202">[Vishal Joshi、 Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [ @vishalrjoshi ](http://twitter.com/vishalrjoshi))</span><span class="sxs-lookup"><span data-stu-id="bc65b-202">[Vishal Joshi, Microsoft](http://vishaljoshi.blogspot.com/) (twitter: [@vishalrjoshi](http://twitter.com/vishalrjoshi))</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="bc65b-203">[上一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="bc65b-203">[Previous](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)
[Next](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)</span></span>
