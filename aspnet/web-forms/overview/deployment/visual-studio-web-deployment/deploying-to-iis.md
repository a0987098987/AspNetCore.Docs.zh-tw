---
uid: web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
title: 使用 Visual Studio 的 ASP.NET Web 部署：部署到測試 |Microsoft Docs
author: tdykstra
description: 本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或協力廠商裝載提供者，使用...
ms.author: riande
ms.date: 01/16/2019
ms.assetid: 8bf2c4fb-4ee5-4841-bfc2-03462c1f7a7a
msc.legacyurl: /web-forms/overview/deployment/visual-studio-web-deployment/deploying-to-iis
msc.type: authoredcontent
ms.openlocfilehash: d49dfad368ca4b81bb865103a99ec223a1cc66df
ms.sourcegitcommit: 184ba5b44d1c393076015510ac842b77bc9d4d93
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2019
ms.locfileid: "54396333"
---
<a name="aspnet-web-deployment-using-visual-studio-deploying-to-test"></a><span data-ttu-id="a0cb1-103">使用 Visual Studio 的 ASP.NET Web 部署：部署到測試</span><span class="sxs-lookup"><span data-stu-id="a0cb1-103">ASP.NET Web Deployment using Visual Studio: Deploying to Test</span></span>
====================
<span data-ttu-id="a0cb1-104">藉由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="a0cb1-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="a0cb1-105">本系列教學課程會示範如何部署 （發行） 的 ASP.NET web 應用程式至 Azure App Service Web Apps 或使用 Visual Studio 2017 的協力廠商裝載提供者。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-105">This tutorial series shows you how to deploy (publish) an ASP.NET web application to Azure App Service Web Apps or to a third-party hosting provider using Visual Studio 2017.</span></span> <span data-ttu-id="a0cb1-106">這個系列的相關資訊，請參閱[系列的第一個教學課程](introduction.md)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-106">For information about the series, see [the first tutorial in the series](introduction.md).</span></span>

## <a name="overview"></a><span data-ttu-id="a0cb1-107">總覽</span><span class="sxs-lookup"><span data-stu-id="a0cb1-107">Overview</span></span>

<span data-ttu-id="a0cb1-108">在本教學課程中，您會將 ASP.NET web 應用程式 Internet Information Server (IIS) 來部署您的本機電腦上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-108">In this tutorial, you'll deploy an ASP.NET web application to Internet Information Server (IIS) on your local computer.</span></span>

<span data-ttu-id="a0cb1-109">通常當您開發應用程式時，執行並在 Visual Studio 中進行測試。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-109">Generally when you develop an application, you run it and test it in Visual Studio.</span></span> <span data-ttu-id="a0cb1-110">根據預設，Visual Studio 2017 中的 web 應用程式專案使用 IIS Express 做為開發 web 伺服器。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-110">By default, web application projects in Visual Studio 2017 use IIS Express as the development web server.</span></span> <span data-ttu-id="a0cb1-111">IIS Express 的行為更類似於 Visual Studio 程式開發伺服器 (也稱為 Cassini)，Visual Studio 2017 預設使用的完整 IIS。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-111">IIS Express behaves more like full IIS than the Visual Studio Development Server (also known as Cassini), which Visual Studio 2017 uses by default.</span></span> <span data-ttu-id="a0cb1-112">但是，開發 web 伺服器的運作方式完全類似 IIS。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-112">But neither development web server works exactly like IIS.</span></span> <span data-ttu-id="a0cb1-113">因此，應用程式無法執行並測試 Visual Studio 中的正確但失敗時將它部署至 IIS。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-113">Consequently, an app could run and test correctly in Visual Studio but fail when it's deployed to IIS.</span></span>

<span data-ttu-id="a0cb1-114">您可以可靠地測試您的應用程式，有兩種：</span><span class="sxs-lookup"><span data-stu-id="a0cb1-114">You can reliably test your application in two ways:</span></span>

1. <span data-ttu-id="a0cb1-115">應用程式部署至 IIS 使用相同的程序，您稍後將用來將它部署到生產環境的開發電腦上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-115">Deploy your application to IIS on your development computer using the same process that you'll use later to deploy it to your production environment.</span></span>

   <span data-ttu-id="a0cb1-116">您可以設定 Visual Studio 使用 IIS，當您執行 web 專案，但不會測試您的部署程序。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-116">You can configure Visual Studio to use IIS when you run a web project, but that wouldn't test your deployment process.</span></span> <span data-ttu-id="a0cb1-117">這個方法會驗證您的部署程序，和您的應用程式正確地在 IIS 下執行。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-117">This method validates your deployment process and that your application runs correctly under IIS.</span></span>

2. <span data-ttu-id="a0cb1-118">應用程式部署至實際執行環境類似的測試環境。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-118">Deploy your application to a test environment similar to your production environment.</span></span> 
 
   <span data-ttu-id="a0cb1-119">這些教學課程的實際執行環境是 Azure App Service 中的 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-119">The production environment for these tutorials is Web Apps in Azure App Service.</span></span> <span data-ttu-id="a0cb1-120">理想的測試環境是 Azure 服務中建立額外的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-120">The ideal test environment is an additional web app created in the Azure Service.</span></span> <span data-ttu-id="a0cb1-121">雖然它會設定為生產 web 應用程式的相同方式，您只會使用它進行測試。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-121">Though it would be set up the same way as a production web app, you would only use it for testing.</span></span>

<span data-ttu-id="a0cb1-122">選項 2 是測試的最可靠方式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-122">Option 2 is the most reliable way to test.</span></span> <span data-ttu-id="a0cb1-123">如果您使用選項 2，您不一定需要使用選項 1。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-123">If you use option 2, you don't necessarily need to use option 1.</span></span> <span data-ttu-id="a0cb1-124">不過如果您要部署的協力廠商裝載提供者，選項 2 可能不可行或可能耗費資源，因此本教學課程系列會顯示這兩種方法。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-124">However if you're deploying to a third-party hosting provider, option 2 might not be feasible or might be expensive, so this tutorial series shows both methods.</span></span> <span data-ttu-id="a0cb1-125">選項 2 的指導方針中提供[部署到生產環境](deploying-to-production.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-125">Guidance for option 2 is provided in the [Deploying to the Production Environment](deploying-to-production.md) tutorial.</span></span>

<span data-ttu-id="a0cb1-126">如需在 Visual Studio 中使用 web 伺服器的詳細資訊，請參閱[ASP.NET Web 專案的 Visual Studio 中的 Web 伺服器](https://msdn.microsoft.com/library/58wxa9w5.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-126">For more information about using web servers in Visual Studio, see [Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>

<span data-ttu-id="a0cb1-127">提醒：如果您收到錯誤訊息，或當您瀏覽本教學課程，項目無法運作，請務必檢查[疑難排解頁面](troubleshooting.md)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-127">Reminder: If you receive an error message or something doesn't work as you go through the tutorial, be sure to check the [troubleshooting page](troubleshooting.md).</span></span>

## <a name="download-the-contoso-university-starter-project"></a><span data-ttu-id="a0cb1-128">下載 Contoso 大學入門專案</span><span class="sxs-lookup"><span data-stu-id="a0cb1-128">Download the Contoso University starter project</span></span>

<span data-ttu-id="a0cb1-129">下載並安裝入門的 Contoso 大學的 Visual Studio 方案和專案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-129">Download and install the Contoso University Visual Studio starter solution and project.</span></span> <span data-ttu-id="a0cb1-130">此解決方案包含已完成本教學課程。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-130">This solution contains the completed tutorial.</span></span> 

[<span data-ttu-id="a0cb1-131">下載入門專案</span><span class="sxs-lookup"><span data-stu-id="a0cb1-131">Download Starter Project</span></span>](http://go.microsoft.com/fwlink/p/?LinkId=282627)

## <a name="install-iis"></a><span data-ttu-id="a0cb1-132">安裝 IIS</span><span class="sxs-lookup"><span data-stu-id="a0cb1-132">Install IIS</span></span>

<span data-ttu-id="a0cb1-133">若要部署至 IIS 的開發電腦上，確認已安裝 IIS 和 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-133">To deploy to IIS on your development computer, confirm that IIS and Web Deploy are installed.</span></span> <span data-ttu-id="a0cb1-134">根據預設，Visual Studio 會安裝 Web Deploy，但 IIS 不會包含於預設 Windows 10、windows 8 或 Windows 7 的組態。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-134">By default, Visual Studio installs Web Deploy, but IIS isn't included in the default Windows 10, Windows 8, or Windows 7 configuration.</span></span> <span data-ttu-id="a0cb1-135">如果您已經安裝 IIS 和預設應用程式集區已設為.NET 4，請跳至[下一節](#sqlexpress)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-135">If you've already installed IIS and the default application pool is already set to .NET 4, skip to [the next section](#sqlexpress).</span></span>

1. <span data-ttu-id="a0cb1-136">建議您使用[Web Platform Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx)安裝 IIS 和 Web Deploy。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-136">It's recommended you use the [Web Platform Installer (WPI)](https://www.microsoft.com/web/downloads/platform.aspx) to install IIS and Web Deploy.</span></span> <span data-ttu-id="a0cb1-137">WPI 安裝建議的 IIS 設定，如有必要，其中包含 IIS 和 Web Deploy 的必要條件。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-137">WPI installs a recommended IIS configuration that includes IIS and Web Deploy prerequisites if necessary.</span></span>

   <span data-ttu-id="a0cb1-138">如果您已經安裝 IIS、 Web Deploy，或任何及其必要元件，會安裝 WPI，只會遺失。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-138">If you've already installed IIS, Web Deploy, or any of their required components, the WPI installs only what is missing.</span></span>

   * <span data-ttu-id="a0cb1-139">您可以使用 Web Platform Installer 來安裝 IIS 和 Web Deploy:</span><span class="sxs-lookup"><span data-stu-id="a0cb1-139">Use the Web Platform Installer to install IIS and Web Deploy:</span></span>
    
     ![使用 WPI 安裝 IIS](deploying-to-iis/_static/image30.png)

     ![安裝 Web Deploy 使用 WPI](deploying-to-iis/_static/image31.png)

     <span data-ttu-id="a0cb1-142">您會看到訊息指出將會安裝 IIS 7。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-142">You'll see messages indicating that IIS 7 will be installed.</span></span> <span data-ttu-id="a0cb1-143">適用於 Windows 8; 中的 IIS 8 的連結但 Windows 8 和更新版本中，瀏覽下列步驟，請確定已安裝 ASP.NET 4.7:</span><span class="sxs-lookup"><span data-stu-id="a0cb1-143">The link works for IIS 8 in Windows 8; but for Windows 8 and later, go through the following steps to make sure that ASP.NET 4.7 is installed:</span></span>

   * <span data-ttu-id="a0cb1-144">開啟**控制台中** > **程式** > **程式和功能** > **開啟或關閉開啟的 Windows 功能**.</span><span class="sxs-lookup"><span data-stu-id="a0cb1-144">Open **Control Panel** > **Programs** > **Programs and Features** > **Turn Windows features on or off**.</span></span>

   * <span data-ttu-id="a0cb1-145">依序展開**Internet Information Services**， **World Wide Web 服務**，並**應用程式開發功能**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-145">Expand **Internet Information Services**, **World Wide Web Services**, and **Application Development Features**.</span></span>
   
   * <span data-ttu-id="a0cb1-146">確認**ASP.NET 4.7**已選取。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-146">Confirm that **ASP.NET 4.7** is selected.</span></span>

     ![選取 ASP.NET 4.7](deploying-to-iis/_static/image1a.png)

   * <span data-ttu-id="a0cb1-148">確認**World Wide Web 服務**並**IIS 管理主控台**已選取。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-148">Confirm that **World Wide Web Services** and **IIS Management Console** is selected.</span></span> <span data-ttu-id="a0cb1-149">這會安裝 IIS 和 IIS 管理員。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-149">This installs IIS and IIS Manager.</span></span>
    
     ![選取 World Wide Web 服務](deploying-to-iis/_static/image24.png)    
  
   * <span data-ttu-id="a0cb1-151">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-151">Select **OK**.</span></span> <span data-ttu-id="a0cb1-152">表示安裝正在進行對話方塊訊息會出現。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-152">Dialog box messages indicating installation is taking place appear.</span></span>

<span data-ttu-id="a0cb1-153">安裝 IIS 之後, 執行**IIS 管理員**先確定.NET Framework 第 4 版，都會指派給預設應用程式集區。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-153">After installing IIS, run **IIS Manager** to make sure that the .NET Framework version 4 is assigned to the default application pool.</span></span>

1. <span data-ttu-id="a0cb1-154">按 WINDOWS + R 來開啟**執行** 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-154">Press WINDOWS+R to open the **Run** dialog box.</span></span>

   <span data-ttu-id="a0cb1-155">(在 Windows 8 或更新版本中，輸入 「 執行 」**啟動**頁面。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-155">(On Windows 8 or later, enter "run" on the **Start** page.</span></span> <span data-ttu-id="a0cb1-156">在 Windows 7 中，選取**執行**從**開始**功能表。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-156">In Windows 7, select **Run** from the **Start** menu.</span></span> <span data-ttu-id="a0cb1-157">如果**執行**不在**開始**功能表上，以滑鼠右鍵按一下工作列中，選取**屬性**，選取**開始 功能表**索引標籤上，選取**自訂**，然後選取**執行命令**。)</span><span class="sxs-lookup"><span data-stu-id="a0cb1-157">If **Run** isn't in the **Start** menu, right-click the taskbar, select **Properties**, select the **Start Menu** tab, select **Customize**, and select **Run command**.)</span></span>

2. <span data-ttu-id="a0cb1-158">請輸入"inetmgr"並選取**確定**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-158">Enter "inetmgr" and select **OK**.</span></span>

3. <span data-ttu-id="a0cb1-159">在 **連線**窗格中，展開伺服器節點，然後選取**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-159">In the **Connections** pane, expand the server node and select **Application Pools**.</span></span> <span data-ttu-id="a0cb1-160">在 **應用程式集區**窗格如果**DefaultAppPool**是指派給.NET framework 第 4 版如如下圖所示，請跳至下一節。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-160">In the **Application Pools** pane if **DefaultAppPool** is assigned to the .NET framework version 4 as in the following illustration, skip to the next section.</span></span>

   ![Inetmgr_showing_4.0_app_pools](deploying-to-iis/_static/image5a.png)

4. <span data-ttu-id="a0cb1-162">如果您看到只有兩個應用程式集區，而且都設為.NET Framework 2.0，請在 IIS 中安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-162">If you see only two application pools and both are set to .NET Framework 2.0, install ASP.NET 4 in IIS.</span></span>

   <span data-ttu-id="a0cb1-163">適用於 Windows 8 或更新版本，請參閱上一節的指示，並確定該 ASP.NET 4.7 安裝，或請參閱[如何在 Windows 8 和 Windows Server 2012 上安裝 ASP.NET 4.5](https://support.microsoft.com/kb/2736284)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-163">For Windows 8 or later, see the previous section's instructions for making sure that ASP.NET 4.7 is installed or see [How to install ASP.NET 4.5 on Windows 8 and Windows Server 2012](https://support.microsoft.com/kb/2736284).</span></span> <span data-ttu-id="a0cb1-164">適用於 Windows 7，請以滑鼠右鍵按一下開啟命令提示字元視窗**命令提示字元**在 Windows 中**開始**功能表，然後選取**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-164">For Windows 7, open a command prompt window by right-clicking **Command Prompt** in the Windows **Start** menu and selecting **Run as Administrator**.</span></span> <span data-ttu-id="a0cb1-165">執行[aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx)若要使用下列命令在 IIS 中安裝 ASP.NET 4。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-165">Run [aspnet\_regiis.exe](https://msdn.microsoft.com/library/k6h9cz8h.aspx) to install ASP.NET 4 in IIS using the following commands.</span></span> <span data-ttu-id="a0cb1-166">（在 32 位元系統中，取代"Framework64 」 與 「 架構 」）。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-166">(In 32-bit systems, replace "Framework64" with "Framework".)</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample1.cmd)]

   <span data-ttu-id="a0cb1-167">此命令會建立新的應用程式集區的 .NET Framework 4 中，但預設應用程式集區會保留設為 2.0。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-167">This command creates new application pools for the .NET Framework 4, but the default application pool will remain set to 2.0.</span></span> <span data-ttu-id="a0cb1-168">您要部署到該應用程式集區的目標.NET 4，因此變更為.NET 4 應用程式集區的應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-168">You're deploying an application that targets .NET 4 to that application pool, so change the application pool to .NET 4.</span></span>

5. <span data-ttu-id="a0cb1-169">如果您關閉**IIS 管理員**，再執行一次，依序展開伺服器節點，然後選取**應用程式集區**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-169">If you closed **IIS Manager**, run it again, expand the server node, and select **Application Pools**.</span></span>

6. <span data-ttu-id="a0cb1-170">在 **應用程式集區**窗格中，選取**DefaultAppPool**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-170">In the **Application Pools** pane, select **DefaultAppPool**.</span></span> <span data-ttu-id="a0cb1-171">在 **動作**窗格中，選取**基本設定**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-171">In the **Actions** pane, select **Basic Settings**.</span></span>

   ![Inetmgr_selecting_Basic_Settings_for_app_pool](deploying-to-iis/_static/image25.png)

7. <span data-ttu-id="a0cb1-173">在 [**編輯應用程式集區**] 對話方塊中，變更 **.NET CLR 版本**來 **.NET CLR v4.0.30319**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-173">In the **Edit Application Pool** dialog box, change the **.NET CLR version** to **.NET CLR v4.0.30319**.</span></span> <span data-ttu-id="a0cb1-174">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-174">Select **OK**.</span></span>

   ![Selecting_.NET_4_for_DefaultAppPool](deploying-to-iis/_static/image6a.png)

<span data-ttu-id="a0cb1-176">您現在準備好要發行至 IIS 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-176">You're now ready to publish a web application to IIS.</span></span> <span data-ttu-id="a0cb1-177">首先，不過，建立用於測試的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-177">First, however, create databases for testing.</span></span>

<a id="sqlexpress"></a>

## <a name="install-sql-server-express"></a><span data-ttu-id="a0cb1-178">安裝 SQL Server Express</span><span class="sxs-lookup"><span data-stu-id="a0cb1-178">Install SQL Server Express</span></span>

<span data-ttu-id="a0cb1-179">LocalDB 不被設計用於在 IIS 中，因此您的測試環境必須具有安裝 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-179">LocalDB isn't designed to work in IIS, so your test environment has to have SQL Server Express installed.</span></span> <span data-ttu-id="a0cb1-180">如果您使用 Visual 的 Studio 2010 SQL Server Express，它是已安裝預設值。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-180">If you're using Visual Studio 2010 SQL Server Express, it's already installed by default.</span></span> <span data-ttu-id="a0cb1-181">如果您使用 Visual Studio 2012 或更新版本，安裝 SQL Server Express。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-181">If you're using Visual Studio 2012 or later, install SQL Server Express.</span></span>

<span data-ttu-id="a0cb1-182">若要安裝 SQL Server Express，請下載並安裝從[下載中心：Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-182">To install SQL Server Express, download and install it from [Download Center: Microsoft SQL Server 2017 Express edition](https://www.microsoft.com/sql-server/sql-server-editions-express).</span></span> 

<span data-ttu-id="a0cb1-183">在 SQL Server 安裝中心 的第一個頁面上，選取**新的 SQL Server 獨立安裝或將功能加入到現有安裝**並遵循指示接受預設選項。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-183">On the first page of the SQL Server Installation Center, select **New SQL Server stand-alone installation or add features to an existing installation** and follow the instructions accepting the default choices.</span></span> <span data-ttu-id="a0cb1-184">在安裝精靈中，接受預設設定。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-184">In the installation wizard, accept the default settings.</span></span> <span data-ttu-id="a0cb1-185">如需安裝選項的詳細資訊，請參閱[從安裝精靈 （安裝程式） 安裝 SQL Server](https://msdn.microsoft.com/library/ms143219.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-185">For more information about installation options, see [Install SQL Server from the Installation Wizard (Setup)](https://msdn.microsoft.com/library/ms143219.aspx).</span></span>

## <a name="create-sql-server-express-databases-for-the-test-environment"></a><span data-ttu-id="a0cb1-186">建立測試環境的 SQL Server Express 資料庫</span><span class="sxs-lookup"><span data-stu-id="a0cb1-186">Create SQL Server Express databases for the test environment</span></span>

<span data-ttu-id="a0cb1-187">Contoso 大學應用程式有兩個資料庫：</span><span class="sxs-lookup"><span data-stu-id="a0cb1-187">The Contoso University application has two databases:</span></span> 

1. <span data-ttu-id="a0cb1-188">成員資格資料庫</span><span class="sxs-lookup"><span data-stu-id="a0cb1-188">Membership database</span></span> 
2. <span data-ttu-id="a0cb1-189">應用程式資料庫</span><span class="sxs-lookup"><span data-stu-id="a0cb1-189">Application database</span></span> 

<span data-ttu-id="a0cb1-190">兩個不同的資料庫或單一資料庫，您可以部署這些資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-190">You can deploy these databases to two separate databases or to a single database.</span></span> <span data-ttu-id="a0cb1-191">結合它們可讓您更輕鬆兩者之間的資料庫聯結。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-191">Combining them makes database joins between them easier.</span></span> 

<span data-ttu-id="a0cb1-192">如果您要部署到協力廠商裝載提供者，主控方案可能也讓您將它們結合的原因。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-192">If you're deploying to a third-party hosting provider, your hosting plan might also give you a reason to combine them.</span></span> <span data-ttu-id="a0cb1-193">例如，提供者可能會因為多個資料庫的多個或甚至不可能會允許多個資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-193">For example, the provider might charge more for multiple databases or might not even allow more than one database.</span></span>

<span data-ttu-id="a0cb1-194">在本教學課程中，您會將部署到測試環境中的兩個資料庫和在預備與生產環境中的一個資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-194">In this tutorial, you'll deploy to two databases in the test environment and to one database in the staging and production environments.</span></span>

<span data-ttu-id="a0cb1-195">從**檢視**功能表，在 Visual Studio 中，選取**伺服器總管**(**資料庫總管**Visual Web Developer 中)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-195">From the **View** menu in Visual Studio, select **Server Explorer** (**Database Explorer** in Visual Web Developer).</span></span> <span data-ttu-id="a0cb1-196">以滑鼠右鍵按一下**資料連接**，然後選取**建立新的 SQL Server 資料庫**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-196">Right-click **Data Connections** and select **Create New SQL Server Database**.</span></span>

![Selecting_Create_New_SQL_Server_Database](deploying-to-iis/_static/image8.png)

<span data-ttu-id="a0cb1-198">在**建立新的 SQL Server 資料庫**對話方塊方塊中，輸入 「。 \SQLExpress"中**伺服器名稱** 方塊中，"aspnet 集 ContosoUniversity"中**新的資料庫名稱** 方塊中。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-198">In the **Create New SQL Server Database** dialog box, enter ".\SQLExpress" in the **Server name** box and "aspnet-ContosoUniversity" in the **New database name** box.</span></span> <span data-ttu-id="a0cb1-199">選取 [確定]。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-199">Select **OK**.</span></span>

![建立 aspnet ContosoUniversity](deploying-to-iis/_static/image9.png)

<span data-ttu-id="a0cb1-201">請遵循相同的程序，以建立新的 SQL Server Express School 資料庫，名為`ContosoUniversity`。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-201">Follow the same procedure to create a new SQL Server Express School database named `ContosoUniversity`.</span></span>

<span data-ttu-id="a0cb1-202">**伺服器總管**顯示兩個新的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-202">**Server Explorer** shows the two new databases.</span></span>

![在 [伺服器總管] 中的新資料庫](deploying-to-iis/_static/image10.png)

## <a name="create-a-grant-script-for-the-new-databases"></a><span data-ttu-id="a0cb1-204">建立新的資料庫授與指令碼</span><span class="sxs-lookup"><span data-stu-id="a0cb1-204">Create a grant script for the new databases</span></span>

<span data-ttu-id="a0cb1-205">當您開發電腦上，在 IIS 中執行應用程式時，應用程式會使用預設應用程式集區的認證來存取資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-205">When the application runs in IIS on your development computer, the application uses the default application pool's credentials to access the database.</span></span> <span data-ttu-id="a0cb1-206">不過，根據預設，應用程式集區不具有開啟資料庫的權限。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-206">However, by default, the application pool doesn't have permission to open the databases.</span></span> <span data-ttu-id="a0cb1-207">這表示您需要執行指令碼，以授與該權限。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-207">This means you need to run a script to grant that permission.</span></span> <span data-ttu-id="a0cb1-208">在本節中，您會建立該指令碼，並執行更新版本，以確定應用程式在執行於 IIS 時，可以開啟資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-208">In this section, you'll create that script and run it later to make sure that the application can open the databases when it runs in IIS.</span></span>

<span data-ttu-id="a0cb1-209">在文字編輯器中，請將下列 SQL 命令複製到新的檔案，並將它儲存成*Grant.sql*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-209">In a text editor, copy the following SQL commands into a new file and save it as *Grant.sql*.</span></span> 

[!code-sql[Main](deploying-to-iis/samples/sample2.sql)]

<span data-ttu-id="a0cb1-210">在 Visual Studio 中，開啟 Contoso 大學的解決方案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-210">In Visual Studio, open the Contoso University solution.</span></span> <span data-ttu-id="a0cb1-211">以滑鼠右鍵按一下方案 （不是其中一個專案），然後選取**新增**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-211">Right-click the solution (not one of the projects), and select **Add**.</span></span> <span data-ttu-id="a0cb1-212">選取 **現有的項目**，瀏覽至*Grant.sql*，並將它開啟。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-212">Select **Existing Item**, browse to *Grant.sql*, and open it.</span></span>

> [!NOTE]
> <span data-ttu-id="a0cb1-213">此指令碼被設計給使用 SQL Server Express 2012 或更新版本，並在 Windows 10、windows 8 或 Windows 7 中的 IIS 設定，因為它們會指定在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-213">This script is designed to work with SQL Server Express 2012 or later and with the IIS settings in Windows 10, Windows 8, or Windows 7 as they are specified in this tutorial.</span></span> <span data-ttu-id="a0cb1-214">如果您使用不同版本的 SQL Server 或 Windows，或如果您將 IIS 設定您的電腦上以不同的方式，可能必須使用這個指令碼的變更。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-214">If you're using a different version of SQL Server or Windows, or if you set up IIS on your computer differently, changes to this script might be required.</span></span> <span data-ttu-id="a0cb1-215">如需有關 SQL Server 指令碼的詳細資訊，請參閱 < [SQL Server 線上叢書 》](https://go.microsoft.com/fwlink/?LinkId=132511)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-215">For more information about SQL Server scripts, see [SQL Server Books Online](https://go.microsoft.com/fwlink/?LinkId=132511).</span></span>

> [!NOTE] 
> <span data-ttu-id="a0cb1-216">**安全性注意事項**這段指令碼提供`db_owner`在執行階段，也就是您會有在生產環境中存取資料庫之使用者的權限。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-216">**Security Note** This script gives `db_owner` permissions to the user that accesses the database at run time, which is what you'll have in the production environment.</span></span> <span data-ttu-id="a0cb1-217">在某些情況下，您可能想要指定具有完整的資料庫結構描述更新的權限只部署，並指定執行階段不同的使用者具有權限才能讀取和寫入資料的使用者。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-217">In some scenarios, you might want to specify a user that has full database schema update permissions only for deployment and specify for run time a different user that has permissions only to read and write data.</span></span> <span data-ttu-id="a0cb1-218">如需詳細資訊，請參閱 <<c0> [ 檢閱 Code First 移轉自動 Web.config 變更](#reviewingmigrations)稍後在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-218">For more information, see [Reviewing the Automatic Web.config Changes for Code First Migrations](#reviewingmigrations) later in this tutorial.</span></span>

<a id="publish"></a>

## <a name="run-the-grant-script-in-the-application-database"></a><span data-ttu-id="a0cb1-219">執行應用程式資料庫中的 授與指令碼</span><span class="sxs-lookup"><span data-stu-id="a0cb1-219">Run the grant script in the application database</span></span>

<span data-ttu-id="a0cb1-220">您可以設定執行指令碼授與成員資格資料庫中，在部署期間因為該資料庫的部署使用的發行設定檔[dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing)提供者。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-220">You can configure the publish profile to run the grant script in the membership database during deployment because that database deployment uses the [dbDacFx](https://docs.microsoft.com/iis/publish/using-web-deploy/dbdacfx-provider-for-incremental-database-publishing) provider.</span></span> <span data-ttu-id="a0cb1-221">您無法執行指令碼，Code First 移轉在部署期間，也就是您要部署的應用程式資料庫的方式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-221">You can't run scripts during Code First Migrations deployment, which is how you're deploying the application database.</span></span> <span data-ttu-id="a0cb1-222">這表示您不必以手動方式執行應用程式資料庫中的 部署前的指令碼。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-222">This means you have to manually run the script before deployment in the application database.</span></span>

1. <span data-ttu-id="a0cb1-223">在 Visual Studio 中開啟*Grant.sql*您稍早建立的檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-223">In Visual Studio, open the *Grant.sql* file that you created earlier.</span></span>

2. <span data-ttu-id="a0cb1-224">選取 **連線**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-224">Select **Connect**.</span></span> 

    ![[連線] 按鈕](deploying-to-iis/_static/image11.png)

3. <span data-ttu-id="a0cb1-226">在 **連接到伺服器**對話方塊方塊中，輸入 *。 \SQLExpress*作為**伺服器名稱**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-226">In the **Connect to Server** dialog box, enter *.\SQLExpress* as the **Server Name**.</span></span> <span data-ttu-id="a0cb1-227">選取 **連線**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-227">Select **Connect**.</span></span>

4. <span data-ttu-id="a0cb1-228">在 [資料庫] 下拉式清單中選取**ContosoUniversity**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-228">In the database drop-down list select **ContosoUniversity**.</span></span> <span data-ttu-id="a0cb1-229">選取 **執行**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-229">Select **Execute**.</span></span> 

   ![](deploying-to-iis/_static/image12.png)

<span data-ttu-id="a0cb1-230">預設應用程式集區識別現在具有足夠的權限，在 Code First 移轉來建立資料庫資料表，當應用程式的應用程式資料庫中。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-230">The default application pool identity now has sufficient permissions in the application database for Code First Migrations to create the database tables when the application runs.</span></span>

## <a name="publish-to-iis"></a><span data-ttu-id="a0cb1-231">發佈至 IIS</span><span class="sxs-lookup"><span data-stu-id="a0cb1-231">Publish to IIS</span></span>

<span data-ttu-id="a0cb1-232">有數種方式，您可以部署到使用 Visual Studio 和 Web Deploy 的 IIS:</span><span class="sxs-lookup"><span data-stu-id="a0cb1-232">There are several ways you can deploy to IIS using Visual Studio and Web Deploy:</span></span>

* <span data-ttu-id="a0cb1-233">使用單鍵發行的 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-233">Use Visual Studio one-click publish.</span></span>
* <span data-ttu-id="a0cb1-234">從命令列發佈。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-234">Publish from the command line.</span></span>
* <span data-ttu-id="a0cb1-235">建立部署套件，並使用 IIS 管理員進行安裝。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-235">Create a deployment package and install it using IIS Manager.</span></span> <span data-ttu-id="a0cb1-236">封裝具有的所有檔案和在 IIS 中安裝站台所需的中繼資料的.zip 檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-236">The package has a .zip file with all the files and metadata required to install a site in IIS.</span></span>
* <span data-ttu-id="a0cb1-237">建立部署套件，然後使用命令列進行安裝。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-237">Create a deployment package and install it using the command line.</span></span>

<span data-ttu-id="a0cb1-238">您已執行在先前的教學課程，以設定 Visual Studio 來自動化部署工作適用於所有這些方法的程序。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-238">The process you went through in the previous tutorials to set up Visual Studio to automate deployment tasks applies to all of these methods.</span></span> <span data-ttu-id="a0cb1-239">在這些教學課程中，您將使用的前兩個方法。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-239">In these tutorials, you'll use the first two methods.</span></span> <span data-ttu-id="a0cb1-240">使用部署套件的相關資訊，請參閱[部署 web 應用程式建立及安裝 web 部署套件](https://go.microsoft.com/fwlink/p/?LinkId=282413#package)Visual Studio 及 ASP.NET 的 Web 部署內容對應中。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-240">For information about using deployment packages, see [Deploying a web application by creating and installing a web deployment package](https://go.microsoft.com/fwlink/p/?LinkId=282413#package) in the Web Deployment Content Map for Visual Studio and ASP.NET.</span></span>

<span data-ttu-id="a0cb1-241">在發行之前，請確定您系統管理員模式中執行 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-241">Before publishing, make sure that you're running Visual Studio in administrator mode.</span></span> <span data-ttu-id="a0cb1-242">如果您沒有看到 **（系統管理員）** 在標題列中，關閉 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-242">If you don't see **(Administrator)** in the title bar, close Visual Studio.</span></span> <span data-ttu-id="a0cb1-243">在 Windows 8 （或更新版本）**開始**網頁或 Windows 7**開始**功能表上，以滑鼠右鍵按一下 Visual Studio 圖示，然後選取**系統管理員身分執行**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-243">In the Windows 8 (or later) **Start** page or the Windows 7 **Start** menu, right-click the Visual Studio icon and select **Run as Administrator**.</span></span> <span data-ttu-id="a0cb1-244">系統管理員模式只會發行當您要在本機電腦發行至 IIS 時所需。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-244">Administrator mode is only required for publishing when you're publishing to IIS on the local computer.</span></span>

### <a name="create-the-publish-profile"></a><span data-ttu-id="a0cb1-245">建立發行設定檔</span><span class="sxs-lookup"><span data-stu-id="a0cb1-245">Create the publish profile</span></span>

1. <span data-ttu-id="a0cb1-246">在 **方案總管**，以滑鼠右鍵按一下**ContosoUniversity**專案 (不**ContosoUniversity.DAL**專案)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-246">In **Solution Explorer**, right-click the **ContosoUniversity** project (not the **ContosoUniversity.DAL** project).</span></span> <span data-ttu-id="a0cb1-247">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-247">Select **Publish**.</span></span> <span data-ttu-id="a0cb1-248">**發佈**頁面隨即出現。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-248">The **Publish** page appears.</span></span>

2. <span data-ttu-id="a0cb1-249">選取 **新的設定檔**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-249">Select **New Profile**.</span></span> <span data-ttu-id="a0cb1-250">**挑選發行目標** 對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-250">The **Pick a publish target** dialog box appears.</span></span>

3. <span data-ttu-id="a0cb1-251">選取  **IIS、 FTP 等**。選取 建立設定檔。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-251">Select **IIS, FTP, etc**. Select **Create Profile**.</span></span> <span data-ttu-id="a0cb1-252">**發佈** 精靈隨即出現。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-252">The **Publish** wizard appears.</span></span>

   ![發佈 Web 精靈設定檔 索引標籤](deploying-to-iis/_static/image26.png)

4. <span data-ttu-id="a0cb1-254">從**發行方法**下拉式選單中，選取**Web Deploy**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-254">From the **Publish method** drop-down menu, select **Web Deploy**.</span></span>

5. <span data-ttu-id="a0cb1-255">針對**伺服器**，輸入*localhost*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-255">For **Server**, enter *localhost*.</span></span>

6. <span data-ttu-id="a0cb1-256">針對**站台名稱**，輸入*Default Web Site/ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-256">For **Site name**, enter *Default Web Site/ContosoUniversity*.</span></span>

7. <span data-ttu-id="a0cb1-257">針對**目的地 URL**，輸入*http://localhost/ContosoUniversity*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-257">For **Destination URL**, enter *http://localhost/ContosoUniversity*.</span></span>

   <span data-ttu-id="a0cb1-258">**目的地 URL**設定不需要。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-258">The **Destination URL** setting isn't required.</span></span> <span data-ttu-id="a0cb1-259">完成 Visual Studio 部署應用程式，它會自動開啟預設瀏覽器對這個 URL。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-259">When Visual Studio finishes deploying the application, it automatically opens your default browser to this URL.</span></span> <span data-ttu-id="a0cb1-260">如果您不想要在部署後自動開啟瀏覽器，將此方塊保留空白。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-260">If you don't want the browser to open automatically after deployment, leave this box blank.</span></span>

8. <span data-ttu-id="a0cb1-261">選取 **驗證連線**確認設定是否正確，而且您可以在本機電腦上連接到 IIS。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-261">Select **Validate Connection** to verify that the settings are correct and you can connect to IIS on the local computer.</span></span>

   <span data-ttu-id="a0cb1-262">綠色的核取記號驗證連線成功。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-262">A green check mark verifies that the connection is successful.</span></span>

   ![發佈 Web 精靈連線 索引標籤](deploying-to-iis/_static/image27.png)

9. <span data-ttu-id="a0cb1-264">選取 [**下一步**前往**設定**] 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-264">Select **Next** to advance to the **Settings** tab.</span></span>

10. <span data-ttu-id="a0cb1-265">**組態**下拉式清單方塊中指定的組建組態來部署。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-265">The **Configuration** drop-down box specifies the build configuration to deploy.</span></span> <span data-ttu-id="a0cb1-266">將它設定為預設值**發行**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-266">Leave it set to the default value of **Release**.</span></span> <span data-ttu-id="a0cb1-267">您不會在本教學課程部署偵錯組建。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-267">You won't be deploying Debug builds in this tutorial.</span></span>

11. <span data-ttu-id="a0cb1-268">依序展開**檔案發佈選項**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-268">Expand **File Publish Options**.</span></span> <span data-ttu-id="a0cb1-269">選取 **排除應用程式中的檔案\_Data 資料夾**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-269">Select **Exclude files from the App\_Data folder**.</span></span>

    <span data-ttu-id="a0cb1-270">在測試環境中，應用程式存取您建立本機 SQL Server Express 執行個體中，非.mdf 檔案中的資料庫*應用程式\_資料*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-270">In the test environment, the application accesses the databases that you created in the local SQL Server Express instance, not the .mdf files in the *App\_Data* folder.</span></span>

12. <span data-ttu-id="a0cb1-271">離開**發行期間預先編譯**並**移除目的地上的其他檔案**清除核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-271">Leave the **Precompile during publishing** and **Remove additional files at destination** check boxes cleared.</span></span>

    ![在 設定 索引標籤的 檔案發行選項](deploying-to-iis/_static/image15a.png)

    <span data-ttu-id="a0cb1-273">先行編譯是主要的大型網站很有用的選項。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-273">Precompiling is an option that is useful mainly for large sites.</span></span> <span data-ttu-id="a0cb1-274">它可以減少啟動時間發行網站之後，要求頁面的第一次。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-274">It can reduce startup time the first time a page is requested after the site is published.</span></span>

    <span data-ttu-id="a0cb1-275">您不需要移除其他檔案，因為這是您第一次部署，而且不會有任何檔案的目的地資料夾中尚未。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-275">You don't need to remove additional files since this is your first deployment and there won't be any files in the destination folder yet.</span></span>

    > [!NOTE] 
    > <span data-ttu-id="a0cb1-276">如果您選取**移除目的地上的其他檔案**後續部署到相同的站台，請確定您使用的是預覽功能，以便您看到事先將刪除的檔案，然後再部署。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-276">If you select **Remove additional files at destination** for a subsequent deployment to the same site, make sure that you use the preview feature so that you see in advance which files will be deleted before you deploy.</span></span> <span data-ttu-id="a0cb1-277">預期的行為是，Web Deploy 將會刪除您已刪除您的專案中的目的地伺服器上的檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-277">The expected behavior is that Web Deploy will delete files on the destination server that you have deleted in your project.</span></span> <span data-ttu-id="a0cb1-278">不過，比較來源和目的地資料夾下的整個資料夾結構;並在某些情況下，在 Web 部署可能會刪除您不想要刪除的檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-278">However, the entire folder structure under the source and destination folders is compared; and in some scenarios, Web Deploy might delete files you don't want to delete.</span></span>
    > 
    > <span data-ttu-id="a0cb1-279">比方說如果當您部署專案的根資料夾時，您可以有在伺服器上的子資料夾中的 web 應用程式，將會刪除子資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-279">For example if you have a web application in a subfolder on the server when you deploy a project to the root folder, the subfolder will be deleted.</span></span> <span data-ttu-id="a0cb1-280">您可能必須在 contoso.com 的主要站台的一個專案和 contoso.com/blog 的部落格的另一個專案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-280">You might have one project for the main site at contoso.com and another project for a blog at contoso.com/blog.</span></span> <span data-ttu-id="a0cb1-281">部落格應用程式是在子資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-281">The blog application is in a subfolder.</span></span> <span data-ttu-id="a0cb1-282">如果您選取**移除目的地上的其他檔案**部落格應用程式部署時的主要站台，將會刪除。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-282">If you select **Remove additional files at destination** when you deploy the main site, the blog application will be deleted.</span></span>
    > 
    > <span data-ttu-id="a0cb1-283">如需其他範例，您的應用程式\_資料資料夾可能會意外刪除。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-283">For another example, your App\_Data folder might get deleted unexpectedly.</span></span> <span data-ttu-id="a0cb1-284">特定的資料庫，例如 SQL Server Compact 資料庫檔案儲存在應用程式\_Data 資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-284">Certain databases such as SQL Server Compact store database files in the App\_Data folder.</span></span> <span data-ttu-id="a0cb1-285">初始部署之後，您不想要保留在後續的部署中，複製資料庫檔案，因此您選取**排除應用程式\_資料**[封裝/發行 Web] 索引標籤。這麼做之後，如果您之後**移除其他檔案目的地**選取您的資料庫檔案和應用程式\_資料資料夾本身將會刪除您所發行下一次。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-285">After the initial deployment, you don't want to keep copying the database files in subsequent deployments, so you select  **Exclude App\_Data** on the Package/Publish Web tab. After you do that if you have **Remove additional files at destination** selected, your database files and the App\_Data folder itself will be deleted the next time you publish.</span></span>

### <a name="configure-deployment-for-the-membership-database"></a><span data-ttu-id="a0cb1-286">設定成員資格資料庫的部署</span><span class="sxs-lookup"><span data-stu-id="a0cb1-286">Configure deployment for the membership database</span></span>

<span data-ttu-id="a0cb1-287">下列步驟適用於**DefaultConnection**在對話方塊中的資料庫**資料庫**一節。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-287">The following steps apply to the **DefaultConnection** database in the dialog box's **Databases** section.</span></span>

1. <span data-ttu-id="a0cb1-288">在 **遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-288">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express membership database.</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample3.cmd)]

   <span data-ttu-id="a0cb1-289">部署程序會將已部署的 Web.config 檔案中此連接字串因為**在執行階段使用此連接字串**已選取。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-289">The deployment process puts this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

    <span data-ttu-id="a0cb1-290">您也可以取得的連接字串**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-290">You can also get the connection string from **Server Explorer**.</span></span> <span data-ttu-id="a0cb1-291">在 [**伺服器總管**，展開**資料連接**，然後選取 **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity**資料庫，然後從**屬性**] 視窗複製**連接字串**值。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-291">In **Server Explorer**, expand **Data Connections** and select the **&lt;machinename&gt;\sqlexpress.aspnet-ContosoUniversity** database, then from the **Properties** window copy the **Connection String** value.</span></span> <span data-ttu-id="a0cb1-292">連接字串會有一個額外的設定，您可以將其刪除： `Pooling=False`。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-292">That connection string will have one additional setting that you can delete: `Pooling=False`.</span></span>

2. <span data-ttu-id="a0cb1-293">選取 **更新資料庫**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-293">Select **Update database**.</span></span>

   <span data-ttu-id="a0cb1-294">這會導致在部署期間，目的地資料庫中建立的資料庫結構描述。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-294">This causes the database schema to be created in the destination database during deployment.</span></span> <span data-ttu-id="a0cb1-295">您可以在下一個步驟中，指定您需要執行其他指令碼： 一個用來授與資料庫存取權的預設應用程式集區，另一個用於部署的資料。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-295">In next steps, you specify the additional scripts that you need to run: one to grant database access to the default application pool and one to deploy data.</span></span>

3. <span data-ttu-id="a0cb1-296">選取 **設定資料庫更新**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-296">Select **Configure database updates**.</span></span>
 
4. <span data-ttu-id="a0cb1-297">在 **設定資料庫更新**對話方塊中，選取**新增 SQL 指令碼**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-297">In the **Configure Database Updates** dialog box, select **Add SQL Script**.</span></span> <span data-ttu-id="a0cb1-298">瀏覽至*Grant.sql*您稍早儲存的方案資料夾中的指令碼。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-298">Navigate to the *Grant.sql* script that you saved earlier in the solution folder.</span></span>

5. <span data-ttu-id="a0cb1-299">重複此程序加入*aspnet-資料-dev.sql*指令碼。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-299">Repeat the process to add the *aspnet-data-dev.sql* script.</span></span>

   ![設定成員資格資料庫的資料庫更新](deploying-to-iis/_static/image16.png)

6. <span data-ttu-id="a0cb1-301">選取 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-301">Select **Close**.</span></span>

### <a name="configure-deployment-for-the-application-database"></a><span data-ttu-id="a0cb1-302">設定應用程式資料庫的部署</span><span class="sxs-lookup"><span data-stu-id="a0cb1-302">Configure deployment for the application database</span></span>

<span data-ttu-id="a0cb1-303">當 Visual Studio 偵測到 Entity Framework`DbContext`類別，它會建立中的項目**資料庫**區段，其中含有**Execute Code First Migrations**核取方塊，而不是**更新資料庫**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-303">When Visual Studio detects an Entity Framework `DbContext` class, it creates an entry in the **Databases** section that has an **Execute Code First Migrations** check box instead of an **Update Database** check box.</span></span> <span data-ttu-id="a0cb1-304">本教學課程中，您將使用該核取方塊來指定 Code First 移轉部署。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-304">For this tutorial, you'll use that check box to specify Code First Migrations deployment.</span></span>

<span data-ttu-id="a0cb1-305">在某些情況下，您可能會使用`DbContext`資料庫，但您想要的 dbDacFx 提供者而非移轉，來部署資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-305">In some scenarios, you might be using a `DbContext` database but you want to use the dbDacFx provider instead of Migrations to deploy the database.</span></span> <span data-ttu-id="a0cb1-306">在此情況下，請參閱 <<c0> [ 如何部署沒有移轉的 Code First 資料庫？](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations)在 MSDN 上 「 ASP.NET Web 部署常見問題集。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-306">In that case, see [How do I deploy a Code First database without Migrations?](https://msdn.microsoft.com/library/ee942158.aspx#deploy_code_first_without_migrations) in the ASP.NET Web Deployment FAQ on MSDN.</span></span>

<span data-ttu-id="a0cb1-307">下列步驟適用於**SchoolContext**在對話方塊中的資料庫**資料庫**一節。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-307">The following steps apply to the **SchoolContext** database in the dialog box's **Databases** section.</span></span>

1. <span data-ttu-id="a0cb1-308">在 **遠端的連接字串**方塊中，輸入下列連接字串指向新的 SQL Server Express 應用程式資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-308">In the **Remote connection string** box, enter the following connection string that points to the new SQL Server Express application database.</span></span>

   [!code-console[Main](deploying-to-iis/samples/sample4.cmd)]

   <span data-ttu-id="a0cb1-309">部署程序會將已部署的 Web.config 檔案中此連接字串因為**在執行階段使用此連接字串**已選取。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-309">The deployment process puts this connection string in the deployed Web.config file because **Use this connection string at runtime** is selected.</span></span>

   <span data-ttu-id="a0cb1-310">您也可以取得的應用程式資料庫連接字串**伺服器總管**相同的方式取得成員資格資料庫連接字串。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-310">You can also get the application database connection string from **Server Explorer** in the same way you got the membership database connection string.</span></span>

2. <span data-ttu-id="a0cb1-311">選取  **Execute Code First Migrations （在應用程式啟動時執行）**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-311">Select **Execute Code First Migrations (runs on application start)**.</span></span>

   <span data-ttu-id="a0cb1-312">這個選項會讓部署程序來設定已部署的 Web.config 檔來指定`MigrateDatabaseToLatestVersion`初始設定式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-312">This option causes the deployment process to configure the deployed Web.config file to specify the `MigrateDatabaseToLatestVersion` initializer.</span></span> <span data-ttu-id="a0cb1-313">應用程式部署之後第一次存取資料庫時，此初始設定式會將資料庫自動更新為最新版本。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-313">This initializer automatically updates the database to the latest version when the application accesses the database for the first time after deployment.</span></span>

### <a name="configure-publish-profile-transforms"></a><span data-ttu-id="a0cb1-314">設定發行設定檔轉換</span><span class="sxs-lookup"><span data-stu-id="a0cb1-314">Configure publish profile transforms</span></span>

1. <span data-ttu-id="a0cb1-315">選取 **關閉**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-315">Select **Close**.</span></span> <span data-ttu-id="a0cb1-316">選取 **是**當詢問您是否要儲存變更。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-316">Select **Yes** when you are asked if you want to save changes.</span></span>

2. <span data-ttu-id="a0cb1-317">在 **方案總管**，展開**屬性**，展開**PublishProfiles**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-317">In **Solution Explorer**, expand **Properties**, expand **PublishProfiles**.</span></span>

3. <span data-ttu-id="a0cb1-318">以滑鼠右鍵按一下*CustomProfile.pubxml*並將它重新命名*Test.pubxml*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-318">Right-click *CustomProfile.pubxml* and rename it *Test.pubxml*.</span></span>

4. <span data-ttu-id="a0cb1-319">以滑鼠右鍵按一下*Test.pubxml*。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-319">Right-click *Test.pubxml*.</span></span> <span data-ttu-id="a0cb1-320">選取 **新增設定轉換**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-320">Select **Add Config Transform**.</span></span>

   ![新增組態轉換功能表](deploying-to-iis/_static/image17.png)

   <span data-ttu-id="a0cb1-322">Visual Studio 會建立*Web.Test.config*轉換檔案並開啟它。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-322">Visual Studio creates the *Web.Test.config* transform file and opens it.</span></span>

5. <span data-ttu-id="a0cb1-323">在  *Web.Test.config*轉換檔中，開啟之後，立即插入下列程式碼組態標記。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-323">In the *Web.Test.config* transform file, insert the following code immediately after the opening configuration tag.</span></span>

   [!code-xml[Main](deploying-to-iis/samples/sample5.xml)]

    <span data-ttu-id="a0cb1-324">當您使用測試發行設定檔時，這項轉換會設定"Test"的環境標記。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-324">When you use the Test publish profile, this transform sets the environment indicator to "Test".</span></span> <span data-ttu-id="a0cb1-325">在已部署的網站中，您會看到 「 （測試） 」 之後的"Contoso University"H1 標題。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-325">In the deployed site, you'll see "(Test)" after the "Contoso University" H1 heading.</span></span>

6. <span data-ttu-id="a0cb1-326">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-326">Save and close the file.</span></span>

7. <span data-ttu-id="a0cb1-327">以滑鼠右鍵按一下*Web.Test.config*檔案，然後選取**預覽轉換**藉此確定您編寫的轉換會產生預期的變更。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-327">Right-click the *Web.Test.config* file and select **Preview Transform** to make sure that the transform you coded produces the expected changes.</span></span>

    <span data-ttu-id="a0cb1-328">**Web.config 預覽** 視窗會顯示結果的同時套用*Web.Release.config*轉換並*Web.Test.config*轉換。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-328">The **Web.config Preview** window shows the result of applying both the *Web.Release.config* transforms and the *Web.Test.config* transforms.</span></span>

### <a name="preview-the-deployment-updates"></a><span data-ttu-id="a0cb1-329">預覽部署更新</span><span class="sxs-lookup"><span data-stu-id="a0cb1-329">Preview the deployment updates</span></span>

1. <span data-ttu-id="a0cb1-330">開啟**發佈 Web**精靈一次 (以滑鼠右鍵按一下 ContosoUniversity 專案中，選取**發佈**，然後**預覽**)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-330">Open the **Publish Web** wizard again (right-click the ContosoUniversity project, select **Publish**, then **Preview**).</span></span>

2. <span data-ttu-id="a0cb1-331">在  **Preview**對話方塊中，選取**開始預覽**若要查看將複製的檔案清單。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-331">In the **Preview** dialog box, select **Start Preview** to see a list of the files that will be copied.</span></span> 

   ![發行預覽](deploying-to-iis/_static/image29.png)

   <span data-ttu-id="a0cb1-333">您也可以選取**預覽資料庫**連結來查看將在成員資格資料庫中執行的指令碼。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-333">You can also select the **Preview database** link to see the scripts that will run in the membership database.</span></span> <span data-ttu-id="a0cb1-334">（任何指令碼會不執行 Code First 移轉部署，因此沒有任何項目預覽應用程式資料庫）。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-334">(No scripts are run for Code First Migrations deployment, so there's nothing to preview for the application database.)</span></span>

3. <span data-ttu-id="a0cb1-335">選取 [發行]。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-335">Select **Publish**.</span></span>

   <span data-ttu-id="a0cb1-336">如果 Visual Studio 不在系統管理員模式中，您可能會收到權限錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-336">If Visual Studio is not in administrator mode, you might get a permissions error message.</span></span> <span data-ttu-id="a0cb1-337">在此情況下，關閉 Visual Studio，在系統管理員模式中開啟它並再次嘗試發行。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-337">In that case, close Visual Studio, open it in administrator mode, and try to publish again.</span></span>

   <span data-ttu-id="a0cb1-338">如果 Visual Studio 系統管理員模式**輸出**視窗報告成功建置並發行。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-338">If Visual Studio is in administrator mode, the **Output** window reports successful build and publish.</span></span>

   ![Output_window_publish_Test](deploying-to-iis/_static/image20.png)

   <span data-ttu-id="a0cb1-340">如果您在輸入中的 URL**目的地 URL**  方塊中的發行設定檔**連線**索引標籤上，瀏覽器會自動開啟至 IIS 中執行您的電腦上的 Contoso 大學首頁上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-340">If you entered the URL in the **Destination URL** box on the publish profile **Connection** tab, the browser automatically opens to the Contoso University Home page running in IIS on your computer.</span></span>

## <a name="test-in-the-test-environment"></a><span data-ttu-id="a0cb1-341">在測試環境中測試</span><span class="sxs-lookup"><span data-stu-id="a0cb1-341">Test in the test environment</span></span>

<span data-ttu-id="a0cb1-342">請注意，環境指示器會顯示 [（測試）] 會顯示，而不是"(Dev)，" *Web.config*環境指標的轉換是否成功。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-342">Notice that the environment indicator shows "(Test)" instead of "(Dev)," which shows that the *Web.config* transformation for the environment indicator was successful.</span></span>

<span data-ttu-id="a0cb1-343">執行**講師**頁面，確認第一個程式碼植入講師資料的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-343">Run the **Instructors** page to verify that Code First seeded the database with instructor data.</span></span> <span data-ttu-id="a0cb1-344">當您選取此頁面時，可能需要幾分鐘的時間載入，因為第一個程式碼，則會建立資料庫，然後再執行`Seed`方法。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-344">When you select this page, it may take a few minutes to load because Code First creates the database and then runs the `Seed` method.</span></span> <span data-ttu-id="a0cb1-345">（它並沒有進行，因為尚未存取資料庫沒有嘗試過的應用程式已在首頁上時。）</span><span class="sxs-lookup"><span data-stu-id="a0cb1-345">(It didn't do that when you were on the home page because the application didn't try to access the database yet.)</span></span>

<span data-ttu-id="a0cb1-346">選取 [**學生**] 索引標籤，確認已部署的資料庫有任何學生。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-346">Select the **Students** tab to verify that the deployed database has no students.</span></span>

<span data-ttu-id="a0cb1-347">選取 **新增的學生**從**學生**功能表。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-347">Select **Add Students** from the **Students** menu.</span></span> <span data-ttu-id="a0cb1-348">新增為學生，，然後檢視 在新的學生**學生**頁面。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-348">Add a student, and then view the new student in the **Students** page.</span></span> <span data-ttu-id="a0cb1-349">這會確認您可以成功地寫入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-349">This verifies that you can successfully write to the database.</span></span>

<span data-ttu-id="a0cb1-350">從**課程**功能表上，選取**更新信用額度**。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-350">From the **Courses** menu, select **Update Credits**.</span></span> <span data-ttu-id="a0cb1-351">**更新信用額度**頁面會要求系統管理員權限，所以**登入**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-351">The **Update Credits** page requires administrator permissions, so the **Log In** page is displayed.</span></span> <span data-ttu-id="a0cb1-352">輸入您建立舊版 （"admin"和"devpwd 」） 的系統管理員帳戶認證。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-352">Enter the administrator account credentials that you created earlier ("admin" and "devpwd").</span></span> <span data-ttu-id="a0cb1-353">**更新信用額度**頁面隨即顯示。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-353">The **Update Credits** page is displayed.</span></span> <span data-ttu-id="a0cb1-354">這會確認您在上一個教學課程中建立的系統管理員帳戶已正確部署到測試環境。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-354">This verifies that the administrator account that you created in the previous tutorial was correctly deployed to the test environment.</span></span>

<span data-ttu-id="a0cb1-355">確認*ELMAH*資料夾存在於*c:\inetpub\wwwroot\ContosoUniversity*資料夾，並只在其中的預留位置檔案。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-355">Verify that an *ELMAH* folder exists in the *c:\inetpub\wwwroot\ContosoUniversity* folder with only the placeholder file in it.</span></span>

<a id="reviewingmigrations"></a>

## <a name="review-the-automatic-webconfig-changes-for-code-first-migrations"></a><span data-ttu-id="a0cb1-356">檢閱自動 Web.config 變更的 Code First 移轉</span><span class="sxs-lookup"><span data-stu-id="a0cb1-356">Review the automatic Web.config changes for Code First Migrations</span></span>

<span data-ttu-id="a0cb1-357">開啟*Web.config*在已部署的應用程式中的檔案*C:\inetpub\wwwroot\ContosoUniversity*和您所見，部署程序設定 Code First 移轉，自動更新為最新版本的資料庫。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-357">Open the *Web.config* file in the deployed application at *C:\inetpub\wwwroot\ContosoUniversity* and you can see where the deployment process configured Code First Migrations to automatically update the database to the latest version.</span></span>

![](deploying-to-iis/_static/image21.png)

<span data-ttu-id="a0cb1-358">部署程序也會建立新的連接字串，針對專門用來更新資料庫結構描述的 Code First 移轉：</span><span class="sxs-lookup"><span data-stu-id="a0cb1-358">The deployment process also created a new connection string for Code First Migrations to use exclusively for updating the database schema:</span></span>

![Database_Publish 連接字串](deploying-to-iis/_static/image22.png)

<span data-ttu-id="a0cb1-360">此額外的連接字串可讓您指定的資料庫結構描述更新的一個使用者帳戶和不同的使用者帳戶存取應用程式資料。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-360">This additional connection string enables you to specify one user account for database schema updates and a different user account for application data access.</span></span> <span data-ttu-id="a0cb1-361">比方說，您可以指派**db\_擁有者**Code First 移轉的角色並**db\_datareader**具有**db\_資料寫入元**應用程式的角色。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-361">For example, you could assign the **db\_owner** role to Code First Migrations and **db\_datareader** with **db\_datawriter** roles to the application.</span></span> <span data-ttu-id="a0cb1-362">這是常見的深度防禦模式，以防止潛在的惡意程式碼變更資料庫結構描述應用程式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-362">This is a common defense-in-depth pattern that prevents potentially malicious code in the application from changing the database schema.</span></span> <span data-ttu-id="a0cb1-363">（例如，這可能是在 SQL 資料隱碼攻擊成功。）這些教學課程，請勿使用此模式。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-363">(For example, this might happen in a successful SQL injection attack.) These tutorials don't use this pattern.</span></span> <span data-ttu-id="a0cb1-364">若要實作此模式在您的案例中，執行下列步驟：</span><span class="sxs-lookup"><span data-stu-id="a0cb1-364">To implement this pattern in your scenario, take these steps:</span></span>

1. <span data-ttu-id="a0cb1-365">在 **發佈 Web**精靈下方**設定**索引標籤上，輸入連接字串，指定具有完整的資料庫結構描述更新權限的使用者。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-365">In the **Publish Web** wizard under the **Settings** tab, enter the connection string that specifies a user with full database schema update permissions.</span></span> <span data-ttu-id="a0cb1-366">清除**在執行階段使用此連接字串**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-366">Clear the **Use this connection string at runtime** check box.</span></span> <span data-ttu-id="a0cb1-367">在已部署的 Web.config 檔案中，這會成為`DatabasePublish`連接字串。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-367">In the deployed Web.config file, this becomes the `DatabasePublish` connection string.</span></span>

2. <span data-ttu-id="a0cb1-368">建立 Web.config 檔案轉換為您想要在執行階段使用的應用程式的連接字串。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-368">Create a Web.config file transformation for the connection string that you want the application to use at run time.</span></span>

## <a name="summary"></a><span data-ttu-id="a0cb1-369">總結</span><span class="sxs-lookup"><span data-stu-id="a0cb1-369">Summary</span></span>

<span data-ttu-id="a0cb1-370">您已經在您的開發電腦上部署至 IIS 的應用程式並進行了測試那里。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-370">You've now deployed your application to IIS on your development computer and tested it there.</span></span>

![在測試中的 [首頁] 頁面](deploying-to-iis/_static/image23.png)

<span data-ttu-id="a0cb1-372">這會確認部署程序複製到正確的位置 （不包括您不想要部署的檔案） 的應用程式的內容，也該 Web Deploy IIS 正確地在部署期間設定。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-372">This verifies that the deployment process copied the application's content to the right location (excluding the files that you did not want to deploy) and also that Web Deploy configured IIS correctly during deployment.</span></span> <span data-ttu-id="a0cb1-373">在下一個教學課程中，您將執行一個會尋找尚未完成的部署工作的多個測試： 設定資料夾權限*Elm ah*資料夾。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-373">In the next tutorial, you'll run one more test that finds a deployment task that has not yet been done: setting folder permissions on the *Elm ah* folder.</span></span>

## <a name="more-information"></a><span data-ttu-id="a0cb1-374">詳細資訊</span><span class="sxs-lookup"><span data-stu-id="a0cb1-374">More information</span></span>

<span data-ttu-id="a0cb1-375">如需在 Visual Studio 中執行 IIS 或 IIS Express 的資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="a0cb1-375">For information about running IIS or IIS Express in Visual Studio, see the following resources:</span></span>

- <span data-ttu-id="a0cb1-376">[IIS Express 概觀](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview)IIS.net 網站上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-376">[IIS Express Overview](https://www.iis.net/learn/extensions/introduction-to-iis-express/iis-express-overview) on the IIS.net site.</span></span>
- <span data-ttu-id="a0cb1-377">[簡介 IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) Scott Guthrie 的部落格上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-377">[Introducing IIS Express](https://weblogs.asp.net/scottgu/archive/2010/06/28/introducing-iis-express.aspx) on Scott Guthrie's blog.</span></span>
- <span data-ttu-id="a0cb1-378">[Web 伺服器，在 Visual Studio 中的，針對 ASP.NET Web 專案](https://msdn.microsoft.com/library/58wxa9w5.aspx)。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-378">[Web Servers in Visual Studio for ASP.NET Web Projects](https://msdn.microsoft.com/library/58wxa9w5.aspx).</span></span>
- <span data-ttu-id="a0cb1-379">[核心 IIS 之間的差異和 ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) ASP.NET 網站上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-379">[Core Differences Between IIS and the ASP.NET Development Server](../../older-versions-getting-started/deploying-web-site-projects/core-differences-between-iis-and-the-asp-net-development-server-cs.md) on the ASP.NET site.</span></span>

<span data-ttu-id="a0cb1-380">如在中度信任執行的應用程式時，就可能發生哪些問題的相關資訊，請參閱 <<c0> [ 裝載在中度信任 ASP.NET 應用程式](http://www.4guysfromrolla.com/articles/100307-1.aspx)Rolla 站台的四個夥伴上。</span><span class="sxs-lookup"><span data-stu-id="a0cb1-380">For information about what issues might arise when your application runs in medium trust, see [Hosting ASP.NET Applications in Medium Trust](http://www.4guysfromrolla.com/articles/100307-1.aspx) on the four Guys from Rolla site.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="a0cb1-381">[上一頁](project-properties.md)
> [下一頁](setting-folder-permissions.md)</span><span class="sxs-lookup"><span data-stu-id="a0cb1-381">[Previous](project-properties.md)
[Next](setting-folder-permissions.md)</span></span>
