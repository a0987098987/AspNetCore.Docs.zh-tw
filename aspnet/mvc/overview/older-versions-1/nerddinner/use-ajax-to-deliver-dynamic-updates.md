---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: 使用 AJAX 傳送動態更新 |Microsoft Docs
author: microsoft
description: 步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用 Ajax 為基礎的方法整合在 dinner 詳細資料...
ms.author: aspnetcontent
ms.date: 07/27/2010
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 9f11c4c15c0ac9bab8d53b18a4e07be4b864b2c7
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37825197"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="3a45f-103">使用 AJAX 傳送動態更新</span><span class="sxs-lookup"><span data-stu-id="3a45f-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="3a45f-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="3a45f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="3a45f-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="3a45f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="3a45f-106">這是一套免費的步驟 10 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="3a45f-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="3a45f-107">步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用整合在 dinner 詳細資料頁面的 Ajax 架構方法。</span><span class="sxs-lookup"><span data-stu-id="3a45f-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="3a45f-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="3a45f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="3a45f-109">NerdDinner 步驟 10： 啟用像是 AJAX 接受</span><span class="sxs-lookup"><span data-stu-id="3a45f-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="3a45f-110">讓我們現在實作支援 RSVP 興趣參加 dinner 登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="3a45f-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="3a45f-111">我們將會啟用這個選項使用 AJAX 為基礎的方法整合在 dinner 詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="3a45f-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="3a45f-112">指出使用者是否為聚會</span><span class="sxs-lookup"><span data-stu-id="3a45f-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="3a45f-113">使用者可以瀏覽 */Dinners/詳細資料 / [id*] 若要查看特定 dinner 詳細的 URL:</span><span class="sxs-lookup"><span data-stu-id="3a45f-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="3a45f-114">動作方法的實作 Details() 就像這樣：</span><span class="sxs-lookup"><span data-stu-id="3a45f-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="3a45f-115">若要實作 RSVP 支援我們第一個步驟會將"IsUserRegistered(username) 「 協助程式方法新增至我們的 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。</span><span class="sxs-lookup"><span data-stu-id="3a45f-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="3a45f-116">這個 helper 方法會傳回 true 或 false 取決於使用者目前是否聚會吃晚餐：</span><span class="sxs-lookup"><span data-stu-id="3a45f-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="3a45f-117">我們 Details.aspx 檢視範本，以便顯示適當的訊息，指出使用者是否已註冊或不適用於事件，我們就可以加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="3a45f-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="3a45f-118">然後，現在當使用者瀏覽其所註冊的 Dinner 他們會看到此訊息：</span><span class="sxs-lookup"><span data-stu-id="3a45f-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="3a45f-119">以及當他們瀏覽的 Dinner，它們並不註冊就會看到下列訊息：</span><span class="sxs-lookup"><span data-stu-id="3a45f-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="3a45f-120">實作註冊動作方法</span><span class="sxs-lookup"><span data-stu-id="3a45f-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="3a45f-121">讓我們現在新增吃晚餐 RSVP 來啟用使用者，從詳細資料頁面所需的功能。</span><span class="sxs-lookup"><span data-stu-id="3a45f-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="3a45f-122">若要這樣做，我們將建立新的 「 RSVPController"類別的 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [加入]-&gt;控制器功能表命令。</span><span class="sxs-lookup"><span data-stu-id="3a45f-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="3a45f-123">我們將實作會採用吃晚餐做為引數的識別碼，擷取適當的 Dinner 物件，如果登入的使用者目前已註冊，使用者的清單中，如果檢查新 RSVPController 類別內的 [註冊] 動作方法不將它們的 RSVP 物件：</span><span class="sxs-lookup"><span data-stu-id="3a45f-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="3a45f-124">請注意，上例我們要如何傳回簡單的字串做為動作方法的輸出。</span><span class="sxs-lookup"><span data-stu-id="3a45f-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="3a45f-125">我們無法內嵌檢視範本 – 在此訊息，但因為它是很小，我們將只傳回字串之類的訊息上方與控制器的基底類別上使用 Content() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="3a45f-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="3a45f-126">使用 AJAX RSVPForEvent 動作方法的呼叫</span><span class="sxs-lookup"><span data-stu-id="3a45f-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="3a45f-127">我們將使用 AJAX 來叫用註冊動作方法，從我們的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="3a45f-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="3a45f-128">實作此並不難。</span><span class="sxs-lookup"><span data-stu-id="3a45f-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="3a45f-129">首先我們會將新增兩個指令碼程式庫參考：</span><span class="sxs-lookup"><span data-stu-id="3a45f-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="3a45f-130">第一個程式庫參考的核心 ASP.NET AJAX 用戶端指令碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="3a45f-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="3a45f-131">這個檔案是大約 24 k （壓縮） 的大小，且包含核心用戶端 AJAX 功能。</span><span class="sxs-lookup"><span data-stu-id="3a45f-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="3a45f-132">第二個程式庫包含整合與 ASP.NET MVC 的內建 AJAX helper 方法 （我們將使用） 的公用程式函式。</span><span class="sxs-lookup"><span data-stu-id="3a45f-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="3a45f-133">我們可以接著更新檢視範本程式碼，我們稍早新增以便而不是輸出的 「 您未註冊此事件 」 的訊息，我們改為呈現連結的推入執行叫用我們的 RSVP 控制站上我們 RSVPForEvent 動作方法的 AJAX 呼叫和 RSVPs 使用者：</span><span class="sxs-lookup"><span data-stu-id="3a45f-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="3a45f-134">上述範例中使用 Ajax.ActionLink() helper 方法是內建於 ASP.NET MVC，並為 Html.ActionLink() 協助程式方法類似，不同之處在於而不是執行標準的巡覽它進行 AJAX 呼叫動作方法時按下連結。</span><span class="sxs-lookup"><span data-stu-id="3a45f-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="3a45f-135">以上所述，我們會呼叫 「 RSVP 」 控制站上的 [註冊] 動作方法，並將 DinnerID 做為 「 識別碼 」 參數傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="3a45f-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="3a45f-136">我們將傳遞的最終 AjaxOptions 參數可讓您表示我們想要採取的動作方法傳回的內容，並更新 HTML &lt;div&gt;其識別碼是"rsvpmsg 」 頁面上的項目。</span><span class="sxs-lookup"><span data-stu-id="3a45f-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="3a45f-137">然後，現在當使用者瀏覽至 dinner 它們未註冊，但他們會看到它的 RSVP 的連結：</span><span class="sxs-lookup"><span data-stu-id="3a45f-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="3a45f-138">如果使用者按一下 「 此事件的 RSVP"連結就會一目了然的註冊動作方法的 AJAX 呼叫 RSVP 站上，並完成時就會看到更新的訊息如下所示：</span><span class="sxs-lookup"><span data-stu-id="3a45f-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="3a45f-139">網路頻寬和流量時發出此 AJAX 呼叫是真正的輕量的相關。</span><span class="sxs-lookup"><span data-stu-id="3a45f-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="3a45f-140">當使用者按一下 」 的這個事件的 RSVP 」 連結時，對小型的 HTTP POST 網路要求 */Dinners/Register/1*在網路看起來如下所示的 URL:</span><span class="sxs-lookup"><span data-stu-id="3a45f-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="3a45f-141">與我們註冊，動作方法的回應就是：</span><span class="sxs-lookup"><span data-stu-id="3a45f-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="3a45f-142">這個輕量型的呼叫很快速，而且即使是在較慢的網路運作。</span><span class="sxs-lookup"><span data-stu-id="3a45f-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="3a45f-143">新增 jQuery 動畫</span><span class="sxs-lookup"><span data-stu-id="3a45f-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="3a45f-144">我們所實作的 AJAX 功能的運作方式也和快速。</span><span class="sxs-lookup"><span data-stu-id="3a45f-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="3a45f-145">有時可能因此快速、 但，使用者可能不會注意到新的文字，已取代的 RSVP 連結。</span><span class="sxs-lookup"><span data-stu-id="3a45f-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="3a45f-146">若要讓結果更明顯，我們可以加入簡單的動畫，讓大家注意已經更新訊息。</span><span class="sxs-lookup"><span data-stu-id="3a45f-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="3a45f-147">預設的 ASP.NET MVC 專案範本包含 jQuery-Microsoft 也支援的絕佳的 （和非常受歡迎的） 的開放原始碼 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="3a45f-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="3a45f-148">jQuery 提供許多功能，包括美觀的 HTML DOM 選取項目和效果庫。</span><span class="sxs-lookup"><span data-stu-id="3a45f-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="3a45f-149">若要使用 jQuery 中，我們將第一次新增對它的指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="3a45f-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="3a45f-150">因為我們要在我們的網站內使用 jQuery 中的許多地方，我們將新增的指令碼參考，我們 Site.master 主版頁面檔案內，讓所有頁面可以都使用它即可。</span><span class="sxs-lookup"><span data-stu-id="3a45f-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="3a45f-151">*秘訣︰ 請確定您已安裝 VS 2008 sp1 可讓 JavaScript 檔案 （包括 jQuery） 更豐富的 intellisense 支援的 JavaScript intellisense hotfix。您可以下載從： http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="3a45f-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="3a45f-152">通常使用 JQuery 撰寫的程式碼會使用全域"$ （）"JavaScript 方法，可擷取一或多個 HTML 項目，使用 CSS 選取器。</span><span class="sxs-lookup"><span data-stu-id="3a45f-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="3a45f-153">例如， <em>$("#rsvpmsg")</em>選取識別碼 rsvpmsg，為任何 HTML 項目時<em>$(".something")</em>會選取所有項目與 「 某事物 」 CSS 類別名稱。</span><span class="sxs-lookup"><span data-stu-id="3a45f-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="3a45f-154">您也可以撰寫更進階的查詢，像是 「 請傳回所有選取的選項按鈕的項目 」 使用選取器的查詢，例如： <em>$("輸入 [@type= technet 收音機] [@checked]")</em>。</span><span class="sxs-lookup"><span data-stu-id="3a45f-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="3a45f-155">一旦您已選取項目，您可以呼叫方法對其採取動作，例如隱藏它們： *$(「 #rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="3a45f-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="3a45f-156">我們的 RSVP 案例中，我們會定義名為"AnimateRSVPMessage 「 選取 」 rsvpmsg 」 簡單的 JavaScript 函式&lt;div&gt;並以動畫顯示其文字內容的大小。</span><span class="sxs-lookup"><span data-stu-id="3a45f-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="3a45f-157">下列程式碼會啟動小型的文字，然後會導致它在 400 毫秒的時間範圍內增加：</span><span class="sxs-lookup"><span data-stu-id="3a45f-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="3a45f-158">我們可以再網路總 AJAX 呼叫成功完成將其名稱傳遞至我們 Ajax.ActionLink() 協助程式方法之後呼叫這個 JavaScript 函式 (透過 AjaxOptions"OnSuccess"事件屬性):</span><span class="sxs-lookup"><span data-stu-id="3a45f-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="3a45f-159">而現在時按下 」 的這個事件的 RSVP 」 連結，和我們的 AJAX 呼叫已順利完成，內容的訊息傳送後會建立動畫並變大：</span><span class="sxs-lookup"><span data-stu-id="3a45f-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="3a45f-160">除了提供"OnSuccess"事件，AjaxOptions 物件會公開 OnBegin、 OnFailure 和 OnComplete （以及各種不同的其他屬性和有用的選項），您可以處理的事件。</span><span class="sxs-lookup"><span data-stu-id="3a45f-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="3a45f-161">清除-回覆的部分檢視的重構</span><span class="sxs-lookup"><span data-stu-id="3a45f-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="3a45f-162">詳細資料檢視範本開始有點長，哪些加班將它有點難理解。</span><span class="sxs-lookup"><span data-stu-id="3a45f-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="3a45f-163">若要改善程式碼的可讀性，讓我們完成藉由建立部分檢視 – RSVPStatus.ascx – 封裝所有我們詳細資料 頁面的 RSVP 檢視程式碼。</span><span class="sxs-lookup"><span data-stu-id="3a45f-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="3a45f-164">我們可以執行這項操作 \Views\Dinners 資料夾上按一下滑鼠右鍵，然後選擇 [加入]-&gt;檢視功能表命令。</span><span class="sxs-lookup"><span data-stu-id="3a45f-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="3a45f-165">我們必須採用 Dinner 物件作為其強型別 ViewModel 它。</span><span class="sxs-lookup"><span data-stu-id="3a45f-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="3a45f-166">我們可以接著複製/貼上的 RSVP 內容從我們的 Details.aspx 檢視到其中。</span><span class="sxs-lookup"><span data-stu-id="3a45f-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="3a45f-167">一旦我們所做的我們也建立另一個部分檢視 – EditAndDeleteLinks.ascx-封裝編輯和刪除連結檢視程式碼。</span><span class="sxs-lookup"><span data-stu-id="3a45f-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="3a45f-168">我們也會採用 Dinner 物件作為其強型別的 ViewModel，並複製/貼上的編輯和刪除的邏輯，從我們的 Details.aspx 檢視到其中。</span><span class="sxs-lookup"><span data-stu-id="3a45f-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="3a45f-169">我們的詳細資料檢視範本可以，則只包含兩個在底部的 Html.RenderPartial() 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="3a45f-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="3a45f-170">這會讓程式碼更容易閱讀及維護。</span><span class="sxs-lookup"><span data-stu-id="3a45f-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="3a45f-171">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="3a45f-171">Next Step</span></span>

<span data-ttu-id="3a45f-172">現在來看看如何我們可以使用 AJAX，更進一步，並加入我們的應用程式中的互動式對應支援。</span><span class="sxs-lookup"><span data-stu-id="3a45f-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="3a45f-173">[上一頁](secure-applications-using-authentication-and-authorization.md)
> [下一頁](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="3a45f-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
