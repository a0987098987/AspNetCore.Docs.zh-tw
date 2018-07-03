---
uid: web-pages/overview/getting-started/introducing-razor-syntax-c
title: 使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介 |Microsoft Docs
author: tfitzmac
description: 本章概述您程式設計的 ASP.NET 網頁使用 Razor 語法。 ASP.NET 是 Microsoft 的技術，用於執行動態 web 的 pa...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: aa67d304-583b-4bf8-a231-195656cfb587
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-c
msc.type: authoredcontent
ms.openlocfilehash: 6e0af63ffab5ce1a4d582cbe1e9456da20df2b64
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37363399"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-c"></a><span data-ttu-id="be547-104">使用 Razor 語法 (C#) 的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="be547-104">Introduction to ASP.NET Web Programming Using the Razor Syntax (C#)</span></span>
====================
<span data-ttu-id="be547-105">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="be547-105">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="be547-106">這篇文章概述您程式設計的 ASP.NET 網頁使用 Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="be547-106">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax.</span></span> <span data-ttu-id="be547-107">ASP.NET 是 Microsoft 的技術，用於在 web 伺服器上執行動態網頁。</span><span class="sxs-lookup"><span data-stu-id="be547-107">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span> <span data-ttu-id="be547-108">本文章著重於使用 C# 程式設計語言中。</span><span class="sxs-lookup"><span data-stu-id="be547-108">This articles focuses on using the C# programming language.</span></span>
> 
> <span data-ttu-id="be547-109">**您將學到什麼**:</span><span class="sxs-lookup"><span data-stu-id="be547-109">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="be547-110">前 8 的程式設計入門程式設計使用 Razor 語法的 ASP.NET Web Pages 的提示。</span><span class="sxs-lookup"><span data-stu-id="be547-110">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="be547-111">您需要的基本程式設計概念。</span><span class="sxs-lookup"><span data-stu-id="be547-111">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="be547-112">ASP.NET server 程式碼和 Razor 語法是有關。</span><span class="sxs-lookup"><span data-stu-id="be547-112">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="be547-113">軟體版本</span><span class="sxs-lookup"><span data-stu-id="be547-113">Software versions</span></span>
> 
> 
> - <span data-ttu-id="be547-114">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="be547-114">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="be547-115">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="be547-115">This tutorial also works with ASP.NET Web Pages 2.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="be547-116">最佳的 8 程式設計秘訣</span><span class="sxs-lookup"><span data-stu-id="be547-116">The Top 8 Programming Tips</span></span>

<span data-ttu-id="be547-117">此區段會列出您一定要知道當您開始撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的一些秘訣。</span><span class="sxs-lookup"><span data-stu-id="be547-117">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

> [!NOTE]
> <span data-ttu-id="be547-118">根據 Razor 語法的 C# 程式設計語言，以及這最常使用以 ASP.NET Web Pages 的語言。</span><span class="sxs-lookup"><span data-stu-id="be547-118">The Razor syntax is based on the C# programming language, and that's the language that's used most often with ASP.NET Web Pages.</span></span> <span data-ttu-id="be547-119">不過，Razor 語法也支援 Visual Basic 語言，以及您會看到您也可以執行在 Visual Basic 中的所有項目。</span><span class="sxs-lookup"><span data-stu-id="be547-119">However, the Razor syntax also supports the Visual Basic language, and everything you see you can also do in Visual Basic.</span></span> <span data-ttu-id="be547-120">如需詳細資訊，請參閱附錄[Visual Basic 語言與語法](https://go.microsoft.com/fwlink/?LinkId=202908)。</span><span class="sxs-lookup"><span data-stu-id="be547-120">For details, see the appendix [Visual Basic Language and Syntax](https://go.microsoft.com/fwlink/?LinkId=202908).</span></span>


<span data-ttu-id="be547-121">在本文稍後，您可以找到大部分的這些程式設計技術的更多詳細。</span><span class="sxs-lookup"><span data-stu-id="be547-121">You can find more details about most of these programming techniques later in the article.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="be547-122">1.您將程式碼新增至頁面上，使用 @ 字元</span><span class="sxs-lookup"><span data-stu-id="be547-122">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="be547-123">`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="be547-123">The `@` character starts inline expressions, single statement blocks, and multi-statement blocks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample1.html)]

<span data-ttu-id="be547-124">這是這些陳述式如下所示在網頁瀏覽器中執行時：</span><span class="sxs-lookup"><span data-stu-id="be547-124">This is what these statements look like when the page runs in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-c/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="be547-126">**HTML 編碼**</span><span class="sxs-lookup"><span data-stu-id="be547-126">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="be547-127">當您在頁面上，使用顯示的內容時`@`字元 ASP.NET HTML 編碼的輸出，如上述範例中，所示。</span><span class="sxs-lookup"><span data-stu-id="be547-127">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="be547-128">這會取代 HTML 的保留的字元 (例如`<`並`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。</span><span class="sxs-lookup"><span data-stu-id="be547-128">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="be547-129">而不進行 HTML 編碼，從伺服器程式碼的輸出可能不正確，會顯示，並可能會暴露於安全性風險的頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-129">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="be547-130">如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後的。</span><span class="sxs-lookup"><span data-stu-id="be547-130">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="be547-131">您可以深入了解中的 HTML 編碼[使用表單](https://go.microsoft.com/fwlink/?LinkId=202892)。</span><span class="sxs-lookup"><span data-stu-id="be547-131">You can read more about HTML encoding in [Working with Forms](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-in-braces"></a><span data-ttu-id="be547-132">2.大括號括住程式碼區塊</span><span class="sxs-lookup"><span data-stu-id="be547-132">2. You enclose code blocks in braces</span></span>

<span data-ttu-id="be547-133">A*程式碼區塊*包含一或多個程式碼陳述式和大括號括住。</span><span class="sxs-lookup"><span data-stu-id="be547-133">A *code block* includes one or more code statements and is enclosed in braces.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample2.html)]

<span data-ttu-id="be547-134">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="be547-134">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-c/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-semicolon"></a><span data-ttu-id="be547-136">3.區塊中，您最後使用分號的每個程式碼陳述式</span><span class="sxs-lookup"><span data-stu-id="be547-136">3. Inside a block, you end each code statement with a semicolon</span></span>

<span data-ttu-id="be547-137">程式碼區塊中，每個完整的程式碼陳述式必須以分號結尾。</span><span class="sxs-lookup"><span data-stu-id="be547-137">Inside a code block, each complete code statement must end with a semicolon.</span></span> <span data-ttu-id="be547-138">內嵌運算式不以分號結尾。</span><span class="sxs-lookup"><span data-stu-id="be547-138">Inline expressions don't end with a semicolon.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample3.html)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="be547-139">4.您可以使用變數來儲存值</span><span class="sxs-lookup"><span data-stu-id="be547-139">4. You use variables to store values</span></span>

<span data-ttu-id="be547-140">您可以儲存中的值*變數*，包括字串、 數字和日期等。您建立新變數使用`var`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="be547-140">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `var` keyword.</span></span> <span data-ttu-id="be547-141">您可以直接在頁面上，使用中插入變數的值`@`。</span><span class="sxs-lookup"><span data-stu-id="be547-141">You can insert variable values directly in a page using `@`.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample4.html)]

<span data-ttu-id="be547-142">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="be547-142">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-c/_static/image3.jpg)

<a id="ID_StringLiterals"></a>
### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="be547-144">5.您將常值字串值括在雙引號中</span><span class="sxs-lookup"><span data-stu-id="be547-144">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="be547-145">A*字串*是會被視為文字的字元序列。</span><span class="sxs-lookup"><span data-stu-id="be547-145">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="be547-146">若要指定為字串，您將它括在雙引號中：</span><span class="sxs-lookup"><span data-stu-id="be547-146">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample5.cshtml)]

<span data-ttu-id="be547-147">如果您想要顯示的字串包含反斜線字元 ( `\` ) 或雙引號 ( `"` )，使用*逐字字串常值*，前面會加上`@`運算子。</span><span class="sxs-lookup"><span data-stu-id="be547-147">If the string that you want to display contains a backslash character ( `\` ) or double quotation marks ( `"` ), use a *verbatim string literal* that's prefixed with the `@` operator.</span></span> <span data-ttu-id="be547-148">(在 C# 中，\ 字元具有特殊意義，除非您使用逐字字串常值。)</span><span class="sxs-lookup"><span data-stu-id="be547-148">(In C#, the \ character has special meaning unless you use a verbatim string literal.)</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample6.html)]

<span data-ttu-id="be547-149">若要內嵌雙引號，請使用逐字字串常值並重複引號：</span><span class="sxs-lookup"><span data-stu-id="be547-149">To embed double quotation marks, use a verbatim string literal and repeat the quotation marks:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample7.html)]

<span data-ttu-id="be547-150">以下是在網頁中使用這兩個範例的結果：</span><span class="sxs-lookup"><span data-stu-id="be547-150">Here's the result of using both of these examples in a page:</span></span>

![Razor-Img4](introducing-razor-syntax-c/_static/image4.jpg)

> [!NOTE]
> <span data-ttu-id="be547-152">請注意，`@`字元用來將標記在 C# 中逐字字串常值，以及將 ASP.NET 頁面的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-152">Notice that the `@` character is used both to mark verbatim string literals in C# and to mark code in ASP.NET pages.</span></span>


### <a name="6-code-is-case-sensitive"></a><span data-ttu-id="be547-153">6.程式碼會區分大小寫</span><span class="sxs-lookup"><span data-stu-id="be547-153">6. Code is case sensitive</span></span>

<span data-ttu-id="be547-154">以 C# 關鍵字 (例如`var`， `true`，和`if`) 和變數名稱會區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="be547-154">In C#, keywords (like `var`, `true`, and `if`) and variable names are case sensitive.</span></span> <span data-ttu-id="be547-155">下列程式碼會建立兩個不同的變數，`lastName`和 `LastName.`</span><span class="sxs-lookup"><span data-stu-id="be547-155">The following lines of code create two different variables, `lastName` and `LastName.`</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample8.cshtml)]

<span data-ttu-id="be547-156">如果您宣告一個變數`var lastName = "Smith";`如果您嘗試參考該變數做為頁面中的`@LastName`，會發生錯誤，因為`LastName`無法辨識。</span><span class="sxs-lookup"><span data-stu-id="be547-156">If you declare a variable as `var lastName = "Smith";` and if you try to reference that variable in your page as `@LastName`, an error results because `LastName` won't be recognized.</span></span>

> [!NOTE]
> <span data-ttu-id="be547-157">在 Visual Basic 中的關鍵字而變數則屬於*不*區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="be547-157">In Visual Basic, keywords and variables are *not* case sensitive.</span></span>


### <a name="7-much-of-your-coding-involves-objects"></a><span data-ttu-id="be547-158">7.大部分您撰寫程式碼牽涉到物件</span><span class="sxs-lookup"><span data-stu-id="be547-158">7. Much of your coding involves objects</span></span>

<span data-ttu-id="be547-159">*物件*代表您可以使用程式設計的項目&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列） 等等。物件具有描述其特性的屬性，以及您可以用來讀取，或變更&#8212;的文字 方塊中的物件具有`Text`（還有其他） 的屬性，要求物件具有`Url`屬性，電子郵件訊息已`From`屬性，與客戶物件都有`FirstName`屬性。</span><span class="sxs-lookup"><span data-stu-id="be547-159">An *object* represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics and that you can read or change &#8212; a text box object has a `Text` property (among others), a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="be547-160">物件也有方法&quot;動詞&quot;他們可以執行。</span><span class="sxs-lookup"><span data-stu-id="be547-160">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="be547-161">範例包括將檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。</span><span class="sxs-lookup"><span data-stu-id="be547-161">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="be547-162">您會經常使用`Request`物件，例如值的文字方塊 （表單欄位） 的資訊讓您在頁面上，進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型。下列範例示範如何存取的屬性`Request`物件，以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：</span><span class="sxs-lookup"><span data-stu-id="be547-162">You'll often work with the `Request` object, which gives you information like the values of text boxes (form fields) on the page, what type of browser made the request, the URL of the page, the user identity, etc. The following example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample9.html)]

<span data-ttu-id="be547-163">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="be547-163">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-c/_static/image5.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="be547-165">8.您可以撰寫程式碼，讓決策</span><span class="sxs-lookup"><span data-stu-id="be547-165">8. You can write code that makes decisions</span></span>

<span data-ttu-id="be547-166">動態網頁的重要功能是，您可以決定如何根據條件。</span><span class="sxs-lookup"><span data-stu-id="be547-166">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="be547-167">若要這樣做最常見的方法是使用`if`陳述式 (和選擇性`else`陳述式)。</span><span class="sxs-lookup"><span data-stu-id="be547-167">The most common way to do this is with the `if` statement (and optional `else` statement).</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample10.cshtml)]

<span data-ttu-id="be547-168">陳述式`if(IsPost)`是本文撰寫的簡略方法`if(IsPost == true)`。</span><span class="sxs-lookup"><span data-stu-id="be547-168">The statement `if(IsPost)` is a shorthand way of writing `if(IsPost == true)`.</span></span> <span data-ttu-id="be547-169">連同`if`陳述式，有各種不同的方式來測試條件，重複程式碼區塊，並依此類推，這是本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="be547-169">Along with `if` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="be547-170">在瀏覽器中顯示結果 (按一下後**送出**):</span><span class="sxs-lookup"><span data-stu-id="be547-170">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-c/_static/image6.jpg)

> [!TIP] 
> 
> <a id="SB_HttpGetPost"></a>
> ### <a name="http-get-and-post-methods-and-the-ispost-property"></a><span data-ttu-id="be547-172">HTTP GET 和 POST 方法與 IsPost 屬性</span><span class="sxs-lookup"><span data-stu-id="be547-172">HTTP GET and POST Methods and the IsPost Property</span></span>
> 
> <span data-ttu-id="be547-173">用於 web 網頁 (HTTP) 的通訊協定支援非常有限的用來對伺服器提出要求的方法 （動詞命令）。</span><span class="sxs-lookup"><span data-stu-id="be547-173">The protocol used for web pages (HTTP) supports a very limited number of methods (verbs) that are used to make requests to the server.</span></span> <span data-ttu-id="be547-174">最常見的兩個為 GET，會用來讀取的頁面和文章，用來提交頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-174">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="be547-175">一般情況下，使用者要求的頁面上，第一次要求頁面時使用 GET。</span><span class="sxs-lookup"><span data-stu-id="be547-175">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="be547-176">如果使用者在表單中填入，然後再按一下 [提交] 按鈕，瀏覽器會對伺服器提出 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="be547-176">If the user fills in a form and then clicks a submit button, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="be547-177">在 web 程式設計中，通常很有用知道是否要求網頁時正在為 GET 或 POST，讓您知道如何處理頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-177">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="be547-178">ASP.NET Web Pages 中，您可以使用`IsPost`屬性，以查看要求是 GET 或 POST。</span><span class="sxs-lookup"><span data-stu-id="be547-178">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="be547-179">如果要求是某篇文章，`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="be547-179">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="be547-180">您會看到的許多範例會示範如何處理值的方式而定頁面`IsPost`。</span><span class="sxs-lookup"><span data-stu-id="be547-180">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="be547-181">簡單的程式碼範例</span><span class="sxs-lookup"><span data-stu-id="be547-181">A Simple Code Example</span></span>

<span data-ttu-id="be547-182">此程序會示範如何建立一個網頁，說明基本的程式設計技術。</span><span class="sxs-lookup"><span data-stu-id="be547-182">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="be547-183">在範例中，您可以建立讓使用者輸入兩個數字，然後再將它們加入，並顯示結果頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-183">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="be547-184">在編輯器中，建立新的檔案並將它命名*AddNumbers.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="be547-184">In your editor, create a new file and name it *AddNumbers.cshtml*.</span></span>
2. <span data-ttu-id="be547-185">將下列程式碼和標記複製到頁面上，而且要取代已經在頁面中的任何項目中。</span><span class="sxs-lookup"><span data-stu-id="be547-185">Copy the following code and markup into the page, replacing anything already in the page.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample11.cshtml)]

    <span data-ttu-id="be547-186">以下是您必須注意的事項：</span><span class="sxs-lookup"><span data-stu-id="be547-186">Here are some things for you to note:</span></span>

    - <span data-ttu-id="be547-187">`@`字元會在頁面中，啟動程式碼的第一個區塊，而且它前面出現`totalMessage`內嵌頁面底部附近的變數。</span><span class="sxs-lookup"><span data-stu-id="be547-187">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable that's embedded near the bottom of the page.</span></span>
    - <span data-ttu-id="be547-188">位於頁面頂端的區塊會括在大括弧。</span><span class="sxs-lookup"><span data-stu-id="be547-188">The block at the top of the page is enclosed in braces.</span></span>
    - <span data-ttu-id="be547-189">在頂端的區塊，每一行都會以分號結尾。</span><span class="sxs-lookup"><span data-stu-id="be547-189">In the block at the top, all lines end with a semicolon.</span></span>
    - <span data-ttu-id="be547-190">變數`total`， `num1`， `num2`，和`totalMessage`儲存數個數字和字串。</span><span class="sxs-lookup"><span data-stu-id="be547-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="be547-191">常值字串值，指派給`totalMessage`變數是雙引號括住。</span><span class="sxs-lookup"><span data-stu-id="be547-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="be547-192">由於程式碼時區分大小寫，`totalMessage`頁面底部附近，會使用變數，其名稱必須完全符合在上方的變數。</span><span class="sxs-lookup"><span data-stu-id="be547-192">Because the code is case-sensitive, when the `totalMessage` variable is used near the bottom of the page, its name must match the variable at the top exactly.</span></span>
    - <span data-ttu-id="be547-193">運算式`num1.AsInt() + num2.AsInt()`示範如何使用物件和方法。</span><span class="sxs-lookup"><span data-stu-id="be547-193">The expression `num1.AsInt() + num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="be547-194">`AsInt`上每個變數的方法會將轉換使用者輸入的數字 （整數），以便您可以在其上執行算術運算的字串。</span><span class="sxs-lookup"><span data-stu-id="be547-194">The `AsInt` method on each variable converts the string entered by a user to a number (an integer) so that you can perform arithmetic on it.</span></span>
    - <span data-ttu-id="be547-195">`<form>`標記會包括`method="post"`屬性。</span><span class="sxs-lookup"><span data-stu-id="be547-195">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="be547-196">這表示當使用者按一下**新增**，頁面將會傳送到伺服器使用 HTTP POST 方法。</span><span class="sxs-lookup"><span data-stu-id="be547-196">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="be547-197">當提交頁面時，`if(IsPost)`測試評估為 true，條件式程式碼執行時，顯示個數字相加的結果。</span><span class="sxs-lookup"><span data-stu-id="be547-197">When the page is submitted, the `if(IsPost)` test evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="be547-198">儲存頁面，並在瀏覽器中執行它。</span><span class="sxs-lookup"><span data-stu-id="be547-198">Save the page and run it in a browser.</span></span> <span data-ttu-id="be547-199">(請確定中選取頁面**檔案**才能執行這個工作區。)輸入兩個整數數字，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="be547-199">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span> 

    ![Razor Img7](introducing-razor-syntax-c/_static/image7.jpg)

## <a name="basic-programming-concepts"></a><span data-ttu-id="be547-201">基本程式設計概念</span><span class="sxs-lookup"><span data-stu-id="be547-201">Basic Programming Concepts</span></span>

<span data-ttu-id="be547-202">這篇文章為您提供 ASP.NET web 程式設計的概觀。</span><span class="sxs-lookup"><span data-stu-id="be547-202">This article provides you with an overview of ASP.NET web programming.</span></span> <span data-ttu-id="be547-203">它不是詳盡的檢查，只是透過將最常使用的程式設計概念的快速教學。</span><span class="sxs-lookup"><span data-stu-id="be547-203">It isn't an exhaustive examination, just a quick tour through the programming concepts you'll use most often.</span></span> <span data-ttu-id="be547-204">即便如此，它涵蓋了幾乎所有您要開始使用 ASP.NET Web Pages。</span><span class="sxs-lookup"><span data-stu-id="be547-204">Even so, it covers almost everything you'll need to get started with ASP.NET Web Pages.</span></span>

<span data-ttu-id="be547-205">但首先，小小的技術背景。</span><span class="sxs-lookup"><span data-stu-id="be547-205">But first, a little technical background.</span></span>

### <a name="the-razor-syntax-server-code-and-aspnet"></a><span data-ttu-id="be547-206">Razor 語法、 伺服端程式碼和 ASP.NET</span><span class="sxs-lookup"><span data-stu-id="be547-206">The Razor Syntax, Server Code, and ASP.NET</span></span>

<span data-ttu-id="be547-207">Razor 語法是一個簡單的程式設計語法，將伺服器為基礎的程式碼內嵌在網頁中。</span><span class="sxs-lookup"><span data-stu-id="be547-207">Razor syntax is a simple programming syntax for embedding server-based code in a web page.</span></span> <span data-ttu-id="be547-208">使用 Razor 語法的網頁，在中，有兩種類型的內容： 用戶端內容和伺服器程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-208">In a web page that uses the Razor syntax, there are two kinds of content: client content and server code.</span></span> <span data-ttu-id="be547-209">用戶端的內容是您要用來在網頁中的項目： HTML 標記 （項目），樣式資訊，例如 CSS、 或許有些用戶端指令碼，例如 JavaScript 和純文字。</span><span class="sxs-lookup"><span data-stu-id="be547-209">Client content is the stuff you're used to in web pages: HTML markup (elements), style information such as CSS, maybe some client script such as JavaScript, and plain text.</span></span>

<span data-ttu-id="be547-210">Razor 語法可讓您將伺服器程式碼新增至這個用戶端內容。</span><span class="sxs-lookup"><span data-stu-id="be547-210">Razor syntax lets you add server code to this client content.</span></span> <span data-ttu-id="be547-211">如果網頁沒有伺服端程式碼，在伺服器執行該程式碼第一次後, 才會傳送至瀏覽器的頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-211">If there's server code in the page, the server runs that code first, before it sends the page to the browser.</span></span> <span data-ttu-id="be547-212">藉由在伺服器上執行，程式碼可以執行可能更複雜使用單獨的情況，就像是存取伺服器端資料庫的用戶端內容來執行動作的工作。</span><span class="sxs-lookup"><span data-stu-id="be547-212">By running on the server, the code can perform tasks that can be a lot more complex to do using client content alone, like accessing server-based databases.</span></span> <span data-ttu-id="be547-213">伺服端程式碼以動態方式可以建立用戶端內容的最重要的是，&#8212;它可以產生 HTML 標記或其他內容，即時並再將它傳送至以及頁面可能會包含任何靜態 HTML 瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="be547-213">Most importantly, server code can dynamically create client content &#8212; it can generate HTML markup or other content on the fly and then send it to the browser along with any static HTML that the page might contain.</span></span> <span data-ttu-id="be547-214">瀏覽器的觀點而言，伺服器程式碼所產生的用戶端內容並沒什麼兩樣任何其他的用戶端內容。</span><span class="sxs-lookup"><span data-stu-id="be547-214">From the browser's perspective, client content that's generated by your server code is no different than any other client content.</span></span> <span data-ttu-id="be547-215">如您所見，所需的伺服器程式碼都非常簡單。</span><span class="sxs-lookup"><span data-stu-id="be547-215">As you've already seen, the server code that's required is quite simple.</span></span>

<span data-ttu-id="be547-216">包含 Razor 語法的 ASP.NET web pages 有特殊的檔案副檔名 (*.cshtml*或是 *.vbhtml*)。</span><span class="sxs-lookup"><span data-stu-id="be547-216">ASP.NET web pages that include the Razor syntax have a special file extension (*.cshtml* or *.vbhtml*).</span></span> <span data-ttu-id="be547-217">伺服器會辨識這些擴充功能，執行含有 Razor 語法標記，然後再將網頁傳送至瀏覽器的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-217">The server recognizes these extensions, runs the code that's marked with Razor syntax, and then sends the page to the browser.</span></span>

### <a name="where-does-aspnet-fit-in"></a><span data-ttu-id="be547-218">沒有 ASP.NET 適用於什麼情況？</span><span class="sxs-lookup"><span data-stu-id="be547-218">Where does ASP.NET fit in?</span></span>

<span data-ttu-id="be547-219">Razor 語法是以呼叫為基礎的 Microsoft.NET Framework 的 ASP.NET 的 microsoft 技術為基礎。</span><span class="sxs-lookup"><span data-stu-id="be547-219">Razor syntax is based on a technology from Microsoft called ASP.NET, which in turn is based on the Microsoft .NET Framework.</span></span> <span data-ttu-id="be547-220">.Net Framework 是大型的完整程式設計架構，microsoft 開發幾乎任何類型的電腦應用程式。</span><span class="sxs-lookup"><span data-stu-id="be547-220">The.NET Framework is a big, comprehensive programming framework from Microsoft for developing virtually any type of computer application.</span></span> <span data-ttu-id="be547-221">ASP.NET 是專為建立 web 應用程式的.NET framework 組件。</span><span class="sxs-lookup"><span data-stu-id="be547-221">ASP.NET is the part of the .NET Framework that's specifically designed for creating web applications.</span></span> <span data-ttu-id="be547-222">開發人員使用 ASP.NET 來建立許多最大和最高流量網站的世界中。</span><span class="sxs-lookup"><span data-stu-id="be547-222">Developers have used ASP.NET to create many of the largest and highest-traffic websites in the world.</span></span> <span data-ttu-id="be547-223">(每當您看到的檔案名稱副檔名 *.aspx*中站台的 URL 的一部分，就會知道使用 ASP.NET 撰寫的網站。)</span><span class="sxs-lookup"><span data-stu-id="be547-223">(Any time you see the file-name extension *.aspx* as part of the URL in a site, you'll know that the site was written using ASP.NET.)</span></span>

<span data-ttu-id="be547-224">Razor 語法可讓您的 ASP.NET 中，但使用簡化的語法，可以更輕鬆地瞭解您是否專家如果您是新手，就能讓您更具生產力的所有功能。</span><span class="sxs-lookup"><span data-stu-id="be547-224">The Razor syntax gives you all the power of ASP.NET, but using a simplified syntax that's easier to learn if you're a beginner and that makes you more productive if you're an expert.</span></span> <span data-ttu-id="be547-225">即使這種語法用法很簡單，系列 ASP.NET 和.NET Framework 關係表示隨著您的網站變得更複雜的您會有較大的架構可供您的能力。</span><span class="sxs-lookup"><span data-stu-id="be547-225">Even though this syntax is simple to use, its family relationship to ASP.NET and the .NET Framework means that as your websites become more sophisticated, you have the power of the larger frameworks available to you.</span></span>

![Razor-Img8](introducing-razor-syntax-c/_static/image8.jpg)

> [!TIP] 
> 
> <span data-ttu-id="be547-227">**類別和執行個體**</span><span class="sxs-lookup"><span data-stu-id="be547-227">**Classes and Instances**</span></span>
> 
> <span data-ttu-id="be547-228">ASP.NET server 程式碼會使用物件，會依次組建於類別的概念。</span><span class="sxs-lookup"><span data-stu-id="be547-228">ASP.NET server code uses objects, which are in turn built on the idea of classes.</span></span> <span data-ttu-id="be547-229">此類別是定義或範本物件。</span><span class="sxs-lookup"><span data-stu-id="be547-229">The class is the definition or template for an object.</span></span> <span data-ttu-id="be547-230">例如，應用程式可能會包含`Customer`類別來定義的屬性和任何客戶物件所需的方法。</span><span class="sxs-lookup"><span data-stu-id="be547-230">For example, an application might contain a `Customer` class that defines the properties and methods that any customer object needs.</span></span>
> 
> <span data-ttu-id="be547-231">當應用程式必須使用實際的客戶資訊時，它會建立的執行個體 (或*具現化*) customer 物件。</span><span class="sxs-lookup"><span data-stu-id="be547-231">When the application needs to work with actual customer information, it creates an instance of (or *instantiates*) a customer object.</span></span> <span data-ttu-id="be547-232">每個個別的客戶是獨立執行個體`Customer`類別。</span><span class="sxs-lookup"><span data-stu-id="be547-232">Each individual customer is a separate instance of the `Customer` class.</span></span> <span data-ttu-id="be547-233">每個執行個體支援相同的屬性和方法，但每個執行個體的屬性值是通常不同，因為每個客戶物件都是唯一。</span><span class="sxs-lookup"><span data-stu-id="be547-233">Every instance supports the same properties and methods, but the property values for each instance are typically different, because each customer object is unique.</span></span> <span data-ttu-id="be547-234">一位客戶物件中`LastName`屬性可能是"Smith"，另一個客戶物件，在`LastName`屬性可能是"Jones"。</span><span class="sxs-lookup"><span data-stu-id="be547-234">In one customer object, the `LastName` property might be "Smith"; in another customer object, the `LastName` property might be "Jones."</span></span>
> 
> <span data-ttu-id="be547-235">同樣地，是在您的網站中任何個別網頁`Page`的執行個體的物件`Page`類別。</span><span class="sxs-lookup"><span data-stu-id="be547-235">Similarly, any individual web page in your site is a `Page` object that's an instance of the `Page` class.</span></span> <span data-ttu-id="be547-236">在頁面上的按鈕`Button`的執行個體的物件`Button`類別，並依此類推。</span><span class="sxs-lookup"><span data-stu-id="be547-236">A button on the page is a `Button` object that is an instance of the `Button` class, and so on.</span></span> <span data-ttu-id="be547-237">每個執行個體具有其本身的特性，但它們全都以基礎物件的類別定義中指定的內容。</span><span class="sxs-lookup"><span data-stu-id="be547-237">Each instance has its own characteristics, but they all are based on what's specified in the object's class definition.</span></span>


## <a name="basic-syntax"></a><span data-ttu-id="be547-238">基本語法</span><span class="sxs-lookup"><span data-stu-id="be547-238">Basic Syntax</span></span>

<span data-ttu-id="be547-239">先前您已看到如何建立 ASP.NET Web Pages 頁面，以及如何將伺服器程式碼新增至 HTML 標記的基本範例。</span><span class="sxs-lookup"><span data-stu-id="be547-239">Earlier you saw a basic example of how to create an ASP.NET Web Pages page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="be547-240">這裡您將了解撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的基本概念&#8212;也就是程式設計語言規則。</span><span class="sxs-lookup"><span data-stu-id="be547-240">Here you'll learn the basics of writing ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="be547-241">如果您是熟悉程式設計 （尤其是如果您已使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大多什麼您閱讀這裡應該不陌生。</span><span class="sxs-lookup"><span data-stu-id="be547-241">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="be547-242">您可能需要在自己熟悉只伺服端程式碼新增至標記中的如何 *.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="be547-242">You'll probably need to familiarize yourself only with how server code is added to markup in *.cshtml* files.</span></span>

<a id="BM_CombiningTextMarkupAndCode"></a>
### <a name="combining-text-markup-and-code-in-code-blocks"></a><span data-ttu-id="be547-243">結合文字、 標記和程式碼區塊中的程式碼</span><span class="sxs-lookup"><span data-stu-id="be547-243">Combining Text, Markup, and Code in Code Blocks</span></span>

<span data-ttu-id="be547-244">在伺服器程式碼區塊，您通常會想要輸出的文字或標記 （或兩者） 頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-244">In server code blocks, you often want to output text or markup (or both) to the page.</span></span> <span data-ttu-id="be547-245">如果伺服器程式碼區塊包含的文字，不是程式碼，而是應轉譯為是，ASP.NET 必須能夠區別該文字與程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-245">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="be547-246">有幾個方式可做到這點。</span><span class="sxs-lookup"><span data-stu-id="be547-246">There are several ways to do this.</span></span>

- <span data-ttu-id="be547-247">在類似 HTML 元素括住文字`<p></p>`或`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="be547-247">Enclose the text in an HTML element like `<p></p>` or `<em></em>`:</span></span>   

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample12.cshtml)]

    <span data-ttu-id="be547-248">HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。</span><span class="sxs-lookup"><span data-stu-id="be547-248">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="be547-249">當 ASP.NET 看到 HTML 開頭標記 (例如`<p>`)，它會呈現所有項目包括項目且其內容做為解決伺服器程式碼運算式，因為它會在瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="be547-249">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything including the element and its content as is to the browser, resolving server-code expressions as it goes.</span></span>
- <span data-ttu-id="be547-250">使用`@:`運算子或`<text>`項目。</span><span class="sxs-lookup"><span data-stu-id="be547-250">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="be547-251">`@:`輸出內容包含純文字] 或 [無對應的 HTML 標記。 單行`<text>`元素會括住要輸出的多行。</span><span class="sxs-lookup"><span data-stu-id="be547-251">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="be547-252">當您不想要呈現的 HTML 項目作為輸出的一部分，則這些選項會很有用。</span><span class="sxs-lookup"><span data-stu-id="be547-252">These options are useful when you don't want to render an HTML element as part of the output.</span></span>  

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample13.cshtml)]

    <span data-ttu-id="be547-253">如果您想要輸出多個文字行或不相符的 HTML 標記，您可以在前面上使用的每一行`@:`，或您可以用括號中的一行`<text>`項目。</span><span class="sxs-lookup"><span data-stu-id="be547-253">If you want to output multiple lines of text or unmatched HTML tags, you can precede each line with `@:`, or you can enclose the line in a `<text>` element.</span></span> <span data-ttu-id="be547-254">像是`@:`運算子，`<text>`標記由 ASP.NET 識別的文字內容，而且永遠不會在網頁輸出中轉譯。</span><span class="sxs-lookup"><span data-stu-id="be547-254">Like the `@:` operator,`<text>` tags are used by ASP.NET to identify text content and are never rendered in the page output.</span></span>

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample14.cshtml)]

    <span data-ttu-id="be547-255">第一個範例會重複上一個範例，但是會使用一對`<text>`標記括住要轉譯的文字。</span><span class="sxs-lookup"><span data-stu-id="be547-255">The first example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span> <span data-ttu-id="be547-256">在第二個範例中，`<text>`並`</text>`標記括住三行，全部都有部分未包含的文字和不相符的 HTML 標記 (`<br />`)，以及伺服器程式碼和相符的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="be547-256">In the second example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="be547-257">同樣地，您也無法在個別使用每一行`@:`運算子; 其中一個的方式運作。</span><span class="sxs-lookup"><span data-stu-id="be547-257">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    > [!NOTE]
    > <span data-ttu-id="be547-258">當您輸出文字，這一節所示&#8212;使用 HTML 元素，`@:`運算子，或`<text>`項目&#8212;ASP.NET 不進行 HTML 編碼的輸出。</span><span class="sxs-lookup"><span data-stu-id="be547-258">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="be547-259">(如先前所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面都會加上輸出`@`，除非在這一節所述的特殊案例。)</span><span class="sxs-lookup"><span data-stu-id="be547-259">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="be547-260">Whitespace</span><span class="sxs-lookup"><span data-stu-id="be547-260">Whitespace</span></span>

<span data-ttu-id="be547-261">額外的空格，陳述式 （內部和外部字串常值），就不會影響陳述式：</span><span class="sxs-lookup"><span data-stu-id="be547-261">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample15.cshtml)]

<span data-ttu-id="be547-262">陳述式中的分行符號陳述式上的沒有作用，而且您可以包裝陳述式，以提高可讀性。</span><span class="sxs-lookup"><span data-stu-id="be547-262">A line break in a statement has no effect on the statement, and you can wrap statements for readability.</span></span> <span data-ttu-id="be547-263">下列陳述式都相同：</span><span class="sxs-lookup"><span data-stu-id="be547-263">The following statements are the same:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample16.cshtml)]

<span data-ttu-id="be547-264">不過，您無法包裝字串常值的中間一條線。</span><span class="sxs-lookup"><span data-stu-id="be547-264">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="be547-265">下列範例無法運作︰</span><span class="sxs-lookup"><span data-stu-id="be547-265">The following example doesn't work:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample17.cshtml)]

<span data-ttu-id="be547-266">若要結合換行至類似上述的程式碼的多行的長字串，有兩個選項。</span><span class="sxs-lookup"><span data-stu-id="be547-266">To combine a long string that wraps to multiple lines like the above code, there are two options.</span></span> <span data-ttu-id="be547-267">您可以使用串連運算子 (`+`)，您將在本文稍後看到。</span><span class="sxs-lookup"><span data-stu-id="be547-267">You can use the concatenation operator (`+`), which you'll see later in this article.</span></span> <span data-ttu-id="be547-268">您也可以使用`@`建立逐字字串常值，如您稍早在本文中所見的字元。</span><span class="sxs-lookup"><span data-stu-id="be547-268">You can also use the `@` character to create a verbatim string literal, as you saw earlier in this article.</span></span> <span data-ttu-id="be547-269">您可以跨行中斷逐字字串常值：</span><span class="sxs-lookup"><span data-stu-id="be547-269">You can break verbatim string literals across lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample18.cshtml)]

### <a name="code-and-markup-comments"></a><span data-ttu-id="be547-270">程式碼 （和標記） 的註解</span><span class="sxs-lookup"><span data-stu-id="be547-270">Code (and Markup) Comments</span></span>

<span data-ttu-id="be547-271">註解可讓您為您自己或其他保留資訊。</span><span class="sxs-lookup"><span data-stu-id="be547-271">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="be547-272">它們也可讓您停用 (*標記為註解*) 程式碼或標記，您不想要執行，但想要保留您目前的頁面中的區段。</span><span class="sxs-lookup"><span data-stu-id="be547-272">They also allow you to disable (*comment out*) a section of code or markup that you don't want to run but want to keep in your page for the time being.</span></span>

<span data-ttu-id="be547-273">沒有其他註解的 Razor 程式碼和 HTML 標記的語法。</span><span class="sxs-lookup"><span data-stu-id="be547-273">There's different commenting syntax for Razor code and for HTML markup.</span></span> <span data-ttu-id="be547-274">如同所有 Razor 程式碼中，Razor 註解會處理 （然後移除） 之前的網頁傳送到瀏覽器在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="be547-274">As with all Razor code, Razor comments are processed (and then removed) on the server before the page is sent to the browser.</span></span> <span data-ttu-id="be547-275">因此，Razor 註解語法可讓您將註解放在程式碼 （或甚至到標記），您可以看到當您編輯檔案，但使用者沒有看到，即使是在網頁原始檔。</span><span class="sxs-lookup"><span data-stu-id="be547-275">Therefore, the Razor commenting syntax lets you put comments into the code (or even into the markup) that you can see when you edit the file, but that users don't see, even in the page source.</span></span>

<span data-ttu-id="be547-276">ASP.NET Razor 註解，您開始的註解`@*`，且結尾必須與`*@`。</span><span class="sxs-lookup"><span data-stu-id="be547-276">For ASP.NET Razor comments, you start the comment with `@*` and end it with `*@`.</span></span> <span data-ttu-id="be547-277">註解可以在一行或多行︰</span><span class="sxs-lookup"><span data-stu-id="be547-277">The comment can be on one line or multiple lines:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample19.cshtml)]

<span data-ttu-id="be547-278">以下是程式碼區塊中的註解：</span><span class="sxs-lookup"><span data-stu-id="be547-278">Here is a comment within a code block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample20.cshtml)]

<span data-ttu-id="be547-279">以下相同的程式碼區塊，使用程式碼行標記為註解，讓它不會執行：</span><span class="sxs-lookup"><span data-stu-id="be547-279">Here is the same block of code, with the line of code commented out so that it won't run:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample21.cshtml)]

<span data-ttu-id="be547-280">程式碼區塊中，若要使用 Razor 註解語法，或者您可以使用註解您使用，例如 C# 的程式設計語言的語法：</span><span class="sxs-lookup"><span data-stu-id="be547-280">Inside a code block, as an alternative to using Razor comment syntax, you can use the commenting syntax of the programming language you're using, such as C#:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample22.cshtml)]

<span data-ttu-id="be547-281">在 C# 中，單行註解前面會加上`//`字元和多行註解開頭`/*`結尾`*/`。</span><span class="sxs-lookup"><span data-stu-id="be547-281">In C#, single-line comments are preceded by the `//` characters, and multi-line comments begin with `/*` and end with `*/`.</span></span> <span data-ttu-id="be547-282">（如同 Razor 註解，C# 註解不會轉譯至瀏覽器。）</span><span class="sxs-lookup"><span data-stu-id="be547-282">(As with Razor comments, C# comments are not rendered to the browser.)</span></span>

<span data-ttu-id="be547-283">標記，您可能也知道，您可以建立於 HTML 註解：</span><span class="sxs-lookup"><span data-stu-id="be547-283">For markup, as you probably know, you can create an HTML comment:</span></span>

[!code-xml[Main](introducing-razor-syntax-c/samples/sample23.xml)]

<span data-ttu-id="be547-284">HTML 註解開頭`<!--`字元，並以結尾`-->`。</span><span class="sxs-lookup"><span data-stu-id="be547-284">HTML comments start with `<!--` characters and end with `-->`.</span></span> <span data-ttu-id="be547-285">您可以使用 HTML 註解來括住的文字，不僅也任何 HTML 標記，您可能想要保留的頁面中，但不想要呈現。</span><span class="sxs-lookup"><span data-stu-id="be547-285">You can use HTML comments to surround not only text, but also any HTML markup that you may want to keep in the page but don't want to render.</span></span> <span data-ttu-id="be547-286">此 HTML 註解會隱藏的標記和其所包含的文字的整個內容：</span><span class="sxs-lookup"><span data-stu-id="be547-286">This HTML comment will hide the entire content of the tags and the text they contain:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample24.html)]

<span data-ttu-id="be547-287">不同於 Razor 註解，HTML 註解*是*在網頁上呈現和使用者可以檢視網頁原始檔來查看它們。</span><span class="sxs-lookup"><span data-stu-id="be547-287">Unlike Razor comments, HTML comments *are* rendered to the page and the user can see them by viewing the page source.</span></span>

<span data-ttu-id="be547-288">Razor 會對 C# 的巢狀區塊中的限制。</span><span class="sxs-lookup"><span data-stu-id="be547-288">Razor has limitations on nested blocks of C#.</span></span> <span data-ttu-id="be547-289">如需詳細資訊，請參閱[名為 C# 變數和巢狀區塊產生中斷程式碼](http://aspnetwebstack.codeplex.com/workitem/1914)</span><span class="sxs-lookup"><span data-stu-id="be547-289">For more information see [Named C# Variables and Nested Blocks Generate Broken Code](http://aspnetwebstack.codeplex.com/workitem/1914)</span></span>

## <a name="variables"></a><span data-ttu-id="be547-290">變數</span><span class="sxs-lookup"><span data-stu-id="be547-290">Variables</span></span>

<span data-ttu-id="be547-291">變數是您用來儲存資料的具名的物件。</span><span class="sxs-lookup"><span data-stu-id="be547-291">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="be547-292">您可以任何項目，名稱變數名稱必須以字母字元開頭但不能包含空格或保留的字元。</span><span class="sxs-lookup"><span data-stu-id="be547-292">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="be547-293">變數和資料類型</span><span class="sxs-lookup"><span data-stu-id="be547-293">Variables and Data Types</span></span>

<span data-ttu-id="be547-294">變數可以有特定的資料類型，表示何種資料儲存在變數中。</span><span class="sxs-lookup"><span data-stu-id="be547-294">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="be547-295">您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在各種不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數).</span><span class="sxs-lookup"><span data-stu-id="be547-295">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="be547-296">而且有許多您可以使用其他資料型別。</span><span class="sxs-lookup"><span data-stu-id="be547-296">And there are many other data types you can use.</span></span>

<span data-ttu-id="be547-297">不過，您通常不需要指定變數的類型。</span><span class="sxs-lookup"><span data-stu-id="be547-297">However, you generally don't have to specify a type for a variable.</span></span> <span data-ttu-id="be547-298">大部分的情況下，ASP.NET 可以找出如何使用變數中的資料為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="be547-298">Most of the time, ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="be547-299">（有時候您必須指定一個類型，您會看到範例即為 true）。</span><span class="sxs-lookup"><span data-stu-id="be547-299">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="be547-300">您宣告變數使用`var`關鍵字 （如果您不想要指定型別） 或使用型別名稱：</span><span class="sxs-lookup"><span data-stu-id="be547-300">You declare a variable using the `var` keyword (if you don't want to specify a type) or by using the name of the type:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample25.cshtml)]

<span data-ttu-id="be547-301">下列範例會顯示變數的部分典型用途，在網頁中：</span><span class="sxs-lookup"><span data-stu-id="be547-301">The following example shows some typical uses of variables in a web page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample26.cshtml)]

<span data-ttu-id="be547-302">如果您結合先前的範例，在頁面中，您會看到瀏覽器中顯示：</span><span class="sxs-lookup"><span data-stu-id="be547-302">If you combine the previous examples in a page, you see this displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-c/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="be547-304">轉換和測試資料類型</span><span class="sxs-lookup"><span data-stu-id="be547-304">Converting and Testing Data Types</span></span>

<span data-ttu-id="be547-305">雖然 ASP.NET 通常可以自動判斷資料型別，有時它不可轉換。</span><span class="sxs-lookup"><span data-stu-id="be547-305">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="be547-306">因此，您可能需要協助 ASP.NET 執行的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="be547-306">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="be547-307">即使您沒有轉換的型別，有時候很有幫助測試以查看何種資料類型您可能會使用。</span><span class="sxs-lookup"><span data-stu-id="be547-307">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="be547-308">最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。</span><span class="sxs-lookup"><span data-stu-id="be547-308">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="be547-309">下列範例示範一般的情況下，您必須將字串轉換為數字。</span><span class="sxs-lookup"><span data-stu-id="be547-309">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample27.cshtml)]

<span data-ttu-id="be547-310">因此，使用者輸入來到您為字串。</span><span class="sxs-lookup"><span data-stu-id="be547-310">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="be547-311">即使您已提示使用者輸入數字，即使它們已送出使用者輸入和您在程式碼中讀取時所輸入數字、 資料的格式字串。</span><span class="sxs-lookup"><span data-stu-id="be547-311">Even if you've prompted users to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="be547-312">因此，您必須將字串轉換為數字。</span><span class="sxs-lookup"><span data-stu-id="be547-312">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="be547-313">在範例中，如果您嘗試對值執行算術運算，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：</span><span class="sxs-lookup"><span data-stu-id="be547-313">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

<span data-ttu-id="be547-314">*無法隱含地轉換成 'int'，' string' 類型。*</span><span class="sxs-lookup"><span data-stu-id="be547-314">*Cannot implicitly convert type 'string' to 'int'.*</span></span>

<span data-ttu-id="be547-315">若要轉換成整數的值，請呼叫`AsInt`方法。</span><span class="sxs-lookup"><span data-stu-id="be547-315">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="be547-316">如果轉換成功，然後您可以加入數字。</span><span class="sxs-lookup"><span data-stu-id="be547-316">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="be547-317">下表列出一些常見的轉換和測試方法的變數。</span><span class="sxs-lookup"><span data-stu-id="be547-317">The following table lists some common conversion and test methods for variables.</span></span>

<span data-ttu-id="be547-318">::: 資料列:::::: 資料行:::<strong>方法</strong>::: 資料行後端:::::: 資料行:::<strong>描述</strong>::: 資料行後端:::::: 資料行:::<strong>範例</strong>::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-318">:::row::: :::column::: <strong>Method</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Example</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-319">::: 資料列:::::: 資料行::: `AsInt(), IsInt()` ::: 資料行後端:::::: 資料行::: 轉換字串，表示為整數的整數 （例如"593")。</span><span class="sxs-lookup"><span data-stu-id="be547-319">:::row::: :::column::: `AsInt(), IsInt()` :::column-end::: :::column::: Converts a string that represents a whole number (like "593") to an integer.</span></span>
<span data-ttu-id="be547-320">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-320">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample28.cs)]</span></span>
    <span data-ttu-id="be547-321">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-321">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-322">::: 資料列:::::: 資料行::: `AsBool(), IsBool()` ::: 資料行後端:::::: 資料行::: 轉換字串 like &quot;，則為 true&quot;或&quot;false&quot;布林型別。</span><span class="sxs-lookup"><span data-stu-id="be547-322">:::row::: :::column::: `AsBool(), IsBool()` :::column-end::: :::column::: Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
<span data-ttu-id="be547-323">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-323">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample29.cs)]</span></span>
    <span data-ttu-id="be547-324">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-324">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-325">::: 資料列:::::: 資料行::: `AsFloat(), IsFloat()` ::: 資料行後端:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;浮點數。</span><span class="sxs-lookup"><span data-stu-id="be547-325">:::row::: :::column::: `AsFloat(), IsFloat()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
<span data-ttu-id="be547-326">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-326">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample30.cs)]</span></span>
    <span data-ttu-id="be547-327">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-327">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-328">::: 資料列:::::: 資料行::: `AsDecimal(), IsDecimal()` ::: 資料行後端:::::: 資料行::: 將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;十進位數字。</span><span class="sxs-lookup"><span data-stu-id="be547-328">:::row::: :::column::: `AsDecimal(), IsDecimal()` :::column-end::: :::column::: Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="be547-329">（在 ASP.NET 中，十進位數字是更精確比浮點數）。::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-329">(In ASP.NET, a decimal number is more precise than a floating-point number.) :::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample31.cs)]</span></span>
    <span data-ttu-id="be547-330">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-330">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-331">::: 資料列:::::: 資料行::: `AsDateTime(), IsDateTime()` ::: 資料行後端:::::: 資料行::: 將 asp.net 代表的日期和時間值的字串轉換`DateTime`型別。</span><span class="sxs-lookup"><span data-stu-id="be547-331">:::row::: :::column::: `AsDateTime(), IsDateTime()` :::column-end::: :::column::: Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
<span data-ttu-id="be547-332">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-332">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample32.cs)]</span></span>
    <span data-ttu-id="be547-333">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-333">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-334">::: 資料列:::::: 資料行::: `ToString()` ::: 資料行後端:::::: 資料行::: 將其他任何資料類型轉換為字串。</span><span class="sxs-lookup"><span data-stu-id="be547-334">:::row::: :::column::: `ToString()` :::column-end::: :::column::: Converts any other data type to a string.</span></span>
<span data-ttu-id="be547-335">::: 資料行後端:::::: 資料行::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]</span><span class="sxs-lookup"><span data-stu-id="be547-335">:::column-end::: :::column::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample33.js)]</span></span>
    <span data-ttu-id="be547-336">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-336">:::column-end::: :::row-end:::</span></span>

## <a name="operators"></a><span data-ttu-id="be547-337">運算子</span><span class="sxs-lookup"><span data-stu-id="be547-337">Operators</span></span>

<span data-ttu-id="be547-338">運算子是關鍵字或字元，會告訴 ASP.NET 在運算式中執行命令的類型。</span><span class="sxs-lookup"><span data-stu-id="be547-338">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="be547-339">C# 語言 （和 Razor 語法為基礎） 支援許多運算子，但您只需要識別一些開始使用。</span><span class="sxs-lookup"><span data-stu-id="be547-339">The C# language (and the Razor syntax that's based on it) supports many operators, but you only need to recognize a few to get started.</span></span> <span data-ttu-id="be547-340">下表摘要說明最常見的運算子。</span><span class="sxs-lookup"><span data-stu-id="be547-340">The following table summarizes the most common operators.</span></span>


<span data-ttu-id="be547-341">::: 資料列:::::: 資料行:::<strong>運算子</strong>::: 資料行後端:::::: 資料行:::<strong>描述</strong>::: 資料行後端:::::: 資料行:::<strong>範例</strong>::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-341">:::row::: :::column::: <strong>Operator</strong> :::column-end::: :::column::: <strong>Description</strong> :::column-end::: :::column::: <strong>Examples</strong> :::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-342">::: 資料列:::::: 資料行::: `+` `-` `*` `/` ::: 資料行後端:::::: 資料行::: 用在數值運算式的數學運算子。</span><span class="sxs-lookup"><span data-stu-id="be547-342">:::row::: :::column::: `+` `-` `*` `/` :::column-end::: :::column::: Math operators used in numerical expressions.</span></span>
<span data-ttu-id="be547-343">::: 資料行後端:::::: 資料行::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]</span><span class="sxs-lookup"><span data-stu-id="be547-343">:::column-end::: :::column::: [!code-css[Main](introducing-razor-syntax-c/samples/sample34.css)]</span></span>
    <span data-ttu-id="be547-344">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-344">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-345">::: 資料列:::::: 資料行::: `=` ::: 資料行後端:::::: 資料行::: 指派。</span><span class="sxs-lookup"><span data-stu-id="be547-345">:::row::: :::column::: `=` :::column-end::: :::column::: Assignment.</span></span> <span data-ttu-id="be547-346">將陳述式的右邊的值指派給左邊的物件中。</span><span class="sxs-lookup"><span data-stu-id="be547-346">Assigns the value on the right side of a statement to the object on the left side.</span></span>
<span data-ttu-id="be547-347">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-347">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample35.cs)]</span></span>
    <span data-ttu-id="be547-348">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-348">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-349">::: 資料列:::::: 資料行::: `==` ::: 資料行後端:::::: 資料行::: 等號比較。</span><span class="sxs-lookup"><span data-stu-id="be547-349">:::row::: :::column::: `==` :::column-end::: :::column::: Equality.</span></span> <span data-ttu-id="be547-350">傳回`true`值是否相等。</span><span class="sxs-lookup"><span data-stu-id="be547-350">Returns `true` if the values are equal.</span></span> <span data-ttu-id="be547-351">(請注意區分`=`運算子和`==`運算子。)::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-351">(Notice the distinction between the `=` operator and the `==` operator.) :::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample36.cs)]</span></span>
    <span data-ttu-id="be547-352">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-352">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-353">::: 資料列:::::: 資料行::: `!=` ::: 資料行後端:::::: 資料行::: 不等比較。</span><span class="sxs-lookup"><span data-stu-id="be547-353">:::row::: :::column::: `!=` :::column-end::: :::column::: Inequality.</span></span> <span data-ttu-id="be547-354">傳回`true`值是否不相等。</span><span class="sxs-lookup"><span data-stu-id="be547-354">Returns `true` if the values are not equal.</span></span>
<span data-ttu-id="be547-355">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-355">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample37.cs)]</span></span>
    <span data-ttu-id="be547-356">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-356">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-357">::: 資料列:::::: 資料行::: `< > <= >=` ::: 資料行後端:::::: 資料行::: 較少-相比，更-相比，小於-或-等於、 與大於或等於。</span><span class="sxs-lookup"><span data-stu-id="be547-357">:::row::: :::column::: `< > <= >=` :::column-end::: :::column::: Less-than, greater-than, less-than-or-equal, and greater-than-or-equal.</span></span>
<span data-ttu-id="be547-358">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-358">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample38.cs)]</span></span>
    <span data-ttu-id="be547-359">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-359">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-360">::: 資料列:::::: 資料行::: `+` ::: 資料行後端:::::: 資料行::: 用來將字串的串連。</span><span class="sxs-lookup"><span data-stu-id="be547-360">:::row::: :::column::: `+` :::column-end::: :::column::: Concatenation, which is used to join strings.</span></span> <span data-ttu-id="be547-361">ASP.NET 會知道此運算子和運算式的資料類型的加法運算子之間的差異。</span><span class="sxs-lookup"><span data-stu-id="be547-361">ASP.NET knows the difference between this operator and the addition operator based on the data type of the expression.</span></span>
<span data-ttu-id="be547-362">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-362">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample39.cs)]</span></span>
    <span data-ttu-id="be547-363">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-363">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-364">::: 資料列:::::: 資料行::: `+=` `-=` ::: 資料行後端:::::: 資料行::: 遞增和遞減運算子，以新增和從變數 （分別） 減 1。</span><span class="sxs-lookup"><span data-stu-id="be547-364">:::row::: :::column::: `+=` `-=` :::column-end::: :::column::: The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
<span data-ttu-id="be547-365">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-365">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample40.cs)]</span></span>
    <span data-ttu-id="be547-366">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-366">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-367">::: 資料列:::::: 資料行::: `.` ::: 資料行後端:::::: 資料行::: 點。</span><span class="sxs-lookup"><span data-stu-id="be547-367">:::row::: :::column::: `.` :::column-end::: :::column::: Dot.</span></span> <span data-ttu-id="be547-368">用來區別物件及其屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="be547-368">Used to distinguish objects and their properties and methods.</span></span>
<span data-ttu-id="be547-369">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-369">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample41.cs)]</span></span>
    <span data-ttu-id="be547-370">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-370">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-371">::: 資料列:::::: 資料行::: `()` ::: 資料行後端:::::: 資料行::: 括號。</span><span class="sxs-lookup"><span data-stu-id="be547-371">:::row::: :::column::: `()` :::column-end::: :::column::: Parentheses.</span></span> <span data-ttu-id="be547-372">用來群組運算式，並將參數傳遞給方法。</span><span class="sxs-lookup"><span data-stu-id="be547-372">Used to group expressions and to pass parameters to methods.</span></span>
<span data-ttu-id="be547-373">::: 資料行後端:::::: 資料行::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]</span><span class="sxs-lookup"><span data-stu-id="be547-373">:::column-end::: :::column::: [!code-javascript[Main](introducing-razor-syntax-c/samples/sample42.js)]</span></span>
    <span data-ttu-id="be547-374">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-374">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-375">::: 資料列:::::: 資料行::: `[]` ::: 資料行後端:::::: 資料行::: 方括號。</span><span class="sxs-lookup"><span data-stu-id="be547-375">:::row::: :::column::: `[]` :::column-end::: :::column::: Brackets.</span></span> <span data-ttu-id="be547-376">用來存取陣列或集合中的值。</span><span class="sxs-lookup"><span data-stu-id="be547-376">Used for accessing values in arrays or collections.</span></span>
<span data-ttu-id="be547-377">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-377">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample43.cs)]</span></span>
    <span data-ttu-id="be547-378">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-378">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-379">::: 資料列:::::: 資料行::: `!` ::: 資料行後端:::::: 資料行::: 沒有。</span><span class="sxs-lookup"><span data-stu-id="be547-379">:::row::: :::column::: `!` :::column-end::: :::column::: Not.</span></span> <span data-ttu-id="be547-380">反轉`true`值`false`，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="be547-380">Reverses a `true` value to `false` and vice versa.</span></span> <span data-ttu-id="be547-381">常用縮寫來測試`false`(也就是針對不`true`)。</span><span class="sxs-lookup"><span data-stu-id="be547-381">Typically used as a shorthand way to test for `false` (that is, for not `true`).</span></span>
<span data-ttu-id="be547-382">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-382">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample44.cs)]</span></span>
    <span data-ttu-id="be547-383">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-383">:::column-end::: :::row-end:::</span></span>
* * *
<span data-ttu-id="be547-384">::: 資料列:::::: 資料行::: `&&` <code>&#124;&#124;</code> ::: 資料行後端:::::: 資料行::: 邏輯 AND 和 OR，這用來連結條件一起。</span><span class="sxs-lookup"><span data-stu-id="be547-384">:::row::: :::column::: `&&` <code>&#124;&#124;</code> :::column-end::: :::column::: Logical AND and OR, which are used to link conditions together.</span></span>
<span data-ttu-id="be547-385">::: 資料行後端:::::: 資料行::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]</span><span class="sxs-lookup"><span data-stu-id="be547-385">:::column-end::: :::column::: [!code-csharp[Main](introducing-razor-syntax-c/samples/sample45.cs)]</span></span>
    <span data-ttu-id="be547-386">::: 資料行後端:::::: 資料列結尾:::</span><span class="sxs-lookup"><span data-stu-id="be547-386">:::column-end::: :::row-end:::</span></span>

<a id="ID_WorkingWithFileAndFolderPaths"></a>
## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="be547-387">使用檔案和程式碼中的資料夾路徑</span><span class="sxs-lookup"><span data-stu-id="be547-387">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="be547-388">您通常會在您的程式碼中使用檔案和資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-388">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="be547-389">以下是網站實體資料夾結構的範例，可能出現在您的開發電腦上：</span><span class="sxs-lookup"><span data-stu-id="be547-389">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="be547-390">以下是一些關於 Url 和路徑的基本詳細資料：</span><span class="sxs-lookup"><span data-stu-id="be547-390">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="be547-391">開始使用的網域名稱的 URL (`http://www.example.com`) 或伺服器名稱 (`http://localhost`， `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="be547-391">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="be547-392">URL 對應至主機電腦上的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-392">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="be547-393">例如，`http://myserver`可能會對應至資料夾*C:\websites\mywebsite*伺服器上。</span><span class="sxs-lookup"><span data-stu-id="be547-393">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="be547-394">虛擬路徑是以代表在程式碼中的路徑，而不需要指定完整路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-394">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="be547-395">它包含的 url 的網域或伺服器名稱後面的部分。</span><span class="sxs-lookup"><span data-stu-id="be547-395">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="be547-396">當您使用虛擬路徑時，您可以移動至不同的網域或伺服器程式碼而不需要更新的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-396">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="be547-397">以下是範例，以協助您了解的差異：</span><span class="sxs-lookup"><span data-stu-id="be547-397">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="be547-398">完整的 URL</span><span class="sxs-lookup"><span data-stu-id="be547-398">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="be547-399">伺服器名稱</span><span class="sxs-lookup"><span data-stu-id="be547-399">Server name</span></span> | <span data-ttu-id="be547-400">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="be547-400">*mycompanyserver*</span></span> |
| <span data-ttu-id="be547-401">虛擬路徑</span><span class="sxs-lookup"><span data-stu-id="be547-401">Virtual path</span></span> | <span data-ttu-id="be547-402">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="be547-402">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="be547-403">實體路徑</span><span class="sxs-lookup"><span data-stu-id="be547-403">Physical path</span></span> | <span data-ttu-id="be547-404">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="be547-404">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="be547-405">虛擬根目錄是 /，就像根目錄的 c： 磁碟機 \。</span><span class="sxs-lookup"><span data-stu-id="be547-405">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="be547-406">（虛擬資料夾路徑永遠使用正斜線）。資料夾的虛擬路徑不需要有相同的名稱做為實體的資料夾;它可以是一個別名。</span><span class="sxs-lookup"><span data-stu-id="be547-406">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="be547-407">（在實際執行伺服器上的虛擬路徑很少比對確切的實體路徑。）</span><span class="sxs-lookup"><span data-stu-id="be547-407">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="be547-408">當您使用中的程式碼檔案和資料夾時，有時候您需要參考的實體路徑或按一下 虛擬路徑，視您正在使用哪些物件而定。</span><span class="sxs-lookup"><span data-stu-id="be547-408">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="be547-409">ASP.NET 讓您知道這些工具使用程式碼中的檔案和資料夾路徑：`Server.MapPath`方法，而`~`運算子和`Href`方法。</span><span class="sxs-lookup"><span data-stu-id="be547-409">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="be547-410">轉換虛擬與實體路徑： Server.MapPath 方法</span><span class="sxs-lookup"><span data-stu-id="be547-410">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="be547-411">`Server.MapPath`方法會將轉換的虛擬路徑 (例如 */default.cshtml*) 成絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="be547-411">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="be547-412">每當您需要完整的實體路徑使用這個方法。</span><span class="sxs-lookup"><span data-stu-id="be547-412">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="be547-413">當您讀取或寫入的文字檔或 web 伺服器上的映像檔，就會是一個典型的例子。</span><span class="sxs-lookup"><span data-stu-id="be547-413">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="be547-414">您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，因此這個方法可以將路徑轉換您知道 — 虛擬路徑，來為您在伺服器上對應的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-414">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="be547-415">您將虛擬路徑傳遞至檔案或資料夾的方法，並傳回的實體路徑：</span><span class="sxs-lookup"><span data-stu-id="be547-415">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample46.cshtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="be547-416">參考的虛擬根目錄： ~ 運算子和 Href 方法</span><span class="sxs-lookup"><span data-stu-id="be547-416">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="be547-417">在  *.cshtml*或 *.vbhtml*檔案中，您可以參考虛擬根目錄的路徑使用`~`運算子。</span><span class="sxs-lookup"><span data-stu-id="be547-417">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="be547-418">這是非常好用，因為您可以移動的頁面，在網站中，而且它們包含其他頁面的任何連結不會中斷。</span><span class="sxs-lookup"><span data-stu-id="be547-418">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="be547-419">如果您曾經將您的網站移至不同的位置還有好用。</span><span class="sxs-lookup"><span data-stu-id="be547-419">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="be547-420">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="be547-420">Here are some examples:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample47.cshtml)]

<span data-ttu-id="be547-421">如果網站是`http://myserver/myapp`，以下是 ASP.NET 如何處理此頁面會執行的這些路徑：</span><span class="sxs-lookup"><span data-stu-id="be547-421">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="be547-422">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="be547-422">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="be547-423">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="be547-423">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="be547-424">（您實際上不會看到這些路徑做為變數的值，但，是它們的是，ASP.NET 會將路徑）。</span><span class="sxs-lookup"><span data-stu-id="be547-424">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="be547-425">您可以使用`~`運算子伺服端程式碼 （如上所述） 並在標記中，像這樣：</span><span class="sxs-lookup"><span data-stu-id="be547-425">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample48.html)]

<span data-ttu-id="be547-426">在標記中，您可以使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-426">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="be547-427">ASP.NET 頁面執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-427">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="be547-428">條件式邏輯和迴圈</span><span class="sxs-lookup"><span data-stu-id="be547-428">Conditional Logic and Loops</span></span>

<span data-ttu-id="be547-429">ASP.NET server 程式碼可讓您執行以條件為基礎的工作，並撰寫重複陳述式的程式碼次數 （也就是程式碼會執行迴圈）。</span><span class="sxs-lookup"><span data-stu-id="be547-429">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times (that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="be547-430">測試條件</span><span class="sxs-lookup"><span data-stu-id="be547-430">Testing Conditions</span></span>

<span data-ttu-id="be547-431">若要測試您所使用的簡單條件`if`陳述式，此傳回 true 或 false 取決於您所指定的測試：</span><span class="sxs-lookup"><span data-stu-id="be547-431">To test a simple condition you use the `if` statement, which returns true or false based on a test you specify:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample49.cshtml)]

<span data-ttu-id="be547-432">`if`關鍵字開始區塊。</span><span class="sxs-lookup"><span data-stu-id="be547-432">The `if` keyword starts a block.</span></span> <span data-ttu-id="be547-433">實際的測試 （條件） 是括號括住，並傳回 true 或 false。</span><span class="sxs-lookup"><span data-stu-id="be547-433">The actual test (condition) is in parentheses and returns true or false.</span></span> <span data-ttu-id="be547-434">執行測試結果為 true 的陳述式會括在大括號。</span><span class="sxs-lookup"><span data-stu-id="be547-434">The statements that run if the test is true are enclosed in braces.</span></span> <span data-ttu-id="be547-435">`if`陳述式可包含`else`，指定當條件為 false 時要執行的陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="be547-435">An `if` statement can include an `else` block that specifies statements to run if the condition is false:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample50.cshtml)]

<span data-ttu-id="be547-436">您可以加入多個條件使用`else if`區塊：</span><span class="sxs-lookup"><span data-stu-id="be547-436">You can add multiple conditions using an `else if` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample51.cshtml)]

<span data-ttu-id="be547-437">在此範例中，如果第一個條件，在 if 區塊不是 true，`else if`在檢查條件。</span><span class="sxs-lookup"><span data-stu-id="be547-437">In this example, if the first condition in the if block is not true, the `else if` condition is checked.</span></span> <span data-ttu-id="be547-438">如果符合該條件中的陳述式`else if`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="be547-438">If that condition is met, the statements in the `else if` block are executed.</span></span> <span data-ttu-id="be547-439">如果條件符合中的陳述式`else`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="be547-439">If none of the conditions are met, the statements in the `else` block are executed.</span></span> <span data-ttu-id="be547-440">您可以新增任意數目的 else 的 if 區塊中使用，並關閉與`else`封鎖&quot;一切&quot;條件。</span><span class="sxs-lookup"><span data-stu-id="be547-440">You can add any number of else if blocks, and then close with an `else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="be547-441">若要測試大量的條件，請使用`switch`區塊：</span><span class="sxs-lookup"><span data-stu-id="be547-441">To test a large number of conditions, use a `switch` block:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample52.cshtml)]

<span data-ttu-id="be547-442">要測試的值位於括號內 (在範例中，`weekday`變數)。</span><span class="sxs-lookup"><span data-stu-id="be547-442">The value to test is in parentheses (in the example, the `weekday` variable).</span></span> <span data-ttu-id="be547-443">每個個別的測試會使用`case`陳述式的結尾是冒號 （:）。</span><span class="sxs-lookup"><span data-stu-id="be547-443">Each individual test uses a `case` statement that ends with a colon (:).</span></span> <span data-ttu-id="be547-444">如果值`case`陳述式符合測試值，該案例區塊中的程式碼會執行。</span><span class="sxs-lookup"><span data-stu-id="be547-444">If the value of a `case` statement matches the test value, the code in that case block is executed.</span></span> <span data-ttu-id="be547-445">關閉與每個 case 陳述式`break`陳述式。</span><span class="sxs-lookup"><span data-stu-id="be547-445">You close each case statement with a `break` statement.</span></span> <span data-ttu-id="be547-446">(如果您忘記在每個包含中斷`case`封鎖，請從下一個程式碼`case`陳述式也會執行。)A`switch`區塊通常會有`default`陳述式的最後一個案例作為&quot;一切&quot;執行如果沒有任何其他情況下，則為 true 的選項。</span><span class="sxs-lookup"><span data-stu-id="be547-446">(If you forget to include break in each `case` block, the code from the next `case` statement will run also.) A `switch` block often has a `default` statement as the last case for an &quot;everything else&quot; option that runs if none of the other cases are true.</span></span>

<span data-ttu-id="be547-447">在瀏覽器中顯示的最後兩個條件式區塊的結果：</span><span class="sxs-lookup"><span data-stu-id="be547-447">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-c/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="be547-449">迴圈的程式碼</span><span class="sxs-lookup"><span data-stu-id="be547-449">Looping Code</span></span>

<span data-ttu-id="be547-450">您通常需要重複執行相同的陳述式。</span><span class="sxs-lookup"><span data-stu-id="be547-450">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="be547-451">您可以進行迴圈。</span><span class="sxs-lookup"><span data-stu-id="be547-451">You do this by looping.</span></span> <span data-ttu-id="be547-452">例如，您經常執行相同的陳述式，每個項目集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="be547-452">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="be547-453">如果您知道確實多少次您想要執行迴圈，您可以使用`for`迴圈。</span><span class="sxs-lookup"><span data-stu-id="be547-453">If you know exactly how many times you want to loop, you can use a `for` loop.</span></span> <span data-ttu-id="be547-454">這種迴圈是特別適用於計算或向下計數：</span><span class="sxs-lookup"><span data-stu-id="be547-454">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample53.html)]

<span data-ttu-id="be547-455">迴圈開頭`for`每個以分號結束關鍵字，後面接著括號中的三個陳述式。</span><span class="sxs-lookup"><span data-stu-id="be547-455">The loop begins with the `for` keyword, followed by three statements in parentheses, each terminated with a semicolon.</span></span>

- <span data-ttu-id="be547-456">在括弧內，第一個陳述式 (`var i=10;`) 建立計數器，並且將它初始化為 10。</span><span class="sxs-lookup"><span data-stu-id="be547-456">Inside the parentheses, the first statement (`var i=10;`) creates a counter and initializes it to 10.</span></span> <span data-ttu-id="be547-457">您不需要名稱計數器`i`&#8212;您可以使用任何變數。</span><span class="sxs-lookup"><span data-stu-id="be547-457">You don't have to name the counter `i` &#8212; you can use any variable.</span></span> <span data-ttu-id="be547-458">當`for`迴圈執行時，會自動遞增的計數器。</span><span class="sxs-lookup"><span data-stu-id="be547-458">When the `for` loop runs, the counter is automatically incremented.</span></span>
- <span data-ttu-id="be547-459">第二個陳述式 (`i < 21;`) 設定條件，以您想要計算的幅度。</span><span class="sxs-lookup"><span data-stu-id="be547-459">The second statement (`i < 21;`) sets the condition for how far you want to count.</span></span> <span data-ttu-id="be547-460">您想在此情況下，請移至最多 20 個 （也就繼續執行時的計數器是小於 21）。</span><span class="sxs-lookup"><span data-stu-id="be547-460">In this case, you want it to go to a maximum of 20 (that is, keep going while the counter is less than 21).</span></span>
- <span data-ttu-id="be547-461">第三個陳述式 (`i++` ) 會使用遞增運算子，只需指定 計數器應有 1 加入至它在每次執行迴圈。</span><span class="sxs-lookup"><span data-stu-id="be547-461">The third statement (`i++` ) uses an increment operator, which simply specifies that the counter should have 1 added to it each time the loop runs.</span></span>

<span data-ttu-id="be547-462">括號內是迴圈的每個反覆項目，就會執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-462">Inside the braces is the code that will run for each iteration of the loop.</span></span> <span data-ttu-id="be547-463">標記會建立新的段落 (`<p>`項目) 每次，且輸出中顯示的值中加入一行`i`（計數器）。</span><span class="sxs-lookup"><span data-stu-id="be547-463">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of `i` (the counter).</span></span> <span data-ttu-id="be547-464">當您執行此頁面時，此範例會建立 11 行顯示輸出，表示項目數目每一行中的文字。</span><span class="sxs-lookup"><span data-stu-id="be547-464">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-c/_static/image11.jpg)

<span data-ttu-id="be547-466">如果您正在使用集合或陣列，您經常使用`foreach`迴圈。</span><span class="sxs-lookup"><span data-stu-id="be547-466">If you're working with a collection or array, you often use a `foreach` loop.</span></span> <span data-ttu-id="be547-467">集合是一組類似的物件，而`foreach`迴圈可讓您執行的工作集合中的每個項目上。</span><span class="sxs-lookup"><span data-stu-id="be547-467">A collection is a group of similar objects, and the `foreach` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="be547-468">這種類型的迴圈是方便的集合，因為不像`for`迴圈中，您不需要遞增計數器，或設定限制。</span><span class="sxs-lookup"><span data-stu-id="be547-468">This type of loop is convenient for collections, because unlike a `for` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="be547-469">相反地，`foreach`迴圈的程式碼會繼續透過集合直到完成為止。</span><span class="sxs-lookup"><span data-stu-id="be547-469">Instead, the `foreach` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="be547-470">例如，下列程式碼會傳回中的項目`Request.ServerVariables`集合，也就是物件，包含 web 伺服器的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="be547-470">For example, the following code returns the items in the `Request.ServerVariables` collection, which is an object that contains information about your web server.</span></span> <span data-ttu-id="be547-471">它會使用`foreac`h 的迴圈，以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="be547-471">It uses a `foreac` h loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-html[Main](introducing-razor-syntax-c/samples/sample54.html)]

<span data-ttu-id="be547-472">`foreach`關鍵字後面接著括號您用來宣告變數，表示集合中的單一項目 (在範例中， `var item`)，後面接著`in`關鍵字，後面接著您要循環的集合。</span><span class="sxs-lookup"><span data-stu-id="be547-472">The `foreach` keyword is followed by parentheses where you declare a variable that represents a single item in the collection (in the example, `var item`), followed by the `in` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="be547-473">本文的`foreach`迴圈中，您可以存取目前的項目使用您稍早宣告的變數。</span><span class="sxs-lookup"><span data-stu-id="be547-473">In the body of the `foreach` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-c/_static/image12.jpg)

<span data-ttu-id="be547-475">若要建立更通用的迴圈，請使用`while`陳述式：</span><span class="sxs-lookup"><span data-stu-id="be547-475">To create a more general-purpose loop, use the `while` statement:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample55.cshtml)]

<span data-ttu-id="be547-476">A`while`迴圈開頭`while`關鍵字，後面接著括號，您可以指定迴圈繼續的時間長度 (這裡，只要`countNum`小於 50)，然後重複的區塊。</span><span class="sxs-lookup"><span data-stu-id="be547-476">A `while` loop begins with the `while` keyword, followed by parentheses where you specify how long the loop continues (here, for as long as `countNum` is less than 50), then the block to repeat.</span></span> <span data-ttu-id="be547-477">迴圈通常遞增 （加入） 或遞減 （減去） 的變數或用於計算的物件。</span><span class="sxs-lookup"><span data-stu-id="be547-477">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="be547-478">在範例中，`+=`運算子會加入至 1`countNum`每次執行迴圈。</span><span class="sxs-lookup"><span data-stu-id="be547-478">In the example, the `+=` operator adds 1 to `countNum` each time the loop runs.</span></span> <span data-ttu-id="be547-479">(以遞減的向下計數的迴圈中的變數，您會使用遞減運算子`-=`)。</span><span class="sxs-lookup"><span data-stu-id="be547-479">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`).</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="be547-480">物件和集合</span><span class="sxs-lookup"><span data-stu-id="be547-480">Objects and Collections</span></span>

<span data-ttu-id="be547-481">幾乎在 ASP.NET 網站中所有項目是物件，包括網頁本身。</span><span class="sxs-lookup"><span data-stu-id="be547-481">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="be547-482">本章節將討論一些重要的物件，您將使用經常在您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-482">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="be547-483">頁面物件</span><span class="sxs-lookup"><span data-stu-id="be547-483">Page Objects</span></span>

<span data-ttu-id="be547-484">在 ASP.NET 中最基本的物件是頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-484">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="be547-485">您可以存取任何合格的物件不直接的 page 物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="be547-485">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="be547-486">下列程式碼會取得頁面的檔案路徑，使用`Request`頁面物件：</span><span class="sxs-lookup"><span data-stu-id="be547-486">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample56.cshtml)]

<span data-ttu-id="be547-487">要取消選取您要參考的屬性和目前的頁面物件上的方法，您可以選擇性地使用關鍵字`this`來代表您的程式碼中的頁面物件。</span><span class="sxs-lookup"><span data-stu-id="be547-487">To make it clear that you're referencing properties and methods on the current page object, you can optionally use the keyword `this` to represent the page object in your code.</span></span> <span data-ttu-id="be547-488">以下是上述的程式碼範例中，使用`this`新增至代表的頁面：</span><span class="sxs-lookup"><span data-stu-id="be547-488">Here is the previous code example, with `this` added to represent the page:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample57.cshtml)]

<span data-ttu-id="be547-489">您可以使用屬性`Page`物件，以取得大量資訊，例如：</span><span class="sxs-lookup"><span data-stu-id="be547-489">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="be547-490">`Request`.</span><span class="sxs-lookup"><span data-stu-id="be547-490">`Request`.</span></span> <span data-ttu-id="be547-491">如您所見，這是目前的要求，包括進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="be547-491">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="be547-492">`Response`.</span><span class="sxs-lookup"><span data-stu-id="be547-492">`Response`.</span></span> <span data-ttu-id="be547-493">這是會在伺服器程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="be547-493">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="be547-494">例如，您可以使用此屬性寫入至回應的資訊。</span><span class="sxs-lookup"><span data-stu-id="be547-494">For example, you can use this property to write information into the response.</span></span> 

    [!code-cshtml[Main](introducing-razor-syntax-c/samples/sample58.cshtml)]

<a id="ID_CollectionsAndObjects"></a>
### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="be547-495">（陣列和字典） 的集合物件</span><span class="sxs-lookup"><span data-stu-id="be547-495">Collection Objects (Arrays and Dictionaries)</span></span>

<span data-ttu-id="be547-496">A*集合*是一組相同的類型，例如的集合物件`Customer`資料庫的物件。</span><span class="sxs-lookup"><span data-stu-id="be547-496">A *collection* is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="be547-497">ASP.NET 包含許多的內建集合，例如`Request.Files`集合。</span><span class="sxs-lookup"><span data-stu-id="be547-497">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="be547-498">您通常會使用在集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="be547-498">You'll often work with data in collections.</span></span> <span data-ttu-id="be547-499">兩個常見的集合類型*陣列*並*字典*。</span><span class="sxs-lookup"><span data-stu-id="be547-499">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="be547-500">當您想要儲存一組類似的項目，但不想要建立個別的變數來保存每個項目時，陣列是很有用：</span><span class="sxs-lookup"><span data-stu-id="be547-500">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample59.cshtml)]

<span data-ttu-id="be547-501">使用陣列時，您宣告特定的資料類型，例如`string`， `int`，或`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="be547-501">With arrays, you declare a specific data type, such as `string`, `int`, or `DateTime`.</span></span> <span data-ttu-id="be547-502">若要指出變數可以包含陣列，您將加入宣告的方括號 (例如`string[]`或`int[]`)。</span><span class="sxs-lookup"><span data-stu-id="be547-502">To indicate that the variable can contain an array, you add brackets to the declaration (such as `string[]` or `int[]`).</span></span> <span data-ttu-id="be547-503">您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`foreach`陳述式。</span><span class="sxs-lookup"><span data-stu-id="be547-503">You can access items in an array using their position (index) or by using the `foreach` statement.</span></span> <span data-ttu-id="be547-504">陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。</span><span class="sxs-lookup"><span data-stu-id="be547-504">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample60.cshtml)]

<span data-ttu-id="be547-505">您可以判斷陣列中的項目數，藉由取得其`Length`屬性。</span><span class="sxs-lookup"><span data-stu-id="be547-505">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="be547-506">若要取得特定項目的陣列中 （如果您要搜尋的陣列） 的位置，請使用`Array.IndexOf`方法。</span><span class="sxs-lookup"><span data-stu-id="be547-506">To get the position of a specific item in the array (to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="be547-507">您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。</span><span class="sxs-lookup"><span data-stu-id="be547-507">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="be547-508">在瀏覽器中顯示的字串陣列程式碼的輸出：</span><span class="sxs-lookup"><span data-stu-id="be547-508">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-c/_static/image13.jpg)

<span data-ttu-id="be547-510">字典是索引鍵/值組的集合，您可在此提供金鑰 （或名稱），以設定或擷取對應的值：</span><span class="sxs-lookup"><span data-stu-id="be547-510">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample61.cshtml)]

<span data-ttu-id="be547-511">若要建立字典，您使用`new`關鍵字指出您要建立新的字典物件。</span><span class="sxs-lookup"><span data-stu-id="be547-511">To create a dictionary, you use the `new` keyword to indicate that you're creating a new dictionary object.</span></span> <span data-ttu-id="be547-512">您可以將字典指派給變數使用`var`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="be547-512">You can assign a dictionary to a variable using the `var` keyword.</span></span> <span data-ttu-id="be547-513">指出資料類型的字典使用角括弧中的項目 ( `< >` )。</span><span class="sxs-lookup"><span data-stu-id="be547-513">You indicate the data types of the items in the dictionary using angle brackets ( `< >` ).</span></span> <span data-ttu-id="be547-514">宣告的結尾，在中，您必須新增一組括號，因為這是實際的方法，建立新的字典。</span><span class="sxs-lookup"><span data-stu-id="be547-514">At the end of the declaration, you must add a pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="be547-515">若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定 索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="be547-515">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="be547-516">或者，您可以使用方括號表示索引鍵，並執行簡單的指派，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="be547-516">Alternatively, you can use square brackets to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample62.cs)]

<span data-ttu-id="be547-517">若要從字典取得值，指定金鑰括號括住：</span><span class="sxs-lookup"><span data-stu-id="be547-517">To get a value from the dictionary, you specify the key in brackets:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample63.cs)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="be547-518">呼叫具有參數的方法</span><span class="sxs-lookup"><span data-stu-id="be547-518">Calling Methods with Parameters</span></span>

<span data-ttu-id="be547-519">如同您稍早在本文中，您使用程式設計物件的方法。</span><span class="sxs-lookup"><span data-stu-id="be547-519">As you read earlier in this article, the objects that you program with can have methods.</span></span> <span data-ttu-id="be547-520">例如，`Database`物件可能會有`Database.Connect`方法。</span><span class="sxs-lookup"><span data-stu-id="be547-520">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="be547-521">許多的方法也會有一或多個參數。</span><span class="sxs-lookup"><span data-stu-id="be547-521">Many methods also have one or more parameters.</span></span> <span data-ttu-id="be547-522">A*參數*是一個值傳遞至方法，若要啟用以完成其工作的方法。</span><span class="sxs-lookup"><span data-stu-id="be547-522">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="be547-523">例如，看看的宣告`Request.MapPath`方法，後者會採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="be547-523">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-csharp[Main](introducing-razor-syntax-c/samples/sample64.cs)]

<span data-ttu-id="be547-524">（行已換行，讓它更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="be547-524">(The line has been wrapped to make it more readable.</span></span> <span data-ttu-id="be547-525">請記住，您可以將幾乎任何放置除了內部字串換行符號包含在引號中。)</span><span class="sxs-lookup"><span data-stu-id="be547-525">Remember that you can put line breaks almost any place except inside strings that are enclosed in quotation marks.)</span></span>

<span data-ttu-id="be547-526">這個方法會傳回對應的伺服器上的實體路徑，以指定的虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-526">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="be547-527">方法的三個參數都`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。</span><span class="sxs-lookup"><span data-stu-id="be547-527">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="be547-528">（請注意，在宣告中，列出參數的資料類型，它們會接受的資料）。當您呼叫這個方法時，您必須提供所有的三個參數的值。</span><span class="sxs-lookup"><span data-stu-id="be547-528">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="be547-529">Razor 語法可讓您將參數傳遞至方法的兩個選項：*位置參數*並*具名參數*。</span><span class="sxs-lookup"><span data-stu-id="be547-529">The Razor syntax gives you two options for passing parameters to a method: *positional parameters* and *named parameters*.</span></span> <span data-ttu-id="be547-530">若要呼叫使用位置參數的方法，您會以嚴格的順序，指定在方法宣告中傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="be547-530">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="be547-531">（您將通常知道此順序，請閱讀文件的方法）。您必須遵循的順序，以及您不能略過的任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或`null`您不需要的值為位置參數。</span><span class="sxs-lookup"><span data-stu-id="be547-531">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or `null` for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="be547-532">下列範例假設您有一個名為的資料夾*指令碼*在您的網站。</span><span class="sxs-lookup"><span data-stu-id="be547-532">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="be547-533">程式碼會呼叫`Request.MapPath`方法，並傳遞正確的順序的三個參數值。</span><span class="sxs-lookup"><span data-stu-id="be547-533">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="be547-534">然後，它會顯示產生的對應的路徑。</span><span class="sxs-lookup"><span data-stu-id="be547-534">It then displays the resulting mapped path.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample65.cshtml)]

<span data-ttu-id="be547-535">當方法有許多參數時，您可以使用具名參數，讓您的程式碼更容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="be547-535">When a method has many parameters, you can keep your code more readable by using named parameters.</span></span> <span data-ttu-id="be547-536">若要呼叫方法，使用具名的參數，您可以指定參數名稱後面接著冒號 （:），然後按一下 值。</span><span class="sxs-lookup"><span data-stu-id="be547-536">To call a method using named parameters, you specify the parameter name followed by a colon (:), and then the value.</span></span> <span data-ttu-id="be547-537">具名參數的優點是，您就可以將其傳遞任何您想要的順序。</span><span class="sxs-lookup"><span data-stu-id="be547-537">The advantage of named parameters is that you can pass them in any order you want.</span></span> <span data-ttu-id="be547-538">（一項缺點是，在方法呼叫並不會精簡）。</span><span class="sxs-lookup"><span data-stu-id="be547-538">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="be547-539">下列範例會呼叫與上述相同的方法，但使用具名參數，以提供值：</span><span class="sxs-lookup"><span data-stu-id="be547-539">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample66.cshtml)]

<span data-ttu-id="be547-540">如您所見，參數會傳遞不同的順序。</span><span class="sxs-lookup"><span data-stu-id="be547-540">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="be547-541">不過，如果您執行先前的範例和此範例中，它們會傳回相同的值。</span><span class="sxs-lookup"><span data-stu-id="be547-541">However, if you run the previous example and this example, they'll return the same value.</span></span>

<a id="ID_HandlingErrors"></a>
## <a name="handling-errors"></a><span data-ttu-id="be547-542">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="be547-542">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="be547-543">Try Catch 陳述式</span><span class="sxs-lookup"><span data-stu-id="be547-543">Try-Catch Statements</span></span>

<span data-ttu-id="be547-544">陳述式通常必須在程式碼中可能會失敗的原因，您無法控制。</span><span class="sxs-lookup"><span data-stu-id="be547-544">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="be547-545">例如: </span><span class="sxs-lookup"><span data-stu-id="be547-545">For example:</span></span>

- <span data-ttu-id="be547-546">如果您的程式碼會嘗試建立或存取的檔案，可能會發生各種錯誤。</span><span class="sxs-lookup"><span data-stu-id="be547-546">If your code tries to create or access a file, all sorts of errors might occur.</span></span> <span data-ttu-id="be547-547">您想要的檔案可能不存在，它可能會遭到鎖定，程式碼可能不具有權限，等等。</span><span class="sxs-lookup"><span data-stu-id="be547-547">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="be547-548">同樣地，如果您的程式碼會嘗試更新資料庫中的記錄，可以有權限問題，可能會卸除資料庫的連接，要儲存的資料可能無效，依此類推。</span><span class="sxs-lookup"><span data-stu-id="be547-548">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="be547-549">在程式設計的詞彙中，這些情況下則稱為*例外狀況*。</span><span class="sxs-lookup"><span data-stu-id="be547-549">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="be547-550">如果您的程式碼發生例外狀況，則會產生 （擲回） 的錯誤訊息，最惱人的使用者：</span><span class="sxs-lookup"><span data-stu-id="be547-550">If your code encounters an exception, it generates (throws) an error message that's, at best, annoying to users:</span></span>

![Razor-Img14](introducing-razor-syntax-c/_static/image14.jpg)

<span data-ttu-id="be547-552">在您的程式碼可能會遇到例外狀況的情況下，並以避免此類型的錯誤訊息，您可以使用`try/catch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="be547-552">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `try/catch` statements.</span></span> <span data-ttu-id="be547-553">在 `try`陳述式中，執行您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="be547-553">In the `try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="be547-554">在一或多個`catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="be547-554">In one or more `catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="be547-555">您可以包含更多`catch`陳述式，為您要尋找您預期的錯誤。</span><span class="sxs-lookup"><span data-stu-id="be547-555">You can include as many `catch` statements as you need to look for errors that you are anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="be547-556">我們建議您避免使用`Response.Redirect`方法中的`try/catch`陳述式，因為它會在網頁中造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="be547-556">We recommend that you avoid using the `Response.Redirect` method in `try/catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="be547-557">下列範例顯示建立第一個要求上的文字檔案，然後顯示按鈕，讓使用者可以開啟檔案的頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-557">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="be547-558">此範例刻意使用不正確的檔案名稱，如此會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="be547-558">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="be547-559">程式碼包含`catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，就會出現錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 即使找不到資料夾。</span><span class="sxs-lookup"><span data-stu-id="be547-559">The code includes `catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="be547-560">（您可以取消此範例中的陳述式以查看一切正常運作時，它的執行方式註解）。</span><span class="sxs-lookup"><span data-stu-id="be547-560">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="be547-561">如果您的程式碼未處理例外狀況，您會看到先前的螢幕擷取畫面類似的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="be547-561">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="be547-562">不過，`try/catch`一節可協助避免使用者看到這些錯誤類型。</span><span class="sxs-lookup"><span data-stu-id="be547-562">However, the `try/catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-c/samples/sample67.cshtml)]

## <a name="additional-resources"></a><span data-ttu-id="be547-563">其他資源</span><span class="sxs-lookup"><span data-stu-id="be547-563">Additional Resources</span></span>

<span data-ttu-id="be547-564">**使用 Visual Basic 進行程式設計**</span><span class="sxs-lookup"><span data-stu-id="be547-564">**Programming with Visual Basic**</span></span>


[<span data-ttu-id="be547-565">附錄： Visual Basic 語言和語法</span><span class="sxs-lookup"><span data-stu-id="be547-565">Appendix: Visual Basic Language and Syntax</span></span>](https://go.microsoft.com/fwlink/?LinkId=202908)


<span data-ttu-id="be547-566">**參考文件**</span><span class="sxs-lookup"><span data-stu-id="be547-566">**Reference Documentation**</span></span>


[<span data-ttu-id="be547-567">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="be547-567">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)

[<span data-ttu-id="be547-568">C# 語言</span><span class="sxs-lookup"><span data-stu-id="be547-568">C# Language</span></span>](https://msdn.microsoft.com/library/kx37x362.aspx)
