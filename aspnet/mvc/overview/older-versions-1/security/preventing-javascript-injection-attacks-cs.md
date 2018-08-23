---
uid: mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
title: 防止 JavaScript 插入式攻擊 (C#) |Microsoft Docs
author: StephenWalther
description: 防止 JavaScript 插入式攻擊和跨網站指令攻擊發生給您。 在本教學課程中，Stephen Walther 會說明如何輕鬆地 de...
ms.author: riande
ms.date: 08/19/2008
ms.assetid: d0136da6-81a4-4815-b002-baa84744c09e
msc.legacyurl: /mvc/overview/older-versions-1/security/preventing-javascript-injection-attacks-cs
msc.type: authoredcontent
ms.openlocfilehash: 77d0f0346e9eff756cd74c64c310918f3c367ab1
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833173"
---
<a name="preventing-javascript-injection-attacks-c"></a><span data-ttu-id="2b18f-104">防止 JavaScript 插入式攻擊 (C#)</span><span class="sxs-lookup"><span data-stu-id="2b18f-104">Preventing JavaScript Injection Attacks (C#)</span></span>
====================
<span data-ttu-id="2b18f-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="2b18f-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

[<span data-ttu-id="2b18f-106">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="2b18f-106">Download PDF</span></span>](http://download.microsoft.com/download/8/4/8/84843d8d-1575-426c-bcb5-9d0c42e51416/ASPNET_MVC_Tutorial_06_CS.pdf)

> <span data-ttu-id="2b18f-107">防止 JavaScript 插入式攻擊和跨網站指令攻擊發生給您。</span><span class="sxs-lookup"><span data-stu-id="2b18f-107">Prevent JavaScript Injection Attacks and Cross-Site Scripting Attacks from happening to you.</span></span> <span data-ttu-id="2b18f-108">在本教學課程中，Stephen Walther 會說明如何輕鬆地擊敗這些類型的 HTML 編碼內容的攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-108">In this tutorial, Stephen Walther explains how you can easily defeat these types of attacks by HTML encoding your content.</span></span>


<span data-ttu-id="2b18f-109">本教學課程的目標是要說明如何防止 JavaScript 插入式攻擊，以及在您的 ASP.NET MVC 應用程式中。</span><span class="sxs-lookup"><span data-stu-id="2b18f-109">The goal of this tutorial is to explain how you can prevent JavaScript injection attacks in your ASP.NET MVC applications.</span></span> <span data-ttu-id="2b18f-110">本教學課程會討論兩種方法來保護您的網站，JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-110">This tutorial discusses two approaches to defending your website against a JavaScript injection attack.</span></span> <span data-ttu-id="2b18f-111">您了解如何為您顯示的資料進行編碼，以防止 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-111">You learn how to prevent JavaScript injection attacks by encoding the data that you display.</span></span> <span data-ttu-id="2b18f-112">您也了解如何防止 JavaScript 插入式攻擊的編碼方式表示您接受的資料。</span><span class="sxs-lookup"><span data-stu-id="2b18f-112">You also learn how to prevent JavaScript injection attacks by encoding the data that you accept.</span></span>

## <a name="what-is-a-javascript-injection-attack"></a><span data-ttu-id="2b18f-113">什麼是 JavaScript 插入式攻擊？</span><span class="sxs-lookup"><span data-stu-id="2b18f-113">What is a JavaScript Injection Attack?</span></span>

<span data-ttu-id="2b18f-114">當您接受使用者輸入，並重新顯示使用者輸入時，您就會開啟您的網站以 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-114">Whenever you accept user input and redisplay the user input, you open your website to JavaScript injection attacks.</span></span> <span data-ttu-id="2b18f-115">讓我們來檢查具象的應用程式開啟 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-115">Let's examine a concrete application that is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="2b18f-116">假設您已建立客戶的意見反應網站 （請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="2b18f-116">Imagine that you have created a customer feedback website (see Figure 1).</span></span> <span data-ttu-id="2b18f-117">客戶可以瀏覽網站，並使用您的產品使用者體驗上輸入意見反應。</span><span class="sxs-lookup"><span data-stu-id="2b18f-117">Customers can visit the website and enter feedback on their experience using your products.</span></span> <span data-ttu-id="2b18f-118">當客戶提交他們的意見反應時，意見反應會重新顯示在意見反應 頁面上。</span><span class="sxs-lookup"><span data-stu-id="2b18f-118">When a customer submits their feedback, the feedback is redisplayed on the feedback page.</span></span>


<span data-ttu-id="2b18f-119">[![客戶意見反應網站](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="2b18f-119">[![Customer Feedback Website](preventing-javascript-injection-attacks-cs/_static/image2.png)](preventing-javascript-injection-attacks-cs/_static/image1.png)</span></span>

<span data-ttu-id="2b18f-120">**圖 01**： 客戶意見反應網站 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image3.png))</span><span class="sxs-lookup"><span data-stu-id="2b18f-120">**Figure 01**: Customer Feedback Website ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image3.png))</span></span>


<span data-ttu-id="2b18f-121">客戶意見反應網站使用`controller`列表 1 中。</span><span class="sxs-lookup"><span data-stu-id="2b18f-121">The customer feedback website uses the `controller` in Listing 1.</span></span> <span data-ttu-id="2b18f-122">這`controller`包含名為兩個動作`Index()`和`Create()`。</span><span class="sxs-lookup"><span data-stu-id="2b18f-122">This `controller` contains two actions named `Index()` and `Create()`.</span></span>

<span data-ttu-id="2b18f-123">**列表 1 – `HomeController.cs`**</span><span class="sxs-lookup"><span data-stu-id="2b18f-123">**Listing 1 – `HomeController.cs`**</span></span>

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample1.cs)]

<span data-ttu-id="2b18f-124">`Index()`方法顯示`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="2b18f-124">The `Index()` method displays the `Index` view.</span></span> <span data-ttu-id="2b18f-125">此方法會傳遞所有過去的客戶意見反應，以`Index`（使用 LINQ to SQL 查詢），從資料庫擷取意見反應的檢視。</span><span class="sxs-lookup"><span data-stu-id="2b18f-125">This method passes all of the previous customer feedback to the `Index` view by retrieving the feedback from the database (using a LINQ to SQL query).</span></span>

<span data-ttu-id="2b18f-126">`Create()`方法會建立新的意見項目，並將它加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="2b18f-126">The `Create()` method creates a new Feedback item and adds it to the database.</span></span> <span data-ttu-id="2b18f-127">客戶輸入表單中的訊息會傳遞至`Create()`訊息參數中的方法。</span><span class="sxs-lookup"><span data-stu-id="2b18f-127">The message that the customer enters in the form is passed to the `Create()` method in the message parameter.</span></span> <span data-ttu-id="2b18f-128">建立意見項目並將訊息指派給意見項目`Message`屬性。</span><span class="sxs-lookup"><span data-stu-id="2b18f-128">A Feedback item is created and the message is assigned to the Feedback item's `Message` property.</span></span> <span data-ttu-id="2b18f-129">意見項目提交至資料庫`DataContext.SubmitChanges()`方法呼叫。</span><span class="sxs-lookup"><span data-stu-id="2b18f-129">The Feedback item is submitted to the database with the `DataContext.SubmitChanges()` method call.</span></span> <span data-ttu-id="2b18f-130">最後，訪客會被重新導向回到`Index`檢視顯示所有意見反應。</span><span class="sxs-lookup"><span data-stu-id="2b18f-130">Finally, the visitor is redirected back to the `Index` view where all of the feedback is displayed.</span></span>

<span data-ttu-id="2b18f-131">`Index`檢視包含在 列表 2。</span><span class="sxs-lookup"><span data-stu-id="2b18f-131">The `Index` view is contained in Listing 2.</span></span>

<span data-ttu-id="2b18f-132">**列表 2 – `Index.aspx`**</span><span class="sxs-lookup"><span data-stu-id="2b18f-132">**Listing 2 – `Index.aspx`**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample2.aspx)]

<span data-ttu-id="2b18f-133">`Index`檢視有兩個區段。</span><span class="sxs-lookup"><span data-stu-id="2b18f-133">The `Index` view has two sections.</span></span> <span data-ttu-id="2b18f-134">上方區段會包含實際的客戶意見反應表單。</span><span class="sxs-lookup"><span data-stu-id="2b18f-134">The top section contains the actual customer feedback form.</span></span> <span data-ttu-id="2b18f-135">底部區段包含一個 For...每個迴圈，迴圈的所有先前的客戶意見反應項目，並顯示每個意見項目 EntryDate 和訊息屬性。</span><span class="sxs-lookup"><span data-stu-id="2b18f-135">The bottom section contains a For..Each loop that loops through all of the previous customer feedback items and displays the EntryDate and Message properties for each feedback item.</span></span>

<span data-ttu-id="2b18f-136">客戶意見反應網站是一個簡單的網站。</span><span class="sxs-lookup"><span data-stu-id="2b18f-136">The customer feedback website is a simple website.</span></span> <span data-ttu-id="2b18f-137">不幸的是，網站會開啟 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-137">Unfortunately, the website is open to JavaScript injection attacks.</span></span>

<span data-ttu-id="2b18f-138">假設在客戶的意見反應表單中輸入下列文字：</span><span class="sxs-lookup"><span data-stu-id="2b18f-138">Imagine that you enter the following text into the customer feedback form:</span></span>

[!code-html[Main](preventing-javascript-injection-attacks-cs/samples/sample3.html)]

<span data-ttu-id="2b18f-139">這段文字表示的 JavaScript 指令碼，會顯示警示訊息方塊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-139">This text represents a JavaScript script that displays an alert message box.</span></span> <span data-ttu-id="2b18f-140">有人將此指令碼提交至意見反應之後形成，訊息<em>Boo ！</em>會出現時的任何人造訪客戶意見反應網站未來 （請參閱 圖 2）。</span><span class="sxs-lookup"><span data-stu-id="2b18f-140">After someone submits this script into the feedback form, the message <em>Boo!</em>will appear whenever anyone visits the customer feedback website in the future (see Figure 2).</span></span>


<span data-ttu-id="2b18f-141">[![JavaScript 插入式攻擊](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)</span><span class="sxs-lookup"><span data-stu-id="2b18f-141">[![JavaScript Injection](preventing-javascript-injection-attacks-cs/_static/image5.png)](preventing-javascript-injection-attacks-cs/_static/image4.png)</span></span>

<span data-ttu-id="2b18f-142">**圖 02**: JavaScript 插入式攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="2b18f-142">**Figure 02**: JavaScript Injection ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image6.png))</span></span>


<span data-ttu-id="2b18f-143">現在，您 JavaScript 插入式攻擊的初始回應可能會讓您失去動力。</span><span class="sxs-lookup"><span data-stu-id="2b18f-143">Now, your initial response to JavaScript injection attacks might be apathy.</span></span> <span data-ttu-id="2b18f-144">您可能會認為，JavaScript 插入式攻擊是只是一種*竄改*攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-144">You might think that JavaScript injection attacks are simply a type of *defacement* attack.</span></span> <span data-ttu-id="2b18f-145">您可能會認為，沒有人可以任何動作真正邪惡認可 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-145">You might believe that no one can do anything truly evil by committing a JavaScript injection attack.</span></span>

<span data-ttu-id="2b18f-146">不幸的是，駭客就可以進行一些真的很邪惡的項目，藉由插入網站中的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b18f-146">Unfortunately, a hacker can do some really, really evil things by injecting JavaScript into a website.</span></span> <span data-ttu-id="2b18f-147">若要執行跨網站指令碼 (XSS) 攻擊，您可以使用 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-147">You can use a JavaScript injection attack to perform a Cross-Site Scripting (XSS) attack.</span></span> <span data-ttu-id="2b18f-148">在跨網站指令攻擊中，您可以竊取機密的使用者資訊，並將資訊傳送給另一個網站。</span><span class="sxs-lookup"><span data-stu-id="2b18f-148">In a Cross-Site Scripting attack, you steal confidential user information and send the information to another website.</span></span>

<span data-ttu-id="2b18f-149">比方說，駭客就可以使用 JavaScript 插入式攻擊竊取其他使用者的瀏覽器 cookie 的值。</span><span class="sxs-lookup"><span data-stu-id="2b18f-149">For example, a hacker can use a JavaScript injection attack to steal the values of browser cookies from other users.</span></span> <span data-ttu-id="2b18f-150">機密資訊，例如密碼、 信用卡號碼或身分證號碼 – 儲存在瀏覽器 cookie，如果駭客可以竊取這項資訊使用 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-150">If sensitive information -- such as passwords, credit card numbers, or social security numbers – is stored in the browser cookies, then a hacker can use a JavaScript injection attack to steal this information.</span></span> <span data-ttu-id="2b18f-151">或者，如果使用者輸入具有以 JavaScript 攻擊遭入侵的頁面所包含的表單欄位中的機密資訊，然後駭客可以使用插入的 JavaScript 來抓取表單資料，並將它傳送給另一個網站。</span><span class="sxs-lookup"><span data-stu-id="2b18f-151">Or, if a user enters sensitive information in a form field contained in a page that has been compromised with a JavaScript attack, then the hacker can use the injected JavaScript to grab the form data and send it to another website.</span></span>

<span data-ttu-id="2b18f-152">*請害怕*。</span><span class="sxs-lookup"><span data-stu-id="2b18f-152">*Please be scared*.</span></span> <span data-ttu-id="2b18f-153">重視 JavaScript 插入式攻擊，並保護您的使用者機密資訊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-153">Take JavaScript injection attacks seriously and protect your user's confidential information.</span></span> <span data-ttu-id="2b18f-154">在接下來兩節中，我們會討論兩種技術可供您保護您的 ASP.NET MVC 應用程式，從 JavaScript 插入式攻擊。</span><span class="sxs-lookup"><span data-stu-id="2b18f-154">In the next two sections, we discuss two techniques that you can use to defend your ASP.NET MVC applications from JavaScript injection attacks.</span></span>

## <a name="approach-1-html-encode-in-the-view"></a><span data-ttu-id="2b18f-155">方法 #1： 在檢視中的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="2b18f-155">Approach #1: HTML Encode in the View</span></span>

<span data-ttu-id="2b18f-156">其中一個簡單的方法，防止 JavaScript 插入式攻擊是以 HTML 編碼重新顯示在檢視中的資料時，網站使用者所輸入的任何資料。</span><span class="sxs-lookup"><span data-stu-id="2b18f-156">One easy method of preventing JavaScript injection attacks is to HTML encode any data entered by website users when you redisplay the data in a view.</span></span> <span data-ttu-id="2b18f-157">已更新`Index`列表 3 中的檢視會遵循這種方法。</span><span class="sxs-lookup"><span data-stu-id="2b18f-157">The updated `Index` view in Listing 3 follows this approach.</span></span>

<span data-ttu-id="2b18f-158">**列表 3 – `Index.aspx` (編碼的 HTML)**</span><span class="sxs-lookup"><span data-stu-id="2b18f-158">**Listing 3 – `Index.aspx` (HTML Encoded)**</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample4.aspx)]

<span data-ttu-id="2b18f-159">請注意，值`feedback.Message`是 HTML 編碼之前的值會顯示為下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="2b18f-159">Notice that the value of `feedback.Message` is HTML encoded before the value is displayed with the following code:</span></span>

[!code-aspx[Main](preventing-javascript-injection-attacks-cs/samples/sample5.aspx)]

<span data-ttu-id="2b18f-160">什麼平均值為 HTML 編碼字串？</span><span class="sxs-lookup"><span data-stu-id="2b18f-160">What does it mean to HTML encode a string?</span></span> <span data-ttu-id="2b18f-161">當您以 HTML 編碼字串時，危險字元，例如`<`並`>`這類的 HTML 實體參考會取代`&lt;`和`&gt;`。</span><span class="sxs-lookup"><span data-stu-id="2b18f-161">When you HTML encode a string, dangerous characters such as `<` and `>` are replaced by HTML entity references such as `&lt;` and `&gt;`.</span></span> <span data-ttu-id="2b18f-162">因此當字串`<script>alert("Boo!")</script>`是 HTML 編碼，將它轉換成`&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`。</span><span class="sxs-lookup"><span data-stu-id="2b18f-162">So when the string `<script>alert("Boo!")</script>` is HTML encoded, it gets converted to `&lt;script&gt;alert(&quot;Boo!&quot;)&lt;/script&gt;`.</span></span> <span data-ttu-id="2b18f-163">做為解譯的瀏覽器時，JavaScript 指令碼不會再執行編碼的字串。</span><span class="sxs-lookup"><span data-stu-id="2b18f-163">The encoded string no longer executes as a JavaScript script when interpreted by a browser.</span></span> <span data-ttu-id="2b18f-164">相反地，您可以取得無害的頁面 [圖 3] 中。</span><span class="sxs-lookup"><span data-stu-id="2b18f-164">Instead, you get the harmless page in Figure 3.</span></span>


<span data-ttu-id="2b18f-165">[![失效的 JavaScript 攻擊](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="2b18f-165">[![Defeated JavaScript Attack](preventing-javascript-injection-attacks-cs/_static/image8.png)](preventing-javascript-injection-attacks-cs/_static/image7.png)</span></span>

<span data-ttu-id="2b18f-166">**圖 03**： 讓 JavaScript 攻擊 ([按一下以檢視完整大小的影像](preventing-javascript-injection-attacks-cs/_static/image9.png))</span><span class="sxs-lookup"><span data-stu-id="2b18f-166">**Figure 03**: Defeated JavaScript Attack ([Click to view full-size image](preventing-javascript-injection-attacks-cs/_static/image9.png))</span></span>


<span data-ttu-id="2b18f-167">請注意，在`Index`檢視中 列表 3 的值`feedback.Message`編碼。</span><span class="sxs-lookup"><span data-stu-id="2b18f-167">Notice that in the `Index` view in Listing 3 only the value of `feedback.Message` is encoded.</span></span> <span data-ttu-id="2b18f-168">值`feedback.EntryDate`就未編碼。</span><span class="sxs-lookup"><span data-stu-id="2b18f-168">The value of `feedback.EntryDate` is not encoded.</span></span> <span data-ttu-id="2b18f-169">您只需要將使用者輸入的資料進行編碼。</span><span class="sxs-lookup"><span data-stu-id="2b18f-169">You only need to encode data entered by a user.</span></span> <span data-ttu-id="2b18f-170">在控制器中產生 EntryDate 的值，因為您不需要為 HTML 編碼此值。</span><span class="sxs-lookup"><span data-stu-id="2b18f-170">Because the value of EntryDate was generated in the controller, you don't need to HTML encode this value.</span></span>

## <a name="approach-2-html-encode-in-the-controller"></a><span data-ttu-id="2b18f-171">方法 2： 在控制器中的 HTML 編碼</span><span class="sxs-lookup"><span data-stu-id="2b18f-171">Approach #2: HTML Encode in the Controller</span></span>

<span data-ttu-id="2b18f-172">而不是 HTML 編碼的資料，當您在檢視中顯示資料時，您可以 HTML 編碼之前您送出至資料庫資料的資料。</span><span class="sxs-lookup"><span data-stu-id="2b18f-172">Instead of HTML encoding data when you display the data in a view, you can HTML encode the data just before you submit the data to the database.</span></span> <span data-ttu-id="2b18f-173">此第二種方法會是`controller`列表 4 中。</span><span class="sxs-lookup"><span data-stu-id="2b18f-173">This second approach is taken in the case of the `controller` in Listing 4.</span></span>

<span data-ttu-id="2b18f-174">**列表 4 – `HomeController.cs` (編碼的 HTML)**</span><span class="sxs-lookup"><span data-stu-id="2b18f-174">**Listing 4 – `HomeController.cs` (HTML Encoded)**</span></span>

[!code-csharp[Main](preventing-javascript-injection-attacks-cs/samples/sample6.cs)]

<span data-ttu-id="2b18f-175">請注意，訊息的值是 HTML 編碼之前在該值提交至資料庫內`Create()`動作。</span><span class="sxs-lookup"><span data-stu-id="2b18f-175">Notice that the value of Message is HTML encoded before the value is submitted to the database within the `Create()` action.</span></span> <span data-ttu-id="2b18f-176">當訊息會重新顯示在檢視中時，訊息是 HTML 編碼，而且不會執行任何插入訊息中的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="2b18f-176">When the Message is redisplayed in the view, the Message is HTML encoded and any JavaScript injected in the Message is not executed.</span></span>

<span data-ttu-id="2b18f-177">一般而言，您應該偏好使用透過此第二種方法在本教學課程所討論的第一個方法。</span><span class="sxs-lookup"><span data-stu-id="2b18f-177">Typically, you should favor the first approach discussed in this tutorial over this second approach.</span></span> <span data-ttu-id="2b18f-178">此第二個方法的問題是，您得到 HTML 編碼的資料在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="2b18f-178">The problem with this second approach is that you end up with HTML encoded data in your database.</span></span> <span data-ttu-id="2b18f-179">換句話說，您資料庫的資料會遇到一些瘋狂的尋找字元時修改。</span><span class="sxs-lookup"><span data-stu-id="2b18f-179">In other words, your database data is dirtied with funny looking characters.</span></span>

<span data-ttu-id="2b18f-180">為什麼這是不正確？</span><span class="sxs-lookup"><span data-stu-id="2b18f-180">Why is this bad?</span></span> <span data-ttu-id="2b18f-181">如果您需要在 web 網頁以外的項目中顯示的資料庫資料，您會遇到問題。</span><span class="sxs-lookup"><span data-stu-id="2b18f-181">If you ever need to display the database data in something other than a web page, then you will have problems.</span></span> <span data-ttu-id="2b18f-182">例如，您可以在 Windows Forms 應用程式中不再那麼容易顯示資料。</span><span class="sxs-lookup"><span data-stu-id="2b18f-182">For example, you can no longer easily display the data in a Windows Forms application.</span></span>

## <a name="summary"></a><span data-ttu-id="2b18f-183">總結</span><span class="sxs-lookup"><span data-stu-id="2b18f-183">Summary</span></span>

<span data-ttu-id="2b18f-184">本教學課程的目的是要把您唬住有關 JavaScript 插入式攻擊的潛在客戶。</span><span class="sxs-lookup"><span data-stu-id="2b18f-184">The purpose of this tutorial was to scare you about the prospect of a JavaScript injection attack.</span></span> <span data-ttu-id="2b18f-185">本教學課程中討論過防禦您的 ASP.NET MVC 應用程式，防止 JavaScript 插入式攻擊的兩種方法： 可以是 HTML 編碼使用者提交資料的檢視，或者您可以 HTML 編碼使用者提交控制器中的資料。</span><span class="sxs-lookup"><span data-stu-id="2b18f-185">This tutorial discussed two approaches for defending your ASP.NET MVC applications against JavaScript injection attacks: you can either HTML encode user submitted data in the view or you can HTML encode user submitted data in the controller.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="2b18f-186">[上一頁](authenticating-users-with-windows-authentication-cs.md)
> [下一頁](authenticating-users-with-forms-authentication-vb.md)</span><span class="sxs-lookup"><span data-stu-id="2b18f-186">[Previous](authenticating-users-with-windows-authentication-cs.md)
[Next](authenticating-users-with-forms-authentication-vb.md)</span></span>
