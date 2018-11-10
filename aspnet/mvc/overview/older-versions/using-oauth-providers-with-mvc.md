---
uid: mvc/overview/older-versions/using-oauth-providers-with-mvc
title: 使用 OAuth 提供者與 MVC 4 |Microsoft Docs
author: Rick-Anderson
description: 本教學課程會示範如何建置 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebo 的認證來登入...
ms.author: riande
ms.date: 06/19/2013
ms.assetid: 7a87f16f-0e19-4f15-a88a-094ae866c4a2
msc.legacyurl: /mvc/overview/older-versions/using-oauth-providers-with-mvc
msc.type: authoredcontent
ms.openlocfilehash: d0203b62c911056fc56ed103c1c42f67816cbbf0
ms.sourcegitcommit: 2d3e5422d530203efdaf2014d1d7df31f88d08d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/05/2018
ms.locfileid: "51021751"
---
<a name="using-oauth-providers-with-mvc-4"></a><span data-ttu-id="0568d-103">使用 OAuth 提供者與 MVC 4</span><span class="sxs-lookup"><span data-stu-id="0568d-103">Using OAuth Providers with MVC 4</span></span>
====================
<span data-ttu-id="0568d-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="0568d-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="0568d-105">本教學課程會示範如何建置 ASP.NET MVC 4 web 應用程式，可讓使用者從外部提供者，例如 Facebook、 Twitter、 Microsoft 或 Google 的認證登入，並整合的一些功能，這些提供者將從您web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="0568d-105">This tutorial shows you how to build an ASP.NET MVC 4 web application that enables users to log in with credentials from an external provider, such as Facebook, Twitter, Microsoft, or Google, and then integrate some of the functionality from those providers into your web application.</span></span> <span data-ttu-id="0568d-106">為了簡單起見，本教學課程著重於使用來自 Facebook 的認證。</span><span class="sxs-lookup"><span data-stu-id="0568d-106">For simplicity, this tutorial focuses on working with credentials from Facebook.</span></span>
> 
> <span data-ttu-id="0568d-107">若要使用 ASP.NET MVC 5 web 應用程式中的外部認證，請參閱[建立 ASP.NET MVC 5 應用程式使用 Facebook 和 Google OAuth2 和 OpenID 登入](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md)。</span><span class="sxs-lookup"><span data-stu-id="0568d-107">To use external credentials in an ASP.NET MVC 5 web application, see [Create an ASP.NET MVC 5 App with Facebook and Google OAuth2 and OpenID Sign-on](../security/create-an-aspnet-mvc-5-app-with-facebook-and-google-oauth2-and-openid-sign-on.md).</span></span>
> 
> <span data-ttu-id="0568d-108">啟用您的網站中的這些認證提供極大的好處，因為數百萬位使用者已將這些外部提供者的帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-108">Enabling these credentials in your web sites provides a significant advantage because millions of users already have accounts with these external providers.</span></span> <span data-ttu-id="0568d-109">這些使用者可能會更有必要，如果它們不需要建立並記住一組新的認證，登入您的網站。</span><span class="sxs-lookup"><span data-stu-id="0568d-109">These users may be more inclined to sign up for your site if they do not have to create and remember a new set of credentials.</span></span> <span data-ttu-id="0568d-110">此外，使用者已透過其中一個提供者登入之後，您可以將社交提供者的作業。</span><span class="sxs-lookup"><span data-stu-id="0568d-110">Also, after a user has logged in through one of these providers, you can incorporate social operations from the provider.</span></span>


## <a name="what-youll-build"></a><span data-ttu-id="0568d-111">您將建置</span><span class="sxs-lookup"><span data-stu-id="0568d-111">What you'll build</span></span>

<span data-ttu-id="0568d-112">在本教學課程中有兩個主要目標：</span><span class="sxs-lookup"><span data-stu-id="0568d-112">There are two main goals in this tutorial:</span></span>

1. <span data-ttu-id="0568d-113">讓使用者從 OAuth 提供者的認證來登入。</span><span class="sxs-lookup"><span data-stu-id="0568d-113">Enable a user to log in with credentials from an OAuth provider.</span></span>
2. <span data-ttu-id="0568d-114">從提供者擷取帳戶資訊，並整合您的網站的帳戶註冊這項資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-114">Retrieve account information from the provider and integrate that information with the account registration for your site.</span></span>

<span data-ttu-id="0568d-115">雖然本教學課程中的範例著重於使用 Facebook 作為驗證提供者，您可以修改程式碼來使用任何提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-115">Although the examples in this tutorial focus on using Facebook as the authentication provider, you can modify the code to use any of the providers.</span></span> <span data-ttu-id="0568d-116">實作任何提供者的步驟會在本教學課程中，您會看到的步驟非常類似。</span><span class="sxs-lookup"><span data-stu-id="0568d-116">The steps to implement any provider are very similar to the steps you will see in this tutorial.</span></span> <span data-ttu-id="0568d-117">您只會發現顯著差異時直接呼叫提供者的 API 設定。</span><span class="sxs-lookup"><span data-stu-id="0568d-117">You will only notice significant differences when making direct calls to the provider's API set.</span></span>

## <a name="prerequisites"></a><span data-ttu-id="0568d-118">必要條件</span><span class="sxs-lookup"><span data-stu-id="0568d-118">Prerequisites</span></span>

- <span data-ttu-id="0568d-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs)或[Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span><span class="sxs-lookup"><span data-stu-id="0568d-119">[Microsoft Visual Studio 2012](https://www.microsoft.com/visualstudio/eng/downloads#vs) or [Microsoft Visual Studio Express 2012 for Web](https://www.microsoft.com/visualstudio/eng/downloads#d-2012-express)</span></span>

<span data-ttu-id="0568d-120">或</span><span class="sxs-lookup"><span data-stu-id="0568d-120">Or</span></span>

- <span data-ttu-id="0568d-121">Microsoft Visual Studio 2010 SP1 或[Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span><span class="sxs-lookup"><span data-stu-id="0568d-121">Microsoft Visual Studio 2010 SP1 or [Visual Web Developer Express 2010 SP1](https://www.microsoft.com/visualstudio/eng/downloads#d-2010-express)</span></span>
- [<span data-ttu-id="0568d-122">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="0568d-122">ASP.NET MVC 4</span></span>](https://go.microsoft.com/fwlink/?LinkId=243392)

<span data-ttu-id="0568d-123">此外，本主題假設您有基本了解 ASP.NET MVC 和 Visual Studio。</span><span class="sxs-lookup"><span data-stu-id="0568d-123">Furthermore, this topic assumes you have basic knowledge about ASP.NET MVC and Visual Studio.</span></span> <span data-ttu-id="0568d-124">如果您需要 ASP.NET MVC 4 的簡介，請參閱[ASP.NET MVC 4 簡介](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md)。</span><span class="sxs-lookup"><span data-stu-id="0568d-124">If you need an introduction to ASP.NET MVC 4, see [Intro to ASP.NET MVC 4](getting-started-with-aspnet-mvc4/intro-to-aspnet-mvc-4.md).</span></span>

## <a name="create-the-project"></a><span data-ttu-id="0568d-125">建立專案</span><span class="sxs-lookup"><span data-stu-id="0568d-125">Create the project</span></span>

<span data-ttu-id="0568d-126">在 Visual Studio 中建立新 ASP.NET MVC 4 Web 應用程式，並將它命名&quot;OAuthMVC&quot;。</span><span class="sxs-lookup"><span data-stu-id="0568d-126">In Visual Studio, create a new ASP.NET MVC 4 Web Application, and name it &quot;OAuthMVC&quot;.</span></span> <span data-ttu-id="0568d-127">您可以針對.NET Framework 4.5 或 4。</span><span class="sxs-lookup"><span data-stu-id="0568d-127">You can target either .NET Framework 4.5 or 4.</span></span>

![建立專案](using-oauth-providers-with-mvc/_static/image1.png)

<span data-ttu-id="0568d-129">在 [新增 ASP.NET MVC 4 專案] 視窗中，選取**網際網路應用程式**並保留**Razor**做為檢視引擎。</span><span class="sxs-lookup"><span data-stu-id="0568d-129">In the New ASP.NET MVC 4 Project window, select **Internet Application** and leave **Razor** as the view engine.</span></span>

![選取 網際網路應用程式](using-oauth-providers-with-mvc/_static/image2.png)

## <a name="enable-a-provider"></a><span data-ttu-id="0568d-131">啟用提供者</span><span class="sxs-lookup"><span data-stu-id="0568d-131">Enable a provider</span></span>

<span data-ttu-id="0568d-132">當您建立的 MVC 4 web 應用程式使用網際網路應用程式範本時，專案以名為 AuthConfig.cs 應用程式中的檔案建立\_開始資料夾。</span><span class="sxs-lookup"><span data-stu-id="0568d-132">When you create an MVC 4 web application with the Internet Application template, the project is created with a file named AuthConfig.cs in the App\_Start folder.</span></span>

![AuthConfig 檔案](using-oauth-providers-with-mvc/_static/image3.png)

<span data-ttu-id="0568d-134">AuthConfig 檔案包含程式碼以註冊的外部驗證提供者的用戶端。</span><span class="sxs-lookup"><span data-stu-id="0568d-134">The AuthConfig file contains code to register clients for external authentication providers.</span></span> <span data-ttu-id="0568d-135">根據預設，此程式碼標記為註解，因此沒有外部提供者會啟用。</span><span class="sxs-lookup"><span data-stu-id="0568d-135">By default, this code is commented out, so none of the external providers are enabled.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample1.cs)]

<span data-ttu-id="0568d-136">您必須使用外部驗證用戶端的這段程式碼取消註解。</span><span class="sxs-lookup"><span data-stu-id="0568d-136">You must uncomment this code to use the external authentication client.</span></span> <span data-ttu-id="0568d-137">您取消註解您想要包含在您的網站中的提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-137">You uncomment only the providers you want to include in your site.</span></span> <span data-ttu-id="0568d-138">本教學課程中，您將只啟用 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="0568d-138">For this tutorial, you will only enable the Facebook credentials.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample2.cs)]

<span data-ttu-id="0568d-139">請注意在上述範例中，此方法包含空字串的註冊參數。</span><span class="sxs-lookup"><span data-stu-id="0568d-139">Notice in the above example that the method includes empty strings for the registration parameters.</span></span> <span data-ttu-id="0568d-140">如果您嘗試執行應用程式現在，應用程式擲回引數例外狀況，因為參數不允許空字串。</span><span class="sxs-lookup"><span data-stu-id="0568d-140">If you try to run the application now, the application throws an argument exception because empty strings are not allowed for the parameters.</span></span> <span data-ttu-id="0568d-141">若要提供有效的值，您必須向外部提供者，註冊您的網站下, 一節中所示。</span><span class="sxs-lookup"><span data-stu-id="0568d-141">To provide valid values, you must register your web site with the external providers, as shown in the next section.</span></span>

## <a name="registering-with-an-external-provider"></a><span data-ttu-id="0568d-142">使用外部提供者註冊</span><span class="sxs-lookup"><span data-stu-id="0568d-142">Registering with an external provider</span></span>

<span data-ttu-id="0568d-143">若要驗證從外部提供者的認證的使用者，您必須註冊您的網站與提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-143">To authenticate users with credentials from an external provider, you must register your web site with the provider.</span></span> <span data-ttu-id="0568d-144">當您註冊您的網站時，您會收到包含註冊用戶端時的參數 （例如金鑰或識別碼和密碼）。</span><span class="sxs-lookup"><span data-stu-id="0568d-144">When you register your site, you will receive the parameters (such as key or id, and secret) to include when registering the client.</span></span> <span data-ttu-id="0568d-145">您必須有您想要使用的提供者的帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-145">You must have an account with the providers you wish to use.</span></span>

<span data-ttu-id="0568d-146">本教學課程不會顯示所有使用這些提供者註冊時，您必須執行的步驟。</span><span class="sxs-lookup"><span data-stu-id="0568d-146">This tutorial does not show all of the steps you must perform to register with these providers.</span></span> <span data-ttu-id="0568d-147">步驟通常都不困難。</span><span class="sxs-lookup"><span data-stu-id="0568d-147">The steps are typically not difficult.</span></span> <span data-ttu-id="0568d-148">若要順利註冊您的網站，請遵循這些站台上提供的指示。</span><span class="sxs-lookup"><span data-stu-id="0568d-148">To successfully register your site, follow the instructions provided on those sites.</span></span> <span data-ttu-id="0568d-149">若要開始註冊您的網站，請參閱 「 開發人員網站：</span><span class="sxs-lookup"><span data-stu-id="0568d-149">To get started with registering your site, see the developer site for:</span></span>

- [<span data-ttu-id="0568d-150">Facebook</span><span class="sxs-lookup"><span data-stu-id="0568d-150">Facebook</span></span>](https://developers.facebook.com/)
- [<span data-ttu-id="0568d-151">Google</span><span class="sxs-lookup"><span data-stu-id="0568d-151">Google</span></span>](https://developers.google.com/)
- [<span data-ttu-id="0568d-152">Microsoft</span><span class="sxs-lookup"><span data-stu-id="0568d-152">Microsoft</span></span>](http://manage.dev.live.com/)
- [<span data-ttu-id="0568d-153">Twitter</span><span class="sxs-lookup"><span data-stu-id="0568d-153">Twitter</span></span>](https://dev.twitter.com/)

<span data-ttu-id="0568d-154">當您向 Facebook 註冊您的網站，您可以提供&quot;localhost&quot;站台網域和`&quot; http://localhost/&quot;`url，如下圖所示。</span><span class="sxs-lookup"><span data-stu-id="0568d-154">When registering your site with Facebook, you can provide &quot;localhost&quot; for the site domain and `&quot;http://localhost/&quot;` for the URL, as shown in the image below.</span></span> <span data-ttu-id="0568d-155">使用 localhost 適用於大部分的提供者，但目前不適用於 Microsoft 提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-155">Using localhost works with most providers, but currently does not work with the Microsoft provider.</span></span> <span data-ttu-id="0568d-156">Microsoft 提供者，您必須包含有效的網站 URL。</span><span class="sxs-lookup"><span data-stu-id="0568d-156">For the Microsoft provider, you must include a valid web site URL.</span></span>

![註冊網站](using-oauth-providers-with-mvc/_static/image4.png)

<span data-ttu-id="0568d-158">在上圖中，已移除應用程式識別碼、 應用程式祕密和連絡人的電子郵件的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-158">In the previous image, the values for the app id, app secret, and contact email have been removed.</span></span> <span data-ttu-id="0568d-159">當您實際註冊您的網站時，這些值將會出現。</span><span class="sxs-lookup"><span data-stu-id="0568d-159">When you actually register your site, those values will be present.</span></span> <span data-ttu-id="0568d-160">您會想要注意應用程式識別碼和應用程式祕密的值，因為您會將它們加入您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="0568d-160">You will want to note the values for app id and app secret because you will add them to your application.</span></span>

## <a name="creating-test-users"></a><span data-ttu-id="0568d-161">建立測試使用者</span><span class="sxs-lookup"><span data-stu-id="0568d-161">Creating test users</span></span>

<span data-ttu-id="0568d-162">如果您不介意使用現有的 Facebook 帳戶來測試您的網站，您可以略過本節。</span><span class="sxs-lookup"><span data-stu-id="0568d-162">If you do not mind using an existing Facebook account to test your site, you can skip this section.</span></span>

<span data-ttu-id="0568d-163">您可以輕鬆建立 Facebook 應用程式管理頁面中的應用程式的測試使用者。</span><span class="sxs-lookup"><span data-stu-id="0568d-163">You can easily create test users for your application within the Facebook app management page.</span></span> <span data-ttu-id="0568d-164">您可以使用這些測試帳戶登入您的網站。</span><span class="sxs-lookup"><span data-stu-id="0568d-164">You can use these test accounts to log in to your site.</span></span> <span data-ttu-id="0568d-165">您可以建立測試使用者依序按一下**角色**連結，在左側的導覽窗格中，然後按一下**建立**連結。</span><span class="sxs-lookup"><span data-stu-id="0568d-165">You create test users by clicking the **Roles** link in the left navigation pane and the clicking the **Create** link.</span></span>

![建立測試使用者](using-oauth-providers-with-mvc/_static/image5.png)

<span data-ttu-id="0568d-167">Facebook 站台會自動建立測試帳戶，您要求的數目。</span><span class="sxs-lookup"><span data-stu-id="0568d-167">The Facebook site automatically creates the number of test accounts that you request.</span></span>

## <a name="adding-application-id-and-secret-from-the-provider"></a><span data-ttu-id="0568d-168">從提供者中新增應用程式識別碼和祕密</span><span class="sxs-lookup"><span data-stu-id="0568d-168">Adding application id and secret from the provider</span></span>

<span data-ttu-id="0568d-169">現在，您收到的識別碼和密碼從 Facebook，請回到 AuthConfig 檔案，並將它們加入做為參數值。</span><span class="sxs-lookup"><span data-stu-id="0568d-169">Now that you have received the id and secret from Facebook, go back to the AuthConfig file and add them as the parameter values.</span></span> <span data-ttu-id="0568d-170">如下所示的值不是真正的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-170">The values shown below are not real values.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample3.cs)]

## <a name="log-in-with-external-credentials"></a><span data-ttu-id="0568d-171">外部認證登入</span><span class="sxs-lookup"><span data-stu-id="0568d-171">Log in with external credentials</span></span>

<span data-ttu-id="0568d-172">這是您只需要啟用您的網站中的外部認證。</span><span class="sxs-lookup"><span data-stu-id="0568d-172">That is all you have to do to enable external credentials in your site.</span></span> <span data-ttu-id="0568d-173">執行您的應用程式，然後按一下右上角中的 [登入] 連結。</span><span class="sxs-lookup"><span data-stu-id="0568d-173">Run your application and click the login link in the upper right corner.</span></span> <span data-ttu-id="0568d-174">範本會自動識別您已註冊 Facebook 做為提供者，並包含提供者按鈕。</span><span class="sxs-lookup"><span data-stu-id="0568d-174">The template automatically recognizes that you have registered Facebook as a provider and includes a button for the provider.</span></span> <span data-ttu-id="0568d-175">如果您註冊多個提供者，針對每個按鈕就會自動包含。</span><span class="sxs-lookup"><span data-stu-id="0568d-175">If you register multiple providers, a button for each one is automatically included.</span></span>

![外部登入](using-oauth-providers-with-mvc/_static/image6.png)

<span data-ttu-id="0568d-177">本教學課程並未涵蓋如何自訂登入按鈕外部提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-177">This tutorial does not cover how to customize the log in buttons for the external providers.</span></span> <span data-ttu-id="0568d-178">如需該資訊，請參閱[自訂登入 UI，使用 OAuth/OpenID 時](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx)。</span><span class="sxs-lookup"><span data-stu-id="0568d-178">For that information, see [Customizing the login UI when using OAuth/OpenID](https://blogs.msdn.com/b/pranav_rastogi/archive/2012/08/24/customizing-the-login-ui-when-using-oauth-openid.aspx).</span></span>

<span data-ttu-id="0568d-179">按一下 [Facebook] 按鈕，以 Facebook 認證登入。</span><span class="sxs-lookup"><span data-stu-id="0568d-179">Click on the Facebook button to log in with Facebook credentials.</span></span> <span data-ttu-id="0568d-180">當您選取其中一個外部提供者時，您會被重新導向至該網站，並提示來登入該服務。</span><span class="sxs-lookup"><span data-stu-id="0568d-180">When you select one of the external providers, you are redirected to that site and prompted by that service to log in.</span></span>

<span data-ttu-id="0568d-181">下圖會顯示適用於 Facebook 登入畫面。</span><span class="sxs-lookup"><span data-stu-id="0568d-181">The following image shows the login screen for Facebook.</span></span> <span data-ttu-id="0568d-182">它會註明您使用您的 Facebook 帳戶登入名為 oauthmvcexample 的網站。</span><span class="sxs-lookup"><span data-stu-id="0568d-182">It notes that you are using your Facebook account to log in to a site named oauthmvcexample.</span></span>

![facebook 驗證](using-oauth-providers-with-mvc/_static/image7.png)

<span data-ttu-id="0568d-184">登入 Facebook 認證之後，畫面會告知使用者站台，將會擁有存取權的基本資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-184">After logging in with Facebook credentials, a page informs the user that the site will have access to basic information.</span></span>

![要求權限](using-oauth-providers-with-mvc/_static/image8.png)

<span data-ttu-id="0568d-186">選取之後**移至應用程式**，使用者必須註冊站台。</span><span class="sxs-lookup"><span data-stu-id="0568d-186">After selecting **Go to App**, the user must register for the site.</span></span> <span data-ttu-id="0568d-187">下圖顯示 [註冊] 頁面之後使用者已登入 Facebook 認證。</span><span class="sxs-lookup"><span data-stu-id="0568d-187">The following image shows the registration page after a user has logged in with Facebook credentials.</span></span> <span data-ttu-id="0568d-188">通常從提供者名稱預先填入使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="0568d-188">The user name is typically pre-filled with a name from the provider.</span></span>

![註冊](using-oauth-providers-with-mvc/_static/image9.png)

<span data-ttu-id="0568d-190">按一下 **註冊**以完成註冊。</span><span class="sxs-lookup"><span data-stu-id="0568d-190">Click **Register** to complete registration.</span></span> <span data-ttu-id="0568d-191">關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0568d-191">Close the browser.</span></span>

<span data-ttu-id="0568d-192">您可以看到您的資料庫中已加入新的帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-192">You can see that the new account has been added to your database.</span></span> <span data-ttu-id="0568d-193">在 [伺服器總管] 中，開啟**DefaultConnection**資料庫，並開啟**資料表**資料夾。</span><span class="sxs-lookup"><span data-stu-id="0568d-193">In Server Explorer, open the **DefaultConnection** database and open the **Tables** folder.</span></span>

![資料庫資料表](using-oauth-providers-with-mvc/_static/image10.png)

<span data-ttu-id="0568d-195">以滑鼠右鍵按一下**UserProfile**資料表，然後選取**顯示資料表資料**。</span><span class="sxs-lookup"><span data-stu-id="0568d-195">Right-click the **UserProfile** table and select **Show Table Data**.</span></span>

![顯示資料](using-oauth-providers-with-mvc/_static/image11.png)

<span data-ttu-id="0568d-197">您會看到您所新增的新帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-197">You will see the new account you added.</span></span> <span data-ttu-id="0568d-198">在中的資料看起來**網頁\_OAuthMembership**資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-198">Look at the data in **webpage\_OAuthMembership** table.</span></span> <span data-ttu-id="0568d-199">您會看到您剛才新增的帳戶的外部提供者的相關詳細資料。</span><span class="sxs-lookup"><span data-stu-id="0568d-199">You will see more data related to the external provider for the account you just added.</span></span>

<span data-ttu-id="0568d-200">如果您只想要啟用外部驗證，您已完成。</span><span class="sxs-lookup"><span data-stu-id="0568d-200">If you only want to enable external authentication, you are done.</span></span> <span data-ttu-id="0568d-201">不過，您可以進一步整合來自提供者的資訊到新的使用者註冊程序，如下列各節中所示。</span><span class="sxs-lookup"><span data-stu-id="0568d-201">However, you can further integrate information from the provider into the new user registration process, as shown in the following sections.</span></span>

## <a name="create-models-for-additional-user-information"></a><span data-ttu-id="0568d-202">建立模型的其他使用者資訊</span><span class="sxs-lookup"><span data-stu-id="0568d-202">Create models for additional user information</span></span>

<span data-ttu-id="0568d-203">您注意到在先前章節中，您不需要擷取才能內建的帳戶註冊的任何其他資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-203">As you noticed in the previous sections, you do not need to retrieve any additional information for the built-in account registration to work.</span></span> <span data-ttu-id="0568d-204">不過，大部分的外部提供者傳遞回其他使用者的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-204">However, most external providers pass back additional information about the user.</span></span> <span data-ttu-id="0568d-205">下列各節說明如何保留該資訊，並將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0568d-205">The following sections show how to retain that information and save it to a database.</span></span> <span data-ttu-id="0568d-206">具體來說，您將會保留使用者的完整名稱，使用者的個人網頁的 URI 值和值，指出是否 Facebook 已驗證的帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-206">Specifically, you will retain values for the user's full name, the URI of the user's personal web page, and a value that indicates whether Facebook has verified the account.</span></span>

<span data-ttu-id="0568d-207">您將使用[Code First 移轉](https://msdn.microsoft.com/data/jj591621)加入資料表來儲存額外的使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-207">You will use [Code First Migrations](https://msdn.microsoft.com/data/jj591621) to add a table for storing additional user information.</span></span> <span data-ttu-id="0568d-208">您將加入資料表至現有的資料庫，因此您首先要建立目前資料庫的快照集。</span><span class="sxs-lookup"><span data-stu-id="0568d-208">You are adding the table to an existing database, so you will first need to create a snapshot of the current database.</span></span> <span data-ttu-id="0568d-209">藉由建立目前資料庫的快照集，您可以稍後建立包含新的資料表移轉。</span><span class="sxs-lookup"><span data-stu-id="0568d-209">By creating a snapshot of the current database, you can later create a migration that contains only the new table.</span></span> <span data-ttu-id="0568d-210">若要建立目前資料庫的快照集：</span><span class="sxs-lookup"><span data-stu-id="0568d-210">To create a snapshot of the current database:</span></span>

1. <span data-ttu-id="0568d-211">開啟**套件管理員主控台**</span><span class="sxs-lookup"><span data-stu-id="0568d-211">Open the **Package Manager Console**</span></span>
2. <span data-ttu-id="0568d-212">執行命令**啟用移轉**</span><span class="sxs-lookup"><span data-stu-id="0568d-212">Run the command **enable-migrations**</span></span>
3. <span data-ttu-id="0568d-213">執行命令**IgnoreChanges 新增移轉初始 –**</span><span class="sxs-lookup"><span data-stu-id="0568d-213">Run the command **add-migration initial –IgnoreChanges**</span></span>
4. <span data-ttu-id="0568d-214">執行命令**更新資料庫**</span><span class="sxs-lookup"><span data-stu-id="0568d-214">Run the command **update-database**</span></span>

<span data-ttu-id="0568d-215">現在，您將加入新的屬性。</span><span class="sxs-lookup"><span data-stu-id="0568d-215">Now, you will add the new properties.</span></span> <span data-ttu-id="0568d-216">在 [模型] 資料夾中，開啟 AccountModels.cs 檔案，並尋找 RegisterExternalLoginModel 類別。</span><span class="sxs-lookup"><span data-stu-id="0568d-216">In the Models folder, open the AccountModels.cs file, and find the RegisterExternalLoginModel class.</span></span> <span data-ttu-id="0568d-217">RegisterExternalLoginModel 類別保留來自驗證提供者的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-217">The RegisterExternalLoginModel class holds values that come back from the authentication provider.</span></span> <span data-ttu-id="0568d-218">新增稱為 FullName 和連結，以下的醒目提示的屬性。</span><span class="sxs-lookup"><span data-stu-id="0568d-218">Add properties named FullName and Link, as highlighted below.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample4.cs?highlight=9-13)]

<span data-ttu-id="0568d-219">也在 AccountModels.cs，加入新的類別，稱為 ExtraUserInformation。</span><span class="sxs-lookup"><span data-stu-id="0568d-219">Also in AccountModels.cs, add a new class called ExtraUserInformation.</span></span> <span data-ttu-id="0568d-220">此類別代表會在資料庫中建立的新資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-220">This class represents the new table which will be created in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample5.cs)]

<span data-ttu-id="0568d-221">在 UsersContext 類別中加入醒目提示的程式碼來建立新的類別的 DbSet 屬性。</span><span class="sxs-lookup"><span data-stu-id="0568d-221">In the UsersContext class, add the highlighted code below to create a DbSet property for the new class.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample6.cs?highlight=9)]

<span data-ttu-id="0568d-222">您現在已準備好建立新的資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-222">You are now ready to create the new table.</span></span> <span data-ttu-id="0568d-223">一次，這次，請開啟套件管理員主控台：</span><span class="sxs-lookup"><span data-stu-id="0568d-223">Open the Package Manager Console again and this time:</span></span>

1. <span data-ttu-id="0568d-224">執行命令**新增移轉 AddExtraUserInformation**</span><span class="sxs-lookup"><span data-stu-id="0568d-224">Run the command **add-migration AddExtraUserInformation**</span></span>
2. <span data-ttu-id="0568d-225">執行命令**更新資料庫**</span><span class="sxs-lookup"><span data-stu-id="0568d-225">Run the command **update-database**</span></span>

<span data-ttu-id="0568d-226">資料庫中現在有新的資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-226">The new table now exists in the database.</span></span>

## <a name="retrieve-the-additional-data"></a><span data-ttu-id="0568d-227">擷取其他的資料</span><span class="sxs-lookup"><span data-stu-id="0568d-227">Retrieve the additional data</span></span>

<span data-ttu-id="0568d-228">有兩種方式可擷取其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="0568d-228">There are two ways to retrieve additional user data.</span></span> <span data-ttu-id="0568d-229">第一種方式是保留使用者資料期間所傳遞回，根據預設，驗證要求。</span><span class="sxs-lookup"><span data-stu-id="0568d-229">The first way is to retain user data that is passed back, by default, during the authentication request.</span></span> <span data-ttu-id="0568d-230">第二種方式是特別呼叫提供者 API，並要求的詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-230">The second way is to specifically call the provider API and request more information.</span></span> <span data-ttu-id="0568d-231">FullName 和連結的值會自動傳遞回 by Facebook。</span><span class="sxs-lookup"><span data-stu-id="0568d-231">Values for FullName and Link are automatically passed back by Facebook.</span></span> <span data-ttu-id="0568d-232">值，指出是否 Facebook 已驗證的帳戶是透過對 Facebook API 的呼叫來擷取。</span><span class="sxs-lookup"><span data-stu-id="0568d-232">A value that indicates whether Facebook has verified the account is retrieved through a call to the Facebook API.</span></span> <span data-ttu-id="0568d-233">首先，您將為 FullName 和連結，來填入值，並將更新版本中，會取得已驗證的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-233">First, you will populate values for FullName and Link, and then later, you will get the verified value.</span></span>

<span data-ttu-id="0568d-234">若要擷取的其他使用者資料，請開啟**AccountController.cs**中的檔案**控制站**資料夾。</span><span class="sxs-lookup"><span data-stu-id="0568d-234">To retrieve the additional user data, open the **AccountController.cs** file in the **Controllers** folder.</span></span>

<span data-ttu-id="0568d-235">此檔案包含的邏輯記錄、 註冊和管理帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-235">This file contains the logic for logging, registering, and managing accounts.</span></span> <span data-ttu-id="0568d-236">特別是，請注意呼叫的方法**ExternalLoginCallback**並**ExternalLoginConfirmation**。</span><span class="sxs-lookup"><span data-stu-id="0568d-236">In particular, notice the methods called **ExternalLoginCallback** and **ExternalLoginConfirmation**.</span></span> <span data-ttu-id="0568d-237">在這些方法中，您可以新增自訂應用程式的外部登入作業的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0568d-237">Within these methods, you can add code to customize external login operations for your application.</span></span> <span data-ttu-id="0568d-238">第一行**ExternalLoginCallback**方法包含：</span><span class="sxs-lookup"><span data-stu-id="0568d-238">The first line of the **ExternalLoginCallback** method contains:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample7.cs)]

<span data-ttu-id="0568d-239">其他使用者資料會傳回到**ExtraData**屬性**AuthenticationResult**從傳回的物件**VerifyAuthentication**方法。</span><span class="sxs-lookup"><span data-stu-id="0568d-239">Additional user data is passed back in the **ExtraData** property of the **AuthenticationResult** object that is returned from the **VerifyAuthentication** method.</span></span> <span data-ttu-id="0568d-240">Facebook 用戶端包含下列值**ExtraData**屬性：</span><span class="sxs-lookup"><span data-stu-id="0568d-240">The Facebook client contains the following values in the **ExtraData** property:</span></span>

- <span data-ttu-id="0568d-241">id</span><span class="sxs-lookup"><span data-stu-id="0568d-241">id</span></span>
- <span data-ttu-id="0568d-242">名稱</span><span class="sxs-lookup"><span data-stu-id="0568d-242">name</span></span>
- <span data-ttu-id="0568d-243">連結</span><span class="sxs-lookup"><span data-stu-id="0568d-243">link</span></span>
- <span data-ttu-id="0568d-244">性別</span><span class="sxs-lookup"><span data-stu-id="0568d-244">gender</span></span>
- <span data-ttu-id="0568d-245">accesstoken</span><span class="sxs-lookup"><span data-stu-id="0568d-245">accesstoken</span></span>

<span data-ttu-id="0568d-246">其他提供者都會在 ExtraData 屬性類似，但稍有不同的資料。</span><span class="sxs-lookup"><span data-stu-id="0568d-246">Other providers will have similar but slightly different data in the ExtraData property.</span></span>

<span data-ttu-id="0568d-247">如果您的站台的新手使用者，您會擷取部分的其他資料，並將該資料傳遞到 [確認] 檢視。</span><span class="sxs-lookup"><span data-stu-id="0568d-247">If the user is new to your site, you will retrieve some the additional data and pass that data to the confirmation view.</span></span> <span data-ttu-id="0568d-248">只有當使用者是您的站台的新手，就會執行方法中的程式碼的最後一個區塊。</span><span class="sxs-lookup"><span data-stu-id="0568d-248">The last block of code in the method is run only if the user is new to your site.</span></span> <span data-ttu-id="0568d-249">將下面這一行：</span><span class="sxs-lookup"><span data-stu-id="0568d-249">Replace the following line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample8.cs)]

<span data-ttu-id="0568d-250">使用下列這一行：</span><span class="sxs-lookup"><span data-stu-id="0568d-250">With this line:</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample9.cs)]

<span data-ttu-id="0568d-251">這項變更只會包含 FullName 和 連結屬性的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-251">This change merely includes values for the FullName and Link properties.</span></span>

<span data-ttu-id="0568d-252">在  **ExternalLoginConfirmation**方法中，修改下列程式碼以反白顯示儲存額外的使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-252">In the **ExternalLoginConfirmation** method, modify the code as highlighted below to save the additional user information.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample10.cs?highlight=4,7-13)]

## <a name="adjusting-the-view"></a><span data-ttu-id="0568d-253">調整檢視</span><span class="sxs-lookup"><span data-stu-id="0568d-253">Adjusting the view</span></span>

<span data-ttu-id="0568d-254">在 [註冊] 頁面中，將會顯示您從提供者擷取其他使用者資料。</span><span class="sxs-lookup"><span data-stu-id="0568d-254">The additional user data that you retrieve from the provider will be displayed in the registration page.</span></span>

<span data-ttu-id="0568d-255">在 **檢視**/**帳戶**資料夾中，開啟**ExternalLoginConfirmation.cshtml**。</span><span class="sxs-lookup"><span data-stu-id="0568d-255">In the **Views**/**Account** folder, open **ExternalLoginConfirmation.cshtml**.</span></span> <span data-ttu-id="0568d-256">下方 使用者名稱與現有欄位，請為 FullName、 連結和 PictureLink 新增欄位。</span><span class="sxs-lookup"><span data-stu-id="0568d-256">Below the existing field for user name, add fields for FullName, Link, and PictureLink.</span></span>

[!code-cshtml[Main](using-oauth-providers-with-mvc/samples/sample11.cshtml)]

<span data-ttu-id="0568d-257">您就幾乎準備好執行應用程式，並註冊新的使用者儲存的其他資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-257">You are now almost ready to run the application and register a new user with the additional information saved.</span></span> <span data-ttu-id="0568d-258">您必須已經不是向網站註冊的帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-258">You must have an account that is not already registered with the site.</span></span> <span data-ttu-id="0568d-259">您可以使用不同的測試帳戶，或刪除的資料列**UserProfile**並**網頁\_OAuthMembership**您想要重複使用之帳戶的資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-259">You can either use a different test account, or delete the rows in the **UserProfile** and **webpages\_OAuthMembership** tables for the account you wish to reuse.</span></span> <span data-ttu-id="0568d-260">刪除資料列，您要確定帳戶已註冊一次。</span><span class="sxs-lookup"><span data-stu-id="0568d-260">By deleting those rows, you will ensure that the account is registered again.</span></span>

<span data-ttu-id="0568d-261">執行應用程式並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="0568d-261">Run the application and register the new user.</span></span> <span data-ttu-id="0568d-262">請注意，這次 [確認] 頁面包含多個值。</span><span class="sxs-lookup"><span data-stu-id="0568d-262">Notice that this time the confirmation page contains more values.</span></span>

![註冊](using-oauth-providers-with-mvc/_static/image12.png)

<span data-ttu-id="0568d-264">完成註冊之後, 關閉瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="0568d-264">After completing registration, close the browser.</span></span> <span data-ttu-id="0568d-265">請注意新的值，在資料庫中尋找**ExtraUserInformation**資料表。</span><span class="sxs-lookup"><span data-stu-id="0568d-265">Look in the database to notice the new values in the **ExtraUserInformation** table.</span></span>

## <a name="install-nuget-package-for-facebook-api"></a><span data-ttu-id="0568d-266">安裝適用於 Facebook API 的 NuGet 套件</span><span class="sxs-lookup"><span data-stu-id="0568d-266">Install NuGet package for Facebook API</span></span>

<span data-ttu-id="0568d-267">提供 Facebook [API](https://developers.facebook.com/docs/reference/apis/) ，您可以呼叫執行作業。</span><span class="sxs-lookup"><span data-stu-id="0568d-267">Facebook provides an [API](https://developers.facebook.com/docs/reference/apis/) that you can call to perform operations.</span></span> <span data-ttu-id="0568d-268">您可以呼叫 Facebook API，將傳送的 HTTP 要求導向，或使用 安裝 NuGet 套件，可簡化傳送這些要求。</span><span class="sxs-lookup"><span data-stu-id="0568d-268">You can call the Facebook API either by directing sending HTTP requests, or by using installing a NuGet package that facilitates sending those requests.</span></span> <span data-ttu-id="0568d-269">使用 NuGet 套件會顯示在本教學課程中，但安裝 NuGet 套件並不重要。</span><span class="sxs-lookup"><span data-stu-id="0568d-269">Using a NuGet package is shown in this tutorial, but installing NuGet package is not essential.</span></span> <span data-ttu-id="0568d-270">本教學課程會示範如何使用 Facebook C# SDK 套件。</span><span class="sxs-lookup"><span data-stu-id="0568d-270">This tutorial shows how to use the Facebook C# SDK package.</span></span> <span data-ttu-id="0568d-271">另外還有其他協助呼叫 Facebook API 的 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="0568d-271">There are other NuGet packages that assist with calling the Facebook API.</span></span>

<span data-ttu-id="0568d-272">從**管理 NuGet 套件**windows 中，選取 Facebook C# SDK 封裝。</span><span class="sxs-lookup"><span data-stu-id="0568d-272">From the **Manage NuGet Packages** windows, select the Facebook C# SDK package.</span></span>

![安裝套件](using-oauth-providers-with-mvc/_static/image13.png)

<span data-ttu-id="0568d-274">您將使用 Facebook C# SDK 呼叫使用者需要存取權杖的作業。</span><span class="sxs-lookup"><span data-stu-id="0568d-274">You will use the Facebook C# SDK to call an operation that requires the access token for the user.</span></span> <span data-ttu-id="0568d-275">下節會說明如何取得存取權杖。</span><span class="sxs-lookup"><span data-stu-id="0568d-275">The next section shows how to get the access token.</span></span>

## <a name="retrieve-access-token"></a><span data-ttu-id="0568d-276">擷取存取權杖</span><span class="sxs-lookup"><span data-stu-id="0568d-276">Retrieve access token</span></span>

<span data-ttu-id="0568d-277">使用者的認證通過驗證後，最外部提供者傳回存取權杖。</span><span class="sxs-lookup"><span data-stu-id="0568d-277">Most external providers pass back an access token after the user's credentials are verified.</span></span> <span data-ttu-id="0568d-278">此存取權杖是非常重要，因為它可讓您呼叫作業，則只能用於已驗證的使用者。</span><span class="sxs-lookup"><span data-stu-id="0568d-278">This access token is very important because it enables you to call operations that are only available to authenticated users.</span></span> <span data-ttu-id="0568d-279">因此，擷取及儲存存取權杖是必要時您想要提供更多的功能。</span><span class="sxs-lookup"><span data-stu-id="0568d-279">Therefore, retrieving and storing the access token is essential when you want to provide more functionality.</span></span>

<span data-ttu-id="0568d-280">根據外部提供者，存取權杖只在有限可能是時間的有效的。</span><span class="sxs-lookup"><span data-stu-id="0568d-280">Depending on the external provider, the access token may be valid for only a limited amount of time.</span></span> <span data-ttu-id="0568d-281">若要確保您擁有有效的存取權杖，您會擷取它每次使用者登入，並將它儲存為工作階段值，而非將它儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0568d-281">To ensure that you have a valid access token, you will retrieve it each time the user logs in, and store it as a session value rather than save it to a database.</span></span>

<span data-ttu-id="0568d-282">在  **ExternalLoginCallback**方法，存取權杖也會跟著傳遞回到**ExtraData**屬性**AuthenticationResult**物件。</span><span class="sxs-lookup"><span data-stu-id="0568d-282">In the **ExternalLoginCallback** method, the access token is also passed back in the **ExtraData** property of the **AuthenticationResult** object.</span></span> <span data-ttu-id="0568d-283">將反白顯示的程式碼加入**ExternalLoginCallback**以儲存存取權杖**工作階段**物件。</span><span class="sxs-lookup"><span data-stu-id="0568d-283">Add the highlighted code to **ExternalLoginCallback** to save the access token in the **Session** object.</span></span> <span data-ttu-id="0568d-284">每次使用者登入的 Facebook 帳戶，都會執行此程式碼。</span><span class="sxs-lookup"><span data-stu-id="0568d-284">This code is run every time the user logs in with a Facebook account.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample12.cs?highlight=11-14)]

<span data-ttu-id="0568d-285">雖然此範例會擷取來自 Facebook 存取權杖，您可以透過相同的索引鍵名稱為來自任何外部提供者擷取存取權杖&quot;accesstoken&quot;。</span><span class="sxs-lookup"><span data-stu-id="0568d-285">Although this example retrieves an access token from Facebook, you can retrieve the access token from any external provider through the same key named &quot;accesstoken&quot;.</span></span>

## <a name="logging-off"></a><span data-ttu-id="0568d-286">登出</span><span class="sxs-lookup"><span data-stu-id="0568d-286">Logging off</span></span>

<span data-ttu-id="0568d-287">預設值**登出**方法記錄使用者登出您的應用程式，但不加以記錄將使用者登出外部提供者。</span><span class="sxs-lookup"><span data-stu-id="0568d-287">The default **LogOff** method logs the user out of your application, but does not log the user out of the external provider.</span></span> <span data-ttu-id="0568d-288">登使用者登出 Facebook，並防止保存之後，使用者已登出此語彙基元中，您可以新增下列醒目提示的程式碼，以**登出**AccountController 中的方法。</span><span class="sxs-lookup"><span data-stu-id="0568d-288">To log the user out of Facebook, and prevent the token from persisting after the user has logged off, you can add the following highlighted code to the **LogOff** method in the AccountController.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample13.cs?highlight=6-14)]

<span data-ttu-id="0568d-289">在您提供的值`next`參數是要使用之後使用者已登出 Facebook 的 URL。</span><span class="sxs-lookup"><span data-stu-id="0568d-289">The value you provide in the `next` parameter is the URL to use after the user has logged out of Facebook.</span></span> <span data-ttu-id="0568d-290">在本機電腦上測試時，您會提供正確的連接埠號碼，您本機站台。</span><span class="sxs-lookup"><span data-stu-id="0568d-290">When testing on your local computer, you would provide the correct port number for your local site.</span></span> <span data-ttu-id="0568d-291">在生產環境中，您會提供預設頁面上，例如 contoso.com。</span><span class="sxs-lookup"><span data-stu-id="0568d-291">In production, you would provide a default page, such as contoso.com.</span></span>

## <a name="retrieve-user-information-that-requires-the-access-token"></a><span data-ttu-id="0568d-292">擷取需要存取權杖的使用者資訊</span><span class="sxs-lookup"><span data-stu-id="0568d-292">Retrieve user information that requires the access token</span></span>

<span data-ttu-id="0568d-293">既然您已儲存的存取權杖，並安裝 Facebook C# SDK 封裝，您可以一起使用這些向 Facebook 要求額外的使用者資訊。</span><span class="sxs-lookup"><span data-stu-id="0568d-293">Now that you have stored the access token and installed the Facebook C# SDK package, you can use them together to request additional user information from Facebook.</span></span> <span data-ttu-id="0568d-294">在  **ExternalLoginConfirmation**方法，建立的執行個體**FacebookClient**類別傳遞存取權杖的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-294">In the **ExternalLoginConfirmation** method, create an instance of the **FacebookClient** class by passing the value of the access token.</span></span> <span data-ttu-id="0568d-295">要求的值**驗證**目前已經過驗證的使用者屬性。</span><span class="sxs-lookup"><span data-stu-id="0568d-295">Request the value of the **verified** property for the current, authenticated user.</span></span> <span data-ttu-id="0568d-296">**驗證**屬性會指出是否 Facebook 已驗證的帳戶透過其他方式，例如將訊息傳送至行動電話。</span><span class="sxs-lookup"><span data-stu-id="0568d-296">The **verified** property indicates whether Facebook has validated the account through some other means, such as sending a message to a cell phone.</span></span> <span data-ttu-id="0568d-297">在資料庫中儲存此值。</span><span class="sxs-lookup"><span data-stu-id="0568d-297">Save this value in the database.</span></span>

[!code-csharp[Main](using-oauth-providers-with-mvc/samples/sample14.cs?highlight=7-18,25)]

<span data-ttu-id="0568d-298">您一次必須刪除使用者資料庫中的記錄，或使用不同的 Facebook 帳戶。</span><span class="sxs-lookup"><span data-stu-id="0568d-298">You will again need to either delete the records in the database for the user, or use a different Facebook account.</span></span>

<span data-ttu-id="0568d-299">執行應用程式，並註冊新的使用者。</span><span class="sxs-lookup"><span data-stu-id="0568d-299">Run the application, and register the new user.</span></span> <span data-ttu-id="0568d-300">看看**ExtraUserInformation**資料表，以查看已驗證屬性的值。</span><span class="sxs-lookup"><span data-stu-id="0568d-300">Look at the **ExtraUserInformation** table to see the value for the Verified property.</span></span>

## <a name="conclusion"></a><span data-ttu-id="0568d-301">結論</span><span class="sxs-lookup"><span data-stu-id="0568d-301">Conclusion</span></span>

<span data-ttu-id="0568d-302">在本教學課程中，您可以建立網站與 Facebook 整合進行使用者驗證和註冊資料。</span><span class="sxs-lookup"><span data-stu-id="0568d-302">In this tutorial, you created a site that is integrated with Facebook for user authentication and registration data.</span></span> <span data-ttu-id="0568d-303">您已了解設定 MVC 4 web 應用程式，以及如何自訂該預設行為的預設行為。</span><span class="sxs-lookup"><span data-stu-id="0568d-303">You learned about the default behavior that is set up for MVC 4 web application, and how to customize that default behavior.</span></span>

## <a name="related-topics"></a><span data-ttu-id="0568d-304">相關主題</span><span class="sxs-lookup"><span data-stu-id="0568d-304">Related topics</span></span>

- [<span data-ttu-id="0568d-305">使用驗證和 SQL DB 建立 ASP.NET MVC 應用程式並部署至 Azure App Service</span><span class="sxs-lookup"><span data-stu-id="0568d-305">Create an ASP.NET MVC app with auth and SQL DB and deploy to Azure App Service</span></span>](https://docs.microsoft.com/aspnet/core/security/authorization/secure-data)
