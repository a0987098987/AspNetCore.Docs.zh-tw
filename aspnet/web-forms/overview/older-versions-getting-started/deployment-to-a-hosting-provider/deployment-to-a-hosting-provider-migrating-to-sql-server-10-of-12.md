---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
title: 使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 小時，共 12 |Microsoft Docs
author: tdykstra
description: 這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫，使用 Visual Stu...
ms.author: aspnetcontent
ms.date: 11/17/2011
ms.assetid: a89d6f32-b71b-4036-8ff7-5f8ac2a6eca8
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12
msc.type: authoredcontent
ms.openlocfilehash: bc0bca18d2f6e4cdbb527ab75952e9a50eb49b20
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37827353"
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-migrating-to-sql-server---10-of-12"></a><span data-ttu-id="da36b-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 移轉至 SQL Server-10 小時，共 12</span><span class="sxs-lookup"><span data-stu-id="da36b-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Migrating to SQL Server - 10 of 12</span></span>
====================
<span data-ttu-id="da36b-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="da36b-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="da36b-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="da36b-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="da36b-106">這一系列的教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式專案使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 包含 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="da36b-107">如果您安裝 Web Publish Update，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="da36b-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="da36b-108">在數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="da36b-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="da36b-109">顯示 Visual Studio 2012 RC 版本之後引入的部署功能，示範如何部署 SQL Server Compact，以外的 SQL Server 版本，並示範如何部署至 Azure App Service Web Apps 的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="da36b-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="da36b-110">總覽</span><span class="sxs-lookup"><span data-stu-id="da36b-110">Overview</span></span>

<span data-ttu-id="da36b-111">本教學課程會示範如何從 SQL Server Compact 移轉至 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="da36b-111">This tutorial shows you how to migrate from SQL Server Compact to SQL Server.</span></span> <span data-ttu-id="da36b-112">您可能想要這麼做的其中一個原因是利用 SQL Server Compact 不支援，例如預存程序、 觸發程序、 檢視或複寫的 SQL Server 功能。</span><span class="sxs-lookup"><span data-stu-id="da36b-112">One reason you might want to do that is to take advantage of SQL Server features that SQL Server Compact does not support, such as stored procedures, triggers, views, or replication.</span></span> <span data-ttu-id="da36b-113">如需有關 SQL Server Compact 和 SQL Server 之間的差異的詳細資訊，請參閱 <<c0> [ 部署 SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="da36b-113">For more information about the differences between SQL Server Compact and SQL Server, see the [Deploying SQL Server Compact](deployment-to-a-hosting-provider-deploying-sql-server-compact-databases-2-of-12.md) tutorial.</span></span>

### <a name="sql-server-express-versus-full-sql-server-for-development"></a><span data-ttu-id="da36b-114">SQL Server Express 與完整的 SQL Server 進行開發</span><span class="sxs-lookup"><span data-stu-id="da36b-114">SQL Server Express versus full SQL Server for Development</span></span>

<span data-ttu-id="da36b-115">一旦您已經決定要升級到 SQL Server，您可能想要用於開發和測試環境中的 SQL Server 或 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="da36b-115">Once you've decided to upgrade to SQL Server, you might want to use SQL Server or SQL Server Express in your development and test environments.</span></span> <span data-ttu-id="da36b-116">除了 database engine 功能和工具支援中的差異，還有 SQL Server Compact 和 SQL Server 的其他版本之間的差異提供者實作。</span><span class="sxs-lookup"><span data-stu-id="da36b-116">In addition to the differences in tool support and in database engine features, there are differences in provider implementations between SQL Server Compact and other versions of SQL Server.</span></span> <span data-ttu-id="da36b-117">這些差異可能會導致相同的程式碼來產生不同的結果。</span><span class="sxs-lookup"><span data-stu-id="da36b-117">These differences can cause the same code to generate different results.</span></span> <span data-ttu-id="da36b-118">因此，如果您決定要保留 SQL Server Compact 為您的開發資料庫，您應該徹底測試您的網站在 SQL Server 或 SQL Server Express 在測試環境中每個部署至生產環境之前。</span><span class="sxs-lookup"><span data-stu-id="da36b-118">Therefore, if you decide to keep SQL Server Compact as your development database, you should thoroughly test your site in SQL Server or SQL Server Express in a test environment before each deployment to production.</span></span>

<span data-ttu-id="da36b-119">不同於 SQL Server Compact，SQL Server Express 是基本上相同的資料庫引擎，使用相同的.NET 提供者與完整的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="da36b-119">Unlike SQL Server Compact, SQL Server Express is essentially the same database engine and uses the same .NET provider as full SQL Server.</span></span> <span data-ttu-id="da36b-120">當您使用 SQL Server Express 進行測試時，您可以確信取得相同的結果，因為您將使用 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="da36b-120">When you test with SQL Server Express, you can be confident of getting the same results as you will with SQL Server.</span></span> <span data-ttu-id="da36b-121">您可以使用相同的資料庫工具大部分都與 SQL Server Express，您可以使用 SQL Server (值得注意的例外狀況，正在[SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx))，而且它支援 SQL Server 的其他功能，例如預存程序、 檢視、 觸發程序，和複寫。</span><span class="sxs-lookup"><span data-stu-id="da36b-121">You can use most of the same database tools with SQL Server Express that you can use with SQL Server (a notable exception being [SQL Server Profiler](https://msdn.microsoft.com/library/ms181091.aspx)), and it supports other features of SQL Server like stored procedures, views, triggers, and replication.</span></span> <span data-ttu-id="da36b-122">（您通常必須在生產網站中，不過使用完整的 SQL Server。</span><span class="sxs-lookup"><span data-stu-id="da36b-122">(You typically have to use full SQL Server in a production website, however.</span></span> <span data-ttu-id="da36b-123">SQL Server Express 可以執行在共用主控環境中，但原先的設計並非如此，而且許多主機服務提供者不支援。）</span><span class="sxs-lookup"><span data-stu-id="da36b-123">SQL Server Express can run in a shared hosting environment, but it was not designed for that, and many hosting providers do not support it.)</span></span>

<span data-ttu-id="da36b-124">如果您使用 Visual Studio 2012，您通常選擇 SQL Server Express LocalDB 的開發環境，因為這是使用 Visual Studio 的預設安裝的項目。</span><span class="sxs-lookup"><span data-stu-id="da36b-124">If you are using Visual Studio 2012, you typically choose SQL Server Express LocalDB for your development environment because that is what is installed by default with Visual Studio.</span></span> <span data-ttu-id="da36b-125">不過，LocalDB 不適用於 IIS，因此您必須針對您的測試環境使用 SQL Server 或 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="da36b-125">However, LocalDB does not work in IIS, so for your test environment you have to use either SQL Server or SQL Server Express.</span></span>

### <a name="combining-databases-versus-keeping-them-separate"></a><span data-ttu-id="da36b-126">結合相對於它們個別的資料庫</span><span class="sxs-lookup"><span data-stu-id="da36b-126">Combining Databases versus Keeping Them Separate</span></span>

<span data-ttu-id="da36b-127">Contoso 大學應用程式有兩個 SQL Server Compact 資料庫： 成員資格資料庫 (*aspnet.sdf*) 和應用程式資料庫 (*School.sdf*)。</span><span class="sxs-lookup"><span data-stu-id="da36b-127">The Contoso University application has two SQL Server Compact databases: the membership database (*aspnet.sdf*) and the application database (*School.sdf*).</span></span> <span data-ttu-id="da36b-128">當您移轉時，您可以在兩個不同的資料庫或單一資料庫，移轉這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-128">When you migrate, you can migrate these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="da36b-129">您可以將它們結合為簡化您的應用程式資料庫和成員資格資料庫之間的資料庫聯結。</span><span class="sxs-lookup"><span data-stu-id="da36b-129">You might want to combine them in order to facilitate database joins between your application database and your membership database.</span></span> <span data-ttu-id="da36b-130">主控方案可能也會提供將它們結合的原因。</span><span class="sxs-lookup"><span data-stu-id="da36b-130">Your hosting plan might also provide a reason to combine them.</span></span> <span data-ttu-id="da36b-131">例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-131">For example, the hosting provider might charge more for multiple databases or might not even allow more than one database.</span></span> <span data-ttu-id="da36b-132">這種情況使用 Cytanium Lite 裝載用於本教學課程中，可讓單一 SQL Server 資料庫的帳戶。</span><span class="sxs-lookup"><span data-stu-id="da36b-132">That's the case with the Cytanium Lite hosting account that's used for this tutorial, which allows only a single SQL Server database.</span></span>

<span data-ttu-id="da36b-133">在本教學課程中，您會將兩個資料庫移轉這種方式：</span><span class="sxs-lookup"><span data-stu-id="da36b-133">In this tutorial, you'll migrate your two databases this way:</span></span>

- <span data-ttu-id="da36b-134">將移轉到開發環境中的兩個 LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-134">Migrate to two LocalDB databases in the development environment.</span></span>
- <span data-ttu-id="da36b-135">將移轉至測試環境中的兩個 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-135">Migrate to two SQL Server Express databases in the test environment.</span></span>
- <span data-ttu-id="da36b-136">將移轉至一個結合完整 SQL Server 資料庫的生產環境中。</span><span class="sxs-lookup"><span data-stu-id="da36b-136">Migrate to one combined full SQL Server database in the production environment.</span></span>

<span data-ttu-id="da36b-137">提醒： 如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="da36b-137">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="installing-sql-server-express"></a><span data-ttu-id="da36b-138">安裝 SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="da36b-138">Installing SQL Server Express</span></span>

<span data-ttu-id="da36b-139">根據預設，Visual Studio 2010 中，會自動安裝 SQL Server Express，但預設並未安裝 Visual Studio 2012。</span><span class="sxs-lookup"><span data-stu-id="da36b-139">SQL Server Express is automatically installed by default with Visual Studio 2010, but by default it is not installed with Visual Studio 2012.</span></span> <span data-ttu-id="da36b-140">若要安裝 SQL Server 2012 Express，請按一下下列連結</span><span class="sxs-lookup"><span data-stu-id="da36b-140">To install SQL Server 2012 Express, click the following link</span></span>

- [<span data-ttu-id="da36b-141">SQL Server Express 2012</span><span class="sxs-lookup"><span data-stu-id="da36b-141">SQL Server Express 2012</span></span>](https://www.microsoft.com/download/details.aspx?id=29062)

<span data-ttu-id="da36b-142">選擇*繁體中文/x64/SQLEXPR\_x64\_ENU.exe*或是*繁體中文/x86/SQLEXPR\_x86\_ENU.exe*，並在 [安裝精靈] 中，接受預設值設定。</span><span class="sxs-lookup"><span data-stu-id="da36b-142">Choose *ENU/x64/SQLEXPR\_x64\_ENU.exe* or *ENU/x86/SQLEXPR\_x86\_ENU.exe*, and in the installation wizard accept the default settings.</span></span> <span data-ttu-id="da36b-143">如需安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。</span><span class="sxs-lookup"><span data-stu-id="da36b-143">For more information about installation options, see [Install SQL Server 2012 from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="creating-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="da36b-144">針對測試環境中建立 SQL Server Express 資料庫</span><span class="sxs-lookup"><span data-stu-id="da36b-144">Creating SQL Server Express Databases for the Test Environment</span></span>

<span data-ttu-id="da36b-145">下一個步驟是建立的 ASP.NET 成員資格和 School 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-145">The next step is to create the ASP.NET membership and School databases.</span></span>

<span data-ttu-id="da36b-146">從**檢視**功能表中，選取**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接**，然後選取**建立新的 SQL Server 資料庫**。</span><span class="sxs-lookup"><span data-stu-id="da36b-146">From the **View** menu select **Server Explorer** (**Database Explorer** in Visual Web Developer), and then right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image1.png)

<span data-ttu-id="da36b-148">中**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**] 方塊中，「 aspnet-測試 」 中**新的資料庫名稱**，然後按一下 [ **[確定]**。</span><span class="sxs-lookup"><span data-stu-id="da36b-148">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-Test" in the **New database name** box, then click **OK**.</span></span>

![Create_New_SQL_Server_Database_aspnet](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image2.png)

<span data-ttu-id="da36b-150">請遵循相同的程序，以建立新的 SQL Server Express School 資料庫，名為"學校-Test"。</span><span class="sxs-lookup"><span data-stu-id="da36b-150">Follow the same procedure to create a new SQL Server Express School database named "School-Test".</span></span>

<span data-ttu-id="da36b-151">（您要新增 「 測試 」 這些資料庫名稱，因為稍後您將在其中建立開發環境中，每個資料庫的其他執行個體，您需要能夠區分這兩組的資料庫。）</span><span class="sxs-lookup"><span data-stu-id="da36b-151">(You're appending "Test" to these database names because later you'll create an additional instance of each database for the development environment, and you need to be able to differentiate the two sets of databases.)</span></span>

<span data-ttu-id="da36b-152">**伺服器總管**現在會顯示兩個新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-152">**Server Explorer** now shows the two new databases.</span></span>

![New_databases_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image3.png)

## <a name="creating-a-grant-script-for-the-new-databases"></a><span data-ttu-id="da36b-154">建立新的資料庫授與指令碼</span><span class="sxs-lookup"><span data-stu-id="da36b-154">Creating a Grant Script for the New Databases</span></span>

<span data-ttu-id="da36b-155">當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區的認證存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-155">When the application runs in IIS on your development computer, the application accesses the database by using the default application pool's credentials.</span></span> <span data-ttu-id="da36b-156">不過，根據預設，應用程式集區身分識別沒有權限來開啟資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-156">However, by default, the application pool identity does not have permission to open the databases.</span></span> <span data-ttu-id="da36b-157">因此，您必須執行的指令碼，以授與該權限。</span><span class="sxs-lookup"><span data-stu-id="da36b-157">So you have to run a script to grant that permission.</span></span> <span data-ttu-id="da36b-158">在本節中，您會建立指令碼，您將會執行更新版本，以確定應用程式在執行於 IIS 時，可以開啟資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-158">In this section you create the script that you'll run later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="da36b-159">在方案的*SolutionFiles*中所建立的資料夾[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，建立新的 SQL 檔案命名為*Grant.sql*。</span><span class="sxs-lookup"><span data-stu-id="da36b-159">In the solution's *SolutionFiles* folder that you created in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, create a new SQL file named *Grant.sql*.</span></span> <span data-ttu-id="da36b-160">將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="da36b-160">Copy the following SQL commands into the file, and then save and close the file:</span></span>

[!code-sql[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample1.sql)]

> [!NOTE]
> <span data-ttu-id="da36b-161">此指令碼可配合搭配 SQL Server 2008 與 Windows 7 中的 IIS 設定，因為它們會指定在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="da36b-161">This script is designed to work with SQL Server 2008 and with the IIS settings in Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="da36b-162">如果您使用不同版本的 SQL Server 或 Windows，或如果您將 IIS 設定您的電腦上以不同的方式，可能必須使用這個指令碼的變更。</span><span class="sxs-lookup"><span data-stu-id="da36b-162">If you're using a different version of SQL Server or of Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="da36b-163">如需有關 SQL Server 指令碼的詳細資訊，請參閱 < [SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。</span><span class="sxs-lookup"><span data-stu-id="da36b-163">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="da36b-164">**安全性注意事項**此指令碼會為 db\_使用者在執行階段，也就是您會有在生產環境中存取資料庫的擁有者權限。</span><span class="sxs-lookup"><span data-stu-id="da36b-164">**Security Note** This script gives db\_owner permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="da36b-165">在某些情況下，您可能想要指定具有完整的資料庫結構描述更新的權限只部署，並指定執行階段不同的使用者具有權限才能讀取和寫入資料的使用者。</span><span class="sxs-lookup"><span data-stu-id="da36b-165">In some scenarios you might want to specify a user that has full database schema update permissions only for deployment, and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="da36b-166">如需詳細資訊，請參閱 <<c0>  **檢閱自動 Web.config 變更的 Code First Migrations**中[部署到 IIS 作為測試環境](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="da36b-166">For more information, see **Reviewing the Automatic Web.config Changes for Code First Migrations** in [Deploying to IIS as a Test Environment](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12.md).</span></span>


## <a name="configuring-database-deployment-for-the-test-environment"></a><span data-ttu-id="da36b-167">設定測試環境的資料庫部署</span><span class="sxs-lookup"><span data-stu-id="da36b-167">Configuring Database Deployment for the Test Environment</span></span>

<span data-ttu-id="da36b-168">接下來，您將設定 Visual Studio，使它將會執行下列工作的每個資料庫：</span><span class="sxs-lookup"><span data-stu-id="da36b-168">Next, you'll configure Visual Studio so that it will do the following tasks for each database:</span></span>

- <span data-ttu-id="da36b-169">產生 SQL 指令碼會建立目的地資料庫中的來源資料庫的結構 （資料表、 資料行、 條件約束等）。</span><span class="sxs-lookup"><span data-stu-id="da36b-169">Generate a SQL script that creates the source database's structure (tables, columns, constraints, etc.) in the destination database.</span></span>
- <span data-ttu-id="da36b-170">產生 SQL 指令碼，將來源資料庫的資料插入至目的地資料庫中的資料表。</span><span class="sxs-lookup"><span data-stu-id="da36b-170">Generate a SQL script that inserts the source database's data into the tables in the destination database.</span></span>
- <span data-ttu-id="da36b-171">執行產生的指令碼，並建立目的地資料庫中授與指令碼。</span><span class="sxs-lookup"><span data-stu-id="da36b-171">Run the generated scripts, and the Grant script that you created, in the destination database.</span></span>

<span data-ttu-id="da36b-172">開啟**專案屬性**視窗，然後選取**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-172">Open the **Project Properties** window and select the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="da36b-173">請確定**主動 （發行）** 或**Release**中選取**組態**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="da36b-173">Make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="da36b-174">按一下 **啟用此頁面**。</span><span class="sxs-lookup"><span data-stu-id="da36b-174">Click **Enable this Page**.</span></span>

![Package_Publish_SQL_tab_Enable_This_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image4.png)

<span data-ttu-id="da36b-176">**封裝/發行 SQL**  索引標籤通常停用，因為它會指定舊版的部署方法。</span><span class="sxs-lookup"><span data-stu-id="da36b-176">The **Package/Publish SQL** tab is normally disabled because it specifies a legacy deployment method.</span></span> <span data-ttu-id="da36b-177">大部分的情況下，您應該設定中的資料庫部署**發佈 Web**精靈。</span><span class="sxs-lookup"><span data-stu-id="da36b-177">For most scenarios, you should configure database deployment in the **Publish Web** wizard.</span></span> <span data-ttu-id="da36b-178">從 SQL Server Compact 移轉至 SQL Server 或 SQL Server Express 是特殊案例，這個方法會是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="da36b-178">Migrating from SQL Server Compact to SQL Server or SQL Server Express is a special case for which this method is a good choice.</span></span>

<span data-ttu-id="da36b-179">按一下 **從 Web.config 匯入**。</span><span class="sxs-lookup"><span data-stu-id="da36b-179">Click **Import from Web.config**.</span></span>

![Selecting_Import_from_Web.config](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image5.png)

<span data-ttu-id="da36b-181">Visual Studio 中的連接字串會尋找*Web.config*檔案中，尋找另一個用於成員資格資料庫，並使用另一個用於 School 資料庫，並將對應至每個連接字串中的資料列**資料庫項目**資料表。</span><span class="sxs-lookup"><span data-stu-id="da36b-181">Visual Studio looks for connection strings in the *Web.config* file, finds one for the membership database and one for the School database, and adds a row corresponding to each connection string in the **Database Entries** table.</span></span> <span data-ttu-id="da36b-182">找到的連接字串會針對現有的 SQL Server Compact 資料庫，而且下一個步驟會設定方式和位置來部署這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-182">The connection strings it finds are for the existing SQL Server Compact databases, and your next step will be to configure how and where to deploy these databases.</span></span>

<span data-ttu-id="da36b-183">輸入中的資料庫部署設定**資料庫項目詳細資料**一節**資料庫項目**資料表。</span><span class="sxs-lookup"><span data-stu-id="da36b-183">You enter database deployment settings in the **Database Entry Details** section below the **Database Entries** table.</span></span> <span data-ttu-id="da36b-184">在顯示的設定**資料庫項目詳細資料**兩者中的資料列屬於 區段**資料庫項目**選取資料表，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="da36b-184">The settings shown in the **Database Entry Details** section pertain to whichever row in the **Database Entries** table is selected, as shown in the following illustration.</span></span>

![Database_Entry_Details_section_of_Package_Publish_SQL_tab](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image6.png)

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="da36b-186">成員資格資料庫進行的部署設定</span><span class="sxs-lookup"><span data-stu-id="da36b-186">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="da36b-187">選取  **DefaultConnection 部署**中的資料列**資料庫項目**資料表中，以設定將套用至成員資格資料庫的設定。</span><span class="sxs-lookup"><span data-stu-id="da36b-187">Select the **DefaultConnection-Deployment** row in the **Database Entries** table in order to configure settings that apply to the membership database.</span></span>

<span data-ttu-id="da36b-188">在 **目的地資料庫的連接字串**，輸入指向新的 SQL Server Express 成員資格資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="da36b-188">In **Connection string for destination database**, enter a connection string that points to the new SQL Server Express membership database.</span></span> <span data-ttu-id="da36b-189">您可以取得您需要從連接字串**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="da36b-189">You can get the connection string you need from **Server Explorer**.</span></span> <span data-ttu-id="da36b-190">在**伺服器總管**，展開**資料連接**，然後選取**aspnetTest**資料庫，然後從**屬性**視窗複製**連接字串**值。</span><span class="sxs-lookup"><span data-stu-id="da36b-190">In **Server Explorer**, expand **Data Connections** and select the **aspnetTest** database, then from the **Properties** window copy the **Connection String** value.</span></span>

![aspnet_connection_string_in_Server_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image7.png)

<span data-ttu-id="da36b-192">相同的連接字串在此處重現：</span><span class="sxs-lookup"><span data-stu-id="da36b-192">The same connection string is reproduced here:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample2.cmd)]

<span data-ttu-id="da36b-193">複製並貼到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-193">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="da36b-194">請確定**提取資料及/或從現有的資料庫結構描述**已選取。</span><span class="sxs-lookup"><span data-stu-id="da36b-194">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span> <span data-ttu-id="da36b-195">這是什麼情況導致自動產生並在目的地資料庫中執行的 SQL 指令碼。</span><span class="sxs-lookup"><span data-stu-id="da36b-195">This is what causes SQL scripts to be automatically generated and run in the destination database.</span></span>

<span data-ttu-id="da36b-196">**來源資料庫的連接字串**的值取自*Web.config*檔案，並指向開發 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-196">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="da36b-197">這是來源資料庫將會使用產生指令碼，將稍後在目的地資料庫中執行。</span><span class="sxs-lookup"><span data-stu-id="da36b-197">This is the source database that will be used to generate the scripts that will run later in the destination database.</span></span> <span data-ttu-id="da36b-198">因為您想要部署之資料庫的生產版本，則會將"aspnet Dev.sdf"變更"aspnet Prod.sdf"。</span><span class="sxs-lookup"><span data-stu-id="da36b-198">Since you want to deploy the production version of the database, change "aspnet-Dev.sdf" to "aspnet-Prod.sdf".</span></span>

<span data-ttu-id="da36b-199">變更**資料庫的指令碼選項**從**僅限結構描述**來**結構描述和資料**，因為您想要複製您的資料 （使用者帳戶和角色），以及資料庫結構。</span><span class="sxs-lookup"><span data-stu-id="da36b-199">Change **Database scripting options** from **Schema Only** to **Schema and data**, since you want to copy your data (user accounts and roles) as well as the database structure.</span></span>

<span data-ttu-id="da36b-200">若要設定執行授與指令碼您稍早建立的部署，您必須將它們新增至**資料庫指令碼**一節。</span><span class="sxs-lookup"><span data-stu-id="da36b-200">To configure deployment to run the grant scripts that you created earlier, you have to add them to the **Database Scripts** section.</span></span> <span data-ttu-id="da36b-201">按一下 **新增指令碼**，然後在**新增 SQL 指令碼**對話方塊方塊中，瀏覽至您儲存授與指令碼的資料夾 （這是包含您的方案檔案的資料夾）。</span><span class="sxs-lookup"><span data-stu-id="da36b-201">Click **Add Script**, and in the **Add SQL Scripts** dialog box, navigate to the folder where you stored the grant script (this is the folder that contains your solution file).</span></span> <span data-ttu-id="da36b-202">選取名為的檔案*Grant.sql*，然後按一下**開啟**。</span><span class="sxs-lookup"><span data-stu-id="da36b-202">Select the file named *Grant.sql*, and click **Open**.</span></span>

<span data-ttu-id="da36b-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-203">[![Select_File_dialog_box_grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image9.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image8.png)</span></span>

<span data-ttu-id="da36b-204">設定**DefaultConnection 部署**中的資料列**資料庫項目**現在看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="da36b-204">The settings for the **DefaultConnection-Deployment** row in **Database Entries** now look like the following illustration:</span></span>

![Database_Entry_Details_for_DefaultConnection_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image10.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="da36b-206">School 資料庫的部署設定</span><span class="sxs-lookup"><span data-stu-id="da36b-206">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="da36b-207">接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定 School 資料庫的部署設定。</span><span class="sxs-lookup"><span data-stu-id="da36b-207">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure deployment settings for the School database.</span></span>

<span data-ttu-id="da36b-208">您可以使用您稍早用來取得新的 SQL Server Express 資料庫的連接字串的相同方法。</span><span class="sxs-lookup"><span data-stu-id="da36b-208">You can use the same method you used earlier to get the connection string for the new SQL Server Express database.</span></span> <span data-ttu-id="da36b-209">複製此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-209">Copy this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample3.cmd)]

<span data-ttu-id="da36b-210">請確定**提取資料及/或從現有的資料庫結構描述**已選取。</span><span class="sxs-lookup"><span data-stu-id="da36b-210">Make sure that **Pull data and/or schema from an existing database** is selected.</span></span>

<span data-ttu-id="da36b-211">**來源資料庫的連接字串**的值取自*Web.config*檔案，並指向開發 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-211">The **Connection string for the source database** value is extracted from the *Web.config* file and points to the development SQL Server Compact database.</span></span> <span data-ttu-id="da36b-212">「 學校 Dev.sdf 」 變成 「 學校-Prod.sdf 「 部署資料庫的實際執行版本。</span><span class="sxs-lookup"><span data-stu-id="da36b-212">Change "School-Dev.sdf" to "School-Prod.sdf" to deploy the production version of the database.</span></span> <span data-ttu-id="da36b-213">(您在應用程式永遠不會建立學校 Prod.sdf 檔案\_Data 資料夾，因此您也會將該檔案從測試環境中，複製到應用程式\_稍後 ContosoUniversity 專案資料夾中的 [資料] 資料夾。)</span><span class="sxs-lookup"><span data-stu-id="da36b-213">(You never created a School-Prod.sdf file in the App\_Data folder, so you'll copy that file from the test environment to the App\_Data folder in the ContosoUniversity project folder later.)</span></span>

<span data-ttu-id="da36b-214">變更**資料庫的指令碼選項**要**結構描述和資料**。</span><span class="sxs-lookup"><span data-stu-id="da36b-214">Change **Database scripting options** to **Schema and data**.</span></span>

<span data-ttu-id="da36b-215">您也想要執行的指令碼，以授與讀取和寫入此資料庫的權限應用程式集區身分識別，因此請新增*Grant.sql*成員資格資料庫的指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="da36b-215">You also want to run the script to grant read and write permission for this database to the application pool identity, so add the *Grant.sql* script file as you did for the membership database.</span></span>

<span data-ttu-id="da36b-216">當您完成時，設定**SchoolContext 部署**中的資料列**資料庫項目**看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="da36b-216">When you're done, the settings for the **SchoolContext-Deployment** row in **Database Entries** look like the following illustration:</span></span>

![Database_Entry_Details_for_SchoolContext_Test](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image11.png)

<span data-ttu-id="da36b-218">變更儲存到**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-218">Save the changes to the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="da36b-219">複製*學校 Prod.sdf*檔案*c:\inetpub\wwwroot\ContosoUniversity\App\_Data*資料夾*應用程式\_資料*資料夾中ContosoUniversity 專案中。</span><span class="sxs-lookup"><span data-stu-id="da36b-219">Copy the *School-Prod.sdf* file from the *c:\inetpub\wwwroot\ContosoUniversity\App\_Data* folder to the *App\_Data* folder in the ContosoUniversity project.</span></span>

### <a name="specifying-transacted-mode-for-the-grant-script"></a><span data-ttu-id="da36b-220">指定授與指令碼的交易的模式</span><span class="sxs-lookup"><span data-stu-id="da36b-220">Specifying Transacted Mode for the Grant Script</span></span>

<span data-ttu-id="da36b-221">部署程序會產生指令碼來部署資料庫結構描述和資料。</span><span class="sxs-lookup"><span data-stu-id="da36b-221">The deployment process generates scripts that deploy the database schema and data.</span></span> <span data-ttu-id="da36b-222">根據預設，這些指令碼會在交易中執行。</span><span class="sxs-lookup"><span data-stu-id="da36b-222">By default, these scripts run in a transaction.</span></span> <span data-ttu-id="da36b-223">不過，自訂的指令碼 （例如授與指令碼中），預設不執行在交易中。</span><span class="sxs-lookup"><span data-stu-id="da36b-223">However, custom scripts (like the grant scripts) by default do not run in a transaction.</span></span> <span data-ttu-id="da36b-224">如果在部署程序中混合交易模式，您可能會收到逾時錯誤，在部署期間執行的指令碼時。</span><span class="sxs-lookup"><span data-stu-id="da36b-224">If the deployment process mixes transaction modes, you might get a timeout error when the scripts run during deployment.</span></span> <span data-ttu-id="da36b-225">在本節中，您可以編輯專案檔才能設定自訂的指令碼，來在交易中執行。</span><span class="sxs-lookup"><span data-stu-id="da36b-225">In this section, you edit the project file in order to configure the custom scripts to run in a transaction.</span></span>

<span data-ttu-id="da36b-226">在 **方案總管**，以滑鼠右鍵按一下**ContosoUniversity**專案，然後選取**卸載專案**。</span><span class="sxs-lookup"><span data-stu-id="da36b-226">In **Solution Explorer**, right-click the **ContosoUniversity** project and select **Unload Project**.</span></span>

![Unload_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image12.png)

<span data-ttu-id="da36b-228">然後再次以滑鼠右鍵按一下專案，並選取**編輯 ContosoUniversity.csproj**。</span><span class="sxs-lookup"><span data-stu-id="da36b-228">Then right-click the project again and select **Edit ContosoUniversity.csproj**.</span></span>

![Edit_Project_in_Solution_Explorer](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image13.png)

<span data-ttu-id="da36b-230">Visual Studio 編輯器會顯示您的專案檔的 XML 內容。</span><span class="sxs-lookup"><span data-stu-id="da36b-230">The Visual Studio editor shows you the XML content of the project file.</span></span> <span data-ttu-id="da36b-231">請注意，有數個`PropertyGroup`項目。</span><span class="sxs-lookup"><span data-stu-id="da36b-231">Notice that there are several `PropertyGroup` elements.</span></span> <span data-ttu-id="da36b-232">(在圖中，內容`PropertyGroup`略過項目。)</span><span class="sxs-lookup"><span data-stu-id="da36b-232">(In the image, the contents of the `PropertyGroup` elements have been omitted.)</span></span>

![專案檔編輯器 視窗](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image15.png)

<span data-ttu-id="da36b-234">第一個不含任何`Condition`屬性，不論套用設定組建組態。</span><span class="sxs-lookup"><span data-stu-id="da36b-234">The first one, which has no `Condition` attribute, is for settings that apply regardless of build configuration.</span></span> <span data-ttu-id="da36b-235">一`PropertyGroup`項目僅適用於偵錯組建組態 (請注意`Condition`屬性)、 一個僅適用於發行組建組態，而且其中一個只適用於測試的組建組態。</span><span class="sxs-lookup"><span data-stu-id="da36b-235">One `PropertyGroup` element applies only to the Debug build configuration (note the `Condition` attribute), one applies only to the Release build configuration, and one applies only to the Test build configuration.</span></span> <span data-ttu-id="da36b-236">內`PropertyGroup`發行 組建組態項目，您會看到`PublishDatabaseSettings`上輸入包含設定項目**封裝/發行 SQL**  索引標籤。沒有`Object`授與指令碼的每個對應的項目指定 （請注意這兩個執行個體的 「 Grant.sql"中）。</span><span class="sxs-lookup"><span data-stu-id="da36b-236">Within the `PropertyGroup` element for the Release build configuration, you'll see a `PublishDatabaseSettings` element that contains the settings you entered on the **Package/Publish SQL** tab. There is an `Object` element that corresponds to each of the grant scripts you specified (notice the two instances of "Grant.sql").</span></span> <span data-ttu-id="da36b-237">根據預設，`Transacted`的屬性`Source`每個授與指令碼的項目是`False`。</span><span class="sxs-lookup"><span data-stu-id="da36b-237">By default, the `Transacted` attribute of the `Source` element for each grant script is `False`.</span></span>

![Transacted_false](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image16.png)

<span data-ttu-id="da36b-239">值變更`Transacted`的屬性`Source`項目`True`。</span><span class="sxs-lookup"><span data-stu-id="da36b-239">Change the value of the `Transacted` attribute of the `Source` element to `True`.</span></span>

![Transacted_true](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image17.png)

<span data-ttu-id="da36b-241">儲存並關閉專案檔，然後在專案上按一下滑鼠右鍵**方案總管**，然後選取**重新載入專案**。</span><span class="sxs-lookup"><span data-stu-id="da36b-241">Save and close the project file, and then right-click the project in **Solution Explorer** and select **Reload Project**.</span></span>

![Reload_project](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image18.png)

## <a name="setting-up-webconfig-transformations-for-the-connection-strings"></a><span data-ttu-id="da36b-243">設定連接字串的 Web.Config 轉換</span><span class="sxs-lookup"><span data-stu-id="da36b-243">Setting up Web.Config Transformations for the Connection Strings</span></span>

<span data-ttu-id="da36b-244">連接字串，針對新的 SQL Express 資料庫，是您在上輸入**封裝/發行 SQL**  索引標籤用於 Web deploy 只在部署期間更新目的地資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-244">The connection strings for the new SQL Express databases that you entered on the **Package/Publish SQL** tab are used by Web Deploy only for updating the destination database during deployment.</span></span> <span data-ttu-id="da36b-245">您仍然必須設定*Web.config*轉換，以便在已部署的連接字串*Web.config*檔案指向新的 SQL Server Express 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-245">You still have to set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file point to the new SQL Server Express databases.</span></span> <span data-ttu-id="da36b-246">(當您使用**封裝/發行 SQL**索引標籤上，您無法在發行設定檔中設定連接字串。)</span><span class="sxs-lookup"><span data-stu-id="da36b-246">(When you use the **Package/Publish SQL** tab, you can't configure connection strings in the publish profile.)</span></span>

<span data-ttu-id="da36b-247">開啟*Web.Test.config* ，並取代`connectionStrings`項目`connectionStrings`在下列範例中的項目。</span><span class="sxs-lookup"><span data-stu-id="da36b-247">Open *Web.Test.config* and replace the `connectionStrings` element with the `connectionStrings` element in the following example.</span></span> <span data-ttu-id="da36b-248">（請確定您僅能複製 connectionStrings 項目，而不周圍的程式碼，以提供內容如下所示）。</span><span class="sxs-lookup"><span data-stu-id="da36b-248">(Make sure you only copy the connectionStrings element, not the surrounding code that is shown here to provide context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample4.xml?highlight=2-11)]

<span data-ttu-id="da36b-249">此程式碼會造成`connectionString`並`providerName`每個屬性`add`要被取代的項目中已部署*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="da36b-249">This code causes the `connectionString` and `providerName` attributes of each `add` element to be replaced in the deployed *Web.config* file.</span></span> <span data-ttu-id="da36b-250">這些連接字串不是您在輸入項目完全相同**封裝/發行 SQL**  索引標籤。設定"MultipleActiveResultSets = True 」 是因為 Entity Framework 和通用提供者必須已加入它們。</span><span class="sxs-lookup"><span data-stu-id="da36b-250">These connection strings are not identical to the ones you entered in the **Package/Publish SQL** tab. The setting "MultipleActiveResultSets=True" has been added to them because it's required for the Entity Framework and the Universal Providers.</span></span>

## <a name="installing-sql-server-compact"></a><span data-ttu-id="da36b-251">安裝 SQL Server Compact</span><span class="sxs-lookup"><span data-stu-id="da36b-251">Installing SQL Server Compact</span></span>

<span data-ttu-id="da36b-252">SqlServerCompact NuGet 套件提供 SQL Server Compact 資料庫引擎的 Contoso 大學應用程式的組件。</span><span class="sxs-lookup"><span data-stu-id="da36b-252">The SqlServerCompact NuGet package provides the SQL Server Compact database engine assemblies for the Contoso University application.</span></span> <span data-ttu-id="da36b-253">但現在它不是應用程式，但 Web Deploy，必須要能夠讀取 SQL Server Compact 資料庫中，若要建立 SQL Server 資料庫中執行的指令碼。</span><span class="sxs-lookup"><span data-stu-id="da36b-253">But now it is not the application but Web Deploy that must be able to read the SQL Server Compact databases, in order to create scripts to run in the SQL Server databases.</span></span> <span data-ttu-id="da36b-254">若要啟用 Web Deploy 讀取 SQL Server Compact 資料庫，請安裝 SQL Server Compact 在開發電腦上使用下列連結： [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6)。</span><span class="sxs-lookup"><span data-stu-id="da36b-254">To enable Web Deploy to read SQL Server Compact databases, install SQL Server Compact on the development computer by using the following link: [Microsoft SQL Server Compact 4.0](https://www.microsoft.com/downloads/details.aspx?FamilyID=15F7C9B3-A150-4AD2-823E-E4E0DCF85DF6).</span></span>

## <a name="deploying-to-the-test-environment"></a><span data-ttu-id="da36b-255">部署到測試環境</span><span class="sxs-lookup"><span data-stu-id="da36b-255">Deploying to the Test Environment</span></span>

<span data-ttu-id="da36b-256">若要發行到測試環境，您必須建立已設定為使用的發行設定檔**封裝/發行 SQL**的資料庫發行的發行設定檔資料庫設定 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-256">In order to publish to the Test environment, you have to create a publish profile that is configured to use the **Package/Publish SQL** tab for database publishing instead of the publish profile database settings.</span></span>

<span data-ttu-id="da36b-257">首先，刪除現有的 「 測試 」 設定檔。</span><span class="sxs-lookup"><span data-stu-id="da36b-257">First, delete the existing Test profile.</span></span>

<span data-ttu-id="da36b-258">在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="da36b-258">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="da36b-259">選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-259">Select the **Profile** tab.</span></span>

<span data-ttu-id="da36b-260">按一下 **管理設定檔**。</span><span class="sxs-lookup"><span data-stu-id="da36b-260">Click **Manage Profiles**.</span></span>

<span data-ttu-id="da36b-261">選取 **測試**，按一下**移除**，然後按一下**關閉**。</span><span class="sxs-lookup"><span data-stu-id="da36b-261">Select **Test**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="da36b-262">關閉**發佈 Web** 」 精靈來儲存這項變更。</span><span class="sxs-lookup"><span data-stu-id="da36b-262">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="da36b-263">接下來，建立新的 「 測試 」 設定檔，並使用它來發行專案。</span><span class="sxs-lookup"><span data-stu-id="da36b-263">Next, create a new Test profile and use it to publish the project.</span></span>

<span data-ttu-id="da36b-264">在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="da36b-264">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="da36b-265">選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-265">Select the **Profile** tab.</span></span>

<span data-ttu-id="da36b-266">選取  **&lt;新...&gt;** 從下拉式清單，然後輸入"Test"作為設定檔名稱。</span><span class="sxs-lookup"><span data-stu-id="da36b-266">Select **&lt;New...&gt;** from the drop-down list, and enter "Test" as the profile name.</span></span>

<span data-ttu-id="da36b-267">在 **服務 URL**方塊中，輸入*localhost*。</span><span class="sxs-lookup"><span data-stu-id="da36b-267">In the **Service URL** box, enter *localhost*.</span></span>

<span data-ttu-id="da36b-268">在 **網站/應用程式**方塊中，輸入*Default Web Site/ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="da36b-268">In the **Site/application** box, enter *Default Web Site/ContosoUniversity*.</span></span>

<span data-ttu-id="da36b-269">在 **目的地 URL**方塊中，輸入 `http://localhost/ContosoUniversity/` 。</span><span class="sxs-lookup"><span data-stu-id="da36b-269">In the **Destination URL** box, enter `http://localhost/ContosoUniversity/`.</span></span>

<span data-ttu-id="da36b-270">按 [ **下一步**]。</span><span class="sxs-lookup"><span data-stu-id="da36b-270">Click **Next**.</span></span>

<span data-ttu-id="da36b-271">**設定**索引標籤會警告您，**封裝/發行 SQL**已設定 索引標籤，並且讓您覆寫它們，即可啟用新的資料庫發行改進的機會。</span><span class="sxs-lookup"><span data-stu-id="da36b-271">The **Settings** tab warns you that the **Package/Publish SQL** tab has been configured, and it gives you an opportunity to override them by clicking enable the new database publishing improvements.</span></span> <span data-ttu-id="da36b-272">針對此部署，您不想覆寫**封裝/發行 SQL**索引標籤上設定，因此只要按一下**下一步**。</span><span class="sxs-lookup"><span data-stu-id="da36b-272">For this deployment you don't want to override the **Package/Publish SQL** tab settings, so just click **Next**.</span></span>

![Publish_Web_wizard_Settings_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image19.png)

<span data-ttu-id="da36b-274">上的訊息**Preview**  索引標籤表示**沒有任何資料庫選取要發行**，但這只表示，發行設定檔中未設定資料庫發行。</span><span class="sxs-lookup"><span data-stu-id="da36b-274">A message on the **Preview** tab indicates that **No databases are selected to publish**, but this only means that database publishing is not configured in the publish profile.</span></span>

<span data-ttu-id="da36b-275">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="da36b-275">Click **Publish**.</span></span>

![Publish_Web_wizard_Preview_tab_Migrate](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image20.png)

<span data-ttu-id="da36b-277">Visual Studio 部署應用程式和瀏覽器開啟至網站的 [首頁] 頁面中的測試環境。</span><span class="sxs-lookup"><span data-stu-id="da36b-277">Visual Studio deploys the application and opens the browser to the home page of the site in the test environment.</span></span> <span data-ttu-id="da36b-278">執行 Instructors 頁面，以查看它會顯示您稍早所見的相同資料。</span><span class="sxs-lookup"><span data-stu-id="da36b-278">Run the Instructors page to see that it displays the same data that you saw earlier.</span></span> <span data-ttu-id="da36b-279">執行**新增的學生**頁面上，加入新的學生，然後檢視 在新的學生**學生**頁面。</span><span class="sxs-lookup"><span data-stu-id="da36b-279">Run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="da36b-280">這會確認您可以更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-280">This verifies that the you can update the database.</span></span> <span data-ttu-id="da36b-281">選取 **更新信用額度**（您必須登入） 的頁面確認已部署的成員資格資料庫，而且您可以存取它。</span><span class="sxs-lookup"><span data-stu-id="da36b-281">Select the **Update Credits** page (you'll need to log in) to verify that the membership database was deployed and you have access to it.</span></span>

## <a name="creating-a-sql-server-database-for-the-production-environment"></a><span data-ttu-id="da36b-282">針對生產環境中建立 SQL Server 資料庫</span><span class="sxs-lookup"><span data-stu-id="da36b-282">Creating a SQL Server Database for the Production Environment</span></span>

<span data-ttu-id="da36b-283">既然您已部署至測試環境，您可以設定部署至生產環境就緒。</span><span class="sxs-lookup"><span data-stu-id="da36b-283">Now that you've deployed to the test environment, you're ready to set up deployment to production.</span></span> <span data-ttu-id="da36b-284">就如同針對測試環境中，建立將部署到資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-284">You begin as you did for the test environment, by creating a database to deploy to.</span></span> <span data-ttu-id="da36b-285">從概觀，您應該還記得，Cytanium Lite 主控方案將只允許單一的 SQL Server 資料庫，因此您會將只有一個資料庫，而不是兩個設定。</span><span class="sxs-lookup"><span data-stu-id="da36b-285">As you recall from the Overview, the Cytanium Lite hosting plan only allows a single SQL Server database, so you will set up only one database, not two.</span></span> <span data-ttu-id="da36b-286">所有的資料表和資料的成員資格和學校 SQL Server Compact 資料庫將會部署到生產環境中的一個 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-286">All of the tables and data from the membership and School SQL Server Compact databases will be deployed into one SQL Server database in production.</span></span>

<span data-ttu-id="da36b-287">移至在 Cytanium 控制台中[ http://panel.cytanium.com ](http://panel.cytanium.com)。</span><span class="sxs-lookup"><span data-stu-id="da36b-287">Go to the Cytanium control panel at [http://panel.cytanium.com](http://panel.cytanium.com).</span></span> <span data-ttu-id="da36b-288">按住滑鼠**資料庫**，然後按一下**SQL Server 2008**。</span><span class="sxs-lookup"><span data-stu-id="da36b-288">Hold the mouse over **Databases** and then click **SQL Server 2008**.</span></span>

<span data-ttu-id="da36b-289">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-289">[![Selecting_Databases_in_Control_Panel](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image22.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image21.png)</span></span>

<span data-ttu-id="da36b-290">在  **SQL Server 2008**頁面上，按一下**Create Database**。</span><span class="sxs-lookup"><span data-stu-id="da36b-290">In the **SQL Server 2008** page, click **Create Database**.</span></span>

<span data-ttu-id="da36b-291">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-291">[![Selecting_Create_Database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image24.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image23.png)</span></span>

<span data-ttu-id="da36b-292">命名資料庫 [學校]，然後按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="da36b-292">Name the database "School" and click **Save**.</span></span> <span data-ttu-id="da36b-293">（頁面會自動新增前置詞"contosou 」，因此有效的名稱會是"contosouSchool 」）。</span><span class="sxs-lookup"><span data-stu-id="da36b-293">(The page automatically adds the prefix "contosou", so the effective name will be "contosouSchool".)</span></span>

<span data-ttu-id="da36b-294">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-294">[![Naming_the_database](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image26.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image25.png)</span></span>

<span data-ttu-id="da36b-295">在相同頁面上，按一下**Create User**。</span><span class="sxs-lookup"><span data-stu-id="da36b-295">On the same page, click **Create User**.</span></span> <span data-ttu-id="da36b-296">在 Cytanium 的伺服器，而不是使用整合式的 Windows 安全性，並讓應用程式集區識別，開啟您的資料庫，您將建立具有權限開啟您的資料庫的使用者。</span><span class="sxs-lookup"><span data-stu-id="da36b-296">On Cytanium's servers, rather than using integrated Windows security and letting the application pool identity open your database, you'll create a user that has authority to open your database.</span></span> <span data-ttu-id="da36b-297">將使用者的認證加入到生產環境中的連接字串*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="da36b-297">You'll add the user's credentials to the connection strings that go in the production *Web.config* file.</span></span> <span data-ttu-id="da36b-298">在此步驟中，您會建立這些認證。</span><span class="sxs-lookup"><span data-stu-id="da36b-298">In this step you create those credentials.</span></span>

<span data-ttu-id="da36b-299">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-299">[![Creating_a_database_user](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image28.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image27.png)</span></span>

<span data-ttu-id="da36b-300">中的必要欄位填滿**SQL 使用者屬性**頁面：</span><span class="sxs-lookup"><span data-stu-id="da36b-300">Fill in the required fields in the **SQL User Properties** page:</span></span>

- <span data-ttu-id="da36b-301">請輸入"ContosoUniversityUser 」 作為名稱。</span><span class="sxs-lookup"><span data-stu-id="da36b-301">Enter "ContosoUniversityUser" as the name.</span></span>
- <span data-ttu-id="da36b-302">輸入密碼。</span><span class="sxs-lookup"><span data-stu-id="da36b-302">Enter a password.</span></span>
- <span data-ttu-id="da36b-303">選取  **contosouSchool**作為預設的資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-303">Select **contosouSchool** as the default database.</span></span>
- <span data-ttu-id="da36b-304">選取  **contosouSchool**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="da36b-304">Select the **contosouSchool** check box.</span></span>

<span data-ttu-id="da36b-305">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="da36b-305">[![SQL_User_Properties_page](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image30.png)](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image29.png)</span></span>

## <a name="configuring-database-deployment-for-the-production-environment"></a><span data-ttu-id="da36b-306">設定生產環境的資料庫部署</span><span class="sxs-lookup"><span data-stu-id="da36b-306">Configuring Database Deployment for the Production Environment</span></span>

<span data-ttu-id="da36b-307">現在您已準備好進行設定中的資料庫部署設定**封裝/發行 SQL**索引標籤，如先前所做為測試環境。</span><span class="sxs-lookup"><span data-stu-id="da36b-307">Now you're ready to set up database deployment settings in the **Package/Publish SQL** tab, as you did earlier for the test environment.</span></span>

<span data-ttu-id="da36b-308">開啟**專案屬性**視窗中，選取**封裝/發行 SQL**索引標籤，並確定**作用 （發行）** 或是**版本**是中已選取**組態**下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="da36b-308">Open the **Project Properties** window, select the **Package/Publish SQL** tab, and make sure that **Active (Release)** or **Release** is selected in the **Configuration** drop-down list.</span></span>

<span data-ttu-id="da36b-309">當您設定每個資料庫的部署設定時，您針對生產和測試環境中所執行的主要差別是在您設定連接字串的方式。</span><span class="sxs-lookup"><span data-stu-id="da36b-309">When you configure deployment settings for each database, the key difference between what you do for production and test environments is in how you configure connection strings.</span></span> <span data-ttu-id="da36b-310">為測試環境中，您輸入不同的目的地資料庫的連接字串，但生產環境的目的地連接字串會適用於這兩個資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-310">For the test environment you entered different destination database connection strings, but for the production environment the destination connection string will be the same for both databases.</span></span> <span data-ttu-id="da36b-311">這是因為您要將這兩個資料庫部署到生產環境中的一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-311">This is because you are deploying both databases to one database in production.</span></span>

### <a name="configuring-deployment-settings-for-the-membership-database"></a><span data-ttu-id="da36b-312">成員資格資料庫進行的部署設定</span><span class="sxs-lookup"><span data-stu-id="da36b-312">Configuring Deployment Settings for the Membership Database</span></span>

<span data-ttu-id="da36b-313">若要設定將套用至成員資格資料庫的設定，請選取**DefaultConnection 部署**中的資料列**資料庫項目**資料表。</span><span class="sxs-lookup"><span data-stu-id="da36b-313">To configure settings that apply to the membership database, select the **DefaultConnection-Deployment** row in the **Database Entries** table.</span></span>

<span data-ttu-id="da36b-314">在 **目的地資料庫的連接字串**，輸入指向您剛才建立的新實際執行 SQL Server 資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="da36b-314">In **Connection string for destination database**, enter a connection string that points to the new production SQL Server database that you just created.</span></span> <span data-ttu-id="da36b-315">您可以從您的歡迎電子郵件，以取得連接字串。</span><span class="sxs-lookup"><span data-stu-id="da36b-315">You can get the connection string from your welcome email.</span></span> <span data-ttu-id="da36b-316">電子郵件的相關部分包含下列的範例連接字串：</span><span class="sxs-lookup"><span data-stu-id="da36b-316">The relevant part of the email contains the following sample connection string:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample5.cmd)]

<span data-ttu-id="da36b-317">取代的三個變數之後，您需要的連接字串看起來像此範例中：</span><span class="sxs-lookup"><span data-stu-id="da36b-317">After you replace the three variables, the connection string you need looks like this example:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample6.cmd)]

<span data-ttu-id="da36b-318">複製並貼到此連接字串**目的地資料庫的連接字串**中**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-318">Copy and paste this connection string into **Connection string for destination database** in the **Package/Publish SQL** tab.</span></span>

<span data-ttu-id="da36b-319">請確定**提取資料及/或從現有的資料庫結構描述**是仍選取，而**資料庫的指令碼選項**仍**結構描述和資料**。</span><span class="sxs-lookup"><span data-stu-id="da36b-319">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="da36b-320">在 **資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="da36b-320">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

![Disable_Grant_script](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/_static/image31.png)

### <a name="configuring-deployment-settings-for-the-school-database"></a><span data-ttu-id="da36b-322">School 資料庫的部署設定</span><span class="sxs-lookup"><span data-stu-id="da36b-322">Configuring Deployment Settings for the School Database</span></span>

<span data-ttu-id="da36b-323">接下來，選取**SchoolContext 部署**中的資料列**資料庫項目**資料表中，以設定 School 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-323">Next, select the **SchoolContext-Deployment** row in the **Database Entries** table in order to configure the School database settings.</span></span>

<span data-ttu-id="da36b-324">複製到相同的連接字串**目的地資料庫的連接字串**您複製到該欄位的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-324">Copy the same connection string into **Connection string for destination database** that you copied into that field for the membership database.</span></span>

<span data-ttu-id="da36b-325">請確定**提取資料及/或從現有的資料庫結構描述**是仍選取，而**資料庫的指令碼選項**仍**結構描述和資料**。</span><span class="sxs-lookup"><span data-stu-id="da36b-325">Make sure that **Pull data and/or schema from an existing database** is still selected, and that **Database scripting options** is still **Schema and Data**.</span></span>

<span data-ttu-id="da36b-326">在 **資料庫指令碼**方塊中，清除 Grant.sql 指令碼旁邊的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="da36b-326">In the **Database Scripts** box, clear the check box next to the Grant.sql script.</span></span>

<span data-ttu-id="da36b-327">變更儲存到**封裝/發行 SQL**  索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-327">Save the changes to the **Package/Publish SQL** tab.</span></span>

## <a name="setting-up-webconfig-transforms-for-the-connection-strings-to-production-databases"></a><span data-ttu-id="da36b-328">設定 Web.Config 轉換到生產環境資料庫的連接字串</span><span class="sxs-lookup"><span data-stu-id="da36b-328">Setting Up Web.Config Transforms for the Connection Strings to Production Databases</span></span>

<span data-ttu-id="da36b-329">接下來，您會設定*Web.config*轉換，以便在已部署的連接字串*Web.config*檔以指向新的生產環境資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-329">Next, you'll set up *Web.config* transformations so that the connection strings in the deployed *Web.config* file to point to the new production database.</span></span> <span data-ttu-id="da36b-330">您輸入的連接字串**封裝/發行 SQL** Web deploy 使用的索引標籤是應用程式需要使用時，除了新增一個相同`MultipleResultSets`選項。</span><span class="sxs-lookup"><span data-stu-id="da36b-330">The connection string that you entered on the **Package/Publish SQL** tab for Web Deploy to use is the same as the one the application needs to use, except for the addition of the `MultipleResultSets` option.</span></span>

<span data-ttu-id="da36b-331">開啟*Web.Production.config* ，並取代`connectionStrings`項目`connectionStrings`項目，如下列範例所示。</span><span class="sxs-lookup"><span data-stu-id="da36b-331">Open *Web.Production.config* and replace the `connectionStrings` element with a `connectionStrings` element that looks like the following example.</span></span> <span data-ttu-id="da36b-332">(只複製`connectionStrings`項目，不可說明內容周圍的標記。)</span><span class="sxs-lookup"><span data-stu-id="da36b-332">(Only copy the `connectionStrings` element, not the surrounding tags that are provided to show the context.)</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample7.xml?highlight=2-11)]

<span data-ttu-id="da36b-333">您有時會看到建議，告訴您永遠加密中的連接字串*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="da36b-333">You sometimes see advice that tells you to always encrypt connection strings in the *Web.config* file.</span></span> <span data-ttu-id="da36b-334">這可能是適當，如果您已部署到您自己的公司網路上的伺服器。</span><span class="sxs-lookup"><span data-stu-id="da36b-334">This might be appropriate if you were deploying to servers on your own company's network.</span></span> <span data-ttu-id="da36b-335">當您要部署到共用裝載環境中時，不過，您將信任的主控提供者，安全性作法，並不是必要或實際加密連接字串。</span><span class="sxs-lookup"><span data-stu-id="da36b-335">When you are deploying to a shared hosting environment, though, you're trusting the security practices of the hosting provider, and it's not necessary or practical to encrypt the connection strings.</span></span>

## <a name="deploying-to-the-production-environment"></a><span data-ttu-id="da36b-336">部署到生產環境</span><span class="sxs-lookup"><span data-stu-id="da36b-336">Deploying to the Production Environment</span></span>

<span data-ttu-id="da36b-337">現在您已準備好要部署到生產環境。</span><span class="sxs-lookup"><span data-stu-id="da36b-337">Now you're ready to deploy to production.</span></span> <span data-ttu-id="da36b-338">Web Deploy 會讀取您的專案中的 SQL Server Compact 資料庫*應用程式\_資料*資料夾，並重新建立所有的資料表和實際執行 SQL Server 資料庫中的資料。</span><span class="sxs-lookup"><span data-stu-id="da36b-338">Web Deploy will read the SQL Server Compact databases in your project's *App\_Data* folder and re-create all of their tables and data in the production SQL Server database.</span></span> <span data-ttu-id="da36b-339">若要使用發行**封裝/發行 Web**  索引標籤上設定，您必須建立新的發行設定檔，用於生產環境。</span><span class="sxs-lookup"><span data-stu-id="da36b-339">In order to publish by using the **Package/Publish Web** tab settings, you have to create a new publish profile for production.</span></span>

<span data-ttu-id="da36b-340">首先，刪除現有的生產環境設定檔，如同您先前的測試設定檔。</span><span class="sxs-lookup"><span data-stu-id="da36b-340">First, delete the existing Production profile as you did the Test profile earlier.</span></span>

<span data-ttu-id="da36b-341">在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="da36b-341">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="da36b-342">選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-342">Select the **Profile** tab.</span></span>

<span data-ttu-id="da36b-343">按一下 **管理設定檔**。</span><span class="sxs-lookup"><span data-stu-id="da36b-343">Click **Manage Profiles**.</span></span>

<span data-ttu-id="da36b-344">選取 **生產**，按一下**移除**，然後按一下**關閉**。</span><span class="sxs-lookup"><span data-stu-id="da36b-344">Select **Production**, click **Remove**, and then click **Close**.</span></span>

<span data-ttu-id="da36b-345">關閉**發佈 Web** 」 精靈來儲存這項變更。</span><span class="sxs-lookup"><span data-stu-id="da36b-345">Close the **Publish Web** wizard to save this change.</span></span>

<span data-ttu-id="da36b-346">接下來，建立新的實際執行設定檔，並使用它來發行專案。</span><span class="sxs-lookup"><span data-stu-id="da36b-346">Next, create a new Production profile and use it to publish the project.</span></span>

<span data-ttu-id="da36b-347">在 **方案總管**，以滑鼠右鍵按一下 ContosoUniversity 專案，然後按一下**發佈**。</span><span class="sxs-lookup"><span data-stu-id="da36b-347">In **Solution Explorer**, right-click the ContosoUniversity project, and click **Publish**.</span></span>

<span data-ttu-id="da36b-348">選取 [**設定檔**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-348">Select the **Profile** tab.</span></span>

<span data-ttu-id="da36b-349">按一下 **匯入**，並選取您稍早下載的.publishsettings 檔案。</span><span class="sxs-lookup"><span data-stu-id="da36b-349">Click **Import**, and select the .publishsettings file that you downloaded earlier.</span></span>

<span data-ttu-id="da36b-350">在上**連接**索引標籤中變更**目的地 URL**暫存 URL 正確，在此範例是 http://contosouniversity.com.vserver01.cytanium.com 。</span><span class="sxs-lookup"><span data-stu-id="da36b-350">On the **Connection** tab, change the **Destination URL** to the correct temporary URL, which in this example is http://contosouniversity.com.vserver01.cytanium.com.</span></span>

<span data-ttu-id="da36b-351">將設定檔的重新命名為實際執行環境。</span><span class="sxs-lookup"><span data-stu-id="da36b-351">Rename the profile to Production.</span></span> <span data-ttu-id="da36b-352">(選取**設定檔**索引標籤，然後按一下**管理設定檔**若要這麼做)。</span><span class="sxs-lookup"><span data-stu-id="da36b-352">(Select the **Profile** tab and click **Manage Profiles** to do that).</span></span>

<span data-ttu-id="da36b-353">關閉**發佈 Web** 」 精靈來儲存您的變更。</span><span class="sxs-lookup"><span data-stu-id="da36b-353">Close the **Publish Web** wizard to save your changes.</span></span>

<span data-ttu-id="da36b-354">在實際的應用程式，在其資料庫已更新在生產環境中，您必須執行兩個額外步驟現在發行之前：</span><span class="sxs-lookup"><span data-stu-id="da36b-354">In a real application in which the database was being updated in production, you would do two additional steps now before you publish:</span></span>

1. <span data-ttu-id="da36b-355">上傳*應用程式\_offline.htm*，如下所示[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="da36b-355">Upload *app\_offline.htm*, as shown in the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial.</span></span>
2. <span data-ttu-id="da36b-356">使用**檔案管理員**Cytanium 控制台的複製功能*aspnet Prod.sdf*並*學校 Prod.sdf*檔案從生產網站到*應用程式\_資料*ContosoUniversity 專案的資料夾。</span><span class="sxs-lookup"><span data-stu-id="da36b-356">Use the **File Manager** feature of the Cytanium control panel to copy the *aspnet-Prod.sdf* and *School-Prod.sdf* files from the production site to the *App\_Data* folder of the ContosoUniversity project.</span></span> <span data-ttu-id="da36b-357">這可確保您要部署到新的 SQL Server 資料庫的資料，包括最新生產環境網站所做的更新。</span><span class="sxs-lookup"><span data-stu-id="da36b-357">This ensures that the data you're deploying to the new SQL Server database includes the latest updates made by your production website.</span></span>

<span data-ttu-id="da36b-358">在**Web 單鍵發佈**工具列上，請確定**生產**設定檔已選取，然後按一下**發行**。</span><span class="sxs-lookup"><span data-stu-id="da36b-358">In the **Web One Click Publish** toolbar, make sure that the **Production** profile is selected, and then click **Publish**.</span></span>

<span data-ttu-id="da36b-359">如果您上傳<em>應用程式\_offline.htm</em>發佈之前，您必須使用<strong>檔案管理員</strong>Cytanium 控制項台中刪除公用程式<em>應用程式\_離線。</em>在測試之前 htm。</span><span class="sxs-lookup"><span data-stu-id="da36b-359">If you uploaded <em>app\_offline.htm</em> before publishing, you have to use the <strong>File Manager</strong> utility in the Cytanium control panel to delete <em>app\_offline.</em>htm before you test.</span></span> <span data-ttu-id="da36b-360">您也可以在此同時刪除<em>.sdf</em>從檔案<em>應用程式\_資料</em>資料夾。</span><span class="sxs-lookup"><span data-stu-id="da36b-360">You can also at the same time delete the <em>.sdf</em> files from the <em>App\_Data</em> folder.</span></span>

<span data-ttu-id="da36b-361">您現在可以開啟瀏覽器並移至您的公用網站 URL，以測試應用程式的相同方式部署到測試環境之後。</span><span class="sxs-lookup"><span data-stu-id="da36b-361">You can now open a browser and go to the URL of your public site to test the application the same way you did after deploying to the test environment.</span></span>

## <a name="switching-to-sql-server-express-localdb-in-development"></a><span data-ttu-id="da36b-362">切換至 SQL Server Express LocalDB 中開發</span><span class="sxs-lookup"><span data-stu-id="da36b-362">Switching to SQL Server Express LocalDB in Development</span></span>

<span data-ttu-id="da36b-363">如概觀中所述，它通常最好是在您用於測試和生產環境的開發中使用相同的資料庫引擎。</span><span class="sxs-lookup"><span data-stu-id="da36b-363">As was explained in the Overview, it's generally best to use the same database engine in development that you use in test and production.</span></span> <span data-ttu-id="da36b-364">（請記得在開發中使用 SQL Server Express 的優點是資料庫的適用相同開發、 測試和生產環境中）。在本節中您將設定 ContosoUniversity 專案從 Visual Studio 執行應用程式時，使用 SQL Server Express LocalDB。</span><span class="sxs-lookup"><span data-stu-id="da36b-364">(Remember that the advantage to using SQL Server Express in development is that the database will work the same in your development, test, and production environments.) In this section you'll set up the ContosoUniversity project to use SQL Server Express LocalDB when you run the application from Visual Studio.</span></span>

<span data-ttu-id="da36b-365">若要執行此移轉的最簡單方式是讓 Code First 和成員資格系統為您建立這兩個新的開發資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-365">The simplest way to perform this migration is to let Code First and the membership system create both new development databases for you.</span></span> <span data-ttu-id="da36b-366">使用此方法來移轉需要三個步驟：</span><span class="sxs-lookup"><span data-stu-id="da36b-366">Using this method to migrate requires three steps:</span></span>

1. <span data-ttu-id="da36b-367">變更連接字串，以指定新的 SQL Express LocalDB 資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-367">Change the connection strings to specify new SQL Express LocalDB databases.</span></span>
2. <span data-ttu-id="da36b-368">執行 Web Site Administration Tool，若要建立的系統管理員使用者。</span><span class="sxs-lookup"><span data-stu-id="da36b-368">Run the Web Site Administration Tool to create an administrator user.</span></span> <span data-ttu-id="da36b-369">這會建立成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-369">This creates the membership database.</span></span>
3. <span data-ttu-id="da36b-370">您可以使用 Code First 移轉更新資料庫命令來建立並植入的應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="da36b-370">Use the Code First Migrations update-database command to create and seed the application database.</span></span>

### <a name="updating-connection-strings-in-the-webconfig-file"></a><span data-ttu-id="da36b-371">正在更新 Web.config 檔案中的連接字串</span><span class="sxs-lookup"><span data-stu-id="da36b-371">Updating Connection Strings in the Web.config file</span></span>

<span data-ttu-id="da36b-372">開啟*Web.config*檔案，並將`connectionStrings`為下列程式碼的項目：</span><span class="sxs-lookup"><span data-stu-id="da36b-372">Open the *Web.config* file and replace the `connectionStrings` element with the following code:</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample8.xml)]

### <a name="creating-the-membership-database"></a><span data-ttu-id="da36b-373">建立成員資格資料庫</span><span class="sxs-lookup"><span data-stu-id="da36b-373">Creating the Membership Database</span></span>

<span data-ttu-id="da36b-374">在 **方案總管**，選取 ContosoUniversity 專案，然後按一下  **ASP.NET 組態**中**專案**功能表。</span><span class="sxs-lookup"><span data-stu-id="da36b-374">In **Solution Explorer**, select the ContosoUniversity project, and then click **ASP.NET Configuration** in the **Project** menu.</span></span>

<span data-ttu-id="da36b-375">選取 [安全性] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-375">Select the Security tab.</span></span>

<span data-ttu-id="da36b-376">按一下 **建立或管理角色**，然後建立**管理員**角色。</span><span class="sxs-lookup"><span data-stu-id="da36b-376">Click **Create or Manage Roles**, and then create an **Administrator** role.</span></span>

<span data-ttu-id="da36b-377">返回 [安全性] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="da36b-377">Return to the Security tab.</span></span>

<span data-ttu-id="da36b-378">按一下 **建立使用者**，然後選取**管理員**核取方塊，並建立名為管理員的使用者</span><span class="sxs-lookup"><span data-stu-id="da36b-378">Click **Create user**, and then select the **Administrator** check box and create a user named admin.</span></span>

<span data-ttu-id="da36b-379">關閉**Web Site Administration Tool**。</span><span class="sxs-lookup"><span data-stu-id="da36b-379">Close the **Web Site Administration Tool**.</span></span>

### <a name="creating-the-school-database"></a><span data-ttu-id="da36b-380">建立 School 資料庫</span><span class="sxs-lookup"><span data-stu-id="da36b-380">Creating the School Database</span></span>

<span data-ttu-id="da36b-381">開啟 [套件管理員主控台] 視窗。</span><span class="sxs-lookup"><span data-stu-id="da36b-381">Open the Package Manager Console window.</span></span>

<span data-ttu-id="da36b-382">在 **預設專案**下拉式清單中，選取 ContosoUniversity.DAL 專案。</span><span class="sxs-lookup"><span data-stu-id="da36b-382">In the **Default project** drop-down list, select the ContosoUniversity.DAL project.</span></span>

<span data-ttu-id="da36b-383">輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="da36b-383">Enter the following command:</span></span>

[!code-powershell[Main](deployment-to-a-hosting-provider-migrating-to-sql-server-10-of-12/samples/sample9.ps1)]

<span data-ttu-id="da36b-384">Code First 移轉適用於建立資料庫，然後套用 AddBirthDate 移轉時，在初始移轉，然後它會執行 Seed 方法。</span><span class="sxs-lookup"><span data-stu-id="da36b-384">Code First Migrations applies the Initial migration that creates the database and then applies the AddBirthDate migration, then it runs the Seed method.</span></span>

<span data-ttu-id="da36b-385">按下 ctrl+f5 來執行站台。</span><span class="sxs-lookup"><span data-stu-id="da36b-385">Run the site by pressing Control-F5.</span></span> <span data-ttu-id="da36b-386">當您對測試與生產環境時，執行**新增的學生**頁面上，加入新的學生，然後檢視 在新的學生**學生**頁面。</span><span class="sxs-lookup"><span data-stu-id="da36b-386">As you did for the test and production environments, run the **Add Students** page, add a new student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="da36b-387">這會確認已建立 School 資料庫，並初始化，您具有讀取和寫入權限。</span><span class="sxs-lookup"><span data-stu-id="da36b-387">This verifies that the School database was created and initialized and that you have read and write access to it.</span></span>

<span data-ttu-id="da36b-388">選取 **更新信用額度**頁面上，並確認已部署的成員資格資料庫，而且您有存取權限登入。</span><span class="sxs-lookup"><span data-stu-id="da36b-388">Select the **Update Credits** page and log in to verify that the membership database was deployed and that you have access to it.</span></span> <span data-ttu-id="da36b-389">如果您沒有不會移轉您的使用者帳戶，建立系統管理員帳戶，然後選取**更新信用額度**頁面，確認它是否有效。</span><span class="sxs-lookup"><span data-stu-id="da36b-389">If you did not migrate your user accounts, create an administrator account and then select the **Update Credits** page to verify that it works.</span></span>

## <a name="cleaning-up-sql-server-compact-files"></a><span data-ttu-id="da36b-390">清除 SQL Server Compact 檔案</span><span class="sxs-lookup"><span data-stu-id="da36b-390">Cleaning Up SQL Server Compact Files</span></span>

<span data-ttu-id="da36b-391">您不再需要的檔案和 NuGet 套件包含支援 SQL Server Compact。</span><span class="sxs-lookup"><span data-stu-id="da36b-391">You no longer need files and NuGet packages that were included to support SQL Server Compact.</span></span> <span data-ttu-id="da36b-392">如果您想要 （此步驟不需要），您可以清除不必要的檔案和參考。</span><span class="sxs-lookup"><span data-stu-id="da36b-392">If you want (this step is not required), you can clean up unneeded files and references.</span></span>

<span data-ttu-id="da36b-393">在 [**方案總管] 中**，刪除 *.sdf*從檔案*應用程式\_資料*資料夾和*amd64*和*x86*資料夾，從*bin*資料夾。</span><span class="sxs-lookup"><span data-stu-id="da36b-393">In **Solution Explorer**, delete the *.sdf* files from the *App\_Data* folder and the *amd64* and *x86* folders from the *bin* folder.</span></span>

<span data-ttu-id="da36b-394">在 **方案總管**解決方案 （不是其中一個專案），以滑鼠右鍵按一下，然後按一下 **管理方案的 NuGet 套件**。</span><span class="sxs-lookup"><span data-stu-id="da36b-394">In **Solution Explorer**, right-click the solution (not one of the projects), and then click **Manage NuGet Packages for Solution**.</span></span>

<span data-ttu-id="da36b-395">在左窗格中**管理 NuGet 套件**對話方塊中，選取**已安裝的套件**。</span><span class="sxs-lookup"><span data-stu-id="da36b-395">In the left pane of the **Manage NuGet Packages** dialog box, select **Installed packages**.</span></span>

<span data-ttu-id="da36b-396">選取  **EntityFramework.SqlServerCompact**封裝，然後按一下**管理**。</span><span class="sxs-lookup"><span data-stu-id="da36b-396">Select the **EntityFramework.SqlServerCompact** package and click **Manage**.</span></span>

<span data-ttu-id="da36b-397">在 [**選取專案**] 對話方塊中，選取這兩個專案。</span><span class="sxs-lookup"><span data-stu-id="da36b-397">In the **Select Projects** dialog box, both projects are selected.</span></span> <span data-ttu-id="da36b-398">若要解除安裝這兩個專案中的封裝，請清除這兩個核取方塊，然後按一下 **確定**。</span><span class="sxs-lookup"><span data-stu-id="da36b-398">To uninstall the package in both projects, clear both check boxes, then click **OK**.</span></span>

<span data-ttu-id="da36b-399">在詢問您是否也解除安裝相依封裝對話方塊中，按一下 [否]。</span><span class="sxs-lookup"><span data-stu-id="da36b-399">In the dialog box that asks if you want to uninstall the dependent packages also, click No.</span></span> <span data-ttu-id="da36b-400">其中之一是，您必須讓 Entity Framework 套件。</span><span class="sxs-lookup"><span data-stu-id="da36b-400">One of these is the Entity Framework package that you have to keep.</span></span>

<span data-ttu-id="da36b-401">遵循相同的程序來解除安裝**SqlServerCompact**封裝。</span><span class="sxs-lookup"><span data-stu-id="da36b-401">Follow the same procedure to uninstall the **SqlServerCompact** package.</span></span> <span data-ttu-id="da36b-402">(封裝必須解除安裝，依此順序因為**EntityFramework.SqlServerCompact**套件需倚賴**SqlServerCompact**封裝。)</span><span class="sxs-lookup"><span data-stu-id="da36b-402">(The packages must be uninstalled in this order because the **EntityFramework.SqlServerCompact** package depends on the **SqlServerCompact** package.)</span></span>

<span data-ttu-id="da36b-403">您現在已成功移轉至 SQL Server Express 和 SQL Server 完整版。</span><span class="sxs-lookup"><span data-stu-id="da36b-403">You have now successfully migrated to SQL Server Express and full SQL Server.</span></span> <span data-ttu-id="da36b-404">在下一個教學課程中您要進行另一個資料庫變更，而且您將了解如何部署資料庫變更，當您測試和生產環境的資料庫使用 SQL Server Express 和 SQL Server 完整版。</span><span class="sxs-lookup"><span data-stu-id="da36b-404">In the next tutorial you'll make another database change, and you'll see how to deploy database changes when your test and production databases use SQL Server Express and full SQL Server.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="da36b-405">[上一頁](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
> [下一頁](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="da36b-405">[Previous](deployment-to-a-hosting-provider-deploying-a-database-update-9-of-12.md)
[Next](deployment-to-a-hosting-provider-deploying-a-sql-server-database-update-11-of-12.md)</span></span>
