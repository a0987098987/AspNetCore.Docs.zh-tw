---
uid: web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
title: "使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 做為測試環境-12 個 5 |Microsoft 文件"
author: tdykstra
description: "這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含 SQL Server Compact 資料庫使用視覺化 Stu..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 11/17/2011
ms.topic: article
ms.assetid: 493b2a66-816c-485c-8315-952ed1085ccc
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/older-versions-getting-started/deployment-to-a-hosting-provider/deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12
msc.type: authoredcontent
ms.openlocfilehash: a7995844ee6ed19efa130c4f6c019214d6652ea7
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="deploying-an-aspnet-web-application-with-sql-server-compact-using-visual-studio-or-visual-web-developer-deploying-to-iis-as-a-test-environment---5-of-12"></a><span data-ttu-id="d763d-103">使用 SQL Server Compact 使用 Visual Studio 或 Visual Web Developer 將 ASP.NET Web 應用程式部署： 部署至 IIS 做為測試環境-5 的 12</span><span class="sxs-lookup"><span data-stu-id="d763d-103">Deploying an ASP.NET Web Application with SQL Server Compact using Visual Studio or Visual Web Developer: Deploying to IIS as a Test Environment - 5 of 12</span></span>
====================
<span data-ttu-id="d763d-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="d763d-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="d763d-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="d763d-105">Download Starter Project</span></span>](http://code.msdn.microsoft.com/Deploying-an-ASPNET-Web-4e31366b)

> <span data-ttu-id="d763d-106">這一系列的教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式專案，其中包含使用 Visual Studio 2012 RC 或 Visual Studio Express 2012 RC for Web 的 SQL Server Compact 資料庫。</span><span class="sxs-lookup"><span data-stu-id="d763d-106">This series of tutorials shows you how to deploy (publish) an ASP.NET web application project that includes a SQL Server Compact database by using Visual Studio 2012 RC or Visual Studio Express 2012 RC for Web.</span></span> <span data-ttu-id="d763d-107">如果您安裝 Web 發行更新，您也可以使用 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="d763d-107">You can also use Visual Studio 2010 if you install the Web Publish Update.</span></span> <span data-ttu-id="d763d-108">數列的簡介，請參閱[系列的第一個教學課程](deployment-to-a-hosting-provider-introduction-1-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="d763d-108">For an introduction to the series, see [the first tutorial in the series](deployment-to-a-hosting-provider-introduction-1-of-12.md).</span></span>
> 
> <span data-ttu-id="d763d-109">顯示部署 Visual Studio 2012 RC 發行之後，引進的功能，示範如何將 SQL Server Compact 以外的 SQL Server 版本的部署和示範如何將部署至 Azure App Service Web 應用程式的教學課程，請參閱[ASP.NET Web 部署使用 Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="d763d-109">For a tutorial that shows deployment features introduced after the RC release of Visual Studio 2012, shows how to deploy SQL Server editions other than SQL Server Compact, and shows how to deploy to Azure App Service Web Apps, see [ASP.NET Web Deployment using Visual Studio](../../deployment/visual-studio-web-deployment/introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="d763d-110">總覽</span><span class="sxs-lookup"><span data-stu-id="d763d-110">Overview</span></span>

<span data-ttu-id="d763d-111">本教學課程會示範如何在 ASP.NET web 應用程式部署至 IIS 在本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="d763d-111">This tutorial shows how to deploy an ASP.NET web application to IIS on the local computer.</span></span>

<span data-ttu-id="d763d-112">當您開發應用程式時，您通常會測試執行 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="d763d-112">When you develop an application, you generally test by running it in Visual Studio.</span></span> <span data-ttu-id="d763d-113">根據預設，這表示您使用 Visual Studio 程式開發伺服器 (也稱為 Cassini)。</span><span class="sxs-lookup"><span data-stu-id="d763d-113">By default, this means you're using the Visual Studio Development Server (also known as Cassini).</span></span> <span data-ttu-id="d763d-114">Visual Studio 程式開發伺服器可讓您輕鬆地在 Visual Studio 中，在開發期間進行測試，但它不能與 IIS 完全相同。</span><span class="sxs-lookup"><span data-stu-id="d763d-114">The Visual Studio Development Server makes it easy to test during development in Visual Studio, but it doesn't work exactly like IIS.</span></span> <span data-ttu-id="d763d-115">如此一來，很可能您測試在 Visual Studio 中，但失敗時的裝載環境中將它部署到 IIS 應用程式將會正確執行。</span><span class="sxs-lookup"><span data-stu-id="d763d-115">As a result, it's possible that an application will run correctly when you test it in Visual Studio, but fail when it's deployed to IIS in a hosting environment.</span></span>

<span data-ttu-id="d763d-116">您可以更可靠地測試您的應用程式，以下列方式：</span><span class="sxs-lookup"><span data-stu-id="d763d-116">You can test your application more reliably in these ways:</span></span>

1. <span data-ttu-id="d763d-117">使用 IIS Express 或完整的 IIS Visual Studio 程式開發伺服器時，代替您測試在 Visual Studio 在開發期間。</span><span class="sxs-lookup"><span data-stu-id="d763d-117">Use IIS Express or full IIS instead of the Visual Studio Development Server when you test in Visual Studio during development.</span></span> <span data-ttu-id="d763d-118">這個方法通常會模擬更精確地您的網站在 IIS 底下的執行方式。</span><span class="sxs-lookup"><span data-stu-id="d763d-118">This method generally emulates more accurately how your site will run under IIS.</span></span> <span data-ttu-id="d763d-119">不過，這個方法不會測試您的部署程序或驗證的部署程序的結果將會正確執行。</span><span class="sxs-lookup"><span data-stu-id="d763d-119">However, this method does not test your deployment process or validate that the result of the deployment process will run correctly.</span></span>
2. <span data-ttu-id="d763d-120">使用相同的程序，您稍後會使用將其部署到生產環境部署到 IIS 應用程式，在開發電腦上。</span><span class="sxs-lookup"><span data-stu-id="d763d-120">Deploy the application to IIS on your development computer by using the same process that you'll use later to deploy it to your production environment.</span></span> <span data-ttu-id="d763d-121">這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。</span><span class="sxs-lookup"><span data-stu-id="d763d-121">This method validates your deployment process in addition to validating that your application will run correctly under IIS.</span></span>
3. <span data-ttu-id="d763d-122">部署應用程式是盡可能接近實際執行環境的測試環境。</span><span class="sxs-lookup"><span data-stu-id="d763d-122">Deploy the application to a test environment that is as close as possible to your production environment.</span></span> <span data-ttu-id="d763d-123">由於這些教學課程的生產環境是協力廠商主機服務提供者，理想的測試環境會是具有主機服務提供者的第二個帳戶。</span><span class="sxs-lookup"><span data-stu-id="d763d-123">Since the production environment for these tutorials is a third-party hosting provider, the ideal test environment would be a second account with the hosting provider.</span></span> <span data-ttu-id="d763d-124">您會使用這個第二個帳戶僅供測試，但它會做為實際帳戶的相同方式設定。</span><span class="sxs-lookup"><span data-stu-id="d763d-124">You would use this second account only for testing, but it would be set up the same way as the production account.</span></span>

<span data-ttu-id="d763d-125">本教學課程示範選項 2 的步驟。</span><span class="sxs-lookup"><span data-stu-id="d763d-125">This tutorial shows the steps for option 2.</span></span> <span data-ttu-id="d763d-126">在結尾處提供的選項 3 的指導方針[部署到生產環境](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md)教學課程中，而且在本教學課程結尾處有資源的選項 1 的連結。</span><span class="sxs-lookup"><span data-stu-id="d763d-126">Guidance for option 3 is provided at the end of the [Deploying to the Production Environment](deployment-to-a-hosting-provider-deploying-to-the-production-environment-7-of-12.md) tutorial, and at the end of this tutorial there are links to resources for option 1.</span></span>

<span data-ttu-id="d763d-127">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md)。</span><span class="sxs-lookup"><span data-stu-id="d763d-127">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](deployment-to-a-hosting-provider-creating-and-installing-deployment-packages-12-of-12.md).</span></span>

## <a name="configuring-the-application-to-run-in-medium-trust"></a><span data-ttu-id="d763d-128">在中度信任中設定要執行的應用程式</span><span class="sxs-lookup"><span data-stu-id="d763d-128">Configuring the Application to Run in Medium Trust</span></span>

<span data-ttu-id="d763d-129">安裝 IIS 和之前部署至它，您要使站台執行更像在一般的共用裝載環境中會變更 Web.config 檔案設定。</span><span class="sxs-lookup"><span data-stu-id="d763d-129">Before installing IIS and deploying to it, you'll change a Web.config file setting in order to make the site run more like it will in a typical shared hosting environment.</span></span>

<span data-ttu-id="d763d-130">主機服務提供者通常執行您的網站*中度信任*，這表示不允許執行幾個步驟。</span><span class="sxs-lookup"><span data-stu-id="d763d-130">Hosting providers typically run your web site in *medium trust*, which means there are some things it is not allowed to do.</span></span> <span data-ttu-id="d763d-131">例如，應用程式程式碼無法存取 Windows 登錄，無法讀取或寫入應用程式的資料夾階層之外的檔案。</span><span class="sxs-lookup"><span data-stu-id="d763d-131">For example, application code can't access the Windows registry and can't read or write files that are outside of your application's folder hierarchy.</span></span> <span data-ttu-id="d763d-132">根據預設應用程式執行的*高信任度*您本機電腦上，這表示應用程式可能可以執行一些作業，當您部署到生產環境時將會失敗。</span><span class="sxs-lookup"><span data-stu-id="d763d-132">By default your application runs in *high trust* on your local computer, which means that the application might be able to do things that would fail when you deploy it to production.</span></span> <span data-ttu-id="d763d-133">因此，若要讓測試環境更準確地反映實際執行環境，您會設定要在中度信任中執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d763d-133">Therefore, to make the test environment more accurately reflect the production environment, you'll configure the application to run in medium trust.</span></span>

<span data-ttu-id="d763d-134">在應用程式 Web.config 檔案中，加入**信任**中的項目**system.web**項目，如這個範例所示。</span><span class="sxs-lookup"><span data-stu-id="d763d-134">In the application Web.config file, add a **trust** element in the **system.web** element, as shown in this example.</span></span>

[!code-xml[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample1.xml?highlight=4)]

<span data-ttu-id="d763d-135">在 IIS 中的中度信任現在將會執行應用程式即使在本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="d763d-135">The application will now run in medium trust in IIS even on your local computer.</span></span> <span data-ttu-id="d763d-136">這項設定可讓您在生產環境中將會失敗的應用程式程式碼以做到盡早攔截的任何嘗試行為。</span><span class="sxs-lookup"><span data-stu-id="d763d-136">This setting enables you to catch as early as possible any attempts by application code to do something that would fail in production.</span></span>

> [!NOTE]
> <span data-ttu-id="d763d-137">如果您使用 Entity Framework Code First 移轉，請確定您有 5.0 版或更新的版本。</span><span class="sxs-lookup"><span data-stu-id="d763d-137">If you are using Entity Framework Code First Migrations, make sure that you have version 5.0 or later installed.</span></span> <span data-ttu-id="d763d-138">在 Entity Framework 4.3 版中，移轉需要完全信任，才能更新資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="d763d-138">In Entity Framework version 4.3, Migrations requires full trust in order to update the database schema.</span></span>


## <a name="installing-iis-and-web-deploy"></a><span data-ttu-id="d763d-139">安裝 IIS 和 Web Deploy</span><span class="sxs-lookup"><span data-stu-id="d763d-139">Installing IIS and Web Deploy</span></span>

<span data-ttu-id="d763d-140">若要在開發電腦部署到 IIS，您必須擁有 IIS 和 Web Deploy 安裝。</span><span class="sxs-lookup"><span data-stu-id="d763d-140">To deploy to IIS on your development computer, you must have IIS and Web Deploy installed.</span></span> <span data-ttu-id="d763d-141">預設的 Windows 7 組態不包含這些。</span><span class="sxs-lookup"><span data-stu-id="d763d-141">These are not included in the default Windows 7 configuration.</span></span> <span data-ttu-id="d763d-142">如果您已安裝 IIS 和 Web Deploy，請跳至下一節。</span><span class="sxs-lookup"><span data-stu-id="d763d-142">If you have already installed both IIS and Web Deploy, skip to the next section.</span></span>

<span data-ttu-id="d763d-143">使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，因為 Web Platform Installer 安裝 iis 建議的組態，而且它會自動安裝 IIS 和 Web 的必要條件如果需要，部署。</span><span class="sxs-lookup"><span data-stu-id="d763d-143">Using the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) is the preferred way to install IIS and Web Deploy, because the Web Platform Installer installs a recommended configuration for IIS and it automatically installs the prerequisites for IIS and Web Deploy if necessary.</span></span>

<span data-ttu-id="d763d-144">若要執行 Web Platform Installer 安裝 IIS 和 Web Deploy，請使用下列連結。</span><span class="sxs-lookup"><span data-stu-id="d763d-144">To run Web Platform Installer to install IIS and Web Deploy, use the following link.</span></span> <span data-ttu-id="d763d-145">如果您已安裝 IIS、 Web Deploy 或任何必要的元件，會安裝 Web Platform Installer，只會遺失。</span><span class="sxs-lookup"><span data-stu-id="d763d-145">If you already have installed IIS, Web Deploy or any of their required components, the Web Platform Installer installs only what is missing.</span></span>

- [<span data-ttu-id="d763d-146">安裝 IIS 和 Web Deploy 使用 WebPI，</span><span class="sxs-lookup"><span data-stu-id="d763d-146">Install IIS and Web Deploy using WebPI</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

## <a name="setting-the-default-application-pool-to-net-4"></a><span data-ttu-id="d763d-147">將預設應用程式集區設定為.NET 4</span><span class="sxs-lookup"><span data-stu-id="d763d-147">Setting the Default Application Pool to .NET 4</span></span>

<span data-ttu-id="d763d-148">安裝 IIS 之後, 執行**IIS 管理員**，確定.NET Framework 第 4 版預設應用程式集區指派。</span><span class="sxs-lookup"><span data-stu-id="d763d-148">After installing IIS, run **IIS Manager** to make sure that the .NET Framework version 4 is assigned to the default application pool.</span></span>

<span data-ttu-id="d763d-149">從 Windows**啟動**功能表上，選取**執行**，輸入 「 inetmgr"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="d763d-149">From the Windows **Start** menu, select **Run**, enter "inetmgr", and then click **OK**.</span></span> <span data-ttu-id="d763d-150">(如果**執行**命令不會在您**啟動**功能表上，您可以按 Windows 鍵和 R，以開啟它。</span><span class="sxs-lookup"><span data-stu-id="d763d-150">(If the **Run** command is not in your **Start** menu, you can press the Windows Key and R to open it.</span></span> <span data-ttu-id="d763d-151">或以滑鼠右鍵按一下工作列，請按一下**屬性**，選取**開始功能表**索引標籤上，按一下 **自訂**，然後選取**執行命令**。)</span><span class="sxs-lookup"><span data-stu-id="d763d-151">Or right-click the taskbar, click **Properties**, select the **Start Menu** tab, click **Customize**, and select **Run command**.)</span></span>

<span data-ttu-id="d763d-152">在**連線** 窗格中，展開伺服器節點，然後選取**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="d763d-152">In the **Connections** pane, expand the server node and select **Application Pools**.</span></span> <span data-ttu-id="d763d-153">在**應用程式集區** 窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版圖所示，請跳至下一節。</span><span class="sxs-lookup"><span data-stu-id="d763d-153">In the **Application Pools** pane, if **DefaultAppPool** is assigned to the .NET framework version 4 as in the following illustration, skip to the next section.</span></span>

<span data-ttu-id="d763d-154">[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-154">[![Inetmgr_showing_4.0_app_pools](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image2.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image1.png)</span></span>

<span data-ttu-id="d763d-155">如果您看到只有兩個應用程式集區，而這兩種都設定為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4:</span><span class="sxs-lookup"><span data-stu-id="d763d-155">If you see only two application pools and both of them are set to the .NET Framework 2.0, you have to install ASP.NET 4 in IIS:</span></span>

- <span data-ttu-id="d763d-156">開啟命令提示字元視窗，以滑鼠右鍵按一下**命令提示字元**windows**啟動**功能表，然後選取**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="d763d-156">Open a command prompt window by right-clicking **Command Prompt** in the Windows **Start** menu and selecting **Run as Administrator**.</span></span> <span data-ttu-id="d763d-157">然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="d763d-157">Then run [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) to install ASP.NET 4 in IIS, using the following commands.</span></span> <span data-ttu-id="d763d-158">（在 64 位元系統中，取代"架構"與"Framework64"）。</span><span class="sxs-lookup"><span data-stu-id="d763d-158">(In 64-bit systems, replace "Framework" with "Framework64".)</span></span>

    [!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample2.cmd)]

    <span data-ttu-id="d763d-159">[![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-159">[![aspnet_regiis_installing_ASP.NET_4](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image4.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image3.png)</span></span>

    <span data-ttu-id="d763d-160">此命令會建立新的應用程式集區的.NET Framework 4，但預設應用程式集區將仍會設定為 2.0。</span><span class="sxs-lookup"><span data-stu-id="d763d-160">This command creates new application pools for the .NET Framework 4, but the default application pool will still be set to 2.0.</span></span> <span data-ttu-id="d763d-161">您將部署應用程式目標為.NET 4 至該應用程式集區，因此您需要將應用程式集區變更為.NET 4。</span><span class="sxs-lookup"><span data-stu-id="d763d-161">You'll be deploying an application that targets .NET 4 to that application pool, so you have to change the application pool to .NET 4.</span></span>

<span data-ttu-id="d763d-162">如果您關閉**IIS 管理員**、 執行一次、 展開伺服器節點，然後按一下**應用程式集區**顯示**應用程式集區** 窗格。</span><span class="sxs-lookup"><span data-stu-id="d763d-162">If you closed **IIS Manager**, run it again, expand the server node, and click **Application Pools** to display the **Application Pools** pane again.</span></span>

<span data-ttu-id="d763d-163">在**應用程式集區**] 窗格中，按一下 [ **DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="d763d-163">In the **Application Pools** pane, click **DefaultAppPool**, and then in the **Actions** pane click **Basic Settings**.</span></span>

<span data-ttu-id="d763d-164">[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-164">[![Inetmgr_selecting_Basic_Settings_for_app_pool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image6.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image5.png)</span></span>

<span data-ttu-id="d763d-165">在**編輯應用程式集區**對話方塊中，變更**.NET Framework 版本**至**.NET Framework v4.0.30319**按一下**[確定]**。</span><span class="sxs-lookup"><span data-stu-id="d763d-165">In the **Edit Application Pool** dialog box, change **.NET Framework version** to **.NET Framework v4.0.30319** and click **OK**.</span></span>

<span data-ttu-id="d763d-166">[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-166">[![Selecting_.NET_4_for_DefaultAppPool](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image8.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image7.png)</span></span>

<span data-ttu-id="d763d-167">現在您已經準備好要發行至 IIS。</span><span class="sxs-lookup"><span data-stu-id="d763d-167">You are now ready to publish to IIS.</span></span>

## <a name="publishing-to-iis"></a><span data-ttu-id="d763d-168">發行至 IIS</span><span class="sxs-lookup"><span data-stu-id="d763d-168">Publishing to IIS</span></span>

<span data-ttu-id="d763d-169">有數種方式，您可以使用 Visual Studio 2010 和 Web Deploy 來部署：</span><span class="sxs-lookup"><span data-stu-id="d763d-169">There are several ways you can deploy using Visual Studio 2010 and Web Deploy:</span></span>

- <span data-ttu-id="d763d-170">使用單鍵發行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d763d-170">Use Visual Studio one-click publish.</span></span>
- <span data-ttu-id="d763d-171">建立*部署套件*及使用 IIS 管理員 UI 進行安裝。</span><span class="sxs-lookup"><span data-stu-id="d763d-171">Create a *deployment package* and install it using the IIS Manager UI.</span></span> <span data-ttu-id="d763d-172">部署套件組成*.zip*檔案，其中包含所有檔案和在 IIS 中安裝站台所需的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="d763d-172">The deployment package consists of a *.zip* file that contains all the files and metadata needed to install a site in IIS.</span></span>
- <span data-ttu-id="d763d-173">建立部署套件，並使用命令列進行安裝。</span><span class="sxs-lookup"><span data-stu-id="d763d-173">Create a deployment package and install it using the command line.</span></span>

<span data-ttu-id="d763d-174">您已在上一個教學課程中設定 Visual Studio，來自動化部署工作套用到所有這三個方法的程序。</span><span class="sxs-lookup"><span data-stu-id="d763d-174">The process you went through in the previous tutorials to set up Visual Studio to automate deployment tasks applies to all of these three methods.</span></span> <span data-ttu-id="d763d-175">在這些教學課程中，您將使用這些方法的第一個。</span><span class="sxs-lookup"><span data-stu-id="d763d-175">In these tutorials you'll use the first of these methods.</span></span> <span data-ttu-id="d763d-176">使用部署套件的相關資訊，請參閱[ASP.NET 部署內容地圖](https://msdn.microsoft.com/library/bb386521.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d763d-176">For information about using deployment packages, see [ASP.NET Deployment Content Map](https://msdn.microsoft.com/library/bb386521.aspx).</span></span>

<span data-ttu-id="d763d-177">在發行之前，請確定您系統管理員模式中執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="d763d-177">Before publishing, make sure that you are running Visual Studio in administrator mode.</span></span> <span data-ttu-id="d763d-178">(在 Windows 7**啟動**功能表上，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。)需要發行只有當您發行至 IIS 在本機電腦上的系統管理員模式。</span><span class="sxs-lookup"><span data-stu-id="d763d-178">(In the Windows 7 **Start** menu, right-click the icon for the version of Visual Studio you're using and select **Run as Administrator**.) Administrator mode is required for publishing only when you are publishing to IIS on the local computer.</span></span>

<span data-ttu-id="d763d-179">在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="d763d-179">In **Solution Explorer**, right-click the ContosoUniversity project (not the ContosoUniversity.DAL project) and select **Publish**.</span></span>

<span data-ttu-id="d763d-180">**發行 Web**精靈 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="d763d-180">The **Publish Web** wizard appears.</span></span>

![Publish_Web_wizard_Profile_tab](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image9.png)

<span data-ttu-id="d763d-182">在下拉式清單中，選取**&lt;新增...&gt;**.</span><span class="sxs-lookup"><span data-stu-id="d763d-182">In the drop-down list, select **&lt;New...&gt;**.</span></span>

<span data-ttu-id="d763d-183">在**新設定檔**對話方塊中，輸入 「 測試 」，，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="d763d-183">In the **New Profile** dialog box, enter "Test", and then click **OK**.</span></span>

![New_Profile_dialog_box](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image10.png)

<span data-ttu-id="d763d-185">這個名稱是 Web.Test.config 中間節點相同轉換您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="d763d-185">This name is the same as the middle node of the Web.Test.config transform file that you created earlier.</span></span> <span data-ttu-id="d763d-186">此對應是什麼情況會讓您使用此設定檔來發佈時要套用的 Web.Test.config 轉換。</span><span class="sxs-lookup"><span data-stu-id="d763d-186">This correspondence is what causes the Web.Test.config transformations to be applied when you publish by using this profile.</span></span>

<span data-ttu-id="d763d-187">精靈會自動移到**連接** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d763d-187">The wizard automatically advances to the **Connection** tab.</span></span>

<span data-ttu-id="d763d-188">在**服務 URL**方塊中，輸入*localhost*。</span><span class="sxs-lookup"><span data-stu-id="d763d-188">In the **Service URL** box, enter *localhost*.</span></span>

<span data-ttu-id="d763d-189">在**網站/應用程式**方塊中，輸入*預設網站/ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="d763d-189">In the **Site/application** box, enter *Default Web Site/ContosoUniversity*.</span></span>

<span data-ttu-id="d763d-190">在**目的地 URL**方塊中，輸入`http://localhost/ContosoUniversity`。</span><span class="sxs-lookup"><span data-stu-id="d763d-190">In the **Destination URL** box, enter `http://localhost/ContosoUniversity`.</span></span>

<span data-ttu-id="d763d-191">**目的地 URL**設定不是必要的。</span><span class="sxs-lookup"><span data-stu-id="d763d-191">The **Destination URL** setting isn't required.</span></span> <span data-ttu-id="d763d-192">當 Visual Studio 完成部署應用程式時，它會自動開啟預設的瀏覽器對這個 URL。</span><span class="sxs-lookup"><span data-stu-id="d763d-192">When Visual Studio finishes deploying the application, it automatically opens your default browser to this URL.</span></span> <span data-ttu-id="d763d-193">如果您不想要在部署後會自動開啟瀏覽器，將此方塊保留空白。</span><span class="sxs-lookup"><span data-stu-id="d763d-193">If you don't want the browser to open automatically after deployment, leave this box blank.</span></span>

![Publish_Web_wizard_Connection_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image11.png)

<span data-ttu-id="d763d-195">按一下**驗證連線**確認設定正確無誤，而且您可以在本機電腦上連接到 IIS。</span><span class="sxs-lookup"><span data-stu-id="d763d-195">Click **Validate Connection** to verify that the settings are correct and you can connect to IIS on the local computer.</span></span>

<span data-ttu-id="d763d-196">綠色的核取記號會驗證是成功的連線。</span><span class="sxs-lookup"><span data-stu-id="d763d-196">A green check mark verifies that the connection is successful.</span></span>

![Publish_Web_wizard_Connection_tab_validated](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image12.png)

<span data-ttu-id="d763d-198">按一下**下一步**前進到**設定** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d763d-198">Click **Next** to advance to the **Settings** tab.</span></span>

<span data-ttu-id="d763d-199">**組態**下拉式清單方塊指定要部署的組建組態。</span><span class="sxs-lookup"><span data-stu-id="d763d-199">The **Configuration** drop-down box specifies the build configuration to deploy.</span></span> <span data-ttu-id="d763d-200">預設值是版本中，這是您想要。</span><span class="sxs-lookup"><span data-stu-id="d763d-200">The default value is Release, which is what you want.</span></span>

<span data-ttu-id="d763d-201">保留**移除目的端的其他檔案**清除核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d763d-201">Leave the **Remove additional files at destination** check box cleared.</span></span> <span data-ttu-id="d763d-202">由於這是您第一次部署時，將不會有任何檔案的目的地資料夾中尚未。</span><span class="sxs-lookup"><span data-stu-id="d763d-202">Since this is your first deployment, there won't be any files in the destination folder yet.</span></span>

<span data-ttu-id="d763d-203">在**資料庫**區段中，輸入下列值中的連接字串方塊**SchoolContext**:</span><span class="sxs-lookup"><span data-stu-id="d763d-203">In the **Databases** section, enter the following value in the connection string box for **SchoolContext**:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample3.cmd)]

<span data-ttu-id="d763d-204">部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。</span><span class="sxs-lookup"><span data-stu-id="d763d-204">The deployment process will put this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

<span data-ttu-id="d763d-205">也在**SchoolContext**，選取**套用 Code First 移轉**。</span><span class="sxs-lookup"><span data-stu-id="d763d-205">Also under **SchoolContext**, select **Apply Code First Migrations**.</span></span> <span data-ttu-id="d763d-206">這個選項會讓部署程序來設定已部署的 Web.config 檔案來指定`MigrateDatabaseToLatestVersion`初始設定式。</span><span class="sxs-lookup"><span data-stu-id="d763d-206">This option causes the deployment process to configure the deployed Web.config file to specify the `MigrateDatabaseToLatestVersion` initializer.</span></span> <span data-ttu-id="d763d-207">應用程式部署後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="d763d-207">This initializer automatically updates the database to the latest version when the application accesses the database for the first time after deployment.</span></span>

<span data-ttu-id="d763d-208">在連接字串 方塊的**DefaultConnection**，輸入下列值：</span><span class="sxs-lookup"><span data-stu-id="d763d-208">In the connection string box for **DefaultConnection**, enter the following value:</span></span>

[!code-console[Main](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/samples/sample4.cmd)]

<span data-ttu-id="d763d-209">保留**更新資料庫**清除。</span><span class="sxs-lookup"><span data-stu-id="d763d-209">Leave **Update database** cleared.</span></span> <span data-ttu-id="d763d-210">將部署成員資格資料庫，藉由複製應用程式中的.sdf 檔案\_資料，且您不想執行任何動作與這個資料庫的部署程序。</span><span class="sxs-lookup"><span data-stu-id="d763d-210">The membership database will be deployed by copying the .sdf file in App\_Data, and you don't want the deployment process to do anything else with this database.</span></span>

![Publish_Web_wizard_Settings_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image13.png)

<span data-ttu-id="d763d-212">按一下**下一步**前進到**預覽** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="d763d-212">Click **Next** to advance to the **Preview** tab.</span></span>

<span data-ttu-id="d763d-213">在**預覽**索引標籤上，按一下 **啟動預覽**若要查看將複製的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="d763d-213">In the **Preview** tab, click **Start Preview** to see a list of the files that will be copied.</span></span>

![Publish_Web_wizard_Preview_tab_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image14.png)

![Publish_Web_wizard_Preview_tab_Test_with_file_list](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image15.png)

<span data-ttu-id="d763d-216">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="d763d-216">Click **Publish**.</span></span>

<span data-ttu-id="d763d-217">如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。</span><span class="sxs-lookup"><span data-stu-id="d763d-217">If Visual Studio is not in administrator mode, you might get an error message that indicates a permissions error.</span></span> <span data-ttu-id="d763d-218">在此情況下，關閉 Visual Studio，以系統管理員模式開啟，並再次嘗試發行。</span><span class="sxs-lookup"><span data-stu-id="d763d-218">In that case, close Visual Studio, open it in administrator mode, and try to publish again.</span></span>

<span data-ttu-id="d763d-219">如果 Visual Studio 是以系統管理員模式**輸出**視窗中報告成功建置並發行。</span><span class="sxs-lookup"><span data-stu-id="d763d-219">If Visual Studio is in administrator mode, the **Output** window reports successful build and publish.</span></span>

![Output_window_publish_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image16.png)

<span data-ttu-id="d763d-221">瀏覽器會自動開啟 Contoso 大學首頁，即可在本機電腦上執行於 IIS。</span><span class="sxs-lookup"><span data-stu-id="d763d-221">The browser automatically opens to the Contoso University Home page running in IIS on the local computer.</span></span>

<span data-ttu-id="d763d-222">[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-222">[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image18.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image17.png)</span></span>

## <a name="testing-in-the-test-environment"></a><span data-ttu-id="d763d-223">在測試環境中測試</span><span class="sxs-lookup"><span data-stu-id="d763d-223">Testing in the Test Environment</span></span>

<span data-ttu-id="d763d-224">請注意，環境標記會顯示 「 （測試） 」 而不是 「 （開發） 」，其中顯示*Web.config*環境指標的轉換是否成功。</span><span class="sxs-lookup"><span data-stu-id="d763d-224">Notice that the environment indicator shows "(Test)" instead of "(Dev)", which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

<span data-ttu-id="d763d-225">[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-225">[![Home_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image20.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image19.png)</span></span>

<span data-ttu-id="d763d-226">執行**學生**頁面，確認已部署的資料庫已經沒有學生。</span><span class="sxs-lookup"><span data-stu-id="d763d-226">Run the **Students** page to verify that the deployed database has no students.</span></span> <span data-ttu-id="d763d-227">當您選取此頁面時可能需要幾分鐘才能載入，因為第一個程式碼會建立資料庫，然後執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="d763d-227">When you select this page it may take a few minutes to load because Code First creates the database and then runs the `Seed` method.</span></span> <span data-ttu-id="d763d-228">（它未執行作業，當應用程式沒有存取資料庫尚未嘗試過，因為已在首頁上時）。</span><span class="sxs-lookup"><span data-stu-id="d763d-228">(It didn't do that when you were on the home page because the application didn't try to access the database yet.)</span></span>

<span data-ttu-id="d763d-229">[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-229">[![Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image22.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image21.png)</span></span>

<span data-ttu-id="d763d-230">執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫：</span><span class="sxs-lookup"><span data-stu-id="d763d-230">Run the **Instructors** page to verify that Code First seeded the database with instructor data:</span></span>

<span data-ttu-id="d763d-231">[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-231">[![Instructors_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image24.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image23.png)</span></span>

<span data-ttu-id="d763d-232">選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫:</span><span class="sxs-lookup"><span data-stu-id="d763d-232">Select **Add Students** from the **Students** menu, add a student, and then view the new student in the **Students** page to verify that you can successfully write to the database:</span></span>

<span data-ttu-id="d763d-233">[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-233">[![Add_Students_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image26.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image25.png)</span></span>

<span data-ttu-id="d763d-234">[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-234">[![Students_page_with_new_student_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image28.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image27.png)</span></span>

<span data-ttu-id="d763d-235">從**課程**功能表上，選取**更新信用額度**。</span><span class="sxs-lookup"><span data-stu-id="d763d-235">From the **Courses** menu, select **Update Credits**.</span></span> <span data-ttu-id="d763d-236">**更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="d763d-236">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="d763d-237">輸入您建立更早 （"admin"、"Pa$ w0rd"） 的系統管理員帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="d763d-237">Enter the administrator account credentials that you created earlier ("admin" and "Pas$w0rd").</span></span> <span data-ttu-id="d763d-238">**更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="d763d-238">The **Update Credits** page is displayed, which verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="d763d-239">[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-239">[![Log_In_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image30.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image29.png)</span></span>

<span data-ttu-id="d763d-240">[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-240">[![Update_Credits_page_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image32.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image31.png)</span></span>

<span data-ttu-id="d763d-241">確認*Elmah*資料夾有只在它的預留位置檔案。</span><span class="sxs-lookup"><span data-stu-id="d763d-241">Verify that an *Elmah* folder exists with only the placeholder file in it.</span></span>

<span data-ttu-id="d763d-242">[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)</span><span class="sxs-lookup"><span data-stu-id="d763d-242">[![Elmah_folder_Test](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image34.png)](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image33.png)</span></span>

<a id="efcfmigrations"></a>

## <a name="reviewing-the-automatic-webconfig-changes-for-code-first-migrations"></a><span data-ttu-id="d763d-243">檢閱自動 Web.config 變更的程式碼 Code First 移轉</span><span class="sxs-lookup"><span data-stu-id="d763d-243">Reviewing the Automatic Web.config Changes for Code First Migrations</span></span>

<span data-ttu-id="d763d-244">開啟*Web.config*中部署的應用程式在檔案*C:\inetpub\wwwroot\ContosoUniversity* ，您可以查看其中部署程序 Code First 移轉來自動設定更新資料庫的最新版本。</span><span class="sxs-lookup"><span data-stu-id="d763d-244">Open the *Web.config* file in the deployed application at *C:\inetpub\wwwroot\ContosoUniversity* and you can see where the deployment process configured Code First Migrations to automatically update the database to the latest version.</span></span>

![](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image35.png)

<span data-ttu-id="d763d-245">部署程序也會建立新的 Code First 移轉，專門用來更新資料庫結構描述的連接字串：</span><span class="sxs-lookup"><span data-stu-id="d763d-245">The deployment process also created a new connection string for Code First Migrations to use exclusively for updating the database schema:</span></span>

![DatabasePublish_connection_string](deployment-to-a-hosting-provider-deploying-to-iis-as-a-test-environment-5-of-12/_static/image36.png)

<span data-ttu-id="d763d-247">這個額外的連接字串可讓您指定一個使用者帳戶的資料庫結構描述更新，以及不同的使用者帳戶存取應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="d763d-247">This additional connection string enables you to specify one user account for database schema updates, and a different user account for application data access.</span></span> <span data-ttu-id="d763d-248">例如，您可能會指派 db\_Code First 移轉，和資料庫的擁有者角色\_datareader 和 db\_datawriter 應用程式的角色。</span><span class="sxs-lookup"><span data-stu-id="d763d-248">For example, you could assign the db\_owner role to Code First Migrations, and db\_datareader and db\_datawriter roles to the application.</span></span> <span data-ttu-id="d763d-249">這是常見的深度防禦模式會防止應用程式無法變更資料庫結構描述中的潛在惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="d763d-249">This is a common defense-in-depth pattern that prevents potentially malicious code in the application from changing the database schema.</span></span> <span data-ttu-id="d763d-250">（例如，這可能會發生在成功的 SQL 資料隱碼攻擊。）此模式不會使用這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="d763d-250">(For example, this might happen in a successful SQL injection attack.) This pattern is not used by these tutorials.</span></span> <span data-ttu-id="d763d-251">它不適用於 SQL Server Compact，並不會套用在稍後的教學課程中，在這一系列將移轉至 SQL Server 時。</span><span class="sxs-lookup"><span data-stu-id="d763d-251">It does not apply to SQL Server Compact, and it does not apply when you migrate to SQL Server in a later tutorial in this series.</span></span> <span data-ttu-id="d763d-252">Cytanium 網站提供一個使用者帳戶來存取您在 Cytanium 建立 SQL Server 資料庫。</span><span class="sxs-lookup"><span data-stu-id="d763d-252">The Cytanium site offers just one user account for accessing the SQL Server database that you create at Cytanium.</span></span> <span data-ttu-id="d763d-253">如果您能夠在您的案例中實作此模式，您可以執行下列步驟來執行它：</span><span class="sxs-lookup"><span data-stu-id="d763d-253">If you are able to implement this pattern in your scenario, you can do it by performing the following steps:</span></span>

1. <span data-ttu-id="d763d-254">在**設定** 索引標籤**發行 Web**精靈 中，輸入完整的資料庫結構描述更新權限與指定的使用者的連接字串，並清除**使用此連接字串在執行階段**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="d763d-254">In the **Settings** tab of the **Publish Web** wizard, enter the connection string that specifies a user with full database schema update permissions, and clear the **Use this connection string at runtime** check box.</span></span> <span data-ttu-id="d763d-255">在已部署 Web.config 檔案中，這會變成`DatabasePublish`連接字串。</span><span class="sxs-lookup"><span data-stu-id="d763d-255">In the deployed Web.config file, this becomes the `DatabasePublish` connection string.</span></span>
2. <span data-ttu-id="d763d-256">建立的連接字串，您想要在執行階段使用的應用程式的 Web.config 檔案轉換。</span><span class="sxs-lookup"><span data-stu-id="d763d-256">Create a Web.config file transformation for the connection string that you want the application to use at run time.</span></span>

<span data-ttu-id="d763d-257">您現在已在開發電腦部署到 IIS 應用程式，而且那里進行測試。</span><span class="sxs-lookup"><span data-stu-id="d763d-257">You have now deployed your application to IIS on your development computer and tested it there.</span></span> <span data-ttu-id="d763d-258">這會確認部署程序複製到正確的位置 （不含您不想要部署的檔案），應用程式的內容，也該 Web Deploy IIS 正確設定在部署期間。</span><span class="sxs-lookup"><span data-stu-id="d763d-258">This verifies that the deployment process copied the application's content to the right location (excluding the files that you did not want to deploy), and also that Web Deploy configured IIS correctly during deployment.</span></span> <span data-ttu-id="d763d-259">在下一個教學課程中，您將執行一項會尋找尚未完成的部署工作的多個測試： 設定資料夾的權限*Elmah*資料夾。</span><span class="sxs-lookup"><span data-stu-id="d763d-259">In the next tutorial, you'll run one more test that finds a deployment task that has not yet been done: setting folder permissions on the *Elmah* folder.</span></span>

## <a name="more-information"></a><span data-ttu-id="d763d-260">更多資訊</span><span class="sxs-lookup"><span data-stu-id="d763d-260">More Information</span></span>

<span data-ttu-id="d763d-261">如需 Visual Studio 中執行 IIS 或 IIS Express，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="d763d-261">For information about running IIS or IIS Express in Visual Studio, see the following resources:</span></span>

- <span data-ttu-id="d763d-262">[IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="d763d-262">[IIS Express Overview](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) on the IIS.net site.</span></span>
- <span data-ttu-id="d763d-263">[導入 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="d763d-263">[Introducing IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) on Scott Guthrie's blog.</span></span>
- <span data-ttu-id="d763d-264">[如何： 在 Visual Studio 中的 Web 專案指定的 Web 伺服器](https://msdn.microsoft.com/library/ms178108.aspx)。</span><span class="sxs-lookup"><span data-stu-id="d763d-264">[How to: Specify the Web Server for Web Projects in Visual Studio](https://msdn.microsoft.com/library/ms178108.aspx).</span></span>
- <span data-ttu-id="d763d-265">[核心 IIS 之間的差異和 ASP.NET 程式開發伺服器](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)ASP.NET 網站上。</span><span class="sxs-lookup"><span data-stu-id="d763d-265">[Core Differences Between IIS and the ASP.NET Development Server](../deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) on the ASP.NET site.</span></span>
- <span data-ttu-id="d763d-266">[在 30 秒內測試 ASP.NET MVC 或 Web Form 應用程式在 IIS 7 上](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx)Rick Anderson 部落格上。</span><span class="sxs-lookup"><span data-stu-id="d763d-266">[Test your ASP.NET MVC or Web Forms Application on IIS 7 in 30 seconds](https://blogs.msdn.com/b/rickandy/archive/2011/04/22/test-you-asp-net-mvc-or-webforms-application-on-iis-7-in-30-seconds.aspx) on Rick Anderson's blog.</span></span> <span data-ttu-id="d763d-267">此項目提供的範例使用 Visual Studio 程式開發伺服器 (Cassini) 來測試為何不可靠，做為測試在 IIS Express 中，以及在 IIS Express 測試為何無法做為測試在 IIS 中的可靠性。</span><span class="sxs-lookup"><span data-stu-id="d763d-267">This entry provides examples of why testing with the Visual Studio Development Server (Cassini) is not as reliable as testing in IIS Express, and why testing in IIS Express is not as reliable as testing in IIS.</span></span>

<span data-ttu-id="d763d-268">在中度信任執行的應用程式時，可能會引發哪些問題的相關資訊，請參閱[裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)4 Guy 從 Rolla 站台上。</span><span class="sxs-lookup"><span data-stu-id="d763d-268">For information about what issues might arise when your application runs in medium trust, see [Hosting ASP.NET Applications in Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) on the 4 Guys from Rolla site.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="d763d-269">[上一頁](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[下一頁](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)</span><span class="sxs-lookup"><span data-stu-id="d763d-269">[Previous](deployment-to-a-hosting-provider-configuring-project-properties-4-of-12.md)
[Next](deployment-to-a-hosting-provider-setting-folder-permissions-6-of-12.md)</span></span>
