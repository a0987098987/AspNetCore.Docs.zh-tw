---
uid: web-pages/overview/ui-layouts-and-themes/4-working-with-forms
title: "使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單 |Microsoft 文件"
author: tfitzmac
description: "表單是您在其中放置使用者輸入的控制，例如文字方塊、 核取方塊、 選項按鈕和下拉式清單的 HTML 文件區段。 使用表單北..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/10/2014
ms.topic: article
ms.assetid: f3f4b8c8-e8f6-4474-ad94-69228a6c01ee
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/4-working-with-forms
msc.type: authoredcontent
ms.openlocfilehash: 8579c444fd19d1a366349cc09f9f768de23055f8
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
<a name="working-with-html-forms-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="a1630-104">使用 ASP.NET Web Pages (Razor) 網站中的 HTML 表單</span><span class="sxs-lookup"><span data-stu-id="a1630-104">Working with HTML Forms in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="a1630-105">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="a1630-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="a1630-106">這篇文章描述如何處理 HTML 表單 （含有文字方塊和按鈕） 當您使用的 ASP.NET Web Pages (Razor) 網站。</span><span class="sxs-lookup"><span data-stu-id="a1630-106">This article describes how to process an HTML form (with text boxes and buttons) when you are working in an ASP.NET Web Pages (Razor) website.</span></span>
> 
> <span data-ttu-id="a1630-107">**您將學習：**</span><span class="sxs-lookup"><span data-stu-id="a1630-107">**What you'll learn:**</span></span> 
> 
> - <span data-ttu-id="a1630-108">如何建立 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="a1630-108">How to create an HTML form.</span></span>
> - <span data-ttu-id="a1630-109">如何讀取表單中的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-109">How to read user input from the form.</span></span>
> - <span data-ttu-id="a1630-110">如何驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-110">How to validate user input.</span></span>
> - <span data-ttu-id="a1630-111">如何將送出頁面之後，還原表單值。</span><span class="sxs-lookup"><span data-stu-id="a1630-111">How to restore form values after the page is submitted.</span></span>
> 
> <span data-ttu-id="a1630-112">這些是 ASP.NET 程式設計概念，文章中：</span><span class="sxs-lookup"><span data-stu-id="a1630-112">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="a1630-113">`Request` 物件。</span><span class="sxs-lookup"><span data-stu-id="a1630-113">The `Request` object.</span></span>
> - <span data-ttu-id="a1630-114">輸入的驗證。</span><span class="sxs-lookup"><span data-stu-id="a1630-114">Input validation.</span></span>
> - <span data-ttu-id="a1630-115">HTML 編碼方式。</span><span class="sxs-lookup"><span data-stu-id="a1630-115">HTML encoding.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="a1630-116">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="a1630-116">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="a1630-117">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="a1630-117">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="a1630-118">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="a1630-118">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="creating-a-simple-html-form"></a><span data-ttu-id="a1630-119">建立簡單的 HTML 表單</span><span class="sxs-lookup"><span data-stu-id="a1630-119">Creating a Simple HTML Form</span></span>

1. <span data-ttu-id="a1630-120">建立新的網站。</span><span class="sxs-lookup"><span data-stu-id="a1630-120">Create a new website.</span></span>
2. <span data-ttu-id="a1630-121">在根資料夾中，會建立名為網頁*Form.cshtml*並輸入下列標記：</span><span class="sxs-lookup"><span data-stu-id="a1630-121">In the root folder, create a web page named *Form.cshtml* and enter the following markup:</span></span>

    [!code-html[Main](4-working-with-forms/samples/sample1.html)]
3. <span data-ttu-id="a1630-122">啟動您的瀏覽器中的頁面。</span><span class="sxs-lookup"><span data-stu-id="a1630-122">Launch the page in your browser.</span></span> <span data-ttu-id="a1630-123">(在 WebMatrix 中，在**檔案**工作區中，以滑鼠右鍵按一下檔案，然後選取**瀏覽器中啟動**。)具有三個輸入欄位的簡單表單和**送出**按鈕會顯示。</span><span class="sxs-lookup"><span data-stu-id="a1630-123">(In WebMatrix, in the **Files** workspace, right-click the file and then select **Launch in browser**.) A simple form with three input fields and a **Submit** button is displayed.</span></span>

    ![具有三個文字方塊的表單螢幕擷取畫面。](4-working-with-forms/_static/image1.jpg)

    <span data-ttu-id="a1630-125">此時，如果您按一下**送出**按鈕時，會發生任何事。</span><span class="sxs-lookup"><span data-stu-id="a1630-125">At this point, if you click the **Submit** button, nothing happens.</span></span> <span data-ttu-id="a1630-126">若要讓表單很有用，您必須加入一些程式碼將在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="a1630-126">To make the form useful, you have to add some code that will run on the server.</span></span>

## <a name="reading-user-input-from-the-form"></a><span data-ttu-id="a1630-127">讀取表單中的使用者輸入</span><span class="sxs-lookup"><span data-stu-id="a1630-127">Reading User Input from the Form</span></span>

<span data-ttu-id="a1630-128">若要處理的表單，您加入讀取送出的欄位值，並加以處理的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a1630-128">To process the form, you add code that reads the submitted field values and does something with them.</span></span> <span data-ttu-id="a1630-129">此程序會示範如何讀取的欄位，並顯示在頁面上的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-129">This procedure shows you how to read the fields and display the user input on the page.</span></span> <span data-ttu-id="a1630-130">（在生產應用程式中，您通常執行更有趣的事情，與使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-130">(In a production application, you generally do more interesting things with user input.</span></span> <span data-ttu-id="a1630-131">您將會執行，使用資料庫的相關文件中。）</span><span class="sxs-lookup"><span data-stu-id="a1630-131">You'll do that in the article about working with databases.)</span></span>

1. <span data-ttu-id="a1630-132">在頂端*Form.cshtml*檔案中，輸入下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="a1630-132">At the top of the *Form.cshtml* file, enter the following code:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample2.cshtml)]

    <span data-ttu-id="a1630-133">當使用者第一次要求頁面時，會顯示空的表單。</span><span class="sxs-lookup"><span data-stu-id="a1630-133">When the user first requests the page, only the empty form is displayed.</span></span> <span data-ttu-id="a1630-134">使用者 （這會是您） 填入表單，然後按一下 **送出**。</span><span class="sxs-lookup"><span data-stu-id="a1630-134">The user (which will be you) fills in the form and then clicks **Submit**.</span></span> <span data-ttu-id="a1630-135">這會將提交 （文章） 伺服器的使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-135">This submits (posts) the user input to the server.</span></span> <span data-ttu-id="a1630-136">根據預設，要求會移至相同的頁面 (也就是*Form.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="a1630-136">By default, the request goes to the same page (namely, *Form.cshtml*).</span></span>

    <span data-ttu-id="a1630-137">當您提交頁面此時，您輸入的值會顯示表單的正上方：</span><span class="sxs-lookup"><span data-stu-id="a1630-137">When you submit the page this time, the values you entered are displayed just above the form:</span></span>

    ![如果螢幕擷取畫面顯示頁面上顯示您所輸入的值。](4-working-with-forms/_static/image2.jpg)

    <span data-ttu-id="a1630-139">查看網頁的程式碼。</span><span class="sxs-lookup"><span data-stu-id="a1630-139">Look at the code for the page.</span></span> <span data-ttu-id="a1630-140">第一次使用`IsPost`方法，以判斷是否網頁回傳 &#8212; 也就是使用者是否按下**送出** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="a1630-140">You first use the `IsPost` method to determine whether the page is being posted &#8212; that is, whether a user clicked the **Submit** button.</span></span> <span data-ttu-id="a1630-141">如果這是使用 post 要求`IsPost`傳回 true。</span><span class="sxs-lookup"><span data-stu-id="a1630-141">If this is a post, `IsPost` returns true.</span></span> <span data-ttu-id="a1630-142">這是標準方式 ASP.NET Web Pages 中，以判斷是否正在使用的初始要求 （GET 要求） 或回傳 （POST 要求）。</span><span class="sxs-lookup"><span data-stu-id="a1630-142">This is the standard way in ASP.NET Web Pages to determine whether you're working with an initial request (a GET request) or a postback (a POST request).</span></span> <span data-ttu-id="a1630-143">(GET 和 POST 需詳細資訊，請參閱資訊看板 「 HTTP GET 和 POST 和 IsPost 屬性的"，在[ASP.NET Web Pages 程式設計使用 Razor 語法的簡介](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost)。)</span><span class="sxs-lookup"><span data-stu-id="a1630-143">(For more information about GET and POST, see the sidebar "HTTP GET and POST and the IsPost Property" in [Introduction to ASP.NET Web Pages Programming Using the Razor Syntax](https://go.microsoft.com/fwlink/?LinkId=202890#SB_HttpGetPost).)</span></span>

    <span data-ttu-id="a1630-144">接下來，您會取得從填入使用者的值`Request.Form`物件，而您將它們放在變數中供稍後。</span><span class="sxs-lookup"><span data-stu-id="a1630-144">Next, you get the values that the user filled in from the `Request.Form` object, and you put them in variables for later.</span></span> <span data-ttu-id="a1630-145">`Request.Form`物件包含所有已送出頁面上，每個索引鍵所識別的值。</span><span class="sxs-lookup"><span data-stu-id="a1630-145">The `Request.Form` object contains all the values that were submitted with the page, each identified by a key.</span></span> <span data-ttu-id="a1630-146">機碼相當於`name`您想要讀取的表單欄位的屬性。</span><span class="sxs-lookup"><span data-stu-id="a1630-146">The key is the equivalent to the `name` attribute of the form field that you want to read.</span></span> <span data-ttu-id="a1630-147">例如，若要讀取`companyname`欄位 （文字方塊），您使用`Request.Form["companyname"]`。</span><span class="sxs-lookup"><span data-stu-id="a1630-147">For example, to read the `companyname` field (text box), you use `Request.Form["companyname"]`.</span></span>

    <span data-ttu-id="a1630-148">表單值會儲存在`Request.Form`物件做為字串。</span><span class="sxs-lookup"><span data-stu-id="a1630-148">Form values are stored in the `Request.Form` object as strings.</span></span> <span data-ttu-id="a1630-149">因此，當您必須使用的數字或日期，或是其他類型的值，您必須從字串轉換成該類型。</span><span class="sxs-lookup"><span data-stu-id="a1630-149">Therefore, when you have to work with a value as a number or a date or some other type, you have to convert it from a string to that type.</span></span> <span data-ttu-id="a1630-150">在範例中，`AsInt`方法`Request.Form`用來將員工欄位 （其中包含員工計數） 的值轉換成整數。</span><span class="sxs-lookup"><span data-stu-id="a1630-150">In the example, the `AsInt` method of the `Request.Form` is used to convert the value of the employees field (which contains an employee count) to an integer.</span></span>
2. <span data-ttu-id="a1630-151">啟動網頁瀏覽器中的，填寫表單的欄位，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="a1630-151">Launch the page in your browser, fill in the form fields, and click **Submit**.</span></span> <span data-ttu-id="a1630-152">頁面會顯示您輸入的值。</span><span class="sxs-lookup"><span data-stu-id="a1630-152">The page displays the values you entered.</span></span>

> [!TIP] 
> 
> <a id="SB_HTMLEncoding"></a>
> ### <a name="html-encoding-for-appearance-and-security"></a><span data-ttu-id="a1630-153">HTML 編碼的外觀和安全性</span><span class="sxs-lookup"><span data-stu-id="a1630-153">HTML Encoding for Appearance and Security</span></span>
> 
> <span data-ttu-id="a1630-154">HTML 具有特殊用途的字元，例如`<`， `>`，和`&`。</span><span class="sxs-lookup"><span data-stu-id="a1630-154">HTML has special uses for characters like `<`, `>`, and `&`.</span></span> <span data-ttu-id="a1630-155">如果這些特殊字元出現未預期的位置，它們可以廢棄的外觀和功能的網頁。</span><span class="sxs-lookup"><span data-stu-id="a1630-155">If these special characters appear where they're not expected, they can ruin the appearance and functionality of your web page.</span></span> <span data-ttu-id="a1630-156">例如，在瀏覽器會解譯`<`like HTML 項目，開頭為字元 （除非它後接空格）`<b>`或`<input ...>`。</span><span class="sxs-lookup"><span data-stu-id="a1630-156">For example, the browser interprets the `<` character (unless it's followed by a space) as the beginning of an HTML element, like `<b>` or `<input ...>`.</span></span> <span data-ttu-id="a1630-157">如果瀏覽器無法辨識的項目，則只會捨棄的字串開頭為`<`直到達到的項目，它一次會辨識。</span><span class="sxs-lookup"><span data-stu-id="a1630-157">If the browser doesn't recognize the element, it simply discards the string that begins with `<` until it reaches something that it again recognizes.</span></span> <span data-ttu-id="a1630-158">很明顯地，這可能會導致一些奇怪的轉譯頁面中。</span><span class="sxs-lookup"><span data-stu-id="a1630-158">Obviously, this can result in some weird rendering in the page.</span></span>
> 
> <span data-ttu-id="a1630-159">HTML 編碼的瀏覽器將解譯成正確的符號的程式碼以取代這些保留的字元。</span><span class="sxs-lookup"><span data-stu-id="a1630-159">HTML encoding replaces these reserved characters with a code that browsers interpret as the correct symbol.</span></span> <span data-ttu-id="a1630-160">例如，`<`字元取代`&lt;`和`>`字元取代`&gt;`。</span><span class="sxs-lookup"><span data-stu-id="a1630-160">For example, the `<` character is replaced with `&lt;` and the `>` character is replaced with `&gt;`.</span></span> <span data-ttu-id="a1630-161">瀏覽器會將這些取代字串轉譯成您想要查看的字元。</span><span class="sxs-lookup"><span data-stu-id="a1630-161">The browser renders these replacement strings as the characters that you want to see.</span></span>
> 
> <span data-ttu-id="a1630-162">它是個不錯的主意，若要使用 HTML 編碼顯示字串的任何時間 （輸入），您會取得使用者。</span><span class="sxs-lookup"><span data-stu-id="a1630-162">It's a good idea to use HTML encoding any time you display strings (input) that you got from a user.</span></span> <span data-ttu-id="a1630-163">如果沒有，使用者可以嘗試取得您的網頁執行惡意指令碼，或做其他事，危害您的站台安全性的或者只是無法預期。</span><span class="sxs-lookup"><span data-stu-id="a1630-163">If you don't, a user can try to get your web page to run a malicious script or do something else that compromises your site security or that's just not what you intend.</span></span> <span data-ttu-id="a1630-164">（這是非常重要，如果您需要使用者輸入，將其儲存在某個位置，以及然後顯示於稍後 &#8212; 例如，部落格的註解中，為使用者檢閱，或類似的）。</span><span class="sxs-lookup"><span data-stu-id="a1630-164">(This is particularly important if you take user input, store it someplace, and then display it later &#8212; for example, as a blog comment, user review, or something like that.)</span></span>
> 
> <span data-ttu-id="a1630-165">為了避免這些問題，ASP.NET Web 網頁會自動將 HTML 編碼的任何文字內容，您從程式碼輸出。</span><span class="sxs-lookup"><span data-stu-id="a1630-165">To help prevent these problems, ASP.NET Web Pages automatically HTML-encodes any text content that you output from your code.</span></span> <span data-ttu-id="a1630-166">例如，當您顯示變數或運算式，例如使用程式碼的內容`@MyVar`，ASP.NET Web Pages 自動編碼輸出。</span><span class="sxs-lookup"><span data-stu-id="a1630-166">For example, when you display the content of a variable or an expression using code such as `@MyVar`, ASP.NET Web Pages automatically encodes the output.</span></span>


## <a name="validating-user-input"></a><span data-ttu-id="a1630-167">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="a1630-167">Validating User Input</span></span>

<span data-ttu-id="a1630-168">使用者會犯錯。</span><span class="sxs-lookup"><span data-stu-id="a1630-168">Users make mistakes.</span></span> <span data-ttu-id="a1630-169">您要求他們以填滿的欄位中，他們忘了，或您要求使用者輸入的員工人數和它們會改為輸入的名稱。</span><span class="sxs-lookup"><span data-stu-id="a1630-169">You ask them to fill in a field, and they forget to, or you ask them to enter the number of employees and they type a name instead.</span></span> <span data-ttu-id="a1630-170">若要確定您的表單已滿正確地處理它之前，您可以驗證使用者的輸入。</span><span class="sxs-lookup"><span data-stu-id="a1630-170">To make sure that a form has been filled in correctly before you process it, you validate the user's input.</span></span>

<span data-ttu-id="a1630-171">此程序示範如何驗證所有的三個表單欄位，以確定使用者未保留空白。</span><span class="sxs-lookup"><span data-stu-id="a1630-171">This procedure shows how to validate all three form fields to make sure the user didn't leave them blank.</span></span> <span data-ttu-id="a1630-172">您也檢查員工計數的值是數字。</span><span class="sxs-lookup"><span data-stu-id="a1630-172">You also check that the employee count value is a number.</span></span> <span data-ttu-id="a1630-173">如果沒有錯誤，您將會顯示錯誤訊息，告知使用者哪些值未通過驗證。</span><span class="sxs-lookup"><span data-stu-id="a1630-173">If there are errors, you'll display an error message that tells the user what values didn't pass validation.</span></span>

1. <span data-ttu-id="a1630-174">在*Form.cshtml*檔案中，取代為下列程式碼中的程式碼的第一個區塊：</span><span class="sxs-lookup"><span data-stu-id="a1630-174">In the *Form.cshtml* file, replace the first block of code with the following code:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample3.cshtml)]

    <span data-ttu-id="a1630-175">若要驗證使用者輸入，您使用`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="a1630-175">To validate user input, you use the `Validation` helper.</span></span> <span data-ttu-id="a1630-176">您註冊必要的欄位，藉由呼叫`Validation.RequireField`。</span><span class="sxs-lookup"><span data-stu-id="a1630-176">You register required fields by calling `Validation.RequireField`.</span></span> <span data-ttu-id="a1630-177">您藉由呼叫註冊其他類型的驗證`Validation.Add`並指定要驗證的欄位和要執行的驗證類型。</span><span class="sxs-lookup"><span data-stu-id="a1630-177">You register other types of validation by calling `Validation.Add` and specifying the field to validate and the type of validation to perform.</span></span>

    <span data-ttu-id="a1630-178">頁面執行時，ASP.NET 會為您的所有驗證。</span><span class="sxs-lookup"><span data-stu-id="a1630-178">When the page runs, ASP.NET does all the validation for you.</span></span> <span data-ttu-id="a1630-179">您可以藉由呼叫來檢查結果`Validation.IsValid`，它會傳回 true，如果所有項目傳遞和任何欄位驗證失敗為 false。</span><span class="sxs-lookup"><span data-stu-id="a1630-179">You can check the results by calling `Validation.IsValid`, which returns true if everything passed and false if any field failed validation.</span></span> <span data-ttu-id="a1630-180">通常您會呼叫`Validation.IsValid`在執行任何處理使用者輸入之前。</span><span class="sxs-lookup"><span data-stu-id="a1630-180">Typically, you call `Validation.IsValid` before you perform any processing on the user input.</span></span>
2. <span data-ttu-id="a1630-181">更新`<body>`項目加上三次呼叫`Html.ValidationMessage`方法，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="a1630-181">Update the `<body>` element by adding three calls to the `Html.ValidationMessage` method, like this:</span></span>

    [!code-cshtml[Main](4-working-with-forms/samples/sample4.cshtml?highlight=8,13,18)]

    <span data-ttu-id="a1630-182">若要顯示驗證錯誤訊息，您可以呼叫 Html。`ValidationMessage`</span><span class="sxs-lookup"><span data-stu-id="a1630-182">To display validation error messages, you can call Html.`ValidationMessage`</span></span> <span data-ttu-id="a1630-183">並將它傳遞您想要的訊息欄位的名稱。</span><span class="sxs-lookup"><span data-stu-id="a1630-183">and pass it the name of the field that you want the message for.</span></span>
3. <span data-ttu-id="a1630-184">執行網頁。</span><span class="sxs-lookup"><span data-stu-id="a1630-184">Run the page.</span></span> <span data-ttu-id="a1630-185">將欄位保留空白，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="a1630-185">Leave the fields blank and click **Submit**.</span></span> <span data-ttu-id="a1630-186">您看到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a1630-186">You see error messages.</span></span>

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，如果使用者輸入未通過驗證。](4-working-with-forms/_static/image3.jpg)
4. <span data-ttu-id="a1630-188">將字串 (例如，"ABC") 加入至**員工計數**欄位，然後按一下**送出**一次。</span><span class="sxs-lookup"><span data-stu-id="a1630-188">Add a string (for example, "ABC") to the **Employee Count** field and click **Submit** again.</span></span> <span data-ttu-id="a1630-189">此時，您會看到錯誤指出，字串不正確的格式，也就是整數。</span><span class="sxs-lookup"><span data-stu-id="a1630-189">This time you see an error that indicates that the string isn't in the right format, namely, an integer.</span></span>

    ![如果螢幕擷取畫面顯示顯示錯誤訊息，如果使用者在 [員工] 欄位輸入字串。](4-working-with-forms/_static/image4.jpg)

<span data-ttu-id="a1630-191">ASP.NET Web 網頁會提供更多選項，驗證使用者輸入，包括自動執行驗證使用用戶端指令碼，以便讓使用者在瀏覽器中取得的立即回應的能力。</span><span class="sxs-lookup"><span data-stu-id="a1630-191">ASP.NET Web Pages provides more options for validating user input, including the ability to automatically perform validation using client script, so that users get immediate feedback in the browser.</span></span> <span data-ttu-id="a1630-192">請參閱[其他資源](#Additional_Resources)> 一節稍後如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a1630-192">See the [Additional Resources](#Additional_Resources) section later for more information.</span></span>

## <a name="restoring-form-values-after-postbacks"></a><span data-ttu-id="a1630-193">在回傳後正在還原的表單值</span><span class="sxs-lookup"><span data-stu-id="a1630-193">Restoring Form Values After Postbacks</span></span>

<span data-ttu-id="a1630-194">當您在上一節中測試網頁時，您可能已注意到，如果發生驗證錯誤，您輸入 （不只是無效的資料） 的所有項目已消失，而且您必須重新輸入所有欄位的值。</span><span class="sxs-lookup"><span data-stu-id="a1630-194">When you tested the page in the previous section, you might have noticed that if you had a validation error, everything you entered (not just the invalid data) was gone, and you had to re-enter values for all the fields.</span></span> <span data-ttu-id="a1630-195">下列說明處理很重要的一點： 當您送出頁面、 處理它，並再一次呈現頁面，頁面會從頭重新建立。</span><span class="sxs-lookup"><span data-stu-id="a1630-195">This illustrates an important point: when you submit a page, process it, and then render the page again, the page is re-created from scratch.</span></span> <span data-ttu-id="a1630-196">如您所見，這表示已在頁面時送出的任何值都會遺失。</span><span class="sxs-lookup"><span data-stu-id="a1630-196">As you saw, this means that any values that were in the page when it was submitted are lost.</span></span>

<span data-ttu-id="a1630-197">您可以修正此問題，不過。</span><span class="sxs-lookup"><span data-stu-id="a1630-197">You can fix this easily, however.</span></span> <span data-ttu-id="a1630-198">您可以存取已送出的值 (在`Request.Form`物件，因此頁面轉譯時，您可以回到表單欄位填入這些值。</span><span class="sxs-lookup"><span data-stu-id="a1630-198">You have access to the values that were submitted (in the `Request.Form` object, so you can fill those values back into the form fields when the page is rendered.</span></span>

1. <span data-ttu-id="a1630-199">在*Form.cshtml*檔案中，取代`value`屬性`<input>`項目使用`value`屬性。:</span><span class="sxs-lookup"><span data-stu-id="a1630-199">In the *Form.cshtml* file, replace the `value` attributes of the `<input>` elements using the `value` attribute.:</span></span> 

    [!code-cshtml[Main](4-working-with-forms/samples/sample5.cshtml?highlight=13,19,25)]

    <span data-ttu-id="a1630-200">`value`屬性`<input>`項目已設定為以動態方式讀取欄位值，超出`Request.Form`物件。</span><span class="sxs-lookup"><span data-stu-id="a1630-200">The `value` attribute of the `<input>` elements has been set to dynamically read the field value out of the `Request.Form` object.</span></span> <span data-ttu-id="a1630-201">第一次要求頁面時中的值`Request.Form`是所有空的物件。</span><span class="sxs-lookup"><span data-stu-id="a1630-201">The first time that the page is requested, the values in the `Request.Form` object are all empty.</span></span> <span data-ttu-id="a1630-202">這是沒問題，因為如此一來表單是空白。</span><span class="sxs-lookup"><span data-stu-id="a1630-202">This is fine, because that way the form is blank.</span></span>
2. <span data-ttu-id="a1630-203">啟動網頁瀏覽器中的，填寫表單的欄位或將它們留白，然後按一下**送出**。</span><span class="sxs-lookup"><span data-stu-id="a1630-203">Launch the page in your browser, fill in the form fields or leave them blank, and click **Submit**.</span></span> <span data-ttu-id="a1630-204">會顯示頁面，其中顯示送出的值。</span><span class="sxs-lookup"><span data-stu-id="a1630-204">A page that shows the submitted values is displayed.</span></span>

    ![forms-5](4-working-with-forms/_static/image5.jpg)

<a id="Additional_Resources"></a>
## <a name="additional-resources"></a><span data-ttu-id="a1630-206">其他資源</span><span class="sxs-lookup"><span data-stu-id="a1630-206">Additional Resources</span></span>

- [<span data-ttu-id="a1630-207">若要取得的 Web 使用者輸入 1,001 方法</span><span class="sxs-lookup"><span data-stu-id="a1630-207">1,001 Ways to Get Input from Web Users</span></span>](https://msdn.microsoft.com/library/ms971057.aspx)
- <span data-ttu-id="a1630-208">[使用表單和處理使用者輸入](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span><span class="sxs-lookup"><span data-stu-id="a1630-208">[Using Forms and Processing User Input](https://msdn.microsoft.com/library/ms525182(VS.90).aspx)</span></span>
- [<span data-ttu-id="a1630-209">在 ASP.NET Web Pages 網站中驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="a1630-209">Validating User Input in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkId=253002)
- <span data-ttu-id="a1630-210">[在 HTML 表單中使用自動完成](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span><span class="sxs-lookup"><span data-stu-id="a1630-210">[Using AutoComplete in HTML Forms](https://msdn.microsoft.com/library/ms533032(VS.85).aspx)</span></span>
