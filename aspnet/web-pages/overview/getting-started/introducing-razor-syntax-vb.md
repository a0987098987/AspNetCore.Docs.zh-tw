---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: "使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介 |Microsoft 文件"
author: tfitzmac
description: "本附錄可讓您使用 ASP.NET Web 網頁程式設計的概觀在 Visual Basic 中為使用 Razor 語法。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/07/2014
ms.topic: article
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: 42beb4ffcff9974230ba0c4a2f243020bcd4f99d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="857cf-103">使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="857cf-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="857cf-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="857cf-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="857cf-105">本文提供您進行程式設計的概觀與 ASP.NET Web 網頁使用 Razor 語法與 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="857cf-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="857cf-106">ASP.NET 是 Microsoft 的技術，用於 web 伺服器上執行動態網頁。</span><span class="sxs-lookup"><span data-stu-id="857cf-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="857cf-107">**您將學習**:</span><span class="sxs-lookup"><span data-stu-id="857cf-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="857cf-108">前 8 的程式設計入門程式設計使用 Razor 語法的 ASP.NET Web Pages 的提示。</span><span class="sxs-lookup"><span data-stu-id="857cf-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="857cf-109">您需要的基本程式設計概念。</span><span class="sxs-lookup"><span data-stu-id="857cf-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="857cf-110">ASP.NET server 程式碼和 Razor 語法與所有有關。</span><span class="sxs-lookup"><span data-stu-id="857cf-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="857cf-111">軟體版本</span><span class="sxs-lookup"><span data-stu-id="857cf-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="857cf-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="857cf-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="857cf-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="857cf-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="857cf-114">大部分的 ASP.NET 網頁使用 Razor 語法的範例使用 C#。</span><span class="sxs-lookup"><span data-stu-id="857cf-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="857cf-115">但 Razor 語法也支援 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="857cf-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="857cf-116">若要程式在 Visual Basic 中 ASP.NET 網頁上，您可以建立與網頁*.vbhtml*檔案的副檔名，然後加入 Visual Basic 程式碼。</span><span class="sxs-lookup"><span data-stu-id="857cf-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="857cf-117">這篇文章會提供使用 Visual Basic 語言及語法來建立 ASP.NET 網頁的概觀。</span><span class="sxs-lookup"><span data-stu-id="857cf-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="857cf-118">Microsoft WebMatrix 的預設網站範本 (**由於這家蛋糕店**，**相片圖庫**，和**入門網站**等等) 都有 C# 和 Visual Basic 版本。</span><span class="sxs-lookup"><span data-stu-id="857cf-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="857cf-119">您可以安裝 Visual Basic 的範本會以 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="857cf-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="857cf-120">在您的網站資料夾中的根資料夾中安裝網站範本*Microsoft 範本*。</span><span class="sxs-lookup"><span data-stu-id="857cf-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="857cf-121">最上層的 8 個程式設計提示</span><span class="sxs-lookup"><span data-stu-id="857cf-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="857cf-122">此區段會列出您一定要知道當您開始撰寫為使用 Razor 語法的 ASP.NET server 程式碼需要的一些秘訣。</span><span class="sxs-lookup"><span data-stu-id="857cf-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="857cf-123">1.您可以將程式碼加入頁面使用 @ 字元</span><span class="sxs-lookup"><span data-stu-id="857cf-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="857cf-124">`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="857cf-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="857cf-125">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-125">The result displayed in a browser:</span></span>

![Razor Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="857cf-127">**HTML 編碼**</span><span class="sxs-lookup"><span data-stu-id="857cf-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="857cf-128">當您在頁面上，使用顯示內容`@`的字元，如同上述範例中，ASP.NET 將 HTML 編碼的輸出。</span><span class="sxs-lookup"><span data-stu-id="857cf-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="857cf-129">這會取代保留的 HTML 字元 (例如`<`和`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。</span><span class="sxs-lookup"><span data-stu-id="857cf-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="857cf-130">而不進行 HTML 編碼，可能不正確，會顯示您伺服器的程式碼的輸出，並可能會暴露於安全性風險的頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="857cf-131">如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後。</span><span class="sxs-lookup"><span data-stu-id="857cf-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="857cf-132">閱讀更多有關中的 HTML 編碼[使用 ASP.NET Web Pages 站台中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkId=202892)。</span><span class="sxs-lookup"><span data-stu-id="857cf-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="857cf-133">2.您將程式碼區塊程式碼...結束程式碼</span><span class="sxs-lookup"><span data-stu-id="857cf-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="857cf-134">程式碼區塊包含一個或多個程式碼陳述式，且括以關鍵字`Code`和`End Code`。</span><span class="sxs-lookup"><span data-stu-id="857cf-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="857cf-135">將開頭`Code`關鍵字之後立即`@`字元 &#8212; 它們之間不能有空格。</span><span class="sxs-lookup"><span data-stu-id="857cf-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="857cf-136">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-136">The result displayed in a browser:</span></span>

![Razor Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="857cf-138">3.區塊中，您最後分行符號與每個程式碼陳述式</span><span class="sxs-lookup"><span data-stu-id="857cf-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="857cf-139">在 Visual Basic 程式碼區塊中，每個陳述式結尾分行符號。</span><span class="sxs-lookup"><span data-stu-id="857cf-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="857cf-140">（在本文稍後您會看到用來在包裝成多行冗長的程式碼陳述式，如有需要）。</span><span class="sxs-lookup"><span data-stu-id="857cf-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="857cf-141">4.您可以使用變數來儲存值</span><span class="sxs-lookup"><span data-stu-id="857cf-141">4. You use variables to store values</span></span>

<span data-ttu-id="857cf-142">您可以儲存在值*變數*，包括字串、 數字和日期等。您建立新變數使用`Dim`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="857cf-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="857cf-143">您可以直接在頁面上，使用插入變數值`@`。</span><span class="sxs-lookup"><span data-stu-id="857cf-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="857cf-144">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-144">The result displayed in a browser:</span></span>

![Razor Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="857cf-146">5.您將常值字串值括在雙引號中</span><span class="sxs-lookup"><span data-stu-id="857cf-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="857cf-147">A*字串*是會被視為文字的字元序列。</span><span class="sxs-lookup"><span data-stu-id="857cf-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="857cf-148">若要指定字串，您將它括在雙引號中：</span><span class="sxs-lookup"><span data-stu-id="857cf-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="857cf-149">若要內嵌引號內的字串值，會插入兩個雙引號字元。</span><span class="sxs-lookup"><span data-stu-id="857cf-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="857cf-150">如果您想要對網頁輸出中出現一次的雙引號字元時，請輸入做為`""`在括住字串，並在您想要出現兩次，如果輸入為`""""`加上引號的字串內。</span><span class="sxs-lookup"><span data-stu-id="857cf-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="857cf-151">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-151">The result displayed in a browser:</span></span>

![Razor Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="857cf-153">6.Visual Basic 程式碼不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="857cf-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="857cf-154">Visual Basic 語言不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="857cf-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="857cf-155">程式設計關鍵字 (像是`Dim`， `If`，和`True`) 和變數名稱 (例如`myString`，或`subTotal`) 可以在任何情況下撰寫。</span><span class="sxs-lookup"><span data-stu-id="857cf-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="857cf-156">下列程式碼會將值指派給變數`lastname`使用小寫名稱，然後再輸出使用大寫名稱的頁面變數的值。</span><span class="sxs-lookup"><span data-stu-id="857cf-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="857cf-157">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-157">The result displayed in a browser:</span></span>

![vb 語法 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="857cf-159">7.大部分的程式碼牽涉到使用物件</span><span class="sxs-lookup"><span data-stu-id="857cf-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="857cf-160">物件代表您可以使用程式設計項目 &#8212;頁面、 文字方塊、 檔案、 映像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列）、 等等。物件具有描述其特性 &#8212; 的屬性文字方塊物件具有`Text`屬性，要求物件具有`Url`屬性，電子郵件訊息有`From`屬性，而客戶物件具有`FirstName`屬性。</span><span class="sxs-lookup"><span data-stu-id="857cf-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="857cf-161">物件也有方法&quot;動詞&quot;他們可以執行。</span><span class="sxs-lookup"><span data-stu-id="857cf-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="857cf-162">範例包括檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="857cf-163">您通常會使用`Request`物件，提供您資訊例如值的表單欄位 （文字方塊等），頁面上何種瀏覽器所做要求的頁面、 使用者識別、 等等的 URL。這個範例示範如何存取屬性`Request`物件以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：</span><span class="sxs-lookup"><span data-stu-id="857cf-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="857cf-164">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-164">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="857cf-166">8.您可以撰寫程式碼所做的決策</span><span class="sxs-lookup"><span data-stu-id="857cf-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="857cf-167">動態網頁的主要功能是您可以判斷如何處理根據條件。</span><span class="sxs-lookup"><span data-stu-id="857cf-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="857cf-168">若要這樣做最常見的方法是使用`If`陳述式 (和選擇性`Else`陳述式)。</span><span class="sxs-lookup"><span data-stu-id="857cf-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="857cf-169">陳述式`If IsPost`會撰寫以快速`If IsPost = True`。</span><span class="sxs-lookup"><span data-stu-id="857cf-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="857cf-170">連同`If`陳述式，有多種方式來測試條件，重複程式碼區塊，以及等等，這是本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="857cf-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="857cf-171">在瀏覽器中顯示的結果 (按一下後**送出**):</span><span class="sxs-lookup"><span data-stu-id="857cf-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="857cf-173">**HTTP GET 和 POST 方法，以及 IsPost 屬性**</span><span class="sxs-lookup"><span data-stu-id="857cf-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="857cf-174">用於網頁 (HTTP) 的通訊協定支援非常有限的方法 (&quot;動詞&quot;)，用來對伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="857cf-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="857cf-175">兩個最常見的是，用來讀取的頁面，以及 POST，用來提交頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="857cf-176">一般情況下，使用者要求網頁，第一次要求頁面時利用 GET 輕鬆得到。</span><span class="sxs-lookup"><span data-stu-id="857cf-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="857cf-177">如果使用者填入表單，並按一下**送出**，瀏覽器對伺服器提出 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="857cf-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="857cf-178">中的 web 程式設計，通常會很有用知道是否正在要求頁面為 GET 或 POST，以了解如何處理頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="857cf-179">ASP.NET Web Pages 中，您可以使用`IsPost`屬性來查看是否有 GET 或 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="857cf-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="857cf-180">如果要求是使用 post 要求`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="857cf-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="857cf-181">您會看到的許多範例會示範如何處理不同的值而定頁面`IsPost`。</span><span class="sxs-lookup"><span data-stu-id="857cf-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="857cf-182">簡單的程式碼範例</span><span class="sxs-lookup"><span data-stu-id="857cf-182">A Simple Code Example</span></span>

<span data-ttu-id="857cf-183">此程序會示範如何建立可說明基本的程式設計技術的頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="857cf-184">在範例中，您可以建立可讓使用者輸入兩個數字，然後將它們加入，並顯示結果頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="857cf-185">在編輯器中，建立新的檔案並將其命名*AddNumbers.vbhtml*。</span><span class="sxs-lookup"><span data-stu-id="857cf-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="857cf-186">複製下列程式碼和標記到頁面上，取代已經在頁面中的任何項目。</span><span class="sxs-lookup"><span data-stu-id="857cf-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="857cf-187">以下是您必須注意的一些事項：</span><span class="sxs-lookup"><span data-stu-id="857cf-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="857cf-188">`@`字元在頁面中，啟動程式碼的第一個區塊，而且之前`totalMessage`內嵌底部附近的變數。</span><span class="sxs-lookup"><span data-stu-id="857cf-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="857cf-189">位於頁面頂端的區塊括在`Code...End Code`。</span><span class="sxs-lookup"><span data-stu-id="857cf-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="857cf-190">變數`total`， `num1`， `num2`，和`totalMessage`儲存幾個數字和字串。</span><span class="sxs-lookup"><span data-stu-id="857cf-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="857cf-191">常值字串值指派給`totalMessage`變數是置於雙引號之間。</span><span class="sxs-lookup"><span data-stu-id="857cf-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="857cf-192">因為 Visual Basic 程式碼是不區分大小寫，當`totalMessage`頁面底部附近使用變數，其名稱就只需要比對變數的宣告在頁面頂端的拼字。</span><span class="sxs-lookup"><span data-stu-id="857cf-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="857cf-193">大小寫並不重要。</span><span class="sxs-lookup"><span data-stu-id="857cf-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="857cf-194">運算式`num1.AsInt()`  +  `num2.AsInt()`示範如何使用物件和方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="857cf-195">`AsInt`上每個變數的方法會將轉換成整數 （整數），可加入使用者輸入的字串。</span><span class="sxs-lookup"><span data-stu-id="857cf-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="857cf-196">`<form>`標記包含`method="post"`屬性。</span><span class="sxs-lookup"><span data-stu-id="857cf-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="857cf-197">這會指定當使用者按一下**新增**，頁面將會傳送至使用 HTTP POST 方法的伺服器。</span><span class="sxs-lookup"><span data-stu-id="857cf-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="857cf-198">送出頁面時，程式碼`If IsPost`評估為 true，條件式程式碼執行時，顯示加上數字的結果。</span><span class="sxs-lookup"><span data-stu-id="857cf-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="857cf-199">儲存頁面，並在瀏覽器中執行。</span><span class="sxs-lookup"><span data-stu-id="857cf-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="857cf-200">(請確定中選取頁面**檔案**才能執行這個工作區。)兩個整數的輸入，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="857cf-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="857cf-202">Visual Basic 語言和語法</span><span class="sxs-lookup"><span data-stu-id="857cf-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="857cf-203">您稍早已基本範例如何建立 ASP.NET 網頁，以及如何將伺服器程式碼加入至 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="857cf-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="857cf-204">這裡您將學習使用 Visual Basic 撰寫 ASP.NET server 程式碼使用 Razor 語法 &#8212; 的基本概念也就是說，程式設計語言規則。</span><span class="sxs-lookup"><span data-stu-id="857cf-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="857cf-205">如果您熟悉程式設計 （特別是如果您使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大部分的讀取這裡都很熟悉。</span><span class="sxs-lookup"><span data-stu-id="857cf-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="857cf-206">您可能需要在您自己熟悉只 WebMatrix 程式碼加入至標記中的方式*.vbhtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="857cf-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a><span data-ttu-id="857cf-207">結合文字、 標記和程式碼區塊中的程式碼</span><span class="sxs-lookup"><span data-stu-id="857cf-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="857cf-208">在伺服器程式碼區塊，您通常要輸出的文字和網頁的標記。</span><span class="sxs-lookup"><span data-stu-id="857cf-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="857cf-209">如果伺服器程式碼區塊包含的文字不是程式碼，而是應轉譯為是，ASP.NET 必須能從程式碼中區分該文字。</span><span class="sxs-lookup"><span data-stu-id="857cf-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="857cf-210">有幾個方式可做到這點。</span><span class="sxs-lookup"><span data-stu-id="857cf-210">There are several ways to do this.</span></span>

- <span data-ttu-id="857cf-211">HTML 區塊項目，例如括住文字`<p></p>`或`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="857cf-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="857cf-212">HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。</span><span class="sxs-lookup"><span data-stu-id="857cf-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="857cf-213">當 ASP.NET 會看到開頭 HTML 標記 (例如， `<p>`)，它將元素呈現的所有項目且做為其內容至瀏覽器 （和解析的伺服器程式碼運算式）。</span><span class="sxs-lookup"><span data-stu-id="857cf-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="857cf-214">使用`@:`運算子或`<text>`項目。</span><span class="sxs-lookup"><span data-stu-id="857cf-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="857cf-215">`@:`輸出內容包含純文字或無對應的 HTML 標記; 單行`<text>`元素會括住多個輸出行。</span><span class="sxs-lookup"><span data-stu-id="857cf-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="857cf-216">當您不想要呈現的 HTML 項目做為輸出的一部分，這些選項會是很有用。</span><span class="sxs-lookup"><span data-stu-id="857cf-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="857cf-217">下列範例會重複上一個範例，但會使用一對`<text>`標記來括住要呈現的文字。</span><span class="sxs-lookup"><span data-stu-id="857cf-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="857cf-218">在下列範例中，`<text>`和`</text>`標記括住三行，其中有部分未包含的文字和無對應的 HTML 標記 (`<br />`) 以及伺服端程式碼，而相符的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="857cf-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="857cf-219">同樣地，您也可以在每一行，以個別`@:`運算子; 任一種方式運作。</span><span class="sxs-lookup"><span data-stu-id="857cf-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="857cf-220">當您在輸出文字本節 &#8212; 中所示使用 HTML 項目，`@:`運算子，或`<text>`元素 &#8212;ASP.NET 不進行 HTML 編碼的輸出。</span><span class="sxs-lookup"><span data-stu-id="857cf-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="857cf-221">(如前文所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面加上輸出`@`，除非在這一節所述的特殊案例。)</span><span class="sxs-lookup"><span data-stu-id="857cf-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="857cf-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="857cf-222">Whitespace</span></span>

<span data-ttu-id="857cf-223">陳述式中 （且字串常值之外） 的額外空間不會影響陳述式：</span><span class="sxs-lookup"><span data-stu-id="857cf-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="857cf-224">長的陳述式分成多行</span><span class="sxs-lookup"><span data-stu-id="857cf-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="857cf-225">您也可以使用底線字元成多行中斷冗長的程式碼陳述式`_`(在 Visual Basic 中稱為*接續字元*) 在每一行程式碼之後。</span><span class="sxs-lookup"><span data-stu-id="857cf-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="857cf-226">若要中斷到下一行的陳述式，在一行的結尾加入一個空格，然後接續字元。</span><span class="sxs-lookup"><span data-stu-id="857cf-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="857cf-227">在下一行繼續陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-227">Continue the statement on the next line.</span></span> <span data-ttu-id="857cf-228">您可以包裝陳述式分成多行，視需要以提高可讀性。</span><span class="sxs-lookup"><span data-stu-id="857cf-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="857cf-229">下列陳述式都相同：</span><span class="sxs-lookup"><span data-stu-id="857cf-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="857cf-230">不過，您無法包裝一條線中間的字串常值。</span><span class="sxs-lookup"><span data-stu-id="857cf-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="857cf-231">下列範例將無法運作：</span><span class="sxs-lookup"><span data-stu-id="857cf-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="857cf-232">要結合多個線條，類似上述的程式碼包裝的長字串，您必須使用*串連運算子*(`&`)，您會看到在本文稍後。</span><span class="sxs-lookup"><span data-stu-id="857cf-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="857cf-233">程式碼註解</span><span class="sxs-lookup"><span data-stu-id="857cf-233">Code comments</span></span>

<span data-ttu-id="857cf-234">註解可讓您保留供您自己或其他資訊。</span><span class="sxs-lookup"><span data-stu-id="857cf-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="857cf-235">Razor 語法的註解前面會加上`@*`，而結尾`*@`。</span><span class="sxs-lookup"><span data-stu-id="857cf-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="857cf-236">程式碼區塊內，您可以使用 Razor 語法的註解，或者您可以使用一般的 Visual Basic 註解字元，也就是單引號 (`'`) 至每一行的前置詞。</span><span class="sxs-lookup"><span data-stu-id="857cf-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="857cf-237">變數</span><span class="sxs-lookup"><span data-stu-id="857cf-237">Variables</span></span>

<span data-ttu-id="857cf-238">變數是您用來儲存資料的具名的物件。</span><span class="sxs-lookup"><span data-stu-id="857cf-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="857cf-239">您可以為變數命名任何項目，但是名稱必須以字母字元開頭，而且不能包含空白字元或保留的字元。</span><span class="sxs-lookup"><span data-stu-id="857cf-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="857cf-240">在 Visual Basic 中，如您所見更早版本，並不重要的變數名稱中的字母大小寫。</span><span class="sxs-lookup"><span data-stu-id="857cf-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="857cf-241">變數和資料類型</span><span class="sxs-lookup"><span data-stu-id="857cf-241">Variables and data types</span></span>

<span data-ttu-id="857cf-242">變數可以有特定資料類型，表示何種資料儲存在變數中。</span><span class="sxs-lookup"><span data-stu-id="857cf-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="857cf-243">您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數).</span><span class="sxs-lookup"><span data-stu-id="857cf-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="857cf-244">而且還有許多其他您可以使用的資料類型。</span><span class="sxs-lookup"><span data-stu-id="857cf-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="857cf-245">不過，您不必指定類型的變數。</span><span class="sxs-lookup"><span data-stu-id="857cf-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="857cf-246">在大部分情況下 ASP.NET 可以找出如何使用變數中的資料為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="857cf-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="857cf-247">（有時候您必須指定一個類型，您會看到範例這是 true）。</span><span class="sxs-lookup"><span data-stu-id="857cf-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="857cf-248">若要宣告但未指定類型的變數，使用`Dim`再加上變數的名稱 (比方說， `Dim myVar`)。</span><span class="sxs-lookup"><span data-stu-id="857cf-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="857cf-249">若要宣告變數的類型時，使用`Dim`變數的名稱，後面接著加號`As`和型別名稱 (比方說， `Dim myVar As String`)。</span><span class="sxs-lookup"><span data-stu-id="857cf-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="857cf-250">下列範例會顯示在網頁中使用變數某些內嵌運算式。</span><span class="sxs-lookup"><span data-stu-id="857cf-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="857cf-251">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-251">The result displayed in a browser:</span></span>

![Razor Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="857cf-253">轉換和測試資料類型</span><span class="sxs-lookup"><span data-stu-id="857cf-253">Converting and testing data types</span></span>

<span data-ttu-id="857cf-254">雖然 ASP.NET 通常可以自動判斷的資料型別，有時無法。</span><span class="sxs-lookup"><span data-stu-id="857cf-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="857cf-255">因此，您可能需要幫助 ASP.NET 執行的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="857cf-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="857cf-256">即使您沒有將類型轉換，有時候很有幫助測試以查看何種資料類型您可能使用。</span><span class="sxs-lookup"><span data-stu-id="857cf-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="857cf-257">最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。</span><span class="sxs-lookup"><span data-stu-id="857cf-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="857cf-258">下列範例顯示一般的情況下，您必須將字串轉換成數字。</span><span class="sxs-lookup"><span data-stu-id="857cf-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="857cf-259">一般而言，使用者輸入都是在您為字串。</span><span class="sxs-lookup"><span data-stu-id="857cf-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="857cf-260">即使您已提示使用者輸入數字，而且即使他們已經提交使用者輸入時，而您閱讀程式碼中，輸入數字，資料字串格式。</span><span class="sxs-lookup"><span data-stu-id="857cf-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="857cf-261">因此，您必須將字串轉換為數字。</span><span class="sxs-lookup"><span data-stu-id="857cf-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="857cf-262">在範例中，如果您嘗試執行算術運算的值，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：</span><span class="sxs-lookup"><span data-stu-id="857cf-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="857cf-263">若要將值轉換成整數，您呼叫`AsInt`方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="857cf-264">如果轉換成功，然後您可以加入數字。</span><span class="sxs-lookup"><span data-stu-id="857cf-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="857cf-265">下表列出一些常見的轉換和測試方法的變數。</span><span class="sxs-lookup"><span data-stu-id="857cf-265">The following table lists some common conversion and test methods for variables.</span></span>

| <span data-ttu-id="857cf-266">**方法**</span><span class="sxs-lookup"><span data-stu-id="857cf-266">**Method**</span></span> | <span data-ttu-id="857cf-267">**說明**</span><span class="sxs-lookup"><span data-stu-id="857cf-267">**Description**</span></span> | <span data-ttu-id="857cf-268">**範例**</span><span class="sxs-lookup"><span data-stu-id="857cf-268">**Example**</span></span> |
| --- | --- | --- |
| `AsInt(), IsInt()` | <span data-ttu-id="857cf-269">將字串表示之間的整數轉換 (例如&quot;593&quot;) 成整數。</span><span class="sxs-lookup"><span data-stu-id="857cf-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)] |
| `AsBool(), IsBool()` | <span data-ttu-id="857cf-270">轉換字串 like &quot;true&quot;或&quot;false&quot;布林型別。</span><span class="sxs-lookup"><span data-stu-id="857cf-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)] |
| `AsFloat(), IsFloat()` | <span data-ttu-id="857cf-271">將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;浮點數。</span><span class="sxs-lookup"><span data-stu-id="857cf-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)] |
| `AsDecimal(), IsDecimal()` | <span data-ttu-id="857cf-272">將具有類似的十進位值的字串轉換&quot;1.3&quot;或&quot;7.439&quot;為十進位數字。</span><span class="sxs-lookup"><span data-stu-id="857cf-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="857cf-273">（在 ASP.NET、 十進位數字是更精確比浮點數）。</span><span class="sxs-lookup"><span data-stu-id="857cf-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)] |
| `AsDateTime(), IsDateTime()` | <span data-ttu-id="857cf-274">將 asp.net 代表日期和時間值的字串轉換`DateTime`型別。</span><span class="sxs-lookup"><span data-stu-id="857cf-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)] |
| `ToString()` | <span data-ttu-id="857cf-275">將其他任何資料類型轉換為字串。</span><span class="sxs-lookup"><span data-stu-id="857cf-275">Converts any other data type to a string.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)] |

## <a name="operators"></a><span data-ttu-id="857cf-276">運算子</span><span class="sxs-lookup"><span data-stu-id="857cf-276">Operators</span></span>

<span data-ttu-id="857cf-277">運算子是命令的關鍵字或字元，會告知 ASP.NET 何種在運算式中執行。</span><span class="sxs-lookup"><span data-stu-id="857cf-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="857cf-278">Visual Basic 支援許多運算子，但您只需要辨識一些開始進行開發的 ASP.NET web pages。</span><span class="sxs-lookup"><span data-stu-id="857cf-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="857cf-279">下表摘要說明最常見的運算子。</span><span class="sxs-lookup"><span data-stu-id="857cf-279">The following table summarizes the most common operators.</span></span>

| <span data-ttu-id="857cf-280">**Operator**</span><span class="sxs-lookup"><span data-stu-id="857cf-280">**Operator**</span></span> | <span data-ttu-id="857cf-281">**說明**</span><span class="sxs-lookup"><span data-stu-id="857cf-281">**Description**</span></span> | <span data-ttu-id="857cf-282">**範例**</span><span class="sxs-lookup"><span data-stu-id="857cf-282">**Examples**</span></span> |
| --- | --- | --- |
| `+ - * /` | <span data-ttu-id="857cf-283">數學運算子用在數值運算式。</span><span class="sxs-lookup"><span data-stu-id="857cf-283">Math operators used in numerical expressions.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)] |
| `=` | <span data-ttu-id="857cf-284">指派和等號比較。</span><span class="sxs-lookup"><span data-stu-id="857cf-284">Assignment and equality.</span></span> <span data-ttu-id="857cf-285">根據內容，將陳述式右邊的值指派給物件，在左邊，或檢查值相等。</span><span class="sxs-lookup"><span data-stu-id="857cf-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)] |
| `<>` | <span data-ttu-id="857cf-286">不等。</span><span class="sxs-lookup"><span data-stu-id="857cf-286">Inequality.</span></span> <span data-ttu-id="857cf-287">傳回`True`值是否不相等。</span><span class="sxs-lookup"><span data-stu-id="857cf-287">Returns `True` if the values are not equal.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)] |
| `< > <= >=` | <span data-ttu-id="857cf-288">小於、 大於、 小於或等於、 與大於或等於。</span><span class="sxs-lookup"><span data-stu-id="857cf-288">Less than, greater than, less than or equal, and greater than or equal.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)] |
| `&` | <span data-ttu-id="857cf-289">串連，用來加入字串。</span><span class="sxs-lookup"><span data-stu-id="857cf-289">Concatenation, which is used to join strings.</span></span> | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)] |
| `+= -=` | <span data-ttu-id="857cf-290">遞增和遞減運算子，其中加入和減去 1 （分別） 變數。</span><span class="sxs-lookup"><span data-stu-id="857cf-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)] |
| `.` | <span data-ttu-id="857cf-291">點。</span><span class="sxs-lookup"><span data-stu-id="857cf-291">Dot.</span></span> <span data-ttu-id="857cf-292">用來區別物件及其屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-292">Used to distinguish objects and their properties and methods.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)] |
| `()` | <span data-ttu-id="857cf-293">括號。</span><span class="sxs-lookup"><span data-stu-id="857cf-293">Parentheses.</span></span> <span data-ttu-id="857cf-294">群組運算式，用來將參數傳遞至方法，以及存取成員的陣列與集合。</span><span class="sxs-lookup"><span data-stu-id="857cf-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span> | [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)] |
| `Not` | <span data-ttu-id="857cf-295">不是。</span><span class="sxs-lookup"><span data-stu-id="857cf-295">Not.</span></span> <span data-ttu-id="857cf-296">反轉，則為 true 的值為 false，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="857cf-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="857cf-297">一般而言，若要測試的簡略方法做`False`(也就是針對不`True`)。</span><span class="sxs-lookup"><span data-stu-id="857cf-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)] |
| `AndAlso OrElse` | <span data-ttu-id="857cf-298">邏輯 AND 和 OR，用來連結條件在一起。</span><span class="sxs-lookup"><span data-stu-id="857cf-298">Logical AND and OR, which are used to link conditions together.</span></span> | [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)] |

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="857cf-299">使用檔案和程式碼中的資料夾路徑</span><span class="sxs-lookup"><span data-stu-id="857cf-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="857cf-300">您通常將程式碼中使用檔案和資料夾路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="857cf-301">以下是網站的實體資料夾結構的範例，就可能會出現在開發電腦上：</span><span class="sxs-lookup"><span data-stu-id="857cf-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="857cf-302">以下是一些基本的詳細 Url 和路徑：</span><span class="sxs-lookup"><span data-stu-id="857cf-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="857cf-303">開始使用的網域名稱的 URL (`http://www.example.com`) 或伺服器名稱 (`http://localhost`， `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="857cf-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="857cf-304">URL 對應至主機電腦上的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="857cf-305">例如，`http://myserver`可能對應至資料夾*C:\websites\mywebsite*在伺服器上。</span><span class="sxs-lookup"><span data-stu-id="857cf-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="857cf-306">虛擬路徑是代表程式碼中的路徑，而不必指定完整路徑的縮寫。</span><span class="sxs-lookup"><span data-stu-id="857cf-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="857cf-307">它包含的 url 的網域或伺服器名稱後面的部分。</span><span class="sxs-lookup"><span data-stu-id="857cf-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="857cf-308">當您使用的虛擬路徑時，您可以移動至不同的網域或伺服器的程式碼而不需要更新的路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="857cf-309">以下是範例，可協助您了解的差異：</span><span class="sxs-lookup"><span data-stu-id="857cf-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="857cf-310">完整的 URL</span><span class="sxs-lookup"><span data-stu-id="857cf-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="857cf-311">伺服器名稱</span><span class="sxs-lookup"><span data-stu-id="857cf-311">Server name</span></span> | <span data-ttu-id="857cf-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="857cf-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="857cf-313">虛擬路徑</span><span class="sxs-lookup"><span data-stu-id="857cf-313">Virtual path</span></span> | <span data-ttu-id="857cf-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="857cf-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="857cf-315">實體路徑</span><span class="sxs-lookup"><span data-stu-id="857cf-315">Physical path</span></span> | <span data-ttu-id="857cf-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="857cf-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="857cf-317">虛擬根目錄為 /，如同您的 c： 根磁碟機是 \。</span><span class="sxs-lookup"><span data-stu-id="857cf-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="857cf-318">（虛擬資料夾路徑一律使用正斜線）。資料夾的虛擬路徑不需要有相同的名稱做為實體的資料夾。它可以是別名。</span><span class="sxs-lookup"><span data-stu-id="857cf-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="857cf-319">（在生產伺服器上的虛擬路徑很少比對確切的實體路徑。）</span><span class="sxs-lookup"><span data-stu-id="857cf-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="857cf-320">當您使用中程式碼檔案和資料夾時，有時候您需要參考實體的路徑和有時虛擬路徑，視您正在使用哪些物件而定。</span><span class="sxs-lookup"><span data-stu-id="857cf-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="857cf-321">ASP.NET 為您提供這些工具使用程式碼中的檔案和資料夾路徑：`Server.MapPath`方法，而`~`運算子和`Href`方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="857cf-322">轉換虛擬與實體路徑： Server.MapPath 方法</span><span class="sxs-lookup"><span data-stu-id="857cf-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="857cf-323">`Server.MapPath`方法會將轉換的虛擬路徑 (例如*/default.cshtml*) 至絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="857cf-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="857cf-324">使用此方法每次您需要完整的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="857cf-325">典型的範例是當您正在讀取或寫入文字檔案或 web 伺服器上的映像檔。</span><span class="sxs-lookup"><span data-stu-id="857cf-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="857cf-326">您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，這個方法可以將路徑轉換，因此您知道 — 虛擬路徑，您的伺服器上的對應路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="857cf-327">您將虛擬路徑傳遞至檔案或資料夾方法，並傳回的實體路徑：</span><span class="sxs-lookup"><span data-stu-id="857cf-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="857cf-328">參考的虛擬根目錄： ~ 運算子和 Href 方法</span><span class="sxs-lookup"><span data-stu-id="857cf-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="857cf-329">在*.cshtml*或*.vbhtml*檔案中，您可以參考虛擬根路徑使用`~`運算子。</span><span class="sxs-lookup"><span data-stu-id="857cf-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="857cf-330">這是非常實用，因為您可以移動的頁面，在網站中，而且任何連結至其他頁面包含不會中斷。</span><span class="sxs-lookup"><span data-stu-id="857cf-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="857cf-331">如果您曾經將您的網站移至不同的位置很方便。</span><span class="sxs-lookup"><span data-stu-id="857cf-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="857cf-332">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="857cf-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="857cf-333">如果網站`http://myserver/myapp`，以下是如何 ASP.NET 會將這些路徑網頁執行時：</span><span class="sxs-lookup"><span data-stu-id="857cf-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="857cf-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="857cf-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="857cf-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="857cf-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="857cf-336">（實際上不會看到這些路徑做為變數的值，但 ASP.NET 會將路徑，是原先）。</span><span class="sxs-lookup"><span data-stu-id="857cf-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="857cf-337">您可以使用`~`運算子 （如上述） 的伺服器程式碼中，在標記中，像這樣：</span><span class="sxs-lookup"><span data-stu-id="857cf-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="857cf-338">在標記中，您使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="857cf-339">ASP.NET 網頁執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="857cf-340">條件式邏輯和迴圈</span><span class="sxs-lookup"><span data-stu-id="857cf-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="857cf-341">ASP.NET server 程式碼可讓您根據條件和撰寫程式碼的重複次數也就是程式碼陳述式執行迴圈執行工作）。</span><span class="sxs-lookup"><span data-stu-id="857cf-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="857cf-342">測試條件</span><span class="sxs-lookup"><span data-stu-id="857cf-342">Testing conditions</span></span>

<span data-ttu-id="857cf-343">若要測試您所使用的簡單條件`If...Then`陳述式會傳回`True`或`False`根據您指定的測試：</span><span class="sxs-lookup"><span data-stu-id="857cf-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="857cf-344">`If`關鍵字開始區塊。</span><span class="sxs-lookup"><span data-stu-id="857cf-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="857cf-345">實際的測試 （條件） 遵循`If`關鍵字，並傳回 true 或 false。</span><span class="sxs-lookup"><span data-stu-id="857cf-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="857cf-346">`If`陳述式結尾`Then`。</span><span class="sxs-lookup"><span data-stu-id="857cf-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="857cf-347">如果測試為 true，將執行的陳述式會括住`If`和`End If`。</span><span class="sxs-lookup"><span data-stu-id="857cf-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="857cf-348">`If`陳述式可包含`Else`指定當條件為 false 時要執行的陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="857cf-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="857cf-349">如果`If`陳述式會啟動程式碼區塊，您不必使用法線`Code...End Code`包含區塊的陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="857cf-350">您可以加入`@`區塊，它會運作。</span><span class="sxs-lookup"><span data-stu-id="857cf-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="857cf-351">這個方法適用於`If`以及其他的 Visual Basic 程式設計關鍵字後面的程式碼區塊，包括`For`， `For Each`，`Do While`等等。</span><span class="sxs-lookup"><span data-stu-id="857cf-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="857cf-352">您可以加入多個條件使用一或多個`ElseIf`區塊：</span><span class="sxs-lookup"><span data-stu-id="857cf-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="857cf-353">在此範例中，如果第一個條件中`If`區塊不成立，`ElseIf`條件會檢查。</span><span class="sxs-lookup"><span data-stu-id="857cf-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="857cf-354">如果符合該條件中的陳述式`ElseIf`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="857cf-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="857cf-355">如果不符合任何一個條件中的陳述式`Else`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="857cf-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="857cf-356">您可以加入任意數目的`ElseIf`封鎖，並關閉與`Else`為封鎖&quot;一切&quot;條件。</span><span class="sxs-lookup"><span data-stu-id="857cf-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="857cf-357">若要測試條件數量龐大，請使用`Select Case`區塊：</span><span class="sxs-lookup"><span data-stu-id="857cf-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="857cf-358">要測試的值是在括號內 （在範例中，weekday 變數）。</span><span class="sxs-lookup"><span data-stu-id="857cf-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="857cf-359">每個個別的測試會使用`Case`列出值的陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="857cf-360">如果值`Case`陳述式的測試值相符中的程式碼`Case`執行區塊。</span><span class="sxs-lookup"><span data-stu-id="857cf-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="857cf-361">在瀏覽器中顯示最後兩個條件式區塊的結果：</span><span class="sxs-lookup"><span data-stu-id="857cf-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="857cf-363">迴圈的程式碼</span><span class="sxs-lookup"><span data-stu-id="857cf-363">Looping code</span></span>

<span data-ttu-id="857cf-364">您通常需要重複執行相同的陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="857cf-365">您的迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-365">You do this by looping.</span></span> <span data-ttu-id="857cf-366">例如，您通常會執行相同的陳述式，每個項目集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="857cf-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="857cf-367">如果您知道完全多少次您想要執行迴圈，您可以使用`For`迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="857cf-368">這種迴圈是特別適用於計數或向下計數：</span><span class="sxs-lookup"><span data-stu-id="857cf-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="857cf-369">迴圈開頭`For`關鍵字，後面接著三個項目：</span><span class="sxs-lookup"><span data-stu-id="857cf-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="857cf-370">之後立即`For`陳述式，宣告計數器變數 (不需要使用`Dim`) 並在指定範圍， `i = 10 to 20`。</span><span class="sxs-lookup"><span data-stu-id="857cf-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="857cf-371">這表示變數`i`會在 10 開始計算，並繼續進行直到達到 20 （含）。</span><span class="sxs-lookup"><span data-stu-id="857cf-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="857cf-372">之間`For`和`Next`陳述式是在區塊的內容。</span><span class="sxs-lookup"><span data-stu-id="857cf-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="857cf-373">這可以包含一或多個程式碼執行陳述式與每個迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="857cf-374">`Next i`陳述式會結束迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="857cf-375">它會遞增計數器，並開始迴圈的下一個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="857cf-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="857cf-376">程式碼之間的行`For`和`Next`行包含迴圈的每個反覆項目執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="857cf-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="857cf-377">標記會建立新的段落 (`<p>`元素) 在每次且會將行加入輸出中顯示的值 i （計數器）。</span><span class="sxs-lookup"><span data-stu-id="857cf-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="857cf-378">當您執行此頁面時，此範例會建立 11 行顯示輸出，以指出項目編號每一行文字。</span><span class="sxs-lookup"><span data-stu-id="857cf-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="857cf-380">如果您使用集合或陣列，您通常使用`For Each`迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="857cf-381">集合是一組類似的物件，而`For Each`迴圈可讓您執行集合中每個項目的工作。</span><span class="sxs-lookup"><span data-stu-id="857cf-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="857cf-382">這種類型的迴圈很方便的集合，因為與不同的是`For`迴圈中，您不必遞增計數器或設定的限制。</span><span class="sxs-lookup"><span data-stu-id="857cf-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="857cf-383">相反地，`For Each`迴圈的程式碼會繼續執行此集合直到完成為止。</span><span class="sxs-lookup"><span data-stu-id="857cf-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="857cf-384">這個範例會傳回中的項目`Request.ServerVariables`集合 （其中包含您的 web 伺服器的相關資訊）。</span><span class="sxs-lookup"><span data-stu-id="857cf-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="857cf-385">它會使用`For Each`迴圈以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="857cf-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="857cf-386">`For Each`關鍵字後面都代表單一項目集合中的變數 (在範例中， `myItem`)，後面接著`In`關鍵字，後面接著您想要重複使用的集合。</span><span class="sxs-lookup"><span data-stu-id="857cf-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="857cf-387">本文的`For Each`迴圈中，您可以存取目前的項目，使用您稍早宣告的變數。</span><span class="sxs-lookup"><span data-stu-id="857cf-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="857cf-389">若要建立更通用的迴圈，請使用`Do While`陳述式：</span><span class="sxs-lookup"><span data-stu-id="857cf-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="857cf-390">此迴圈開頭`Do While`關鍵字，後面接著條件，後面接著要重複的區塊。</span><span class="sxs-lookup"><span data-stu-id="857cf-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="857cf-391">通常遞增迴圈 （加入） 或遞減 （被減數） 變數或物件用來計算。</span><span class="sxs-lookup"><span data-stu-id="857cf-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="857cf-392">在範例中，`+=`運算子增加 1 變數的值在每次執行迴圈。</span><span class="sxs-lookup"><span data-stu-id="857cf-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="857cf-393">(以遞減的變數向下計數在迴圈中，您會使用遞減運算子`-=`。)</span><span class="sxs-lookup"><span data-stu-id="857cf-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="857cf-394">物件和集合</span><span class="sxs-lookup"><span data-stu-id="857cf-394">Objects and Collections</span></span>

<span data-ttu-id="857cf-395">幾乎所有的作業在 ASP.NET 網站是物件，包括網頁本身。</span><span class="sxs-lookup"><span data-stu-id="857cf-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="857cf-396">本節討論一些重要的物件，您將使用經常在您的程式碼中。</span><span class="sxs-lookup"><span data-stu-id="857cf-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="857cf-397">頁面物件</span><span class="sxs-lookup"><span data-stu-id="857cf-397">Page objects</span></span>

<span data-ttu-id="857cf-398">在 ASP.NET 中的最基本物件是的頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="857cf-399">您可以存取直接沒有任何合格的物件的頁面物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="857cf-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="857cf-400">下列程式碼取得網頁的檔案路徑，使用`Request`頁面的物件：</span><span class="sxs-lookup"><span data-stu-id="857cf-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="857cf-401">您可以使用屬性的`Page`物件取得許多資訊，例如：</span><span class="sxs-lookup"><span data-stu-id="857cf-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="857cf-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="857cf-402">`Request`.</span></span> <span data-ttu-id="857cf-403">如您所見，這是目前的要求，包括何種瀏覽器所做要求的頁面、 使用者識別、 等等的 URL 的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="857cf-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="857cf-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="857cf-404">`Response`.</span></span> <span data-ttu-id="857cf-405">這是會在伺服端程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="857cf-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="857cf-406">例如，您可以使用此屬性寫入至回應的資訊。</span><span class="sxs-lookup"><span data-stu-id="857cf-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="857cf-407">（陣列和字典） 的集合物件</span><span class="sxs-lookup"><span data-stu-id="857cf-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="857cf-408">集合是一組相同的類型，例如的集合物件的`Customer`資料庫中的物件。</span><span class="sxs-lookup"><span data-stu-id="857cf-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="857cf-409">ASP.NET 包含許多的內建集合，例如`Request.Files`集合。</span><span class="sxs-lookup"><span data-stu-id="857cf-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="857cf-410">您通常會使用集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="857cf-410">You'll often work with data in collections.</span></span> <span data-ttu-id="857cf-411">兩個常見的集合型別是*陣列*和*字典*。</span><span class="sxs-lookup"><span data-stu-id="857cf-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="857cf-412">當您想要儲存的類似項目集合，但不想要建立個別的變數來保存每個項目時，陣列會是很有用：</span><span class="sxs-lookup"><span data-stu-id="857cf-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="857cf-413">使用陣列時，您宣告為特定的資料類型，例如`String`， `Integer`，或`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="857cf-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="857cf-414">若要表示變數可以包含陣列，請在您的宣告中變數的名稱加上括號 (例如`Dim myVar() As String`)。</span><span class="sxs-lookup"><span data-stu-id="857cf-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="857cf-415">您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`For Each`陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="857cf-416">陣列索引以零為起始的是 &#8212;也就是說，第一個項目是在位置 0，第二個項目位於位置 1，依此類推。</span><span class="sxs-lookup"><span data-stu-id="857cf-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="857cf-417">您可以判斷陣列中的項目數目，藉由取得其`Length`屬性。</span><span class="sxs-lookup"><span data-stu-id="857cf-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="857cf-418">若要取得特定項目的陣列中的位置 (也就是搜尋陣列)，使用`Array.IndexOf`方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="857cf-419">您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。</span><span class="sxs-lookup"><span data-stu-id="857cf-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="857cf-420">在瀏覽器中顯示的字串陣列程式碼的輸出：</span><span class="sxs-lookup"><span data-stu-id="857cf-420">The output of the string array code displayed in a browser:</span></span>

![Razor Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="857cf-422">字典是索引鍵/值組的集合，您提供的索引鍵 （或名稱） 來設定或擷取對應的值：</span><span class="sxs-lookup"><span data-stu-id="857cf-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="857cf-423">若要建立字典，請使用`New`關鍵字表示，您要建立新`Dictionary`物件。</span><span class="sxs-lookup"><span data-stu-id="857cf-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="857cf-424">您可以將變數使用字典`Dim`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="857cf-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="857cf-425">指出資料類型的字典使用括號中的項目 ( `( )` )。</span><span class="sxs-lookup"><span data-stu-id="857cf-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="857cf-426">在宣告的結尾，您必須加入另一組括號，因為這是實際的方法，建立新的字典。</span><span class="sxs-lookup"><span data-stu-id="857cf-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="857cf-427">若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="857cf-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="857cf-428">或者，您可以使用括號表示索引鍵，並執行簡單指派，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="857cf-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="857cf-429">若要從字典取得值，您可以指定索引鍵在括號中：</span><span class="sxs-lookup"><span data-stu-id="857cf-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="857cf-430">呼叫具有參數的方法</span><span class="sxs-lookup"><span data-stu-id="857cf-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="857cf-431">如同稍早在本文中，您以程式設計的物件有方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="857cf-432">例如，`Database`物件可能會有`Database.Connect`方法。</span><span class="sxs-lookup"><span data-stu-id="857cf-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="857cf-433">許多方法也有一或多個參數。</span><span class="sxs-lookup"><span data-stu-id="857cf-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="857cf-434">A*參數*是傳遞至方法的值以啟用方法，以完成其工作。</span><span class="sxs-lookup"><span data-stu-id="857cf-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="857cf-435">例如，查看的宣告`Request.MapPath`方法，這個方法會採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="857cf-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="857cf-436">這個方法會傳回至指定的虛擬路徑對應的伺服器上的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="857cf-437">方法有三個參數`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。</span><span class="sxs-lookup"><span data-stu-id="857cf-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="857cf-438">（請注意在宣告中，列出參數的資料，它們會接受的資料類型）。當您呼叫這個方法時，您必須提供所有的三個參數的值。</span><span class="sxs-lookup"><span data-stu-id="857cf-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="857cf-439">當您使用 Visual Basic 含有 Razor 語法時，您有兩個選項傳遞至方法的參數：*位置參數*或*具名參數*。</span><span class="sxs-lookup"><span data-stu-id="857cf-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="857cf-440">若要呼叫方法，使用位置參數，您傳入參數嚴格的順序，方法宣告中所指定。</span><span class="sxs-lookup"><span data-stu-id="857cf-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="857cf-441">（您將通常知道此順序閱讀文件的方法）。您必須遵循的順序，和任何參數 &#8212; 無法略過如果有必要，您傳遞空字串 (`""`) 或您不需要的值是位置參數為 null。</span><span class="sxs-lookup"><span data-stu-id="857cf-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="857cf-442">下列範例假設您有一個名為資料夾*指令碼*在您的網站。</span><span class="sxs-lookup"><span data-stu-id="857cf-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="857cf-443">程式碼會呼叫`Request.MapPath`依正確順序的三個參數的方法，並傳遞值。</span><span class="sxs-lookup"><span data-stu-id="857cf-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="857cf-444">然後，它會顯示產生的對應的路徑。</span><span class="sxs-lookup"><span data-stu-id="857cf-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="857cf-445">當有許多方法的參數時，您可以將程式碼較為簡潔且更容易閱讀使用具名參數。</span><span class="sxs-lookup"><span data-stu-id="857cf-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="857cf-446">若要呼叫的方法使用具名的參數，指定參數名稱，後面接著`:=`，然後提供值。</span><span class="sxs-lookup"><span data-stu-id="857cf-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="857cf-447">具名參數的優點是您可以在任何您想要的順序新增它們。</span><span class="sxs-lookup"><span data-stu-id="857cf-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="857cf-448">（的缺點是在方法呼叫不是精簡）。</span><span class="sxs-lookup"><span data-stu-id="857cf-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="857cf-449">下列範例會呼叫上述方法，但使用具名參數，以提供值：</span><span class="sxs-lookup"><span data-stu-id="857cf-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="857cf-450">如您所見，將參數傳遞不同的順序。</span><span class="sxs-lookup"><span data-stu-id="857cf-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="857cf-451">不過，如果您執行上述範例，此範例中，它們會傳回相同的值。</span><span class="sxs-lookup"><span data-stu-id="857cf-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="857cf-452">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="857cf-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="857cf-453">Try Catch 陳述式</span><span class="sxs-lookup"><span data-stu-id="857cf-453">Try-Catch statements</span></span>

<span data-ttu-id="857cf-454">您通常會需要陳述式可能失敗的原因，您無法控制的程式碼。</span><span class="sxs-lookup"><span data-stu-id="857cf-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="857cf-455">例如: </span><span class="sxs-lookup"><span data-stu-id="857cf-455">For example:</span></span>

- <span data-ttu-id="857cf-456">如果您的程式碼嘗試開啟、 建立、 讀取或寫入檔案時，可能會發生各種錯誤。</span><span class="sxs-lookup"><span data-stu-id="857cf-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="857cf-457">您想要的檔案可能不存在，可能會遭到鎖定，程式碼可能不具有權限，等等。</span><span class="sxs-lookup"><span data-stu-id="857cf-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="857cf-458">同樣地，如果您的程式碼會嘗試以更新資料庫中的記錄，可以有權限問題、 可能會卸除資料庫的連接、 要儲存的資料可能不正確，依此類推。</span><span class="sxs-lookup"><span data-stu-id="857cf-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="857cf-459">在程式設計的詞彙，這些情況下會呼叫*例外狀況*。</span><span class="sxs-lookup"><span data-stu-id="857cf-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="857cf-460">如果您的程式碼遇到例外狀況，則會產生 （會擲回） 錯誤訊息也就是，最多只能惱人給使用者。</span><span class="sxs-lookup"><span data-stu-id="857cf-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="857cf-462">在您的程式碼可能會遇到例外狀況的情況下，以及為了避免此類型的錯誤訊息，您可以使用`Try/Catch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="857cf-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="857cf-463">在`Try`陳述式中，執行您正在檢查的程式碼。</span><span class="sxs-lookup"><span data-stu-id="857cf-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="857cf-464">一或多個`Catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="857cf-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="857cf-465">您可以包含最大數量`Catch`陳述式，當您需要尋找您所預期的錯誤。</span><span class="sxs-lookup"><span data-stu-id="857cf-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="857cf-466">我們建議您避免使用`Response.Redirect`方法中的`Try/Catch`陳述式，因為它會在頁面中造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="857cf-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="857cf-467">下列範例顯示建立第一次要求的文字檔，然後顯示按鈕，可讓使用者開啟檔案的頁面。</span><span class="sxs-lookup"><span data-stu-id="857cf-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="857cf-468">此範例刻意使用不正確的檔案名稱，因此它將會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="857cf-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="857cf-469">程式碼包含`Catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，發生錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 甚至無法尋找資料夾。</span><span class="sxs-lookup"><span data-stu-id="857cf-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="857cf-470">（您可以取消此範例中的陳述式才能看到一切正常運作時，它的執行方式註解）。</span><span class="sxs-lookup"><span data-stu-id="857cf-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="857cf-471">如果您的程式碼未處理例外狀況，您會看到錯誤頁面類似上一個螢幕擷取畫面。</span><span class="sxs-lookup"><span data-stu-id="857cf-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="857cf-472">不過，`Try/Catch`一節可協助防止使用者看到這些類型的錯誤。</span><span class="sxs-lookup"><span data-stu-id="857cf-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="857cf-473">其他資源</span><span class="sxs-lookup"><span data-stu-id="857cf-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="857cf-474">參考文件</span><span class="sxs-lookup"><span data-stu-id="857cf-474">Reference Documentation</span></span>

- [<span data-ttu-id="857cf-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="857cf-475">ASP.NET</span></span>](https://msdn.microsoft.com/en-us/library/ee532866.aspx)
- [<span data-ttu-id="857cf-476">Visual Basic 語言</span><span class="sxs-lookup"><span data-stu-id="857cf-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/en-us/library/2x7h1hfk.aspx)
