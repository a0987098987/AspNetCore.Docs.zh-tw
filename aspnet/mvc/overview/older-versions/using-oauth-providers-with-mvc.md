---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: "OAuth 提供者使用 MVC 4 |Microsoft 文件"
author: tfitzmac
description: "本教學課程會示範如何建立 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebo 認證登入..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/19/2013
ms.topic: article
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: f0d053cecbf9a59f258470ee370852e3f112908c
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="using-oauth-providers-with-mvc-4"></a><span data-ttu-id="89179-103">MVC 4 搭配使用 OAuth 提供者</span><span class="sxs-lookup"><span data-stu-id="89179-103">Using OAuth Providers with MVC 4</span></span>
====================
<span data-ttu-id="89179-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="89179-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="89179-105">本教學課程會示範如何建立 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebook、 Twitter、 Microsoft 或 Google 認證登入，然後整合到那些提供者的功能部分您web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="89179-105">This tutorial shows you how to build an ASP.NET MVC 4 web application that enables users to log in with credentials from an external provider, such as Facebook, Twitter, Microsoft, or Google, and then integrate some of the functionality from those providers into your web application.</span></span> <span data-ttu-id="89179-106">為了簡單起見，本教學課程著重於使用來自 Facebook 的認證。</span><span class="sxs-lookup"><span data-stu-id="89179-106">For simplicity, this tutorial focuses on working with credentials from Facebook.</span></span>
> 
> <span data-ttu-id="89179-107">若要使用 ASP.NET MVC 5 web 應用程式中的外部認證，請參閱[建立 ASP.NET MVC 5 應用程式與 Facebook、 Google OAuth2 和 OpenID 登入](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="89179-107">To use external credentials in an ASP.NET MVC 5 web application, see [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
> 
> <span data-ttu-id="89179-108">啟用瀏覽的網站中的這些認證提供更大的優點，因為已經有數百萬名使用者的帳戶與這些外部提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-108">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="89179-109">這些使用者可能比較想要註冊您的站台如果它們不需要建立並記住一組新的認證。</span><span class="sxs-lookup"><span data-stu-id="89179-109">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span> <span data-ttu-id="89179-110">此外，使用者已透過其中一個提供者登入之後，您可以合併社交提供者的作業。</span><span class="sxs-lookup"><span data-stu-id="89179-110">Also, after a user has logged in through one of these providers, you can incorporate social operations from the provider.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="89179-111">您將建置</span><span class="sxs-lookup"><span data-stu-id="89179-111">What you'll build</span></span>

<span data-ttu-id="89179-112">在本教學課程中有兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="89179-112">There are two main goals in this tutorial:</span></span>

1. <span data-ttu-id="89179-113">讓使用者從 OAuth 提供者的認證登入。</span><span class="sxs-lookup"><span data-stu-id="89179-113">Enable a user to log in with credentials from an OAuth provider.</span></span>
2. <span data-ttu-id="89179-114">從提供者擷取帳戶資訊和整合該資訊進行帳戶註冊您的網站。</span><span class="sxs-lookup"><span data-stu-id="89179-114">Retrieve account information from the provider and integrate that information with the account registration for your site.</span></span>

<span data-ttu-id="89179-115">雖然使用 Facebook 做為驗證提供者專注在本教學課程的範例，您可以修改程式碼以使用任何提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-115">Although the examples in this tutorial focus on using Facebook as the authentication provider, you can modify the code to use any of the providers.</span></span> <span data-ttu-id="89179-116">實作任何提供者的步驟會在本教學課程，您會看到的步驟非常類似。</span><span class="sxs-lookup"><span data-stu-id="89179-116">The steps to implement any provider are very similar to the steps you will see in this tutorial.</span></span> <span data-ttu-id="89179-117">您只會注意到顯著的差異提供者的應用程式開發介面直接呼叫時設定。</span><span class="sxs-lookup"><span data-stu-id="89179-117">You will only notice significant differences when making direct calls to the provider's API set.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="89179-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="89179-118">Prerequisites</span></span>

- <span data-ttu-id="89179-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)或[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span><span class="sxs-lookup"><span data-stu-id="89179-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) or [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span></span>

<span data-ttu-id="89179-120">或</span><span class="sxs-lookup"><span data-stu-id="89179-120">Or</span></span>

- <span data-ttu-id="89179-121">Microsoft Visual Studio 2010 SP1 或[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span><span class="sxs-lookup"><span data-stu-id="89179-121">Microsoft Visual Studio 2010 SP1 or [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span></span>
- [<span data-ttu-id="89179-122">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="89179-122">ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)

<span data-ttu-id="89179-123">此外，本主題假設您有關於 ASP.NET MVC 和 Visual Studio 的基本知識。</span><span class="sxs-lookup"><span data-stu-id="89179-123">Furthermore, this topic assumes you have basic knowledge about ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="89179-124">如果您需要 ASP.NET MVC 4 的簡介，請參閱[簡介 ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="89179-124">If you need an introduction to ASP.NET MVC 4, see [Intro to ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).</span></span>

## <a name="create-the-project"></a><span data-ttu-id="89179-125">建立專案</span><span class="sxs-lookup"><span data-stu-id="89179-125">Create the project</span></span>

<span data-ttu-id="89179-126">在 Visual Studio 中，建立新 ASP.NET MVC 4 Web 應用程式，並將其命名&quot;OAuthMVC&quot;。</span><span class="sxs-lookup"><span data-stu-id="89179-126">In Visual Studio, create a new ASP.NET MVC 4 Web Application, and name it &quot;OAuthMVC&quot;.</span></span> <span data-ttu-id="89179-127">您可以針對.NET Framework 4.5 或 4。</span><span class="sxs-lookup"><span data-stu-id="89179-127">You can target either .NET Framework 4.5 or 4.</span></span>

![建立專案](using-oauth-providers-with-mvc/_static/image1.png)

<span data-ttu-id="89179-129">在新的 ASP.NET MVC 4 專案 視窗中，選取**網際網路應用程式**留下**Razor**做為檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="89179-129">In the New ASP.NET MVC 4 Project window, select **Internet Application** and leave **Razor** as the view engine.</span></span>

![選取 網際網路應用程式](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a><span data-ttu-id="89179-131">啟用的提供者</span><span class="sxs-lookup"><span data-stu-id="89179-131">Enable a provider</span></span>

<span data-ttu-id="89179-132">當您使用網際網路應用程式範本建立的 MVC 4 web 應用程式時，使用名為 AuthConfig.cs 應用程式中的檔案建立專案\_開始資料夾。</span><span class="sxs-lookup"><span data-stu-id="89179-132">When you create an MVC 4 web application with the Internet Application template, the project is created with a file named AuthConfig.cs in the App\_Start folder.</span></span>

![AuthConfig 檔案](using-oauth-providers-with-mvc/_static/image3.png)

<span data-ttu-id="89179-134">AuthConfig 檔案包含要登錄外部驗證提供者的用戶端程式碼。</span><span class="sxs-lookup"><span data-stu-id="89179-134">The AuthConfig file contains code to register clients for external authentication providers.</span></span> <span data-ttu-id="89179-135">根據預設，此程式碼會標記為註解，因此未啟用任何外部提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-135">By default, this code is commented out, so none of the external providers are enabled.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

<span data-ttu-id="89179-136">您必須使用外部驗證用戶端的這段程式碼取消註解。</span><span class="sxs-lookup"><span data-stu-id="89179-136">You must uncomment this code to use the external authentication client.</span></span> <span data-ttu-id="89179-137">請取消註解您想要包含在您的網站中的提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-137">You uncomment only the providers you want to include in your site.</span></span> <span data-ttu-id="89179-138">此教學課程中，您將只能啟用 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="89179-138">For this tutorial, you will only enable the Facebook credentials.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

<span data-ttu-id="89179-139">請注意在上述範例中，這個方法會包含空字串的註冊參數。</span><span class="sxs-lookup"><span data-stu-id="89179-139">Notice in the above example that the method includes empty strings for the registration parameters.</span></span> <span data-ttu-id="89179-140">如果您嘗試執行應用程式現在，應用程式擲回引數例外狀況，因為參數不允許空字串。</span><span class="sxs-lookup"><span data-stu-id="89179-140">If you try to run the application now, the application throws an argument exception because empty strings are not allowed for the parameters.</span></span> <span data-ttu-id="89179-141">若要提供有效的值，您必須使用外部提供者，註冊您的網站下一節中所示。</span><span class="sxs-lookup"><span data-stu-id="89179-141">To provide valid values, you must register your web site with the external providers, as shown in the next section.</span></span>

## <a name="registering-with-an-external-provider"></a><span data-ttu-id="89179-142">使用外部提供者登錄</span><span class="sxs-lookup"><span data-stu-id="89179-142">Registering with an external provider</span></span>

<span data-ttu-id="89179-143">若要驗證從外部提供者的認證的使用者，您必須註冊您的網站與提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-143">To authenticate users with credentials from an external provider, you must register your web site with the provider.</span></span> <span data-ttu-id="89179-144">當您註冊您的網站時，您會收到註冊用戶端時所要包括的參數 （例如索引鍵或識別碼和密碼）。</span><span class="sxs-lookup"><span data-stu-id="89179-144">When you register your site, you will receive the parameters (such as key or id, and secret) to include when registering the client.</span></span> <span data-ttu-id="89179-145">您必須有您想要使用的提供者的帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-145">You must have an account with the providers you wish to use.</span></span>

<span data-ttu-id="89179-146">本教學課程不會顯示所有使用這些提供者註冊，必須執行的步驟。</span><span class="sxs-lookup"><span data-stu-id="89179-146">This tutorial does not show all of the steps you must perform to register with these providers.</span></span> <span data-ttu-id="89179-147">步驟通常是不困難。</span><span class="sxs-lookup"><span data-stu-id="89179-147">The steps are typically not difficult.</span></span> <span data-ttu-id="89179-148">若要成功註冊您的網站，請遵循這些站台所提供的指示。</span><span class="sxs-lookup"><span data-stu-id="89179-148">To successfully register your site, follow the instructions provided on those sites.</span></span> <span data-ttu-id="89179-149">若要開始使用註冊您的網站，請參閱的開發人員網站：</span><span class="sxs-lookup"><span data-stu-id="89179-149">To get started with registering your site, see the developer site for:</span></span>

- [<span data-ttu-id="89179-150">Facebook</span><span class="sxs-lookup"><span data-stu-id="89179-150">Facebook</span></span>](https://developers.facebook.com/)
- [<span data-ttu-id="89179-151">Google</span><span class="sxs-lookup"><span data-stu-id="89179-151">Google</span></span>](https://developers.google.com/)
- [<span data-ttu-id="89179-152">Microsoft</span><span class="sxs-lookup"><span data-stu-id="89179-152">Microsoft</span></span>](http://manage.dev.live.com/)
- [<span data-ttu-id="89179-153">Twitter</span><span class="sxs-lookup"><span data-stu-id="89179-153">Twitter</span></span>](https://dev.twitter.com/)

<span data-ttu-id="89179-154">當註冊您的網站與 Facebook，您可以提供&quot;localhost&quot;站台網域和`&quot;http://localhost/&quot;`對於 URL，請在下圖所示。</span><span class="sxs-lookup"><span data-stu-id="89179-154">When registering your site with Facebook, you can provide &quot;localhost&quot; for the site domain and `&quot;http://localhost/&quot;` for the URL, as shown in the image below.</span></span> <span data-ttu-id="89179-155">使用 localhost 適用於大部分的提供者，但目前無法搭配 Microsoft 提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-155">Using localhost works with most providers, but currently does not work with the Microsoft provider.</span></span> <span data-ttu-id="89179-156">Microsoft 提供者，您必須包含有效的網站 URL。</span><span class="sxs-lookup"><span data-stu-id="89179-156">For the Microsoft provider, you must include a valid web site URL.</span></span>

![註冊站台](using-oauth-providers-with-mvc/_static/image4.png)

<span data-ttu-id="89179-158">在上圖中，已移除應用程式識別碼、 應用程式密碼及連絡人的電子郵件的值。</span><span class="sxs-lookup"><span data-stu-id="89179-158">In the previous image, the values for the app id, app secret, and contact email have been removed.</span></span> <span data-ttu-id="89179-159">當您實際註冊您的網站時，這些值將會出現。</span><span class="sxs-lookup"><span data-stu-id="89179-159">When you actually register your site, those values will be present.</span></span> <span data-ttu-id="89179-160">您會想要因為您將會加入您的應用程式，請注意應用程式識別碼和應用程式密碼的值。</span><span class="sxs-lookup"><span data-stu-id="89179-160">You will want to note the values for app id and app secret because you will add them to your application.</span></span>

## <a name="creating-test-users"></a><span data-ttu-id="89179-161">建立測試使用者</span><span class="sxs-lookup"><span data-stu-id="89179-161">Creating test users</span></span>

<span data-ttu-id="89179-162">如果您不介意來測試您的網站使用現有的 Facebook 帳戶，您可以略過本節。</span><span class="sxs-lookup"><span data-stu-id="89179-162">If you do not mind using an existing Facebook account to test your site, you can skip this section.</span></span>

<span data-ttu-id="89179-163">您的應用程式內的 Facebook 應用程式管理頁面中，您可以輕鬆建立測試使用者。</span><span class="sxs-lookup"><span data-stu-id="89179-163">You can easily create test users for your application within the Facebook app management page.</span></span> <span data-ttu-id="89179-164">您可以使用這些測試帳戶來登入您的網站。</span><span class="sxs-lookup"><span data-stu-id="89179-164">You can use these test accounts to log in to your site.</span></span> <span data-ttu-id="89179-165">按一下 建立測試使用者**角色**左瀏覽窗格中按一下連結**建立**連結。</span><span class="sxs-lookup"><span data-stu-id="89179-165">You create test users by clicking the **Roles** link in the left navigation pane and the clicking the **Create** link.</span></span>

![建立測試使用者](using-oauth-providers-with-mvc/_static/image5.png)

<span data-ttu-id="89179-167">Facebook 站台會自動建立測試帳戶，您所要求的數目。</span><span class="sxs-lookup"><span data-stu-id="89179-167">The Facebook site automatically creates the number of test accounts that you request.</span></span>

## <a name="adding-application-id-and-secret-from-the-provider"></a><span data-ttu-id="89179-168">新增應用程式識別碼和密碼，從提供者</span><span class="sxs-lookup"><span data-stu-id="89179-168">Adding application id and secret from the provider</span></span>

<span data-ttu-id="89179-169">既然您已收到識別碼和密碼，來自 Facebook，返回 AuthConfig 檔案，並將它們加入做為參數值。</span><span class="sxs-lookup"><span data-stu-id="89179-169">Now that you have received the id and secret from Facebook, go back to the AuthConfig file and add them as the parameter values.</span></span> <span data-ttu-id="89179-170">如下所示的值不是實際的值。</span><span class="sxs-lookup"><span data-stu-id="89179-170">The values shown below are not real values.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a><span data-ttu-id="89179-171">外部認證登入</span><span class="sxs-lookup"><span data-stu-id="89179-171">Log in with external credentials</span></span>

<span data-ttu-id="89179-172">這就是您必須啟用您的網站中的外部認證執行。</span><span class="sxs-lookup"><span data-stu-id="89179-172">That is all you have to do to enable external credentials in your site.</span></span> <span data-ttu-id="89179-173">執行您的應用程式，然後按一下右上角的登入連結。</span><span class="sxs-lookup"><span data-stu-id="89179-173">Run your application and click the login link in the upper right corner.</span></span> <span data-ttu-id="89179-174">範本會自動識別您已註冊 Facebook 做為提供者，並包含提供者 按鈕。</span><span class="sxs-lookup"><span data-stu-id="89179-174">The template automatically recognizes that you have registered Facebook as a provider and includes a button for the provider.</span></span> <span data-ttu-id="89179-175">如果您註冊多個提供者，針對每個按鈕會自動包含。</span><span class="sxs-lookup"><span data-stu-id="89179-175">If you register multiple providers, a button for each one is automatically included.</span></span>

![外部登入](using-oauth-providers-with-mvc/_static/image6.png)

<span data-ttu-id="89179-177">本教學課程並未涵蓋如何自訂登入按鈕的外部提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-177">This tutorial does not cover how to customize the log in buttons for the external providers.</span></span> <span data-ttu-id="89179-178">如需該資訊，請參閱[自訂登入 UI，使用 OAuth/OpenID 時](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)。</span><span class="sxs-lookup"><span data-stu-id="89179-178">For that information, see [Customizing the login UI when using OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).</span></span>

<span data-ttu-id="89179-179">按一下 [Facebook] 按鈕以 Facebook 認證登入。</span><span class="sxs-lookup"><span data-stu-id="89179-179">Click on the Facebook button to log in with Facebook credentials.</span></span> <span data-ttu-id="89179-180">當您選取其中一個外部提供者時，您會重新導向至該網站，而且該服務提示您登入。</span><span class="sxs-lookup"><span data-stu-id="89179-180">When you select one of the external providers, you are redirected to that site and prompted by that service to log in.</span></span>

<span data-ttu-id="89179-181">下圖顯示 Facebook 登入畫面。</span><span class="sxs-lookup"><span data-stu-id="89179-181">The following image shows the login screen for Facebook.</span></span> <span data-ttu-id="89179-182">它會註明您使用您的 Facebook 帳戶登入名為 oauthmvcexample 網站。</span><span class="sxs-lookup"><span data-stu-id="89179-182">It notes that you are using your Facebook account to log in to a site named oauthmvcexample.</span></span>

![facebook 驗證](using-oauth-providers-with-mvc/_static/image7.png)

<span data-ttu-id="89179-184">登入 Facebook 認證之後，頁面會通知使用者站台，將會擁有存取權的基本資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-184">After logging in with Facebook credentials, a page informs the user that the site will have access to basic information.</span></span>

![要求權限](using-oauth-providers-with-mvc/_static/image8.png)

<span data-ttu-id="89179-186">選取之後**前往應用程式**，使用者必須註冊站台。</span><span class="sxs-lookup"><span data-stu-id="89179-186">After selecting **Go to App**, the user must register for the site.</span></span> <span data-ttu-id="89179-187">使用者已透過 Facebook 認證登入之後下, 圖顯示 [註冊] 頁面。</span><span class="sxs-lookup"><span data-stu-id="89179-187">The following image shows the registration page after a user has logged in with Facebook credentials.</span></span> <span data-ttu-id="89179-188">使用者名稱是以從提供者名稱通常預先填入。</span><span class="sxs-lookup"><span data-stu-id="89179-188">The user name is typically pre-filled with a name from the provider.</span></span>

![註冊](using-oauth-providers-with-mvc/_static/image9.png)

<span data-ttu-id="89179-190">按一下**註冊**以完成註冊。</span><span class="sxs-lookup"><span data-stu-id="89179-190">Click **Register** to complete registration.</span></span> <span data-ttu-id="89179-191">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="89179-191">Close the browser.</span></span>

<span data-ttu-id="89179-192">您可以看到您的資料庫中已加入新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-192">You can see that the new account has been added to your database.</span></span> <span data-ttu-id="89179-193">在 [伺服器總管] 中，開啟**DefaultConnection**資料庫，然後開啟**資料表**資料夾。</span><span class="sxs-lookup"><span data-stu-id="89179-193">In Server Explorer, open the **DefaultConnection** database and open the **Tables** folder.</span></span>

![資料庫資料表](using-oauth-providers-with-mvc/_static/image10.png)

<span data-ttu-id="89179-195">以滑鼠右鍵按一下**UserProfile**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="89179-195">Right-click the **UserProfile** table and select **Show Table Data**.</span></span>

![顯示資料](using-oauth-providers-with-mvc/_static/image11.png)

<span data-ttu-id="89179-197">您會看到您加入新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-197">You will see the new account you added.</span></span> <span data-ttu-id="89179-198">查看中的資料**網頁\_OAuthMembership**資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-198">Look at the data in **webpage\_OAuthMembership** table.</span></span> <span data-ttu-id="89179-199">您會看到您剛才加入的帳戶的外部提供者相關的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="89179-199">You will see more data related to the external provider for the account you just added.</span></span>

<span data-ttu-id="89179-200">如果您只想要啟用外部驗證，您已完成。</span><span class="sxs-lookup"><span data-stu-id="89179-200">If you only want to enable external authentication, you are done.</span></span> <span data-ttu-id="89179-201">不過，您可以進一步整合提供者資訊到新的使用者註冊程序，如下列各節中所示。</span><span class="sxs-lookup"><span data-stu-id="89179-201">However, you can further integrate information from the provider into the new user registration process, as shown in the following sections.</span></span>

## <a name="create-models-for-additional-user-information"></a><span data-ttu-id="89179-202">建立模型以供其他使用者資訊</span><span class="sxs-lookup"><span data-stu-id="89179-202">Create models for additional user information</span></span>

<span data-ttu-id="89179-203">您注意到上一節，您不需要擷取要處理的內建帳戶註冊的任何其他資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-203">As you noticed in the previous sections, you do not need to retrieve any additional information for the built-in account registration to work.</span></span> <span data-ttu-id="89179-204">不過，最外部提供者傳遞使用者的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-204">However, most external providers pass back additional information about the user.</span></span> <span data-ttu-id="89179-205">下列各節顯示如何以保留該資訊，並將它儲存至資料庫。</span><span class="sxs-lookup"><span data-stu-id="89179-205">The following sections show how to retain that information and save it to a database.</span></span> <span data-ttu-id="89179-206">具體來說，您將會保留使用者的完整名稱，使用者的個人網頁，URI 的值和值，指出是否 Facebook 已驗證帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-206">Specifically, you will retain values for the user's full name, the URI of the user's personal web page, and a value that indicates whether Facebook has verified the account.</span></span>

<span data-ttu-id="89179-207">您將使用[Code First 移轉](https://msdn.microsoft.com/data/jj591621)加入儲存額外的使用者資訊的資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-207">You will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) to add a table for storing additional user information.</span></span> <span data-ttu-id="89179-208">因此第一次您必須建立在目前資料庫的快照集，您會加入至現有的資料庫資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-208">You are adding the table to an existing database, so you will first need to create a snapshot of the current database.</span></span> <span data-ttu-id="89179-209">藉由建立目前資料庫的快照集，您稍後可以建立包含新的資料表移轉。</span><span class="sxs-lookup"><span data-stu-id="89179-209">By creating a snapshot of the current database, you can later create a migration that contains only the new table.</span></span> <span data-ttu-id="89179-210">若要建立目前資料庫的快照集：</span><span class="sxs-lookup"><span data-stu-id="89179-210">To create a snapshot of the current database:</span></span>

1. <span data-ttu-id="89179-211">開啟**Package Manager Console**</span><span class="sxs-lookup"><span data-stu-id="89179-211">Open the **Package Manager Console**</span></span>
2. <span data-ttu-id="89179-212">執行命令**啟用移轉**</span><span class="sxs-lookup"><span data-stu-id="89179-212">Run the command **enable-migrations**</span></span>
3. <span data-ttu-id="89179-213">執行命令**IgnoreChanges 新增移轉初始 –**</span><span class="sxs-lookup"><span data-stu-id="89179-213">Run the command **add-migration initial –IgnoreChanges**</span></span>
4. <span data-ttu-id="89179-214">執行命令**更新資料庫**</span><span class="sxs-lookup"><span data-stu-id="89179-214">Run the command **update-database**</span></span>

<span data-ttu-id="89179-215">現在，您將加入新的屬性。</span><span class="sxs-lookup"><span data-stu-id="89179-215">Now, you will add the new properties.</span></span> <span data-ttu-id="89179-216">在 [模型] 資料夾中，開啟 AccountModels.cs 檔案，並尋找 RegisterExternalLoginModel 類別。</span><span class="sxs-lookup"><span data-stu-id="89179-216">In the Models folder, open the AccountModels.cs file, and find the RegisterExternalLoginModel class.</span></span> <span data-ttu-id="89179-217">RegisterExternalLoginModel 類別保留來自驗證提供者的值。</span><span class="sxs-lookup"><span data-stu-id="89179-217">The RegisterExternalLoginModel class holds values that come back from the authentication provider.</span></span> <span data-ttu-id="89179-218">加入以反白顯示下面名為 FullName] 和 [連結的屬性。</span><span class="sxs-lookup"><span data-stu-id="89179-218">Add properties named FullName and Link, as highlighted below.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

<span data-ttu-id="89179-219">也在 AccountModels.cs，加入新的類別稱為 ExtraUserInformation。</span><span class="sxs-lookup"><span data-stu-id="89179-219">Also in AccountModels.cs, add a new class called ExtraUserInformation.</span></span> <span data-ttu-id="89179-220">此類別代表會在資料庫中建立的新資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-220">This class represents the new table which will be created in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

<span data-ttu-id="89179-221">UsersContext 類別中加入反白顯示下列程式碼以建立新類別 DbSet 屬性。</span><span class="sxs-lookup"><span data-stu-id="89179-221">In the UsersContext class, add the highlighted code below to create a DbSet property for the new class.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

<span data-ttu-id="89179-222">您現在準備建立新的資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-222">You are now ready to create the new table.</span></span> <span data-ttu-id="89179-223">開啟 Package Manager Console 再次和此時間：</span><span class="sxs-lookup"><span data-stu-id="89179-223">Open the Package Manager Console again and this time:</span></span>

1. <span data-ttu-id="89179-224">執行命令**新增移轉 AddExtraUserInformation**</span><span class="sxs-lookup"><span data-stu-id="89179-224">Run the command **add-migration AddExtraUserInformation**</span></span>
2. <span data-ttu-id="89179-225">執行命令**更新資料庫**</span><span class="sxs-lookup"><span data-stu-id="89179-225">Run the command **update-database**</span></span>

<span data-ttu-id="89179-226">資料庫中現在有新的資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-226">The new table now exists in the database.</span></span>

## <a name="retrieve-the-additional-data"></a><span data-ttu-id="89179-227">擷取其他資料</span><span class="sxs-lookup"><span data-stu-id="89179-227">Retrieve the additional data</span></span>

<span data-ttu-id="89179-228">有兩種方式，以擷取其他的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="89179-228">There are two ways to retrieve additional user data.</span></span> <span data-ttu-id="89179-229">第一種方式是在保留期間所傳入，根據預設，驗證要求的使用者資料。</span><span class="sxs-lookup"><span data-stu-id="89179-229">The first way is to retain user data that is passed back, by default, during the authentication request.</span></span> <span data-ttu-id="89179-230">第二種方式是特別呼叫提供者應用程式開發介面，並要求的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-230">The second way is to specifically call the provider API and request more information.</span></span> <span data-ttu-id="89179-231">FullName 和連結的值會自動傳遞回由 Facebook。</span><span class="sxs-lookup"><span data-stu-id="89179-231">Values for FullName and Link are automatically passed back by Facebook.</span></span> <span data-ttu-id="89179-232">透過對 Facebook API 呼叫擷取值，指出是否 Facebook 已驗證帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-232">A value that indicates whether Facebook has verified the account is retrieved through a call to the Facebook API.</span></span> <span data-ttu-id="89179-233">首先，您將會填入值 FullName 和連結，並將更新版本中，會取得已驗證的值。</span><span class="sxs-lookup"><span data-stu-id="89179-233">First, you will populate values for FullName and Link, and then later, you will get the verified value.</span></span>

<span data-ttu-id="89179-234">若要擷取的其他使用者資料，請開啟**AccountController.cs**檔案**控制器**資料夾。</span><span class="sxs-lookup"><span data-stu-id="89179-234">To retrieve the additional user data, open the **AccountController.cs** file in the **Controllers** folder.</span></span>

<span data-ttu-id="89179-235">此檔案包含的邏輯記錄、 註冊和管理帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-235">This file contains the logic for logging, registering, and managing accounts.</span></span> <span data-ttu-id="89179-236">特別是，請注意呼叫的方法**ExternalLoginCallback**和**ExternalLoginConfirmation**。</span><span class="sxs-lookup"><span data-stu-id="89179-236">In particular, notice the methods called **ExternalLoginCallback** and **ExternalLoginConfirmation**.</span></span> <span data-ttu-id="89179-237">在這些方法中，您可以加入程式碼以自訂您的應用程式的外部登入作業。</span><span class="sxs-lookup"><span data-stu-id="89179-237">Within these methods, you can add code to customize external login operations for your application.</span></span> <span data-ttu-id="89179-238">第一行**ExternalLoginCallback**方法包含：</span><span class="sxs-lookup"><span data-stu-id="89179-238">The first line of the **ExternalLoginCallback** method contains:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

<span data-ttu-id="89179-239">額外的使用者資料會傳遞回**ExtraData**屬性**AuthenticationResult**從傳回的物件**VerifyAuthentication**方法。</span><span class="sxs-lookup"><span data-stu-id="89179-239">Additional user data is passed back in the **ExtraData** property of the **AuthenticationResult** object that is returned from the **VerifyAuthentication** method.</span></span> <span data-ttu-id="89179-240">Facebook 用戶端包含下列中的值**ExtraData**屬性：</span><span class="sxs-lookup"><span data-stu-id="89179-240">The Facebook client contains the following values in the **ExtraData** property:</span></span>

- <span data-ttu-id="89179-241">id</span><span class="sxs-lookup"><span data-stu-id="89179-241">id</span></span>
- <span data-ttu-id="89179-242">name</span><span class="sxs-lookup"><span data-stu-id="89179-242">name</span></span>
- <span data-ttu-id="89179-243">連結</span><span class="sxs-lookup"><span data-stu-id="89179-243">link</span></span>
- <span data-ttu-id="89179-244">性別</span><span class="sxs-lookup"><span data-stu-id="89179-244">gender</span></span>
- <span data-ttu-id="89179-245">accesstoken</span><span class="sxs-lookup"><span data-stu-id="89179-245">accesstoken</span></span>

<span data-ttu-id="89179-246">其他提供者將 ExtraData 屬性中有相似但稍微不同的資料。</span><span class="sxs-lookup"><span data-stu-id="89179-246">Other providers will have similar but slightly different data in the ExtraData property.</span></span>

<span data-ttu-id="89179-247">如果使用者是您的站台的新手，您會擷取一些額外的資料，並將資料傳遞到 [確認] 檢視。</span><span class="sxs-lookup"><span data-stu-id="89179-247">If the user is new to your site, you will retrieve some the additional data and pass that data to the confirmation view.</span></span> <span data-ttu-id="89179-248">只有當使用者是您的站台的新執行方法中的程式碼的最後一個區塊。</span><span class="sxs-lookup"><span data-stu-id="89179-248">The last block of code in the method is run only if the user is new to your site.</span></span> <span data-ttu-id="89179-249">取代為下列行：</span><span class="sxs-lookup"><span data-stu-id="89179-249">Replace the following line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

<span data-ttu-id="89179-250">以這行：</span><span class="sxs-lookup"><span data-stu-id="89179-250">With this line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

<span data-ttu-id="89179-251">這項變更只會包含 FullName 和 連結屬性的值。</span><span class="sxs-lookup"><span data-stu-id="89179-251">This change merely includes values for the FullName and Link properties.</span></span>

<span data-ttu-id="89179-252">在**ExternalLoginConfirmation**方法，修改下列程式碼以反白顯示，儲存額外的使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-252">In the **ExternalLoginConfirmation** method, modify the code as highlighted below to save the additional user information.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a><span data-ttu-id="89179-253">調整檢視</span><span class="sxs-lookup"><span data-stu-id="89179-253">Adjusting the view</span></span>

<span data-ttu-id="89179-254">您從提供者擷取額外的使用者資料將顯示在 [註冊] 頁面中。</span><span class="sxs-lookup"><span data-stu-id="89179-254">The additional user data that you retrieve from the provider will be displayed in the registration page.</span></span>

<span data-ttu-id="89179-255">在**檢視**/**帳戶**資料夾中，開啟**ExternalLoginConfirmation.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="89179-255">In the **Views**/**Account** folder, open **ExternalLoginConfirmation.cshtml**.</span></span> <span data-ttu-id="89179-256">下列使用者名稱與現有欄位，加入 FullName、 連結和 PictureLink 欄位。</span><span class="sxs-lookup"><span data-stu-id="89179-256">Below the existing field for user name, add fields for FullName, Link, and PictureLink.</span></span>

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

<span data-ttu-id="89179-257">現在您已經準備就緒執行應用程式並註冊新的使用者與儲存的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-257">You are now almost ready to run the application and register a new user with the additional information saved.</span></span> <span data-ttu-id="89179-258">您必須具有尚未註冊與站台的帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-258">You must have an account that is not already registered with the site.</span></span> <span data-ttu-id="89179-259">您可以使用不同的測試帳戶，或刪除資料列中的**UserProfile**和**網頁\_OAuthMembership**您想要重複使用之帳戶的資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-259">You can either use a different test account, or delete the rows in the **UserProfile** and **webpages\_OAuthMembership** tables for the account you wish to reuse.</span></span> <span data-ttu-id="89179-260">刪除這些資料列，您要確定此帳戶重新登錄。</span><span class="sxs-lookup"><span data-stu-id="89179-260">By deleting those rows, you will ensure that the account is registered again.</span></span>

<span data-ttu-id="89179-261">執行應用程式，並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="89179-261">Run the application and register the new user.</span></span> <span data-ttu-id="89179-262">請注意，此時確認頁面包含多個值。</span><span class="sxs-lookup"><span data-stu-id="89179-262">Notice that this time the confirmation page contains more values.</span></span>

![註冊](using-oauth-providers-with-mvc/_static/image12.png)

<span data-ttu-id="89179-264">完成之後註冊，請關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="89179-264">After completing registration, close the browser.</span></span> <span data-ttu-id="89179-265">查詢資料庫中的新值會注意到**ExtraUserInformation**資料表。</span><span class="sxs-lookup"><span data-stu-id="89179-265">Look in the database to notice the new values in the **ExtraUserInformation** table.</span></span>

## <a name="install-nuget-package-for-facebook-api"></a><span data-ttu-id="89179-266">安裝 NuGet 套件的 Facebook 應用程式開發介面</span><span class="sxs-lookup"><span data-stu-id="89179-266">Install NuGet package for Facebook API</span></span>

<span data-ttu-id="89179-267">提供 Facebook [API](https://developers.facebook.com/docs/reference/apis/) ，您可以呼叫執行的作業。</span><span class="sxs-lookup"><span data-stu-id="89179-267">Facebook provides an [API](https://developers.facebook.com/docs/reference/apis/) that you can call to perform operations.</span></span> <span data-ttu-id="89179-268">您可以呼叫 Facebook API，將傳送 HTTP 要求，或使用 安裝 NuGet 套件，可促進傳送這些要求。</span><span class="sxs-lookup"><span data-stu-id="89179-268">You can call the Facebook API either by directing sending HTTP requests, or by using installing a NuGet package that facilitates sending those requests.</span></span> <span data-ttu-id="89179-269">使用 NuGet 封裝會顯示在本教學課程中，但安裝 NuGet 套件並不重要。</span><span class="sxs-lookup"><span data-stu-id="89179-269">Using a NuGet package is shown in this tutorial, but installing NuGet package is not essential.</span></span> <span data-ttu-id="89179-270">本教學課程會示範如何使用 Facebook C# SDK 封裝。</span><span class="sxs-lookup"><span data-stu-id="89179-270">This tutorial shows how to use the Facebook C# SDK package.</span></span> <span data-ttu-id="89179-271">另外還有其他協助呼叫 Facebook API 的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="89179-271">There are other NuGet packages that assist with calling the Facebook API.</span></span>

<span data-ttu-id="89179-272">從**管理 NuGet 封裝**windows 中，選取 Facebook C# SDK 封裝。</span><span class="sxs-lookup"><span data-stu-id="89179-272">From the **Manage NuGet Packages** windows, select the Facebook C# SDK package.</span></span>

![安裝套件](using-oauth-providers-with-mvc/_static/image13.png)

<span data-ttu-id="89179-274">您將使用 Facebook C# SDK 呼叫的使用者需要存取權杖的作業。</span><span class="sxs-lookup"><span data-stu-id="89179-274">You will use the Facebook C# SDK to call an operation that requires the access token for the user.</span></span> <span data-ttu-id="89179-275">下一節將示範如何取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="89179-275">The next section shows how to get the access token.</span></span>

## <a name="retrieve-access-token"></a><span data-ttu-id="89179-276">擷取存取權杖</span><span class="sxs-lookup"><span data-stu-id="89179-276">Retrieve access token</span></span>

<span data-ttu-id="89179-277">使用者的認證通過驗證後，最外部提供者傳回存取權杖。</span><span class="sxs-lookup"><span data-stu-id="89179-277">Most external providers pass back an access token after the user's credentials are verified.</span></span> <span data-ttu-id="89179-278">此存取權杖是非常重要，因為它可讓您呼叫作業，只可用於驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="89179-278">This access token is very important because it enables you to call operations that are only available to authenticated users.</span></span> <span data-ttu-id="89179-279">因此，擷取和儲存的存取語彙基元很重要時，您想要提供更多的功能。</span><span class="sxs-lookup"><span data-stu-id="89179-279">Therefore, retrieving and storing the access token is essential when you want to provide more functionality.</span></span>

<span data-ttu-id="89179-280">根據外部提供者，存取權杖只在有限可能是時間的有效的。</span><span class="sxs-lookup"><span data-stu-id="89179-280">Depending on the external provider, the access token may be valid for only a limited amount of time.</span></span> <span data-ttu-id="89179-281">若要確保您擁有的有效存取權杖，您將會擷取每次使用者登入，並將它儲存成工作階段值，而非將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="89179-281">To ensure that you have a valid access token, you will retrieve it each time the user logs in, and store it as a session value rather than save it to a database.</span></span>

<span data-ttu-id="89179-282">在**ExternalLoginCallback**方法，存取語彙基元也會跟著傳遞回**ExtraData**屬性**AuthenticationResult**物件。</span><span class="sxs-lookup"><span data-stu-id="89179-282">In the **ExternalLoginCallback** method, the access token is also passed back in the **ExtraData** property of the **AuthenticationResult** object.</span></span> <span data-ttu-id="89179-283">將反白顯示的程式碼加入**ExternalLoginCallback**儲存存取權杖**工作階段**物件。</span><span class="sxs-lookup"><span data-stu-id="89179-283">Add the highlighted code to **ExternalLoginCallback** to save the access token in the **Session** object.</span></span> <span data-ttu-id="89179-284">每次使用者的 Facebook 帳戶登入時，會執行這段程式碼。</span><span class="sxs-lookup"><span data-stu-id="89179-284">This code is run every time the user logs in with a Facebook account.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

<span data-ttu-id="89179-285">雖然這個範例會擷取來自 Facebook 存取權杖，您可以透過名為相同的索引鍵從任何外部提供者擷取存取權杖&quot;accesstoken&quot;。</span><span class="sxs-lookup"><span data-stu-id="89179-285">Although this example retrieves an access token from Facebook, you can retrieve the access token from any external provider through the same key named &quot;accesstoken&quot;.</span></span>

## <a name="logging-off"></a><span data-ttu-id="89179-286">登出</span><span class="sxs-lookup"><span data-stu-id="89179-286">Logging off</span></span>

<span data-ttu-id="89179-287">預設值**登出**方法使用者登出您的應用程式，但不會記錄使用者登出外部提供者。</span><span class="sxs-lookup"><span data-stu-id="89179-287">The default **LogOff** method logs the user out of your application, but does not log the user out of the external provider.</span></span> <span data-ttu-id="89179-288">若要讓使用者從 Facebook 登並防止保存使用者登出後權杖，您可以加入下列反白顯示的程式碼加入**登出**AccountController 中的方法。</span><span class="sxs-lookup"><span data-stu-id="89179-288">To log the user out of Facebook, and prevent the token from persisting after the user has logged off, you can add the following highlighted code to the **LogOff** method in the AccountController.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

<span data-ttu-id="89179-289">您在中提供的值`next`參數是使用者已登出 Facebook 後使用的 URL。</span><span class="sxs-lookup"><span data-stu-id="89179-289">The value you provide in the `next` parameter is the URL to use after the user has logged out of Facebook.</span></span> <span data-ttu-id="89179-290">測試時在本機電腦上，您會提供正確的連接埠號碼的本機網站。</span><span class="sxs-lookup"><span data-stu-id="89179-290">When testing on your local computer, you would provide the correct port number for your local site.</span></span> <span data-ttu-id="89179-291">在生產環境中，您會提供預設頁面上，例如 contoso.com。</span><span class="sxs-lookup"><span data-stu-id="89179-291">In production, you would provide a default page, such as contoso.com.</span></span>

## <a name="retrieve-user-information-that-requires-the-access-token"></a><span data-ttu-id="89179-292">擷取需要的存取權杖的使用者資訊</span><span class="sxs-lookup"><span data-stu-id="89179-292">Retrieve user information that requires the access token</span></span>

<span data-ttu-id="89179-293">現在，您有儲存的存取權杖，並已安裝的 Facebook C# SDK 套件，您可以一起使用這些要求來自 Facebook 的其他使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="89179-293">Now that you have stored the access token and installed the Facebook C# SDK package, you can use them together to request additional user information from Facebook.</span></span> <span data-ttu-id="89179-294">在**ExternalLoginConfirmation**方法，建立的執行個體**FacebookClient**類別藉由傳遞存取權杖的值。</span><span class="sxs-lookup"><span data-stu-id="89179-294">In the **ExternalLoginConfirmation** method, create an instance of the **FacebookClient** class by passing the value of the access token.</span></span> <span data-ttu-id="89179-295">要求的值**驗證**目前已驗證使用者的屬性。</span><span class="sxs-lookup"><span data-stu-id="89179-295">Request the value of the **verified** property for the current, authenticated user.</span></span> <span data-ttu-id="89179-296">**驗證**屬性會指出是否 Facebook 已驗證的帳戶透過其他方式，例如將訊息傳送到行動電話。</span><span class="sxs-lookup"><span data-stu-id="89179-296">The **verified** property indicates whether Facebook has validated the account through some other means, such as sending a message to a cell phone.</span></span> <span data-ttu-id="89179-297">將此值儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="89179-297">Save this value in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

<span data-ttu-id="89179-298">您一次必須刪除使用者的資料庫中的記錄，或使用不同的 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="89179-298">You will again need to either delete the records in the database for the user, or use a different Facebook account.</span></span>

<span data-ttu-id="89179-299">執行應用程式，並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="89179-299">Run the application, and register the new user.</span></span> <span data-ttu-id="89179-300">查看**ExtraUserInformation**資料表來查看已驗證屬性的值。</span><span class="sxs-lookup"><span data-stu-id="89179-300">Look at the **ExtraUserInformation** table to see the value for the Verified property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="89179-301">結論</span><span class="sxs-lookup"><span data-stu-id="89179-301">Conclusion</span></span>

<span data-ttu-id="89179-302">在此教學課程中，您可以建立網站與 Facebook 整合使用者驗證和註冊資料。</span><span class="sxs-lookup"><span data-stu-id="89179-302">In this tutorial, you created a site that is integrated with Facebook for user authentication and registration data.</span></span> <span data-ttu-id="89179-303">您會學為設定之 MVC 4 web 應用程式，以及如何自訂該預設行為的預設行為。</span><span class="sxs-lookup"><span data-stu-id="89179-303">You learned about the default behavior that is set up for MVC 4 web application, and how to customize that default behavior.</span></span>

## <a name="related-topics"></a><span data-ttu-id="89179-304">相關主題</span><span class="sxs-lookup"><span data-stu-id="89179-304">Related topics</span></span>

- [<span data-ttu-id="89179-305">驗證與 SQL 資料庫中建立 ASP.NET MVC 應用程式並部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="89179-305">Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service</span></span>](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
