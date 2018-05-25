---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 建立 MVC 5 應用程式與 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#) |Microsoft 文件
author: Rick-Anderson
description: 本教學課程會示範如何建置讓使用者能夠登入來自外部的驗證認證搭配使用 OAuth 2.0 的 ASP.NET MVC 5 web 應用程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 04/03/2015
ms.topic: article
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: c289c209b50f0c2c1f2d8b15a3aedeaebf671d0b
ms.sourcegitcommit: 24c32648ab0c6f0be15333d7c23c1bf680858c43
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/21/2018
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="481f2-103">建立 ASP.NET MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#)</span><span class="sxs-lookup"><span data-stu-id="481f2-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="481f2-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="481f2-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="481f2-105">本教學課程會示範如何建立 ASP.NET MVC 5 web 應用程式，讓使用者能夠登入使用[OAuth 2.0](http://oauth.net/2/)使用從外部驗證提供者，例如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google 認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="481f2-106">為了簡單起見，本教學課程著重於使用 Facebook 和 Google 認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="481f2-107">啟用瀏覽的網站中的這些認證提供更大的優點，因為已經有數百萬名使用者的帳戶與這些外部提供者。</span><span class="sxs-lookup"><span data-stu-id="481f2-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="481f2-108">這些使用者可能比較想要註冊您的站台如果它們不需要建立並記住一組新的認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="481f2-109">另請參閱[使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。</span><span class="sxs-lookup"><span data-stu-id="481f2-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="481f2-110">本教學課程也會示範如何加入分析資料的使用者，以及如何使用可將角色的成員資格應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="481f2-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="481f2-111">本教學課程中所編寫的[Rick Anderson](https://blogs.msdn.com/rickAndy) (請在 Twitter 上關注我： [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="481f2-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="481f2-112">快速入門</span><span class="sxs-lookup"><span data-stu-id="481f2-112">Getting Started</span></span>

<span data-ttu-id="481f2-113">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="481f2-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="481f2-114">安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="481f2-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="481f2-115">Dropbox、 GitHub、 Linkedin、 Instagram、 緩衝區、 Salesforce、 資料流、 堆疊 Exchange、 Tripit、 Twitch、 Twitter、 yahoo ！ 和更多的說明，請參閱此[範例專案](https://github.com/matthewdunsdon/oauthforaspnet)。</span><span class="sxs-lookup"><span data-stu-id="481f2-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="481f2-116">您必須安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本，才能使用 Google OAuth 2 和偵錯在本機，而 SSL 警告。</span><span class="sxs-lookup"><span data-stu-id="481f2-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="481f2-117">按一下**新專案**從**啟動** 頁面上，或者您可以使用功能表並選取**檔案**，然後**新專案**。</span><span class="sxs-lookup"><span data-stu-id="481f2-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="481f2-118">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="481f2-118">Creating Your First Application</span></span>

<span data-ttu-id="481f2-119">按一下**新專案**，然後選取**Visual C#** 在左邊，然後**Web** ，然後選取  **ASP.NET Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="481f2-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="481f2-120">將您的專案"MvcAuth"，再按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="481f2-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="481f2-121">在**新增 ASP.NET 專案**] 對話方塊中，按一下 [ **MVC**。</span><span class="sxs-lookup"><span data-stu-id="481f2-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="481f2-122">如果無法驗證**個別使用者帳戶**，按一下 **變更驗證**按鈕，然後選取**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="481f2-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="481f2-123">藉由檢查**雲端中的主機**，應用程式將會很容易就能在 Azure 中裝載。</span><span class="sxs-lookup"><span data-stu-id="481f2-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="481f2-124">如果您選取**雲端中的主機**，完成 [設定] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="481f2-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="481f2-125">使用 NuGet 來更新為最新的 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="481f2-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="481f2-126">使用 NuGet 套件管理員來更新[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="481f2-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="481f2-127">選取**更新**左側功能表中。</span><span class="sxs-lookup"><span data-stu-id="481f2-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="481f2-128">您可以按一下**全部更新** 按鈕，或者您可以搜尋只有 OWIN 封裝 （在下圖所示）：</span><span class="sxs-lookup"><span data-stu-id="481f2-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="481f2-129">在下列影像顯示只有 OWIN 封裝：</span><span class="sxs-lookup"><span data-stu-id="481f2-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="481f2-130">從封裝管理員主控台 (PMC)，您可以輸入`Update-Package`命令，將會更新所有封裝。</span><span class="sxs-lookup"><span data-stu-id="481f2-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="481f2-131">按**F5**或**Ctrl + F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="481f2-132">在下面的影像中的連接埠號碼是 1234年。</span><span class="sxs-lookup"><span data-stu-id="481f2-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="481f2-133">當您執行應用程式時，您會看到不同的通訊埠編號。</span><span class="sxs-lookup"><span data-stu-id="481f2-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="481f2-134">根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示以查看**首頁**，**有關**，**連絡人**，**註冊**和**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="481f2-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="481f2-135">在專案中的 SSL 設定</span><span class="sxs-lookup"><span data-stu-id="481f2-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="481f2-136">若要連接到 Google 和 Facebook 驗證提供者，您必須將 IIS Express 設定為使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="481f2-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="481f2-137">請務必保留登入之後，使用 SSL 以及不卸除回為 HTTP，登入 cookie 只做為密碼做為您的使用者名稱和密碼，而不使用的 SSL，您要先將它以純文字傳送，在網路上。</span><span class="sxs-lookup"><span data-stu-id="481f2-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="481f2-138">此外，您已記執行交握和安全通道 （這是大量構成 HTTPS 低於 HTTP） 的時間執行 MVC 管線之前，因此將重新導向回到 HTTP 您登入之後將不會使目前的要求或未來要求快得多。</span><span class="sxs-lookup"><span data-stu-id="481f2-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="481f2-139">在**方案總管] 中**，按一下 [ **MvcAuth**專案。</span><span class="sxs-lookup"><span data-stu-id="481f2-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="481f2-140">按 F4 鍵以顯示專案屬性。</span><span class="sxs-lookup"><span data-stu-id="481f2-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="481f2-141">或者，從**檢視**功能表您可以選取**屬性 視窗**。</span><span class="sxs-lookup"><span data-stu-id="481f2-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="481f2-142">變更**啟用 SSL**為 True。</span><span class="sxs-lookup"><span data-stu-id="481f2-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="481f2-143">複製 SSL URL (這將成為`https://localhost:44300/`除非您已建立其他 SSL 專案)。</span><span class="sxs-lookup"><span data-stu-id="481f2-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="481f2-144">在**方案總管 中**，以滑鼠右鍵按一下**MvcAuth**專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="481f2-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="481f2-145">選取**Web**索引標籤，然後貼到 SSL URL**專案 Url**方塊。</span><span class="sxs-lookup"><span data-stu-id="481f2-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="481f2-146">儲存檔案 (Ctl + S)。</span><span class="sxs-lookup"><span data-stu-id="481f2-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="481f2-147">您將需要此 URL，若要設定 Facebook 和 Google 驗證的應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="481f2-148">新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)屬性`Home`需要的所有要求的控制站都必須使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="481f2-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="481f2-149">最安全的作法是新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)應用程式的篩選。</span><span class="sxs-lookup"><span data-stu-id="481f2-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="481f2-150">請參閱節&quot;保護應用程式的 SSL 和授權屬性&quot;在我 tutoral[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="481f2-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="481f2-151">如下所示主控制器的一部分。</span><span class="sxs-lookup"><span data-stu-id="481f2-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="481f2-152">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="481f2-153">如果您已安裝憑證，在過去，您可以略過此章節的其餘部分，並跳至[OAuth 2 建立 Google 應用程式和應用程式連接至專案](#goog)，否則請遵循指示信任的自我簽署IIS Express 產生的憑證。</span><span class="sxs-lookup"><span data-stu-id="481f2-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="481f2-154">讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，表示 localhost。</span><span class="sxs-lookup"><span data-stu-id="481f2-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="481f2-155">即會顯示*首頁*頁面上並不沒有出現任何 SSL 的警告。</span><span class="sxs-lookup"><span data-stu-id="481f2-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="481f2-156">Google Chrome 也接受該憑證，並會顯示 HTTPS 內容，而不發出警告。</span><span class="sxs-lookup"><span data-stu-id="481f2-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="481f2-157">Firefox 會使用自己的憑證存放區，因此它會顯示警告。</span><span class="sxs-lookup"><span data-stu-id="481f2-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="481f2-158">對於我們的應用程式可以安全地按一下**我了解風險**。</span><span class="sxs-lookup"><span data-stu-id="481f2-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="481f2-159">OAuth 2 建立 Google 應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="481f2-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="481f2-160">目前的 Google OAuth 指示，請參閱[設定 Google 驗證中 ASP.NET Core](/aspnet/core/security/authentication/social/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="481f2-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="481f2-161">瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。</span><span class="sxs-lookup"><span data-stu-id="481f2-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="481f2-162">如果您尚未建立的專案之前，請選取**認證**在左側的索引標籤上，然後選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="481f2-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="481f2-163">在左側的索引標籤上，按一下**認證**。</span><span class="sxs-lookup"><span data-stu-id="481f2-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="481f2-164">按一下**建立認證**然後**OAuth 用戶端識別碼**。</span><span class="sxs-lookup"><span data-stu-id="481f2-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="481f2-165">在**建立用戶端識別碼** 對話方塊中，保留預設值**Web 應用程式**應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="481f2-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="481f2-166">設定**授權 JavaScript**來源可您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立其他 SSL 專案)</span><span class="sxs-lookup"><span data-stu-id="481f2-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="481f2-167">設定**授權重新導向 URI**至：</span><span class="sxs-lookup"><span data-stu-id="481f2-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="481f2-168">按一下 OAuth 同意畫面功能表項目，然後設定您的電子郵件地址和產品名稱。</span><span class="sxs-lookup"><span data-stu-id="481f2-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="481f2-169">當您完成表單按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="481f2-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="481f2-170">按一下文件庫功能表項目，請搜尋**Google + API**，請按一下它，然後按下啟用。</span><span class="sxs-lookup"><span data-stu-id="481f2-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="481f2-171">下圖顯示已啟用的應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="481f2-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="481f2-172">從 Google Api API 管理員 中，請瀏覽**認證**索引標籤，以取得**用戶端識別碼**。</span><span class="sxs-lookup"><span data-stu-id="481f2-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="481f2-173">下載應用程式密碼儲存在 JSON 檔案。</span><span class="sxs-lookup"><span data-stu-id="481f2-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="481f2-174">複製並貼上**ClientId**和**ClientSecret**到`UseGoogleAuthentication`方法中找到*Startup.Auth.cs*檔案*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="481f2-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="481f2-175">**ClientId**和**ClientSecret**值如下所示的範例和沒有作用。</span><span class="sxs-lookup"><span data-stu-id="481f2-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="481f2-176">安全性-永遠不會儲存敏感的資料來源上的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="481f2-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="481f2-177">為了簡化範例上述程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="481f2-178">請參閱[ASP.NET 和 Azure App Service 部署的密碼和其他機密資料的最佳做法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="481f2-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="481f2-179">按**CTRL + F5**建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="481f2-180">按一下**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="481f2-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="481f2-181">在下**使用另一個服務登入**，按一下  **Google**。</span><span class="sxs-lookup"><span data-stu-id="481f2-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="481f2-182">若您遺漏任何上述的步驟會收到 HTTP 401 錯誤。</span><span class="sxs-lookup"><span data-stu-id="481f2-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="481f2-183">重新檢查您上述的步驟。</span><span class="sxs-lookup"><span data-stu-id="481f2-183">Recheck your steps above.</span></span> <span data-ttu-id="481f2-184">若您遺漏必要的設定 (例如**產品名稱**)、 新增遺失的項目，並儲存，則可能需要幾分鐘，讓驗證才能運作。</span><span class="sxs-lookup"><span data-stu-id="481f2-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="481f2-185">安裝程式會重新導向至 Google 站台，您將在其中輸入您的認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="481f2-186">輸入您的認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限：</span><span class="sxs-lookup"><span data-stu-id="481f2-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="481f2-187">按一下**接受**。</span><span class="sxs-lookup"><span data-stu-id="481f2-187">Click **Accept**.</span></span> <span data-ttu-id="481f2-188">您將會被重新導向至**註冊**MvcAuth 應用程式，您可以在其中註冊您的 Google 帳戶頁面。</span><span class="sxs-lookup"><span data-stu-id="481f2-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="481f2-189">您可以選擇變更為您的 Gmail 帳戶所使用的本機電子郵件註冊名稱，但您通常想要保留的預設電子郵件別名 （也就是一個用來驗證）。</span><span class="sxs-lookup"><span data-stu-id="481f2-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="481f2-190">按一下 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="481f2-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="481f2-191">在 Facebook 中建立應用程式和應用程式連接至專案</span><span class="sxs-lookup"><span data-stu-id="481f2-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="481f2-192">目前的 Facebook OAuth2 驗證指示，請參閱[設定 Facebook 驗證](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="481f2-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="481f2-193">Facebook OAuth2 驗證，您要複製到專案的某些設定從您在 Facebook 中建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="481f2-194">在瀏覽器中，瀏覽至[ https://developers.facebook.com/apps ](https://developers.facebook.com/apps)並輸入您的 Facebook 認證登入。</span><span class="sxs-lookup"><span data-stu-id="481f2-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="481f2-195">如果您不已註冊為 Facebook 開發人員，請按一下**身為開發人員註冊**並依照指示進行註冊。</span><span class="sxs-lookup"><span data-stu-id="481f2-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="481f2-196">在**應用程式**索引標籤上，按一下 **建立新的應用程式**。</span><span class="sxs-lookup"><span data-stu-id="481f2-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![建立新的應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="481f2-198">輸入**應用程式名稱**和**類別**，然後按一下 **建立應用程式**。</span><span class="sxs-lookup"><span data-stu-id="481f2-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="481f2-199">這必須是唯一的 Facebook。</span><span class="sxs-lookup"><span data-stu-id="481f2-199">This must be unique across Facebook.</span></span> <span data-ttu-id="481f2-200"><strong>應用程式命名空間</strong>屬於您的應用程式用來存取驗證的 Facebook 應用程式的 URL (例如，https://apps.facebook.com/{App命名空間})。</span><span class="sxs-lookup"><span data-stu-id="481f2-200">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="481f2-201">如果您未指定<strong>應用程式命名空間</strong>、<strong>應用程式識別碼</strong>將使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="481f2-201">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="481f2-202"><strong>應用程式識別碼</strong>是下一個步驟中，您會看到一個長時間的系統產生數字。</span><span class="sxs-lookup"><span data-stu-id="481f2-202">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![建立新的應用程式 對話方塊](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="481f2-204">提交的一般安全性檢查。</span><span class="sxs-lookup"><span data-stu-id="481f2-204">Submit the standard security check.</span></span>

    ![安全性檢查](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="481f2-206">選取**設定**左側的功能表列![Facebook 開發人員的功能表列](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="481f2-206">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="481f2-207">在**基本**頁面的 [設定] 區段選取**加入平台**來指定您要加入網站的應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-207">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="481f2-208">![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="481f2-208">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="481f2-209">選取**網站**從平台的選項。</span><span class="sxs-lookup"><span data-stu-id="481f2-209">Select **Website** from the platform choices.</span></span>  
  
    ![平台選項](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="481f2-211">請記下您**應用程式識別碼**和您**應用程式秘鑰**，讓您可以稍後在本教學課程新增到您的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-211">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="481f2-212">此外，請加入您的站台 URL (`https://localhost:44300/`) 來測試 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-212">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="481f2-213">此外，請加入**Contact Email**。</span><span class="sxs-lookup"><span data-stu-id="481f2-213">Also, add a **Contact Email**.</span></span> <span data-ttu-id="481f2-214">然後，選取**儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="481f2-214">Then, select **Save Changes**.</span></span>   

    ![基本應用程式詳細資料頁面](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="481f2-216">請注意，您只能使用已註冊的電子郵件別名來進行驗證。</span><span class="sxs-lookup"><span data-stu-id="481f2-216">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="481f2-217">其他使用者及測試帳戶將無法註冊。</span><span class="sxs-lookup"><span data-stu-id="481f2-217">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="481f2-218">您可以授與其他上 Facebook 應用程式的 Facebook 帳戶存取**開發人員角色** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="481f2-218">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="481f2-219">在 Visual Studio 中開啟*應用程式\_Start\Startup.Auth.cs*。</span><span class="sxs-lookup"><span data-stu-id="481f2-219">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="481f2-220">複製並貼上**AppId**和**應用程式秘鑰**到`UseFacebookAuthentication`方法。</span><span class="sxs-lookup"><span data-stu-id="481f2-220">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="481f2-221">**AppId**和**應用程式秘鑰**值如下所示的範例，將無法運作。</span><span class="sxs-lookup"><span data-stu-id="481f2-221">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="481f2-222">按一下**儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="481f2-222">Click **Save Changes**.</span></span>
13. <span data-ttu-id="481f2-223">按**CTRL + F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-223">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="481f2-224">選取**登入**以顯示登入頁面。</span><span class="sxs-lookup"><span data-stu-id="481f2-224">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="481f2-225">按一下**Facebook**下**使用另一個服務登入。**</span><span class="sxs-lookup"><span data-stu-id="481f2-225">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="481f2-226">輸入您的 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="481f2-226">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="481f2-227">系統會提示您授與應用程式存取您的公用設定檔和 friend 清單的權限。</span><span class="sxs-lookup"><span data-stu-id="481f2-227">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook 應用程式詳細資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="481f2-229">您現在已登入。</span><span class="sxs-lookup"><span data-stu-id="481f2-229">You are now logged in.</span></span> <span data-ttu-id="481f2-230">您現在可以註冊此帳戶與應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-230">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="481f2-231">當您註冊時，要將項目加入至*使用者*成員資格資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="481f2-231">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="481f2-232">檢查成員資格資料</span><span class="sxs-lookup"><span data-stu-id="481f2-232">Examine the Membership Data</span></span>

<span data-ttu-id="481f2-233">在**檢視**功能表上，按一下 **伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="481f2-233">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="481f2-234">展開**DefaultConnection (MvcAuth)**，依序展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**按一下**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="481f2-234">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers 資料表資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="481f2-236">將設定檔資料加入至使用者類別</span><span class="sxs-lookup"><span data-stu-id="481f2-236">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="481f2-237">本節中您會將出生日期以及主城鎮使用者資料在註冊期間，在下圖所示。</span><span class="sxs-lookup"><span data-stu-id="481f2-237">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![使用家用城鎮和 Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="481f2-239">開啟*Models\IdentityModels.cs*檔案，然後加入出生日期和從家裡城鎮屬性：</span><span class="sxs-lookup"><span data-stu-id="481f2-239">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="481f2-240">開啟*Models\AccountViewModels.cs*檔案與設定的出生日期和從家裡城鎮屬性中的`ExternalLoginConfirmationViewModel`。</span><span class="sxs-lookup"><span data-stu-id="481f2-240">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="481f2-241">開啟*Controllers\AccountController.cs*檔案，然後加入程式碼中的出生日期和從家裡城鎮`ExternalLoginConfirmation`動作方法所示：</span><span class="sxs-lookup"><span data-stu-id="481f2-241">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="481f2-242">新增生日及家用城鎮*Views\Account\ExternalLoginConfirmation.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="481f2-242">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="481f2-243">刪除成員資格資料庫，因此您可以再次向您的應用程式註冊您的 Facebook 帳戶，並確認您可以新增新的生日及家用城鎮設定檔資訊。</span><span class="sxs-lookup"><span data-stu-id="481f2-243">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="481f2-244">從**方案總管] 中**，按一下 [**顯示所有檔案**圖示，然後以滑鼠右鍵按一下*新增\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf*按一下**刪除**。</span><span class="sxs-lookup"><span data-stu-id="481f2-244">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="481f2-245">從**工具**功能表上，按一下  **NuGet 封裝管理員**，然後按一下  **Package Manager Console** (PMC)。</span><span class="sxs-lookup"><span data-stu-id="481f2-245">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="481f2-246">PMC 中輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="481f2-246">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="481f2-247">Enable-migrations</span><span class="sxs-lookup"><span data-stu-id="481f2-247">Enable-Migrations</span></span>
2. <span data-ttu-id="481f2-248">新增移轉初始化</span><span class="sxs-lookup"><span data-stu-id="481f2-248">Add-Migration Init</span></span>
3. <span data-ttu-id="481f2-249">更新資料庫</span><span class="sxs-lookup"><span data-stu-id="481f2-249">Update-Database</span></span>

<span data-ttu-id="481f2-250">執行應用程式，並使用 FaceBook 和 Google 登入並註冊某些使用者。</span><span class="sxs-lookup"><span data-stu-id="481f2-250">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="481f2-251">檢查成員資格資料</span><span class="sxs-lookup"><span data-stu-id="481f2-251">Examine the Membership Data</span></span>

<span data-ttu-id="481f2-252">在**檢視**功能表上，按一下 **伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="481f2-252">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="481f2-253">以滑鼠右鍵按一下**AspNetUsers**按一下**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="481f2-253">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="481f2-254">`HomeTown`和`BirthDate`欄位如下所示。</span><span class="sxs-lookup"><span data-stu-id="481f2-254">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="481f2-255">登出您的應用程式，並與另一個帳戶中的記錄</span><span class="sxs-lookup"><span data-stu-id="481f2-255">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="481f2-256">如果您登入您的應用程式與 Facebook、，然後登出並嘗試登入一次使用不同的 Facebook 帳戶 （使用相同的瀏覽器），您會立即登入使用先前用過的 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="481f2-256">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="481f2-257">若要使用其他帳戶，您要瀏覽至 Facebook，並在 Facebook 登出。</span><span class="sxs-lookup"><span data-stu-id="481f2-257">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="481f2-258">相同的規則適用於任何其他第 3 個合作對象驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="481f2-258">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="481f2-259">或者，您可以使用不同的瀏覽器登入另一個帳戶。</span><span class="sxs-lookup"><span data-stu-id="481f2-259">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="481f2-260">後續步驟</span><span class="sxs-lookup"><span data-stu-id="481f2-260">Next Steps</span></span>

<span data-ttu-id="481f2-261">請參閱[簡介的 Yahoo 和 LinkedIn OAuth 安全性提供者設定 owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)由 Jerrie Pelser Yahoo 和 LinkedIn 的指示。</span><span class="sxs-lookup"><span data-stu-id="481f2-261">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="481f2-262">請參閱 Jerrie 的 ASP.NET MVC 5，以取得啟用社交登入按鈕的社交登入按鈕看起來很。</span><span class="sxs-lookup"><span data-stu-id="481f2-262">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="481f2-263">請遵循我教學課程[驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)，這會繼續本教學課程，並顯示下列：</span><span class="sxs-lookup"><span data-stu-id="481f2-263">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="481f2-264">如何將您的應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="481f2-264">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="481f2-265">如何保護您的角色的應用程式。</span><span class="sxs-lookup"><span data-stu-id="481f2-265">How to secure you app with roles.</span></span>
3. <span data-ttu-id="481f2-266">如何保護您的應用程式[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)和[授權](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)篩選器。</span><span class="sxs-lookup"><span data-stu-id="481f2-266">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="481f2-267">如何使用應用程式開發介面的成員資格加入使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="481f2-267">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="481f2-268">請在您所喜歡本教學課程的方式，我們可以改進留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="481f2-268">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="481f2-269">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="481f2-269">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="481f2-270">您甚至可以要求並票選加入 ASP.NET 的新功能。</span><span class="sxs-lookup"><span data-stu-id="481f2-270">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="481f2-271">例如，您可以在此投票的工具[建立及管理使用者和角色。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="481f2-271">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="481f2-272">ASP.NET 外部驗證服務的運作方式很好的說明，請參閱 Robert McMurray[外部驗證服務](https://asp.net/web-api/overview/security/external-authentication-services)。</span><span class="sxs-lookup"><span data-stu-id="481f2-272">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="481f2-273">Robert 的發行項也會啟用 Microsoft 和 Twitter 驗證的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="481f2-273">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="481f2-274">Tom Dykstra 的絕佳[EF/MVC 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)示範如何使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="481f2-274">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
