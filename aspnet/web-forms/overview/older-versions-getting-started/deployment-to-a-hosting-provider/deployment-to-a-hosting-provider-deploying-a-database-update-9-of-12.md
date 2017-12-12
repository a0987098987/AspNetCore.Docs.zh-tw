---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
title: "使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-9 / 12 |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: a8d776af-4735-4612-87f6-9f326587f2d3
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12
msc.type: authoredcontent
ms.openlocfilehash: 44faca6ac2cdb9193f5c7f6b1d7f5a26e6e22248
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-a-database-update---9-of-12"></a><span data-ttu-id="eb643-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署資料庫更新-9 / 12</span><span class="sxs-lookup"><span data-stu-id="eb643-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying a Database Update - 9 of 12</span></span>
====================
<span data-ttu-id="eb643-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="eb643-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="eb643-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="eb643-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="eb643-106">這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="eb643-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="eb643-107">如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="eb643-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="eb643-108">數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="eb643-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="eb643-109">顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="eb643-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="eb643-110">概觀</span><span class="sxs-lookup"><span data-stu-id="eb643-110">Overview</span></span>

<span data-ttu-id="eb643-111">本教學課程中，您在資料庫變更及相關的程式碼變更，在 Visual Studio 中，測試所做的變更，然後將更新部署到測試和實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="eb643-111">In this tutorial, you make a database change and related code changes, test the changes in Visual Studio, then deploy the update to both the test and production environments.</span></span>

<span data-ttu-id="eb643-112">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="eb643-112">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="adding-a-new-column-to-a-table"></a><span data-ttu-id="eb643-113">將新的資料行加入資料表</span><span class="sxs-lookup"><span data-stu-id="eb643-113">Adding a New Column to a Table</span></span>

<span data-ttu-id="eb643-114">在本節中，您加入的出生日期資料行`Person`基底類別`Student`和`Instructor`實體。</span><span class="sxs-lookup"><span data-stu-id="eb643-114">In this section, you add a birth date column to the `Person` base class for the `Student` and `Instructor` entities.</span></span> <span data-ttu-id="eb643-115">然後您可以更新，使其顯示新的資料行顯示講師資料的頁面。</span><span class="sxs-lookup"><span data-stu-id="eb643-115">Then you update the page that displays instructor data so that it displays the new column.</span></span>

<span data-ttu-id="eb643-116">在*ContosoUniversity.DAL*專案中，開啟*Person.cs*和結尾加入下列屬性`Person`類別 （應該會有兩個左右大括號後面）：</span><span class="sxs-lookup"><span data-stu-id="eb643-116">In the *ContosoUniversity.DAL* project, open *Person.cs* and add the following property at the end of the `Person` class (there should be two closing curly braces following it):</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample1.cs)]

<span data-ttu-id="eb643-117">接著，更新 Seed 方法，使其提供新的資料行的值。</span><span class="sxs-lookup"><span data-stu-id="eb643-117">Next, update the Seed method so that it provides a value for the new column.</span></span> <span data-ttu-id="eb643-118">開啟*Migrations\Configuration.cs*取代開始的程式碼區塊和`var instructors = new List<Instructor>`與下列程式碼區塊其中包括出生日期資訊：</span><span class="sxs-lookup"><span data-stu-id="eb643-118">Open *Migrations\Configuration.cs* and replace the code block that begins `var instructors = new List<Instructor>` with the following code block which includes birth date information:</span></span>

[!code-csharp[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample2.cs)]

<span data-ttu-id="eb643-119">在 ContosoUniversity 專案中，開啟*Instructors.aspx*並加入新的範本欄位，以顯示出生日期。</span><span class="sxs-lookup"><span data-stu-id="eb643-119">In the ContosoUniversity project, open *Instructors.aspx* and add a new template field to display the birth date.</span></span> <span data-ttu-id="eb643-120">雇用日期和 office 指派的項目之間，將它加入：</span><span class="sxs-lookup"><span data-stu-id="eb643-120">Add it between the ones for hire date and office assignment:</span></span>

[!code-aspx[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample3.aspx)]

<span data-ttu-id="eb643-121">（如果程式碼縮排不同步，您可以按 CTRL-K 然後 CTRL-D 設為自動重新格式化檔案）。</span><span class="sxs-lookup"><span data-stu-id="eb643-121">(If code indentation gets out of sync, you can press CTRL-K and then CTRL-D to automatically reformat the file.)</span></span>

<span data-ttu-id="eb643-122">建置方案，然後再開啟**Package Manager Console**視窗。</span><span class="sxs-lookup"><span data-stu-id="eb643-122">Build the solution, and then open the **Package Manager Console** window.</span></span> <span data-ttu-id="eb643-123">請確定 ContosoUniversity.DAL 仍然做為選取狀態**預設專案**。</span><span class="sxs-lookup"><span data-stu-id="eb643-123">Make sure that ContosoUniversity.DAL is still selected as the **Default project**.</span></span>

<span data-ttu-id="eb643-124">在**Package Manager Console**視窗中，選取**ContosoUniversity.DAL**為**預設專案**，然後輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="eb643-124">In the **Package Manager Console** window, select **ContosoUniversity.DAL** as the **Default project**, and then enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample4.ps1)]

<span data-ttu-id="eb643-125">此命令完成時，Visual Studio 會開啟定義新的類別檔案`DbMIgration`類別，然後在`Up`方法，您可以看到建立新的資料行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="eb643-125">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` class, and in the `Up` method you can see the code that creates the new column.</span></span>

![AddBirthDate_migration_code](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image1.png)

<span data-ttu-id="eb643-127">建置方案，，然後輸入下列命令中的**Package Manager Console**視窗 （請確定已選取 ContosoUniversity.DAL 專案）：</span><span class="sxs-lookup"><span data-stu-id="eb643-127">Build the solution, and then enter the following command in the **Package Manager Console** window (make sure the ContosoUniversity.DAL project is still selected):</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/samples/sample5.ps1)]

<span data-ttu-id="eb643-128">當命令完成時，執行應用程式，並選取講師頁面。</span><span class="sxs-lookup"><span data-stu-id="eb643-128">When the command finishes, run the application and select the Instructors page.</span></span> <span data-ttu-id="eb643-129">當網頁載入時，您就會看到有新出生日期欄位。</span><span class="sxs-lookup"><span data-stu-id="eb643-129">When the page loads, you see that it has the new birth date field.</span></span>

<span data-ttu-id="eb643-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="eb643-130">[![Instructors_page_with_birth_date](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image3.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image2.png)</span></span>

## <a name="deploying-the-database-update-to-the-test-environment"></a><span data-ttu-id="eb643-131">將資料庫更新部署到測試環境</span><span class="sxs-lookup"><span data-stu-id="eb643-131">Deploying the Database Update to the Test Environment</span></span>

<span data-ttu-id="eb643-132">在**方案總管 中**選取 ContosoUniversity 專案。</span><span class="sxs-lookup"><span data-stu-id="eb643-132">In **Solution Explorer** select the ContosoUniversity project.</span></span>

<span data-ttu-id="eb643-133">在**Web 一個按一下 Publish**工具列上，選取**測試**發行設定檔，然後按一下 **發行 Web**。</span><span class="sxs-lookup"><span data-stu-id="eb643-133">In the **Web One Click Publish** toolbar, select the **Test** publish profile, and then click **Publish Web**.</span></span> <span data-ttu-id="eb643-134">(如果已停用工具列，ContosoUniversity 中選取專案**方案總管 中**。)</span><span class="sxs-lookup"><span data-stu-id="eb643-134">(If the toolbar is disabled, select the ContosoUniversity project in **Solution Explorer**.)</span></span>

<span data-ttu-id="eb643-135">Visual Studio 會部署更新的應用程式，並瀏覽器開啟至首頁。</span><span class="sxs-lookup"><span data-stu-id="eb643-135">Visual Studio deploys the updated application, and the browser opens to the home page.</span></span> <span data-ttu-id="eb643-136">執行講師頁面以確認更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="eb643-136">Run the Instructors page to verify that the update was successfully deployed.</span></span> <span data-ttu-id="eb643-137">當應用程式嘗試存取此頁面的資料庫時，Code First 更新資料庫結構描述並執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="eb643-137">When the application tries to access the database for this page, Code First updates the database schema and runs the `Seed` method.</span></span> <span data-ttu-id="eb643-138">當頁面顯示時，您會看到預期**出生日期**中它的日期資料行。</span><span class="sxs-lookup"><span data-stu-id="eb643-138">When the page displays, you see the expected **Birth Date** column with dates in it.</span></span>

<span data-ttu-id="eb643-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="eb643-139">[![Instructors_page_with_birth_date_Test](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image5.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image4.png)</span></span>

## <a name="deploying-the-database-update-to-the-production-environment"></a><span data-ttu-id="eb643-140">將資料庫更新部署至生產環境</span><span class="sxs-lookup"><span data-stu-id="eb643-140">Deploying the Database Update to the Production Environment</span></span>

<span data-ttu-id="eb643-141">您現在可以部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="eb643-141">You can now deploy to production.</span></span> <span data-ttu-id="eb643-142">唯一的差別在於您將使用*應用程式\_offline.htm*以避免使用者存取網站，而您要部署的變更，因而更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="eb643-142">The only difference is that you'll use *app\_offline.htm* to prevent users from accessing the site and thus updating the database while you're deploying changes.</span></span> <span data-ttu-id="eb643-143">生產環境部署中，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="eb643-143">For production deployment perform the following steps:</span></span>

- <span data-ttu-id="eb643-144">上傳*應用程式\_offline.htm*生產網站的檔案。</span><span class="sxs-lookup"><span data-stu-id="eb643-144">Upload the *app\_offline.htm* file to the production site.</span></span>
- <span data-ttu-id="eb643-145">在 Visual Studio 中，選擇 在實際執行設定檔**Web 一個按一下 Publish**工具列，並按一下**發行 Web**。</span><span class="sxs-lookup"><span data-stu-id="eb643-145">In Visual Studio, choose the Production profile in the **Web One Click Publish** toolbar and click **Publish Web**.</span></span>
- <span data-ttu-id="eb643-146">刪除*應用程式\_offline.htm*從生產網站的檔案。</span><span class="sxs-lookup"><span data-stu-id="eb643-146">Delete the *app\_offline.htm* file from the production site.</span></span>

> [!NOTE]
> <span data-ttu-id="eb643-147">在實際執行環境中使用您的應用程式時您應該實作備份計劃。</span><span class="sxs-lookup"><span data-stu-id="eb643-147">While your application is in use in the production environment you should be implementing a backup plan.</span></span> <span data-ttu-id="eb643-148">也就是說，您應該要定期複製*學校 Prod.sdf*和*aspnet Prod.sdf*檔案從實際站台至安全的儲存體位置，以及您應該保留這類的數個層代備份。</span><span class="sxs-lookup"><span data-stu-id="eb643-148">That is, you should be periodically copying the *School-Prod.sdf* and *aspnet-Prod.sdf* files from the production site to a secure storage location, and you should be keeping several generations of such backups.</span></span> <span data-ttu-id="eb643-149">當您更新資料庫時，請立即變更之前的備份複本。</span><span class="sxs-lookup"><span data-stu-id="eb643-149">When you update the database, you should make a backup copy from immediately before the change.</span></span> <span data-ttu-id="eb643-150">然後，如果您犯了錯誤，並不探索它，直到它部署到生產環境之後，您將仍然能夠將資料庫復原到它損毀前的狀態。</span><span class="sxs-lookup"><span data-stu-id="eb643-150">Then, if you make a mistake and don't discover it until after you have deployed it to production, you will still be able to recover the database to the state it was in before it became corrupted.</span></span>


<span data-ttu-id="eb643-151">當 Visual Studio 會在瀏覽器中開啟首頁 URL*應用程式\_offline.htm*頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="eb643-151">When Visual Studio opens the home page URL in the browser, the *app\_offline.htm* page is displayed.</span></span> <span data-ttu-id="eb643-152">當您刪除之後*應用程式\_offline.htm*檔案中，您可以瀏覽至您的首頁上，確認該更新已成功部署。</span><span class="sxs-lookup"><span data-stu-id="eb643-152">After you delete the *app\_offline.htm* file, you can browse to your home page again to verify that the update was successfully deployed.</span></span>

<span data-ttu-id="eb643-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="eb643-153">[![Instructors_page_with_birth_date_Prod](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image7.png)](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12/_static/image6.png)</span></span>

<span data-ttu-id="eb643-154">您現在已部署應用程式更新包含測試與實際的資料庫變更。</span><span class="sxs-lookup"><span data-stu-id="eb643-154">You've now deployed an application update that included a database change to both test and production.</span></span> <span data-ttu-id="eb643-155">下一個教學課程會示範如何將資料庫從 SQL Server Compact 移轉至 SQL Server Express 和 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="eb643-155">The next tutorial shows you how to migrate your database from SQL Server Compact to SQL Server Express and SQL Server.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="eb643-156">[上一頁](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[下一頁](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="eb643-156">[Previous](deployment-to-a-hosting-provider-deploying-a-code-only-update-8-of-12.md)
[Next](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12.md)</span></span>
