---
uid: mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
title: 提供動態更新使用 AJAX |Microsoft 文件
author: microsoft
description: 步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用 Ajax 為基礎的方式整合至 dinner 詳細資料...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 18700815-8e6c-4489-91af-7ea9dab6529e
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-ajax-to-deliver-dynamic-updates
msc.type: authoredcontent
ms.openlocfilehash: 7cea3ee2ec52261521941efac484e91a53f6310b
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30870176"
---
<a name="use-ajax-to-deliver-dynamic-updates"></a><span data-ttu-id="96909-103">用於 AJAX 傳送動態更新</span><span class="sxs-lookup"><span data-stu-id="96909-103">Use AJAX to Deliver Dynamic Updates</span></span>
====================
<span data-ttu-id="96909-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="96909-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="96909-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="96909-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="96909-106">這是一套免費的步驟 10 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="96909-106">This is step 10 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="96909-107">步驟 10 實作支援登入的使用者 RSVP 興趣參加 dinner，使用整合至 dinner 詳細資料頁面 Ajax 為基礎的方法。</span><span class="sxs-lookup"><span data-stu-id="96909-107">Step 10 implements support for logged-in users to RSVP their interest in attending a dinner, using an Ajax-based approach integrated within the dinner details page.</span></span>
> 
> <span data-ttu-id="96909-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="96909-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-10-ajax-enabling-rsvps-accepts"></a><span data-ttu-id="96909-109">NerdDinner 步驟 10: AJAX 啟用與會者接受</span><span class="sxs-lookup"><span data-stu-id="96909-109">NerdDinner Step 10: AJAX Enabling RSVPs Accepts</span></span>

<span data-ttu-id="96909-110">我們現在實作支援 RSVP 興趣參加 dinner 登入的使用者。</span><span class="sxs-lookup"><span data-stu-id="96909-110">Let's now implement support for logged-in users to RSVP their interest in attending a dinner.</span></span> <span data-ttu-id="96909-111">我們將會啟用此使用 AJAX 為基礎的方式整合至 dinner 詳細資料頁面。</span><span class="sxs-lookup"><span data-stu-id="96909-111">We'll enable this using an AJAX-based approach integrated within the dinner details page.</span></span>

### <a name="indicating-whether-the-user-is-rsvpd"></a><span data-ttu-id="96909-112">指出是否使用者 rsvp 的活動</span><span class="sxs-lookup"><span data-stu-id="96909-112">Indicating whether the user is RSVP'd</span></span>

<span data-ttu-id="96909-113">使用者可以瀏覽 */Dinners/詳細資料 / [識別碼*] 若要查看有關特定 dinner 詳細資料的 URL:</span><span class="sxs-lookup"><span data-stu-id="96909-113">Users can visit the */Dinners/Details/[id*] URL to see details about a particular dinner:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image1.png)

<span data-ttu-id="96909-114">動作方法實作 Details() 就像這樣：</span><span class="sxs-lookup"><span data-stu-id="96909-114">The Details() action method is implemented like so:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample1.cs)]

<span data-ttu-id="96909-115">實作 RSVP 支援我們第一個步驟是加入 「 IsUserRegistered(username)"helper 方法我們 Dinner 物件 （在我們稍早建立的 Dinner.cs 部分類別）。</span><span class="sxs-lookup"><span data-stu-id="96909-115">Our first step to implement RSVP support will be to add an "IsUserRegistered(username)" helper method to our Dinner object (within the Dinner.cs partial class we built earlier).</span></span> <span data-ttu-id="96909-116">這個 helper 方法會傳回 true 或 false，根據是否使用者目前 rsvp 的活動的晚餐：</span><span class="sxs-lookup"><span data-stu-id="96909-116">This helper method returns true or false depending on whether the user is currently RSVP'd for the Dinner:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample2.cs)]

<span data-ttu-id="96909-117">我們 Details.aspx 檢視範本，以顯示適當的訊息，指出使用者是否已註冊或不適用於事件，我們就可以加入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="96909-117">We can then add the following code to our Details.aspx view template to display an appropriate message indicating whether the user is registered or not for the event:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample3.html)]

<span data-ttu-id="96909-118">然後，現在當使用者造訪的 Dinner，針對已註冊他們會看到此訊息：</span><span class="sxs-lookup"><span data-stu-id="96909-118">And now when a user visits a Dinner they are registered for they'll see this message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image2.png)

<span data-ttu-id="96909-119">和當他們在瀏覽的 Dinner，它們未登錄的就會看到下列訊息：</span><span class="sxs-lookup"><span data-stu-id="96909-119">And when they visit a Dinner they are not registered for they'll see the below message:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image3.png)

### <a name="implementing-the-register-action-method"></a><span data-ttu-id="96909-120">實作註冊動作方法</span><span class="sxs-lookup"><span data-stu-id="96909-120">Implementing the Register Action Method</span></span>

<span data-ttu-id="96909-121">讓我們現在加入 RSVP 的晚餐來讓使用者從詳細資料頁面所需的功能。</span><span class="sxs-lookup"><span data-stu-id="96909-121">Let's now add the functionality necessary to enable users to RSVP for a dinner from the details page.</span></span>

<span data-ttu-id="96909-122">若要實作這種情況，我們將建立新的 「 RSVPController 」 類別 \Controllers 目錄上按一下滑鼠右鍵，然後選擇 [新增]-&gt;控制器功能表命令。</span><span class="sxs-lookup"><span data-stu-id="96909-122">To implement this, we'll create a new "RSVPController" class by right-clicking on the \Controllers directory and choosing the Add-&gt;Controller menu command.</span></span>

<span data-ttu-id="96909-123">我們會實作新 Dinner 做為引數所需的識別碼，擷取適當的 Dinner 物件，如果登入使用者目前已註冊，使用者清單中，如果檢查 RSVPController 類別內的 「 註冊 」 動作方法無法將 RSVP 物件加入它們：</span><span class="sxs-lookup"><span data-stu-id="96909-123">We'll implement a "Register" action method within the new RSVPController class that takes an id for a Dinner as an argument, retrieves the appropriate Dinner object, checks to see if the logged-in user is currently in the list of users who have registered for it, and if not adds an RSVP object for them:</span></span>

[!code-csharp[Main](use-ajax-to-deliver-dynamic-updates/samples/sample4.cs)]

<span data-ttu-id="96909-124">請注意，上述我們如何傳回簡單的字串做為動作方法的輸出。</span><span class="sxs-lookup"><span data-stu-id="96909-124">Notice above how we are returning a simple string as the output of the action method.</span></span> <span data-ttu-id="96909-125">我們無法內嵌檢視範本 – 內此訊息，但因為它是很小，我們將只傳回字串訊息類似上述控制器的基底類別等使用 Content() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="96909-125">We could have embedded this message within a view template – but since it is so small we'll just use the Content() helper method on the controller base class and return a string message like above.</span></span>

### <a name="calling-the-rsvpforevent-action-method-using-ajax"></a><span data-ttu-id="96909-126">呼叫使用 AJAX RSVPForEvent 動作方法</span><span class="sxs-lookup"><span data-stu-id="96909-126">Calling the RSVPForEvent Action Method using AJAX</span></span>

<span data-ttu-id="96909-127">我們將使用 AJAX 叫用註冊的動作方法，從我們的詳細資料檢視。</span><span class="sxs-lookup"><span data-stu-id="96909-127">We'll use AJAX to invoke the Register action method from our Details view.</span></span> <span data-ttu-id="96909-128">此實作是很簡單。</span><span class="sxs-lookup"><span data-stu-id="96909-128">Implementing this is pretty easy.</span></span> <span data-ttu-id="96909-129">首先我們會將兩個指令碼程式庫參考：</span><span class="sxs-lookup"><span data-stu-id="96909-129">First we'll add two script library references:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample5.html)]

<span data-ttu-id="96909-130">第一個程式庫參考核心 ASP.NET AJAX 用戶端指令碼程式庫。</span><span class="sxs-lookup"><span data-stu-id="96909-130">The first library references the core ASP.NET AJAX client-side script library.</span></span> <span data-ttu-id="96909-131">這個檔案是大約 24 k （壓縮） 的大小，而且包含核心的用戶端 AJAX 功能。</span><span class="sxs-lookup"><span data-stu-id="96909-131">This file is approximately 24k in size (compressed) and contains core client-side AJAX functionality.</span></span> <span data-ttu-id="96909-132">第二個程式庫包含整合與 ASP.NET MVC 的內建 AJAX helper 方法 （我們將使用） 的公用程式函式。</span><span class="sxs-lookup"><span data-stu-id="96909-132">The second library contains utility functions that integrate with ASP.NET MVC's built-in AJAX helper methods (which we'll use shortly).</span></span>

<span data-ttu-id="96909-133">我們可以更新檢視的範本程式碼我們稍早加入，好讓而不是 outputing 「 您未註冊此事件 」 的訊息，我們改為將連結呈現的推送時執行我們 RSVP 控制站上的我們 RSVPForEvent 動作方法會叫用的 AJAX 呼叫和 RSVPs 使用者：</span><span class="sxs-lookup"><span data-stu-id="96909-133">We can then update the view template code we added earlier so that instead of outputing a "You are not registered for this event" message, we instead render a link that when pushed performs an AJAX call that invokes our RSVPForEvent action method on our RSVP controller and RSVPs the user:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample6.aspx)]

<span data-ttu-id="96909-134">上面使用 Ajax.ActionLink() helper 方法是內建 ASP.NET MVC，類似於 Html.ActionLink() helper 方法不同之處在於而不是執行標準巡覽 exports 動作方法的 AJAX 呼叫在按下連結時。</span><span class="sxs-lookup"><span data-stu-id="96909-134">The Ajax.ActionLink() helper method used above is built-into ASP.NET MVC and is similar to the Html.ActionLink() helper method except that instead of performing a standard navigation it makes an AJAX call to the action method when the link is clicked.</span></span> <span data-ttu-id="96909-135">上方我們會呼叫 「 RSVP"控制站上的"Register"動作方法，並將 DinnerID 做為 「 識別碼 」 參數傳遞給它。</span><span class="sxs-lookup"><span data-stu-id="96909-135">Above we are calling the "Register" action method on the "RSVP" controller and passing the DinnerID as the "id" parameter to it.</span></span> <span data-ttu-id="96909-136">我們傳遞最後一個 AjaxOptions 參數表示我們想要採取的動作方法傳回的內容並更新 HTML &lt;div&gt;其識別碼是"rsvpmsg 」 頁面上的元素。</span><span class="sxs-lookup"><span data-stu-id="96909-136">The final AjaxOptions parameter we are passing indicates that we want to take the content returned from the action method and update the HTML &lt;div&gt; element on the page whose id is "rsvpmsg".</span></span>

<span data-ttu-id="96909-137">然後，現在當使用者瀏覽至 dinner 它們未登錄的但他們會看到它 RSVP 的連結：</span><span class="sxs-lookup"><span data-stu-id="96909-137">And now when a user browses to a dinner they aren't registered for yet they'll see a link to RSVP for it:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image4.png)

<span data-ttu-id="96909-138">如果按下 「 RSVP 這個事件 」 連結它們要進行註冊動作方法的 AJAX 呼叫 RSVP 站上，並在完成時，它們就會看到更新的訊息類似下面的：</span><span class="sxs-lookup"><span data-stu-id="96909-138">If they click the "RSVP for this event" link they'll make an AJAX call to the Register action method on the RSVP controller, and when it completes they'll see an updated message like below:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image5.png)

<span data-ttu-id="96909-139">網路頻寬和真的輕量型進行此 AJAX 呼叫時所涉及的流量。</span><span class="sxs-lookup"><span data-stu-id="96909-139">The network bandwidth and traffic involved when making this AJAX call is really lightweight.</span></span> <span data-ttu-id="96909-140">當使用者按一下"RSVP 這個事件 」 連結時，對小型的 HTTP POST 網路要求 */Dinners/Register/1*看起來類似下面的在網路的 URL:</span><span class="sxs-lookup"><span data-stu-id="96909-140">When the user clicks on the "RSVP for this event" link, a small HTTP POST network request is made to the */Dinners/Register/1* URL that looks like below on the wire:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample7.cmd)]

<span data-ttu-id="96909-141">而只是我們註冊動作方法的回應：</span><span class="sxs-lookup"><span data-stu-id="96909-141">And the response from our Register action method is simply:</span></span>

[!code-console[Main](use-ajax-to-deliver-dynamic-updates/samples/sample8.cmd)]

<span data-ttu-id="96909-142">此輕量型的呼叫是快速，而且即使透過慢速網路運作。</span><span class="sxs-lookup"><span data-stu-id="96909-142">This lightweight call is fast and will work even over a slow network.</span></span>

### <a name="adding-a-jquery-animation"></a><span data-ttu-id="96909-143">新增 jQuery 動畫</span><span class="sxs-lookup"><span data-stu-id="96909-143">Adding a jQuery Animation</span></span>

<span data-ttu-id="96909-144">我們實作 AJAX 功能的運作方式也和 fast。</span><span class="sxs-lookup"><span data-stu-id="96909-144">The AJAX functionality we implemented works well and fast.</span></span> <span data-ttu-id="96909-145">有時還是有可能發生因此快速，不過，使用者可能不會注意到，已用新文字取代 RSVP 連結。</span><span class="sxs-lookup"><span data-stu-id="96909-145">Sometimes it can happen so fast, though, that a user might not notice that the RSVP link has been replaced with new text.</span></span> <span data-ttu-id="96909-146">為了讓結果更明顯我們可以加入簡單的動畫，以強調更新訊息。</span><span class="sxs-lookup"><span data-stu-id="96909-146">To make the outcome a little more obvious we can add a simple animation to draw attention to the update message.</span></span>

<span data-ttu-id="96909-147">預設的 ASP.NET MVC 專案範本包括 jQuery – Microsoft 也支援絕佳 （和相當常用） 開放原始碼 JavaScript 程式庫。</span><span class="sxs-lookup"><span data-stu-id="96909-147">The default ASP.NET MVC project template includes jQuery – an excellent (and very popular) open source JavaScript library that is also supported by Microsoft.</span></span> <span data-ttu-id="96909-148">jQuery 提供許多功能，包括 nice HTML DOM 選取範圍和影響文件庫。</span><span class="sxs-lookup"><span data-stu-id="96909-148">jQuery provides a number of features, including a nice HTML DOM selection and effects library.</span></span>

<span data-ttu-id="96909-149">若要使用 jQuery 我們會先將新增指令碼參考。</span><span class="sxs-lookup"><span data-stu-id="96909-149">To use jQuery we'll first add a script reference to it.</span></span> <span data-ttu-id="96909-150">因為我們將在我們的網站中使用 jQuery 內的許多地方，我們會將新增指令碼參考內我們 Site.master 主版頁面檔案，讓所有頁面可以都使用它。</span><span class="sxs-lookup"><span data-stu-id="96909-150">Because we are going to be using jQuery within a variety of places within our site, we'll add the script reference within our Site.master master page file so that all pages can use it.</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample9.html)]

<span data-ttu-id="96909-151">*提示： 請確定您已安裝可提供 JavaScript 檔案 （包括 jQuery） 更豐富的 intellisense 支援的 VS 2008 sp1 的 JavaScript intellisense hotfix。您可以下載從： http://tinyurl.com/vs2008javascripthotfix*</span><span class="sxs-lookup"><span data-stu-id="96909-151">*Tip: make sure you have installed the JavaScript intellisense hotfix for VS 2008 SP1 that enables richer intellisense support for JavaScript files (including jQuery). You can download it from: http://tinyurl.com/vs2008javascripthotfix*</span></span>

<span data-ttu-id="96909-152">通常使用 JQuery 撰寫的程式碼會使用全域"$ （）"JavaScript 方法，可擷取一或多個 HTML 項目，使用 CSS 選取器。</span><span class="sxs-lookup"><span data-stu-id="96909-152">Code written using JQuery often uses a global "$()" JavaScript method that retrieves one or more HTML elements using a CSS selector.</span></span> <span data-ttu-id="96909-153">例如， <em>$("#rsvpmsg")</em>選取識別碼為 rsvpmsg，任何 HTML 項目時<em>$(".something")</em>會選取所有項目 「 結果 」 CSS 類別名稱。</span><span class="sxs-lookup"><span data-stu-id="96909-153">For example, <em>$("#rsvpmsg")</em> selects any HTML element with the id of rsvpmsg, while <em>$(".something")</em> would select all elements with the "something" CSS class name.</span></span> <span data-ttu-id="96909-154">您也可以撰寫更進階的查詢，例如 「 傳回所有選取的選項按鈕 」 使用 「 類似的選取器查詢： <em>$(「 輸入 [@type= 無線電] [@checked]")</em>。</span><span class="sxs-lookup"><span data-stu-id="96909-154">You can also write more advanced queries like "return all of the checked radio buttons" using a selector query like: <em>$("input[@type=radio][@checked]")</em>.</span></span>

<span data-ttu-id="96909-155">一旦您已選取項目，您可以對它們採取的動作，例如隱藏它們呼叫方法： *$("#rsvpmsg").hide();*</span><span class="sxs-lookup"><span data-stu-id="96909-155">Once you've selected elements, you can call methods on them to take action, like hiding them: *$("#rsvpmsg").hide();*</span></span>

<span data-ttu-id="96909-156">我們 RSVP 的案例，我們會定義簡單的 JavaScript 函式，名為"AnimateRSVPMessage 「 選取 」 rsvpmsg" &lt;div&gt;和以動畫顯示其文字內容的大小。</span><span class="sxs-lookup"><span data-stu-id="96909-156">For our RSVP scenario, we'll define a simple JavaScript function named "AnimateRSVPMessage" that selects the "rsvpmsg" &lt;div&gt; and animates the size of its text content.</span></span> <span data-ttu-id="96909-157">下列程式碼啟動小型的文字，然後原因它增加 400 毫秒的時間範圍內：</span><span class="sxs-lookup"><span data-stu-id="96909-157">The below code starts the text small and then causes it to increase over a 400 milliseconds timeframe:</span></span>

[!code-html[Main](use-ajax-to-deliver-dynamic-updates/samples/sample10.html)]

<span data-ttu-id="96909-158">我們可以再 wire 總我們 AJAX 呼叫成功完成將其名稱傳遞給我們 Ajax.ActionLink() helper 方法之後呼叫這個 JavaScript 函式 (透過 AjaxOptions"OnSuccess 」 事件屬性):</span><span class="sxs-lookup"><span data-stu-id="96909-158">We can then wire-up this JavaScript function to be called after our AJAX call successfully completes by passing its name to our Ajax.ActionLink() helper method (via the AjaxOptions "OnSuccess" event property):</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample11.aspx)]

<span data-ttu-id="96909-159">和現在時按下 「 RSVP 這個事件 」 連結和我們的 AJAX 呼叫已順利完成，內容傳送的訊息後將會以動畫顯示成長大型：</span><span class="sxs-lookup"><span data-stu-id="96909-159">And now when the "RSVP for this event" link is clicked and our AJAX call completes successfully, the content message sent back will animate and grow large:</span></span>

![](use-ajax-to-deliver-dynamic-updates/_static/image6.png)

<span data-ttu-id="96909-160">除了提供 「 OnSuccess 」 事件，AjaxOptions 物件會公開 OnBegin、 OnFailure 和 OnComplete （以及各種不同的其他屬性和有用的選項），您可以處理的事件。</span><span class="sxs-lookup"><span data-stu-id="96909-160">In addition to providing an "OnSuccess" event, the AjaxOptions object exposes OnBegin, OnFailure, and OnComplete events that you can handle (along with a variety of other properties and useful options).</span></span>

### <a name="cleanup---refactor-out-a-rsvp-partial-view"></a><span data-ttu-id="96909-161">清除-重構出 RSVP 的部分檢視</span><span class="sxs-lookup"><span data-stu-id="96909-161">Cleanup - Refactor out a RSVP Partial View</span></span>

<span data-ttu-id="96909-162">我們的詳細資料檢視範本正在啟動以取得更長，哪些加班會更有點難了解。</span><span class="sxs-lookup"><span data-stu-id="96909-162">Our details view template is starting to get a little long, which overtime will make it a little harder to understand.</span></span> <span data-ttu-id="96909-163">為了協助改善程式碼的可讀性，讓我們先完成所建立的部分檢視 – RSVPStatus.ascx – 我們的詳細資料頁面的 RSVP 檢視程式碼的所有封裝。</span><span class="sxs-lookup"><span data-stu-id="96909-163">To help improve the code readability, let's finish up by creating a partial view – RSVPStatus.ascx – that encapsulate all of the RSVP view code for our Details page.</span></span>

<span data-ttu-id="96909-164">我們可以這樣 \Views\Dinners 資料夾上按一下滑鼠右鍵，然後選擇 [新增]-&gt;檢視功能表命令。</span><span class="sxs-lookup"><span data-stu-id="96909-164">We can do this by right-clicking on the \Views\Dinners folder and then choosing the Add-&gt;View menu command.</span></span> <span data-ttu-id="96909-165">我們必須才會建立其強型別 ViewModel Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="96909-165">We'll have it take a Dinner object as its strongly-typed ViewModel.</span></span> <span data-ttu-id="96909-166">我們可以再複製/貼上 RSVP 內容從我們 Details.aspx 檢視它。</span><span class="sxs-lookup"><span data-stu-id="96909-166">We can then copy/paste the RSVP content from our Details.aspx view into it.</span></span>

<span data-ttu-id="96909-167">一旦我們所做的我們也建立另一個部分檢視 – EditAndDeleteLinks.ascx-封裝編輯和刪除連結檢視程式碼。</span><span class="sxs-lookup"><span data-stu-id="96909-167">Once we've done that, let's also create another partial view – EditAndDeleteLinks.ascx - that encapsulates our Edit and Delete link view code.</span></span> <span data-ttu-id="96909-168">我們也會讓它接受以其強型別 」 ViewModel，Dinner 物件和複製/貼上的編輯和刪除的邏輯，從它我們 Details.aspx 檢視。</span><span class="sxs-lookup"><span data-stu-id="96909-168">We'll also have it take a Dinner object as its strongly-typed ViewModel, and copy/paste the Edit and Delete logic from our Details.aspx view into it.</span></span>

<span data-ttu-id="96909-169">詳細資料檢視範本，然後只包含兩個在底部的 Html.RenderPartial() 方法呼叫：</span><span class="sxs-lookup"><span data-stu-id="96909-169">Our details view template can then just include two Html.RenderPartial() method calls at the bottom:</span></span>

[!code-aspx[Main](use-ajax-to-deliver-dynamic-updates/samples/sample12.aspx)]

<span data-ttu-id="96909-170">這讓清除器來讀取與維護的程式碼。</span><span class="sxs-lookup"><span data-stu-id="96909-170">This makes the code cleaner to read and maintain.</span></span>

### <a name="next-step"></a><span data-ttu-id="96909-171">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="96909-171">Next Step</span></span>

<span data-ttu-id="96909-172">我們現在看我們可以更進一步使用 AJAX 和互動式對應支援將我們的應用程式的方法。</span><span class="sxs-lookup"><span data-stu-id="96909-172">Let's now look at how we can use AJAX even further and add interactive mapping support to our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="96909-173">[上一頁](secure-applications-using-authentication-and-authorization.md)
> [下一頁](use-ajax-to-implement-mapping-scenarios.md)</span><span class="sxs-lookup"><span data-stu-id="96909-173">[Previous](secure-applications-using-authentication-and-authorization.md)
[Next](use-ajax-to-implement-mapping-scenarios.md)</span></span>
