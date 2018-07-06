---
uid: visual-studio/overview/2012/windows-azure-authentication
title: Windows Azure 驗證 |Microsoft Docs
author: Rick-Anderson
description: Windows Azure Active Directory 的 Microsoft ASP.NET 工具可以輕鬆啟用 Windows Azure 網站上所裝載的 web 應用程式的驗證...
ms.author: aspnetcontent
ms.date: 02/20/2013
ms.assetid: a3cef801-a54b-4ebd-93c3-55764e2e14b1
msc.legacyurl: /visual-studio/overview/2012/windows-azure-authentication
msc.type: authoredcontent
ms.openlocfilehash: e3443fb627dc8d7d5011341828556b4c13836170
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37812806"
---
<a name="windows-azure-authentication"></a><span data-ttu-id="92e49-103">Windows Azure 驗證</span><span class="sxs-lookup"><span data-stu-id="92e49-103">Windows Azure Authentication</span></span>
====================
<span data-ttu-id="92e49-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="92e49-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="92e49-105">Microsoft ASP.NET 工具針對 Windows Azure Active Directory 輕鬆地啟用上裝載的 web 應用程式的驗證[Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/)。</span><span class="sxs-lookup"><span data-stu-id="92e49-105">Microsoft ASP.NET tools for Windows Azure Active Directory makes it simple to enable authentication for web applications hosted on [Windows Azure Web Sites](https://www.windowsazure.com/home/features/web-sites/).</span></span> <span data-ttu-id="92e49-106">您可以使用 Windows Azure 驗證來驗證您的組織，從您內部部署 Active Directory 同步處理的公司帳戶或建立您自己自訂的 Windows Azure Active Directory 網域中的使用者從 Office 365 使用者。</span><span class="sxs-lookup"><span data-stu-id="92e49-106">You can use Windows Azure Authentication to authenticate Office 365 users from your organization, corporate accounts synced from your on-premise Active Directory or users created in your own custom Windows Azure Active Directory domain.</span></span> <span data-ttu-id="92e49-107">啟用 Windows Azure 驗證設定您的應用程式，來驗證使用者使用單一[Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/)租用戶。</span><span class="sxs-lookup"><span data-stu-id="92e49-107">Enabling Windows Azure Authentication configures your application to authenticate users using a single [Windows Azure Active Directory](https://docs.microsoft.com/azure/active-directory/) tenant.</span></span>
> 
> <span data-ttu-id="92e49-108">ASP.NET 的 Windows Azure 驗證工具不支援雲端服務中的 web 角色，但我們計劃在未來的版本中這麼做。</span><span class="sxs-lookup"><span data-stu-id="92e49-108">The ASP.NET Windows Azure Authentication tool is not supported for web roles in a cloud service but we plan to do so in a future release.</span></span> <span data-ttu-id="92e49-109">[Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) 支援在 Windows Azure web 角色中。</span><span class="sxs-lookup"><span data-stu-id="92e49-109">[Windows Identity Foundation](https://msdn.microsoft.com/library/hh291066(v=VS.110).aspx) (WIF) is supported in Windows Azure web roles.</span></span>
> 
> <span data-ttu-id="92e49-110">如需如何設定您的內部部署 Active Directory 與 Windows Azure Active Directory 租用戶之間的同步處理的詳細資訊，請參閱[來實作和管理使用 AD FS 2.0 單一登入](https://technet.microsoft.com/library/jj205462.aspx)。</span><span class="sxs-lookup"><span data-stu-id="92e49-110">For details on how to setup synchronization between your on-premise Active Directory and your Windows Azure Active Directory tenant please see [Use AD FS 2.0 to implement and manage single sign-on](https://technet.microsoft.com/library/jj205462.aspx).</span></span>
> 
> <span data-ttu-id="92e49-111">Windows Azure Active Directory 是目前可供[免費預覽服務](https://azure.microsoft.com/free/?WT.mc_id=A443DD604)。</span><span class="sxs-lookup"><span data-stu-id="92e49-111">Windows Azure Active Directory is currently available as a [free preview service](https://azure.microsoft.com/free/?WT.mc_id=A443DD604).</span></span>


## <a name="requirements"></a><span data-ttu-id="92e49-112">需求：</span><span class="sxs-lookup"><span data-stu-id="92e49-112">Requirements:</span></span>

- <span data-ttu-id="92e49-113">Visual Studio 2012 或[Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)</span><span class="sxs-lookup"><span data-stu-id="92e49-113">Visual Studio 2012 or [Visual Studio Express 2012](https://www.microsoft.com/visualstudio/11/products/express)</span></span>
- <span data-ttu-id="92e49-114">[Web 工具擴充功能適用於 Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409)或[Web 工具延伸模組，適用於 Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span><span class="sxs-lookup"><span data-stu-id="92e49-114">[Web Tools Extensions for Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282228&amp;clcid=0x409) or [Web Tools Extensions for Visual Studio Express 2012](https://go.microsoft.com/fwlink/?LinkID=282231&amp;clcid=0x409)</span></span>
- <span data-ttu-id="92e49-115">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306)或[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span><span class="sxs-lookup"><span data-stu-id="92e49-115">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://go.microsoft.com/fwlink/?LinkID=282306) or [Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio Express 2012 for Web](https://go.microsoft.com/fwlink/?LinkId=282652)</span></span>

## <a name="create-an-aspnet-web-application-with-visual-studio-2012"></a><span data-ttu-id="92e49-116">使用 Visual Studio 2012 中建立 ASP.NET Web 應用程式</span><span class="sxs-lookup"><span data-stu-id="92e49-116">Create an ASP.NET Web Application with Visual Studio 2012</span></span>

<span data-ttu-id="92e49-117">您可以使用 Visual Studio 2012 中建立任何 Web 應用程式，本教學課程使用 ASP.NET MVC 內部網路範本。</span><span class="sxs-lookup"><span data-stu-id="92e49-117">You can create any Web Application with Visual Studio 2012, this tutorial uses the ASP.NET MVC intranet template.</span></span>

1. <span data-ttu-id="92e49-118">建立新 ASP.NET MVC 4 內部網路應用程式，並接受所有預設值。</span><span class="sxs-lookup"><span data-stu-id="92e49-118">Create a new ASP.NET MVC 4 Intranet Application and accept all the defaults.</span></span> <span data-ttu-id="92e49-119">(它必須是 In **tra** net 和 not In **ter** net 專案)。</span><span class="sxs-lookup"><span data-stu-id="92e49-119">(It must be an In **tra** net and not In **ter** net project).</span></span>  
     ![](windows-azure-authentication/_static/image1.png)

## <a name="enable-window-azure-authentication-when-you-are-a-global-administrator-of-the-tenet"></a><span data-ttu-id="92e49-120">Window Azure 驗證 （當啟用您的租用戶的全域系統管理員）</span><span class="sxs-lookup"><span data-stu-id="92e49-120">Enable Window Azure Authentication (When you are a Global Administrator of the Tenet)</span></span>

<span data-ttu-id="92e49-121">如果您沒有現有的 Windows Azure Active Directory 租用戶 （例如，透過現有的 Office 365 帳戶） 您可以建立新的租用戶註冊[新的 Windows Azure Active Directory 帳戶](http://g.microsoftonline.com/0AX00en/5)。</span><span class="sxs-lookup"><span data-stu-id="92e49-121">If you do not have an existing Windows Azure Active Directory tenant (For example, through an existing Office 365 account) you can create a new tenant by signing up for a [new Windows Azure Active Directory account](http://g.microsoftonline.com/0AX00en/5).</span></span>

1. <span data-ttu-id="92e49-122">從 [專案] 功能表中選取**啟用 Windows Azure 驗證**:</span><span class="sxs-lookup"><span data-stu-id="92e49-122">From the Project menu select **Enable Windows Azure Authentication**:</span></span>  
  
   ![](windows-azure-authentication/_static/image2.png)

2. <span data-ttu-id="92e49-123">輸入您的 Windows Azure Active Directory 租用戶 (例如，contoso.onmicrosoft.com) 的網域，然後按一下**啟用**:</span><span class="sxs-lookup"><span data-stu-id="92e49-123">Enter the domain for your Windows Azure Active Directory tenant (for example, contoso.onmicrosoft.com) and click **Enable**:</span></span>

![](windows-azure-authentication/_static/image3.png)

3. <span data-ttu-id="92e49-124">在 Web 驗證對話方塊登入 Windows Azure Active Directory 租用戶系統管理員身分：</span><span class="sxs-lookup"><span data-stu-id="92e49-124">In the Web Authentication dialog sign in as an administrator for your Windows Azure Active Directory tenant:</span></span>  
  
   ![](windows-azure-authentication/_static/image4.png)

![](windows-azure-authentication/_static/image5.png)

## <a name="enable-window-azure-by-a-non-administrator-of-the-tenet"></a><span data-ttu-id="92e49-125">非系統管理員的原則來啟用 Windows Azure</span><span class="sxs-lookup"><span data-stu-id="92e49-125">Enable Window Azure by a non-administrator of the Tenet</span></span>

<span data-ttu-id="92e49-126">如果您沒有 Windows Azure Active Directory 租用戶的全域系統管理員權限，您可以取消選取佈建應用程式的核取方塊。</span><span class="sxs-lookup"><span data-stu-id="92e49-126">If you do not have Global Administrator privilege for your Windows Azure Active Directory tenant, you can uncheck the checkbox for provisioning the application.</span></span>

![](windows-azure-authentication/_static/image6.png)

<span data-ttu-id="92e49-127">對話方塊會顯示**網域**，**應用程式主體識別碼**並**回覆 URL**所需的佈建與 Azure Active Directory 應用程式租用戶。</span><span class="sxs-lookup"><span data-stu-id="92e49-127">The dialog will display the **Domain**, **Application Principal Id** and **Reply URL** which are required for provisioning the application with an Azure Active Directory tenet.</span></span> <span data-ttu-id="92e49-128">您必須擁有足夠的權限，來佈建應用程式的任何人提供這項資訊。</span><span class="sxs-lookup"><span data-stu-id="92e49-128">You need to give this information to someone who has sufficient privilege to provision the application.</span></span> <span data-ttu-id="92e49-129">請參閱[如何實作單一登入 Windows Azure Active Directory-ASP.NET 應用程式與](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)如需有關如何使用 cmdlet 來手動建立服務主體。</span><span class="sxs-lookup"><span data-stu-id="92e49-129">See[How to implement single sign-on with Windows Azure Active Directory - ASP.NET Application](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect) for details on how to use cmdlet to create the service principal manually.</span></span>  
<span data-ttu-id="92e49-130">一旦應用程式已成功佈建，您可以按一下**持續與選取的設定更新 web.config**。</span><span class="sxs-lookup"><span data-stu-id="92e49-130">Once the application has been successfully provisioned, you can click on **Continue to update web.config with the selected settings**.</span></span> <span data-ttu-id="92e49-131">如果您想要繼續開發應用程式，同時等候發生，您可以按一下 [佈建**關閉]，請記住專案檔案中的設定**。</span><span class="sxs-lookup"><span data-stu-id="92e49-131">If you want to continue developing the application while waiting for provisioning to happen, you can click **Close to remember the settings in project file**.</span></span> <span data-ttu-id="92e49-132">在下一次叫用 啟用 Windows Azure 驗證，並取消佈建的核取方塊，您會看到相同的設定，您可以按一下**繼續**，然後按一下 **套用這些設定，在 web.config**.</span><span class="sxs-lookup"><span data-stu-id="92e49-132">The next time you invoke Enable Windows Azure Authentication and uncheck the provisioning checkbox, you will see the same settings and you can click **Continue**, then click, **Apply these settings in web.config**.</span></span>

1. <span data-ttu-id="92e49-133">稍候，正在設定 Windows Azure 驗證和使用 Windows Azure Active Directory 佈建應用程式。</span><span class="sxs-lookup"><span data-stu-id="92e49-133">Wait while your application is configured for Windows Azure Authentication and provisioned with Windows Azure Active Directory.</span></span>
2. <span data-ttu-id="92e49-134">Windows Azure 驗證啟用您的應用程式之後，按一下 **關閉：**</span><span class="sxs-lookup"><span data-stu-id="92e49-134">Once Windows Azure Authentication has been enabled for your application, click **Close:**</span></span> 

    ![](windows-azure-authentication/_static/image7.png)
3. <span data-ttu-id="92e49-135">按 f5 鍵執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92e49-135">Hit F5 to run your application.</span></span> <span data-ttu-id="92e49-136">您應該會自動取得重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="92e49-136">You should automatically get redirected to login page.</span></span> <span data-ttu-id="92e49-137">您可以使用目錄租用戶的使用者認證來登入應用程式...</span><span class="sxs-lookup"><span data-stu-id="92e49-137">Use the directory tenet user credentials to login to the application..</span></span>  

    ![](windows-azure-authentication/_static/image1.jpg)
4. <span data-ttu-id="92e49-138">因為您的應用程式目前使用的自我簽署的測試憑證，所以您會收到警告之瀏覽器的不受信任的憑證授權單位所發出的憑證。</span><span class="sxs-lookup"><span data-stu-id="92e49-138">Because your application is currently using a self-signed test certificate you will receive a warning from the browser that the certificate was not issued by a trusted certificate authority.</span></span>

    <span data-ttu-id="92e49-139">這項警告可以放心忽略在本機開發期間按一下**繼續瀏覽此網站：**</span><span class="sxs-lookup"><span data-stu-id="92e49-139">This warning can be safely ignored during local development by clicking **Continue to this website:**</span></span> 

    ![](windows-azure-authentication/_static/image8.png)
5. <span data-ttu-id="92e49-140">現在已成功登入使用 Windows Azure 驗證您的應用程式 ！</span><span class="sxs-lookup"><span data-stu-id="92e49-140">You have now successfully logged in to your application using Windows Azure Authentication!</span></span>

    ![](windows-azure-authentication/_static/image2.jpg)

<span data-ttu-id="92e49-141">啟用 Windows Azure 驗證可以進行下列變更您的應用程式：</span><span class="sxs-lookup"><span data-stu-id="92e49-141">Enabling Windows Azure authentication makes the following changes to your application:</span></span>

- <span data-ttu-id="92e49-142">反跨網站偽造要求 ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) 類別 (*應用程式\_Start\AntiXsrfConfig.cs* ) 加入至專案。</span><span class="sxs-lookup"><span data-stu-id="92e49-142">An Anti-Cross-Site Request Forgery ([CSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))) class ( *App\_Start\AntiXsrfConfig.cs* ) is added to your project.</span></span>
- <span data-ttu-id="92e49-143">NuGet 套件`System.IdentityModel.Tokens.ValidatingIssuerNameRegistry`加入至專案。</span><span class="sxs-lookup"><span data-stu-id="92e49-143">The NuGet packages `System.IdentityModel.Tokens.ValidatingIssuerNameRegistry` is added to your project.</span></span>
- <span data-ttu-id="92e49-144">在您的應用程式中的 Windows Identity Foundation 設定會設定為接受來自 Windows Azure Active Directory 租用戶的安全性權杖。</span><span class="sxs-lookup"><span data-stu-id="92e49-144">Windows Identity Foundation settings in your application will be configured to accept security tokens from your Windows Azure Active Directory tenant.</span></span> <span data-ttu-id="92e49-145">按一下來查看所做的變更的展開的檢視影像*Web.config*檔案。</span><span class="sxs-lookup"><span data-stu-id="92e49-145">Click on the image below to see an expanded view of the changes made to the *Web.config* file.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.png)
- <span data-ttu-id="92e49-146">將佈建服務主體應用程式在 Windows Azure Active Directory 租用戶中。</span><span class="sxs-lookup"><span data-stu-id="92e49-146">A service principal for your application in your Windows Azure Active Directory tenant will be provisioned.</span></span>
- <span data-ttu-id="92e49-147">會啟用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="92e49-147">HTTPS is enabled.</span></span>

## <a name="deploy-the-application-to-windows-azure"></a><span data-ttu-id="92e49-148">部署至 Windows Azure 應用程式</span><span class="sxs-lookup"><span data-stu-id="92e49-148">Deploy the application to Windows Azure</span></span>

<span data-ttu-id="92e49-149">如需完整指示，請參閱 < [ASP.NET Web 應用程式至 Windows Azure 網站部署](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet)。</span><span class="sxs-lookup"><span data-stu-id="92e49-149">For complete instructions, see [Deploying an ASP.NET Web Application to a Windows Azure Web Site](https://docs.microsoft.com/azure/app-service-web/app-service-web-get-started-dotnet).</span></span>

<span data-ttu-id="92e49-150">若要發佈的應用程式使用 Windows Azure 驗證至 Azure 網站：</span><span class="sxs-lookup"><span data-stu-id="92e49-150">To publish an application using Windows Azure Authentication to an Azure Web Site:</span></span>

1. <span data-ttu-id="92e49-151">以滑鼠右鍵按一下您的應用程式，然後選取**發行：**</span><span class="sxs-lookup"><span data-stu-id="92e49-151">Right click on your application and select **Publish:**</span></span> 

    ![](windows-azure-authentication/_static/image3.jpg)
2. <span data-ttu-id="92e49-152">從 [發佈 Web] 對話方塊下載並匯入發行設定檔的 Azure 網站。</span><span class="sxs-lookup"><span data-stu-id="92e49-152">From the Publish Web dialog download and import a publishing profile for your Azure Web Site.</span></span>

    ![](windows-azure-authentication/_static/image4.jpg)
3. <span data-ttu-id="92e49-153">**連接**索引標籤會顯示**目的地 URL** (公用面向應用程式 URL)。</span><span class="sxs-lookup"><span data-stu-id="92e49-153">The **Connection** tab shows the **Destination URL** (the public facing URL for your application).</span></span> <span data-ttu-id="92e49-154">按一下 **驗證連線**來測試您的連線：</span><span class="sxs-lookup"><span data-stu-id="92e49-154">Click **Validate Connection** to test your connection:</span></span>

    ![](windows-azure-authentication/_static/image5.jpg)
4. <span data-ttu-id="92e49-155">如果您已經發行到此 Azure 網站之前，請考慮檢查**移除目的地上的其他檔案**設定，以確保您的應用程式完全發行。</span><span class="sxs-lookup"><span data-stu-id="92e49-155">If you have published to this Azure Web Site previously consider checking the **Remove additional files at destination** setting to ensure your application publishes cleanly.</span></span> <span data-ttu-id="92e49-156">請注意**啟用 Windows Azure 驗證**就 slected 核取方塊。</span><span class="sxs-lookup"><span data-stu-id="92e49-156">Notice the **Enable Windows Azure Authentication** check box is slected.</span></span>  

    ![](windows-azure-authentication/_static/image10.png)
5. <span data-ttu-id="92e49-157">選擇性： 在**Preview**索引標籤處按一下**開始預覽**查看部署的檔案。</span><span class="sxs-lookup"><span data-stu-id="92e49-157">Optional: On the **Preview** tab click **Start Preview** to see the files deployed.</span></span>

    ![](windows-azure-authentication/_static/image6.jpg)
6. <span data-ttu-id="92e49-158">按一下 **發行。**</span><span class="sxs-lookup"><span data-stu-id="92e49-158">Click **Publish.**</span></span>

    <span data-ttu-id="92e49-159">系統會提示您啟用 Windows Azure 驗證目標主機。</span><span class="sxs-lookup"><span data-stu-id="92e49-159">You will be prompted to enable Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="92e49-160">按一下 **啟用**繼續：</span><span class="sxs-lookup"><span data-stu-id="92e49-160">Click **Enable** to continue:</span></span>

    ![](windows-azure-authentication/_static/image11.png)
7. <span data-ttu-id="92e49-161">輸入您的 Windows Azure Active Directory 租用戶系統管理員認證：</span><span class="sxs-lookup"><span data-stu-id="92e49-161">Enter your administrator credentials for your Windows Azure Active Directory tenant:</span></span>

    ![](windows-azure-authentication/_static/image7.jpg)
8. <span data-ttu-id="92e49-162">一旦成功發行您的應用程式，瀏覽器會開啟至已發行的網站。</span><span class="sxs-lookup"><span data-stu-id="92e49-162">Once your application has been successfully published, a browser will open to the published web site.</span></span>

    > [!NOTE]
    > <span data-ttu-id="92e49-163">可能需要最多五分鐘的時間 （通常必須更少） 完整佈建 Windows Azure Active Directory 與目標主機啟用 Windows Azure 驗證後的應用程式。</span><span class="sxs-lookup"><span data-stu-id="92e49-163">It may take up to five minutes (typically must less) for your application to be fully provisioned with Windows Azure Active Directory after enabling Windows Azure Authentication for the target host.</span></span> <span data-ttu-id="92e49-164">當您第一次執行應用程式如果您收到錯誤 ACS50001： 找不到名稱 '[領域]' 的信賴憑證者合作對象，然後等候幾分鐘的時間，並嘗試再次執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="92e49-164">When you first run your application if you receive error ACS50001: Relying party with name ‘[realm]' was not found, then wait a few minutes and try running the application again.</span></span>
9. <span data-ttu-id="92e49-165">出現提示時，您的目錄中的使用者身分登入：</span><span class="sxs-lookup"><span data-stu-id="92e49-165">When prompted, log in as a user in your directory:</span></span>

    ![](windows-azure-authentication/_static/image8.jpg)
10. <span data-ttu-id="92e49-166">您現在已成功登入您的 Azure 裝載使用 Windows Azure 驗證應用程式。</span><span class="sxs-lookup"><span data-stu-id="92e49-166">You have now successfully logged into your Azure hosted application using Windows Azure Authentication.</span></span>  
  
     ![](windows-azure-authentication/_static/image9.jpg)

## <a name="known-issues"></a><span data-ttu-id="92e49-167">已知問題</span><span class="sxs-lookup"><span data-stu-id="92e49-167">Known Issues</span></span>

#### <a name="role-based-authorization-fails-when-using-windows-azure-authenticationopop"></a><span data-ttu-id="92e49-168">以角色為基礎的授權失敗時使用 Windows Azure 驗證 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-168">Role-based authorization fails when using Windows Azure Authentication<o:p></o:p></span></span>

<span data-ttu-id="92e49-169">Windows Azure Authentication 目前未提供必要的角色宣告，因此可以執行以角色為基礎的授權。</span><span class="sxs-lookup"><span data-stu-id="92e49-169">Windows Azure Authentication does not currently provide the necessary role claim so that role-based authorization can be performed.</span></span> <span data-ttu-id="92e49-170">已驗證的使用者角色必須以手動方式從 Windows Azure Active Directory。 < o: p>< >< 擷取 / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-170">The role of the authenticated user must be manually retrieved from Windows Azure Active Directory.<o:p></o:p></span></span>

#### <a name="browsing-to-an-application-with-windows-azure-authentication-results-in-the-error-acs20016-the-domain-of-the-logged-in-user-livecom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="92e49-171">瀏覽至應用程式與 Windows Azure 驗證錯誤會產生 「 ACS20016 登入的使用者 (live.com) 的網域不符合任何允許網域這個 STS 」 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-171">Browsing to an application with Windows Azure Authentication results in the error "ACS20016 The domain of the logged in user (live.com) does not match any allowed domain of this STS"<o:p></o:p></span></span>

<span data-ttu-id="92e49-172">如果您已經登入 Microsoft 帳戶 （例如 hotmail.com、 live.com、 outlook.com），而且您嘗試存取的應用程式已啟用 Windows Azure 驗證，您就可能取得 400 錯誤回應，因為您的 Microsoft 帳戶的網域無法辨識 Windows Azure Active Directory。</span><span class="sxs-lookup"><span data-stu-id="92e49-172">If you are already logged in to a Microsoft Account (for example hotmail.com, live.com, outlook.com) and you attempt to access an application that has enabled Windows Azure Authentication you may get a 400 error response because the domain of your Microsoft Account is not recognized by Windows Azure Active Directory.</span></span> <span data-ttu-id="92e49-173">若要登入的應用程式，登出您的 Microsoft 帳戶第一次。 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-173">To log into the application, log out from your Microsoft Account first.<o:p></o:p></span></span>

#### <a name="logging-into-an-application-with-windows-azure-authentication-enabled-and-a-x509certificatevalidationmode-other-than-none-results-in-certificate-validation-errors-for-the-accountsaccesscontrolwindowsnet-certificateopop"></a><span data-ttu-id="92e49-174">以外的其他登入啟用的 Windows Azure 驗證應用程式和 X509CertificateValidationMode 無導致憑證驗證錯誤 accounts.accesscontrol.windows.net 憑證 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-174">Logging into an application with Windows Azure Authentication enabled and a X509CertificateValidationMode other than None results in certificate validation errors for the accounts.accesscontrol.windows.net certificate<o:p></o:p></span></span>

<span data-ttu-id="92e49-175">則不需要憑證驗證，且應該維持在停用。</span><span class="sxs-lookup"><span data-stu-id="92e49-175">Certificate validation is not required and should be left disabled.</span></span> <span data-ttu-id="92e49-176">簽發者憑證的指紋會進行驗證。 WSFederationAuthenticationModule < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-176">The thumbprint of the issuer certificate is validated by the WSFederationAuthenticationModule.<o:p></o:p></span></span>

#### <a name="when-attempting-to-enable-windows-azure-authentication-the-web-authentication-dialog-shows-the-error-acs20016-the-domain-of-the-logged-in-user-contosoonmicrosoftcom-does-not-match-any-allowed-domain-of-this-stsopop"></a><span data-ttu-id="92e49-177">當您嘗試啟用 Windows Azure 驗證時 [Web 驗證] 對話方塊會顯示錯誤 「 ACS20016： 登入的使用者 (contoso.onmicrosoft.com) 的網域不符合此 STS 的任何允許的網域。 」< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-177">When attempting to enable Windows Azure Authentication the Web Authentication dialog shows the error "ACS20016: The domain of the logged in user (contoso.onmicrosoft.com) does not match any allowed domain of this STS."<o:p></o:p></span></span>

<span data-ttu-id="92e49-178">當您先前已成功登入使用其他 Windows Azure Active Directory 帳戶，從相同的 Visual Studio 程序中，您會看到此錯誤。</span><span class="sxs-lookup"><span data-stu-id="92e49-178">You may see this error when you have previously successfully logged in using a different Windows Azure Active Directory account from within the same Visual Studio process.</span></span> <span data-ttu-id="92e49-179">從指定的帳戶登出或重新啟動 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="92e49-179">Log out from the specified account or restart Visual Studio.</span></span> <span data-ttu-id="92e49-180">如果您先前已登入，並選取 「 讓我保持登中 」 的選項則您可能需要清除瀏覽器 cookie。 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-180">If you previously logged in and selected the option to "Keep me signed in" then you may need to clear your browser cookies.<o:p></o:p></span></span>

## <a name="acs20012-the-request-is-not-a-valid-ws-federation-protocol-message-opop"></a><span data-ttu-id="92e49-181">ACS20012： 要求不是有效的 WS-同盟通訊協定訊息 < o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-181">ACS20012: The request is not a valid WS-Federation protocol message <o:p></o:p></span></span>

<span data-ttu-id="92e49-182">如果您已經登入一些其他 Microsoft ID 以其中一項 Azure 服務，會發生這項目。</span><span class="sxs-lookup"><span data-stu-id="92e49-182">This can happen if you are already logged in with some other Microsoft ID to one of the Azure services.</span></span> <span data-ttu-id="92e49-183">使用私用瀏覽器視窗中，例如在 IE 中的 InPrivate 或 chrome Incognito 或清除所有 cookie。</span><span class="sxs-lookup"><span data-stu-id="92e49-183">Use Private browser window like InPrivate in IE or Incognito in Chrome or clear all the cookies.</span></span> <span data-ttu-id="92e49-184">< o: p>< >< / o: p>< ></span><span class="sxs-lookup"><span data-stu-id="92e49-184"><o:p></o:p></span></span>

## <a name="additional-resources"></a><span data-ttu-id="92e49-185">其他資源</span><span class="sxs-lookup"><span data-stu-id="92e49-185">Additional Resources</span></span>

- <span data-ttu-id="92e49-186">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) -Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="92e49-186">[Microsoft ASP.NET Tools for Windows Azure Active Directory – Visual Studio 2012](https://blogs.msdn.com/b/vbertocci/archive/2013/02/18/microsoft-asp-net-tools-for-windows-azure-active-directory-visual-studio-2012.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="92e49-187">Windows Azure 功能： 身分識別</span><span class="sxs-lookup"><span data-stu-id="92e49-187">Windows Azure Features: Identity</span></span>](https://docs.microsoft.com/azure/active-directory/)
- [<span data-ttu-id="92e49-188">TechNet: Windows Azure Active Directory</span><span class="sxs-lookup"><span data-stu-id="92e49-188">TechNet: Windows Azure Active Directory</span></span>](https://technet.microsoft.com/library/hh967619.aspx)
- [<span data-ttu-id="92e49-189">Windows Azure Active Directory： 為您的組織開發的應用程式</span><span class="sxs-lookup"><span data-stu-id="92e49-189">Windows Azure Active Directory: Develop Apps for your organization</span></span>](https://activedirectory.windowsazure.com/Develop/Single-Tenant.aspx)
- [<span data-ttu-id="92e49-190">Windows Azure Active Directory： 開發應用程式的多個組織</span><span class="sxs-lookup"><span data-stu-id="92e49-190">Windows Azure Active Directory: Develop Apps for multiple organizations</span></span>](https://activedirectory.windowsazure.com/Develop/Multi-Tenant.aspx)
- [<span data-ttu-id="92e49-191">如何實作單一登入 Windows Azure Active directory</span><span class="sxs-lookup"><span data-stu-id="92e49-191">How to implement single sign-on with Windows Azure Active Directory</span></span>](https://github.com/Azure-Samples/active-directory-dotnet-webapp-openidconnect)
- <span data-ttu-id="92e49-192">[單一登入 windows Azure Active Directory： 深入了解](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx)-Vittorio Bertocci</span><span class="sxs-lookup"><span data-stu-id="92e49-192">[Single Sign-On with Windows Azure Active Directory: a Deep Dive](https://blogs.msdn.com/b/vbertocci/archive/2012/07/05/single-sign-on-with-windows-azure-active-directory-a-deep-dive.aspx) – Vittorio Bertocci</span></span>
- [<span data-ttu-id="92e49-193">使用 AD FS 2.0 實作和管理單一登入</span><span class="sxs-lookup"><span data-stu-id="92e49-193">Use AD FS 2.0 to implement and manage single sign-on</span></span>](https://technet.microsoft.com/library/jj205462.aspx)
