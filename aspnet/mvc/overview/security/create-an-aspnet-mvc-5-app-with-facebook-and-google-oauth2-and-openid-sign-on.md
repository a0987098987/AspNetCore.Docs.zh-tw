---
uid: mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
title: 建立 MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#) |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置 ASP.NET MVC 5 web 應用程式，可讓使用者能夠登入使用 OAuth 2.0 搭配來自外部的驗證認證...
ms.author: aspnetcontent
ms.date: 04/03/2015
ms.assetid: 81ee500f-fc37-40d6-8722-f1b64720fbb6
msc.legacyurl: /mvc/overview/security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on
msc.type: authoredcontent
ms.openlocfilehash: 6af4990f726bfcd0c45eb6991418661f9b8ccbf6
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37824702"
---
<a name="create-an-aspnet-mvc-5-app-with-facebook-twitter-linkedin-and-google-oauth2-sign-on-c"></a><span data-ttu-id="3b5f7-103">建立 ASP.NET MVC 5 應用程式使用 Facebook、 Twitter、 LinkedIn 和 Google OAuth2 登入 (C#)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-103">Create an ASP.NET MVC 5 App with Facebook, Twitter, LinkedIn and Google OAuth2 Sign-on (C#)</span></span>
====================
<span data-ttu-id="3b5f7-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> <span data-ttu-id="3b5f7-105">本教學課程會示範如何使用建置 ASP.NET MVC 5 web 應用程式，可讓使用者能夠登入[OAuth 2.0](http://oauth.net/2/)使用來自外部驗證提供者，例如 Facebook、 Twitter、 LinkedIn、 Microsoft 或 Google 的認證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-105">This tutorial shows you how to build an ASP.NET MVC 5 web application that enables users to log in using [OAuth 2.0](http://oauth.net/2/) with credentials from an external authentication provider, such as Facebook, Twitter, LinkedIn, Microsoft, or Google.</span></span> <span data-ttu-id="3b5f7-106">為了簡單起見，本教學課程著重於使用來自 Facebook 和 Google 的認證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-106">For simplicity, this tutorial focuses on working with credentials from Facebook and Google.</span></span>
> 
> <span data-ttu-id="3b5f7-107">啟用您的網站中的這些認證提供極大的好處，因為數百萬位使用者已將這些外部提供者的帳戶。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-107">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="3b5f7-108">這些使用者可能會更有必要，如果它們不需要建立並記住一組新的認證，登入您的網站。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-108">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span>
> 
> <span data-ttu-id="3b5f7-109">另請參閱[使用 SMS 和電子郵件雙因素驗證的 ASP.NET MVC 5 應用程式](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-109">See also [ASP.NET MVC 5 app with SMS and email Two-Factor Authentication](aspnet-mvc-5-app-with-sms-and-email-two-factor-authentication.md).</span></span>
> 
> <span data-ttu-id="3b5f7-110">本教學課程也會示範如何新增使用者設定檔資料，以及如何使用成員資格 API 來新增角色。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-110">The tutorial also shows how to add profile data for the user, and how to use the Membership API to add roles.</span></span> <span data-ttu-id="3b5f7-111">本教學課程以寫入[Rick Anderson](https://blogs.msdn.com/rickAndy) (請在 Twitter 上關注我： [ @RickAndMSFT ](https://twitter.com/RickAndMSFT) )。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-111">This tutorial was written by [Rick Anderson](https://blogs.msdn.com/rickAndy) ( Please follow me on Twitter: [@RickAndMSFT](https://twitter.com/RickAndMSFT) ).</span></span>


<a id="start"></a>
## <a name="getting-started"></a><span data-ttu-id="3b5f7-112">快速入門</span><span class="sxs-lookup"><span data-stu-id="3b5f7-112">Getting Started</span></span>

<span data-ttu-id="3b5f7-113">開始安裝並執行[Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058)或是[Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-113">Start by installing and running [Visual Studio Express 2013 for Web](https://go.microsoft.com/fwlink/?LinkId=299058) or [Visual Studio 2013](https://go.microsoft.com/fwlink/?LinkId=306566).</span></span> <span data-ttu-id="3b5f7-114">安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-114">Install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher.</span></span> <span data-ttu-id="3b5f7-115">如需使用 Dropbox、 GitHub、 Linkedin、 Instagram、 緩衝區、 Salesforce、 資料流、 Stack Exchange、 Tripit、 Twitch、 Twitter、 yahoo ！ 和更多說明，請參閱此[範例專案](https://github.com/matthewdunsdon/oauthforaspnet)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-115">For help with Dropbox, GitHub, Linkedin, Instagram, Buffer, Salesforce, STEAM, Stack Exchange, Tripit, Twitch, Twitter, Yahoo!, and more, see this [sample project](https://github.com/matthewdunsdon/oauthforaspnet).</span></span>

> [!NOTE]
> <span data-ttu-id="3b5f7-116">您必須安裝 Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521)或更高版本，若要使用 Google OAuth 2 並在本機偵錯，而不需要 SSL 警告。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-116">You must install Visual Studio [2013 Update 3](https://go.microsoft.com/fwlink/?LinkId=390521) or higher to use Google OAuth 2 and to debug locally without SSL warnings.</span></span>


<span data-ttu-id="3b5f7-117">按一下 **新的專案**從**開始**頁面上，或者您可以使用功能表，然後選取**檔案**，然後**新專案**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-117">Click **New Project** from the **Start** page, or you can use the menu and select **File**, and then **New Project**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image1.png)  
 

<a id="1st"></a>
## <a name="creating-your-first-application"></a><span data-ttu-id="3b5f7-118">建立第一個應用程式</span><span class="sxs-lookup"><span data-stu-id="3b5f7-118">Creating Your First Application</span></span>

<span data-ttu-id="3b5f7-119">按一下 **新的專案**，然後選取**Visual C#** 左邊，然後**Web** ，然後選取**ASP.NET Web 應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-119">Click **New Project**, then select **Visual C#** on the left, then **Web** and then select **ASP.NET Web Application**.</span></span> <span data-ttu-id="3b5f7-120">命名您的專案 「 MvcAuth"，然後按一下**確定**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-120">Name your project "MvcAuth" and then click **OK**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image2.png)

<span data-ttu-id="3b5f7-121">在 [**新的 ASP.NET 專案**] 對話方塊中，按一下**MVC**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-121">In the **New ASP.NET Project** dialog, click **MVC**.</span></span> <span data-ttu-id="3b5f7-122">如果無法驗證**個別使用者帳戶**，按一下**變更驗證**按鈕，然後選取**個別使用者帳戶**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-122">If the Authentication is not **Individual User Accounts**, click the **Change Authentication** button and select **Individual User Accounts**.</span></span> <span data-ttu-id="3b5f7-123">藉由檢查**雲端中的主機**，應用程式將會很容易就能在 Azure 中裝載。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-123">By checking **Host in the cloud**, the app will be very easy to host in Azure.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image3.png)

<span data-ttu-id="3b5f7-124">如果您選取**雲端中的主機**，完成 [設定] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-124">If you selected **Host in the cloud**, complete the configure dialog.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image4.png)


### <a name="use-nuget-to-update-to-the-latest-owin-middleware"></a><span data-ttu-id="3b5f7-125">使用 NuGet 來更新至最新的 OWIN 中介軟體</span><span class="sxs-lookup"><span data-stu-id="3b5f7-125">Use NuGet to update to the latest OWIN middleware</span></span>

<span data-ttu-id="3b5f7-126">使用 NuGet 套件管理員來更新[OWIN 中介軟體](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-126">Use the NuGet package manager to update the [OWIN middleware](../../../aspnet/overview/owin-and-katana/getting-started-with-owin-and-katana.md).</span></span> <span data-ttu-id="3b5f7-127">選取 **更新**左側功能表中。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-127">Select **Updates** in the left menu.</span></span> <span data-ttu-id="3b5f7-128">您可以按一下**全部更新** 按鈕，或者您可以搜尋只 OWIN 套件 （在下圖所示）：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-128">You can click on the **Update All** button or you can search for only OWIN packages (shown in the next image):</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image5.png)

<span data-ttu-id="3b5f7-129">下圖中會顯示只有 OWIN 封裝：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-129">In the image below, only OWIN packages are shown:</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image6.png)

<span data-ttu-id="3b5f7-130">從套件管理員主控台 (PMC)，您可以輸入`Update-Package`命令，將會更新所有封裝。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-130">From the Package Manager Console (PMC), you can enter the `Update-Package` command, which will update all packages.</span></span>

<span data-ttu-id="3b5f7-131">按下**F5**或是**Ctrl + F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-131">Press **F5** or **Ctrl+F5** to run the application.</span></span> <span data-ttu-id="3b5f7-132">在下面的影像中的連接埠號碼是 1234。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-132">In the image below, the port number is 1234.</span></span> <span data-ttu-id="3b5f7-133">當您執行應用程式時，您會看到不同的連接埠號碼。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-133">When you run the application, you'll see a different port number.</span></span>

<span data-ttu-id="3b5f7-134">根據您的瀏覽器視窗的大小，您可能需要按一下 瀏覽圖示，以查看**首頁**，**有關**，**連絡**，**註冊**並**登入**連結。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-134">Depending on the size of your browser window, you might need to click the navigation icon to see the **Home**, **About**, **Contact**, **Register** and **Log in** links.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image7.png)  
![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image8.png) 

<a id="ssl"></a>
## <a name="setting-up-ssl-in-the-project"></a><span data-ttu-id="3b5f7-135">設定專案中的 SSL</span><span class="sxs-lookup"><span data-stu-id="3b5f7-135">Setting up SSL in the Project</span></span>

<span data-ttu-id="3b5f7-136">若要連接到 Google 和 Facebook 等的驗證提供者，您必須將 IIS Express 設定為使用 SSL。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-136">To connect to authentication providers like Google and Facebook, you will need to set up IIS-Express to use SSL.</span></span> <span data-ttu-id="3b5f7-137">務必要保留登入之後，使用 SSL 以及不卸除回為 HTTP，您的登入 cookie 只做為密碼做為您的使用者名稱和密碼，且未使用的 SSL，您要先將它以純文字傳送，透過網路。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-137">It's important to keep using SSL after login and not drop back to HTTP, your login cookie is just as secret as your username and password, and without using SSL you're sending it in clear-text across the wire.</span></span> <span data-ttu-id="3b5f7-138">此外，您等於已經跨執行交握及安全通道 （這是大多數有何 HTTPS 比 HTTP 更慢） 的時間執行 MVC 管線之前，因此重新導向回 HTTP 之後您的登入並不會造成未來的目前要求要求快得多。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-138">Besides, you've already taken the time to perform the handshake and secure the channel (which is the bulk of what makes HTTPS slower than HTTP) before the MVC pipeline is run, so redirecting back to HTTP after you're logged in won't make the current request or future requests much faster.</span></span>

1. <span data-ttu-id="3b5f7-139">在 **方案總管**，按一下**MvcAuth**專案。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-139">In **Solution Explorer**, click the **MvcAuth** project.</span></span>
2. <span data-ttu-id="3b5f7-140">按 F4 鍵以顯示專案屬性。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-140">Hit the F4 key to show the project properties.</span></span> <span data-ttu-id="3b5f7-141">或者，從**檢視**您可以選取的功能表**屬性 視窗**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-141">Alternatively, from the **View** menu you can select **Properties Window**.</span></span>
3. <span data-ttu-id="3b5f7-142">變更**啟用 SSL**設為 True。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-142">Change **SSL Enabled** to True.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image9.png)
4. <span data-ttu-id="3b5f7-143">複製 SSL URL (這將成為`https://localhost:44300/`除非您已建立 SSL 的其他專案)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-143">Copy the SSL URL (which will be `https://localhost:44300/` unless you've created other SSL projects).</span></span>
5. <span data-ttu-id="3b5f7-144">在 **方案總管**，以滑鼠右鍵按一下**MvcAuth**專案，然後選取**屬性**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-144">In **Solution Explorer**, right click the **MvcAuth** project and select **Properties**.</span></span>
6. <span data-ttu-id="3b5f7-145">選取 [ **Web**索引標籤，然後貼到 SSL URL**專案 Url** ] 方塊中。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-145">Select the **Web** tab, and then paste the SSL URL into the **Project Url** box.</span></span> <span data-ttu-id="3b5f7-146">儲存檔案 (Ctl + S)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-146">Save the file (Ctl+S).</span></span> <span data-ttu-id="3b5f7-147">您將需要此 URL，以設定 Facebook 和 Google 驗證的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-147">You will need this URL to configure Facebook and Google authentication apps.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image10.png)
7. <span data-ttu-id="3b5f7-148">新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)屬性設定為`Home`控制器要求所有要求都必須使用 HTTPS。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-148">Add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) attribute to the `Home` controller to require all requests must use HTTPS.</span></span> <span data-ttu-id="3b5f7-149">更安全的方法是新增[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx)應用程式的篩選條件。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-149">A more secure approach is to add the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute.aspx) filter to the application.</span></span> <span data-ttu-id="3b5f7-150">請參閱章節&quot;保護的應用程式使用 SSL 和授權屬性&quot;在我 tutoral[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-150">See the section &quot;Protect the Application with SSL and the Authorize Attribute&quot; in my tutoral [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data).</span></span> <span data-ttu-id="3b5f7-151">主控制器的部分如下所示。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-151">A portion of the Home controller is shown below.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample1.cs?highlight=1)]
8. <span data-ttu-id="3b5f7-152">按 CTRL+F5 執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-152">Press CTRL+F5 to run the application.</span></span> <span data-ttu-id="3b5f7-153">如果您已安裝憑證，在過去，您可以略過本節的其餘部分，並跳至[建立 Google app for OAuth 2 和應用程式連接到專案](#goog)，否則請遵循指示來信任的自我簽署IIS Express 產生的憑證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-153">If you've installed the certificate in the past, you can skip the rest of this section and jump to [Creating a Google app for OAuth 2 and connecting the app to the project](#goog), otherwise, follow the instructions to trust the self-signed certificate that IIS Express has generated.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image11.png)
9. <span data-ttu-id="3b5f7-154">讀取**安全性警告**對話方塊，然後按一下**是**如果您想要安裝憑證，代表 localhost。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-154">Read the **Security Warning** dialog and then click **Yes** if you want to install the certificate representing localhost.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image12.png)
10. <span data-ttu-id="3b5f7-155">IE 顯示*首頁*頁面上，並有沒有出現 SSL 警告。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-155">IE shows the *Home* page and there are no SSL warnings.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image13.png)
11. <span data-ttu-id="3b5f7-156">Google Chrome 也接受憑證，並會顯示 HTTPS 內容，而不發出警告。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-156">Google Chrome also accepts the certificate and will show HTTPS content without a warning.</span></span> <span data-ttu-id="3b5f7-157">Firefox 會使用自己的憑證存放區，因此它會顯示警告。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-157">Firefox uses its own certificate store, so it will display a warning.</span></span> <span data-ttu-id="3b5f7-158">我們的應用程式對於您可以放心地按一下**我了解風險**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-158">For our application you can safely click **I Understand the Risks**.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image14.png)

<a id="goog"></a>
## <a name="creating-a-google-app-for-oauth-2-and-connecting-the-app-to-the-project"></a><span data-ttu-id="3b5f7-159">建立 Google app for OAuth 2 和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="3b5f7-159">Creating a Google app for OAuth 2 and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="3b5f7-160">目前的 Google OAuth 指示，請參閱[ASP.NET Core 中的 設定 Google 驗證](/aspnet/core/security/authentication/social/google-logins)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-160">For current Google OAuth instructions, see [Configuring Google authentication in ASP.NET Core](/aspnet/core/security/authentication/social/google-logins).</span></span>

1. <span data-ttu-id="3b5f7-161">瀏覽至[Google 開發人員主控台](https://console.developers.google.com/)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-161">Navigate to the [Google Developers Console](https://console.developers.google.com/).</span></span>
2. <span data-ttu-id="3b5f7-162">如果您尚未建立的專案之前，請選取**認證**左側的索引標籤，然後選取**建立**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-162">If you haven't created a project before, select **Credentials** in the left tab, and then select **Create**.</span></span>
3. <span data-ttu-id="3b5f7-163">在左側索引標籤中，按一下**認證**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-163">In the left tab, click **Credentials**.</span></span>
4. <span data-ttu-id="3b5f7-164">按一下 **建立認證**再**OAuth 用戶端識別碼**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-164">Click **Create credentials** then **OAuth client ID**.</span></span> 

    1. <span data-ttu-id="3b5f7-165">在 [**建立用戶端識別碼**] 對話方塊中，保留預設值**Web 應用程式**應用程式類型。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-165">In the **Create Client ID** dialog, keep the default **Web application** for the application type.</span></span>
    2. <span data-ttu-id="3b5f7-166">設定**授權的 JavaScript**來源，則您在上面使用的 SSL URL (`https://localhost:44300/`除非您已建立 SSL 的其他專案)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-166">Set the **Authorized JavaScript** origins to the SSL URL you used above (`https://localhost:44300/` unless you've created other SSL projects)</span></span>
    3. <span data-ttu-id="3b5f7-167">設定**授權重新導向 URI**來：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-167">Set the **Authorized redirect URI** to:</span></span>  
         `https://localhost:44300/signin-google`
5. <span data-ttu-id="3b5f7-168">按一下 OAuth 同意畫面功能表項目，然後設定您的電子郵件地址和產品名稱。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-168">Click the OAuth Consent screen menu item, then set your email address and product name.</span></span> <span data-ttu-id="3b5f7-169">當您完成表單按一下**儲存**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-169">When you have completed the form click **Save**.</span></span>
6. <span data-ttu-id="3b5f7-170">按一下 [程式庫] 功能表項目、 搜尋**Google + API**、 對它按一下，然後按下啟用。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-170">Click the Library menu item, search **Google+ API**, click on it then press Enable.</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image15.png)  
  
   <span data-ttu-id="3b5f7-171">下圖顯示已啟用的 Api。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-171">The image below shows the enabled APIs.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image16.png)
7. <span data-ttu-id="3b5f7-172">從 [Google Api API 管理員] 中，瀏覽**認證**索引標籤，以取得**用戶端識別碼**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-172">From the Google APIs API Manager, visit the **Credentials** tab to obtain the **Client ID**.</span></span> <span data-ttu-id="3b5f7-173">若要對應用程式祕密儲存 JSON 檔案的下載。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-173">Download to save a JSON file with application secrets.</span></span> <span data-ttu-id="3b5f7-174">複製並貼上**ClientId**並**ClientSecret**成`UseGoogleAuthentication`方法中找到*Startup.Auth.cs*檔案*App_Start*資料夾。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-174">Copy and paste the **ClientId** and **ClientSecret** into the `UseGoogleAuthentication` method found in the *Startup.Auth.cs* file in the *App_Start* folder.</span></span> <span data-ttu-id="3b5f7-175">**ClientId**並**ClientSecret**如下所示的值是範例，而且無法運作。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-175">The **ClientId** and **ClientSecret** values shown below are samples and don't work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample2.cs?highlight=37-39)]

    > [!WARNING]
    > <span data-ttu-id="3b5f7-176">安全性-機密資料絕不儲存在原始程式碼中。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-176">Security - Never store sensitive data in your source code.</span></span> <span data-ttu-id="3b5f7-177">上述保持簡單的範例程式碼中加入的帳戶和認證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-177">The account and credentials are added to the code above to keep the sample simple.</span></span> <span data-ttu-id="3b5f7-178">請參閱[將密碼和其他機密資料部署到 ASP.NET 和 Azure App Service 最佳作法](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-178">See [Best practices for deploying passwords and other sensitive data to ASP.NET and Azure App Service](../../../identity/overview/features-api/best-practices-for-deploying-passwords-and-other-sensitive-data-to-aspnet-and-azure.md).</span></span>
8. <span data-ttu-id="3b5f7-179">按下**CTRL + F5**以建置並執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-179">Press **CTRL+F5** to build and run the application.</span></span> <span data-ttu-id="3b5f7-180">按一下 **登入**連結。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-180">Click the **Log in** link.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image17.png)
9. <span data-ttu-id="3b5f7-181">底下**另一個服務用來登入**，按一下**Google**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-181">Under **Use another service to log in**, click **Google**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image18.png)

    > [!NOTE]
    > <span data-ttu-id="3b5f7-182">若您遺漏任何上述步驟，您會將 HTTP 401 錯誤。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-182">If you miss any of the steps above you will get a HTTP 401 error.</span></span> <span data-ttu-id="3b5f7-183">重新檢查您上述的步驟。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-183">Recheck your steps above.</span></span> <span data-ttu-id="3b5f7-184">如果您遺漏必要的設定 (例如**產品名稱**)、 新增遺失的項目，並儲存，可能需要幾分鐘，讓驗證才能運作。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-184">If you miss a required setting (for example **product name**), add the missing item and save; it can take a few minutes for authentication to work.</span></span>
10. <span data-ttu-id="3b5f7-185">您將會重新導向至 Google 網站，您會在此輸入您的認證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-185">You will be redirected to the Google site where you will enter your credentials.</span></span>   
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image19.png)
11. <span data-ttu-id="3b5f7-186">您輸入認證之後，系統會提示您授與您剛才建立的 web 應用程式的權限：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-186">After you enter your credentials, you will be prompted to give permissions to the web application you just created:</span></span>
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image20.png)
12. <span data-ttu-id="3b5f7-187">按一下 **接受**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-187">Click **Accept**.</span></span> <span data-ttu-id="3b5f7-188">您現在會重新導向回到**註冊**MvcAuth 應用程式，您可以在此註冊您的 Google 帳戶頁面。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-188">You will now be redirected back to the **Register** page of the MvcAuth application where you can register your Google account.</span></span> <span data-ttu-id="3b5f7-189">您可以選擇變更用於 Gmail 帳戶、 本機電子郵件註冊名稱，但您通常想要保留預設電子郵件別名 （也就是一個您用來驗證）。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-189">You have the option of changing the local email registration name used for your Gmail account, but you generally want to keep the default email alias (that is, the one you used for authentication).</span></span> <span data-ttu-id="3b5f7-190">按一下 [註冊]。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-190">Click **Register**.</span></span>  
  
    ![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image21.png)

<a id="fb"></a>
## <a name="creating-the-app-in-facebook-and-connecting-the-app-to-the-project"></a><span data-ttu-id="3b5f7-191">在 Facebook 中建立應用程式和應用程式連線至專案</span><span class="sxs-lookup"><span data-stu-id="3b5f7-191">Creating the app in Facebook and connecting the app to the project</span></span>

> [!WARNING]
> <span data-ttu-id="3b5f7-192">目前 Facebook OAuth2 驗證的指示，請參閱[設定 Facebook 驗證](/aspnet/core/security/authentication/social/facebook-logins)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-192">For current Facebook OAuth2 authentication instructions, see [Configuring Facebook authentication](/aspnet/core/security/authentication/social/facebook-logins)</span></span>

<span data-ttu-id="3b5f7-193">Facebook OAuth2 驗證，您需要將複製到您的專案部分設定從您在 Facebook 中建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-193">For Facebook OAuth2 authentication, you need to copy to your project some settings from an application that you create in Facebook.</span></span>

1. <span data-ttu-id="3b5f7-194">在瀏覽器中瀏覽至[ https://developers.facebook.com/apps ](https://developers.facebook.com/apps)並輸入您的 Facebook 認證登入。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-194">In your browser, navigate to [https://developers.facebook.com/apps](https://developers.facebook.com/apps) and log in by entering your Facebook credentials.</span></span>
2. <span data-ttu-id="3b5f7-195">如果您未註冊為 Facebook 開發人員，按一下**註冊為開發人員**並依照指示進行註冊。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-195">If you aren't already registered as a Facebook developer, click **Register as a Developer** and follow the directions to register.</span></span>
3. <span data-ttu-id="3b5f7-196">在 **應用程式**索引標籤上，按一下**建立新的應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-196">On the **Apps** tab, click **Create New App**.</span></span>

    ![建立新的應用程式](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image22.png)
4. <span data-ttu-id="3b5f7-198">請輸入**應用程式名稱**並**類別目錄**，然後按一下 **建立的應用程式**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-198">Enter an **App Name** and **Category**, then click **Create App**.</span></span>

    <span data-ttu-id="3b5f7-199"><strong>應用程式命名空間</strong>屬於您的應用程式將用來存取驗證的 Facebook 應用程式的 URL (例如，https\://apps.facebook.com/{App 命名空間})。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-199">The <strong>App Namespace</strong> is the part of the URL that your App will use to access the Facebook application for authentication (for example, https\://apps.facebook.com/{App Namespace}).</span></span> <span data-ttu-id="3b5f7-200">如果您未指定<strong>應用程式命名空間</strong>，則<strong>應用程式識別碼</strong>將用於 URL。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-200">If you don't specify an <strong>App Namespace</strong>, the <strong>App ID</strong> will be used for the URL.</span></span> <span data-ttu-id="3b5f7-201"><strong>應用程式識別碼</strong>是長時間的系統產生的數字，您會看到的下一個步驟。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-201">The <strong>App ID</strong> is a long system-generated number that you will see in the next step.</span></span>

    ![建立新的應用程式 對話方塊](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image23.png)
5. <span data-ttu-id="3b5f7-203">送出標準的安全性檢查。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-203">Submit the standard security check.</span></span>

    ![安全性檢查](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image24.png)
6. <span data-ttu-id="3b5f7-205">選取 **設定**左側的功能表列![Facebook 開發人員的功能表列](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-205">Select **Settings** for the left menu bar.![Facebook Developer's menu bar](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image25.png)</span></span>
7. <span data-ttu-id="3b5f7-206">在上**基本**選取頁面的 [設定] 區段**新增平台**來指定您要加入網站應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-206">On the **Basic** settings section of the page select **Add Platform** to specify that you are adding a website application.</span></span> <span data-ttu-id="3b5f7-207">![基本設定](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-207">![Basic Settings](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image26.png)</span></span>
8. <span data-ttu-id="3b5f7-208">選取 **網站**從平台的選擇。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-208">Select **Website** from the platform choices.</span></span>  
  
    ![平台選擇](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image27.png)
9. <span data-ttu-id="3b5f7-210">請記下的您**應用程式識別碼**和您**應用程式祕密**，讓您可以新增到您的 MVC 應用程式的這兩個稍後在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-210">Make a note of your **App ID** and your **App Secret** so that you can add both into your MVC application later in this tutorial.</span></span> <span data-ttu-id="3b5f7-211">此外，新增您的網站 URL (`https://localhost:44300/`) 來測試您的 MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-211">Also, Add your Site URL (`https://localhost:44300/`) to test your MVC application.</span></span> <span data-ttu-id="3b5f7-212">此外，新增**Contact Email**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-212">Also, add a **Contact Email**.</span></span> <span data-ttu-id="3b5f7-213">然後，選取**儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-213">Then, select **Save Changes**.</span></span>   

    ![基本應用程式詳細資料頁面](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image28.png)

    > [!NOTE]
    > <span data-ttu-id="3b5f7-215">請注意，您將只能使用已註冊的電子郵件別名來進行驗證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-215">Note that you will only be able to authenticate using the email alias you have registered.</span></span> <span data-ttu-id="3b5f7-216">其他使用者與測試帳戶將無法註冊。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-216">Other users and test accounts will not be able to register.</span></span> <span data-ttu-id="3b5f7-217">您可以授與其他 Facebook 帳戶的存取權的 Facebook 應用程式**開發人員角色** 索引標籤。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-217">You can grant other Facebook accounts access to the application on the Facebook **Developer Roles** tab.</span></span>
10. <span data-ttu-id="3b5f7-218">在 Visual Studio 中開啟*應用程式\_Start\Startup.Auth.cs*。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-218">In Visual Studio, open *App\_Start\Startup.Auth.cs*.</span></span>
11. <span data-ttu-id="3b5f7-219">複製並貼上**AppId**並**應用程式祕密**成`UseFacebookAuthentication`方法。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-219">Copy and paste the **AppId** and **App Secret** into the `UseFacebookAuthentication` method.</span></span> <span data-ttu-id="3b5f7-220">**AppId**並**應用程式祕密**如下所示的值是範例，將無法運作。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-220">The **AppId** and **App Secret** values shown below are samples and will not work.</span></span>

    [!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample3.cs?highlight=33-35,38-39)]
12. <span data-ttu-id="3b5f7-221">按一下 **儲存變更**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-221">Click **Save Changes**.</span></span>
13. <span data-ttu-id="3b5f7-222">按下**CTRL + F5**執行應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-222">Press **CTRL+F5** to run the application.</span></span>


<span data-ttu-id="3b5f7-223">選取 **登入**以顯示登入頁面。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-223">Select **Log in** to display the Login page.</span></span> <span data-ttu-id="3b5f7-224">按一下  **Facebook**下方**使用其他服務進行登入。**</span><span class="sxs-lookup"><span data-stu-id="3b5f7-224">Click **Facebook** under **Use another service to log in.**</span></span>

<span data-ttu-id="3b5f7-225">輸入您的 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-225">Enter your Facebook credentials.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image29.png)

<span data-ttu-id="3b5f7-226">系統會提示您授與應用程式存取您的公用設定檔及朋友清單的權限。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-226">You will be prompted to grant permission for the application to access your public profile and friend list.</span></span>

![Facebook 應用程式詳細資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image30.png)

<span data-ttu-id="3b5f7-228">您現在已登入。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-228">You are now logged in.</span></span> <span data-ttu-id="3b5f7-229">您現在可以註冊此帳戶與應用程式。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-229">You can now register this account with the application.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image31.png)

<span data-ttu-id="3b5f7-230">當您註冊時，要將項目加入至*使用者*的成員資格資料庫的資料表。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-230">When you register, an entry is added to the *Users* table of the membership database.</span></span>

<a id="mdb"></a>
## <a name="examine-the-membership-data"></a><span data-ttu-id="3b5f7-231">檢查成員資格資料</span><span class="sxs-lookup"><span data-stu-id="3b5f7-231">Examine the Membership Data</span></span>

<span data-ttu-id="3b5f7-232">在 **檢視**功能表上，按一下**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-232">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image32.png)

<span data-ttu-id="3b5f7-233">依序展開**DefaultConnection (MvcAuth)**，展開**資料表**，以滑鼠右鍵按一下**AspNetUsers**然後按一下**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-233">Expand **DefaultConnection (MvcAuth)**, expand **Tables**, right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image33.png)

![aspnetusers 資料表資料](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image34.png)

<a id="ap"></a>
## <a name="adding-profile-data-to-the-user-class"></a><span data-ttu-id="3b5f7-235">將設定檔資料新增至 User 類別</span><span class="sxs-lookup"><span data-stu-id="3b5f7-235">Adding Profile Data to the User Class</span></span>

<span data-ttu-id="3b5f7-236">在本節中您將新增出生日期和主要城鎮的使用者資料在註冊期間，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-236">In this section you'll add birth date and home town to the user data during registration, as shown in the following image.</span></span>

![使用家用 town 和 Bday reg](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image35.png)

<span data-ttu-id="3b5f7-238">開啟*Models\IdentityModels.cs*檔案，並新增出生日期和從家裡 town 屬性：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-238">Open the *Models\IdentityModels.cs* file and add birth date and home town properties:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample4.cs?highlight=3-4)]

<span data-ttu-id="3b5f7-239">開啟*Models\AccountViewModels.cs*檔案與設定的 birth 中的日期和從家裡 town 屬性`ExternalLoginConfirmationViewModel`。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-239">Open the *Models\AccountViewModels.cs* file and the set birth date and home town properties in `ExternalLoginConfirmationViewModel`.</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample5.cs?highlight=8-9)]

<span data-ttu-id="3b5f7-240">開啟*Controllers\AccountController.cs*檔案，並新增程式碼中的出生日期和從家裡 town`ExternalLoginConfirmation`動作方法所示：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-240">Open the *Controllers\AccountController.cs* file and add code for birth date and home town in the `ExternalLoginConfirmation` action method as shown:</span></span>

[!code-csharp[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample6.cs?highlight=21-23)]

<span data-ttu-id="3b5f7-241">新增出生日期及首頁的城鎮*Views\Account\ExternalLoginConfirmation.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-241">Add birth date and home town to the *Views\Account\ExternalLoginConfirmation.cshtml* file:</span></span>

[!code-cshtml[Main](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/samples/sample7.cshtml?highlight=27-40)]

<span data-ttu-id="3b5f7-242">因此您可以再次向您的應用程式註冊您的 Facebook 帳戶，並確認您可以加入新的出生日期和主要城鎮的設定檔資訊，請刪除成員資格資料庫。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-242">Delete the membership database so you can again register your Facebook account with your application and verify you can add the new birth date and home town profile information.</span></span>

<span data-ttu-id="3b5f7-243">從**方案總管**，按一下**顯示所有檔案**圖示，然後以滑鼠右鍵按一下*新增\_Data\aspnet-MvcAuth-&lt;時間戳記&gt;.mdf* ，按一下 **刪除**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-243">From **Solution Explorer**, click the **Show All Files** icon, then right click *Add\_Data\aspnet-MvcAuth-&lt;dateStamp&gt;.mdf* and click **Delete**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image36.png)

<span data-ttu-id="3b5f7-244">從**工具**功能表上，按一下**NuGet 封裝管理員**，然後按一下**Package Manager Console** (PMC)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-244">From the **Tools** menu, click **NuGet Package Manger**, then click **Package Manager Console** (PMC).</span></span> <span data-ttu-id="3b5f7-245">在 PMC 中，輸入下列命令。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-245">Enter the following commands in the PMC.</span></span>

1. <span data-ttu-id="3b5f7-246">啟用移轉</span><span class="sxs-lookup"><span data-stu-id="3b5f7-246">Enable-Migrations</span></span>
2. <span data-ttu-id="3b5f7-247">新增移轉 Init</span><span class="sxs-lookup"><span data-stu-id="3b5f7-247">Add-Migration Init</span></span>
3. <span data-ttu-id="3b5f7-248">更新資料庫</span><span class="sxs-lookup"><span data-stu-id="3b5f7-248">Update-Database</span></span>

<span data-ttu-id="3b5f7-249">執行應用程式，並使用 FaceBook 和 Google 登入並註冊某些使用者。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-249">Run the application and use FaceBook and Google to log in and register some users.</span></span>

## <a name="examine-the-membership-data"></a><span data-ttu-id="3b5f7-250">檢查成員資格資料</span><span class="sxs-lookup"><span data-stu-id="3b5f7-250">Examine the Membership Data</span></span>

<span data-ttu-id="3b5f7-251">在 **檢視**功能表上，按一下**伺服器總管**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-251">In the **View** menu, click **Server Explorer**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image37.png)

<span data-ttu-id="3b5f7-252">以滑鼠右鍵按一下**AspNetUsers**然後按一下**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-252">Right click **AspNetUsers** and click **Show Table Data**.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image38.png)

<span data-ttu-id="3b5f7-253">`HomeTown`和`BirthDate`如下所示的欄位。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-253">The `HomeTown` and `BirthDate` fields are shown below.</span></span>

![](create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on/_static/image39.png)

<a id="off"></a>
## <a name="logging-off-your-app-and-logging-in-with-another-account"></a><span data-ttu-id="3b5f7-254">登出您的應用程式，並使用另一個帳戶中的記錄</span><span class="sxs-lookup"><span data-stu-id="3b5f7-254">Logging off your App and Logging in With Another Account</span></span>

<span data-ttu-id="3b5f7-255">如果您登入您的應用程式使用 Facebook、，然後登出並嘗試登入一次使用不同的 Facebook 帳戶 （使用相同的瀏覽器），您會立即登入使用先前用過的 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-255">If you log on to your app with Facebook,, and then log out and try to log in again with a different Facebook account (using the same browser), you will be immediately logged in to the previous Facebook account you used.</span></span> <span data-ttu-id="3b5f7-256">若要使用另一個帳戶，您需要瀏覽至 Facebook，並在 Facebook 登出。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-256">In order to use another account, you need to navigate to Facebook and log out at Facebook.</span></span> <span data-ttu-id="3b5f7-257">相同的規則適用於任何其他第 3 方驗證提供者。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-257">The same rule applies to any other 3rd party authentication provider.</span></span> <span data-ttu-id="3b5f7-258">或者，您可以使用不同的瀏覽器登入另一個帳戶。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-258">Alternatively, you can log in with another account by using a different browser.</span></span>

## <a name="next-steps"></a><span data-ttu-id="3b5f7-259">後續步驟</span><span class="sxs-lookup"><span data-stu-id="3b5f7-259">Next Steps</span></span>

<span data-ttu-id="3b5f7-260">請參閱[簡介 Yahoo 和 LinkedIn OAuth 安全性提供者設定 owin](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/)由 Jerrie Pelser Yahoo 與 LinkedIn 的指示。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-260">See [Introducing the Yahoo and LinkedIn OAuth security providers for OWIN](http://www.jerriepelser.com/blog/introducing-the-yahoo-linkedin-oauth-security-providers-for-owin/) by Jerrie Pelser for Yahoo and LinkedIn instructions.</span></span> <span data-ttu-id="3b5f7-261">請參閱 Jerrie 的美化取得啟用社交登入按鈕的 ASP.NET MVC 5 的社交登入按鈕。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-261">See Jerrie's Pretty social login buttons for ASP.NET MVC 5 to get enable social login buttons.</span></span>

<span data-ttu-id="3b5f7-262">請遵循我教學課程[使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)，它會繼續本教學課程中，並顯示下列：</span><span class="sxs-lookup"><span data-stu-id="3b5f7-262">Follow my tutorial [Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data), which continues this tutorial and shows the following:</span></span>

1. <span data-ttu-id="3b5f7-263">如何將您的應用程式部署至 Azure。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-263">How to deploy your app to Azure.</span></span>
2. <span data-ttu-id="3b5f7-264">如何保護您的應用程式角色。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-264">How to secure you app with roles.</span></span>
3. <span data-ttu-id="3b5f7-265">如何保護您的應用程式[RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx)並[授權](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx)篩選器。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-265">How to secure your app with the [RequireHttps](https://msdn.microsoft.com/library/system.web.mvc.requirehttpsattribute(v=vs.108).aspx) and [Authorize](https://msdn.microsoft.com/library/system.web.mvc.authorizeattribute(v=vs.100).aspx) filters.</span></span>
4. <span data-ttu-id="3b5f7-266">如何使用成員資格 API 來新增使用者和角色。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-266">How to use the membership API to add users and roles.</span></span>

<span data-ttu-id="3b5f7-267">您喜歡本教學課程中的方式，和我們可以改善，歡迎留下意見反應。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-267">Please leave feedback on how you liked this tutorial and what we could improve.</span></span> <span data-ttu-id="3b5f7-268">您也可以要求在新的主題[顯示我如何使用程式碼](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-268">You can also request new topics at [Show Me How With Code](http://aspnet.uservoice.com/forums/228522-show-me-how-with-code).</span></span> <span data-ttu-id="3b5f7-269">您甚至可以要求並票選新功能新增至 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-269">You can even ask for and vote on new features to be added to ASP.NET.</span></span> <span data-ttu-id="3b5f7-270">例如，您可以在此投票的工具[建立和管理使用者和角色。](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span><span class="sxs-lookup"><span data-stu-id="3b5f7-270">For example, you can vote for a tool to [create and manage users and roles.](http://aspnet.uservoice.com/forums/41199-general-asp-net/suggestions/5646857-asp-net-identity-membership-db-tool-to-mangage-use)</span></span>

<span data-ttu-id="3b5f7-271">ASP.NET 外部驗證服務的運作方式的合理的解釋，請參閱 Robert McMurray[外部驗證服務](https://asp.net/web-api/overview/security/external-authentication-services)。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-271">For an good explanation of how ASP.NET External Authentication Services work, see Robert McMurray's [External Authentication Services](https://asp.net/web-api/overview/security/external-authentication-services).</span></span> <span data-ttu-id="3b5f7-272">Robert 的文章也會啟用 Microsoft 和 Twitter 驗證的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-272">Robert's article also goes into detail in enabling Microsoft and Twitter authentication.</span></span> <span data-ttu-id="3b5f7-273">Tom Dykstra 的傑出[EF/MVC 教學課程](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md)示範如何使用 Entity Framework。</span><span class="sxs-lookup"><span data-stu-id="3b5f7-273">Tom Dykstra's excellent [EF/MVC tutorial](../getting-started/getting-started-with-ef-using-mvc/creating-an-entity-framework-data-model-for-an-asp-net-mvc-application.md) shows how to work with the Entity Framework.</span></span>
