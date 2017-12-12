---
uid: visual-studio/overview/2012/windows-azure-authentication
title: "Windows Azure Authentication |Microsoft 文件"
author: Rick-Anderson
description: "Windows Azure Active Directory 的 Microsoft ASP.NET 工具可簡化針對裝載 Windows Azure 網站上的 web 應用程式啟用驗證..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2013
ms.topic: article
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: 1cb38d66bd0373159e54abf822fba9c5829774ac
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="windows-azure-authentication"></a><span data-ttu-id="ca3f4-103">Windows Azure 驗證</span><span class="sxs-lookup"><span data-stu-id="ca3f4-103">Windows Azure Authentication</span></span>
====================
<span data-ttu-id="ca3f4-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="ca3f4-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="ca3f4-105">Microsoft ASP.NET 工具的 Windows Azure Active Directory 可簡化針對上主控的 web 應用程式啟用驗證[Windows Azure Web Sites](https://www.windowsazure.com/en-us/home/features/web-sites/)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-105">Microsoft ASP.NET tools for Windows Azure Active Directory makes it simple to enable authentication for web applications hosted on [Windows Azure Web Sites](https://www.windowsazure.com/en-us/home/features/web-sites/).</span></span> <span data-ttu-id="ca3f4-106">您可以使用 Windows Azure 驗證來驗證 Office 365 使用者，從您的組織，從您在內部部署 Active Directory 同步處理的公司帳戶或建立您自己自訂的 Windows Azure Active Directory 網域中的使用者。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-106">You can use Windows Azure Authentication to authenticate Office 365 users from your organization, corporate accounts synced from your on-premise Active Directory or users created in your own custom Windows Azure Active Directory domain.</span></span> <span data-ttu-id="ca3f4-107">啟用 Windows Azure 驗證設定您的應用程式驗證使用者使用單一[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租用戶。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-107">Enabling Windows Azure Authentication configures your application to authenticate users using a single [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.</span></span>
> 
> <span data-ttu-id="ca3f4-108">ASP.NET Windows Azure 驗證工具不支援的雲端服務中的 web 角色，但我們計劃在未來的版本中這麼做。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-108">The ASP.NET Windows Azure Authentication tool is not supported for web roles in a cloud service but we plan to do so in a future release.</span></span> <span data-ttu-id="ca3f4-109">[Windows Identity Foundation](https://msdn.microsoft.com/en-us/library/hh291066(v=VS.110).aspx) (WIF) 適用於 Windows Azure web 角色。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-109">[Windows Identity Foundation](https://msdn.microsoft.com/en-us/library/hh291066(v=VS.110).aspx) (WIF) is supported in Windows Azure web roles.</span></span>
> 
> <span data-ttu-id="ca3f4-110">如需有關如何設定您在內部部署 Active Directory 與 Windows Azure Active Directory 租用戶之間同步處理的詳細資訊，請參閱[實作和管理使用 AD FS 2.0 單一登入](https://technet.microsoft.com/en-us/library/jj205462.aspx)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-110">For details on how to setup synchronization between your on-premise Active Directory and your Windows Azure Active Directory tenant please see [Use AD FS 2.0 to implement and manage single sign-on](https://technet.microsoft.com/en-us/library/jj205462.aspx).</span></span>
> 
> <span data-ttu-id="ca3f4-111">Windows Azure Active Directory 是目前可供[免費預覽服務](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-111">Windows Azure Active Directory is currently available as a [free preview service](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>


## <a name="requirements"></a><span data-ttu-id="ca3f4-112">需求：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-112">Requirements:</span></span>

- <span data-ttu-id="ca3f4-113">Visual Studio 2012 或[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express)</span><span class="sxs-lookup"><span data-stu-id="ca3f4-113">Visual Studio 2012 or [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/en-us/products/express)</span></span>
- <span data-ttu-id="ca3f4-114">[Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)或[Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="ca3f4-114">[Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) or [Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span></span>
- <span data-ttu-id="ca3f4-115">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306)或[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span><span class="sxs-lookup"><span data-stu-id="ca3f4-115">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) or [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span></span>

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a><span data-ttu-id="ca3f4-116">使用 Visual Studio 2012 中建立 ASP.NET Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3f4-116">Create an ASP.NET Web Application with Visual Studio 2012</span></span>

<span data-ttu-id="ca3f4-117">您可以使用 Visual Studio 2012 中建立任何 Web 應用程式，本教學課程使用 ASP.NET MVC 內部網路範本。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-117">You can create any Web Application with Visual Studio 2012, this tutorial uses the ASP.NET MVC intranet template.</span></span>

1. <span data-ttu-id="ca3f4-118">建立新 ASP.NET MVC 4 內部網路應用程式，並接受所有預設值。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-118">Create a new ASP.NET MVC 4 Intranet Application and accept all the defaults.</span></span> <span data-ttu-id="ca3f4-119">(它必須是 In**周遊**網路而不在**輸入**net 專案)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-119">(It must be an In **tra** net and not In **ter** net project).</span></span>  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a><span data-ttu-id="ca3f4-120">啟用 Window Azure 驗證 （當您的租用戶的全域系統管理員）</span><span class="sxs-lookup"><span data-stu-id="ca3f4-120">Enable Window Azure Authentication (When you are a Global Administrator of the Tenet)</span></span>

<span data-ttu-id="ca3f4-121">如果您沒有現有的 Windows Azure Active Directory 租用戶 （例如，透過現有的 Office 365 帳戶） 您可以建立新的租用戶註冊[新的 Windows Azure Active Directory 帳戶](http://g.microsoftonline.com/0AX00en/5)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-121">If you do not have an existing Windows Azure Active Directory tenant (For example, through an existing Office 365 account) you can create a new tenant by signing up for a [new Windows Azure Active Directory account](http://g.microsoftonline.com/0AX00en/5).</span></span>

1. <span data-ttu-id="ca3f4-122">從 [專案] 功能表選取**啟用 Windows Azure Authentication**:</span><span class="sxs-lookup"><span data-stu-id="ca3f4-122">From the Project menu select **Enable Windows Azure Authentication**:</span></span>  
  
 ![](windows-azure-authentication/_static/image2.png)

2. <span data-ttu-id="ca3f4-123">Windows Azure Active Directory 租用戶 (例如 contoso.onmicrosoft.com) 輸入的網域，然後按一下**啟用**:</span><span class="sxs-lookup"><span data-stu-id="ca3f4-123">Enter the domain for your Windows Azure Active Directory tenant (for example, contoso.onmicrosoft.com) and click **Enable**:</span></span>

![](windows-azure-authentication/_static/image3.png)

3. <span data-ttu-id="ca3f4-124">在 Web 驗證對話方塊登入 Windows Azure Active Directory 租用戶系統管理員身分：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-124">In the Web Authentication dialog sign in as an administrator for your Windows Azure Active Directory tenant:</span></span>  
  
 ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a><span data-ttu-id="ca3f4-125">非系統管理員的租用戶啟用 Windows Azure</span><span class="sxs-lookup"><span data-stu-id="ca3f4-125">Enable Window Azure by a non-administrator of the Tenet</span></span>

<span data-ttu-id="ca3f4-126">如果您沒有 Windows Azure Active Directory 租用戶全域管理員權限，您可以取消勾選的核取方塊佈建應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-126">If you do not have Global Administrator privilege for your Windows Azure Active Directory tenant, you can uncheck the checkbox for provisioning the application.</span></span>

![](windows-azure-authentication/_static/image6.png)

<span data-ttu-id="ca3f4-127">對話方塊會顯示**網域**，**應用程式主體識別碼**和**回覆 URL**所需的佈建與 Azure Active Directory 應用程式租用戶。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-127">The dialog will display the **Domain**, **Application Principal Id** and **Reply URL** which are required for provisioning the application with an Azure Active Directory tenet.</span></span> <span data-ttu-id="ca3f4-128">您需要將此資訊提供給擁有足夠的權限，來佈建應用程式的任何人。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-128">You need to give this information to someone who has sufficient privilege to provision the application.</span></span> <span data-ttu-id="ca3f4-129">請參閱[如何實作單一登入 Windows Azure Active Directory-ASP.NET 應用程式與](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)如需有關如何使用指令程式來手動建立服務主體的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-129">See[How to implement single sign-on with Windows Azure Active Directory - ASP.NET Application](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) for details on how to use cmdlet to create the service principal manually.</span></span>  
<span data-ttu-id="ca3f4-130">一旦應用程式已成功佈建，您可以按一下**繼續與所選擇的設定更新 web.config**。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-130">Once the application has been successfully provisioned, you can click on **Continue to update web.config with the selected settings**.</span></span> <span data-ttu-id="ca3f4-131">如果您想要繼續開發應用程式，等候要發生的情況，您可以按一下 [佈建**關閉]，請記住專案檔中的設定**。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-131">If you want to continue developing the application while waiting for provisioning to happen, you can click **Close to remember the settings in project file**.</span></span> <span data-ttu-id="ca3f4-132">下次您叫用 啟用 Windows Azure 驗證，並取消佈建的核取方塊，您會看到相同的設定，然後您可以按一下**繼續**，然後按 **套用這些設定，在 web.config 中**.</span><span class="sxs-lookup"><span data-stu-id="ca3f4-132">The next time you invoke Enable Windows Azure Authentication and uncheck the provisioning checkbox, you will see the same settings and you can click **Continue**, then click, **Apply these settings in web.config**.</span></span>

1. <span data-ttu-id="ca3f4-133">您的應用程式設定成使用 Windows Azure 驗證及使用 Windows Azure Active Directory 佈建稍候。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-133">Wait while your application is configured for Windows Azure Authentication and provisioned with Windows Azure Active Directory.</span></span>
2. <span data-ttu-id="ca3f4-134">一旦您的應用程式已啟用 Windows Azure 驗證，按一下 **關閉：**</span><span class="sxs-lookup"><span data-stu-id="ca3f4-134">Once Windows Azure Authentication has been enabled for your application, click **Close:**</span></span> 

    ![](windows-azure-authentication/_static/image7.png)
3. <span data-ttu-id="ca3f4-135">按 F5 執行您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-135">Hit F5 to run your application.</span></span> <span data-ttu-id="ca3f4-136">您應該會自動取得重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-136">You should automatically get redirected to login page.</span></span> <span data-ttu-id="ca3f4-137">您可以使用目錄租用戶的使用者認證來登入應用程式...</span><span class="sxs-lookup"><span data-stu-id="ca3f4-137">Use the directory tenet user credentials to login to the application..</span></span>  

    ![](windows-azure-authentication/_static/image1.jpg)
4. <span data-ttu-id="ca3f4-138">您的應用程式目前使用的自我簽署的測試憑證，因為您會收到警告從憑證不受信任的憑證授權單位所發出的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-138">Because your application is currently using a self-signed test certificate you will receive a warning from the browser that the certificate was not issued by a trusted certificate authority.</span></span>

    <span data-ttu-id="ca3f4-139">這個警告可以放心忽略在本機開發期間按一下**繼續瀏覽此網站：**</span><span class="sxs-lookup"><span data-stu-id="ca3f4-139">This warning can be safely ignored during local development by clicking **Continue to this website:**</span></span> 

    ![](windows-azure-authentication/_static/image8.png)
5. <span data-ttu-id="ca3f4-140">您現在已成功登入使用 Windows Azure Authentication 應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="ca3f4-140">You have now successfully logged in to your application using Windows Azure Authentication!</span></span>

    ![](windows-azure-authentication/_static/image2.jpg)

<span data-ttu-id="ca3f4-141">驗證啟用 Windows Azure 應用程式進行下列變更：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-141">Enabling Windows Azure authentication makes the following changes to your application:</span></span>

- <span data-ttu-id="ca3f4-142">反跨網站要求偽造 ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) 類別 (*應用程式\_Start\AntiXsrfConfig.cs* ) 加入至專案。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-142">An Anti-Cross-Site Request Forgery ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) class ( *App\_Start\AntiXsrfConfig.cs* ) is added to your project.</span></span>
- <span data-ttu-id="ca3f4-143">NuGet 封裝`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`加入至專案。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-143">The NuGet packages `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` is added to your project.</span></span>
- <span data-ttu-id="ca3f4-144">設定 Windows Identity Foundation 應用程式中的將設定為接受來自您的 Windows Azure Active Directory 租用戶的安全性權杖。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-144">Windows Identity Foundation settings in your application will be configured to accept security tokens from your Windows Azure Active Directory tenant.</span></span> <span data-ttu-id="ca3f4-145">按一下以查看變更的展開的檢視下面的影像*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-145">Click on the image below to see an expanded view of the changes made to the *Web.config* file.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.png)
- <span data-ttu-id="ca3f4-146">將佈建服務主體中您的 Windows Azure Active Directory 租用戶應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-146">A service principal for your application in your Windows Azure Active Directory tenant will be provisioned.</span></span>
- <span data-ttu-id="ca3f4-147">啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-147">HTTPS is enabled.</span></span>

## <a name="deploy-the-application-to-windows-azure"></a><span data-ttu-id="ca3f4-148">部署至 Windows Azure 應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3f4-148">Deploy the application to Windows Azure</span></span>

<span data-ttu-id="ca3f4-149">如需完整指示，請參閱[ASP.NET Web 應用程式至 Windows Azure 網站部署](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-149">For complete instructions, see [Deploying an ASP.NET Web Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="ca3f4-150">若要發行使用 Windows Azure 驗證至 Azure 網站的應用程式：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-150">To publish an application using Windows Azure Authentication to an Azure Web Site:</span></span>

1. <span data-ttu-id="ca3f4-151">您的應用程式上按一下滑鼠右鍵，然後選取**發行：**</span><span class="sxs-lookup"><span data-stu-id="ca3f4-151">Right click on your application and select **Publish:**</span></span> 

    ![](windows-azure-authentication/_static/image3.jpg)
2. <span data-ttu-id="ca3f4-152">從 [發行 Web] 對話方塊下載並匯入發行設定檔的 Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-152">From the Publish Web dialog download and import a publishing profile for your Azure Web Site.</span></span>

    ![](windows-azure-authentication/_static/image4.jpg)
3. <span data-ttu-id="ca3f4-153">**連接**索引標籤上顯示**目的地 URL** （公用對您應用程式的 URL)。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-153">The **Connection** tab shows the **Destination URL** (the public facing URL for your application).</span></span> <span data-ttu-id="ca3f4-154">按一下**驗證連線**來測試您的連線：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-154">Click **Validate Connection** to test your connection:</span></span>

    ![](windows-azure-authentication/_static/image5.jpg)
4. <span data-ttu-id="ca3f4-155">如果您已經發行這個 Azure Web 站台之前，請考慮檢查**移除目的端的其他檔案**完全發行設定，以確保您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-155">If you have published to this Azure Web Site previously consider checking the **Remove additional files at destination** setting to ensure your application publishes cleanly.</span></span> <span data-ttu-id="ca3f4-156">請注意**啟用 Windows Azure Authentication**就 slected 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-156">Notice the **Enable Windows Azure Authentication** check box is slected.</span></span>  

    ![](windows-azure-authentication/_static/image10.png)
5. <span data-ttu-id="ca3f4-157">選擇性： 在**預覽**索引標籤處按一下**啟動預覽**來查看部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-157">Optional: On the **Preview** tab click **Start Preview** to see the files deployed.</span></span>

    ![](windows-azure-authentication/_static/image6.jpg)
6. <span data-ttu-id="ca3f4-158">按一下**發行。**</span><span class="sxs-lookup"><span data-stu-id="ca3f4-158">Click **Publish.**</span></span>

    <span data-ttu-id="ca3f4-159">系統會提示您啟用 Windows Azure 驗證目標主機。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-159">You will be prompted to enable Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="ca3f4-160">按一下**啟用**繼續：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-160">Click **Enable** to continue:</span></span>

    ![](windows-azure-authentication/_static/image11.png)
7. <span data-ttu-id="ca3f4-161">輸入您的 Windows Azure Active Directory 租用戶系統管理員認證：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-161">Enter your administrator credentials for your Windows Azure Active Directory tenant:</span></span>

    ![](windows-azure-authentication/_static/image7.jpg)
8. <span data-ttu-id="ca3f4-162">一旦已成功發行您的應用程式，瀏覽器會開啟至已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-162">Once your application has been successfully published, a browser will open to the published web site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="ca3f4-163">可能需要最多五分鐘的時間 （通常是必須更少） 完全佈建的 Windows Azure Active Directory 後啟用 Windows Azure 驗證目標主機的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-163">It may take up to five minutes (typically must less) for your application to be fully provisioned with Windows Azure Active Directory after enabling Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="ca3f4-164">當您第一次執行應用程式如果您收到錯誤 ACS50001： 找不到信賴憑證者的合作對象名稱 [領域]，然後稍候幾分鐘，再嘗試執行的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-164">When you first run your application if you receive error ACS50001: Relying party with name ‘[realm]' was not found, then wait a few minutes and try running the application again.</span></span>
9. <span data-ttu-id="ca3f4-165">出現提示時，您的目錄中的使用者身分登入：</span><span class="sxs-lookup"><span data-stu-id="ca3f4-165">When prompted, log in as a user in your directory:</span></span>

    ![](windows-azure-authentication/_static/image8.jpg)
10. <span data-ttu-id="ca3f4-166">您現在已成功登入您的 Azure 裝載使用 Windows Azure Authentication 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-166">You have now successfully logged into your Azure hosted application using Windows Azure Authentication.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a><span data-ttu-id="ca3f4-167">已知問題</span><span class="sxs-lookup"><span data-stu-id="ca3f4-167">Known Issues</span></span>

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a><span data-ttu-id="ca3f4-168">以角色為基礎的授權失敗時使用 Windows Azure Authentication < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-168">Role-based authorization fails when using Windows Azure Authentication<o:p></o:p></span></span>

<span data-ttu-id="ca3f4-169">Windows Azure 驗證目前未提供必要的角色宣告，因此可以執行以角色為基礎的授權。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-169">Windows Azure Authentication does not currently provide the necessary role claim so that role-based authorization can be performed.</span></span> <span data-ttu-id="ca3f4-170">已驗證使用者的角色必須從 Windows Azure Active Directory。 < o: p>< >< 手動擷取 / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-170">The role of the authenticated user must be manually retrieved from Windows Azure Active Directory.<o:p></o:p></span></span>

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="ca3f4-171">瀏覽至 Windows Azure 驗證會產生錯誤的應用程式 [ACS20016 登入的使用者 (live.com) 的網域不符合任何允許網域的 STS] < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-171">Browsing to an application with Windows Azure Authentication results in the error "ACS20016 The domain of the logged in user (live.com) does not match any allowed domain of this STS"<o:p></o:p></span></span>

<span data-ttu-id="ca3f4-172">如果您已經登入 Microsoft 帳戶 （例如 hotmail.com、 live.com、 outlook.com），而且您嘗試存取已啟用 Windows Azure Authentication 應用程式可能會出現 400 錯誤回應因為您的 Microsoft 帳戶的網域無法辨識由 Windows Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-172">If you are already logged in to a Microsoft Account (for example hotmail.com, live.com, outlook.com) and you attempt to access an application that has enabled Windows Azure Authentication you may get a 400 error response because the domain of your Microsoft Account is not recognized by Windows Azure Active Directory.</span></span> <span data-ttu-id="ca3f4-173">若要記錄至應用程式時，登出您的 Microsoft 帳戶第一次。 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-173">To log into the application, log out from your Microsoft Account first.<o:p></o:p></span></span>

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a><span data-ttu-id="ca3f4-174">無結果中 < o: p>< >< accounts.accesscontrol.windows.net 憑證的憑證驗證錯誤以外登入啟用的 Windows Azure 驗證的應用程式和 X509CertificateValidationMode / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-174">Logging into an application with Windows Azure Authentication enabled and a X509CertificateValidationMode other than None results in certificate validation errors for the accounts.accesscontrol.windows.net certificate<o:p></o:p></span></span>

<span data-ttu-id="ca3f4-175">不需要憑證驗證，而且應該會保持停用。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-175">Certificate validation is not required and should be left disabled.</span></span> <span data-ttu-id="ca3f4-176">簽發者憑證的指紋驗證 WSFederationAuthenticationModule。 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-176">The thumbprint of the issuer certificate is validated by the WSFederationAuthenticationModule.<o:p></o:p></span></span>

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="ca3f4-177">當您嘗試啟用 Windows Azure 驗證時 Web 驗證對話方塊中會顯示錯誤 「 ACS20016： 登入的使用者 (contoso.onmicrosoft.com) 的網域不符合此 STS 的任何允許的網域。 」< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-177">When attempting to enable Windows Azure Authentication the Web Authentication dialog shows the error "ACS20016: The domain of the logged in user (contoso.onmicrosoft.com) does not match any allowed domain of this STS."<o:p></o:p></span></span>

<span data-ttu-id="ca3f4-178">您先前已成功登入使用其他 Windows Azure Active Directory 帳戶，從相同的 Visual Studio 處理序內時，您可能會看到這個錯誤。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-178">You may see this error when you have previously successfully logged in using a different Windows Azure Active Directory account from within the same Visual Studio process.</span></span> <span data-ttu-id="ca3f4-179">從指定的帳戶登出或重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-179">Log out from the specified account or restart Visual Studio.</span></span> <span data-ttu-id="ca3f4-180">如果您先前已登入並選取 「 讓我保持登中 」 的選項則您可能必須清除瀏覽器 cookie。 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-180">If you previously logged in and selected the option to "Keep me signed in" then you may need to clear your browser cookies.<o:p></o:p></span></span>

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a><span data-ttu-id="ca3f4-181">ACS20012： 要求不是有效的 WS-同盟通訊協定訊息 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-181">ACS20012: The request is not a valid WS-Federation protocol message <o:p></o:p></span></span>

<span data-ttu-id="ca3f4-182">如果您已登入其中一項 Azure 服務的一些其他 Microsoft ID，這可能會發生。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-182">This can happen if you are already logged in with some other Microsoft ID to one of the Azure services.</span></span> <span data-ttu-id="ca3f4-183">使用私用瀏覽器視窗，例如在 IE 中的 InPrivate 或 Incognito 在 Chrome 中的，或清除所有 cookie。</span><span class="sxs-lookup"><span data-stu-id="ca3f4-183">Use Private browser window like InPrivate in IE or Incognito in Chrome or clear all the cookies.</span></span> <span data-ttu-id="ca3f4-184">< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="ca3f4-184"><o:p></o:p></span></span>

## <a name="additional-resources"></a><span data-ttu-id="ca3f4-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="ca3f4-185">Additional Resources</span></span>

- <span data-ttu-id="ca3f4-186">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="ca3f4-186">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="ca3f4-187">Windows Azure 功能： 身分識別</span><span class="sxs-lookup"><span data-stu-id="ca3f4-187">Windows Azure Features: Identity</span></span>](https://docs.microsoft.com/azure/active-directory/)
- [<span data-ttu-id="ca3f4-188">TechNet: Windows Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca3f4-188">TechNet: Windows Azure Active Directory</span></span>](https://technet.microsoft.com/en-us/library/hh967619.aspx)
- [<span data-ttu-id="ca3f4-189">Windows Azure Active Directory： 為您的組織開發應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3f4-189">Windows Azure Active Directory: Develop Apps for your organization</span></span>](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [<span data-ttu-id="ca3f4-190">Windows Azure Active Directory： 多個組織中開發應用程式</span><span class="sxs-lookup"><span data-stu-id="ca3f4-190">Windows Azure Active Directory: Develop Apps for multiple organizations</span></span>](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [<span data-ttu-id="ca3f4-191">如何實作單一登入與 Windows Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="ca3f4-191">How to implement single sign-on with Windows Azure Active Directory</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- <span data-ttu-id="ca3f4-192">[單一登入 windows Azure Active Directory： 深入了解](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx)– Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="ca3f4-192">[Single Sign-On with Windows Azure Active Directory: a Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="ca3f4-193">實作和管理使用 AD FS 2.0 單一登入</span><span class="sxs-lookup"><span data-stu-id="ca3f4-193">Use AD FS 2.0 to implement and manage single sign-on</span></span>](https://technet.microsoft.com/en-us/library/jj205462.aspx)
