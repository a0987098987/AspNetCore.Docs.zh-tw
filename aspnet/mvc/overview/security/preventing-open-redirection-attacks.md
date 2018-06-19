---
uid: mvc/overview/security/preventing-open-redirection-attacks
title: 無法開啟重新導向攻擊 (C#) |Microsoft 文件
author: jongalloway
description: 本教學課程說明如何避免開啟重新導向攻擊，以及在 ASP.NET MVC 應用程式中。 本教學課程將告訴您所做的變更...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/27/2014
ms.topic: article
ms.assetid: 69fb02e0-f5b7-4c35-878c-fa87164fc785
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/preventing-open-redirection-attacks
msc.type: authoredcontent
ms.openlocfilehash: ec1cd1791eb6d32e7c1ea50bc6626929cad2960e
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879669"
---
<a name="preventing-open-redirection-attacks-c"></a><span data-ttu-id="8b9a2-104">無法開啟重新導向攻擊 (C#)</span><span class="sxs-lookup"><span data-stu-id="8b9a2-104">Preventing Open Redirection Attacks (C#)</span></span>
====================
<span data-ttu-id="8b9a2-105">由[Jon Galloway](https://github.com/jongalloway)</span><span class="sxs-lookup"><span data-stu-id="8b9a2-105">by [Jon Galloway](https://github.com/jongalloway)</span></span>

> <span data-ttu-id="8b9a2-106">本教學課程說明如何避免開啟重新導向攻擊，以及在 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-106">This tutorial explains how you can prevent open redirection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="8b9a2-107">本教學課程會討論中 AccountController ASP.NET MVC 3 中所做的變更，並示範如何將這些變更套用在您現有的 ASP.NET MVC 1.0 和 2 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-107">This tutorial discusses the changes that have been made in the AccountController in ASP.NET MVC 3 and demonstrates how you can apply these changes in your existing ASP.NET MVC 1.0 and 2 applications.</span></span>


## <a name="what-is-an-open-redirection-attack"></a><span data-ttu-id="8b9a2-108">什麼是開啟的重新導向攻擊？</span><span class="sxs-lookup"><span data-stu-id="8b9a2-108">What is an Open Redirection Attack?</span></span>

<span data-ttu-id="8b9a2-109">指定透過例如查詢字串或表單資料要求的 URL 重新導向至任何 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-109">Any web application that redirects to a URL that is specified via the request such as the querystring or form data can potentially be tampered with to redirect users to an external, malicious URL.</span></span> <span data-ttu-id="8b9a2-110">這種竄改稱為開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-110">This tampering is called an open redirection attack.</span></span>

<span data-ttu-id="8b9a2-111">每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL，沒有遭到竄改。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-111">Whenever your application logic redirects to a specified URL, you must verify that the redirection URL hasn't been tampered with.</span></span> <span data-ttu-id="8b9a2-112">使用預設 AccountController 在 ASP.NET MVC 1.0 和 ASP.NET MVC 2 的登入很容易就能開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-112">The login used in the default AccountController for both ASP.NET MVC 1.0 and ASP.NET MVC 2 is vulnerable to open redirection attacks.</span></span> <span data-ttu-id="8b9a2-113">幸運的是，所以可以輕鬆地更新您現有的應用程式使用 ASP.NET MVC 3 Preview 更正。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-113">Fortunately, it is easy to update your existing applications to use the corrections from the ASP.NET MVC 3 Preview.</span></span>

<span data-ttu-id="8b9a2-114">若要了解這項弱點，讓我們看看登入重新導向預設的 ASP.NET MVC 2 Web 應用程式專案中的運作方式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-114">To understand the vulnerability, let's look at how the login redirection works in a default ASP.NET MVC 2 Web Application project.</span></span> <span data-ttu-id="8b9a2-115">在此應用程式，嘗試瀏覽具有 [Authorize] 屬性的控制器動作會重新導向未經授權的使用者 /Account/LogOn 檢視。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-115">In this application, attempting to visit a controller action that has the [Authorize] attribute will redirect unauthorized users to the /Account/LogOn view.</span></span> <span data-ttu-id="8b9a2-116">此重新導向至 /Account/LogOn 會包含傳回的 Url 查詢字串參數，以便使用者可以傳回至原本要求的 URL 之後使用者成功登入。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-116">This redirect to /Account/LogOn will include a returnUrl querystring parameter so that the user can be returned to the originally requested URL after they have successfully logged in.</span></span>

<span data-ttu-id="8b9a2-117">在以下的螢幕擷取畫面，我們可以看到，嘗試存取 /Account/ChangePassword 檢視時不會記錄在會導致重新導向至 /Account/LogOn 嗎？ReturnUrl = %2faccount%2fChangePassword %2f。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-117">In the screenshot below, we can see that an attempt to access the /Account/ChangePassword view when not logged in results in a redirect to /Account/LogOn?ReturnUrl=%2fAccount%2fChangePassword%2f.</span></span>

[![](preventing-open-redirection-attacks/_static/image2.png)](preventing-open-redirection-attacks/_static/image1.png)

<span data-ttu-id="8b9a2-118">**圖 01**： 與開啟的重新導向的登入頁面</span><span class="sxs-lookup"><span data-stu-id="8b9a2-118">**Figure 01**: Login page with an open redirection</span></span>

<span data-ttu-id="8b9a2-119">ReturnUrl querystring 參數未經過驗證，因為攻擊者可以修改它的任何 URL 位址中插入參數來進行開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-119">Since the ReturnUrl querystring parameter is not validated, an attacker can modify it to inject any URL address into the parameter to conduct an open redirection attack.</span></span> <span data-ttu-id="8b9a2-120">若要示範這點，我們可以修改的 ReturnUrl 參數[ http://bing.com ](http://bing.com)，因此會產生登入 URL/帳戶/登入？ReturnUrl =<http://www.bing.com/>。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-120">To demonstrate this, we can modify the ReturnUrl parameter to [http://bing.com](http://bing.com), so the resulting login URL will be /Account/LogOn?ReturnUrl=<http://www.bing.com/>.</span></span> <span data-ttu-id="8b9a2-121">在成功登入至站台，我們會重新導向至[ http://bing.com ](http://bing.com)。此重新導向未經過驗證，因為它無法改為指向嘗試欺騙使用者惡意網站。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-121">Upon successfully logging in to the site, we are redirected to [http://bing.com](http://bing.com). Since this redirection is not validated, it could instead point to a malicious site that attempts to trick the user.</span></span>

### <a name="a-more-complex-open-redirection-attack"></a><span data-ttu-id="8b9a2-122">更複雜的開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="8b9a2-122">A more complex Open Redirection Attack</span></span>

<span data-ttu-id="8b9a2-123">開啟的重新導向攻擊會特別危險，因為攻擊者可讓您知道我們正在嘗試登入特定網站，讓我們更容易遭受[網路釣魚攻擊](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx)。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-123">Open redirection attacks are especially dangerous because an attacker knows that we're trying to log into a specific website, which makes us vulnerable to a [phishing attack](https://www.microsoft.com/protect/fraud/phishing/symptoms.aspx).</span></span> <span data-ttu-id="8b9a2-124">例如，攻擊者無法傳送惡意電子郵件給網站使用者嘗試擷取其密碼。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-124">For example, an attacker could send malicious emails to website users in an attempt to capture their passwords.</span></span> <span data-ttu-id="8b9a2-125">讓我們看看 NerdDinner 站台上運作方式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-125">Let's look at how this would work on the NerdDinner site.</span></span> <span data-ttu-id="8b9a2-126">（請注意，已更新即時 NerdDinner 網站以防止開啟重新導向攻擊）。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-126">(Note that the live NerdDinner site has been updated to protect against open redirection attacks.)</span></span>

<span data-ttu-id="8b9a2-127">首先，攻擊者會傳送給我們連結登入頁面上，其中包含重新導向至其偽造的頁面 NerdDinner:</span><span class="sxs-lookup"><span data-stu-id="8b9a2-127">First, an attacker sends us a link to the login page on NerdDinner that includes a redirect to their forged page:</span></span>

[http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn](http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn)

<span data-ttu-id="8b9a2-128">請注意，傳回的 URL 指向 nerddiner.com，遺漏"n"從 word dinner。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-128">Note that the return URL points to nerddiner.com, which is missing an "n" from the word dinner.</span></span> <span data-ttu-id="8b9a2-129">在此範例中，這是攻擊者可以控制的網域。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-129">In this example, this is a domain that the attacker controls.</span></span> <span data-ttu-id="8b9a2-130">當我們存取上面的連結時，我們正在採取合法 NerdDinner.com 登入頁面。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-130">When we access the above link, we're taken to the legitimate NerdDinner.com login page.</span></span>

[![](preventing-open-redirection-attacks/_static/image4.png)](preventing-open-redirection-attacks/_static/image3.png)

<span data-ttu-id="8b9a2-131">**圖 02**: NerdDinner 與開啟的重新導向的登入頁面</span><span class="sxs-lookup"><span data-stu-id="8b9a2-131">**Figure 02**: NerdDinner login page with an open redirection</span></span>

<span data-ttu-id="8b9a2-132">當我們正確登入時，ASP.NET MVC AccountController 登入動作重新導向至我們 returnUrl querystring 參數中指定的 URL。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-132">When we correctly log in, the ASP.NET MVC AccountController's LogOn action redirects us to the URL specified in the returnUrl querystring parameter.</span></span> <span data-ttu-id="8b9a2-133">在此情況下，它是攻擊者有輸入 URL，也就是[ http://nerddiner.com/Account/LogOn ](http://nerddiner.com/Account/LogOn)。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-133">In this case, it's the URL that the attacker has entered, which is [http://nerddiner.com/Account/LogOn](http://nerddiner.com/Account/LogOn).</span></span> <span data-ttu-id="8b9a2-134">我們非常戒慎守望，尤其是因為攻擊者已經確定，請小心就很可能是我們不會注意到這一點，除非其偽造的頁面看起來完全合法的登入頁面。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-134">Unless we're extremely watchful, it's very likely we won't notice this, especially because the attacker has been careful to make sure that their forged page looks exactly like the legitimate login page.</span></span> <span data-ttu-id="8b9a2-135">這個登入頁面將包含錯誤訊息，要求，我們登入一次。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-135">This login page includes an error message requesting that we login again.</span></span> <span data-ttu-id="8b9a2-136">同時，我們必須輸入錯誤密碼。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-136">Clumsy us, we must have mistyped our password.</span></span>

[![](preventing-open-redirection-attacks/_static/image6.png)](preventing-open-redirection-attacks/_static/image5.png)

<span data-ttu-id="8b9a2-137">**圖 03**： 偽造 NerdDinner 登入畫面</span><span class="sxs-lookup"><span data-stu-id="8b9a2-137">**Figure 03**: Forged NerdDinner Login screen</span></span>

<span data-ttu-id="8b9a2-138">當我們重新輸入我們的使用者名稱和密碼時，偽造的登入頁面將儲存的資訊，並將我們傳送至合法 NerdDinner.com 站台。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-138">When we retype our user name and password, the forged login page saves the information and sends us back to the legitimate NerdDinner.com site.</span></span> <span data-ttu-id="8b9a2-139">此時，NerdDinner.com 站台已經驗證，因此偽造的登入頁面可以重新導向至該頁面的直接。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-139">At this point, the NerdDinner.com site has already authenticated us, so the forged login page can redirect directly to that page.</span></span> <span data-ttu-id="8b9a2-140">最終結果是，攻擊者有我們的使用者名稱和密碼，我們不知道，我們已將它提供給它們。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-140">The end result is that the attacker has our user name and password, and we are unaware that we've provided it to them.</span></span>

## <a name="looking-at-the-vulnerable-code-in-the-accountcontroller-logon-action"></a><span data-ttu-id="8b9a2-141">查看 AccountController 登入動作中的弱點程式碼</span><span class="sxs-lookup"><span data-stu-id="8b9a2-141">Looking at the vulnerable code in the AccountController LogOn Action</span></span>

<span data-ttu-id="8b9a2-142">ASP.NET MVC 2 應用程式中的登入動作的程式碼如下所示。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-142">The code for the LogOn action in an ASP.NET MVC 2 application is shown below.</span></span> <span data-ttu-id="8b9a2-143">請注意，成功的登入，控制器會傳回重新導向根據 returnurl 進行。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-143">Note that upon a successful login, the controller returns a redirect to the returnUrl.</span></span> <span data-ttu-id="8b9a2-144">您可以查看針對 returnUrl 參數執行任何驗證。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-144">You can see that no validation is being performed against the returnUrl parameter.</span></span>

<span data-ttu-id="8b9a2-145">**列出 1 – 在 ASP.NET MVC 2 登入動作 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-145">**Listing 1 – ASP.NET MVC 2 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample1.cs)]

<span data-ttu-id="8b9a2-146">現在讓我們看看 ASP.NET MVC 3 登入動作所做的變更。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-146">Now let's look at the changes to the ASP.NET MVC 3 LogOn action.</span></span> <span data-ttu-id="8b9a2-147">此程式碼已變更為呼叫中名為的 System.Web.Mvc.Url 協助程式類別的新方法來驗證 returnUrl 參數`IsLocalUrl()`。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-147">This code has been changed to validate the returnUrl parameter by calling a new method in the System.Web.Mvc.Url helper class named `IsLocalUrl()`.</span></span>

<span data-ttu-id="8b9a2-148">**列出 2 – 在 ASP.NET MVC 3 登入動作 `AccountController.cs`**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-148">**Listing 2 – ASP.NET MVC 3 LogOn action in `AccountController.cs`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample2.cs)]

<span data-ttu-id="8b9a2-149">這個部分已經變更驗證 System.Web.Mvc.Url 協助程式類別中呼叫新方法的傳回 URL 參數`IsLocalUrl()`。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-149">This has been changed to validate the return URL parameter by calling a new method in the System.Web.Mvc.Url helper class, `IsLocalUrl()`.</span></span>

## <a name="protecting-your-aspnet-mvc-10-and-mvc-2-applications"></a><span data-ttu-id="8b9a2-150">保護您的 ASP.NET MVC 1.0 和 MVC 2 應用程式</span><span class="sxs-lookup"><span data-stu-id="8b9a2-150">Protecting Your ASP.NET MVC 1.0 and MVC 2 Applications</span></span>

<span data-ttu-id="8b9a2-151">我們可以利用 ASP.NET MVC 3 變更我們現有的 ASP.NET MVC 1.0 和 2 的應用程式中藉由加入 IsLocalUrl() helper 方法更新登入動作，以驗證 returnUrl 參數。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-151">We can take advantage of the ASP.NET MVC 3 changes in our existing ASP.NET MVC 1.0 and 2 applications by adding the IsLocalUrl() helper method and updating the LogOn action to validate the returnUrl parameter.</span></span>

<span data-ttu-id="8b9a2-152">UrlHelper IsLocalUrl() 方法實際上只呼叫 System.Web.WebPages，做為此驗證中的方法也會使用 ASP.NET Web Pages 應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-152">The UrlHelper IsLocalUrl() method actually just calling into a method in System.Web.WebPages, as this validation is also used by ASP.NET Web Pages applications.</span></span>

<span data-ttu-id="8b9a2-153">**列出 3 – 從 ASP.NET MVC 3 UrlHelper IsLocalUrl() 方法 `class`**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-153">**Listing 3 – The IsLocalUrl() method from the ASP.NET MVC 3 UrlHelper `class`**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample3.cs)]

<span data-ttu-id="8b9a2-154">IsUrlLocalToHost 方法包含的實際驗證邏輯，在列出的 4 所示。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-154">The IsUrlLocalToHost method contains the actual validation logic, as shown in Listing 4.</span></span>

<span data-ttu-id="8b9a2-155">**列出 4 – 自 System.Web.WebPages RequestExtensions 類別 IsUrlLocalToHost() 方法**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-155">**Listing 4 – IsUrlLocalToHost() method from the System.Web.WebPages RequestExtensions class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample4.cs)]

<span data-ttu-id="8b9a2-156">在我們的 ASP.NET MVC 1.0 或 2 的應用程式中，我們會將 IsLocalUrl() 方法新增至 AccountController，但是您可以建議盡可能將它加入至個別的 helper 類別。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-156">In our ASP.NET MVC 1.0 or 2 application, we'll add a IsLocalUrl() method to the AccountController, but you're encouraged to add it to a separate helper class if possible.</span></span> <span data-ttu-id="8b9a2-157">我們會讓兩個小變更 IsLocalUrl() 的 ASP.NET MVC 3 版本，以便將 AccountController 內運作。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-157">We will make two small changes to the ASP.NET MVC 3 version of IsLocalUrl() so that it will work inside the AccountController.</span></span> <span data-ttu-id="8b9a2-158">首先，我們會將它變更從公用方法的私用的方法，因為控制器中的公用方法可以存取為控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-158">First, we'll change it from a public method to a private method, since public methods in controllers can be accessed as controller actions.</span></span> <span data-ttu-id="8b9a2-159">其次，我們將修改檢查 URL 主機應用程式主機的呼叫。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-159">Second, we'll modify the call that checks the URL host against the application host.</span></span> <span data-ttu-id="8b9a2-160">呼叫會用到的本機 RequestContext UrlHelper 類別中的欄位。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-160">That call makes use of a local RequestContext field in the UrlHelper class.</span></span> <span data-ttu-id="8b9a2-161">而不是使用這種情況。RequestContext.HttpContext.Request.Url.Host，我們將使用此。Request.Url.Host。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-161">Instead of using this.RequestContext.HttpContext.Request.Url.Host, we will use this.Request.Url.Host.</span></span> <span data-ttu-id="8b9a2-162">下列程式碼會顯示已修改的 IsLocalUrl() 方法控制器類別搭配使用，在 ASP.NET MVC 1.0 和 2 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-162">The following code shows the modified IsLocalUrl() method for use with a controller class in ASP.NET MVC 1.0 and 2 applications.</span></span>

<span data-ttu-id="8b9a2-163">**列出 5 – IsLocalUrl() 方法，這會修改用於 MVC 控制器類別**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-163">**Listing 5 – IsLocalUrl() method, which is modified for use with an MVC Controller class**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample5.cs)]

<span data-ttu-id="8b9a2-164">既然 IsLocalUrl() 方法是在位置中，我們可以從驗證 returnUrl 參數中，我們登入動作呼叫它，如下列程式碼所示。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-164">Now that the IsLocalUrl() method is in place, we can call it from our LogOn action to validate the returnUrl parameter, as shown in the following code.</span></span>

<span data-ttu-id="8b9a2-165">**列出 6 – 已更新登入方法，這些方法會驗證 returnUrl 參數**</span><span class="sxs-lookup"><span data-stu-id="8b9a2-165">**Listing 6 – Updated LogOn method which validates the returnUrl parameter**</span></span>

[!code-csharp[Main](preventing-open-redirection-attacks/samples/sample6.cs)]

<span data-ttu-id="8b9a2-166">現在我們可以嘗試使用外部的傳回 URL 登入來測試開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-166">Now we can test an open redirection attack by attempting to log in using an external return URL.</span></span> <span data-ttu-id="8b9a2-167">讓我們使用 /Account/LogOn 嗎？ReturnUrl =<http://www.bing.com/>一次。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-167">Let's use /Account/LogOn?ReturnUrl=<http://www.bing.com/> again.</span></span>

[![](preventing-open-redirection-attacks/_static/image8.png)](preventing-open-redirection-attacks/_static/image7.png)

<span data-ttu-id="8b9a2-168">**圖 04**： 測試更新的登入動作</span><span class="sxs-lookup"><span data-stu-id="8b9a2-168">**Figure 04**: Testing the updated LogOn Action</span></span>

<span data-ttu-id="8b9a2-169">已成功登入之後，我們會重新導向至 Home/Index 控制器動作，而不是外部 URL。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-169">After successfully logging in, we are redirected to the Home/Index Controller action rather than the external URL.</span></span>

[![](preventing-open-redirection-attacks/_static/image10.png)](preventing-open-redirection-attacks/_static/image9.png)

<span data-ttu-id="8b9a2-170">**圖 05**： 擊敗開啟重新導向攻擊</span><span class="sxs-lookup"><span data-stu-id="8b9a2-170">**Figure 05**: Open Redirection attack defeated</span></span>

## <a name="summary"></a><span data-ttu-id="8b9a2-171">總結</span><span class="sxs-lookup"><span data-stu-id="8b9a2-171">Summary</span></span>

<span data-ttu-id="8b9a2-172">重新導向 Url 會當做應用程式的 URL 中的參數傳遞，就會發生開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-172">Open redirection attacks can occur when redirection URLs are passed as parameters in the URL for an application.</span></span> <span data-ttu-id="8b9a2-173">範本包含程式碼，以避免 ASP.NET MVC 3 開啟重新導向攻擊。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-173">The ASP.NET MVC 3 template includes code to protect against open redirection attacks.</span></span> <span data-ttu-id="8b9a2-174">您可以加入這個程式碼並進行一些修改 ASP.NET MVC 1.0 和 2 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-174">You can add this code with some modification to ASP.NET MVC 1.0 and 2 applications.</span></span> <span data-ttu-id="8b9a2-175">若要防止開啟重新導向攻擊，登入 ASP.NET 1.0 和 2 的應用程式時，加入 IsLocalUrl() 方法並驗證登入作用中的 returnUrl 參數。</span><span class="sxs-lookup"><span data-stu-id="8b9a2-175">To protect against open redirection attacks when logging into ASP.NET 1.0 and 2 applications, add a IsLocalUrl() method and validate the returnUrl parameter in the LogOn action.</span></span>
