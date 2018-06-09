---
uid: visual-studio/overview/2013/creating-web-projects-in-visual-studio
title: 建立 ASP.NET Web 專案在 Visual Studio 2013 |Microsoft 文件
author: tdykstra
description: 本主題說明在 Visual Studio 2013 與此處 Update 3 中建立 ASP.NET web 專案的選項是 web 開發 c 的新功能的一些...
ms.author: aspnetcontent
manager: wpickett
ms.date: 12/01/2014
ms.topic: article
ms.assetid: 61941e64-0c0d-4996-9270-cb8ccfd0cabc
ms.technology: ''
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2013/creating-web-projects-in-visual-studio
msc.type: authoredcontent
ms.openlocfilehash: aacae7a9ccf483b21d3c6796c0411d558fa3c75b
ms.sourcegitcommit: 6784510cfb589308c3875ccb5113eb31031766b4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/08/2018
ms.locfileid: "28038861"
---
<a name="creating-aspnet-web-projects-in-visual-studio-2013"></a><span data-ttu-id="7d09a-103">在 Visual Studio 2013 中建立 ASP.NET Web 專案</span><span class="sxs-lookup"><span data-stu-id="7d09a-103">Creating ASP.NET Web Projects in Visual Studio 2013</span></span>
====================
<span data-ttu-id="7d09a-104">由[Tom Dykstra](https://github.com/tdykstra)</span><span class="sxs-lookup"><span data-stu-id="7d09a-104">by [Tom Dykstra](https://github.com/tdykstra)</span></span>

> <span data-ttu-id="7d09a-105">本主題說明在 Visual Studio 2013 Update 3 中建立 ASP.NET web 專案的選項</span><span class="sxs-lookup"><span data-stu-id="7d09a-105">This topic explains the options for creating ASP.NET web projects in Visual Studio 2013 with Update 3</span></span>
> 
> <span data-ttu-id="7d09a-106">以下是一些相較於舊版的 Visual Studio 網頁開發的新功能：</span><span class="sxs-lookup"><span data-stu-id="7d09a-106">Here are some of the new features for web development compared to earlier versions of Visual Studio:</span></span>
> 
> - <span data-ttu-id="7d09a-107">建立的簡單 UI 專案該優惠[支援多個 ASP.NET 架構](#add)（Web Forms、 MVC、 及 Web 應用程式開發介面）。</span><span class="sxs-lookup"><span data-stu-id="7d09a-107">A simple UI for creating projects that offer [support for multiple ASP.NET frameworks](#add) (Web Forms, MVC, and Web API).</span></span>
> - <span data-ttu-id="7d09a-108">[ASP.NET Identity](#indauth)，新的 ASP.NET 成員資格系統，一樣會在所有 ASP.NET 架構和運作方式與 web 裝載 IIS 以外的軟體。</span><span class="sxs-lookup"><span data-stu-id="7d09a-108">[ASP.NET Identity](#indauth), a new ASP.NET membership system that works the same in all ASP.NET frameworks and works with web hosting software other than IIS.</span></span>
> - <span data-ttu-id="7d09a-109">使用[Bootstrap](#bootstrap)提供回應式設計和主題功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-109">The use of [Bootstrap](#bootstrap) to provide responsive design and theming capabilities.</span></span>
> - <span data-ttu-id="7d09a-110">用來只對 MVC 中，例如提供的 Web Form 的新功能[自動測試專案的建立](#testproj)和[內部網路網站範本](#winauth)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-110">New features for Web Forms that used to be offered only for MVC, such as [automatic test project creation](#testproj) and an [Intranet site template](#winauth).</span></span>
> 
> <span data-ttu-id="7d09a-111">如需如何為 Azure 雲端服務或 Azure 行動服務中建立 web 專案的資訊，請參閱[開始使用 Azure 雲端服務和 ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/)和[建立 Leaderboard 應用程式使用 Azure 行動服務的.NET後端](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-111">For information about how to create web projects for Azure Cloud Services or Azure Mobile Services, see [Get Started with Azure Cloud Services and ASP.NET](https://azure.microsoft.com/documentation/articles/cloud-services-dotnet-get-started/) and [Creating a Leaderboard App with Azure Mobile Services .NET Backend](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/).</span></span>


<a id="prerequisites"></a>
## <a name="prerequisites"></a><span data-ttu-id="7d09a-112">必要條件</span><span class="sxs-lookup"><span data-stu-id="7d09a-112">Prerequisites</span></span>

<span data-ttu-id="7d09a-113">本文適用於[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)與[Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409)安裝。</span><span class="sxs-lookup"><span data-stu-id="7d09a-113">This article applies to [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566) with [Update 3](https://go.microsoft.com/fwlink/?linkid=397827&amp;clcid=0x409) installed.</span></span>

<a id="wap"></a>
## <a name="web-application-projects-versus-web-site-projects"></a><span data-ttu-id="7d09a-114">與網站專案的 web 應用程式專案</span><span class="sxs-lookup"><span data-stu-id="7d09a-114">Web application projects versus web site projects</span></span>

<span data-ttu-id="7d09a-115">ASP.NET 可讓您可以選擇兩種類型的 web 專案： *web 應用程式專案*和*網站專案*。</span><span class="sxs-lookup"><span data-stu-id="7d09a-115">ASP.NET gives you a choice between two kinds of web projects: *web application projects* and *web site projects*.</span></span> <span data-ttu-id="7d09a-116">我們建議您進行新開發，web 應用程式專案，這篇文章僅適用於 web 應用程式專案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-116">We recommend web application projects for new development, and this article applies only to web application projects.</span></span> <span data-ttu-id="7d09a-117">如需詳細資訊，請參閱[Web 應用程式專案與 Visual Studio 中的網站專案](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx)MSDN 網站上的。</span><span class="sxs-lookup"><span data-stu-id="7d09a-117">For more information, see [Web Application Projects versus Web Site Projects in Visual Studio](https://msdn.microsoft.com/library/dd547590(v=vs.120).aspx) on the MSDN site.</span></span>

<a id="overview"></a>
## <a name="overview-of-web-application-project-creation"></a><span data-ttu-id="7d09a-118">建立 web 應用程式專案的概觀</span><span class="sxs-lookup"><span data-stu-id="7d09a-118">Overview of web application project creation</span></span>

<span data-ttu-id="7d09a-119">下列步驟顯示如何建立 web 專案：</span><span class="sxs-lookup"><span data-stu-id="7d09a-119">The following steps show how to create a web project:</span></span>

1. <span data-ttu-id="7d09a-120">按一下**新專案**中**啟動**頁面或在**檔案**功能表。</span><span class="sxs-lookup"><span data-stu-id="7d09a-120">Click **New Project** in the **Start** page or in the **File** menu.</span></span>
2. <span data-ttu-id="7d09a-121">在**新專案**] 對話方塊中，按一下 [ **Web**的左窗格中和**ASP.NET Web 應用程式**中間窗格內。</span><span class="sxs-lookup"><span data-stu-id="7d09a-121">In the **New Project** dialog, click **Web** in the left pane and **ASP.NET Web Application** in the middle pane.</span></span>

    ![[新增專案] 對話](creating-web-projects-in-visual-studio/_static/image1.png)

    <span data-ttu-id="7d09a-123">您可以選擇**雲端**建立的左窗格中[Azure 雲端服務](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy)， [Azure 行動服務](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx)，或[Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-123">You can choose **Cloud** in the left pane to create an [Azure Cloud Service](https://docs.microsoft.com/azure/cloud-services/cloud-services-how-to-create-deploy), [Azure Mobile Service](https://msdn.microsoft.com/library/windows/apps/dn629482.aspx), or [Azure WebJob](https://azure.microsoft.com/documentation/articles/web-sites-dotnet-deploy-webjobs).</span></span> <span data-ttu-id="7d09a-124">本主題並未涵蓋這些範本。</span><span class="sxs-lookup"><span data-stu-id="7d09a-124">This topic doesn't cover those templates.</span></span>
3. <span data-ttu-id="7d09a-125">在右窗格中，按一下 **加入 Application Insights 加入專案**核取方塊，如果您想健全狀況與使用量監視您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-125">In the right pane, click the **Add Application Insights to Project** check box if you want health and usage monitoring for your application.</span></span> <span data-ttu-id="7d09a-126">如需詳細資訊，請參閱[監視 web 應用程式中的效能](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-126">For more information, see [Monitor performance in web applications](https://azure.microsoft.com/documentation/articles/app-insights-web-monitor-performance/).</span></span>
4. <span data-ttu-id="7d09a-127">指定專案**名稱**，**位置**，和其他選項，以及然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-127">Specify project **Name**, **Location**, and other options, and then click **OK**.</span></span>

    <span data-ttu-id="7d09a-128">**新增 ASP.NET 專案**對話方塊隨即出現。</span><span class="sxs-lookup"><span data-stu-id="7d09a-128">The **New ASP.NET Project** dialog appears.</span></span>

    ![[新增專案] 對話](creating-web-projects-in-visual-studio/_static/image2.png)
5. <span data-ttu-id="7d09a-130">按一下 [範本]。</span><span class="sxs-lookup"><span data-stu-id="7d09a-130">Click a template.</span></span>

    ![選取的範本](creating-web-projects-in-visual-studio/_static/image3.png)
6. <span data-ttu-id="7d09a-132">如果您想要加入支援其他架構未包含範本中，按一下適當的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-132">If you want to add support for additional frameworks not included in the template, click the appropriate check box.</span></span> <span data-ttu-id="7d09a-133">（在範例中所示，您可以新增 MVC 和/或 Web API Web Form 專案。）</span><span class="sxs-lookup"><span data-stu-id="7d09a-133">(In the example shown, you could add MVC and/or Web API to a Web Forms project.)</span></span>

    ![將架構](creating-web-projects-in-visual-studio/_static/image4.png)
7. <a id="testproj"></a><span data-ttu-id="7d09a-135">如果您想要加入單元測試專案，按一下**加入單元測試**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-135">If you want to add a unit test project, click **Add unit tests**.</span></span>

    ![加入單元測試](creating-web-projects-in-visual-studio/_static/image5.png)
8. <span data-ttu-id="7d09a-137">如果您想要不同的驗證方法比預設提供的範本，請按一下**變更驗證**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-137">If you want a different authentication method than what the template provides by default, click **Change Authentication**.</span></span>

    ![設定驗證 按鈕](creating-web-projects-in-visual-studio/_static/image6.png)

    ![設定驗證對話方塊](creating-web-projects-in-visual-studio/_static/image7.png)

<a id="azurenewproj"></a>
### <a name="create-a-web-app-or-virtual-machine-in-azure"></a><span data-ttu-id="7d09a-140">在 Azure 中建立的 web 應用程式或虛擬機器</span><span class="sxs-lookup"><span data-stu-id="7d09a-140">Create a web app or virtual machine in Azure</span></span>

<span data-ttu-id="7d09a-141">Visual Studio 包含可讓您輕鬆地使用 Azure 服務來裝載 web 應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-141">Visual Studio includes features that make it easy to work with Azure services for hosting web applications.</span></span> <span data-ttu-id="7d09a-142">例如，您可以直接從 Visual Studio IDE 執行下列各項：</span><span class="sxs-lookup"><span data-stu-id="7d09a-142">For example, you can do all of the following right from the Visual Studio IDE:</span></span>

- <span data-ttu-id="7d09a-143">建立及管理 web 應用程式或透過網際網路提供您的應用程式的虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="7d09a-143">Create and manage web apps or virtual machines that make your application available over the Internet.</span></span>
- <span data-ttu-id="7d09a-144">檢視雲端中執行應用程式所建立的記錄檔。</span><span class="sxs-lookup"><span data-stu-id="7d09a-144">View logs created by the application as it runs in the cloud.</span></span>
- <span data-ttu-id="7d09a-145">在 偵錯模式中從遠端執行應用程式會在雲端中執行時。</span><span class="sxs-lookup"><span data-stu-id="7d09a-145">Run in debug mode remotely while the application runs in the cloud.</span></span>
- <span data-ttu-id="7d09a-146">Viiew 及管理其他 Azure 服務，例如 SQL 資料庫。</span><span class="sxs-lookup"><span data-stu-id="7d09a-146">Viiew and manage other Azure services such as SQL databases.</span></span>

<span data-ttu-id="7d09a-147">您可以[建立 Azure 帳戶](https://www.windowsazure.com/pricing/free-trial/)免費，包含基本的服務，例如 web 應用程式，而且如果您是 MSDN 訂戶可以[啟用權益](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/)，可以讓您向其他 Azure 每月信用額度服務。</span><span class="sxs-lookup"><span data-stu-id="7d09a-147">You can [create an Azure account](https://www.windowsazure.com/pricing/free-trial/) that includes basic services such as web apps for free, and if you are an MSDN subscriber you can [activate benefits](https://azure.microsoft.com/pricing/member-offers/visual-studio-subscriptions/) that give you monthly credits toward additional Azure services.</span></span> 

<span data-ttu-id="7d09a-148">根據預設**新增 ASP.NET 專案**對話方塊可讓您建立 web 應用程式或新的 web 專案的虛擬機器。</span><span class="sxs-lookup"><span data-stu-id="7d09a-148">By default the **New ASP.NET Project** dialog box enables you to create a web app or virtual machine for a new web project.</span></span> <span data-ttu-id="7d09a-149">如果您不想要建立新的 web 應用程式或虛擬機器，清除**雲端中的主機**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-149">If you don't want to create a new web app or virtual machine, clear the **Host in the cloud** check box.</span></span>

![建立遠端資源](creating-web-projects-in-visual-studio/_static/image8.png)

<span data-ttu-id="7d09a-151">核取方塊標題可能**雲端中的主機**或**建立遠端資源**，並在任一情況下影響相同。</span><span class="sxs-lookup"><span data-stu-id="7d09a-151">The check box caption might be **Host in the cloud** or **Create remote resources**, and in either case the effect is the same.</span></span> <span data-ttu-id="7d09a-152">如果您將選取的核取方塊，則 Visual Studio 會建立在 Azure App Service web 應用程式預設。</span><span class="sxs-lookup"><span data-stu-id="7d09a-152">If you leave the check box selected, Visual Studio creates a web app in Azure App Service by default.</span></span> <span data-ttu-id="7d09a-153">您可以使用下拉式清單方塊，若要變更**虛擬機器**如果您偏好。</span><span class="sxs-lookup"><span data-stu-id="7d09a-153">You can use the drop-down box to change that to a **Virtual Machine** if you prefer.</span></span> <span data-ttu-id="7d09a-154">如果您沒有已登入至 Azure，系統提示您輸入的 Azure 認證。</span><span class="sxs-lookup"><span data-stu-id="7d09a-154">If you're not already signed in to Azure, you're prompted for Azure credentials.</span></span> <span data-ttu-id="7d09a-155">登入之後，對話方塊可讓您設定 Visual Studio 會為您的專案建立的資源。</span><span class="sxs-lookup"><span data-stu-id="7d09a-155">After you sign in, a dialog box enables you to configure the resources that Visual Studio will create for your project.</span></span> <span data-ttu-id="7d09a-156">下圖顯示 web 應用程式中; 對話方塊如果您選擇建立虛擬機器，則會出現不同的選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-156">The following illustration shows the dialog for a web app; different options appear if you choose to create a virtual machine.</span></span>

![設定 Azure 應用程式設定](creating-web-projects-in-visual-studio/_static/image9.png)

<span data-ttu-id="7d09a-158">如需如何使用此程序來建立 Azure 資源的詳細資訊，請參閱[開始使用 Azure 和 ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)和[使用 Visual Studio 中建立網站的虛擬機器](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-158">For more information about how to use this process for creating Azure resources, see [Get Started with Azure and ASP.NET](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet) and [Creating a virtual machine for a web site with Visual Studio](https://azure.microsoft.com/documentation/articles/virtual-machines-dotnet-create-visual-studio-powershell/).</span></span>

<span data-ttu-id="7d09a-159">這篇文章的其餘部分提供可用的範本和其選項的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-159">The remainder of this article provides more information about the available templates and their options.</span></span> <span data-ttu-id="7d09a-160">發行項也導入啟動程序、 配置和佈景主題的架構，用於範本。</span><span class="sxs-lookup"><span data-stu-id="7d09a-160">The article also introduces Bootstrap, the layout and theming framework used in the templates.</span></span>

<a id="vs2013"></a>
## <a name="visual-studio-2013-web-project-templates"></a><span data-ttu-id="7d09a-161">Visual Studio 2013 Web 專案範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-161">Visual Studio 2013 Web Project Templates</span></span>

<span data-ttu-id="7d09a-162">Visual Studio 會使用範本來建立 web 專案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-162">Visual Studio uses templates to create web projects.</span></span> <span data-ttu-id="7d09a-163">專案範本可以建立新的專案中的檔案和資料夾、 安裝 NuGet 封裝，以及提供基本的實用應用程式的範例程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d09a-163">A project template can create files and folders in the new project, install NuGet packages, and provide sample code for a rudimentary working application.</span></span> <span data-ttu-id="7d09a-164">範本會實作最新 web 標準和適用於示範最佳作法來使用 ASP.NET 技術，以及讓您跳開始建立您自己的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-164">Templates implement the latest web standards and are intended to demonstrate best practices for how to use ASP.NET technologies as well as give you a jump start on creating your own application.</span></span>

<span data-ttu-id="7d09a-165">Visual Studio 2013.NET 4.5 或更新版本的.NET framework 為目標的專案的 web 專案範本提供下列選項：</span><span class="sxs-lookup"><span data-stu-id="7d09a-165">Visual Studio 2013 provides the following choices for web project templates for projects that target .NET 4.5 or later versions of the .NET framework:</span></span>

- [<span data-ttu-id="7d09a-166">空白的範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-166">Empty template</span></span>](#empty)
- [<span data-ttu-id="7d09a-167">Web Form 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-167">Web Forms template</span></span>](#wf)
- [<span data-ttu-id="7d09a-168">MVC 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-168">MVC template</span></span>](#mvc)
- [<span data-ttu-id="7d09a-169">Web API 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-169">Web API template</span></span>](#webapi)
- [<span data-ttu-id="7d09a-170">單一頁面應用程式範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-170">Single Page Application template</span></span>](#spa)
- [<span data-ttu-id="7d09a-171">Azure 行動服務範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-171">Azure Mobile Service template</span></span>](https://azure.microsoft.com/documentation/articles/mobile-services-dotnet-backend-windows-store-dotnet-leaderboard/)
- [<span data-ttu-id="7d09a-172">Visual Studio 2012 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-172">Visual Studio 2012 Templates</span></span>](#vs2012)

<span data-ttu-id="7d09a-173">您也可以安裝 Visual Studio 擴充功能提供[Facebook 範本](#facebook)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-173">You can also install a Visual Studio extension that provides a [Facebook template](#facebook).</span></span>

<span data-ttu-id="7d09a-174">如需如何建立.NET 4 為目標的專案相關資訊，請參閱[Visual Studio 2012 範本](#vs2012)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="7d09a-174">For information about how to create projects that target .NET 4, see [Visual Studio 2012 Templates](#vs2012) later in this topic.</span></span>

<span data-ttu-id="7d09a-175">如需如何建立 ASP.NET 應用程式，行動用戶端的相關資訊，請參閱[ASP.NET 中的行動支援](../../../mobile/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-175">For information about how to create ASP.NET applications for mobile clients, see [Mobile Support in ASP.NET](../../../mobile/index.md).</span></span>

<a id="empty"></a>
### <a name="empty-template"></a><span data-ttu-id="7d09a-176">空白的範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-176">Empty Template</span></span>

<span data-ttu-id="7d09a-177">空白範本提供 ASP.NET web 應用程式，例如專案檔案的檔案和裸機最小的資料夾 (*.csproj*或。*vbproj*) 和*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-177">The Empty template provides the bare minimum folders and files for an ASP.NET web app, such as a project file (*.csproj* or .*vbproj*) and a *Web.config* file.</span></span> <span data-ttu-id="7d09a-178">您可以使用下方的核取方塊，以新增支援 Web Forms、 MVC，及/或 Web API**加入資料夾和核心參考：** 標籤。</span><span class="sxs-lookup"><span data-stu-id="7d09a-178">You can add support for Web Forms, MVC, and/or Web API by using the check boxes under the **Add folders and core references for:** label.</span></span>

<span data-ttu-id="7d09a-179">用於空白的範本未驗證選項可用。</span><span class="sxs-lookup"><span data-stu-id="7d09a-179">For the Empty template no authentication options are available.</span></span> <span data-ttu-id="7d09a-180">範例應用程式中實作驗證功能和空白的範本並不會建立範例應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-180">Authentication functionality is implemented in sample applications, and the Empty template does not create a sample application.</span></span>

<a id="wf"></a>
### <a name="web-forms-template"></a><span data-ttu-id="7d09a-181">Web Form 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-181">Web Forms Template</span></span>

<span data-ttu-id="7d09a-182">Web Form 架構提供下列功能，可讓您快速建置網站豐富的 UI 和資料存取功能：</span><span class="sxs-lookup"><span data-stu-id="7d09a-182">The Web Forms framework provides the following features that let you rapidly build web sites that are rich in UI and data access features:</span></span>

- <span data-ttu-id="7d09a-183">在 Visual Studio 中的 WYSIWYG 設計工具。</span><span class="sxs-lookup"><span data-stu-id="7d09a-183">A WYSIWYG designer in Visual Studio.</span></span>
- <span data-ttu-id="7d09a-184">伺服器控制項的呈現 HTML，您可以自訂藉由設定屬性和樣式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-184">Server controls that render HTML and that you can customize by setting properties and styles.</span></span>
- <span data-ttu-id="7d09a-185">資料存取和顯示資料的控制豐富 assortment。</span><span class="sxs-lookup"><span data-stu-id="7d09a-185">A rich assortment of controls for data access and data display.</span></span>
- <span data-ttu-id="7d09a-186">會公開您可以像您程式的事件的事件模型會程式 WPF 用戶端應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-186">An event model that exposes events which you can program like you would program a client application such as WPF.</span></span>
- <span data-ttu-id="7d09a-187">自動保留的 HTTP 要求之間的狀態 （資料）。</span><span class="sxs-lookup"><span data-stu-id="7d09a-187">Automatic preservation of state (data) between HTTP requests.</span></span>

<span data-ttu-id="7d09a-188">一般情況下，建立 Web Form 應用程式需要較少撰寫的程式，比使用 ASP.NET MVC 架構建立相同的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-188">In general, creating a Web Forms application requires less programming effort than creating the same application by using the ASP.NET MVC framework.</span></span> <span data-ttu-id="7d09a-189">不過，Web Form 不只適用於快速應用程式開發。</span><span class="sxs-lookup"><span data-stu-id="7d09a-189">However, Web Forms is not just for rapid application development.</span></span> <span data-ttu-id="7d09a-190">有許多複雜的商業應用程式和 Web Form 基礎架構。</span><span class="sxs-lookup"><span data-stu-id="7d09a-190">There are many complex commercial applications and frameworks built on top of Web Forms.</span></span>

<span data-ttu-id="7d09a-191">Web Form 頁面和頁面上的控制項自動產生傳送至瀏覽器標記的許多，因為您不需要更細微的控制 ASP.NET MVC 提供 HTML 的種類。</span><span class="sxs-lookup"><span data-stu-id="7d09a-191">Because a Web Forms page and the controls on the page automatically generate much of the markup that's sent to the browser, you don't have the kind of fine-grained control over the HTML that ASP.NET MVC offers.</span></span> <span data-ttu-id="7d09a-192">設定網頁及控制項的宣告式模型盡量縮短的程式碼，您必須撰寫，但會隱藏一些 HTML 和 HTTP 的行為。</span><span class="sxs-lookup"><span data-stu-id="7d09a-192">The declarative model for configuring pages and controls minimizes the amount of code you have to write but hides some of the behavior of HTML and HTTP.</span></span> <span data-ttu-id="7d09a-193">比方說，它就不一定能夠指定正好哪些標記可能會產生由控制項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-193">For example, it's not always possible to specify exactly what markup might be generated by a control.</span></span>

<span data-ttu-id="7d09a-194">Web Form 架構不共享 ASP.NET MVC 為便於模式為基礎的開發作法例如[測試導向開發](http://en.wikipedia.org/wiki/Test-driven_development)，[的重要性分離](http://en.wikipedia.org/wiki/Separation_of_concerns)，[逆轉控制](http://en.wikipedia.org/wiki/Inversion_of_control)，和[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-194">The Web Forms framework doesn't lend itself as readily as ASP.NET MVC to patterns-based development practices such as [test-driven development](http://en.wikipedia.org/wiki/Test-driven_development), [separation of concerns](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversion of control](http://en.wikipedia.org/wiki/Inversion_of_control), and [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection).</span></span> <span data-ttu-id="7d09a-195">如果您想要撰寫程式碼中分解出來，這樣一來，您可以;它不只在 ASP.NET MVC 架構中的自動。</span><span class="sxs-lookup"><span data-stu-id="7d09a-195">If you want to write code factored that way, you can; it's just not as automatic as it is in the ASP.NET MVC framework.</span></span> <span data-ttu-id="7d09a-196">[ASP.NET Web Form MVP](http://webformsmvp.com/)專案顯示有助於隔離維持傳遞所建置的 Web Form 開發考量及測試能力的方法。</span><span class="sxs-lookup"><span data-stu-id="7d09a-196">The [ASP.NET Web Forms MVP](http://webformsmvp.com/) project shows an approach that facilitates separation of concerns and testability while maintaining the rapid development that Web Forms was built to deliver.</span></span> <span data-ttu-id="7d09a-197">Microsoft SharePoint 是建置在 Web Form MVP。</span><span class="sxs-lookup"><span data-stu-id="7d09a-197">Microsoft SharePoint is built on Web Forms MVP.</span></span>

<span data-ttu-id="7d09a-198">Web Form 範本所建立的範例 Web Form 應用程式會使用[Bootstrap](#bootstrap)提供回應式設計和主題功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-198">The Web Forms template creates a sample Web Forms application that uses [Bootstrap](#bootstrap) to provide responsive design and theming features.</span></span> <span data-ttu-id="7d09a-199">下圖顯示 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="7d09a-199">The following illustration shows the home page.</span></span>

![Web Form 範本應用程式首頁](creating-web-projects-in-visual-studio/_static/image10.png)

<span data-ttu-id="7d09a-201">如需 Web Form 的詳細資訊，請參閱[ASP.NET Web Form](https://asp.net/web-forms)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-201">For more information about Web Forms, see [ASP.NET Web Forms](https://asp.net/web-forms).</span></span> <span data-ttu-id="7d09a-202">如需 Web Form 範本會為您的資訊，請參閱[建置基本的 Web Form 應用程式，使用 Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-202">For information about what the Web Forms template does for you, see [Building a basic Web Forms application using Visual Studio 2013](https://blogs.msdn.com/b/webdev/archive/2013/12/19/building-a-basic-web-forms-application-using-visual-studio-2013.aspx).</span></span>

<a id="mvc"></a>
### <a name="mvc-template"></a><span data-ttu-id="7d09a-203">MVC 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-203">MVC Template</span></span>

<span data-ttu-id="7d09a-204">ASP.NET MVC 的用意是為了模式為基礎的開發作法例如[測試導向開發](http://en.wikipedia.org/wiki/Test-driven_development)，[的重要性分離](http://en.wikipedia.org/wiki/Separation_of_concerns)，[逆轉控制](http://en.wikipedia.org/wiki/Inversion_of_control)，和[相依性插入](http://en.wikipedia.org/wiki/Dependency_injection)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-204">ASP.NET MVC was designed to facilitate patterns-based development practices such as [test-driven development](http://en.wikipedia.org/wiki/Test-driven_development), [separation of concerns](http://en.wikipedia.org/wiki/Separation_of_concerns), [inversion of control](http://en.wikipedia.org/wiki/Inversion_of_control), and [dependency injection](http://en.wikipedia.org/wiki/Dependency_injection).</span></span> <span data-ttu-id="7d09a-205">此架構有助於區隔其展示層 web 應用程式的商務邏輯層。</span><span class="sxs-lookup"><span data-stu-id="7d09a-205">The framework encourages separating the business logic layer of a web application from its presentation layer.</span></span> <span data-ttu-id="7d09a-206">透過將應用程式分割成模型 (M)、 (V)、 檢視和控制站 (C)，ASP.NET MVC 可以輕鬆管理複雜性較大的應用程式中。</span><span class="sxs-lookup"><span data-stu-id="7d09a-206">By dividing the application into models (M), views (V), and controllers (C), ASP.NET MVC can make it easier to manage complexity in larger applications.</span></span>

<span data-ttu-id="7d09a-207">使用 ASP.NET MVC 中，您更直接使用 HTML 和 HTTP 比 Web Form 中。</span><span class="sxs-lookup"><span data-stu-id="7d09a-207">With ASP.NET MVC, you work more directly with HTML and HTTP than in Web Forms.</span></span> <span data-ttu-id="7d09a-208">例如，Web Form 可以自動保留 HTTP 要求之間的狀態，但是您必須撰寫程式碼，明確地在 MVC 中。</span><span class="sxs-lookup"><span data-stu-id="7d09a-208">For example, Web Forms can automatically preserve state between HTTP requests, but you have to code that explicitly in MVC.</span></span> <span data-ttu-id="7d09a-209">MVC 模型的優點是它可讓您完全正在進行的應用程式和 web 環境中的運作方式的完整控制權。</span><span class="sxs-lookup"><span data-stu-id="7d09a-209">The advantage of the MVC model is that it enables you to take complete control over exactly what your application is doing and how it behaves in the web environment.</span></span> <span data-ttu-id="7d09a-210">缺點是，您需要撰寫更多程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d09a-210">The disadvantage is that you have to write more code.</span></span>

<span data-ttu-id="7d09a-211">MVC 的用意是可擴充的提供電源開發自訂的架構，其應用程式需求的能力。</span><span class="sxs-lookup"><span data-stu-id="7d09a-211">MVC was designed to be extensible, providing power developers the ability to customize the framework for their application needs.</span></span> <span data-ttu-id="7d09a-212">此外，原始碼 ASP.NET MVC 可 OSI 授權。</span><span class="sxs-lookup"><span data-stu-id="7d09a-212">In addition, the ASP.NET MVC source code is available under an OSI license.</span></span>

<span data-ttu-id="7d09a-213">MVC 範本會建立範例 MVC 5 應用程式使用[Bootstrap](#bootstrap)提供回應式設計和主題功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-213">The MVC template creates a sample MVC 5 application that uses [Bootstrap](#bootstrap) to provide responsive design and theming features.</span></span> <span data-ttu-id="7d09a-214">下圖顯示 [首頁] 頁面。</span><span class="sxs-lookup"><span data-stu-id="7d09a-214">The following illustration shows the home page.</span></span>

![MVC 範例應用程式](creating-web-projects-in-visual-studio/_static/image11.png)

<span data-ttu-id="7d09a-216">如需 MVC 的詳細資訊，請參閱[ASP.NET MVC](https://asp.net/mvc)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-216">For more information about MVC, see [ASP.NET MVC](https://asp.net/mvc).</span></span> <span data-ttu-id="7d09a-217">如需如何選取 MVC 4 範本資訊，請參閱[Visual Studio 2012 範本](#vs2012)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="7d09a-217">For information about how to select the MVC 4 template, see [Visual Studio 2012 templates](#vs2012) later in this article.</span></span>

<a id="webapi"></a>
### <a name="web-api-template"></a><span data-ttu-id="7d09a-218">Web API 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-218">Web API Template</span></span>

<span data-ttu-id="7d09a-219">Web API 範本建立 Web API，包括 MVC 為基礎的 API 說明頁面為基礎的範例 web 服務。</span><span class="sxs-lookup"><span data-stu-id="7d09a-219">The Web API template creates a sample web service based on Web API, including API help pages based on MVC.</span></span>

<span data-ttu-id="7d09a-220">ASP.NET Web API 是一種架構，可讓您更輕鬆地建立連線的用戶端，包括瀏覽器和行動裝置的較大範圍的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="7d09a-220">ASP.NET Web API is a framework that makes it easy to build HTTP services that reach a broad range of clients, including browsers and mobile devices.</span></span> <span data-ttu-id="7d09a-221">ASP.NET Web 應用程式開發介面是.NET Framework 上建置 RESTful 服務的理想平台。</span><span class="sxs-lookup"><span data-stu-id="7d09a-221">ASP.NET Web API is an ideal platform for building RESTful services on the .NET Framework.</span></span>

<span data-ttu-id="7d09a-222">Web API 範本會建立範例 web 服務。</span><span class="sxs-lookup"><span data-stu-id="7d09a-222">The Web API template creates a sample web service.</span></span> <span data-ttu-id="7d09a-223">下圖顯示的範例說明頁面。</span><span class="sxs-lookup"><span data-stu-id="7d09a-223">The following illustrations show sample help pages.</span></span>

![Web API 說明頁面](creating-web-projects-in-visual-studio/_static/image12.png)

![取得應用程式開發介面的 web API 說明頁面](creating-web-projects-in-visual-studio/_static/image13.png)

<span data-ttu-id="7d09a-226">如需 Web API 的詳細資訊，請參閱[ASP.NET Web API](https://asp.net/web-api)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-226">For more information about Web API, see [ASP.NET Web API](https://asp.net/web-api).</span></span>

<a id="spa"></a>
### <a name="single-page-application-template"></a><span data-ttu-id="7d09a-227">單一頁面應用程式範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-227">Single Page Application Template</span></span>

<span data-ttu-id="7d09a-228">單一頁面應用程式 (SPA) 範本會建立範例應用程式使用 JavaScript，HTML 5 和[KnockoutJS](http://knockoutjs.com/)上用戶端和伺服器上的 ASP.NET Web API。</span><span class="sxs-lookup"><span data-stu-id="7d09a-228">The Single Page Application (SPA) template creates a sample application that uses JavaScript, HTML 5, and [KnockoutJS](http://knockoutjs.com/) on the client, and ASP.NET Web API on the server.</span></span>

<span data-ttu-id="7d09a-229">SPA 範本的唯一的驗證選項是[個別使用者帳戶](#indauth)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-229">The only authentication option for the SPA template is [Individual User Accounts](#indauth).</span></span>

<span data-ttu-id="7d09a-230">下圖顯示 SPA 範本建立的範例應用程式的初始狀態。</span><span class="sxs-lookup"><span data-stu-id="7d09a-230">The following illustration shows the initial state of the sample application that the SPA template builds.</span></span>

![SPA 範例應用程式](creating-web-projects-in-visual-studio/_static/image14.png)

<span data-ttu-id="7d09a-232">如需如何使用 SPA 範本所建立的應用程式的資訊，請參閱[Web API 的外部驗證服務](../../../web-api/overview/security/external-authentication-services.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-232">For information about how to create an application by using the SPA template, see [Web API - External Authentication Services](../../../web-api/overview/security/external-authentication-services.md).</span></span>

<span data-ttu-id="7d09a-233">如需有關 ASP.NET 單一頁面應用程式，以及使用 KnockoutJS 以外的 JavaScript 架構的其他 SPA 範本的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7d09a-233">For more information about ASP.NET Single Page Applications, and about additional SPA templates that use JavaScript frameworks other than KnockoutJS, see the following resources:</span></span>

- <span data-ttu-id="7d09a-234">[ASP.NET 單一頁面應用程式](../../../single-page-application/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-234">[ASP.NET Single Page Application](../../../single-page-application/index.md).</span></span>
- [<span data-ttu-id="7d09a-235">了解 VS2013 RC SPA 範本中的安全性功能</span><span class="sxs-lookup"><span data-stu-id="7d09a-235">Understanding Security Features in the SPA Template for VS2013 RC</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/09/20/understanding-security-features-in-spa-template.aspx)
- [<span data-ttu-id="7d09a-236">使用 ASP.NET 的現代化、 可回應的 Web 應用程式的單一頁面應用程式： 建置</span><span class="sxs-lookup"><span data-stu-id="7d09a-236">Single-Page Applications: Build Modern, Responsive Web Apps with ASP.NET</span></span>](https://msdn.microsoft.com/magazine/dn463786.aspx)

<a id="facebook"></a>
### <a name="facebook-template"></a><span data-ttu-id="7d09a-237">Facebook 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-237">Facebook Template</span></span>

<span data-ttu-id="7d09a-238">您可以安裝[Visual Studio 擴充功能，提供 Facebook 範本](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-238">You can install a [Visual Studio extension that provides a Facebook template](https://go.microsoft.com/fwlink/?LinkID=509965&amp;clcid=0x409).</span></span> <span data-ttu-id="7d09a-239">此範本會建立範例應用程式是設計用來在 Facebook web 站台內執行。</span><span class="sxs-lookup"><span data-stu-id="7d09a-239">This template creates a sample application that is designed to run inside the Facebook web site.</span></span> <span data-ttu-id="7d09a-240">它以 ASP.NET MVC 為基礎，並使用即時更新功能的 Web API。</span><span class="sxs-lookup"><span data-stu-id="7d09a-240">It is based on ASP.NET MVC and uses Web API for real-time update functionality.</span></span>

<span data-ttu-id="7d09a-241">因為 Facebook 應用程式的 Facebook 站台內執行，並依賴 Facebook 的驗證使用 Facebook 範本沒有驗證選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-241">No authentication options are available for the Facebook template because Facebook applications run within the Facebook site and rely on Facebook's authentication.</span></span>

<span data-ttu-id="7d09a-242">如需 ASP.NET Facebook 應用程式的詳細資訊，請參閱[更新 MVC Facebook API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-242">For more information about ASP.NET Facebook applications, see [Updating the MVC Facebook API](https://blogs.msdn.com/b/webdev/archive/2014/06/10/updating-the-mvc-facebook-api.aspx).</span></span>

<a id="vs2012"></a>
### <a name="visual-studio-2012-templates"></a><span data-ttu-id="7d09a-243">Visual Studio 2012 範本</span><span class="sxs-lookup"><span data-stu-id="7d09a-243">Visual Studio 2012 Templates</span></span>

<span data-ttu-id="7d09a-244">Visual Studio 2013 的 web 專案建立對話方塊不提供存取 Visual Studio 2012 中有可用的一些範例。</span><span class="sxs-lookup"><span data-stu-id="7d09a-244">The Visual Studio 2013 web project creation dialog does not provide access to some templates that were available in Visual Studio 2012.</span></span> <span data-ttu-id="7d09a-245">如果您想要使用其中一個範本，您可以按一下 [Visual Studio 新專案] 對話方塊的左窗格中的 [Visual Studio 2012] 節點。</span><span class="sxs-lookup"><span data-stu-id="7d09a-245">If you want to use one of these templates, you can click the Visual Studio 2012 node in the left pane of the Visual Studio New Project dialog box.</span></span>

![Visual Studio 2012 範本](creating-web-projects-in-visual-studio/_static/image15.png)

<span data-ttu-id="7d09a-247">**Visual Studio 2012**節點可讓您選擇下列 web 範本，您不需要存取預設的範本清單中的 Visual Studio 2013:</span><span class="sxs-lookup"><span data-stu-id="7d09a-247">The **Visual Studio 2012** node lets you choose the following web templates that you don't have access to in the default list of templates for Visual Studio 2013:</span></span>

- <span data-ttu-id="7d09a-248">ASP.NET MVC 4 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7d09a-248">ASP.NET MVC 4 Web Application</span></span>
- <span data-ttu-id="7d09a-249">ASP.NET Dynamic Data 實體 Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="7d09a-249">ASP.NET Dynamic Data Entities Web Application</span></span>
- <span data-ttu-id="7d09a-250">ASP.NET AJAX 伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="7d09a-250">ASP.NET AJAX Server Control</span></span>
- <span data-ttu-id="7d09a-251">ASP.NET AJAX 伺服器控制項擴充項</span><span class="sxs-lookup"><span data-stu-id="7d09a-251">ASP.NET AJAX Server Control Extender</span></span>
- <span data-ttu-id="7d09a-252">ASP.NET 伺服器控制項</span><span class="sxs-lookup"><span data-stu-id="7d09a-252">ASP.NET Server Control</span></span>

<a id="bootstrap"></a>
## <a name="bootstrap-in-the-visual-studio-2013-web-project-templates"></a><span data-ttu-id="7d09a-253">啟動 Visual Studio 2013 的 web 專案範本中</span><span class="sxs-lookup"><span data-stu-id="7d09a-253">Bootstrap in the Visual Studio 2013 web project templates</span></span>

<span data-ttu-id="7d09a-254">Visual Studio 2013 專案範本使用[Bootstrap](http://getbootstrap.com/)，Twitter 所建立的配置和佈景主題的架構。</span><span class="sxs-lookup"><span data-stu-id="7d09a-254">The Visual Studio 2013 project templates use [Bootstrap](http://getbootstrap.com/), a layout and theming framework created by Twitter.</span></span> <span data-ttu-id="7d09a-255">啟動程序會使用 CSS3 提供回應式設計，這表示配置可以動態的方式適應不同的瀏覽器視窗大小。</span><span class="sxs-lookup"><span data-stu-id="7d09a-255">Bootstrap uses CSS3 to provide responsive design, which means layouts can dynamically adapt to different browser window sizes.</span></span> <span data-ttu-id="7d09a-256">比方說，寬的瀏覽器視窗中 Web Form 範本所建立的 [首頁] 頁面看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="7d09a-256">For example, in a wide browser window the home page created by the Web Forms template looks like the following illustration:</span></span>

![Web Form 範本應用程式首頁](creating-web-projects-in-visual-studio/_static/image16.png)

<span data-ttu-id="7d09a-258">使視窗變窄，並且水平排列的資料行移至垂直排列：</span><span class="sxs-lookup"><span data-stu-id="7d09a-258">Make the window narrower, and the horizontally arranged columns move into vertical arrangement:</span></span>

![啟動程序的垂直資料行的排列方式](creating-web-projects-in-visual-studio/_static/image17.png)

<span data-ttu-id="7d09a-260">稍微，縮小視窗和水平的上方功能表會轉換成您可以按一下以展開為垂直方向的功能表上的圖示：</span><span class="sxs-lookup"><span data-stu-id="7d09a-260">Narrow the window a little more, and the horizontal top menu turns into an icon that you can click to expand into a vertically oriented menu:</span></span>

![啟動程序的功能表項目圖示](creating-web-projects-in-visual-studio/_static/image18.png)

![啟動程序的垂直功能表](creating-web-projects-in-visual-studio/_static/image19.png)

<span data-ttu-id="7d09a-263">您也可以使用啟動程序的主題設定功能，以輕鬆地促使應用程式的外觀及操作中的變更。</span><span class="sxs-lookup"><span data-stu-id="7d09a-263">You can also use Bootstrap's theming feature to easily effect a change in the application's look and feel.</span></span> <span data-ttu-id="7d09a-264">例如，您可以執行下列步驟來變更佈景主題。</span><span class="sxs-lookup"><span data-stu-id="7d09a-264">For example, you can do the following steps to change the theme.</span></span>

1. <span data-ttu-id="7d09a-265">在您的瀏覽器，移至[ http://Bootswatch.com ](http://Bootswatch.com)，選擇的佈景主題，然後按一下**下載**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-265">In your browser, go to [http://Bootswatch.com](http://Bootswatch.com), choose a theme, and then click **Download**.</span></span> <span data-ttu-id="7d09a-266">(這會下載*bootstrap.min.css*依預設，如果您想要檢查 CSS 程式碼，取得*bootstrap.css*而不是縮短的版本。)</span><span class="sxs-lookup"><span data-stu-id="7d09a-266">(This downloads *bootstrap.min.css* by default; if you want to examine the CSS code, get *bootstrap.css* instead of the minified version.)</span></span>
2. <span data-ttu-id="7d09a-267">複製下載的 CSS 檔案的內容。</span><span class="sxs-lookup"><span data-stu-id="7d09a-267">Copy the contents of the downloaded CSS file.</span></span>
3. <span data-ttu-id="7d09a-268">在 Visual Studio 中，建立新**樣式表**檔名為*bootstrap theme.css*中*內容*到其中的資料夾，然後貼上下載 CSS 程式碼。</span><span class="sxs-lookup"><span data-stu-id="7d09a-268">In Visual Studio, create a new **Style Sheet** file named *bootstrap-theme.css* in the *Content* folder and paste the downloaded CSS code into it.</span></span>
4. <span data-ttu-id="7d09a-269">開啟*應用程式\_Start/Bundle.config*並變更*bootstrap.css*至*bootstrap theme.css*。</span><span class="sxs-lookup"><span data-stu-id="7d09a-269">Open *App\_Start/Bundle.config* and change *bootstrap.css* to *bootstrap-theme.css*.</span></span>

<span data-ttu-id="7d09a-270">同樣地，請執行專案，並在應用程式有新的外觀。</span><span class="sxs-lookup"><span data-stu-id="7d09a-270">Run the project again, and the application has a new look.</span></span> <span data-ttu-id="7d09a-271">下圖顯示 Amelia 佈景主題的效果：</span><span class="sxs-lookup"><span data-stu-id="7d09a-271">The following illustration shows the effect of the Amelia theme:</span></span>

![啟動程序 Amelia 佈景主題](creating-web-projects-in-visual-studio/_static/image20.png)

<span data-ttu-id="7d09a-273">許多的啟動程序主題，提供了免費和付費版本。</span><span class="sxs-lookup"><span data-stu-id="7d09a-273">Many Bootstrap themes are available, both free and premium versions.</span></span> <span data-ttu-id="7d09a-274">啟動程序也會提供各種不同的 UI 元件，例如[下拉式清單](http://twitter.github.io/bootstrap/components.html#dropdowns)，[按鈕群組](http://twitter.github.io/bootstrap/components.html#buttonGroups)，和[圖示](http://twitter.github.io/bootstrap/base-css.html#images)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-274">Bootstrap also offers a wide variety of UI components, such as [drop-downs](http://twitter.github.io/bootstrap/components.html#dropdowns), [button groups](http://twitter.github.io/bootstrap/components.html#buttonGroups), and [icons](http://twitter.github.io/bootstrap/base-css.html#images).</span></span> <span data-ttu-id="7d09a-275">如需有關啟動程序的詳細資訊，請參閱[啟動程序的站台](http://twitter.github.io/bootstrap/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-275">For more information about Bootstrap, see [the Bootstrap site](http://twitter.github.io/bootstrap/).</span></span>

<span data-ttu-id="7d09a-276">如果您在 Visual Studio 中使用 Web Form 設計工具，請注意，設計工具不支援 CSS3，因此不會正確地顯示所有的啟動程序的主題或回應式配置變更的效果。</span><span class="sxs-lookup"><span data-stu-id="7d09a-276">If you use the Web Forms designer in Visual Studio, note that the designer doesn't support CSS3, so it doesn't accurately show all the effects of Bootstrap themes or responsive layout changes.</span></span> <span data-ttu-id="7d09a-277">不過，Web Form 頁面會顯示正確的瀏覽器檢視時。</span><span class="sxs-lookup"><span data-stu-id="7d09a-277">However, the Web Forms pages will display correctly when viewed with a browser.</span></span>

<a id="add"></a>
## <a name="adding-support-for-additional-frameworks"></a><span data-ttu-id="7d09a-278">新增其他架構的支援</span><span class="sxs-lookup"><span data-stu-id="7d09a-278">Adding Support for Additional Frameworks</span></span>

<span data-ttu-id="7d09a-279">當您選取範本時，會自動選取範本所用的 framework(s) 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-279">When you select a template, the check box for the framework(s) used by the template is automatically selected.</span></span> <span data-ttu-id="7d09a-280">例如，如果您選取**Web Form**範本， **Web Form**核取方塊已選取，而且您無法取消選取。</span><span class="sxs-lookup"><span data-stu-id="7d09a-280">For example, if you select the **Web Forms** template, the **Web Forms** check box is selected and you can't clear it.</span></span>

![選取的範本](creating-web-projects-in-visual-studio/_static/image21.png)

![將架構](creating-web-projects-in-visual-studio/_static/image22.png)

<span data-ttu-id="7d09a-283">您可以選取核取方塊以建立專案時加入支援該 framework 不包含範本中的架構。</span><span class="sxs-lookup"><span data-stu-id="7d09a-283">You can select the check box for a framework that isn't included in the template in order to add support for that framework when the project is created.</span></span> <span data-ttu-id="7d09a-284">例如，若要啟用 Web Form *.aspx*頁，當您已選取 [MVC] 範本中，選取**Web Form**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-284">For example, to enable the use of Web Forms *.aspx* pages when you've selected the MVC template, select the **Web Forms** check box.</span></span> <span data-ttu-id="7d09a-285">當您使用 Web Form 範本時，請啟用 MVC，請按一下或**MVC**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-285">Or to enable MVC when you're using the Web Forms template, click the **MVC** check box.</span></span> <span data-ttu-id="7d09a-286">新增一種架構，可讓設計階段，以及執行階段支援。</span><span class="sxs-lookup"><span data-stu-id="7d09a-286">Adding a framework enables design-time as well as run-time support.</span></span> <span data-ttu-id="7d09a-287">例如，如果您將 MVC 支援加入至 Web Form 專案時，您將能夠 scaffold 控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="7d09a-287">For example, if you add MVC support to a Web Forms project, you will be able to scaffold controllers and views.</span></span>

<span data-ttu-id="7d09a-288">如果您結合 Web Form 和 MVC 專案中，並啟用[Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx)在 Web Form，可能會有未預期的路由問題其中一個 URL 有多個可能的目標。</span><span class="sxs-lookup"><span data-stu-id="7d09a-288">If you combine Web Forms and MVC in a project and enable [Friendly URLs](http://www.hanselman.com/blog/IntroducingASPNETFriendlyUrlsCleanerURLsEasierRoutingAndMobileViewsForASPNETWebForms.aspx) in Web Forms, there might be unexpected routing problems where one URL has multiple possible targets.</span></span> <span data-ttu-id="7d09a-289">第一次所定義的路由更高的優先順序。</span><span class="sxs-lookup"><span data-stu-id="7d09a-289">The routes that are defined first will take precedence.</span></span> <span data-ttu-id="7d09a-290">例如，如果您有`Home`控制站和*Home.aspx*  頁面上， `http://contoso.com/home` URL 將會移至*Home.aspx*如果您呼叫`EnableFriendlyUrls`方法之前呼叫`MapRoute`方法中的*RouteConfig.cs*，或使用相同的 URL 將會前往的預設檢視您`Home`控制器，如果您呼叫`MapRoute`之前`EnableFriendlyUrls`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-290">For example, if you have a `Home` controller and a *Home.aspx* page, the `http://contoso.com/home` URL will go to *Home.aspx* if you call the `EnableFriendlyUrls` method before you call the `MapRoute` method in *RouteConfig.cs*, or the same URL will go to the default view for your `Home` controller if you call `MapRoute` before `EnableFriendlyUrls`.</span></span>

<span data-ttu-id="7d09a-291">加入一種架構不會新增任何範例應用程式的功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-291">Adding a framework does not add any sample application functionality.</span></span> <span data-ttu-id="7d09a-292">例如，如果您加入 Web Form 支援時您不選取 [MVC] 範本， *Default.aspx*建立首頁上的檔案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-292">For example, if you add Web Forms support when you've selected the MVC template, no *Default.aspx* home page file is created.</span></span> <span data-ttu-id="7d09a-293">會加入只資料夾、 檔案和支援架構所需的參考。</span><span class="sxs-lookup"><span data-stu-id="7d09a-293">Only the folders, files, and references required to support the framework are added.</span></span> <span data-ttu-id="7d09a-294">基於這個原因，新增架構不會變更範本所建立的範例應用程式中的程式碼會實作的驗證選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-294">For that reason, adding frameworks doesn't change authentication options, which are implemented by code in sample applications created by the templates.</span></span> <span data-ttu-id="7d09a-295">例如，如果您選取 空白的範本，並新增 Web Form 或 MVC 支援，**設定驗證**仍會停用按鈕。</span><span class="sxs-lookup"><span data-stu-id="7d09a-295">For example, if you select the Empty template and add Web Forms or MVC support, the **Configure Authentication** button will still be disabled.</span></span>

<span data-ttu-id="7d09a-296">下列各節簡短描述每個核取方塊的效果。</span><span class="sxs-lookup"><span data-stu-id="7d09a-296">The following sections describe briefly the effect of each check box.</span></span>

### <a name="add-web-forms-support"></a><span data-ttu-id="7d09a-297">新增 Web Form 支援</span><span class="sxs-lookup"><span data-stu-id="7d09a-297">Add Web Forms Support</span></span>

<span data-ttu-id="7d09a-298">建立空白*應用程式\_資料*和*模型*資料夾和*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-298">Creates empty *App\_Data* and *Models* folders and a *Global.asax* file.</span></span> <span data-ttu-id="7d09a-299">選取 Web Form 核取方塊可不讓任何其他範本的差異，因此，這些已經被建立空白的範本，以外的所有範本。</span><span class="sxs-lookup"><span data-stu-id="7d09a-299">These are already created by all templates other than the Empty template, so selecting the Web Forms check box makes no difference for other templates.</span></span>

<span data-ttu-id="7d09a-300">根據預設，但是當您將 Web Form 支援新增至其他範本選取易記的 Url 不會自動啟用 Web Form 核取方塊時，Web Form 範本啟用易記 Url。</span><span class="sxs-lookup"><span data-stu-id="7d09a-300">The Web Forms template enables Friendly URLs by default, but when you add Web Forms support to other templates by selecting the Web Forms check box Friendly URLs are not automatically enabled.</span></span>

### <a name="add-mvc-support"></a><span data-ttu-id="7d09a-301">新增 MVC 支援</span><span class="sxs-lookup"><span data-stu-id="7d09a-301">Add MVC Support</span></span>

<span data-ttu-id="7d09a-302">安裝 MVC、 Razor、 和網頁的 NuGet 封裝時，會建立空白*應用程式\_資料*，*控制器*，*模型*，和*檢視*資料夾建立*應用程式\_啟動*資料夾*RouteConfig.cs*檔案，並建立*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-302">Installs MVC, Razor, and WebPages NuGet packages, creates empty *App\_Data*, *Controllers*, *Models*, and *Views* folders, creates *App\_Start* folder with *RouteConfig.cs* file, and creates *Global.asax* file.</span></span>

### <a name="add-web-api-support"></a><span data-ttu-id="7d09a-303">加入 Web 應用程式開發介面支援</span><span class="sxs-lookup"><span data-stu-id="7d09a-303">Add Web API Support</span></span>

<span data-ttu-id="7d09a-304">安裝 WebApi 和 Newtonsoft.Json NuGet 封裝時，會建立空白*應用程式\_資料*，*控制器*，和*模型*資料夾建立*應用程式\_啟動*資料夾*WebApiConfig.cs*檔案，並建立*Global.asax*檔案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-304">Installs WebApi and Newtonsoft.Json NuGet packages, creates empty *App\_Data*, *Controllers*, and *Models* folders, creates *App\_Start* folder with *WebApiConfig.cs* file, and creates *Global.asax* file.</span></span>

<a id="auth"></a>
## <a name="authentication-methods"></a><span data-ttu-id="7d09a-305">驗證方法</span><span class="sxs-lookup"><span data-stu-id="7d09a-305">Authentication Methods</span></span>

<span data-ttu-id="7d09a-306">Visual Studio 2013 提供多種的驗證選項，Web Form、 MVC、 和 Web API 範本：</span><span class="sxs-lookup"><span data-stu-id="7d09a-306">Visual Studio 2013 offers several authentication options for the Web Forms, MVC, and Web API templates:</span></span>

- [<span data-ttu-id="7d09a-307">無驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-307">No Authentication</span></span>](#noauth)
- <span data-ttu-id="7d09a-308">[個別使用者帳戶](#indauth)(ASP.NET Identity，先前稱為 ASP.NET 成員資格)</span><span class="sxs-lookup"><span data-stu-id="7d09a-308">[Individual User Accounts](#indauth) (ASP.NET Identity, formerly known as ASP.NET membership)</span></span>
- <span data-ttu-id="7d09a-309">[組織帳戶](#orgauth)（Windows Server Active Directory 或 Azure Active Directory）</span><span class="sxs-lookup"><span data-stu-id="7d09a-309">[Organizational Accounts](#orgauth) (Windows Server Active Directory or Azure Active Directory)</span></span>
- <span data-ttu-id="7d09a-310">[Windows 驗證](#winauth)（內部網路）</span><span class="sxs-lookup"><span data-stu-id="7d09a-310">[Windows Authentication](#winauth) (Intranet)</span></span>

![設定驗證對話方塊](creating-web-projects-in-visual-studio/_static/image23.png)

<a id="noauth"></a>

### <a name="no-authentication"></a><span data-ttu-id="7d09a-312">無驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-312">No Authentication</span></span>

<span data-ttu-id="7d09a-313">如果您選取**非驗證**，範例應用程式會包含任何的網頁登入，不表示使用者已登入 UI，沒有實體類別的成員資格資料庫，並沒有成員資格資料庫的連接字串。</span><span class="sxs-lookup"><span data-stu-id="7d09a-313">If you select **No Authentication**, the sample application will contain no web pages for logging in, no UI indicating who is logged in, no entity classes for a membership database, and no connection string for a membership database.</span></span>

<a id="indauth"></a>
### <a name="individual-user-accounts"></a><span data-ttu-id="7d09a-314">個別使用者帳戶</span><span class="sxs-lookup"><span data-stu-id="7d09a-314">Individual User Accounts</span></span>

<span data-ttu-id="7d09a-315">如果您選取**個別使用者帳戶**，範例應用程式會設定為使用 ASP.NET Identity （之前稱為 ASP.NET 成員資格） 進行使用者驗證。</span><span class="sxs-lookup"><span data-stu-id="7d09a-315">If you select **Individual User Accounts**, the sample application will be configured to use ASP.NET Identity (formerly known as ASP.NET membership) for user authentication.</span></span> <span data-ttu-id="7d09a-316">ASP.NET 識別可讓使用者帳戶，藉由建立站台上的使用者名稱和密碼或登錄登入例如 Facebook、 Google、 Microsoft 帳戶或 Twitter 社交提供者。</span><span class="sxs-lookup"><span data-stu-id="7d09a-316">ASP.NET Identity enables a user to register an account, by creating a username and password on the site or by signing in with social providers such as Facebook, Google, Microsoft Account, or Twitter.</span></span> <span data-ttu-id="7d09a-317">在 ASP.NET Identity 中的使用者設定檔的預設資料存放區是 SQL Server LocalDB 資料庫，您可以部署到 SQL Server 或 Azure SQL Database 將生產站台。</span><span class="sxs-lookup"><span data-stu-id="7d09a-317">The default data store for user profiles in ASP.NET Identity is a SQL Server LocalDB database, which you can deploy to SQL Server or Azure SQL Database for the production site.</span></span>

<span data-ttu-id="7d09a-318">在 Visual Studio 2013 這些功能可與 Visual Studio 2012 中的相同，但 ASP.NET 成員資格系統的基礎程式碼已經改寫。</span><span class="sxs-lookup"><span data-stu-id="7d09a-318">In Visual Studio 2013 these features are the same as in Visual Studio 2012, but the underlying code for the ASP.NET membership system has been rewritten.</span></span> <span data-ttu-id="7d09a-319">新的程式碼基底的優點包括：</span><span class="sxs-lookup"><span data-stu-id="7d09a-319">Advantages of the new code base include the following:</span></span>

- <span data-ttu-id="7d09a-320">新的成員資格系統根據[OWIN](http://owin.org/)而不是 ASP.NET 表單驗證模組。</span><span class="sxs-lookup"><span data-stu-id="7d09a-320">The new membership system is based on [OWIN](http://owin.org/) rather than the ASP.NET Forms Authentication module.</span></span> <span data-ttu-id="7d09a-321">這表示，無論您使用 Web Form 或 MVC 在 IIS 中，或您自我裝載的 Web API 或 SignalR，您可以使用相同的驗證機制。</span><span class="sxs-lookup"><span data-stu-id="7d09a-321">This means that you can use the same authentication mechanism whether you're using Web Forms or MVC in IIS, or you're self-hosting Web API or SignalR.</span></span>
- <span data-ttu-id="7d09a-322">新的成員資格資料庫受 Entity Framework Code First，且所有資料表都由實體類別，您可以進行修改。</span><span class="sxs-lookup"><span data-stu-id="7d09a-322">The new membership database is managed by Entity Framework Code First, and all of the tables are represented by entity classes that you can modify.</span></span> <span data-ttu-id="7d09a-323">這表示您可以輕鬆地自訂資料庫結構描述和設定檔相關的 web UI，以符合您自己的需求，而且您可以輕鬆地將使用 Code First 移轉您更新的部署。</span><span class="sxs-lookup"><span data-stu-id="7d09a-323">This means that you can easily customize the database schema and profile-related web UI to fit your own needs, and you can easily deploy your updates using Code First Migrations.</span></span>

<span data-ttu-id="7d09a-324">在新的範本，自動實作新的成員資格系統，它可以是任何.NET 4.5 為目標的專案中手動實作或更新版本。</span><span class="sxs-lookup"><span data-stu-id="7d09a-324">The new membership system is implemented automatically in the new templates, and it can be implemented manually in any project that targets .NET 4.5 or later.</span></span>

<span data-ttu-id="7d09a-325">如果您要建立網際網路網站，也就是主要用於外部客戶，ASP.NET Identity 是不錯的選擇。</span><span class="sxs-lookup"><span data-stu-id="7d09a-325">ASP.NET Identity is a good choice if you are creating an Internet web site which is mainly for external customers.</span></span> <span data-ttu-id="7d09a-326">如果您的組織使用 Active Directory 或 Office 365 和您想要建立單一登入的專案，讓員工和商業夥伴**組織帳戶**選項可能就是比較好的選擇。</span><span class="sxs-lookup"><span data-stu-id="7d09a-326">If your organization uses Active Directory or Office 365 and you want to create a project that enables single-sign-on for employees and business partners, the **Organizational Accounts** option might be a better choice.</span></span>

<span data-ttu-id="7d09a-327">如需個別的使用者帳戶選項的詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7d09a-327">For more information about the Individual User Accounts option, see the following resources:</span></span>

- <span data-ttu-id="7d09a-328">[www.asp.net/identity](../../../identity/index.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-328">[www.asp.net/identity](../../../identity/index.md).</span></span> <span data-ttu-id="7d09a-329">ASP.NET 識別的相關文件在 ASP.NET 網站上。</span><span class="sxs-lookup"><span data-stu-id="7d09a-329">Documentation about ASP.NET Identity on the ASP.NET web site.</span></span>
- <span data-ttu-id="7d09a-330">[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-330">[Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../../../mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span> <span data-ttu-id="7d09a-331">也會顯示如何自訂使用者設定檔資料。</span><span class="sxs-lookup"><span data-stu-id="7d09a-331">Also shows how to customize user profile data.</span></span>
- [<span data-ttu-id="7d09a-332">Web 應用程式開發介面的外部驗證服務</span><span class="sxs-lookup"><span data-stu-id="7d09a-332">Web API - External Authentication Services</span></span>](../../../web-api/overview/security/external-authentication-services.md)
- [<span data-ttu-id="7d09a-333">將外部登入加入至您的 ASP.NET 應用程式，在 Visual Studio 2013</span><span class="sxs-lookup"><span data-stu-id="7d09a-333">Adding External Logins to your ASP.NET application in Visual Studio 2013</span></span>](https://blogs.msdn.com/b/webdev/archive/2013/06/27/adding-external-logins-to-your-asp-net-application-in-visual-studio-2013.aspx)

<a id="orgauth"></a>
### <a name="organizational-accounts"></a><span data-ttu-id="7d09a-334">組織帳戶</span><span class="sxs-lookup"><span data-stu-id="7d09a-334">Organizational Accounts</span></span>

<span data-ttu-id="7d09a-335">如果您選取**組織帳戶**，範例應用程式會設定為使用 Windows Identity Foundation (WIF) Azure Active Directory (Azure AD，其中包括 Office 365) 中的使用者帳戶為基礎的驗證或Windows Server Active Directory。</span><span class="sxs-lookup"><span data-stu-id="7d09a-335">If you select **Organizational Accounts**, the sample application will be configured to use Windows Identity Foundation (WIF) for authentication based on user accounts in Azure Active Directory (Azure AD, which includes Office 365) or Windows Server Active Directory.</span></span> <span data-ttu-id="7d09a-336">如需詳細資訊，請參閱[組織帳戶的驗證選項](#orgauthoptions)本主題稍後。</span><span class="sxs-lookup"><span data-stu-id="7d09a-336">For more information, see [Organizational account authentication options](#orgauthoptions) later in this topic.</span></span>

<a id="winauth"></a>
### <a name="windows-authentication"></a><span data-ttu-id="7d09a-337">Windows 驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-337">Windows Authentication</span></span>

<span data-ttu-id="7d09a-338">如果您選取**Windows 驗證**，範例應用程式會設定為使用 Windows 驗證的 IIS 模組進行驗證。</span><span class="sxs-lookup"><span data-stu-id="7d09a-338">If you select **Windows Authentication**, the sample application will be configured to use the Windows Authentication IIS module for authentication.</span></span> <span data-ttu-id="7d09a-339">應用程式會顯示 Active directory 或本機電腦帳戶登入 Windows，但不會包括使用者註冊或登入 UI 的網域和使用者識別碼。</span><span class="sxs-lookup"><span data-stu-id="7d09a-339">The application will display the domain and user ID of the Active directory or local machine account that is logged into Windows but won't include user registration or log-in UI.</span></span> <span data-ttu-id="7d09a-340">此選項適用於內部網路網站。</span><span class="sxs-lookup"><span data-stu-id="7d09a-340">This option is intended for Intranet web sites.</span></span>

<span data-ttu-id="7d09a-341">或者，您可以建立內部網路網站選擇使用 AD 驗證[在內部部署組織帳戶下的選項](#orgauthonprem)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-341">Alternatively, you can create an Intranet site that uses AD authentication by choosing the [On-Premises option under Organizational Accounts](#orgauthonprem).</span></span> <span data-ttu-id="7d09a-342">[內部] 選項會使用 Windows Identity Foundation (WIF)，而不是 Windows 驗證模組。</span><span class="sxs-lookup"><span data-stu-id="7d09a-342">The On-Premises option uses Windows Identity Foundation (WIF) instead of the Windows Authentication module.</span></span> <span data-ttu-id="7d09a-343">若要設定 [內部] 選項，需要一些額外的步驟，但是 WIF 可讓不提供 Windows 驗證模組與功能。</span><span class="sxs-lookup"><span data-stu-id="7d09a-343">Some additional steps are required in order to set up the On-Premises option, but WIF enables features that aren't available with the Windows Authentication module.</span></span> <span data-ttu-id="7d09a-344">比方說，您可以使用 WIF 應用程式存取設定 Active Directory 和查詢目錄資料。</span><span class="sxs-lookup"><span data-stu-id="7d09a-344">For example, with WIF you can configure application access in Active Directory and query directory data.</span></span>

<a id="orgauthoptions"></a>
## <a name="organizational-account-authentication-options"></a><span data-ttu-id="7d09a-345">組織帳戶的驗證選項</span><span class="sxs-lookup"><span data-stu-id="7d09a-345">Organizational account authentication options</span></span>

<span data-ttu-id="7d09a-346">**設定驗證**對話方塊可讓您為 Azure Active Directory (Azure AD，其中包括 Office 365) 或 Windows Server Active Directory (AD) 帳戶驗證的數個選項：</span><span class="sxs-lookup"><span data-stu-id="7d09a-346">The **Configure Authentication** dialog gives you several options for Azure Active Directory (Azure AD, which includes Office 365) or Windows Server Active Directory (AD) account authentication:</span></span>

- <span data-ttu-id="7d09a-347">[雲端的單一組織](#orgauthsingle)(Azure AD 或使用 Azure AD 目錄整合的 AD)</span><span class="sxs-lookup"><span data-stu-id="7d09a-347">[Cloud - Single Organization](#orgauthsingle) (Azure AD, or AD using directory integration with Azure AD)</span></span>
- <span data-ttu-id="7d09a-348">[雲端為多組織](#orgauthmulti)(Azure AD 或使用 Azure AD 目錄整合的 AD)</span><span class="sxs-lookup"><span data-stu-id="7d09a-348">[Cloud - Multi Organization](#orgauthmulti) (Azure AD, or AD using directory integration with Azure AD)</span></span>
- <span data-ttu-id="7d09a-349">[在內部部署](#orgauthonprem)(AD)</span><span class="sxs-lookup"><span data-stu-id="7d09a-349">[On-Premises](#orgauthonprem) (AD)</span></span>

<span data-ttu-id="7d09a-350">如果您想要在其中一個 Azure AD 選項再試一次，但還沒有帳戶，[按一下這裡註冊 Azure AD 帳戶](https://go.microsoft.com/fwlink/?LinkId=309942)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-350">If you want to try one of the Azure AD options but don't have an account yet, [click here to sign up for a Azure AD account](https://go.microsoft.com/fwlink/?LinkId=309942).</span></span>

> [!NOTE]
> <span data-ttu-id="7d09a-351">如果您選擇其中一個 Azure AD 選項，您的專案需要一個資料庫，您必須登入的全域系統管理員帳戶的 Azure AD 租用戶。</span><span class="sxs-lookup"><span data-stu-id="7d09a-351">If you choose one of the Azure AD options, your project requires a database and you have to sign in to a global administrator account for your Azure AD tenant.</span></span> <span data-ttu-id="7d09a-352">輸入組織帳戶的名稱和密碼 (例如， admin@contoso.onmicrosoft.com) 具有 Azure AD 租用戶的系統管理權限。</span><span class="sxs-lookup"><span data-stu-id="7d09a-352">Enter the name and password for an organizational account (for example, admin@contoso.onmicrosoft.com) that has administrative permissions for your Azure AD tenant.</span></span>
> 
> <span data-ttu-id="7d09a-353">**不要輸入 Microsoft 帳戶認證 (例如， contoso@hotmail.com) 在登入對話方塊。**</span><span class="sxs-lookup"><span data-stu-id="7d09a-353">**Don't enter credentials for a Microsoft account (for example, contoso@hotmail.com) in the sign-in dialog box.**</span></span>


<a id="orgauthsingle"></a>
### <a name="cloud---single-organization-authentication"></a><span data-ttu-id="7d09a-354">雲端的單一組織驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-354">Cloud - Single Organization Authentication</span></span>

![單一組織驗證](creating-web-projects-in-visual-studio/_static/image24.png)

<span data-ttu-id="7d09a-356">選擇此選項，如果您想要啟用一個 Azure AD 中所定義的使用者帳戶的驗證[租用戶](https://technet.microsoft.com/library/jj573650.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-356">Choose this option if you want to enable authentication for user accounts that are defined in one Azure AD [tenant](https://technet.microsoft.com/library/jj573650.aspx).</span></span> <span data-ttu-id="7d09a-357">例如，網站為 contoso.com，它將會對可用 contoso.onmicrosoft.com 租用戶中是 Contoso 公司的員工。</span><span class="sxs-lookup"><span data-stu-id="7d09a-357">For example, the site is contoso.com and it will be made available to employees of the Contoso Company who are in the contoso.onmicrosoft.com tenant.</span></span> <span data-ttu-id="7d09a-358">您將無法設定以允許使用者從其他租用戶存取應用程式的 Azure AD。</span><span class="sxs-lookup"><span data-stu-id="7d09a-358">You won't be able to configure Azure AD to allow users from other tenants to access the application.</span></span>

#### <a name="domain"></a><span data-ttu-id="7d09a-359">Domain</span><span class="sxs-lookup"><span data-stu-id="7d09a-359">Domain</span></span>

<span data-ttu-id="7d09a-360">輸入您想要設定的應用程式中，例如 Azure AD 網域： `contoso.onmicrosoft.com`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-360">Enter the Azure AD domain that you want to set up the application in, for example: `contoso.onmicrosoft.com`.</span></span> <span data-ttu-id="7d09a-361">如果您有[自訂網域](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/)，例如`contoso.com`而不是`contoso.onmicrosoft.com`，您可以在此處輸入。</span><span class="sxs-lookup"><span data-stu-id="7d09a-361">If you have a [custom domain](http://www.cloudidentity.com/blog/2013/04/14/adding-a-custom-domain-to-your-windows-azure-ad/), such as `contoso.com` instead of `contoso.onmicrosoft.com`, you can enter that here.</span></span>

#### <a name="access-level"></a><span data-ttu-id="7d09a-362">存取層級</span><span class="sxs-lookup"><span data-stu-id="7d09a-362">Access Level</span></span>

<span data-ttu-id="7d09a-363">如果應用程式需要查詢或使用 Graph API 來更新目錄資訊，請選擇**單一登入，讀取目錄資料**或**單一登入，讀取和寫入目錄資料**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-363">If the application needs to query or update directory information by using the Graph API, choose **Single Sign-On, Read Directory Data** or **Single Sign-On, Read and Write Directory Data**.</span></span> <span data-ttu-id="7d09a-364">否則，請選擇**單一登入**。</span><span class="sxs-lookup"><span data-stu-id="7d09a-364">Otherwise, choose **Single Sign-On**.</span></span> <span data-ttu-id="7d09a-365">如需詳細資訊，請參閱[應用程式的存取層級](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels)和[使用查詢 Azure AD Graph API](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-365">For more information, see [Application Access Levels](https://msdn.microsoft.com/library/windowsazure/b08d91fa-6a64-4deb-92f4-f5857add9ed8#BKMK_AccessLevels) and [Using the Graph API to Query Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151791.aspx).</span></span>

#### <a name="application-id-uri"></a><span data-ttu-id="7d09a-366">應用程式識別碼 URI</span><span class="sxs-lookup"><span data-stu-id="7d09a-366">Application ID URI</span></span>

<span data-ttu-id="7d09a-367">根據預設，範本會建立為您的應用程式識別碼 URI，將專案名稱附加至 Azure AD 網域。</span><span class="sxs-lookup"><span data-stu-id="7d09a-367">By default, the template creates an application ID URI for you by appending the project name to the Azure AD domain.</span></span> <span data-ttu-id="7d09a-368">例如，如果專案名稱是`Example`，且網域為`contoso.onmicrosoft.com`，應用程式識別碼 URI 會變成`https://contoso.onmicrosoft.com/Example`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-368">For example, if the project name is `Example` and the domain is `contoso.onmicrosoft.com`, the application ID URI becomes `https://contoso.onmicrosoft.com/Example`.</span></span> <span data-ttu-id="7d09a-369">如果您想要手動指定應用程式識別碼 URI，展開**更多選項**區段，並在文字方塊中輸入應用程式識別碼 URI。</span><span class="sxs-lookup"><span data-stu-id="7d09a-369">If you want to manually specify the application ID URI, expand the **More Options** section and enter the application ID URI in the text box.</span></span> <span data-ttu-id="7d09a-370">應用程式識別碼 URI 的開頭必須`https://`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-370">The application ID URI must begin with `https://`.</span></span>

<span data-ttu-id="7d09a-371">根據預設，如果已佈建 Azure AD 中的應用程式有相同的應用程式識別碼 URI 與 Visual Studio 使用的專案，專案將會連接到現有的應用程式，而不是佈建一個新。</span><span class="sxs-lookup"><span data-stu-id="7d09a-371">By default, if an application that is already provisioned in Azure AD has the same application ID URI as the one that Visual Studio is using for the project, the project will be connected to the existing application instead of provisioning a new one.</span></span> <span data-ttu-id="7d09a-372">如果您想要在此情況下佈建的新應用程式，清除**覆寫應用程式項目，如果已經存在一個具有相同識別碼**核取方塊。</span><span class="sxs-lookup"><span data-stu-id="7d09a-372">If you want a new application to be provisioned in that case, clear the **Overwrite the application entry if one with the same ID already exists** check box.</span></span>

<span data-ttu-id="7d09a-373">如果**覆寫**清除核取方塊，以及 Visual Studio 找到現有的應用程式使用相同的應用程式識別碼 URI，它會建立新的 URI 附加至 URI，它將使用的數字。</span><span class="sxs-lookup"><span data-stu-id="7d09a-373">If the **Overwrite** check box is cleared, and Visual Studio finds an existing application with the same application ID URI, it creates a new URI by appending a number to the URI it was going to use.</span></span> <span data-ttu-id="7d09a-374">例如，假設 專案名稱是`Example`空白文字方塊中，您清除**覆寫**核取方塊，以及 Azure AD 租用戶已有應用程式的 uri `https://contoso.onmicrosoft.com/Example`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-374">For example, suppose the project name is `Example`, you leave the text box blank, you clear the **Overwrite** check box, and the Azure AD tenant already has an application with the URI `https://contoso.onmicrosoft.com/Example`.</span></span> <span data-ttu-id="7d09a-375">在此情況下，新的應用程式將會佈建與應用程式識別碼 URI 像`https://contoso.onmicrosoft.com/Example_20130619330903`。</span><span class="sxs-lookup"><span data-stu-id="7d09a-375">In that case, a new application will be provisioned with an application ID URI like `https://contoso.onmicrosoft.com/Example_20130619330903`.</span></span>

#### <a name="provisioning-the-application-in-azure-ad"></a><span data-ttu-id="7d09a-376">佈建 Azure AD 中的應用程式</span><span class="sxs-lookup"><span data-stu-id="7d09a-376">Provisioning the application in Azure AD</span></span>

<span data-ttu-id="7d09a-377">若要佈建 Azure AD 中的應用程式，或將專案連接至現有的應用程式，Visual Studio 會需要網域全域管理員認證。</span><span class="sxs-lookup"><span data-stu-id="7d09a-377">In order to provision the application in Azure AD or connect the project to an existing application, Visual Studio needs the credentials of a global administrator for the domain.</span></span> <span data-ttu-id="7d09a-378">當您按一下**確定**中**設定驗證**對話方塊中，系統會提示您輸入使用者名稱及您所指定之網域的全域系統管理員密碼。</span><span class="sxs-lookup"><span data-stu-id="7d09a-378">When you click **OK** in the **Configure Authentication** dialog box, you are prompted for the user name and password of a global administrator for the domain you specified.</span></span> <span data-ttu-id="7d09a-379">稍後，當您按一下**建立專案**中**新增 ASP.NET 專案** 對話方塊中，Visual Studio 會佈建 Azure AD 中的應用程式。</span><span class="sxs-lookup"><span data-stu-id="7d09a-379">Later, when you click **Create Project** in the **New ASP.NET Project** dialog, Visual Studio provisions the application in Azure AD.</span></span> <span data-ttu-id="7d09a-380">請注意，此程序的一部分 Visual Studio 用戶端祕密值中嵌入的 Web.config 檔案中建立之後的一年後到期。</span><span class="sxs-lookup"><span data-stu-id="7d09a-380">Note that as part of this process Visual Studio embeds client secret values in the Web.config file that expire one year after creation.</span></span>

<span data-ttu-id="7d09a-381">如需有關如何建立使用的應用程式資訊**雲端-單一組織**驗證，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7d09a-381">For information about how to create applications that use **Cloud - Single Organization** authentication, see the following resources:</span></span>

- [<span data-ttu-id="7d09a-382">Azure 驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-382">Azure Authentication</span></span>](../2012/windows-azure-authentication.md)
- [<span data-ttu-id="7d09a-383">登入加入至 Web 應用程式，使用 Azure AD</span><span class="sxs-lookup"><span data-stu-id="7d09a-383">Adding Sign-On to Your Web Application Using Azure AD</span></span>](https://msdn.microsoft.com/library/windowsazure/dn151790.aspx)
- [<span data-ttu-id="7d09a-384">使用 Azure Active Dirctory 開發 ASP.NET 應用程式</span><span class="sxs-lookup"><span data-stu-id="7d09a-384">Developing ASP.NET Apps with Azure Active Directory</span></span>](../../../identity/overview/getting-started/developing-aspnet-apps-with-windows-azure-active-directory.md)
- [<span data-ttu-id="7d09a-385">保護 Azure AD 的 ASP.NET Web API 和 Microsoft OWIN 元件</span><span class="sxs-lookup"><span data-stu-id="7d09a-385">Secure ASP.NET Web API with Azure AD and Microsoft OWIN Components</span></span>](https://msdn.microsoft.com/magazine/dn463788.aspx)

<span data-ttu-id="7d09a-386">教學課程有尚未更新 Visual Studio 2013。教學課程引導您以手動方式執行的某些系統會自動在 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="7d09a-386">The tutorials have not yet been updated for Visual Studio 2013; some of what the tutorials direct you to do manually is automated in Visual Studio 2013.</span></span>

<a id="orgauthmulti"></a>
### <a name="cloud---multi-organization-authentication"></a><span data-ttu-id="7d09a-387">雲端-多重組織驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-387">Cloud - Multi Organization Authentication</span></span>

![多個組織的驗證](creating-web-projects-in-visual-studio/_static/image25.png)

<span data-ttu-id="7d09a-389">選擇此選項，如果您想要啟用多個 Azure AD 中所定義的使用者帳戶的驗證[租用戶](https://technet.microsoft.com/library/jj573650.aspx)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-389">Choose this option if you want to enable authentication for user accounts that are defined in multiple Azure AD [tenants](https://technet.microsoft.com/library/jj573650.aspx).</span></span> <span data-ttu-id="7d09a-390">例如，網站為 contoso.com，它將會對可用 contoso.onmicrosoft.com 租用戶中是 Contoso 公司的員工和員工的 Fabrikam 公司 fabrikam.onmicrosoft.com 租用戶中。</span><span class="sxs-lookup"><span data-stu-id="7d09a-390">For example, the site is contoso.com and it will be made available to employees of the Contoso Company who are in the contoso.onmicrosoft.com tenant, and employees of the Fabrikam Company who are in the fabrikam.onmicrosoft.com tenant.</span></span>

<span data-ttu-id="7d09a-391">您輸入的設定和應用程式佈建步驟，類似[單一組織驗證](#orgauthsingle)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-391">The settings that you enter and the application provisioning step are similar to [single organization authentication](#orgauthsingle).</span></span>

<span data-ttu-id="7d09a-392">如需有關如何建立使用的應用程式資訊**雲端為多組織**驗證，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="7d09a-392">For information about how to create applications that use **Cloud - Multi Organization** authentication, see the following resources:</span></span>

- <span data-ttu-id="7d09a-393">[很容易與 Azure Active Directory，ASP.NET Web 應用程式整合&amp;Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) Active Directory 團隊部落格上。</span><span class="sxs-lookup"><span data-stu-id="7d09a-393">[Easy Web App Integration with Azure Active Directory, ASP.NET &amp; Visual Studio](https://blogs.msdn.com/b/active_directory_team_blog/archive/2013/06/26/improved-windows-azure-active-directory-integration-with-asp-net-amp-visual-studio.aspx) on the Active Directory Team blog.</span></span>
- <span data-ttu-id="7d09a-394">[開發多租用戶 Web 應用程式與 Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx)教學課程。</span><span class="sxs-lookup"><span data-stu-id="7d09a-394">[Developing Multi-Tenant Web Applications with Azure AD](https://msdn.microsoft.com/library/windowsazure/dn151789.aspx) tutorial.</span></span> <span data-ttu-id="7d09a-395">本教學課程尚未尚未更新 Visual Studio 2013。某些功能的教學課程會引導您手動執行會自動化 Visual Studio 2013 中。</span><span class="sxs-lookup"><span data-stu-id="7d09a-395">The tutorial hasn't yet been updated for Visual Studio 2013; some of what the tutorial directs you to do manually is automated in Visual Studio 2013.</span></span>
- <span data-ttu-id="7d09a-396">[您必須註冊您的多個組織 ASP.NET 應用程式之前，您可以登入](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-396">[You Have to Sign Up With Your Own Multiple Organizations ASP.NET App Before You Can Sign In](http://www.cloudidentity.com/blog/2013/10/26/you-have-to-sign-up-with-your-own-multiple-organizations-asp-net-app-before-you-can-sign-in/).</span></span> <span data-ttu-id="7d09a-397">建立專案，使用多組織驗證時，會遇到 Vittorio Bertocci，說明如何解決常見的問題人員部落格。</span><span class="sxs-lookup"><span data-stu-id="7d09a-397">Blog by Vittorio Bertocci that explains how to resolve a common problem people encounter when creating a project that uses multi-organization authentication.</span></span>

<a id="orgauthonprem"></a>
### <a name="on-premises-organizational-authentication"></a><span data-ttu-id="7d09a-398">在內部部署組織的驗證</span><span class="sxs-lookup"><span data-stu-id="7d09a-398">On-Premises Organizational Authentication</span></span>

![在內部部署組織的驗證](creating-web-projects-in-visual-studio/_static/image26.png)

<span data-ttu-id="7d09a-400">如果您想要啟用驗證的使用者帳戶會定義在 Windows Server Active Directory (AD)，而且不想使用 Azure AD，請選擇此選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-400">Choose this option if you want to enable authentication for user accounts that are defined in Windows Server Active Directory (AD), and you don't want to use Azure AD.</span></span> <span data-ttu-id="7d09a-401">若要建立內部網路網站或網際網路網站，您可以使用此選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-401">You can use this option to create an Intranet site or an Internet site.</span></span> <span data-ttu-id="7d09a-402">網際網路站台，使用 Active Directory Federation Services (ADFS) 來提供存取至 AD。</span><span class="sxs-lookup"><span data-stu-id="7d09a-402">For an Internet site, use Active Directory Federation Services (ADFS) to provide access to AD.</span></span> <span data-ttu-id="7d09a-403">如需詳細資訊，請參閱[使用 Visual Studio 2013 中的內部組織的驗證選項 (ADFS) 與 ASP.NET](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-403">For more information, see [Use the On-Premises Organizational Authentication Option (ADFS) With ASP.NET in Visual Studio 2013](http://www.cloudidentity.com/blog/2014/02/12/use-the-on-premises-organizational-authentication-option-adfs-with-asp-net-in-visual-studio-2013/).</span></span>

<span data-ttu-id="7d09a-404">對於內部網路網站，或者您可以選擇[Windows 驗證](#winauth)來代替此選項。</span><span class="sxs-lookup"><span data-stu-id="7d09a-404">For an Intranet site, as an alternative you can choose [Windows Authentication](#winauth) instead of this option.</span></span> <span data-ttu-id="7d09a-405">Windows 驗證選項，您不必提供中繼資料文件 URL。</span><span class="sxs-lookup"><span data-stu-id="7d09a-405">For the Windows Authentication option you don't have to provide a metadata document URL.</span></span> <span data-ttu-id="7d09a-406">不過，Windows 驗證沒有提供您可以控制在 Active Directory 中的應用程式存取或查詢目錄資料。</span><span class="sxs-lookup"><span data-stu-id="7d09a-406">However, Windows Authentication does not give you the ability to control application access in Active Directory or to query directory data.</span></span>

#### <a name="on-premises-authority"></a><span data-ttu-id="7d09a-407">在內部部署授權單位</span><span class="sxs-lookup"><span data-stu-id="7d09a-407">On-Premises Authority</span></span>

<span data-ttu-id="7d09a-408">輸入 URL 指向中繼資料文件。</span><span class="sxs-lookup"><span data-stu-id="7d09a-408">Enter a URL that points to the metadata document.</span></span> <span data-ttu-id="7d09a-409">中繼資料文件含有授權單位的座標。</span><span class="sxs-lookup"><span data-stu-id="7d09a-409">The metadata document contains the coordinates of the authority.</span></span> <span data-ttu-id="7d09a-410">應用程式將使用這些座標來推動 web 登入流程。</span><span class="sxs-lookup"><span data-stu-id="7d09a-410">The application will use those coordinates to drive the web sign-on flow.</span></span>

#### <a name="application-id-uri"></a><span data-ttu-id="7d09a-411">應用程式識別碼 URI</span><span class="sxs-lookup"><span data-stu-id="7d09a-411">Application ID URI</span></span>

<span data-ttu-id="7d09a-412">提供 AD 可用來識別此應用程式，或者保留空白，讓 Visual Studio 建立的唯一 URI。</span><span class="sxs-lookup"><span data-stu-id="7d09a-412">Provide a unique URI that AD can use to identify this application, or leave blank to let Visual Studio create one.</span></span>

<a id="nextsteps"></a>
## <a name="next-steps"></a><span data-ttu-id="7d09a-413">後續步驟</span><span class="sxs-lookup"><span data-stu-id="7d09a-413">Next steps</span></span>

<span data-ttu-id="7d09a-414">本文件提供一些基本說明在 Visual Studio 2013 中建立新的 ASP.NET web 專案。</span><span class="sxs-lookup"><span data-stu-id="7d09a-414">This document has provided some basic help for creating a new ASP.NET web project in Visual Studio 2013.</span></span> <span data-ttu-id="7d09a-415">如需使用 Visual studio 針對 web 程式開發的詳細資訊，請參閱[ https://www.asp.net/visual-studio/ ](../../index.md)。</span><span class="sxs-lookup"><span data-stu-id="7d09a-415">For more information about using for Visual Studio for web development, see [https://www.asp.net/visual-studio/](../../index.md).</span></span>
