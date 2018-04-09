---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 使用 Visual Studio 的 ASP.NET Web 部署： 部署至測試 |Microsoft 文件
author: tdykstra
description: 此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/23/2015
ms.topic: article
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: dc11072e053cbddd089e5df4bcea6d2a7af864fc
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a><span data-ttu-id="47b40-103">使用 Visual Studio 的 ASP.NET Web 部署： 部署至測試</span><span class="sxs-lookup"><span data-stu-id="47b40-103">ASP.NET Web Deployment using Visual Studio: Deploying to Test</span></span>
====================
<span data-ttu-id="47b40-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="47b40-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

[<span data-ttu-id="47b40-105">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="47b40-105">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

> <span data-ttu-id="47b40-106">此教學課程會示範如何將部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web 應用程式或協力廠商裝載提供者，使用 Visual Studio 2012 或 Visual Studio 2010。</span><span class="sxs-lookup"><span data-stu-id="47b40-106">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider, by using Visual Studio 2012 or Visual Studio 2010.</span></span> <span data-ttu-id="47b40-107">數列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="47b40-107">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>


## <a name="overview"></a><span data-ttu-id="47b40-108">總覽</span><span class="sxs-lookup"><span data-stu-id="47b40-108">Overview</span></span>

<span data-ttu-id="47b40-109">本教學課程會示範如何在 ASP.NET web 應用程式部署至 IIS 在本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="47b40-109">This tutorial shows how to deploy an ASP.NET web application to IIS on the local computer.</span></span>

<span data-ttu-id="47b40-110">當您開發應用程式時，您通常會測試執行 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="47b40-110">When you develop an application, you generally test by running it in Visual Studio.</span></span> <span data-ttu-id="47b40-111">根據預設，Visual Studio 2012 中的 web 應用程式專案使用 IIS Express 做為程式開發 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="47b40-111">By default, web application projects in Visual Studio 2012 use IIS Express as the development web server.</span></span> <span data-ttu-id="47b40-112">IIS Express 的行為更類似於 Visual Studio 程式開發伺服器 (也稱為 Cassini)，依預設採用 Visual Studio 2010 的完整 IIS。</span><span class="sxs-lookup"><span data-stu-id="47b40-112">IIS Express behaves more like full IIS than the Visual Studio Development Server (also known as Cassini), which Visual Studio 2010 uses by default.</span></span> <span data-ttu-id="47b40-113">但是，開發 web 伺服器的運作方式完全一樣 IIS。</span><span class="sxs-lookup"><span data-stu-id="47b40-113">But neither development web server works exactly like IIS.</span></span> <span data-ttu-id="47b40-114">如此一來，很可能您測試在 Visual Studio 中，但失敗時，它會部署到 IIS 應用程式將會正確執行。</span><span class="sxs-lookup"><span data-stu-id="47b40-114">As a result, it's possible that an application will run correctly when you test it in Visual Studio, but fail when it's deployed to IIS.</span></span>

<span data-ttu-id="47b40-115">您可以更可靠地測試您的應用程式，以下列方式：</span><span class="sxs-lookup"><span data-stu-id="47b40-115">You can test your application more reliably in these ways:</span></span>

1. <span data-ttu-id="47b40-116">使用相同的程序，您稍後會使用將其部署到生產環境部署到 IIS 應用程式，在開發電腦上。</span><span class="sxs-lookup"><span data-stu-id="47b40-116">Deploy the application to IIS on your development computer by using the same process that you'll use later to deploy it to your production environment.</span></span> <span data-ttu-id="47b40-117">您可以設定 Visual Studio 來使用 IIS，當您執行 web 專案中，但這麼做可不會測試您的部署程序。</span><span class="sxs-lookup"><span data-stu-id="47b40-117">You can configure Visual Studio to use IIS when you run a web project, but doing that would not test your deployment process.</span></span> <span data-ttu-id="47b40-118">這個方法會驗證您的應用程式會正確地在 IIS 下執行您部署程序除了驗證。</span><span class="sxs-lookup"><span data-stu-id="47b40-118">This method validates your deployment process in addition to validating that your application will run correctly under IIS.</span></span>
2. <span data-ttu-id="47b40-119">部署至生產環境幾乎完全相同的測試環境。</span><span class="sxs-lookup"><span data-stu-id="47b40-119">Deploy the application to a test environment that is nearly identical to your production environment.</span></span> <span data-ttu-id="47b40-120">由於這些教學課程的生產環境是 Azure App Service 中的 Web 應用程式時，理想的測試環境是 Azure App Service 中建立的其他 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b40-120">Since the production environment for these tutorials is Web Apps in Azure App Service, the ideal test environment is an additional web app created in Azure App Service.</span></span> <span data-ttu-id="47b40-121">您會使用此第二個 web 應用程式僅供測試，但它會設定為生產環境 web 應用程式的相同方式。</span><span class="sxs-lookup"><span data-stu-id="47b40-121">You would use this second web app only for testing, but it would be set up the same way as the production web app.</span></span>

<span data-ttu-id="47b40-122">選項 2 最可靠的方式，來測試，而如果您這樣做，您不一定需要執行選項 1。</span><span class="sxs-lookup"><span data-stu-id="47b40-122">Option 2 is the most reliable way to test, and if you do that, you don't necessarily have to do option 1.</span></span> <span data-ttu-id="47b40-123">不過，如果您要部署到協力廠商裝載提供者選項 2 可能不可行，或可能高度耗費資源，因此這個教學課程 數列會顯示這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="47b40-123">However, if you are deploying to a third-party hosting provider option 2 might not be feasible or might be expensive, so this tutorial series shows both methods.</span></span> <span data-ttu-id="47b40-124">選項 2 的指南中提供[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="47b40-124">Guidance for option 2 is provided in the [Deploying to the Production Environment](deploying-to-production.md) tutorial.</span></span>

<span data-ttu-id="47b40-125">如需有關使用 Visual Studio 中的網頁伺服器的詳細資訊，請參閱[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)。</span><span class="sxs-lookup"><span data-stu-id="47b40-125">For more information about using web servers in Visual Studio, see [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>

<span data-ttu-id="47b40-126">提示： 如果您收到錯誤訊息，或當您瀏覽教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="47b40-126">Reminder: If you get an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="install-iis"></a><span data-ttu-id="47b40-127">安裝 IIS</span><span class="sxs-lookup"><span data-stu-id="47b40-127">Install IIS</span></span>

<span data-ttu-id="47b40-128">若要在開發電腦部署到 IIS，您必須擁有 IIS 和 Web Deploy 安裝。</span><span class="sxs-lookup"><span data-stu-id="47b40-128">To deploy to IIS on your development computer, you must have IIS and Web Deploy installed.</span></span> <span data-ttu-id="47b40-129">使用 Visual Studio 預設會安裝 web Deploy，但 IIS 未包含在預設的 Windows 8 或 Windows 7 中設定。</span><span class="sxs-lookup"><span data-stu-id="47b40-129">Web Deploy is installed by default with Visual Studio, but IIS is not included in the default Windows 8 or Windows 7 configuration.</span></span> <span data-ttu-id="47b40-130">如果您已安裝 IIS，而且預設應用程式集區已設為.NET 4，請跳至[下一節](#sqlexpress)。</span><span class="sxs-lookup"><span data-stu-id="47b40-130">If you have already installed IIS and the default application pool is already set to .NET 4, skip to [the next section](#sqlexpress).</span></span>

1. <span data-ttu-id="47b40-131">使用[Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx)是慣用的方法來安裝 IIS 和 Web Deploy，因為 Web Platform Installer 安裝 iis 建議的組態，而且它會自動安裝 IIS 和 Web 的必要條件如果需要，部署。</span><span class="sxs-lookup"><span data-stu-id="47b40-131">Using the [Web Platform Installer](https://www.microsoft.com/web/downloads/platform.aspx) is the preferred way to install IIS and Web Deploy, because the Web Platform Installer installs a recommended configuration for IIS and it automatically installs the prerequisites for IIS and Web Deploy if necessary.</span></span>

    <span data-ttu-id="47b40-132">若要執行 Web Platform Installer 安裝 IIS 和 Web Deploy，請使用下列連結。</span><span class="sxs-lookup"><span data-stu-id="47b40-132">To run Web Platform Installer to install IIS and Web Deploy, use the following link.</span></span> <span data-ttu-id="47b40-133">如果您已安裝 IIS、 Web Deploy 或任何必要的元件，會安裝 Web Platform Installer，只會遺失。</span><span class="sxs-lookup"><span data-stu-id="47b40-133">If you already have installed IIS, Web Deploy or any of their required components, the Web Platform Installer installs only what is missing.</span></span>

   - [<span data-ttu-id="47b40-134">安裝 IIS 和 Web Deploy 使用 WebPI，</span><span class="sxs-lookup"><span data-stu-id="47b40-134">Install IIS and Web Deploy using WebPI</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=IIS7;ASPNET;NETFramework4;WDeploy)

     <span data-ttu-id="47b40-135">您會看到訊息，指出將會安裝 IIS 7。</span><span class="sxs-lookup"><span data-stu-id="47b40-135">You'll see messages indicating that IIS 7 will be installed.</span></span> <span data-ttu-id="47b40-136">IIS 8 中在 Windows 8 中，但適用於 Windows 8，連結運作確定執行下列步驟來確認已安裝 ASP.NET 4.5:</span><span class="sxs-lookup"><span data-stu-id="47b40-136">The link works for IIS 8 in Windows 8, but for Windows 8 make sure that ASP.NET 4.5 is installed by performing the following steps:</span></span>

   - <span data-ttu-id="47b40-137">開啟**控制台**，**程式和功能**，**開啟或關閉 Windows 功能**。</span><span class="sxs-lookup"><span data-stu-id="47b40-137">Open **Control Panel**, **Programs and Features**, **Turn Windows features on or off**.</span></span>
   - <span data-ttu-id="47b40-138">展開**Internet Information Services**， **World Wide Web 服務**，和**應用程式開發功能**。</span><span class="sxs-lookup"><span data-stu-id="47b40-138">Expand **Internet Information Services**, **World Wide Web Services**, and **Application Development Features**.</span></span>
   - <span data-ttu-id="47b40-139">請確定**ASP.NET 4.5**已選取。</span><span class="sxs-lookup"><span data-stu-id="47b40-139">Make sure that **ASP.NET 4.5** is selected.</span></span>

      ![選取 ASP.NET 4.5](deploying-to-iis/_static/image1.png)

<span data-ttu-id="47b40-141">安裝 IIS 之後, 執行**IIS 管理員**，確定.NET Framework 第 4 版預設應用程式集區指派。</span><span class="sxs-lookup"><span data-stu-id="47b40-141">After installing IIS, run **IIS Manager** to make sure that the .NET Framework version 4 is assigned to the default application pool.</span></span>

1. <span data-ttu-id="47b40-142">按 WINDOWS + R，以開啟**執行** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="47b40-142">Press WINDOWS+R to open the **Run** dialog box.</span></span>

    <span data-ttu-id="47b40-143">(或 Windows 8 中輸入 「 執行 」**啟動**頁面上，或在 Windows 7 中選取**執行**從**啟動**功能表。</span><span class="sxs-lookup"><span data-stu-id="47b40-143">(Or in Windows 8 enter "run" on the **Start** page, or in Windows 7 select **Run** from the **Start** menu.</span></span> <span data-ttu-id="47b40-144">如果**執行**不在**啟動**功能表上，以滑鼠右鍵按一下工作列中，按一下**屬性**，選取**開始功能表**索引標籤上，按一下**自訂**，然後選取**執行命令**。)</span><span class="sxs-lookup"><span data-stu-id="47b40-144">If **Run** isn't in the **Start** menu, right-click the taskbar, click **Properties**, select the **Start Menu** tab, click **Customize**, and select **Run command**.)</span></span>
2. <span data-ttu-id="47b40-145">輸入 「 inetmgr"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="47b40-145">Enter "inetmgr", and then click **OK**.</span></span>
3. <span data-ttu-id="47b40-146">在**連線** 窗格中，展開伺服器節點，然後選取**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="47b40-146">In the **Connections** pane, expand the server node and select **Application Pools**.</span></span> <span data-ttu-id="47b40-147">在**應用程式集區** 窗格中，如果**DefaultAppPool**是指派給.NET framework 第 4 版圖所示，請跳至下一節。</span><span class="sxs-lookup"><span data-stu-id="47b40-147">In the **Application Pools** pane, if **DefaultAppPool** is assigned to the .NET framework version 4 as in the following illustration, skip to the next section.</span></span>

    <span data-ttu-id="47b40-148">[![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)</span><span class="sxs-lookup"><span data-stu-id="47b40-148">[![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image3.png)](deploying-to-iis/_static/image2.png)</span></span>
4. <span data-ttu-id="47b40-149">如果您看到只有兩個應用程式集區，而這兩種都設定為.NET Framework 2.0，您必須安裝在 IIS 中的 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="47b40-149">If you see only two application pools and both of them are set to the .NET Framework 2.0, you have to install ASP.NET 4 in IIS.</span></span>

    <span data-ttu-id="47b40-150">適用於 Windows 8，請參閱指示，在舊版區段中針對已安裝並確定該 ASP.NET 4.5，或請參閱[這篇知識庫文章](https://support.microsoft.com/kb/2736284)。</span><span class="sxs-lookup"><span data-stu-id="47b40-150">For Windows 8, see the instructions in the previous section for making sure that ASP.NET 4.5 is installed, or see [this KB article](https://support.microsoft.com/kb/2736284).</span></span> <span data-ttu-id="47b40-151">Windows 7 中，開啟命令提示字元視窗以滑鼠右鍵按一下**命令提示字元**windows**啟動**功能表，然後選取**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="47b40-151">For Windows 7, open a command prompt window by right-clicking **Command Prompt** in the Windows **Start** menu and selecting **Run as Administrator**.</span></span> <span data-ttu-id="47b40-152">然後執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)在 IIS 中，使用下列命令安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="47b40-152">Then run [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) to install ASP.NET 4 in IIS, using the following commands.</span></span> <span data-ttu-id="47b40-153">（在 32 位元系統中，取代"Framework64"與"架構"）。</span><span class="sxs-lookup"><span data-stu-id="47b40-153">(In 32-bit systems, replace "Framework64" with "Framework".)</span></span>

    [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

    <span data-ttu-id="47b40-154">此命令會建立新的應用程式集區的.NET Framework 4，但預設應用程式集區將仍會設定為 2.0。</span><span class="sxs-lookup"><span data-stu-id="47b40-154">This command creates new application pools for the .NET Framework 4, but the default application pool will still be set to 2.0.</span></span> <span data-ttu-id="47b40-155">您將部署應用程式目標為.NET 4 至該應用程式集區，因此您需要將應用程式集區變更為.NET 4。</span><span class="sxs-lookup"><span data-stu-id="47b40-155">You'll be deploying an application that targets .NET 4 to that application pool, so you have to change the application pool to .NET 4.</span></span>
5. <span data-ttu-id="47b40-156">如果您關閉**IIS 管理員**、 執行一次、 展開伺服器節點，然後按一下**應用程式集區**顯示**應用程式集區** 窗格。</span><span class="sxs-lookup"><span data-stu-id="47b40-156">If you closed **IIS Manager**, run it again, expand the server node, and click **Application Pools** to display the **Application Pools** pane again.</span></span>
6. <span data-ttu-id="47b40-157">在**應用程式集區**] 窗格中，按一下 [ **DefaultAppPool**，然後在**動作**窗格中，按一下**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="47b40-157">In the **Application Pools** pane, click **DefaultAppPool**, and then in the **Actions** pane click **Basic Settings**.</span></span>

    <span data-ttu-id="47b40-158">[![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="47b40-158">[![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image5.png)](deploying-to-iis/_static/image4.png)</span></span>
7. <span data-ttu-id="47b40-159">在**編輯應用程式集區**對話方塊中，變更**.NET Framework 版本**至**.NET Framework v4.0.30319**按一下**[確定]**。</span><span class="sxs-lookup"><span data-stu-id="47b40-159">In the **Edit Application Pool** dialog box, change **.NET Framework version** to **.NET Framework v4.0.30319** and click **OK**.</span></span>

    <span data-ttu-id="47b40-160">[![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)</span><span class="sxs-lookup"><span data-stu-id="47b40-160">[![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image7.png)](deploying-to-iis/_static/image6.png)</span></span>

<span data-ttu-id="47b40-161">IIS 現在準備好要發行的 web 應用程式，但可以這樣做之前您必須建立將使用之資料庫的測試環境中。</span><span class="sxs-lookup"><span data-stu-id="47b40-161">IIS is now ready for you to publish a web application to it, but before you can do that you have to create the databases that you will use in the test environment.</span></span>

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a><span data-ttu-id="47b40-162">安裝 SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="47b40-162">Install SQL Server Express</span></span>

<span data-ttu-id="47b40-163">LocalDB 不被為了在 IIS 中運作，因此您需要擁有 SQL Server Express 安裝您的測試環境。</span><span class="sxs-lookup"><span data-stu-id="47b40-163">LocalDB is not designed to work in IIS, so for your test environment you need to have SQL Server Express installed.</span></span> <span data-ttu-id="47b40-164">如果您使用 Visual Studio 2010 SQL Server Express 中預設已安裝。</span><span class="sxs-lookup"><span data-stu-id="47b40-164">If you are using Visual Studio 2010 SQL Server Express is already installed by default.</span></span> <span data-ttu-id="47b40-165">如果您使用 Visual Studio 2012，您必須安裝它。</span><span class="sxs-lookup"><span data-stu-id="47b40-165">If you are using Visual Studio 2012, you have to install it.</span></span>

<span data-ttu-id="47b40-166">若要安裝 SQL Server Express，將它從安裝[Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062)按一下[ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe)或[ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe)。</span><span class="sxs-lookup"><span data-stu-id="47b40-166">To install SQL Server Express, install it from [Download Center: Microsoft SQL Server 2012 Express](https://www.microsoft.com/download/details.aspx?id=29062) by clicking [ENU\x64\SQLEXPR\_x64\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x64/SQLEXPR_x64_ENU.exe) or [ENU\x86\SQLEXPR\_x86\_ENU.exe](https://download.microsoft.com/download/8/D/D/8DD7BDBA-CEF7-4D8E-8C16-D9F69527F909/ENU/x86/SQLEXPR_x86_ENU.exe).</span></span> <span data-ttu-id="47b40-167">如果您選擇錯誤的其中一個用於您的系統，它將無法安裝，而且您可以嘗試另一個。</span><span class="sxs-lookup"><span data-stu-id="47b40-167">If you choose the wrong one for your system it will fail to install and you can try the other one.</span></span>

<span data-ttu-id="47b40-168">在 SQL Server 安裝中心] 的第一個頁面上，按一下 [**新的 SQL Server 獨立安裝或將功能加入現有安裝**，並遵循指示，接受預設選項。</span><span class="sxs-lookup"><span data-stu-id="47b40-168">On the first page of the SQL Server Installation Center, click **New SQL Server stand-alone installation or add features to an existing installation**, and follow the instructions, accepting the default choices.</span></span> <span data-ttu-id="47b40-169">安裝精靈 中接受預設設定。</span><span class="sxs-lookup"><span data-stu-id="47b40-169">In the installation wizard accept the default settings.</span></span> <span data-ttu-id="47b40-170">如需有關安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server 2012](https://msdn.microsoft.com/library/ms143219.aspx)。</span><span class="sxs-lookup"><span data-stu-id="47b40-170">For more information about installation options, see [Install SQL Server 2012 from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="create-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="47b40-171">建立測試環境的 SQL Server Express 資料庫</span><span class="sxs-lookup"><span data-stu-id="47b40-171">Create SQL Server Express databases for the test environment</span></span>

<span data-ttu-id="47b40-172">Contoso 大學應用程式有兩個資料庫： 成員資格資料庫和應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-172">The Contoso University application has two databases: the membership database and the application database.</span></span> <span data-ttu-id="47b40-173">兩個不同的資料庫或單一資料庫，您可以部署這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-173">You can deploy these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="47b40-174">您可以將它們結合為簡化資料庫應用程式資料庫和成員資格資料庫之間的聯結。</span><span class="sxs-lookup"><span data-stu-id="47b40-174">You might want to combine them in order to facilitate database joins between your application database and your membership database.</span></span> <span data-ttu-id="47b40-175">如果您要部署到協力廠商主機服務提供者，主控方案可能也會提供將它們結合的原因。</span><span class="sxs-lookup"><span data-stu-id="47b40-175">If you are deploying to a third-party hosting provider, your hosting plan might also provide a reason to combine them.</span></span> <span data-ttu-id="47b40-176">例如，主機服務提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-176">For example, the hosting provider might charge more for multiple databases or might not even allow more than one database.</span></span>

<span data-ttu-id="47b40-177">在本教學課程中，您將部署在測試環境中，兩個資料庫，並在預備與生產環境中的一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-177">In this tutorial, you'll deploy to two databases in the test environment, and to one database in the staging and production environments.</span></span>

<span data-ttu-id="47b40-178">從**檢視**在 Visual Studio 中選取的功能表**伺服器總管**(**資料庫總管**在 Visual Web Developer)，然後以滑鼠右鍵按一下**資料連接**選取**建立新的 SQL Server 資料庫**。</span><span class="sxs-lookup"><span data-stu-id="47b40-178">From the **View** menu in Visual Studio select **Server Explorer** (**Database Explorer** in Visual Web Developer), and then right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

<span data-ttu-id="47b40-180">在**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress 」 中**伺服器名稱**方塊和"aspnet-ContosoUniversity 」 中**新的資料庫名稱**方塊，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="47b40-180">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-ContosoUniversity" in the **New database name** box, then click **OK**.</span></span>

![建立 aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

<span data-ttu-id="47b40-182">請遵循相同的程序來建立新的 SQL Server Express School 資料庫名為"ContosoUniversity"。</span><span class="sxs-lookup"><span data-stu-id="47b40-182">Follow the same procedure to create a new SQL Server Express School database named "ContosoUniversity".</span></span>

<span data-ttu-id="47b40-183">**伺服器總管**現在會顯示兩個新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-183">**Server Explorer** now shows the two new databases.</span></span>

![在伺服器總管 中的新資料庫](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a><span data-ttu-id="47b40-185">建立新資料庫的授與指令碼</span><span class="sxs-lookup"><span data-stu-id="47b40-185">Create a grant script for the new databases</span></span>

<span data-ttu-id="47b40-186">當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區認證存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-186">When the application runs in IIS on your development computer, the application accesses the database by using the default application pool's credentials.</span></span> <span data-ttu-id="47b40-187">不過，根據預設，應用程式集區識別沒有權限來開啟資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-187">However, by default, the application pool identity does not have permission to open the databases.</span></span> <span data-ttu-id="47b40-188">因此，您必須執行指令碼授與該權限。</span><span class="sxs-lookup"><span data-stu-id="47b40-188">So you have to run a script to grant that permission.</span></span> <span data-ttu-id="47b40-189">本節中，您會建立指令碼，您將執行更新版本，才能確定應用程式在執行於 IIS 時，可以開啟資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-189">In this section you create the script that you'll run later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="47b40-190">以滑鼠右鍵按一下方案 （不是其中一個專案），然後按一下**加入新項目**，然後再建立新**SQL 檔案**名為*Grant.sql*。</span><span class="sxs-lookup"><span data-stu-id="47b40-190">Right-click the solution (not one of the projects), and click **Add New Item**, and then create a new **SQL File** named *Grant.sql*.</span></span> <span data-ttu-id="47b40-191">將下列 SQL 命令複製到檔案，然後儲存並關閉檔案：</span><span class="sxs-lookup"><span data-stu-id="47b40-191">Copy the following SQL commands into the file, and then save and close the file:</span></span>

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

> [!NOTE]
> <span data-ttu-id="47b40-192">此指令碼被設計用於 SQL Server Express 2012 與 Windows 8 或 Windows 7 中的 IIS 設定為它們指定本教學課程中。</span><span class="sxs-lookup"><span data-stu-id="47b40-192">This script is designed to work with SQL Server Express 2012 and with the IIS settings in Windows 8 or Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="47b40-193">如果您使用不同版本的 SQL Server 或 Windows，或如果您設定 IIS 在您的電腦上以不同的方式，可能必須使用這個指令碼的變更。</span><span class="sxs-lookup"><span data-stu-id="47b40-193">If you're using a different version of SQL Server or of Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="47b40-194">如需有關 SQL Server 指令碼的詳細資訊，請參閱[SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。</span><span class="sxs-lookup"><span data-stu-id="47b40-194">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>


> [!NOTE] 
> 
> <span data-ttu-id="47b40-195">**安全性注意事項**此指令碼會為 db\_使用者在執行階段，您將必須在生產環境中存取資料庫的擁有者權限。</span><span class="sxs-lookup"><span data-stu-id="47b40-195">**Security Note** This script gives db\_owner permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="47b40-196">在某些情況下，您可以指定具有完整的資料庫結構描述更新權限只部署，而且指定的執行階段不同的使用者具有權限僅讀取和寫入資料的使用者。</span><span class="sxs-lookup"><span data-stu-id="47b40-196">In some scenarios you might want to specify a user that has full database schema update permissions only for deployment, and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="47b40-197">如需詳細資訊，請參閱[檢閱自動 Web.config 變更的程式碼 Code First 移轉](#reviewingmigrations)稍後在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="47b40-197">For more information, see [Reviewing the Automatic Web.config Changes for Code First Migrations](#reviewingmigrations) later in this tutorial.</span></span>


<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a><span data-ttu-id="47b40-198">執行應用程式資料庫中授與指令碼</span><span class="sxs-lookup"><span data-stu-id="47b40-198">Run the grant script in the application database</span></span>

<span data-ttu-id="47b40-199">您可以設定執行授與指令碼的成員資格資料庫中，在部署期間因為該資料庫的部署使用 dbDacFx 提供者的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="47b40-199">You can configure the publish profile to run the grant script in the membership database during deployment because that database deployment uses the dbDacFx provider.</span></span> <span data-ttu-id="47b40-200">您無法執行指令碼，Code First 移轉在部署期間，也就是您要部署應用程式資料庫的方式。</span><span class="sxs-lookup"><span data-stu-id="47b40-200">You can't run scripts during Code First Migrations deployment, which is how you're deploying the application database.</span></span> <span data-ttu-id="47b40-201">因此，您必須手動執行部署前指令碼應用程式資料庫中。</span><span class="sxs-lookup"><span data-stu-id="47b40-201">Therefore, you have to manually run the script before deployment in the application database.</span></span>

1. <span data-ttu-id="47b40-202">在 Visual Studio 中開啟*Grant.sql*您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-202">In Visual Studio, open the *Grant.sql* file that you created earlier.</span></span>
2. <span data-ttu-id="47b40-203">按一下 **[Connect]**(連線)。</span><span class="sxs-lookup"><span data-stu-id="47b40-203">Click **Connect**.</span></span> 

    ![連接按鈕](deploying-to-iis/_static/image11.png)
3. <span data-ttu-id="47b40-205">在**連接到伺服器**對話方塊方塊中，輸入*。 \SQLExpress*為**伺服器名稱**，然後按一下 **連接**。</span><span class="sxs-lookup"><span data-stu-id="47b40-205">In the **Connect to Server** dialog box, enter *.\SQLExpress* as the **Server Name**, and then click **Connect**.</span></span>
4. <span data-ttu-id="47b40-206">在 資料庫 下拉式清單中選取**ContosoUniversity**，然後按一下  **Execute**。</span><span class="sxs-lookup"><span data-stu-id="47b40-206">In the database drop-down list select **ContosoUniversity**, and then click **Execute**.</span></span> 

    ![](deploying-to-iis/_static/image12.png)

<span data-ttu-id="47b40-207">預設應用程式集區識別現在會在 Code First 移轉應用程式執行時建立資料庫資料表的應用程式資料庫有足夠的權限。</span><span class="sxs-lookup"><span data-stu-id="47b40-207">The default application pool identity now has sufficient permissions in the application database for Code First Migrations to create the database tables when the application runs.</span></span>

## <a name="publish-to-iis"></a><span data-ttu-id="47b40-208">發行至 IIS</span><span class="sxs-lookup"><span data-stu-id="47b40-208">Publish to IIS</span></span>

<span data-ttu-id="47b40-209">有數種方式，您可以部署到使用 Visual Studio 和 Web Deploy 的 IIS:</span><span class="sxs-lookup"><span data-stu-id="47b40-209">There are several ways you can deploy to IIS using Visual Studio and Web Deploy:</span></span>

- <span data-ttu-id="47b40-210">使用單鍵發行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="47b40-210">Use Visual Studio one-click publish.</span></span>
- <span data-ttu-id="47b40-211">從命令列發行。</span><span class="sxs-lookup"><span data-stu-id="47b40-211">Publish from the command line.</span></span>
- <span data-ttu-id="47b40-212">建立*部署套件*及使用 IIS 管理員 UI 進行安裝。</span><span class="sxs-lookup"><span data-stu-id="47b40-212">Create a *deployment package* and install it using the IIS Manager UI.</span></span> <span data-ttu-id="47b40-213">部署套件組成*.zip*檔案，其中包含所有檔案和在 IIS 中安裝站台所需的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="47b40-213">The deployment package consists of a *.zip* file that contains all the files and metadata needed to install a site in IIS.</span></span>
- <span data-ttu-id="47b40-214">建立部署套件，並使用命令列進行安裝。</span><span class="sxs-lookup"><span data-stu-id="47b40-214">Create a deployment package and install it using the command line.</span></span>

<span data-ttu-id="47b40-215">您已在上一個教學課程中設定 Visual Studio，來自動化部署工作套用到所有這些方法的程序。</span><span class="sxs-lookup"><span data-stu-id="47b40-215">The process you went through in the previous tutorials to set up Visual Studio to automate deployment tasks applies to all of these methods.</span></span> <span data-ttu-id="47b40-216">在這些教學課程中，您將使用這些方法的前兩個。</span><span class="sxs-lookup"><span data-stu-id="47b40-216">In these tutorials you'll use the first two of these methods.</span></span> <span data-ttu-id="47b40-217">使用部署套件的相關資訊，請參閱[部署 web 應用程式建立及安裝 web 部署套件](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)Visual Studio 和 ASP.NET 的 Web 部署內容對應中。</span><span class="sxs-lookup"><span data-stu-id="47b40-217">For information about using deployment packages, see [Deploying a web application by creating and installing a web deployment package](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

<span data-ttu-id="47b40-218">在發行之前，請確定您系統管理員模式中執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="47b40-218">Before publishing, make sure that you are running Visual Studio in administrator mode.</span></span> <span data-ttu-id="47b40-219">如果您沒有看到**（系統管理員）**的標題列中，關閉 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="47b40-219">If you don't see **(Administrator)** in the title bar, close Visual Studio.</span></span> <span data-ttu-id="47b40-220">在 Windows 8**啟動**頁面或 Windows 7**啟動**功能表上，以滑鼠右鍵按一下您要使用的 Visual Studio 版本的圖示，然後選取**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="47b40-220">In the Windows 8 **Start** page or the Windows 7 **Start** menu, right-click the icon for the version of Visual Studio you're using and select **Run as Administrator**.</span></span> <span data-ttu-id="47b40-221">需要發行只有當您發行至 IIS 在本機電腦上的系統管理員模式。</span><span class="sxs-lookup"><span data-stu-id="47b40-221">Administrator mode is required for publishing only when you are publishing to IIS on the local computer.</span></span>

### <a name="create-the-publish-profile"></a><span data-ttu-id="47b40-222">建立發行設定檔</span><span class="sxs-lookup"><span data-stu-id="47b40-222">Create the publish profile</span></span>

1. <span data-ttu-id="47b40-223">在**方案總管 中**，以滑鼠右鍵按一下 ContosoUniversity 專案 （而非 ContosoUniversity.DAL 專案），然後選取**發行**。</span><span class="sxs-lookup"><span data-stu-id="47b40-223">In **Solution Explorer**, right-click the ContosoUniversity project (not the ContosoUniversity.DAL project) and select **Publish**.</span></span>

    <span data-ttu-id="47b40-224">**發行 Web**精靈 隨即出現。</span><span class="sxs-lookup"><span data-stu-id="47b40-224">The **Publish Web** wizard appears.</span></span>

    ![發行網站精靈設定檔 索引標籤](deploying-to-iis/_static/image13.png)
2. <span data-ttu-id="47b40-226">在下拉式清單中，選取**&lt;新增...&gt;**.</span><span class="sxs-lookup"><span data-stu-id="47b40-226">In the drop-down list, select **&lt;New...&gt;**.</span></span> <span data-ttu-id="47b40-227">(最新版 Visual Studio 安裝更新之後，沒有下拉式清單，並按一下 以從頭開始建立新的設定檔的按鈕**自訂**。)</span><span class="sxs-lookup"><span data-stu-id="47b40-227">(With the latest Visual Studio update installed, there is no drop-down list, and the button to click in order to create a new profile from scratch is **Custom**.)</span></span>
3. <span data-ttu-id="47b40-228">在**新設定檔**對話方塊中，輸入 「 測試 」，，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="47b40-228">In the **New Profile** dialog box, enter "Test", and then click **OK**.</span></span>

    <span data-ttu-id="47b40-229">精靈會自動移到**連接** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="47b40-229">The wizard automatically advances to the **Connection** tab.</span></span>
4. <span data-ttu-id="47b40-230">在**服務 URL**方塊中，輸入*localhost*。</span><span class="sxs-lookup"><span data-stu-id="47b40-230">In the **Service URL** box, enter *localhost*.</span></span>
5. <span data-ttu-id="47b40-231">在**網站/應用程式**方塊中，輸入*預設網站/ContosoUniversity*</span><span class="sxs-lookup"><span data-stu-id="47b40-231">In the **Site/application** box, enter *Default Web Site/ContosoUniversity*</span></span>
6. <span data-ttu-id="47b40-232">在**目的地 URL**方塊中，輸入 `http://localhost/ContosoUniversity`</span><span class="sxs-lookup"><span data-stu-id="47b40-232">In the **Destination URL** box, enter `http://localhost/ContosoUniversity`</span></span>

    <span data-ttu-id="47b40-233">**目的地 URL**設定不是必要的。</span><span class="sxs-lookup"><span data-stu-id="47b40-233">The **Destination URL** setting isn't required.</span></span> <span data-ttu-id="47b40-234">當 Visual Studio 完成部署應用程式時，它會自動開啟預設的瀏覽器對這個 URL。</span><span class="sxs-lookup"><span data-stu-id="47b40-234">When Visual Studio finishes deploying the application, it automatically opens your default browser to this URL.</span></span> <span data-ttu-id="47b40-235">如果您不想要在部署後會自動開啟瀏覽器，將此方塊保留空白。</span><span class="sxs-lookup"><span data-stu-id="47b40-235">If you don't want the browser to open automatically after deployment, leave this box blank.</span></span>
7. <span data-ttu-id="47b40-236">按一下**驗證連線**確認設定正確無誤，而且您可以在本機電腦上連接到 IIS。</span><span class="sxs-lookup"><span data-stu-id="47b40-236">Click **Validate Connection** to verify that the settings are correct and you can connect to IIS on the local computer.</span></span>

    <span data-ttu-id="47b40-237">綠色的核取記號會驗證是成功的連線。</span><span class="sxs-lookup"><span data-stu-id="47b40-237">A green check mark verifies that the connection is successful.</span></span>

    ![發行網站精靈連線 索引標籤](deploying-to-iis/_static/image14.png)
8. <span data-ttu-id="47b40-239">按一下**下一步**前進到**設定** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="47b40-239">Click **Next** to advance to the **Settings** tab.</span></span>
9. <span data-ttu-id="47b40-240">**組態**下拉式清單方塊指定要部署的組建組態。</span><span class="sxs-lookup"><span data-stu-id="47b40-240">The **Configuration** drop-down box specifies the build configuration to deploy.</span></span> <span data-ttu-id="47b40-241">將它設為發行版本的預設值。</span><span class="sxs-lookup"><span data-stu-id="47b40-241">Leave it set to the default value of Release.</span></span> <span data-ttu-id="47b40-242">您將不會在本教學課程部署偵錯組建。</span><span class="sxs-lookup"><span data-stu-id="47b40-242">You won't be deploying Debug builds in this tutorial.</span></span>
10. <span data-ttu-id="47b40-243">展開**檔案發行選項**，然後選取**從應用程式中排除檔案\_資料資料夾**。</span><span class="sxs-lookup"><span data-stu-id="47b40-243">Expand **File Publish Options**, and then select **Exclude files from the App\_Data folder**.</span></span>

    <span data-ttu-id="47b40-244">在測試環境中應用程式會存取您本機的 SQL Server Express 執行個體中建立的資料庫，不是.mdf 檔案中*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b40-244">In the test environment the application will access the databases that you created in the local SQL Server Express instance, not the .mdf files in the *App\_Data* folder.</span></span>
11. <span data-ttu-id="47b40-245">保留**發行期間 Precompile**和**移除目的端的其他檔案**清除核取方塊。</span><span class="sxs-lookup"><span data-stu-id="47b40-245">Leave the **Precompile during publishing** and **Remove additional files at destination** check boxes cleared.</span></span>

    ![在 [設定] 索引標籤中的檔案發行選項](deploying-to-iis/_static/image15.png)

    <span data-ttu-id="47b40-247">先行編譯是選項，主要是用於非常大型的網站。它可減少第一次發行網站之後，要求頁面的頁面啟動時間。</span><span class="sxs-lookup"><span data-stu-id="47b40-247">Precompiling is an option that is useful mainly for very large sites; it can reduce page startup time for the first time a page is requested after the site is published.</span></span>

    <span data-ttu-id="47b40-248">您不需要移除其他檔案，因為這是您第一次部署，而且不會有任何檔案的目的地資料夾中尚未。</span><span class="sxs-lookup"><span data-stu-id="47b40-248">You don't need to remove additional files since this is your first deployment and there won't be any files in the destination folder yet.</span></span>

    > [!NOTE] 
    > 
    > [!CAUTION]
    > <span data-ttu-id="47b40-249">如果您選取**移除其他檔案**後續部署相同的站台，請確定您使用預覽功能，讓您查看事先在部署之前，將會刪除的檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-249">If you select **Remove additional files** for a subsequent deployment to the same site, make sure that you use the preview feature so that you see in advance which files will be deleted before you deploy.</span></span> <span data-ttu-id="47b40-250">預期的行為是，Web Deploy 將會刪除您已刪除專案中的目的地伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-250">The expected behavior is that Web Deploy will delete files on the destination server that you have deleted in your project.</span></span> <span data-ttu-id="47b40-251">不過，比較來源和目的地資料夾底下的整個資料夾結構，而且在某些情況下在 Web Deploy 可能會刪除您不想要刪除的檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-251">However, the entire folder structure under the source and destination folders is compared, and in some scenarios Web Deploy might delete files you don't want to delete.</span></span>
    > 
    > <span data-ttu-id="47b40-252">例如，如果您部署專案的根資料夾時，您可以有伺服器上的子資料夾中的 web 應用程式，將刪除子資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b40-252">For example, if you have a web application in a subfolder on the server when you deploy a project to the root folder, the subfolder will be deleted.</span></span> <span data-ttu-id="47b40-253">您可能必須在 contoso.com 的主要站台的單一專案並且在 contoso.com/blog 部落格內容的另一個專案。</span><span class="sxs-lookup"><span data-stu-id="47b40-253">You might have one project for the main site at contoso.com and another project for a blog at contoso.com/blog.</span></span> <span data-ttu-id="47b40-254">部落格應用程式是在子資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b40-254">The blog application is in a subfolder.</span></span> <span data-ttu-id="47b40-255">如果您選取移除目的地的其他檔案，當您部署的主要站台時，將會刪除部落格應用程式。</span><span class="sxs-lookup"><span data-stu-id="47b40-255">If you select Remove additional files at destination when you deploy the main site, the blog application will be deleted.</span></span>
    > 
    > <span data-ttu-id="47b40-256">如需其他範例，您的應用程式\_資料資料夾可能會意外刪除。</span><span class="sxs-lookup"><span data-stu-id="47b40-256">For another example, your App\_Data folder might get deleted unexpectedly.</span></span> <span data-ttu-id="47b40-257">某些資料庫，例如 SQL Server Compact 資料庫檔案儲存在應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b40-257">Certain databases such as SQL Server Compact store database files in the App\_Data folder.</span></span> <span data-ttu-id="47b40-258">初始部署之後，您不想保留在後續的部署中複製資料庫檔案，讓您選取排除的應用程式\_封裝/發行 Web 索引標籤上的資料。如果您有要移除其他檔案，在 選取目的地，將資料庫檔案和應用程式之後\_資料資料夾本身將會刪除您所發佈在下一次。</span><span class="sxs-lookup"><span data-stu-id="47b40-258">After the initial deployment you don't want to keep copying the database files in subsequent deployments so you select  Exclude App\_Data on the Package/Publish Web tab. After you do that, if you have Remove additional files at destination selected, your database files and the App\_Data folder itself will be deleted the next time you publish.</span></span>

### <a name="configure-deployment-for-the-membership-database"></a><span data-ttu-id="47b40-259">設定成員資格資料庫的部署</span><span class="sxs-lookup"><span data-stu-id="47b40-259">Configure deployment for the membership database</span></span>

<span data-ttu-id="47b40-260">下列步驟適用於**DefaultConnection**資料庫**資料庫**對話方塊的區段。</span><span class="sxs-lookup"><span data-stu-id="47b40-260">The following steps apply to the **DefaultConnection** database in the **Databases** section of the dialog box.</span></span>

1. <span data-ttu-id="47b40-261">在**遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 的成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-261">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express membership database.</span></span>

    [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

    <span data-ttu-id="47b40-262">部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。</span><span class="sxs-lookup"><span data-stu-id="47b40-262">The deployment process will put this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

    <span data-ttu-id="47b40-263">您也可以取得的連接字串**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="47b40-263">You can also get the connection string from **Server Explorer**.</span></span> <span data-ttu-id="47b40-264">在**伺服器總管**，依序展開**資料連接**選取 **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**資料庫，然後從**屬性**視窗複製**連接字串**值。</span><span class="sxs-lookup"><span data-stu-id="47b40-264">In **Server Explorer**, expand **Data Connections** and select the **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, then from the **Properties** window copy the **Connection String** value.</span></span> <span data-ttu-id="47b40-265">連接字串將會有一個額外的設定，您可以將其刪除： `Pooling=False`。</span><span class="sxs-lookup"><span data-stu-id="47b40-265">That connection string will have one additional setting that you can delete: `Pooling=False`.</span></span>
2. <span data-ttu-id="47b40-266">選取**更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="47b40-266">Select **Update database**.</span></span>

    <span data-ttu-id="47b40-267">這會導致在部署期間，目的地資料庫中建立的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="47b40-267">This will cause the database schema to be created in the destination database during deployment.</span></span> <span data-ttu-id="47b40-268">您可以在下列步驟中指定您需要執行其他指令碼： 一個，以便授與資料庫存取權的預設應用程式集區以及一到部署的資料。</span><span class="sxs-lookup"><span data-stu-id="47b40-268">In the following steps you specify the additional scripts that you need to run: one to grant database access to the default application pool and one to deploy data.</span></span>
3. <span data-ttu-id="47b40-269">按一下**設定資料庫更新**。</span><span class="sxs-lookup"><span data-stu-id="47b40-269">Click **Configure database updates**.</span></span>
4. <span data-ttu-id="47b40-270">在**設定資料庫更新**對話方塊中，按一下 **加入 SQL 指令碼**然後瀏覽至*Grant.sql*您稍早儲存的方案資料夾中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="47b40-270">In the **Configure Database Updates** dialog box, click **Add SQL Script** and then navigate to the *Grant.sql* script that you saved earlier in the solution folder.</span></span>
5. <span data-ttu-id="47b40-271">重複此程序加入*aspnet-資料-dev.sql*指令碼。</span><span class="sxs-lookup"><span data-stu-id="47b40-271">Repeat the process to add the *aspnet-data-dev.sql* script.</span></span>

    ![設定資料庫更新成員資格資料庫](deploying-to-iis/_static/image16.png)
6. <span data-ttu-id="47b40-273">按一下 [ **關閉**]。</span><span class="sxs-lookup"><span data-stu-id="47b40-273">Click **Close**.</span></span>

### <a name="configure-deployment-for-the-application-database"></a><span data-ttu-id="47b40-274">設定應用程式資料庫的部署</span><span class="sxs-lookup"><span data-stu-id="47b40-274">Configure deployment for the application database</span></span>

<span data-ttu-id="47b40-275">當 Visual Studio 偵測到 Entity Framework`DbContext`類別，它會建立中的項目**資料庫**區段**執行 Code First 移轉**核取方塊，而不是**更新資料庫**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="47b40-275">When Visual Studio detects an Entity Framework `DbContext` class, it creates an entry in the **Databases** section that has an **Execute Code First Migrations** check box instead of an **Update Database** check box.</span></span> <span data-ttu-id="47b40-276">在此教學課程中，您將使用該核取方塊來指定 Code First 移轉部署。</span><span class="sxs-lookup"><span data-stu-id="47b40-276">For this tutorial you'll use that check box to specify Code First Migrations deployment.</span></span>

<span data-ttu-id="47b40-277">在某些情況下，您也可以使用`DbContext`資料庫中，但是想要將資料庫部署，而不是移轉使用 dbDacFx 提供者。</span><span class="sxs-lookup"><span data-stu-id="47b40-277">In some scenarios, you might be using a `DbContext` database but you want to use the dbDacFx provider instead of Migrations to deploy the database.</span></span> <span data-ttu-id="47b40-278">在此情況下，請參閱[如何部署 Code First 移轉沒有資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)在 ASP.NET Web 部署常見問題集 MSDN 上。</span><span class="sxs-lookup"><span data-stu-id="47b40-278">In that case, see [How do I deploy a Code First database without Migrations?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in the ASP.NET Web Deployment FAQ on MSDN.</span></span>

<span data-ttu-id="47b40-279">下列步驟適用於**SchoolContext**資料庫**資料庫**對話方塊的區段。</span><span class="sxs-lookup"><span data-stu-id="47b40-279">The following steps apply to the **SchoolContext** database in the **Databases** section of the dialog box.</span></span>

1. <span data-ttu-id="47b40-280">在**遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-280">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express application database.</span></span>

    [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

    <span data-ttu-id="47b40-281">部署程序將會置於這個連接字串已部署 Web.config 檔案，因為**在執行階段使用此連接字串**已選取。</span><span class="sxs-lookup"><span data-stu-id="47b40-281">The deployment process will put this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

    <span data-ttu-id="47b40-282">您也可以取得應用程式資料庫的連接字串從**伺服器總管**相同的方式取得成員資格資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="47b40-282">You can also get the application database connection string from **Server Explorer** the same way you got the membership database connection string.</span></span>
2. <span data-ttu-id="47b40-283">選取**執行 Code First 移轉 （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="47b40-283">Select **Execute Code First Migrations (runs on application start)**.</span></span>

    <span data-ttu-id="47b40-284">這個選項會讓部署程序來設定已部署的 Web.config 檔案來指定`MigrateDatabaseToLatestVersion`初始設定式。</span><span class="sxs-lookup"><span data-stu-id="47b40-284">This option causes the deployment process to configure the deployed Web.config file to specify the `MigrateDatabaseToLatestVersion` initializer.</span></span> <span data-ttu-id="47b40-285">應用程式部署後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="47b40-285">This initializer automatically updates the database to the latest version when the application accesses the database for the first time after deployment.</span></span>

### <a name="configure-publish-profile-transforms"></a><span data-ttu-id="47b40-286">設定發行設定檔的轉換</span><span class="sxs-lookup"><span data-stu-id="47b40-286">Configure publish profile transforms</span></span>

1. <span data-ttu-id="47b40-287">按一下**關閉**，然後按一下 **是**當詢問您是否要儲存變更。</span><span class="sxs-lookup"><span data-stu-id="47b40-287">Click **Close**, and then click **Yes** when you are asked if you want to save changes.</span></span>
2. <span data-ttu-id="47b40-288">在**方案總管 中**，依序展開**屬性**，依序展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="47b40-288">In **Solution Explorer**, expand **Properties**, expand **PublishProfiles**.</span></span>
3. <span data-ttu-id="47b40-289">Rright 按一下*Test.pubxml，* ，然後按一下 **新增 Config 轉換**。</span><span class="sxs-lookup"><span data-stu-id="47b40-289">Rright-click *Test.pubxml,* and then click **Add Config Transform**.</span></span>

    ![新增 Config 轉換功能表](deploying-to-iis/_static/image17.png)

    <span data-ttu-id="47b40-291">Visual Studio 會建立*Web.Test.config*轉換檔案並開啟它。</span><span class="sxs-lookup"><span data-stu-id="47b40-291">Visual Studio creates the *Web.Test.config* transform file and opens it.</span></span>
4. <span data-ttu-id="47b40-292">在*Web.Test.config*轉換檔案，請插入下列程式碼開頭之後立即組態標記。</span><span class="sxs-lookup"><span data-stu-id="47b40-292">In the *Web.Test.config* transform file, insert the following code immediately after the opening configuration tag.</span></span>

    [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    <span data-ttu-id="47b40-293">當您使用測試發行設定檔時，此轉換設定"Test"的環境標記。</span><span class="sxs-lookup"><span data-stu-id="47b40-293">When you use the Test publish profile, this transform sets the environment indicator to "Test".</span></span> <span data-ttu-id="47b40-294">已部署的站台中您會看到 「 （測試） 」 之後的 「 Contoso 大學"H1 標題。</span><span class="sxs-lookup"><span data-stu-id="47b40-294">In the deployed site you'll see "(Test)" after the "Contoso University" H1 heading.</span></span>
5. <span data-ttu-id="47b40-295">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-295">Save and close the file.</span></span>
6. <span data-ttu-id="47b40-296">以滑鼠右鍵按一下*Web.Test.config*檔案，然後按一下 **預覽轉換**先確定您已編碼的轉換會產生預期的變更。</span><span class="sxs-lookup"><span data-stu-id="47b40-296">Right-click the *Web.Test.config* file and click **Preview Transform** to make sure that the transform you coded produces the expected changes.</span></span>

    <span data-ttu-id="47b40-297">**Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換和*Web.Test.config*轉換。</span><span class="sxs-lookup"><span data-stu-id="47b40-297">The **Web.config Preview** window shows the result of applying both the *Web.Release.config* transforms and the *Web.Test.config* transforms.</span></span>

### <a name="preview-the-deployment-updates"></a><span data-ttu-id="47b40-298">預覽部署更新</span><span class="sxs-lookup"><span data-stu-id="47b40-298">Preview the deployment updates</span></span>

1. <span data-ttu-id="47b40-299">開啟**發行 Web**精靈一次 (ContosoUniversity 專案上按一下滑鼠右鍵，然後按一下**發行**)。</span><span class="sxs-lookup"><span data-stu-id="47b40-299">Open the **Publish Web** wizard again (right-click the ContosoUniversity project and click **Publish**).</span></span>
2. <span data-ttu-id="47b40-300">在**預覽**索引標籤上，請確定**測試**設定檔為仍然選取，然後按一下**啟動預覽**若要查看將複製的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="47b40-300">In the **Preview** tab, make sure that the **Test** profile is still selected, and then click **Start Preview** to see a list of the files that will be copied.</span></span>

    ![[預覽] 按鈕](deploying-to-iis/_static/image18.png)

    ![發行預覽](deploying-to-iis/_static/image19.png)

    <span data-ttu-id="47b40-303">您也可以按一下**預覽資料庫**連結，查看將在成員資格資料庫中執行的指令碼。</span><span class="sxs-lookup"><span data-stu-id="47b40-303">You can also click the **Preview database** link to see the scripts that will run in the membership database.</span></span> <span data-ttu-id="47b40-304">（任何指令碼會不執行 Code First 移轉部署，因此沒有要預覽的應用程式資料庫）。</span><span class="sxs-lookup"><span data-stu-id="47b40-304">(No scripts are run for Code First Migrations deployment, so there is nothing to preview for the application database.)</span></span>
3. <span data-ttu-id="47b40-305">按一下 [發行] 。</span><span class="sxs-lookup"><span data-stu-id="47b40-305">Click **Publish**.</span></span>

    <span data-ttu-id="47b40-306">如果 Visual Studio 不在系統管理員模式中，您可能會收到錯誤訊息，指出權限錯誤。</span><span class="sxs-lookup"><span data-stu-id="47b40-306">If Visual Studio is not in administrator mode, you might get an error message that indicates a permissions error.</span></span> <span data-ttu-id="47b40-307">在此情況下，關閉 Visual Studio，以系統管理員模式開啟，並再次嘗試發行。</span><span class="sxs-lookup"><span data-stu-id="47b40-307">In that case, close Visual Studio, open it in administrator mode, and try to publish again.</span></span>

    <span data-ttu-id="47b40-308">如果 Visual Studio 是以系統管理員模式**輸出**視窗中報告成功建置並發行。</span><span class="sxs-lookup"><span data-stu-id="47b40-308">If Visual Studio is in administrator mode, the **Output** window reports successful build and publish.</span></span>

    ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

    <span data-ttu-id="47b40-310">如果您在輸入中的 URL**目的地 URL**發行設定檔 方塊**連接**索引標籤上，瀏覽器會自動開啟 Contoso 大學首頁，即可在本機電腦上執行於 IIS。</span><span class="sxs-lookup"><span data-stu-id="47b40-310">If you entered the URL in the **Destination URL** box on the publish profile **Connection** tab, the browser automatically opens to the Contoso University Home page running in IIS on the local computer.</span></span>

## <a name="test-in-the-test-environment"></a><span data-ttu-id="47b40-311">在測試環境中測試</span><span class="sxs-lookup"><span data-stu-id="47b40-311">Test in the test environment</span></span>

<span data-ttu-id="47b40-312">請注意，環境標記會顯示 「 （測試） 」 而不是 「 （開發） 」，其中顯示*Web.config*環境指標的轉換是否成功。</span><span class="sxs-lookup"><span data-stu-id="47b40-312">Notice that the environment indicator shows "(Test)" instead of "(Dev)", which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

<span data-ttu-id="47b40-313">執行**講師**頁面，即可確認 Code First 植入講師資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="47b40-313">Run the **Instructors** page to verify that Code First seeded the database with instructor data.</span></span> <span data-ttu-id="47b40-314">當您選取此頁面時，可能需要幾分鐘才能載入，因為第一個程式碼會建立資料庫，然後執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="47b40-314">When you select this page, it may take a few minutes to load because Code First creates the database and then runs the `Seed` method.</span></span> <span data-ttu-id="47b40-315">（它未執行作業，當應用程式沒有存取資料庫尚未嘗試過，因為已在首頁上時）。</span><span class="sxs-lookup"><span data-stu-id="47b40-315">(It didn't do that when you were on the home page because the application didn't try to access the database yet.)</span></span>

<span data-ttu-id="47b40-316">按一下**學生** 索引標籤，確認已部署的資料庫具有不學生。</span><span class="sxs-lookup"><span data-stu-id="47b40-316">Click the **Students** tab to verify that the deployed database has no students.</span></span>

<span data-ttu-id="47b40-317">選取**新增學生**從**學生**功能表上，新增一位學生，，然後再檢視中新的學生**學生**頁面，即可確認您可以成功地寫入至資料庫.</span><span class="sxs-lookup"><span data-stu-id="47b40-317">Select **Add Students** from the **Students** menu, add a student, and then view the new student in the **Students** page to verify that you can successfully write to the database.</span></span>

<span data-ttu-id="47b40-318">從**課程**功能表上，選取**更新信用額度**。</span><span class="sxs-lookup"><span data-stu-id="47b40-318">From the **Courses** menu, select **Update Credits**.</span></span> <span data-ttu-id="47b40-319">**更新信用額度**頁面需要系統管理員權限，所以**登入**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="47b40-319">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="47b40-320">輸入您建立更早 （"admin"、"devpwd"） 的系統管理員帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="47b40-320">Enter the administrator account credentials that you created earlier ("admin" and "devpwd").</span></span> <span data-ttu-id="47b40-321">**更新信用額度**頁面隨即顯示，以確認您在上一個教學課程中建立的系統管理員帳戶已正確地部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="47b40-321">The **Update Credits** page is displayed, which verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="47b40-322">確認*Elmah*資料夾存在於*c:\inetpub\wwwroot\ContosoUniversity*資料夾，並只在它的預留位置檔案。</span><span class="sxs-lookup"><span data-stu-id="47b40-322">Verify that an *Elmah* folder exists in the *c:\inetpub\wwwroot\ContosoUniversity* folder with only the placeholder file in it.</span></span>

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a><span data-ttu-id="47b40-323">檢閱自動 Web.config 變更的 Code First 移轉</span><span class="sxs-lookup"><span data-stu-id="47b40-323">Review the automatic Web.config changes for Code First Migrations</span></span>

<span data-ttu-id="47b40-324">開啟*Web.config*中部署的應用程式在檔案*C:\inetpub\wwwroot\ContosoUniversity* ，您可以查看其中部署程序 Code First 移轉來自動設定更新資料庫的最新版本。</span><span class="sxs-lookup"><span data-stu-id="47b40-324">Open the *Web.config* file in the deployed application at *C:\inetpub\wwwroot\ContosoUniversity* and you can see where the deployment process configured Code First Migrations to automatically update the database to the latest version.</span></span>

![](deploying-to-iis/_static/image21.png)

<span data-ttu-id="47b40-325">部署程序也會建立新的 Code First 移轉，專門用來更新資料庫結構描述的連接字串：</span><span class="sxs-lookup"><span data-stu-id="47b40-325">The deployment process also created a new connection string for Code First Migrations to use exclusively for updating the database schema:</span></span>

![Database_Publish 連接字串](deploying-to-iis/_static/image22.png)

<span data-ttu-id="47b40-327">這個額外的連接字串可讓您指定一個使用者帳戶的資料庫結構描述更新，以及不同的使用者帳戶存取應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="47b40-327">This additional connection string enables you to specify one user account for database schema updates, and a different user account for application data access.</span></span> <span data-ttu-id="47b40-328">例如，您可能會指派**db\_擁有者**Code First 移轉，角色和**db\_datareader**和**db\_datawriter**應用程式的角色。</span><span class="sxs-lookup"><span data-stu-id="47b40-328">For example, you could assign the **db\_owner** role to Code First Migrations, and **db\_datareader** and **db\_datawriter** roles to the application.</span></span> <span data-ttu-id="47b40-329">這是常見的深度防禦模式會防止應用程式無法變更資料庫結構描述中的潛在惡意程式碼。</span><span class="sxs-lookup"><span data-stu-id="47b40-329">This is a common defense-in-depth pattern that prevents potentially malicious code in the application from changing the database schema.</span></span> <span data-ttu-id="47b40-330">（例如，這可能會發生在成功的 SQL 資料隱碼攻擊。）此模式不會使用這些教學課程。</span><span class="sxs-lookup"><span data-stu-id="47b40-330">(For example, this might happen in a successful SQL injection attack.) This pattern is not used by these tutorials.</span></span> <span data-ttu-id="47b40-331">如果您想要實作此模式在您的案例中，您可以執行下列步驟來執行它：</span><span class="sxs-lookup"><span data-stu-id="47b40-331">If you want to implement this pattern in your scenario, you can do it by performing the following steps:</span></span>

1. <span data-ttu-id="47b40-332">在**設定** 索引標籤**發行 Web**精靈 中，輸入完整的資料庫結構描述更新權限與指定的使用者的連接字串，並清除**使用此連接字串在執行階段**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="47b40-332">In the **Settings** tab of the **Publish Web** wizard, enter the connection string that specifies a user with full database schema update permissions, and clear the **Use this connection string at runtime** check box.</span></span> <span data-ttu-id="47b40-333">在已部署 Web.config 檔案中，這會變成`DatabasePublish`連接字串。</span><span class="sxs-lookup"><span data-stu-id="47b40-333">In the deployed Web.config file, this becomes the `DatabasePublish` connection string.</span></span>
2. <span data-ttu-id="47b40-334">建立的連接字串，您想要在執行階段使用的應用程式的 Web.config 檔案轉換。</span><span class="sxs-lookup"><span data-stu-id="47b40-334">Create a Web.config file transformation for the connection string that you want the application to use at run time.</span></span>

## <a name="summary"></a><span data-ttu-id="47b40-335">總結</span><span class="sxs-lookup"><span data-stu-id="47b40-335">Summary</span></span>

<span data-ttu-id="47b40-336">您現在已在開發電腦部署到 IIS 應用程式，而且那里進行測試。</span><span class="sxs-lookup"><span data-stu-id="47b40-336">You have now deployed your application to IIS on your development computer and tested it there.</span></span>

![在測試中的 [首頁] 頁面](deploying-to-iis/_static/image23.png)

<span data-ttu-id="47b40-338">這會確認部署程序複製到正確的位置 （不含您不想要部署的檔案），應用程式的內容，也該 Web Deploy IIS 正確設定在部署期間。</span><span class="sxs-lookup"><span data-stu-id="47b40-338">This verifies that the deployment process copied the application's content to the right location (excluding the files that you did not want to deploy), and also that Web Deploy configured IIS correctly during deployment.</span></span> <span data-ttu-id="47b40-339">在下一個教學課程中，您將執行一項會尋找尚未完成的部署工作的多個測試： 設定資料夾的權限*Elmah*資料夾。</span><span class="sxs-lookup"><span data-stu-id="47b40-339">In the next tutorial, you'll run one more test that finds a deployment task that has not yet been done: setting folder permissions on the *Elmah* folder.</span></span>

## <a name="more-information"></a><span data-ttu-id="47b40-340">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="47b40-340">More information</span></span>

<span data-ttu-id="47b40-341">如需 Visual Studio 中執行 IIS 或 IIS Express，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="47b40-341">For information about running IIS or IIS Express in Visual Studio, see the following resources:</span></span>

- <span data-ttu-id="47b40-342">[IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="47b40-342">[IIS Express Overview](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) on the IIS.net site.</span></span>
- <span data-ttu-id="47b40-343">[導入 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="47b40-343">[Introducing IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) on Scott Guthrie's blog.</span></span>
- <span data-ttu-id="47b40-344">[Web 伺服器，在 Visual Studio 中的，ASP.NET Web 專案](https://msdn.microsoft.com/library/58wxa9w5.aspx)。</span><span class="sxs-lookup"><span data-stu-id="47b40-344">[Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>
- <span data-ttu-id="47b40-345">[核心 IIS 之間的差異和 ASP.NET 程式開發伺服器](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md)ASP.NET 網站上。</span><span class="sxs-lookup"><span data-stu-id="47b40-345">[Core Differences Between IIS and the ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) on the ASP.NET site.</span></span>

<span data-ttu-id="47b40-346">在中度信任執行的應用程式時，可能會引發哪些問題的相關資訊，請參閱[裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)4 Guy 從 Rolla 站台上。</span><span class="sxs-lookup"><span data-stu-id="47b40-346">For information about what issues might arise when your application runs in medium trust, see [Hosting ASP.NET Applications in Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) on the 4 Guys from Rolla site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="47b40-347">[上一頁](project-properties.md)
> [下一頁](setting-folder-permissions.md)</span><span class="sxs-lookup"><span data-stu-id="47b40-347">[Previous](project-properties.md)
[Next](setting-folder-permissions.md)</span></span>
