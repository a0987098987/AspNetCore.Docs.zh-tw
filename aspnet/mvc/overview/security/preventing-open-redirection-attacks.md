---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: 防止開啟重新導向攻擊 (C#) |Microsoft Docs
author: jongalloway
description: 本教學課程說明如何防止開啟重新導向攻擊，以及在您的 ASP.NET MVC 應用程式中。 本教學課程將告訴您所做的變更...
ms.author: riande
ms.date: 02/27/2014
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: 767f9c85527fbcdf34e700eb32fe0c6cad30bf0c
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41832225"
---
<a name="preventing-open-redirection-attacks-c"></a><span data-ttu-id="17036-104">防止開啟重新導向攻擊 (C#)</span><span class="sxs-lookup"><span data-stu-id="17036-104">Preventing Open Redirection Attacks (C#)</span></span>
====================
<span data-ttu-id="17036-105">藉由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="17036-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="17036-106">本教學課程說明如何防止開啟重新導向攻擊，以及在您的 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="17036-106">This tutorial explains how you can prevent open redirection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="17036-107">本教學課程會討論在 ASP.NET MVC 3 中 AccountController 所做的變更，並示範如何套用這些變更，以及在您現有的 ASP.NET MVC 1.0 和 2 個應用程式中。</span><span class="sxs-lookup"><span data-stu-id="17036-107">This tutorial discusses the changes that have been made in the AccountController in ASP.NET MVC 3 and demonstrates how you can apply these changes in your existing ASP.NET MVC 1.0 and 2 applications.</span></span>


## <a name="what-is-an-open-redirection-attack"></a><span data-ttu-id="17036-108">什麼是開啟重新導向攻擊？</span><span class="sxs-lookup"><span data-stu-id="17036-108">What is an Open Redirection Attack?</span></span>

<span data-ttu-id="17036-109">任何重新導向至指定透過例如查詢字串或表單資料要求 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。</span><span class="sxs-lookup"><span data-stu-id="17036-109">Any web application that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="17036-110">這種竄改稱為開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="17036-110">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="17036-111">每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL 未遭竄改。</span><span class="sxs-lookup"><span data-stu-id="17036-111">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="17036-112">使用預設 AccountController ASP.NET MVC 1.0 及 ASP.NET MVC 2 的登入很容易就能開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="17036-112">The login used in the default AccountController for both ASP.NET MVC 1.0 and ASP.NET MVC 2 is vulnerable to open redirection attacks.</span></span> <span data-ttu-id="17036-113">幸運的是，很容易更新您現有的應用程式使用 ASP.NET MVC 3 Preview 更正。</span><span class="sxs-lookup"><span data-stu-id="17036-113">Fortunately, it is easy to update your existing applications to use the corrections from the ASP.NET MVC 3 Preview.</span></span>

<span data-ttu-id="17036-114">若要了解這項弱點，讓我們看看登入重新導向預設 ASP.NET MVC 2 Web 應用程式專案中的運作方式。</span><span class="sxs-lookup"><span data-stu-id="17036-114">To understand the vulnerability, let's look at how the login redirection works in a default ASP.NET MVC 2 Web Application project.</span></span> <span data-ttu-id="17036-115">在此應用程式，嘗試瀏覽控制器動作，將 [Authorize] 屬性會未經授權的使用者重新導向 /Account/LogOn 檢視。</span><span class="sxs-lookup"><span data-stu-id="17036-115">In this application, attempting to visit a controller action that has the [Authorize] attribute will redirect unauthorized users to the /Account/LogOn view.</span></span> <span data-ttu-id="17036-116">此重新導向至 /Account/LogOn 會包含 returnUrl 查詢字串參數，以便使用者可以傳回給原始要求的 URL 之後使用者已成功登入。</span><span class="sxs-lookup"><span data-stu-id="17036-116">This redirect to /Account/LogOn will include a returnUrl querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span>

<span data-ttu-id="17036-117">在以下的螢幕擷取畫面，我們可以看到，嘗試存取時不會記錄在 /Account/ChangePassword 檢視導致重新導向至 /Account/LogOn 嗎？ReturnUrl = %2faccount%2fChangePassword %2f。</span><span class="sxs-lookup"><span data-stu-id="17036-117">In the screenshot below, we can see that an attempt to access the /Account/ChangePassword view when not logged in results in a redirect to /Account/LogOn?ReturnUrl=%2fAccount%2fChangePassword%2f.</span></span>

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

<span data-ttu-id="17036-118">**圖 01**： 具有開啟重新導向的登入頁面</span><span class="sxs-lookup"><span data-stu-id="17036-118">**Figure 01**: Login page with an open redirection</span></span>

<span data-ttu-id="17036-119">ReturnUrl 查詢字串參數無效的因為攻擊者可以修改它來進行開啟重新導向攻擊參數中插入任何 URL 位址。</span><span class="sxs-lookup"><span data-stu-id="17036-119">Since the ReturnUrl querystring parameter is not validated, an attacker can modify it to inject any URL address into the parameter to conduct an open redirection attack.</span></span> <span data-ttu-id="17036-120">為了示範這點，我們可以修改 ReturnUrl 參數[ http://bing.com ](http://bing.com)，因此會產生的登入 URL/帳戶/登入？ReturnUrl =<http://www.bing.com/>。</span><span class="sxs-lookup"><span data-stu-id="17036-120">To demonstrate this, we can modify the ReturnUrl parameter to [http://bing.com](http://bing.com), so the resulting login URL will be /Account/LogOn?ReturnUrl=<http://www.bing.com/>.</span></span> <span data-ttu-id="17036-121">在成功登入網站，我們會重新導向至[ http://bing.com ](http://bing.com)。</span><span class="sxs-lookup"><span data-stu-id="17036-121">Upon successfully logging in to the site, we are redirected to [http://bing.com](http://bing.com).</span></span> <span data-ttu-id="17036-122">因為不會驗證此重新導向，它可以改為指向惡意網站，並會嘗試誘騙使用者。</span><span class="sxs-lookup"><span data-stu-id="17036-122">Since this redirection is not validated, it could instead point to a malicious site that attempts to trick the user.</span></span>

### <a name="a-more-complex-open-redirection-attack"></a><span data-ttu-id="17036-123">更複雜的開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="17036-123">A more complex Open Redirection Attack</span></span>

<span data-ttu-id="17036-124">開啟重新導向攻擊是特別危險，因為攻擊者可讓您知道我們正在嘗試登入特定網站，讓我們更容易遭受[網路釣魚攻擊](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)。</span><span class="sxs-lookup"><span data-stu-id="17036-124">Open redirection attacks are especially dangerous because an attacker knows that we're trying to log into a specific website, which makes us vulnerable to a [phishing attack](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx).</span></span> <span data-ttu-id="17036-125">比方說，攻擊者無法傳送惡意電子郵件給網站的使用者在嘗試擷取其密碼。</span><span class="sxs-lookup"><span data-stu-id="17036-125">For example, an attacker could send malicious emails to website users in an attempt to capture their passwords.</span></span> <span data-ttu-id="17036-126">讓我們看看它 NerdDinner 網站上的運作方式。</span><span class="sxs-lookup"><span data-stu-id="17036-126">Let's look at how this would work on the NerdDinner site.</span></span> <span data-ttu-id="17036-127">（請注意，即時 NerdDinner 網站已更新以防止開啟重新導向攻擊）。</span><span class="sxs-lookup"><span data-stu-id="17036-127">(Note that the live NerdDinner site has been updated to protect against open redirection attacks.)</span></span>

<span data-ttu-id="17036-128">首先，攻擊者會傳送我們連結 NerdDinner 包含重新導向至其偽造的頁面中的 [登入] 頁面：</span><span class="sxs-lookup"><span data-stu-id="17036-128">First, an attacker sends us a link to the login page on NerdDinner that includes a redirect to their forged page:</span></span>

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

<span data-ttu-id="17036-129">請注意，傳回的 URL 指向 nerddiner.com，遺漏"n"從 word dinner。</span><span class="sxs-lookup"><span data-stu-id="17036-129">Note that the return URL points to nerddiner.com, which is missing an "n" from the word dinner.</span></span> <span data-ttu-id="17036-130">在此範例中，這是攻擊者控制的網域。</span><span class="sxs-lookup"><span data-stu-id="17036-130">In this example, this is a domain that the attacker controls.</span></span> <span data-ttu-id="17036-131">當我們存取上面的連結時，我們要進入合法 NerdDinner.com 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="17036-131">When we access the above link, we're taken to the legitimate NerdDinner.com login page.</span></span>

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

<span data-ttu-id="17036-132">**圖 02**: NerdDinner 具有開啟重新導向的登入頁面</span><span class="sxs-lookup"><span data-stu-id="17036-132">**Figure 02**: NerdDinner login page with an open redirection</span></span>

<span data-ttu-id="17036-133">當我們正確登入時，ASP.NET MVC AccountController 登入動作會重新導向我們 returnUrl 查詢字串參數中指定的 URL。</span><span class="sxs-lookup"><span data-stu-id="17036-133">When we correctly log in, the ASP.NET MVC AccountController's LogOn action redirects us to the URL specified in the returnUrl querystring parameter.</span></span> <span data-ttu-id="17036-134">在此情況下，它是攻擊者已輸入 URL，也就是[ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn)。</span><span class="sxs-lookup"><span data-stu-id="17036-134">In this case, it's the URL that the attacker has entered, which is [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn).</span></span> <span data-ttu-id="17036-135">我們非常戒慎守望，特別是因為攻擊者已經過謹慎地確定就很可能是我們不會發現這個問題，除非其偽造的頁面看起來完全合法的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="17036-135">Unless we're extremely watchful, it's very likely we won't notice this, especially because the attacker has been careful to make sure that their forged page looks exactly like the legitimate login page.</span></span> <span data-ttu-id="17036-136">此登入頁面會包含錯誤訊息，要求，我們再次登入。</span><span class="sxs-lookup"><span data-stu-id="17036-136">This login page includes an error message requesting that we login again.</span></span> <span data-ttu-id="17036-137">笨拙我們，我們必須有輸入錯誤，我們的密碼。</span><span class="sxs-lookup"><span data-stu-id="17036-137">Clumsy us, we must have mistyped our password.</span></span>

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

<span data-ttu-id="17036-138">**圖 03**： 偽造 NerdDinner 登入畫面</span><span class="sxs-lookup"><span data-stu-id="17036-138">**Figure 03**: Forged NerdDinner Login screen</span></span>

<span data-ttu-id="17036-139">當我們重新輸入我們的使用者名稱和密碼時，將資訊儲存到偽造的登入頁面，並將我們傳送合法 NerdDinner.com 站台。</span><span class="sxs-lookup"><span data-stu-id="17036-139">When we retype our user name and password, the forged login page saves the information and sends us back to the legitimate NerdDinner.com site.</span></span> <span data-ttu-id="17036-140">此時，NerdDinner.com 站台已經驗證，因此偽造的登入頁面可以重新導向至該頁面的直接。</span><span class="sxs-lookup"><span data-stu-id="17036-140">At this point, the NerdDinner.com site has already authenticated us, so the forged login page can redirect directly to that page.</span></span> <span data-ttu-id="17036-141">最終結果是，攻擊者有沒有我們的使用者名稱和密碼，我們不知道，我們已提供給它們。</span><span class="sxs-lookup"><span data-stu-id="17036-141">The end result is that the attacker has our user name and password, and we are unaware that we've provided it to them.</span></span>

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a><span data-ttu-id="17036-142">查看 AccountController 登入作用中的易受攻擊的程式碼</span><span class="sxs-lookup"><span data-stu-id="17036-142">Looking at the vulnerable code in the AccountController LogOn Action</span></span>

<span data-ttu-id="17036-143">ASP.NET MVC 2 應用程式中的登入動作的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="17036-143">The code for the LogOn action in an ASP.NET MVC 2 application is shown below.</span></span> <span data-ttu-id="17036-144">請注意，成功的登入，控制器傳回重新導向實施 returnurl。</span><span class="sxs-lookup"><span data-stu-id="17036-144">Note that upon a successful login, the controller returns a redirect to the returnUrl.</span></span> <span data-ttu-id="17036-145">您可以看到針對 returnUrl 參數執行任何驗證。</span><span class="sxs-lookup"><span data-stu-id="17036-145">You can see that no validation is being performed against the returnUrl parameter.</span></span>

<span data-ttu-id="17036-146">**列表 1 – 在 ASP.NET MVC 2 登入動作 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="17036-146">**Listing 1 – ASP.NET MVC 2 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

<span data-ttu-id="17036-147">現在讓我們看看 ASP.NET MVC 3 登入動作所做的變更。</span><span class="sxs-lookup"><span data-stu-id="17036-147">Now let's look at the changes to the ASP.NET MVC 3 LogOn action.</span></span> <span data-ttu-id="17036-148">此程式碼已變更為呼叫中名為 System.Web.Mvc.Url helper 類別的新方法來驗證 returnUrl 參數`IsLocalUrl()`。</span><span class="sxs-lookup"><span data-stu-id="17036-148">This code has been changed to validate the returnUrl parameter by calling a new method in the System.Web.Mvc.Url helper class named `IsLocalUrl()`.</span></span>

<span data-ttu-id="17036-149">**列表 2 – 在 ASP.NET MVC 3 登入動作 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="17036-149">**Listing 2 – ASP.NET MVC 3 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

<span data-ttu-id="17036-150">這個部分已經變更來驗證在 System.Web.Mvc.Url 協助程式類別中，呼叫新方法的傳回 URL 參數`IsLocalUrl()`。</span><span class="sxs-lookup"><span data-stu-id="17036-150">This has been changed to validate the return URL parameter by calling a new method in the System.Web.Mvc.Url helper class, `IsLocalUrl()`.</span></span>

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a><span data-ttu-id="17036-151">保護您的 ASP.NET MVC 1.0 和 MVC 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="17036-151">Protecting Your ASP.NET MVC 1.0 and MVC 2 Applications</span></span>

<span data-ttu-id="17036-152">我們可以利用 ASP.NET MVC 3 變更我們現有的 ASP.NET MVC 1.0 和 2 個應用程式中藉由加入 IsLocalUrl() helper 方法更新登入動作，以驗證 returnUrl 參數。</span><span class="sxs-lookup"><span data-stu-id="17036-152">We can take advantage of the ASP.NET MVC 3 changes in our existing ASP.NET MVC 1.0 and 2 applications by adding the IsLocalUrl() helper method and updating the LogOn action to validate the returnUrl parameter.</span></span>

<span data-ttu-id="17036-153">UrlHelper IsLocalUrl() 方法實際上只呼叫 System.Web.WebPages，以作為此驗證中的方法也會使用 ASP.NET Web Pages 應用程式。</span><span class="sxs-lookup"><span data-stu-id="17036-153">The UrlHelper IsLocalUrl() method actually just calling into a method in System.Web.WebPages, as this validation is also used by ASP.NET Web Pages applications.</span></span>

<span data-ttu-id="17036-154">**列表 3 – 從 ASP.NET MVC 3 UrlHelper IsLocalUrl() 方法 `class`**</span><span class="sxs-lookup"><span data-stu-id="17036-154">**Listing 3 – The IsLocalUrl() method from the ASP.NET MVC 3 UrlHelper `class`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

<span data-ttu-id="17036-155">IsUrlLocalToHost 方法包含實際的驗證邏輯，列表 4 中所示。</span><span class="sxs-lookup"><span data-stu-id="17036-155">The IsUrlLocalToHost method contains the actual validation logic, as shown in Listing 4.</span></span>

<span data-ttu-id="17036-156">**列表 4 – IsUrlLocalToHost() System.Web.WebPages RequestExtensions 類別方法**</span><span class="sxs-lookup"><span data-stu-id="17036-156">**Listing 4 – IsUrlLocalToHost() method from the System.Web.WebPages RequestExtensions class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

<span data-ttu-id="17036-157">在我們的 ASP.NET MVC 1.0 或 2 個應用程式中，我們會將 IsLocalUrl() 方法加入 AccountController，但是您可以建議盡可能將它新增至個別協助程式類別。</span><span class="sxs-lookup"><span data-stu-id="17036-157">In our ASP.NET MVC 1.0 or 2 application, we'll add a IsLocalUrl() method to the AccountController, but you're encouraged to add it to a separate helper class if possible.</span></span> <span data-ttu-id="17036-158">我們將會變更兩個小型 IsLocalUrl() 的 ASP.NET MVC 3 版本，讓它能夠 AccountController 內。</span><span class="sxs-lookup"><span data-stu-id="17036-158">We will make two small changes to the ASP.NET MVC 3 version of IsLocalUrl() so that it will work inside the AccountController.</span></span> <span data-ttu-id="17036-159">首先，我們會將它從公用方法的私用的方法，因為可以存取控制器中的公用方法，為控制器動作。</span><span class="sxs-lookup"><span data-stu-id="17036-159">First, we'll change it from a public method to a private method, since public methods in controllers can be accessed as controller actions.</span></span> <span data-ttu-id="17036-160">其次，我們要修改檢查針對應用程式主機 URL 主機的呼叫。</span><span class="sxs-lookup"><span data-stu-id="17036-160">Second, we'll modify the call that checks the URL host against the application host.</span></span> <span data-ttu-id="17036-161">呼叫，會使用本機 RequestContext UrlHelper 類別中的欄位。</span><span class="sxs-lookup"><span data-stu-id="17036-161">That call makes use of a local RequestContext field in the UrlHelper class.</span></span> <span data-ttu-id="17036-162">而不是使用這種情況。RequestContext.HttpContext.Request.Url.Host，我們將使用此。Request.Url.Host。</span><span class="sxs-lookup"><span data-stu-id="17036-162">Instead of using this.RequestContext.HttpContext.Request.Url.Host, we will use this.Request.Url.Host.</span></span> <span data-ttu-id="17036-163">下列程式碼顯示修改的 IsLocalUrl() 方法的控制器類別搭配 ASP.NET MVC 1.0 和 2 個應用程式中。</span><span class="sxs-lookup"><span data-stu-id="17036-163">The following code shows the modified IsLocalUrl() method for use with a controller class in ASP.NET MVC 1.0 and 2 applications.</span></span>

<span data-ttu-id="17036-164">**列表 5 – IsLocalUrl() 方法，這被修改搭配 MVC 控制器類別**</span><span class="sxs-lookup"><span data-stu-id="17036-164">**Listing 5 – IsLocalUrl() method, which is modified for use with an MVC Controller class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

<span data-ttu-id="17036-165">既然 IsLocalUrl() 方法已就緒，我們可以從我們的登入動作，以驗證 returnUrl 參數中，呼叫它，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="17036-165">Now that the IsLocalUrl() method is in place, we can call it from our LogOn action to validate the returnUrl parameter, as shown in the following code.</span></span>

<span data-ttu-id="17036-166">**列表 6-更新登入方法讓 returnUrl 參數進行驗證**</span><span class="sxs-lookup"><span data-stu-id="17036-166">**Listing 6 – Updated LogOn method which validates the returnUrl parameter**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

<span data-ttu-id="17036-167">現在我們可以測試開啟重新導向攻擊嘗試登入使用外部的傳回 URL。</span><span class="sxs-lookup"><span data-stu-id="17036-167">Now we can test an open redirection attack by attempting to log in using an external return URL.</span></span> <span data-ttu-id="17036-168">讓我們使用 /Account/LogOn 嗎？ReturnUrl =<http://www.bing.com/>一次。</span><span class="sxs-lookup"><span data-stu-id="17036-168">Let's use /Account/LogOn?ReturnUrl=<http://www.bing.com/> again.</span></span>

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

<span data-ttu-id="17036-169">**圖 04**： 測試更新的登入動作</span><span class="sxs-lookup"><span data-stu-id="17036-169">**Figure 04**: Testing the updated LogOn Action</span></span>

<span data-ttu-id="17036-170">已成功登入之後，我們將會重新導向至 Home/Index 控制器動作，而不是外部 URL。</span><span class="sxs-lookup"><span data-stu-id="17036-170">After successfully logging in, we are redirected to the Home/Index Controller action rather than the external URL.</span></span>

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

<span data-ttu-id="17036-171">**圖 05**： 擊敗的開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="17036-171">**Figure 05**: Open Redirection attack defeated</span></span>

## <a name="summary"></a><span data-ttu-id="17036-172">總結</span><span class="sxs-lookup"><span data-stu-id="17036-172">Summary</span></span>

<span data-ttu-id="17036-173">開啟重新導向攻擊就會發生重新導向 Url 會當做應用程式的 URL 中的參數傳遞。</span><span class="sxs-lookup"><span data-stu-id="17036-173">Open redirection attacks can occur when redirection URLs are passed as parameters in the URL for an application.</span></span> <span data-ttu-id="17036-174">ASP.NET MVC 3 範本包含程式碼，以防止開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="17036-174">The ASP.NET MVC 3 template includes code to protect against open redirection attacks.</span></span> <span data-ttu-id="17036-175">您可以新增此程式碼進行一些修改 ASP.NET MVC 1.0 和 2 個應用程式。</span><span class="sxs-lookup"><span data-stu-id="17036-175">You can add this code with some modification to ASP.NET MVC 1.0 and 2 applications.</span></span> <span data-ttu-id="17036-176">若要防止開啟重新導向攻擊，登入 ASP.NET 1.0 和 2 個應用程式時，新增 IsLocalUrl() 方法並驗證登入作用中的 returnUrl 參數。</span><span class="sxs-lookup"><span data-stu-id="17036-176">To protect against open redirection attacks when logging into ASP.NET 1.0 and 2 applications, add a IsLocalUrl() method and validate the returnUrl parameter in the LogOn action.</span></span>
