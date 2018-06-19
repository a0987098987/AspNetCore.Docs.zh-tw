---
uid: web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
title: 使用 ASP.NET Web 中的外部網站登入頁面 (Razor) 站台 |Microsoft 文件
author: tfitzmac
description: 這篇文章說明如何登入使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何支援...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/21/2014
ms.topic: article
ms.assetid: ef852096-a5bf-47b3-9945-125cde065093
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/security/enabling-login-from-external-sites-in-an-aspnet-web-pages-site
msc.type: authoredcontent
ms.openlocfilehash: 47d15686194b15b7b06a99d63125c19a41f91ed9
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26530167"
---
<a name="logging-in-using-external-sites-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="e41fe-103">使用外部站台的 ASP.NET Web Pages (Razor) 網站中登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-103">Logging In Using External Sites in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="e41fe-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="e41fe-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="e41fe-105">這篇文章說明如何登入使用 Facebook、 Google、 Twitter、 Yahoo 和其他站台的 ASP.NET Web Pages (Razor) 網站 — 也就是如何在您的站台支援 OAuth 和 OpenID。</span><span class="sxs-lookup"><span data-stu-id="e41fe-105">This article explains how to log in to your ASP.NET Web Pages (Razor) site using Facebook, Google, Twitter, Yahoo, and other sites — that is, how to support OAuth and OpenID in your site.</span></span>
> 
> <span data-ttu-id="e41fe-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="e41fe-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="e41fe-107">如何啟用登入從其他站台，當您使用 WebMatrix 入門網站範本。</span><span class="sxs-lookup"><span data-stu-id="e41fe-107">How to enable login from other sites when you use the WebMatrix Starter Site template.</span></span>
> 
> <span data-ttu-id="e41fe-108">這是發行項中所引進的 ASP.NET 功能：</span><span class="sxs-lookup"><span data-stu-id="e41fe-108">This is the ASP.NET feature introduced in the article:</span></span>
> 
> - <span data-ttu-id="e41fe-109">`OAuthWebSecurity`協助程式。</span><span class="sxs-lookup"><span data-stu-id="e41fe-109">The `OAuthWebSecurity` helper.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="e41fe-110">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="e41fe-110">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="e41fe-111">ASP.NET Web Pages (Razor) 2</span><span class="sxs-lookup"><span data-stu-id="e41fe-111">ASP.NET Web Pages (Razor) 2</span></span>
> - <span data-ttu-id="e41fe-112">WebMatrix 3</span><span class="sxs-lookup"><span data-stu-id="e41fe-112">WebMatrix 3</span></span>

<span data-ttu-id="e41fe-113">ASP.NET Web 網頁包含支援[OAuth](http://oauth.net/)和[OpenID](http://openid.net/)提供者。</span><span class="sxs-lookup"><span data-stu-id="e41fe-113">ASP.NET Web Pages includes support for [OAuth](http://oauth.net/) and [OpenID](http://openid.net/) providers.</span></span> <span data-ttu-id="e41fe-114">使用這些提供者，您可以讓使用者登入您的網站使用其現有的認證從 Facebook、 Twitter、 Microsoft 與 Google。</span><span class="sxs-lookup"><span data-stu-id="e41fe-114">Using these providers, you can let users log into your site using their existing credentials from Facebook, Twitter, Microsoft, and Google.</span></span> <span data-ttu-id="e41fe-115">例如，使用 Facebook 帳戶進行登入，使用者就可以選擇一個 Facebook 圖示，重新導向至在其中輸入他們的使用者資訊的 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e41fe-115">For example, to log in using a Facebook account, users can just choose a Facebook icon, which redirects them to the Facebook login page where they enter their user information.</span></span> <span data-ttu-id="e41fe-116">他們可將其網站上的帳戶產生關聯 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-116">They can then associate the Facebook login with their account on your site.</span></span> <span data-ttu-id="e41fe-117">網頁的成員資格功能相關的增強功能是，使用者可將多個登入 （包括從社交網路網站的登入） 與您的網站上的單一帳戶。</span><span class="sxs-lookup"><span data-stu-id="e41fe-117">A related enhancement to the Web Pages membership features is that users can associate multiple logins (including logins from social networking sites) with a single account on your website.</span></span>

<span data-ttu-id="e41fe-118">下圖顯示登入頁面**入門網站**範本，使用者可以選擇 Facebook、 Twitter、 Google 或 Microsoft 的圖示，以啟用外部的帳戶登入：</span><span class="sxs-lookup"><span data-stu-id="e41fe-118">This image shows the Login page from the **Starter Site** template, where a user can choose a Facebook, Twitter, Google or Microsoft icon to enable logging in with an external account:</span></span>

![外部提供者](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image1.png)

<span data-ttu-id="e41fe-120">您可以啟用 OAuth 和 OpenID 成員資格中的程式碼的幾行取消註解**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e41fe-120">You can enable OAuth and OpenID membership by uncommenting a few lines of code in the **Starter Site** template.</span></span> <span data-ttu-id="e41fe-121">您的方法和屬性用來處理 OAuth 和 OpenID 提供者位於`WebMatrix.Security.OAuthWebSecurity`類別。</span><span class="sxs-lookup"><span data-stu-id="e41fe-121">The methods and properties you use to work with the OAuth and OpenID providers are in the `WebMatrix.Security.OAuthWebSecurity` class.</span></span> <span data-ttu-id="e41fe-122">**入門網站**範本包含完整的成員資格基礎結構，您需要讓使用者登入您的網站使用本機認證或來自另一個站台完成與登入頁面、 成員資格資料庫和所有的程式碼.</span><span class="sxs-lookup"><span data-stu-id="e41fe-122">The **Starter Site** template includes a full membership infrastructure, complete with a login page, a membership database, and all the code you need to let users log into your site using either local credentials or those from another site.</span></span>

<span data-ttu-id="e41fe-123">本節提供如何讓使用者登入從外部站台至站台為基礎的範例**入門網站**範本。</span><span class="sxs-lookup"><span data-stu-id="e41fe-123">This section provides an example of how to let users log in from external sites to a site that's based on the **Starter Site** template.</span></span> <span data-ttu-id="e41fe-124">建立入門網站之後, 您這樣做，（詳細資料）：</span><span class="sxs-lookup"><span data-stu-id="e41fe-124">After creating a starter site, you do this (details follow):</span></span>

- <span data-ttu-id="e41fe-125">針對使用 OAuth 提供者 （Facebook、 Twitter 和 Microsoft） 的網站，您可以建立應用程式外部站台上。</span><span class="sxs-lookup"><span data-stu-id="e41fe-125">For the sites that use an OAuth provider (Facebook, Twitter, and Microsoft), you create an application on the external site.</span></span> <span data-ttu-id="e41fe-126">這讓您將需要以叫用這些站台的登入功能的應用程式金鑰。</span><span class="sxs-lookup"><span data-stu-id="e41fe-126">This gives you application keys that you'll need in order to invoke the login feature for those sites.</span></span>
- <span data-ttu-id="e41fe-127">使用 OpenID 提供者 (Google) 的網站，您沒有建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="e41fe-127">For sites that use an OpenID provider (Google), you do not have to create an application.</span></span> <span data-ttu-id="e41fe-128">針對所有這些站台，您必須有帳戶才能登入，並建立開發人員應用程式。</span><span class="sxs-lookup"><span data-stu-id="e41fe-128">For all of these sites, you must have an account in order to log in and to create developer applications.</span></span>

    > [!NOTE]
    > <span data-ttu-id="e41fe-129">Microsoft 應用程式只接受即時工作網站 URL，因此您無法使用本機網站 URL，測試登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-129">Microsoft applications only accept a live URL for a working website, so you cannot use a local website URL for testing logins.</span></span>
- <span data-ttu-id="e41fe-130">若要指定適當的驗證提供者，以及提交的登入您想要使用的站台，請編輯網站中的幾個檔案。</span><span class="sxs-lookup"><span data-stu-id="e41fe-130">Edit a few files in your website in order to specify the appropriate authentication provider and to submit a login to the site you want to use.</span></span>

<span data-ttu-id="e41fe-131">本文提供不同的指示，進行下列工作：</span><span class="sxs-lookup"><span data-stu-id="e41fe-131">This article provides separate instructions for the following tasks:</span></span>

- [<span data-ttu-id="e41fe-132">啟用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-132">Enabling Google logins</span></span>](#To_enable_Google_logins)
- [<span data-ttu-id="e41fe-133">啟用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-133">Enabling Facebook logins</span></span>](#To_enable_Facebook_logins)
- [<span data-ttu-id="e41fe-134">啟用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-134">Enabling Twitter logins</span></span>](#To_enable_Twitter_logins)

<a id="To_enable_Google_logins"></a>
## <a name="enabling-google-logins"></a><span data-ttu-id="e41fe-135">啟用 Google 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-135">Enabling Google Logins</span></span>

1. <span data-ttu-id="e41fe-136">建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。</span><span class="sxs-lookup"><span data-stu-id="e41fe-136">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="e41fe-137">開啟 *\_AppStart.cshtml*頁面上，並取消註解下列程式碼行。</span><span class="sxs-lookup"><span data-stu-id="e41fe-137">Open the *\_AppStart.cshtml* page and uncomment the following line of code.</span></span> 

    [!code-css[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample1.css)]

### <a name="testing-google-login"></a><span data-ttu-id="e41fe-138">測試 Google 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-138">Testing Google login</span></span>

1. <span data-ttu-id="e41fe-139">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-139">Run the *default.cshtml* page of your site and choose the **Log in** button.</span></span>
2. <span data-ttu-id="e41fe-140">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇  **Google**或**Yahoo**送出按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-140">On the *Login* page, in the **Use another service to log in** section, choose either the **Google** or **Yahoo** submit button.</span></span> <span data-ttu-id="e41fe-141">這個範例會使用 Google 登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-141">This example uses the Google login.</span></span> 

    <span data-ttu-id="e41fe-142">網頁上將要求重新導向至 Google 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e41fe-142">The web page redirects the request to the Google login page.</span></span>

    ![Google 登入](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image2.png)
3. <span data-ttu-id="e41fe-144">現有的 Google 帳戶輸入認證。</span><span class="sxs-lookup"><span data-stu-id="e41fe-144">Enter credentials for an existing Google account.</span></span>
4. <span data-ttu-id="e41fe-145">如果 Google 會詢問您是否要允許*Localhost*使用來自帳戶的資訊，請按一下**允許**。</span><span class="sxs-lookup"><span data-stu-id="e41fe-145">If Google asks you whether you want to allow *Localhost* to use information from the account, click **Allow**.</span></span>

    <span data-ttu-id="e41fe-146">程式碼來驗證使用者使用 Google 語彙基元，並會返回此頁面，在您的網站。</span><span class="sxs-lookup"><span data-stu-id="e41fe-146">The code uses the Google token to authenticate the user, and then returns to this page on your website.</span></span> <span data-ttu-id="e41fe-147">此頁面可讓使用者將其 Google 登入與現有的帳戶，在您的網站產生關聯，或他們可以註冊新的帳戶產生關聯的外部登入您的站台上。</span><span class="sxs-lookup"><span data-stu-id="e41fe-147">This page lets users associate their Google login with an existing account on your website, or they can register a new account on your site to associate the external login with.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image3.png)
5. <span data-ttu-id="e41fe-149">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-149">Choose the **Associate** button.</span></span> <span data-ttu-id="e41fe-150">瀏覽器會返回應用程式的首頁。</span><span class="sxs-lookup"><span data-stu-id="e41fe-150">The browser returns to your application's home page.</span></span>

<a id="To_enable_Facebook_logins"></a>
## <a name="enabling-facebook-logins"></a><span data-ttu-id="e41fe-151">啟用 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-151">Enabling Facebook Logins</span></span>

1. <span data-ttu-id="e41fe-152">移至[Facebook 開發人員網站](https://developers.facebook.com/apps)（登入如果您未登入）。</span><span class="sxs-lookup"><span data-stu-id="e41fe-152">Go to the [Facebook developers site](https://developers.facebook.com/apps) (log in if you're not already logged in).</span></span>
2. <span data-ttu-id="e41fe-153">選擇**建立新的應用程式**按鈕，然後再遵循提示來命名和建立新的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e41fe-153">Choose the **Create New App** button, and then follow the prompts to name and create the new application.</span></span>
3. <span data-ttu-id="e41fe-154">一節中**選取與 Facebook 整合您的應用程式將會如何**，選擇**網站**> 一節。</span><span class="sxs-lookup"><span data-stu-id="e41fe-154">In the section **Select how your app will integrate with Facebook**, choose the **Website** section.</span></span>
4. <span data-ttu-id="e41fe-155">填寫**網站 URL**欄位與您的網站 URL (例如， `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="e41fe-155">Fill in the **Site URL** field with the URL of your site (for example, `http://www.example.com`).</span></span> <span data-ttu-id="e41fe-156">**網域**欄位是選擇性的; 您可以使用此提供驗證整個網域 (例如*example.com*)。</span><span class="sxs-lookup"><span data-stu-id="e41fe-156">The **Domain** field is optional; you can use this to provide authentication for an entire domain (such as *example.com*).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e41fe-157">如果您正在執行網站的 url 在本機電腦上要`http://localhost:12345`（其中數目是本機連接埠號碼），您可以將此值以**網站 URL**欄位以供測試您的網站。</span><span class="sxs-lookup"><span data-stu-id="e41fe-157">If you are running a site on your local computer with a URL like `http://localhost:12345` (where the number is a local port number), you can add this value to the **Site URL** field for testing your site.</span></span> <span data-ttu-id="e41fe-158">不過，任何時候您本機站台的變更的通訊埠編號，您將需要更新**網站 URL**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="e41fe-158">However, any time the port number of your local site changes, you will need to update the **Site URL** field of your application.</span></span>
5. <span data-ttu-id="e41fe-159">選擇**儲存變更** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-159">Choose the **Save Changes** button.</span></span>
6. <span data-ttu-id="e41fe-160">選擇**應用程式**同樣地，索引標籤，然後再檢視應用程式的 [開始] 頁面。</span><span class="sxs-lookup"><span data-stu-id="e41fe-160">Choose the **Apps** tab again, and then view the start page for your application.</span></span>
7. <span data-ttu-id="e41fe-161">複製**應用程式識別碼**和**應用程式秘鑰**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="e41fe-161">Copy the **App ID** and **App Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="e41fe-162">在網站上的程式碼中，您將會 Facebook 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="e41fe-162">You will pass these values to the Facebook provider in your website code.</span></span>
8. <span data-ttu-id="e41fe-163">結束 Facebook 開發人員網站。</span><span class="sxs-lookup"><span data-stu-id="e41fe-163">Exit the Facebook developer site.</span></span>

<span data-ttu-id="e41fe-164">現在您變更兩個頁面中您的網站，讓使用者將能夠登入使用其 Facebook 帳戶的站台。</span><span class="sxs-lookup"><span data-stu-id="e41fe-164">Now you make changes to two pages in your website so that users will able to log into the site using their Facebook accounts.</span></span>

1. <span data-ttu-id="e41fe-165">建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。</span><span class="sxs-lookup"><span data-stu-id="e41fe-165">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="e41fe-166">開啟 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Facebook OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="e41fe-166">Open the *\_AppStart.cshtml* page and uncomment the code for the Facebook OAuth provider.</span></span> <span data-ttu-id="e41fe-167">取消註解的程式碼區塊看起來如下：</span><span class="sxs-lookup"><span data-stu-id="e41fe-167">The uncommented code block looks like the following:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample2.cs)]
3. <span data-ttu-id="e41fe-168">複製**應用程式識別碼**從 Facebook 應用程式做為值的值`appId`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="e41fe-168">Copy the **App ID** value from the Facebook application as the value of the `appId` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="e41fe-169">複製**應用程式秘鑰**從 Facebook 應用程式做為值`appSecret`參數值。</span><span class="sxs-lookup"><span data-stu-id="e41fe-169">Copy **App Secret** value from the Facebook application as the `appSecret` parameter value.</span></span>
5. <span data-ttu-id="e41fe-170">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e41fe-170">Save and close the file.</span></span>

### <a name="testing-facebook-login"></a><span data-ttu-id="e41fe-171">測試 Facebook 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-171">Testing Facebook login</span></span>

1. <span data-ttu-id="e41fe-172">執行站台的*default.cshtml*頁面上，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-172">Run the site's *default.cshtml* page and choose the **Login** button.</span></span>
2. <span data-ttu-id="e41fe-173">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Facebook**圖示。</span><span class="sxs-lookup"><span data-stu-id="e41fe-173">On the *Login* page, in the **Use another service to log in** section, choose the **Facebook** icon.</span></span> 

    <span data-ttu-id="e41fe-174">網頁上將要求重新導向至 Facebook 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e41fe-174">The web page redirects the request to the Facebook login page.</span></span>

    ![oauth 2](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image4.png)
3. <span data-ttu-id="e41fe-176">登入 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="e41fe-176">Log into a Facebook account.</span></span> 

    <span data-ttu-id="e41fe-177">程式碼來驗證您使用 Facebook 語彙基元，然後傳回頁面您可以在其中將與您的網站登入您的 Facebook 登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-177">The code uses the Facebook token to authenticate you and then returns to a page where you can associate your Facebook login with your site's login.</span></span> <span data-ttu-id="e41fe-178">使用者名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="e41fe-178">Your user name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image5.png)
4. <span data-ttu-id="e41fe-180">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-180">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="e41fe-181">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-181">The browser returns to the home page and you are logged in.</span></span>

<a id="To_enable_Twitter_logins"></a>
## <a name="enabling-twitter-logins"></a><span data-ttu-id="e41fe-182">啟用 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-182">Enabling Twitter Logins</span></span>

1. <span data-ttu-id="e41fe-183">瀏覽至[Twitter 開發人員網站](https://dev.twitter.com/)。</span><span class="sxs-lookup"><span data-stu-id="e41fe-183">Browse to the [Twitter developers site](https://dev.twitter.com/).</span></span>
2. <span data-ttu-id="e41fe-184">選擇**建立應用程式**連結，然後再登入網站。</span><span class="sxs-lookup"><span data-stu-id="e41fe-184">Choose the **Create an App** link and then log into the site.</span></span>
3. <span data-ttu-id="e41fe-185">在**建立應用程式**表單中，填寫**名稱**和**描述**欄位。</span><span class="sxs-lookup"><span data-stu-id="e41fe-185">On the **Create an Application** form, fill in the **Name** and **Description** fields.</span></span>
4. <span data-ttu-id="e41fe-186">在**網站**欄位中，輸入您網站的 URL (例如， `http://www.example.com`)。</span><span class="sxs-lookup"><span data-stu-id="e41fe-186">In the **WebSite** field, enter the URL of your site (for example, `http://www.example.com`).</span></span> 

    > [!NOTE]
    > <span data-ttu-id="e41fe-187">如果您要測試您的網站，在本機 (使用的 URL，例如`http://localhost:12345`)，Twitter 可能無法接受 URL。</span><span class="sxs-lookup"><span data-stu-id="e41fe-187">If you're testing your site locally (using a URL like `http://localhost:12345`), Twitter might not accept the URL.</span></span> <span data-ttu-id="e41fe-188">不過，您可以使用本機回送 IP 位址 (例如`http://127.0.0.1:12345`)。</span><span class="sxs-lookup"><span data-stu-id="e41fe-188">However, you might be able to use the local loopback IP address (for example `http://127.0.0.1:12345`).</span></span> <span data-ttu-id="e41fe-189">這樣可簡化測試您的應用程式在本機的程序。</span><span class="sxs-lookup"><span data-stu-id="e41fe-189">This simplifies the process of testing your application locally.</span></span> <span data-ttu-id="e41fe-190">不過，每次變更您的本機網站的通訊埠編號，您必須先更新**網站**應用程式的欄位。</span><span class="sxs-lookup"><span data-stu-id="e41fe-190">However, every time the port number of your local site changes, you'll need to update the **WebSite** field of your application.</span></span>
5. <span data-ttu-id="e41fe-191">在**回呼 URL**欄位中，輸入您的網站，您要讓使用者在後返回記錄到 Twitter 中頁面的 URL。</span><span class="sxs-lookup"><span data-stu-id="e41fe-191">In the **Callback URL** field, enter a URL for the page in your website that you want users to return to after logging into Twitter.</span></span> <span data-ttu-id="e41fe-192">例如，要傳送給使用者 （這會將其登入的狀態） 的入門網站的首頁，輸入您在輸入的相同 URL**網站**欄位。</span><span class="sxs-lookup"><span data-stu-id="e41fe-192">For example, to send users to the home page of the Starter Site (which will recognize their logged-in status), enter the same URL that you entered in the **WebSite** field.</span></span>
6. <span data-ttu-id="e41fe-193">接受條款，然後選擇 [**建立應用程式 Twitter** ] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-193">Accept the terms and choose the **Create your Twitter application** button.</span></span>
7. <span data-ttu-id="e41fe-194">在**我的應用程式**登陸頁面上，選擇您所建立的應用程式。</span><span class="sxs-lookup"><span data-stu-id="e41fe-194">On the **My Applications** landing page, choose the application you created.</span></span>
8. <span data-ttu-id="e41fe-195">在**詳細資料**索引標籤上，捲動到底部，然後選擇**建立我的存取權杖** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-195">On the **Details** tab, scroll to the bottom and choose the **Create My Access Token** button.</span></span>
9. <span data-ttu-id="e41fe-196">上**詳細資料**索引標籤上，複製**取用者索引鍵**和**取用者秘密**應用程式的值並貼到暫時的文字檔。</span><span class="sxs-lookup"><span data-stu-id="e41fe-196">On the **Details** tab, copy the **Consumer Key** and **Consumer Secret** values for your application and paste them into a temporary text file.</span></span> <span data-ttu-id="e41fe-197">在網站上的程式碼中，您將 Twitter 提供者來傳遞這些值。</span><span class="sxs-lookup"><span data-stu-id="e41fe-197">You'll pass these values to the Twitter provider in your website code.</span></span>
10. <span data-ttu-id="e41fe-198">結束 Twitter 站台。</span><span class="sxs-lookup"><span data-stu-id="e41fe-198">Exit the Twitter site.</span></span>

<span data-ttu-id="e41fe-199">現在您變更兩個頁面中您的網站，讓使用者能夠登入使用 Twitter 帳戶的網站。</span><span class="sxs-lookup"><span data-stu-id="e41fe-199">Now you make changes to two pages in your website so that users will be able to log into the site using their Twitter accounts.</span></span>

1. <span data-ttu-id="e41fe-200">建立或開啟 WebMatrix 入門網站範本為基礎的 ASP.NET Web Pages 站台。</span><span class="sxs-lookup"><span data-stu-id="e41fe-200">Create or open an ASP.NET Web Pages site that's based on the WebMatrix Starter Site template.</span></span>
2. <span data-ttu-id="e41fe-201">開啟 *\_AppStart.cshtml*頁面上，並取消程式碼註解 Twitter OAuth 提供者。</span><span class="sxs-lookup"><span data-stu-id="e41fe-201">Open the *\_AppStart.cshtml* page and uncomment the code for the Twitter OAuth provider.</span></span> <span data-ttu-id="e41fe-202">取消註解的程式碼區塊看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="e41fe-202">The uncommented code block looks like this:</span></span> 

    [!code-csharp[Main](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/samples/sample3.cs)]
3. <span data-ttu-id="e41fe-203">複製**取用者索引鍵**從 Twitter 應用程式做為值的值`consumerKey`（引號） 內的參數。</span><span class="sxs-lookup"><span data-stu-id="e41fe-203">Copy the **Consumer Key** value from the Twitter application as the value of the `consumerKey` parameter (inside the quotation marks).</span></span>
4. <span data-ttu-id="e41fe-204">複製**取用者秘密**從 Twitter 應用程式做為值的值`consumerSecret`參數。</span><span class="sxs-lookup"><span data-stu-id="e41fe-204">Copy the **Consumer Secret** value from the Twitter application as the value of the `consumerSecret` parameter.</span></span>
5. <span data-ttu-id="e41fe-205">儲存並關閉檔案。</span><span class="sxs-lookup"><span data-stu-id="e41fe-205">Save and close the file.</span></span>

### <a name="testing-twitter-login"></a><span data-ttu-id="e41fe-206">測試 Twitter 登入</span><span class="sxs-lookup"><span data-stu-id="e41fe-206">Testing Twitter login</span></span>

1. <span data-ttu-id="e41fe-207">執行*default.cshtml*網站頁面，然後選擇 [**登入**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-207">Run the *default.cshtml* page of your site and choose the **Login** button.</span></span>
2. <span data-ttu-id="e41fe-208">在*登入*頁面上，於**使用另一個服務登入**區段中，選擇**Twitter**圖示。</span><span class="sxs-lookup"><span data-stu-id="e41fe-208">On the *Login* page, in the **Use another service to log in** section, choose the **Twitter** icon.</span></span> 

    <span data-ttu-id="e41fe-209">網頁的要求重新導向至您所建立的應用程式的 Twitter 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e41fe-209">The web page redirects the request to a Twitter login page for the application you created.</span></span>

    ![oauth-4](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image6.png)
3. <span data-ttu-id="e41fe-211">登入 Twitter 帳戶。</span><span class="sxs-lookup"><span data-stu-id="e41fe-211">Log into a Twitter account.</span></span>
4. <span data-ttu-id="e41fe-212">程式碼來驗證使用者使用 Twitter 語彙基元，然後傳回您的頁面您可在關聯與您網站的帳戶登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-212">The code uses the Twitter token to authenticate the user and then returns you to a page where you can associate your login with your website account.</span></span> <span data-ttu-id="e41fe-213">您的名稱或電子郵件地址填入**電子郵件**欄位在表單上。</span><span class="sxs-lookup"><span data-stu-id="e41fe-213">Your name or email address is filled into the **Email** field on the form.</span></span>

    ![oauth 5](enabling-login-from-external-sites-in-an-aspnet-web-pages-site/_static/image7.png)
5. <span data-ttu-id="e41fe-215">選擇**關聯** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="e41fe-215">Choose the **Associate** button.</span></span> 

    <span data-ttu-id="e41fe-216">瀏覽器會返回 [首頁] 頁面，並在登入。</span><span class="sxs-lookup"><span data-stu-id="e41fe-216">The browser returns to the home page and you are logged in.</span></span>

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="e41fe-217">其他資源</span><span class="sxs-lookup"><span data-stu-id="e41fe-217">Additional Resources</span></span>


- [<span data-ttu-id="e41fe-218">自訂全站台的行為</span><span class="sxs-lookup"><span data-stu-id="e41fe-218">Customizing Site-Wide Behavior</span></span>](https://go.microsoft.com/fwlink/?LinkId=202906)
- [<span data-ttu-id="e41fe-219">加入 ASP.NET Web Pages 站台中的安全性和成員資格</span><span class="sxs-lookup"><span data-stu-id="e41fe-219">Adding Security and Membership to an ASP.NET Web Pages Site</span></span>](https://go.microsoft.com/fwlink/?LinkID=202904)
