---
uid: web-pages/overview/getting-started/introducing-razor-syntax-vb
title: 使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介 |Microsoft Docs
author: tfitzmac
description: 本附錄提供您的概觀與 ASP.NET Web pages 程式設計在 Visual Basic 中使用 Razor 語法。
ms.author: riande
ms.date: 02/07/2014
ms.assetid: 5da59646-e973-41cd-88a9-c6b2c0594027
msc.legacyurl: /web-pages/overview/getting-started/introducing-razor-syntax-vb
msc.type: authoredcontent
ms.openlocfilehash: cbec035533c37723afcd5bf4aa0c6e1c83dbae23
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41830950"
---
<a name="introduction-to-aspnet-web-programming-using-the-razor-syntax-visual-basic"></a><span data-ttu-id="3d472-103">使用 Razor 語法 (Visual Basic) 的 ASP.NET Web 程式設計簡介</span><span class="sxs-lookup"><span data-stu-id="3d472-103">Introduction to ASP.NET Web Programming Using the Razor Syntax (Visual Basic)</span></span>
====================
<span data-ttu-id="3d472-104">藉由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3d472-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3d472-105">這篇文章概述您程式設計的 ASP.NET 網頁使用 Razor 語法和 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="3d472-105">This article gives you an overview of programming with ASP.NET Web Pages using the Razor syntax and Visual Basic.</span></span> <span data-ttu-id="3d472-106">ASP.NET 是 Microsoft 的技術，用於在 web 伺服器上執行動態網頁。</span><span class="sxs-lookup"><span data-stu-id="3d472-106">ASP.NET is Microsoft's technology for running dynamic web pages on web servers.</span></span>
> 
> <span data-ttu-id="3d472-107">**您將學到什麼**:</span><span class="sxs-lookup"><span data-stu-id="3d472-107">**What you'll learn**:</span></span>
> 
> - <span data-ttu-id="3d472-108">前 8 的程式設計入門程式設計使用 Razor 語法的 ASP.NET Web Pages 的提示。</span><span class="sxs-lookup"><span data-stu-id="3d472-108">The top 8 programming tips for getting started with programming ASP.NET Web Pages using Razor syntax.</span></span>
> - <span data-ttu-id="3d472-109">您需要的基本程式設計概念。</span><span class="sxs-lookup"><span data-stu-id="3d472-109">Basic programming concepts you'll need.</span></span>
> - <span data-ttu-id="3d472-110">ASP.NET server 程式碼和 Razor 語法是有關。</span><span class="sxs-lookup"><span data-stu-id="3d472-110">What ASP.NET server code and the Razor syntax is all about.</span></span>
>   
> 
> ## <a name="software-versions"></a><span data-ttu-id="3d472-111">軟體版本</span><span class="sxs-lookup"><span data-stu-id="3d472-111">Software versions</span></span>
> 
> 
> - <span data-ttu-id="3d472-112">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3d472-112">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3d472-113">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="3d472-113">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="3d472-114">使用 Razor 語法的 ASP.NET Web Pages 的大多數範例使用 C#。</span><span class="sxs-lookup"><span data-stu-id="3d472-114">Most examples of using ASP.NET Web Pages with Razor syntax use C#.</span></span> <span data-ttu-id="3d472-115">但 Razor 語法也支援 Visual Basic。</span><span class="sxs-lookup"><span data-stu-id="3d472-115">But the Razor syntax also supports Visual Basic.</span></span> <span data-ttu-id="3d472-116">若要程式 ASP.NET web 網頁在 Visual Basic 中，您會建立與網頁 *.vbhtml*副檔名，然後新增 Visual Basic 程式碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-116">To program an ASP.NET web page in Visual Basic, you create a web page with a *.vbhtml* filename extension, and then add Visual Basic code.</span></span> <span data-ttu-id="3d472-117">這篇文章會提供使用 Visual Basic 語言和語法，來建立 ASP.NET 網頁的概觀。</span><span class="sxs-lookup"><span data-stu-id="3d472-117">This article gives you an overview of working with the Visual Basic language and syntax to create ASP.NET Webpages.</span></span>

> [!NOTE]
> <span data-ttu-id="3d472-118">Microsoft WebMatrix 的網站預設範本 (**這家麵包店**，**相片圖庫**，並**入門網站**等) 可用於 C# 和 Visual Basic 版本。</span><span class="sxs-lookup"><span data-stu-id="3d472-118">The default website templates for Microsoft WebMatrix (**Bakery**, **Photo Gallery**, and **Starter Site**, etc.) are available in C# and Visual Basic versions.</span></span> <span data-ttu-id="3d472-119">您可以安裝的 Visual Basic 範本，藉由以 NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="3d472-119">You can install the Visual Basic templates by as NuGet packages.</span></span> <span data-ttu-id="3d472-120">網站範本會安裝在您的網站，在名為的資料夾中的根資料夾*Microsoft 範本*。</span><span class="sxs-lookup"><span data-stu-id="3d472-120">Website templates are installed in the root folder of your site in a folder named *Microsoft Templates*.</span></span>


## <a name="the-top-8-programming-tips"></a><span data-ttu-id="3d472-121">最佳的 8 程式設計秘訣</span><span class="sxs-lookup"><span data-stu-id="3d472-121">The Top 8 Programming Tips</span></span>

<span data-ttu-id="3d472-122">此區段會列出您一定要知道當您開始撰寫使用 Razor 語法的 ASP.NET 伺服器程式碼的一些秘訣。</span><span class="sxs-lookup"><span data-stu-id="3d472-122">This section lists a few tips that you absolutely need to know as you start writing ASP.NET server code using the Razor syntax.</span></span>

### <a name="1-you-add-code-to-a-page-using-the--character"></a><span data-ttu-id="3d472-123">1.您將程式碼新增至頁面上，使用 @ 字元</span><span class="sxs-lookup"><span data-stu-id="3d472-123">1. You add code to a page using the @ character</span></span>

<span data-ttu-id="3d472-124">`@`字元開始內嵌運算式、 單一陳述式區塊和多重陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="3d472-124">The `@` character starts inline expressions, single-statement blocks, and multi-statement blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample1.vbhtml)]

<span data-ttu-id="3d472-125">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-125">The result displayed in a browser:</span></span>

![Razor-Img1](introducing-razor-syntax-vb/_static/image1.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3d472-127">**HTML 編碼**</span><span class="sxs-lookup"><span data-stu-id="3d472-127">**HTML Encoding**</span></span>
> 
> <span data-ttu-id="3d472-128">當您在頁面上，使用顯示的內容時`@`字元 ASP.NET HTML 編碼的輸出，如上述範例中，所示。</span><span class="sxs-lookup"><span data-stu-id="3d472-128">When you display content in a page using the `@` character, as in the preceding examples, ASP.NET HTML-encodes the output.</span></span> <span data-ttu-id="3d472-129">這會取代 HTML 的保留的字元 (例如`<`並`>`和`&`) 與啟用顯示網頁，而不會轉譯為 HTML 標記或實體中的字元為字元的代碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-129">This replaces reserved HTML characters (such as `<` and `>` and `&`) with codes that enable the characters to be displayed as characters in a web page instead of being interpreted as HTML tags or entities.</span></span> <span data-ttu-id="3d472-130">而不進行 HTML 編碼，從伺服器程式碼的輸出可能不正確，會顯示，並可能會暴露於安全性風險的頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-130">Without HTML encoding, the output from your server code might not display correctly, and could expose a page to security risks.</span></span>
> 
> <span data-ttu-id="3d472-131">如果您的目標是要輸出轉譯為標記的標記的 HTML 標記 (例如`<p></p>`段落或`<em></em>`來強調文字)，請參閱節[結合文字、 標記和程式碼區塊中的程式碼](#BM_CombiningTextMarkupAndCode)本文稍後的。</span><span class="sxs-lookup"><span data-stu-id="3d472-131">If your goal is to output HTML markup that renders tags as markup (for example `<p></p>` for a paragraph or `<em></em>` to emphasize text), see the section [Combining Text, Markup, and Code in Code Blocks](#BM_CombiningTextMarkupAndCode) later in this article.</span></span>
> 
> <span data-ttu-id="3d472-132">您可以深入了解中的 HTML 編碼[使用 ASP.NET Web Pages 網站中的 HTML 表單](https://go.microsoft.com/fwlink/?LinkId=202892)。</span><span class="sxs-lookup"><span data-stu-id="3d472-132">You can read more about HTML encoding in [Working with HTML Forms in ASP.NET Web Pages Sites](https://go.microsoft.com/fwlink/?LinkId=202892).</span></span>


### <a name="2-you-enclose-code-blocks-with-codeend-code"></a><span data-ttu-id="3d472-133">2.您將程式碼區塊，使用程式碼...結束程式碼</span><span class="sxs-lookup"><span data-stu-id="3d472-133">2. You enclose code blocks with Code...End Code</span></span>

<span data-ttu-id="3d472-134">程式碼區塊包含一個或多個程式碼陳述式，並加上關鍵字`Code`和`End Code`。</span><span class="sxs-lookup"><span data-stu-id="3d472-134">A code block includes one or more code statements and is enclosed with the keywords `Code` and `End Code`.</span></span> <span data-ttu-id="3d472-135">將放在開頭`Code`關鍵字後面`@`字元&#8212;兩者之間不能有空白字元。</span><span class="sxs-lookup"><span data-stu-id="3d472-135">Place the opening `Code` keyword immediately after the `@` character &#8212; there can't be whitespace between them.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample2.vbhtml)]

<span data-ttu-id="3d472-136">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-136">The result displayed in a browser:</span></span>

![Razor-Img2](introducing-razor-syntax-vb/_static/image2.jpg)

### <a name="3-inside-a-block-you-end-each-code-statement-with-a-line-break"></a><span data-ttu-id="3d472-138">3.區塊中，您最終使用分行符號的每個程式碼陳述式</span><span class="sxs-lookup"><span data-stu-id="3d472-138">3. Inside a block, you end each code statement with a line break</span></span>

<span data-ttu-id="3d472-139">在 Visual Basic 程式碼區塊中，每個陳述式結尾分行符號。</span><span class="sxs-lookup"><span data-stu-id="3d472-139">In a Visual Basic code block, each statement ends with a line break.</span></span> <span data-ttu-id="3d472-140">（在本文稍後您會看到用來包裝成多行程式碼陳述式，如有需要）。</span><span class="sxs-lookup"><span data-stu-id="3d472-140">(Later in the article you'll see a way to wrap a long code statement into multiple lines if needed.)</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample3.vbhtml)]

### <a name="4-you-use-variables-to-store-values"></a><span data-ttu-id="3d472-141">4.您可以使用變數來儲存值</span><span class="sxs-lookup"><span data-stu-id="3d472-141">4. You use variables to store values</span></span>

<span data-ttu-id="3d472-142">您可以儲存中的值*變數*，包括字串、 數字和日期等。您建立新變數使用`Dim`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="3d472-142">You can store values in a *variable*, including strings, numbers, and dates, etc. You create a new variable using the `Dim` keyword.</span></span> <span data-ttu-id="3d472-143">您可以直接在頁面上，使用中插入變數的值`@`。</span><span class="sxs-lookup"><span data-stu-id="3d472-143">You can insert variable values directly in a page using `@`.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample4.vbhtml)]

<span data-ttu-id="3d472-144">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-144">The result displayed in a browser:</span></span>

![Razor-Img3](introducing-razor-syntax-vb/_static/image3.jpg)

### <a name="5-you-enclose-literal-string-values-in-double-quotation-marks"></a><span data-ttu-id="3d472-146">5.您將常值字串值括在雙引號中</span><span class="sxs-lookup"><span data-stu-id="3d472-146">5. You enclose literal string values in double quotation marks</span></span>

<span data-ttu-id="3d472-147">A*字串*是會被視為文字的字元序列。</span><span class="sxs-lookup"><span data-stu-id="3d472-147">A *string* is a sequence of characters that are treated as text.</span></span> <span data-ttu-id="3d472-148">若要指定為字串，您將它括在雙引號中：</span><span class="sxs-lookup"><span data-stu-id="3d472-148">To specify a string, you enclose it in double quotation marks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample5.vbhtml)]

<span data-ttu-id="3d472-149">若要內嵌引號內的字串值，會插入兩個雙引號字元。</span><span class="sxs-lookup"><span data-stu-id="3d472-149">To embed double quotation marks within a string value, insert two double quotation mark characters.</span></span> <span data-ttu-id="3d472-150">如果您想出現一次在網頁輸出中的雙引號字元，輸入為`""`在括住字串，並在您想要出現兩次，如果輸入為`""""`內加上引號的字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-150">If you want the double quotation character to appear once in the page output, enter it as `""` within the quoted string, and if you want it to appear twice, enter it as `""""` within the quoted string.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample6.vbhtml)]

<span data-ttu-id="3d472-151">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-151">The result displayed in a browser:</span></span>

![Razor-Img4](introducing-razor-syntax-vb/_static/image4.jpg)

### <a name="6-visual-basic-code-is-not-case-sensitive"></a><span data-ttu-id="3d472-153">6.Visual Basic 程式碼不區分大小寫</span><span class="sxs-lookup"><span data-stu-id="3d472-153">6. Visual Basic code is not case sensitive</span></span>

<span data-ttu-id="3d472-154">Visual Basic 語言不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="3d472-154">The Visual Basic language is not case sensitive.</span></span> <span data-ttu-id="3d472-155">程式設計關鍵字 (例如`Dim`， `If`，和`True`) 和變數名稱 (例如`myString`，或`subTotal`) 可以在任何情況下寫入。</span><span class="sxs-lookup"><span data-stu-id="3d472-155">Programming keywords (like `Dim`, `If`, and `True`) and variable names (like `myString`, or `subTotal`) can be written in any case.</span></span>

<span data-ttu-id="3d472-156">下列程式碼會將值指派給變數`lastname`使用小寫名稱，然後再輸出至使用大寫名稱頁面變數的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-156">The following lines of code assign a value to the variable `lastname` using a lowercase name, and then output the variable value to the page using an uppercase name.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample7.vbhtml)]

<span data-ttu-id="3d472-157">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-157">The result displayed in a browser:</span></span>

![vb 語法 5](introducing-razor-syntax-vb/_static/image5.jpg)

### <a name="7-much-of-your-coding-involves-working-with-objects"></a><span data-ttu-id="3d472-159">7.大部分您撰寫程式碼牽涉到使用物件</span><span class="sxs-lookup"><span data-stu-id="3d472-159">7. Much of your coding involves working with objects</span></span>

<span data-ttu-id="3d472-160">物件表示您可以使用程式設計的項目&#8212;頁面、 文字方塊、 檔案、 影像、 web 要求、 電子郵件訊息、 客戶記錄 （資料庫資料列） 等等。物件具有描述其特性的屬性&#8212;的文字 方塊中的物件具有`Text`屬性，要求物件具有`Url`屬性，電子郵件訊息已`From`屬性，與客戶物件都有`FirstName`屬性。</span><span class="sxs-lookup"><span data-stu-id="3d472-160">An object represents a thing that you can program with &#8212; a page, a text box, a file, an image, a web request, an email message, a customer record (database row), etc. Objects have properties that describe their characteristics &#8212; a text box object has a `Text` property, a request object has a `Url` property, an email message has a `From` property, and a customer object has a `FirstName` property.</span></span> <span data-ttu-id="3d472-161">物件也有方法&quot;動詞&quot;他們可以執行。</span><span class="sxs-lookup"><span data-stu-id="3d472-161">Objects also have methods that are the &quot;verbs&quot; they can perform.</span></span> <span data-ttu-id="3d472-162">範例包括將檔案物件的`Save`方法中，映像物件的`Rotate`方法，以及電子郵件物件的`Send`方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-162">Examples include a file object's `Save` method, an image object's `Rotate` method, and an email object's `Send` method.</span></span>

<span data-ttu-id="3d472-163">您會經常使用`Request`物件，提供您資訊，例如值的表單欄位 （文字方塊等），頁面上進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型。此範例示範如何存取的屬性`Request`物件，以及如何呼叫`MapPath`方法`Request`物件，可讓您在伺服器的頁面上的絕對路徑：</span><span class="sxs-lookup"><span data-stu-id="3d472-163">You'll often work with the `Request` object, which gives you information like the values of form fields on the page (text boxes, etc.), what type of browser made the request, the URL of the page, the user identity, etc. This example shows how to access properties of the `Request` object and how to call the `MapPath` method of the `Request` object, which gives you the absolute path of the page on the server:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample8.html)]

<span data-ttu-id="3d472-164">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-164">The result displayed in a browser:</span></span>

![Razor Img5](introducing-razor-syntax-vb/_static/image6.jpg)

### <a name="8-you-can-write-code-that-makes-decisions"></a><span data-ttu-id="3d472-166">8.您可以撰寫程式碼，讓決策</span><span class="sxs-lookup"><span data-stu-id="3d472-166">8. You can write code that makes decisions</span></span>

<span data-ttu-id="3d472-167">動態網頁的重要功能是，您可以決定如何根據條件。</span><span class="sxs-lookup"><span data-stu-id="3d472-167">A key feature of dynamic web pages is that you can determine what to do based on conditions.</span></span> <span data-ttu-id="3d472-168">若要這樣做最常見的方法是使用`If`陳述式 (和選擇性`Else`陳述式)。</span><span class="sxs-lookup"><span data-stu-id="3d472-168">The most common way to do this is with the `If` statement (and optional `Else` statement).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample9.vbhtml)]

<span data-ttu-id="3d472-169">陳述式`If IsPost`是本文撰寫的簡略方法`If IsPost = True`。</span><span class="sxs-lookup"><span data-stu-id="3d472-169">The statement `If IsPost` is a shorthand way of writing `If IsPost = True`.</span></span> <span data-ttu-id="3d472-170">連同`If`陳述式，有各種不同的方式來測試條件，重複程式碼區塊，並依此類推，這是本文稍後所述。</span><span class="sxs-lookup"><span data-stu-id="3d472-170">Along with `If` statements, there are a variety of ways to test conditions, repeat blocks of code, and so on, which are described later in this article.</span></span>

<span data-ttu-id="3d472-171">在瀏覽器中顯示結果 (按一下後**送出**):</span><span class="sxs-lookup"><span data-stu-id="3d472-171">The result displayed in a browser (after clicking **Submit**):</span></span>

![Razor Img6](introducing-razor-syntax-vb/_static/image7.jpg)

> [!TIP] 
> 
> <span data-ttu-id="3d472-173">**HTTP GET 和 POST 方法與 IsPost 屬性**</span><span class="sxs-lookup"><span data-stu-id="3d472-173">**HTTP GET and POST Methods and the IsPost Property**</span></span>
> 
> <span data-ttu-id="3d472-174">用於 web 網頁 (HTTP) 的通訊協定支援非常有限的數目的方法 (&quot;動詞&quot;)，用來對伺服器提出要求。</span><span class="sxs-lookup"><span data-stu-id="3d472-174">The protocol used for web pages (HTTP) supports a very limited number of methods (&quot;verbs&quot;) that are used to make requests to the server.</span></span> <span data-ttu-id="3d472-175">最常見的兩個為 GET，會用來讀取的頁面和文章，用來提交頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-175">The two most common ones are GET, which is used to read a page, and POST, which is used to submit a page.</span></span> <span data-ttu-id="3d472-176">一般情況下，使用者要求的頁面上，第一次要求頁面時使用 GET。</span><span class="sxs-lookup"><span data-stu-id="3d472-176">In general, the first time a user requests a page, the page is requested using GET.</span></span> <span data-ttu-id="3d472-177">如果使用者填寫表單，並按一下**送出**，瀏覽器對伺服器提出 POST 要求。</span><span class="sxs-lookup"><span data-stu-id="3d472-177">If the user fills in a form and then clicks **Submit**, the browser makes a POST request to the server.</span></span>
> 
> <span data-ttu-id="3d472-178">在 web 程式設計中，通常很有用知道是否要求網頁時正在為 GET 或 POST，讓您知道如何處理頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-178">In web programming, it's often useful to know whether a page is being requested as a GET or as a POST so that you know how to process the page.</span></span> <span data-ttu-id="3d472-179">ASP.NET Web Pages 中，您可以使用`IsPost`屬性，以查看要求是 GET 或 POST。</span><span class="sxs-lookup"><span data-stu-id="3d472-179">In ASP.NET Web Pages, you can use the `IsPost` property to see whether a request is a GET or a POST.</span></span> <span data-ttu-id="3d472-180">如果要求是某篇文章，`IsPost`屬性會傳回 true，而且您可以執行像是讀取表單上的文字方塊的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-180">If the request is a POST, the `IsPost` property will return true, and you can do things like read the values of text boxes on a form.</span></span> <span data-ttu-id="3d472-181">您會看到的許多範例會示範如何處理值的方式而定頁面`IsPost`。</span><span class="sxs-lookup"><span data-stu-id="3d472-181">Many examples you'll see show you how to process the page differently depending on the value of `IsPost`.</span></span>


## <a name="a-simple-code-example"></a><span data-ttu-id="3d472-182">簡單的程式碼範例</span><span class="sxs-lookup"><span data-stu-id="3d472-182">A Simple Code Example</span></span>

<span data-ttu-id="3d472-183">此程序會示範如何建立一個網頁，說明基本的程式設計技術。</span><span class="sxs-lookup"><span data-stu-id="3d472-183">This procedure shows you how to create a page that illustrates basic programming techniques.</span></span> <span data-ttu-id="3d472-184">在範例中，您可以建立讓使用者輸入兩個數字，然後再將它們加入，並顯示結果頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-184">In the example, you create a page that lets users enter two numbers, then it adds them and displays the result.</span></span>

1. <span data-ttu-id="3d472-185">在編輯器中，建立新的檔案並將它命名*AddNumbers.vbhtml*。</span><span class="sxs-lookup"><span data-stu-id="3d472-185">In your editor, create a new file and name it *AddNumbers.vbhtml*.</span></span>
2. <span data-ttu-id="3d472-186">將下列程式碼和標記複製到頁面上，而且要取代已經在頁面中的任何項目中。</span><span class="sxs-lookup"><span data-stu-id="3d472-186">Copy the following code and markup into the page, replacing anything already in the page.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample10.vbhtml)]

    <span data-ttu-id="3d472-187">以下是您必須注意的事項：</span><span class="sxs-lookup"><span data-stu-id="3d472-187">Here are some things for you to note:</span></span>

    - <span data-ttu-id="3d472-188">`@`字元會在頁面中，啟動程式碼的第一個區塊，而且它前面出現`totalMessage`變數內嵌底部附近。</span><span class="sxs-lookup"><span data-stu-id="3d472-188">The `@` character starts the first block of code in the page, and it precedes the `totalMessage` variable embedded near the bottom.</span></span>
    - <span data-ttu-id="3d472-189">位於頁面頂端的區塊括住`Code...End Code`。</span><span class="sxs-lookup"><span data-stu-id="3d472-189">The block at the top of the page is enclosed in `Code...End Code`.</span></span>
    - <span data-ttu-id="3d472-190">變數`total`， `num1`， `num2`，和`totalMessage`儲存數個數字和字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-190">The variables `total`, `num1`, `num2`, and `totalMessage` store several numbers and a string.</span></span>
    - <span data-ttu-id="3d472-191">常值字串值，指派給`totalMessage`變數是雙引號括住。</span><span class="sxs-lookup"><span data-stu-id="3d472-191">The literal string value assigned to the `totalMessage` variable is in double quotation marks.</span></span>
    - <span data-ttu-id="3d472-192">因為 Visual Basic 程式碼是不區分大小寫，當`totalMessage`頁面底部附近，會使用變數，其名稱只必須符合變數宣告，在頁面頂端的拼字。</span><span class="sxs-lookup"><span data-stu-id="3d472-192">Because Visual Basic code is not case sensitive, when the `totalMessage` variable is used near the bottom of the page, its name only needs to match the spelling of the variable declaration at the top of the page.</span></span> <span data-ttu-id="3d472-193">大小寫並不重要。</span><span class="sxs-lookup"><span data-stu-id="3d472-193">The casing doesn't matter.</span></span>
    - <span data-ttu-id="3d472-194">運算式`num1.AsInt()`  +  `num2.AsInt()`示範如何使用物件和方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-194">The expression `num1.AsInt()` + `num2.AsInt()` shows how to work with objects and methods.</span></span> <span data-ttu-id="3d472-195">`AsInt`上每個變數的方法會將轉換為整數 （整數） 可加入的使用者所輸入的字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-195">The `AsInt` method on each variable converts the string entered by a user to a whole number (an integer) that can be added.</span></span>
    - <span data-ttu-id="3d472-196">`<form>`標記會包括`method="post"`屬性。</span><span class="sxs-lookup"><span data-stu-id="3d472-196">The `<form>` tag includes a `method="post"` attribute.</span></span> <span data-ttu-id="3d472-197">這表示當使用者按一下**新增**，頁面將會傳送到伺服器使用 HTTP POST 方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-197">This specifies that when the user clicks **Add**, the page will be sent to the server using the HTTP POST method.</span></span> <span data-ttu-id="3d472-198">當送出頁面、 程式碼`If IsPost`評估為 true，條件式程式碼執行時，顯示個數字相加的結果。</span><span class="sxs-lookup"><span data-stu-id="3d472-198">When the page is submitted, the code `If IsPost` evaluates to true and the conditional code runs, displaying the result of adding the numbers.</span></span>
3. <span data-ttu-id="3d472-199">儲存頁面，並在瀏覽器中執行它。</span><span class="sxs-lookup"><span data-stu-id="3d472-199">Save the page and run it in a browser.</span></span> <span data-ttu-id="3d472-200">(請確定中選取頁面**檔案**才能執行這個工作區。)輸入兩個整數數字，然後按一下**新增** 按鈕。</span><span class="sxs-lookup"><span data-stu-id="3d472-200">(Make sure the page is selected in the **Files** workspace before you run it.) Enter two whole numbers and then click the **Add** button.</span></span>

    ![Razor Img7](introducing-razor-syntax-vb/_static/image8.jpg)

## <a name="visual-basic-language-and-syntax"></a><span data-ttu-id="3d472-202">Visual Basic 語言和語法</span><span class="sxs-lookup"><span data-stu-id="3d472-202">Visual Basic Language and Syntax</span></span>

<span data-ttu-id="3d472-203">先前您已看到如何建立 ASP.NET 網頁，以及如何將伺服器程式碼新增至 HTML 標記的基本範例。</span><span class="sxs-lookup"><span data-stu-id="3d472-203">Earlier you saw a basic example of how to create an ASP.NET web page, and how you can add server code to HTML markup.</span></span> <span data-ttu-id="3d472-204">這裡您將了解使用 Visual Basic 來撰寫 ASP.NET 伺服器程式碼使用 Razor 語法的基本概念&#8212;也就是程式設計語言規則。</span><span class="sxs-lookup"><span data-stu-id="3d472-204">Here you'll learn the basics of using Visual Basic to write ASP.NET server code using the Razor syntax &#8212; that is, the programming language rules.</span></span>

<span data-ttu-id="3d472-205">如果您是熟悉程式設計 （尤其是如果您已使用 C、 c + +、 C#、 Visual Basic 或 JavaScript），大多什麼您閱讀這裡應該不陌生。</span><span class="sxs-lookup"><span data-stu-id="3d472-205">If you're experienced with programming (especially if you've used C, C++, C#, Visual Basic, or JavaScript), much of what you read here will be familiar.</span></span> <span data-ttu-id="3d472-206">您可能需要在自己熟悉只 WebMatrix 程式碼新增至標記中的如何 *.vbhtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="3d472-206">You'll probably need to familiarize yourself only with how WebMatrix code is added to markup in *.vbhtml* files.</span></span>

### <a id="BM_CombiningTextMarkupAndCode"></a>  <span data-ttu-id="3d472-207">結合文字、 標記和程式碼區塊中的程式碼</span><span class="sxs-lookup"><span data-stu-id="3d472-207">Combining text, markup, and code in code blocks</span></span>

<span data-ttu-id="3d472-208">在伺服器程式碼區塊，您通常要輸出的文字和頁面的標記。</span><span class="sxs-lookup"><span data-stu-id="3d472-208">In server code blocks, you'll often want to output text and markup to the page.</span></span> <span data-ttu-id="3d472-209">如果伺服器程式碼區塊包含的文字，不是程式碼，而是應轉譯為是，ASP.NET 必須能夠區別該文字與程式碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-209">If a server code block contains text that's not code and that instead should be rendered as is, ASP.NET needs to be able to distinguish that text from code.</span></span> <span data-ttu-id="3d472-210">有幾個方式可做到這點。</span><span class="sxs-lookup"><span data-stu-id="3d472-210">There are several ways to do this.</span></span>

- <span data-ttu-id="3d472-211">中的 HTML 區塊項目，例如括住文字`<p></p>`或`<em></em>`:</span><span class="sxs-lookup"><span data-stu-id="3d472-211">Enclose the text in an HTML block element like `<p></p>` or `<em></em>`:</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample11.vbhtml)]

    <span data-ttu-id="3d472-212">HTML 項目可以包含文字、 其他 HTML 項目和伺服器程式碼運算式。</span><span class="sxs-lookup"><span data-stu-id="3d472-212">The HTML element can include text, additional HTML elements, and server-code expressions.</span></span> <span data-ttu-id="3d472-213">當 ASP.NET 看到 HTML 開頭標記 (例如`<p>`) 它將元素呈現的所有項目，做為其內容是以瀏覽器 （並解析為伺服器程式碼運算式）。</span><span class="sxs-lookup"><span data-stu-id="3d472-213">When ASP.NET sees the opening HTML tag (for example, `<p>`), it renders everything the element and its content as is to the browser (and resolves the server-code expressions).</span></span>

- <span data-ttu-id="3d472-214">使用`@:`運算子或`<text>`項目。</span><span class="sxs-lookup"><span data-stu-id="3d472-214">Use the `@:` operator or the `<text>` element.</span></span> <span data-ttu-id="3d472-215">`@:`輸出內容包含純文字] 或 [無對應的 HTML 標記。 單行`<text>`元素會括住要輸出的多行。</span><span class="sxs-lookup"><span data-stu-id="3d472-215">The `@:` outputs a single line of content containing plain text or unmatched HTML tags; the `<text>` element encloses multiple lines to output.</span></span> <span data-ttu-id="3d472-216">當您不想要呈現的 HTML 項目作為輸出的一部分，則這些選項會很有用。</span><span class="sxs-lookup"><span data-stu-id="3d472-216">These options are useful when you don't want to render an HTML element as part of the output.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample12.vbhtml)]

    <span data-ttu-id="3d472-217">下列範例會重複上一個範例，但會使用一對`<text>`標記括住要轉譯的文字。</span><span class="sxs-lookup"><span data-stu-id="3d472-217">The following example repeats the previous example but uses a single pair of `<text>` tags to enclose the text to render.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample13.vbhtml)]

    <span data-ttu-id="3d472-218">在下列範例中，`<text>`並`</text>`標記括住三行，全部都有部分未包含的文字和不相符的 HTML 標記 (`<br />`)，以及伺服器程式碼和相符的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="3d472-218">In the following example, the `<text>` and `</text>` tags enclose three lines, all of which have some uncontained text and unmatched HTML tags (`<br />`), along with server code and matched HTML tags.</span></span> <span data-ttu-id="3d472-219">同樣地，您也無法在個別使用每一行`@:`運算子; 其中一個的方式運作。</span><span class="sxs-lookup"><span data-stu-id="3d472-219">Again, you could also precede each line individually with the `@:` operator; either way works.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample14.vbhtml)]

    > [!NOTE]
    > <span data-ttu-id="3d472-220">當您輸出文字，這一節所示&#8212;使用 HTML 元素，`@:`運算子，或`<text>`項目&#8212;ASP.NET 不進行 HTML 編碼的輸出。</span><span class="sxs-lookup"><span data-stu-id="3d472-220">When you output text as shown in this section &#8212; using an HTML element, the `@:` operator, or the `<text>` element &#8212; ASP.NET doesn't HTML-encode the output.</span></span> <span data-ttu-id="3d472-221">(如先前所述，ASP.NET 未編碼的伺服器程式碼運算式和伺服器程式碼區塊，前面都會加上輸出`@`，除非在這一節所述的特殊案例。)</span><span class="sxs-lookup"><span data-stu-id="3d472-221">(As noted earlier, ASP.NET does encode the output of server code expressions and server code blocks that are preceded by `@`, except in the special cases noted in this section.)</span></span>

### <a name="whitespace"></a><span data-ttu-id="3d472-222">Whitespace</span><span class="sxs-lookup"><span data-stu-id="3d472-222">Whitespace</span></span>

<span data-ttu-id="3d472-223">額外的空格，陳述式 （內部和外部字串常值），就不會影響陳述式：</span><span class="sxs-lookup"><span data-stu-id="3d472-223">Extra spaces in a statement (and outside of a string literal) don't affect the statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample15.vbhtml)]

### <a name="breaking-long-statements-into-multiple-lines"></a><span data-ttu-id="3d472-224">長的陳述式分成多行</span><span class="sxs-lookup"><span data-stu-id="3d472-224">Breaking long statements into multiple lines</span></span>

<span data-ttu-id="3d472-225">您也可以使用底線字元為多行中斷很長的程式碼陳述式`_`(在 Visual Basic 中稱為*接續字元*) 在每一行程式碼之後。</span><span class="sxs-lookup"><span data-stu-id="3d472-225">You can break a long code statement into multiple lines by using the underscore character `_` (which in Visual Basic is called the *continuation character*) after each line of code.</span></span> <span data-ttu-id="3d472-226">若要中斷陳述式到下一行中，線的一端會新增一個空格，然後接續字元。</span><span class="sxs-lookup"><span data-stu-id="3d472-226">To break a statement onto the next line, at the end of the line add a space and then the continuation character.</span></span> <span data-ttu-id="3d472-227">在下一行繼續陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-227">Continue the statement on the next line.</span></span> <span data-ttu-id="3d472-228">您可以包裝到您需要以提高可讀性的行數的陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-228">You can wrap statements onto as many lines as you need to improve readability.</span></span> <span data-ttu-id="3d472-229">下列陳述式都相同：</span><span class="sxs-lookup"><span data-stu-id="3d472-229">The following statements are the same:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample16.vbhtml)]

<span data-ttu-id="3d472-230">不過，您無法包裝字串常值的中間一條線。</span><span class="sxs-lookup"><span data-stu-id="3d472-230">However, you can't wrap a line in the middle of a string literal.</span></span> <span data-ttu-id="3d472-231">下列範例無法運作︰</span><span class="sxs-lookup"><span data-stu-id="3d472-231">The following example doesn't work:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample17.vbhtml)]

<span data-ttu-id="3d472-232">若要結合換行至類似上述的程式碼的多行的長字串，您必須使用*串連運算子*(`&`)，您將在本文稍後看到。</span><span class="sxs-lookup"><span data-stu-id="3d472-232">To combine a long string that wraps to multiple lines like the above code, you would need to use the *concatenation operator* (`&`), which you'll see later in this article.</span></span>

### <a name="code-comments"></a><span data-ttu-id="3d472-233">程式碼註解</span><span class="sxs-lookup"><span data-stu-id="3d472-233">Code comments</span></span>

<span data-ttu-id="3d472-234">註解可讓您為您自己或其他保留資訊。</span><span class="sxs-lookup"><span data-stu-id="3d472-234">Comments let you leave notes for yourself or others.</span></span> <span data-ttu-id="3d472-235">Razor 語法的註解前面會加上`@*`結尾`*@`。</span><span class="sxs-lookup"><span data-stu-id="3d472-235">Razor syntax comments are prefixed with `@*` and end with `*@`.</span></span>

[!code-cshtml[Main](introducing-razor-syntax-vb/samples/sample18.cshtml)]

<span data-ttu-id="3d472-236">在程式碼區塊中，您可以使用 Razor 語法的註解，或者您可以使用一般的 Visual Basic 註解字元，也就是單引號 (`'`) 前面加上一行。</span><span class="sxs-lookup"><span data-stu-id="3d472-236">Within code blocks you can use the Razor syntax comments, or you can use ordinary Visual Basic comment character, which is a single quote (`'`) prefixed to each line.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample19.vbhtml)]

## <a name="variables"></a><span data-ttu-id="3d472-237">變數</span><span class="sxs-lookup"><span data-stu-id="3d472-237">Variables</span></span>

<span data-ttu-id="3d472-238">變數是您用來儲存資料的具名的物件。</span><span class="sxs-lookup"><span data-stu-id="3d472-238">A variable is a named object that you use to store data.</span></span> <span data-ttu-id="3d472-239">您可以任何項目，名稱變數名稱必須以字母字元開頭但不能包含空格或保留的字元。</span><span class="sxs-lookup"><span data-stu-id="3d472-239">You can name variables anything, but the name must begin with an alphabetic character and it cannot contain whitespace or reserved characters.</span></span> <span data-ttu-id="3d472-240">在 Visual Basic 中，如您所見更早版本，並不重要的變數名稱中的字母大小寫。</span><span class="sxs-lookup"><span data-stu-id="3d472-240">In Visual Basic, as you saw earlier, the case of the letters in a variable name doesn't matter.</span></span>

### <a name="variables-and-data-types"></a><span data-ttu-id="3d472-241">變數和資料類型</span><span class="sxs-lookup"><span data-stu-id="3d472-241">Variables and data types</span></span>

<span data-ttu-id="3d472-242">變數可以有特定的資料類型，表示何種資料儲存在變數中。</span><span class="sxs-lookup"><span data-stu-id="3d472-242">A variable can have a specific data type, which indicates what kind of data is stored in the variable.</span></span> <span data-ttu-id="3d472-243">您可以儲存字串值的字串變數 (例如&quot;Hello world&quot;)，儲存整數值 （例如 3 或 79） 的整數變數和日期值儲存在各種不同的格式 （例如 2012 年 4 月 12 日或 2009 年 3 月的日期變數).</span><span class="sxs-lookup"><span data-stu-id="3d472-243">You can have string variables that store string values (like &quot;Hello world&quot;), integer variables that store whole-number values (like 3 or 79), and date variables that store date values in a variety of formats (like 4/12/2012 or March 2009).</span></span> <span data-ttu-id="3d472-244">而且有許多您可以使用其他資料型別。</span><span class="sxs-lookup"><span data-stu-id="3d472-244">And there are many other data types you can use.</span></span>

<span data-ttu-id="3d472-245">不過，您不必指定變數的類型。</span><span class="sxs-lookup"><span data-stu-id="3d472-245">However, you don't have to specify a type for a variable.</span></span> <span data-ttu-id="3d472-246">在大部分情況下 ASP.NET 可以找出如何使用變數中的資料為基礎的類型。</span><span class="sxs-lookup"><span data-stu-id="3d472-246">In most cases ASP.NET can figure out the type based on how the data in the variable is being used.</span></span> <span data-ttu-id="3d472-247">（有時候您必須指定一個類型，您會看到範例即為 true）。</span><span class="sxs-lookup"><span data-stu-id="3d472-247">(Occasionally you must specify a type; you'll see examples where this is true.)</span></span>

<span data-ttu-id="3d472-248">若要宣告變數但未指定類型時，使用`Dim`再加上變數的名稱 (例如， `Dim myVar`)。</span><span class="sxs-lookup"><span data-stu-id="3d472-248">To declare a variable without specifying a type, use `Dim` plus the variable name (for instance, `Dim myVar`).</span></span> <span data-ttu-id="3d472-249">若要宣告變數的類型時，使用`Dim`變數的名稱，後面接著加號`As`，然後是類型名稱 (比方說， `Dim myVar As String`)。</span><span class="sxs-lookup"><span data-stu-id="3d472-249">To declare a variable with a type, use `Dim` plus the variable name, followed by `As` and then the type name (for instance, `Dim myVar As String`).</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample20.vbhtml)]

<span data-ttu-id="3d472-250">下列範例會顯示在網頁中使用變數某些內嵌運算式。</span><span class="sxs-lookup"><span data-stu-id="3d472-250">The following example shows some inline expressions that use the variables in a web page.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample21.vbhtml)]

<span data-ttu-id="3d472-251">在瀏覽器中顯示結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-251">The result displayed in a browser:</span></span>

![Razor-Img9](introducing-razor-syntax-vb/_static/image9.jpg)

### <a name="converting-and-testing-data-types"></a><span data-ttu-id="3d472-253">轉換和測試資料類型</span><span class="sxs-lookup"><span data-stu-id="3d472-253">Converting and testing data types</span></span>

<span data-ttu-id="3d472-254">雖然 ASP.NET 通常可以自動判斷資料型別，有時它不可轉換。</span><span class="sxs-lookup"><span data-stu-id="3d472-254">Although ASP.NET can usually determine a data type automatically, sometimes it can't.</span></span> <span data-ttu-id="3d472-255">因此，您可能需要協助 ASP.NET 執行的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="3d472-255">Therefore, you might need to help ASP.NET out by performing an explicit conversion.</span></span> <span data-ttu-id="3d472-256">即使您沒有轉換的型別，有時候很有幫助測試以查看何種資料類型您可能會使用。</span><span class="sxs-lookup"><span data-stu-id="3d472-256">Even if you don't have to convert types, sometimes it's helpful to test to see what type of data you might be working with.</span></span>

<span data-ttu-id="3d472-257">最常見的情況是，您必須將字串轉換成其他類型，例如整數或日期。</span><span class="sxs-lookup"><span data-stu-id="3d472-257">The most common case is that you have to convert a string to another type, such as to an integer or date.</span></span> <span data-ttu-id="3d472-258">下列範例示範一般的情況下，您必須將字串轉換為數字。</span><span class="sxs-lookup"><span data-stu-id="3d472-258">The following example shows a typical case where you must convert a string to a number.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample22.vbhtml)]

<span data-ttu-id="3d472-259">因此，使用者輸入來到您為字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-259">As a rule, user input comes to you as strings.</span></span> <span data-ttu-id="3d472-260">即使您已提示使用者輸入數字，即使它們已送出使用者輸入和您在程式碼中讀取時所輸入數字、 資料的格式字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-260">Even if you've prompted the user to enter a number, and even if they've entered a digit, when user input is submitted and you read it in code, the data is in string format.</span></span> <span data-ttu-id="3d472-261">因此，您必須將字串轉換為數字。</span><span class="sxs-lookup"><span data-stu-id="3d472-261">Therefore, you must convert the string to a number.</span></span> <span data-ttu-id="3d472-262">在範例中，如果您嘗試對值執行算術運算，而不需要轉換，會產生下列錯誤，因為 ASP.NET 無法新增兩個字串：</span><span class="sxs-lookup"><span data-stu-id="3d472-262">In the example, if you try to perform arithmetic on the values without converting them, the following error results, because ASP.NET cannot add two strings:</span></span>

`Cannot implicitly convert type 'string' to 'int'.`

<span data-ttu-id="3d472-263">若要轉換成整數的值，請呼叫`AsInt`方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-263">To convert the values to integers, you call the `AsInt` method.</span></span> <span data-ttu-id="3d472-264">如果轉換成功，然後您可以加入數字。</span><span class="sxs-lookup"><span data-stu-id="3d472-264">If the conversion is successful, you can then add the numbers.</span></span>

<span data-ttu-id="3d472-265">下表列出一些常見的轉換和測試方法的變數。</span><span class="sxs-lookup"><span data-stu-id="3d472-265">The following table lists some common conversion and test methods for variables.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="3d472-266"><strong>方法</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-266"><strong>Method</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-267"><strong>描述</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-267"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-268"><strong>範例</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-268"><strong>Example</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsInt(), IsInt()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-269">將代表整數的字串轉換 (例如&quot;593&quot;) 成整數。</span><span class="sxs-lookup"><span data-stu-id="3d472-269">Converts a string that represents a whole number (like &quot;593&quot;) to an integer.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample23.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsBool(), IsBool()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-270">將轉換的字串，例如&quot;，則為 true&quot;或是&quot;false&quot;布林型別。</span><span class="sxs-lookup"><span data-stu-id="3d472-270">Converts a string like &quot;true&quot; or &quot;false&quot; to a Boolean type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample24.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsFloat(), IsFloat()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-271">將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;浮點數。</span><span class="sxs-lookup"><span data-stu-id="3d472-271">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a floating-point number.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample25.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDecimal(), IsDecimal()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-272">將具有類似的十進位值的字串轉換&quot;1.3&quot;或是&quot;7.439&quot;十進位數字。</span><span class="sxs-lookup"><span data-stu-id="3d472-272">Converts a string that has a decimal value like &quot;1.3&quot; or &quot;7.439&quot; to a decimal number.</span></span> <span data-ttu-id="3d472-273">（在 ASP.NET 中，十進位數字是更精確比浮點數）。</span><span class="sxs-lookup"><span data-stu-id="3d472-273">(In ASP.NET, a decimal number is more precise than a floating-point number.)</span></span> :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample26.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AsDateTime(), IsDateTime()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-274">將 asp.net 代表的日期和時間值的字串轉換`DateTime`型別。</span><span class="sxs-lookup"><span data-stu-id="3d472-274">Converts a string that represents a date and time value to the ASP.NET `DateTime` type.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample27.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `ToString()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-275">將任何其他資料類型轉換為字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-275">Converts any other data type to a string.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample28.vb)]
    :::column-end:::
:::row-end:::


## <a name="operators"></a><span data-ttu-id="3d472-276">運算子</span><span class="sxs-lookup"><span data-stu-id="3d472-276">Operators</span></span>

<span data-ttu-id="3d472-277">運算子是關鍵字或字元，會告訴 ASP.NET 在運算式中執行命令的類型。</span><span class="sxs-lookup"><span data-stu-id="3d472-277">An operator is a keyword or character that tells ASP.NET what kind of command to perform in an expression.</span></span> <span data-ttu-id="3d472-278">Visual Basic 支援許多運算子，但您只需要識別一些開始使用開發 ASP.NET 網頁。</span><span class="sxs-lookup"><span data-stu-id="3d472-278">Visual Basic supports many operators, but you only need to recognize a few to get started developing ASP.NET web pages.</span></span> <span data-ttu-id="3d472-279">下表摘要說明最常見的運算子。</span><span class="sxs-lookup"><span data-stu-id="3d472-279">The following table summarizes the most common operators.</span></span>


:::row:::
    :::column:::
        <span data-ttu-id="3d472-280"><strong>Operator</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-280"><strong>Operator</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-281"><strong>描述</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-281"><strong>Description</strong></span></span>
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-282"><strong>範例</strong></span><span class="sxs-lookup"><span data-stu-id="3d472-282"><strong>Examples</strong></span></span>
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+ - * /`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-283">用在數值運算式的數學運算子。</span><span class="sxs-lookup"><span data-stu-id="3d472-283">Math operators used in numerical expressions.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample29.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-284">指派和相等。</span><span class="sxs-lookup"><span data-stu-id="3d472-284">Assignment and equality.</span></span> <span data-ttu-id="3d472-285">根據內容，將陳述式的右邊的值指派給物件，在左側，或檢查值相等。</span><span class="sxs-lookup"><span data-stu-id="3d472-285">Depending on context, either assigns the value on the right side of a statement to the object on the left side, or checks the values for equality.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample30.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `<>`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-286">不等。</span><span class="sxs-lookup"><span data-stu-id="3d472-286">Inequality.</span></span> <span data-ttu-id="3d472-287">傳回`True`值是否不相等。</span><span class="sxs-lookup"><span data-stu-id="3d472-287">Returns `True` if the values are not equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample31.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `< > <= >=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-288">小於、 大於、 小於或等於、 與大於或等於。</span><span class="sxs-lookup"><span data-stu-id="3d472-288">Less than, greater than, less than or equal, and greater than or equal.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample32.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `&`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-289">串連，用來聯結字串。</span><span class="sxs-lookup"><span data-stu-id="3d472-289">Concatenation, which is used to join strings.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample33.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `+= -=`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-290">遞增和遞減運算子，以新增和從變數 （分別） 減 1。</span><span class="sxs-lookup"><span data-stu-id="3d472-290">The increment and decrement operators, which add and subtract 1 (respectively) from a variable.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample34.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `.`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-291">點。</span><span class="sxs-lookup"><span data-stu-id="3d472-291">Dot.</span></span> <span data-ttu-id="3d472-292">用來區別物件及其屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-292">Used to distinguish objects and their properties and methods.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample35.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `()`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-293">括號。</span><span class="sxs-lookup"><span data-stu-id="3d472-293">Parentheses.</span></span> <span data-ttu-id="3d472-294">群組運算式，用來將參數傳遞至方法，以及存取陣列和集合的成員。</span><span class="sxs-lookup"><span data-stu-id="3d472-294">Used to group expressions, to pass parameters to methods, and to access members of arrays and collections.</span></span>
    :::column-end:::
    :::column:::
        [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample36.vbhtml)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `Not`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-295">不。</span><span class="sxs-lookup"><span data-stu-id="3d472-295">Not.</span></span> <span data-ttu-id="3d472-296">反轉，則為 true 的值為 false，反之亦然。</span><span class="sxs-lookup"><span data-stu-id="3d472-296">Reverses a true value to false and vice versa.</span></span> <span data-ttu-id="3d472-297">常用縮寫來測試`False`(也就是針對不`True`)。</span><span class="sxs-lookup"><span data-stu-id="3d472-297">Typically used as a shorthand way to test for `False` (that is, for not `True`).</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample37.vb)]
    :::column-end:::
:::row-end:::
* * *
:::row:::
    :::column:::
        `AndAlso OrElse`
    :::column-end:::
    :::column:::
        <span data-ttu-id="3d472-298">邏輯 AND 和 OR，這用來連結條件一起。</span><span class="sxs-lookup"><span data-stu-id="3d472-298">Logical AND and OR, which are used to link conditions together.</span></span>
    :::column-end:::
    :::column:::
        [!code-vb[Main](introducing-razor-syntax-vb/samples/sample38.vb)]
    :::column-end:::
:::row-end:::

## <a name="working-with-file-and-folder-paths-in-code"></a><span data-ttu-id="3d472-299">使用檔案和程式碼中的資料夾路徑</span><span class="sxs-lookup"><span data-stu-id="3d472-299">Working with File and Folder Paths in Code</span></span>

<span data-ttu-id="3d472-300">您通常會在您的程式碼中使用檔案和資料夾的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-300">You'll often work with file and folder paths in your code.</span></span> <span data-ttu-id="3d472-301">以下是網站實體資料夾結構的範例，可能出現在您的開發電腦上：</span><span class="sxs-lookup"><span data-stu-id="3d472-301">Here is an example of physical folder structure for a website as it might appear on your development computer:</span></span>

`C:\WebSites\MyWebSite default.cshtml datafile.txt \images Logo.jpg \styles Styles.css`

<span data-ttu-id="3d472-302">以下是一些關於 Url 和路徑的基本詳細資料：</span><span class="sxs-lookup"><span data-stu-id="3d472-302">Here are some essential details about URLs and paths:</span></span>

- <span data-ttu-id="3d472-303">開始使用的網域名稱的 URL (`http://www.example.com`) 或伺服器名稱 (`http://localhost`， `http://mycomputer`)。</span><span class="sxs-lookup"><span data-stu-id="3d472-303">A URL begins with either a domain name (`http://www.example.com`) or a server name (`http://localhost`, `http://mycomputer`).</span></span>
- <span data-ttu-id="3d472-304">URL 對應至主機電腦上的實體路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-304">A URL corresponds to a physical path on a host computer.</span></span> <span data-ttu-id="3d472-305">例如，`http://myserver`可能會對應至資料夾*C:\websites\mywebsite*伺服器上。</span><span class="sxs-lookup"><span data-stu-id="3d472-305">For example, `http://myserver` might correspond to the folder *C:\websites\mywebsite* on the server.</span></span>
- <span data-ttu-id="3d472-306">虛擬路徑是以代表在程式碼中的路徑，而不需要指定完整路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-306">A virtual path is shorthand to represent paths in code without having to specify the full path.</span></span> <span data-ttu-id="3d472-307">它包含的 url 的網域或伺服器名稱後面的部分。</span><span class="sxs-lookup"><span data-stu-id="3d472-307">It includes the portion of a URL that follows the domain or server name.</span></span> <span data-ttu-id="3d472-308">當您使用虛擬路徑時，您可以移動至不同的網域或伺服器程式碼而不需要更新的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-308">When you use virtual paths, you can move your code to a different domain or server without having to update the paths.</span></span>

<span data-ttu-id="3d472-309">以下是範例，以協助您了解的差異：</span><span class="sxs-lookup"><span data-stu-id="3d472-309">Here's an example to help you understand the differences:</span></span>

| <span data-ttu-id="3d472-310">完整的 URL</span><span class="sxs-lookup"><span data-stu-id="3d472-310">Complete URL</span></span> | `http://mycompanyserver/humanresources/CompanyPolicy.htm` |
| --- | --- |
| <span data-ttu-id="3d472-311">伺服器名稱</span><span class="sxs-lookup"><span data-stu-id="3d472-311">Server name</span></span> | <span data-ttu-id="3d472-312">*mycompanyserver*</span><span class="sxs-lookup"><span data-stu-id="3d472-312">*mycompanyserver*</span></span> |
| <span data-ttu-id="3d472-313">虛擬路徑</span><span class="sxs-lookup"><span data-stu-id="3d472-313">Virtual path</span></span> | <span data-ttu-id="3d472-314">*/humanresources/CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3d472-314">*/humanresources/CompanyPolicy.htm*</span></span> |
| <span data-ttu-id="3d472-315">實體路徑</span><span class="sxs-lookup"><span data-stu-id="3d472-315">Physical path</span></span> | <span data-ttu-id="3d472-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span><span class="sxs-lookup"><span data-stu-id="3d472-316">*C:\mywebsites\humanresources\CompanyPolicy.htm*</span></span> |

<span data-ttu-id="3d472-317">虛擬根目錄是 /，就像根目錄的 c： 磁碟機 \。</span><span class="sxs-lookup"><span data-stu-id="3d472-317">The virtual root is /, just like the root of your C: drive is \.</span></span> <span data-ttu-id="3d472-318">（虛擬資料夾路徑永遠使用正斜線）。資料夾的虛擬路徑不需要有相同的名稱做為實體的資料夾;它可以是一個別名。</span><span class="sxs-lookup"><span data-stu-id="3d472-318">(Virtual folder paths always use forward slashes.) The virtual path of a folder doesn't have to have the same name as the physical folder; it can be an alias.</span></span> <span data-ttu-id="3d472-319">（在實際執行伺服器上的虛擬路徑很少比對確切的實體路徑。）</span><span class="sxs-lookup"><span data-stu-id="3d472-319">(On production servers, the virtual path rarely matches an exact physical path.)</span></span>

<span data-ttu-id="3d472-320">當您使用中的程式碼檔案和資料夾時，有時候您需要參考的實體路徑或按一下 虛擬路徑，視您正在使用哪些物件而定。</span><span class="sxs-lookup"><span data-stu-id="3d472-320">When you work with files and folders in code, sometimes you need to reference the physical path and sometimes a virtual path, depending on what objects you're working with.</span></span> <span data-ttu-id="3d472-321">ASP.NET 讓您知道這些工具使用程式碼中的檔案和資料夾路徑：`Server.MapPath`方法，而`~`運算子和`Href`方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-321">ASP.NET gives you these tools for working with file and folder paths in code: the `Server.MapPath` method, and the `~` operator and `Href` method.</span></span>

### <a name="converting-virtual-to-physical-paths-the-servermappath-method"></a><span data-ttu-id="3d472-322">轉換虛擬與實體路徑： Server.MapPath 方法</span><span class="sxs-lookup"><span data-stu-id="3d472-322">Converting virtual to physical paths: the Server.MapPath method</span></span>

<span data-ttu-id="3d472-323">`Server.MapPath`方法會將轉換的虛擬路徑 (例如 */default.cshtml*) 成絕對實體路徑 (例如*C:\WebSites\MyWebSiteFolder\default.cshtml*)。</span><span class="sxs-lookup"><span data-stu-id="3d472-323">The `Server.MapPath` method converts a virtual path (like */default.cshtml*) to an absolute physical path (like *C:\WebSites\MyWebSiteFolder\default.cshtml*).</span></span> <span data-ttu-id="3d472-324">每當您需要完整的實體路徑使用這個方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-324">You use this method any time you need a complete physical path.</span></span> <span data-ttu-id="3d472-325">當您讀取或寫入的文字檔或 web 伺服器上的映像檔，就會是一個典型的例子。</span><span class="sxs-lookup"><span data-stu-id="3d472-325">A typical example is when you're reading or writing a text file or image file on the web server.</span></span>

<span data-ttu-id="3d472-326">您通常不知道您的網站裝載站台伺服器上的絕對實體路徑，因此這個方法可以將路徑轉換您知道 — 虛擬路徑，來為您在伺服器上對應的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-326">You typically don't know the absolute physical path of your site on a hosting site's server, so this method can convert the path you do know — the virtual path — to the corresponding path on the server for you.</span></span> <span data-ttu-id="3d472-327">您將虛擬路徑傳遞至檔案或資料夾的方法，並傳回的實體路徑：</span><span class="sxs-lookup"><span data-stu-id="3d472-327">You pass the virtual path to a file or folder to the method, and it returns the physical path:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample39.vbhtml)]

### <a name="referencing-the-virtual-root-the--operator-and-href-method"></a><span data-ttu-id="3d472-328">參考的虛擬根目錄： ~ 運算子和 Href 方法</span><span class="sxs-lookup"><span data-stu-id="3d472-328">Referencing the virtual root: the ~ operator and Href method</span></span>

<span data-ttu-id="3d472-329">在  *.cshtml*或 *.vbhtml*檔案中，您可以參考虛擬根目錄的路徑使用`~`運算子。</span><span class="sxs-lookup"><span data-stu-id="3d472-329">In a *.cshtml* or *.vbhtml* file, you can reference the virtual root path using the `~` operator.</span></span> <span data-ttu-id="3d472-330">這是非常好用，因為您可以移動的頁面，在網站中，而且它們包含其他頁面的任何連結不會中斷。</span><span class="sxs-lookup"><span data-stu-id="3d472-330">This is very handy because you can move pages around in a site, and any links they contain to other pages won't be broken.</span></span> <span data-ttu-id="3d472-331">如果您曾經將您的網站移至不同的位置還有好用。</span><span class="sxs-lookup"><span data-stu-id="3d472-331">It's also handy in case you ever move your website to a different location.</span></span> <span data-ttu-id="3d472-332">以下是一些範例：</span><span class="sxs-lookup"><span data-stu-id="3d472-332">Here are some examples:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample40.vbhtml)]

<span data-ttu-id="3d472-333">如果網站是`http://myserver/myapp`，以下是 ASP.NET 如何處理此頁面會執行的這些路徑：</span><span class="sxs-lookup"><span data-stu-id="3d472-333">If the website is `http://myserver/myapp`, here's how ASP.NET will treat these paths when the page runs:</span></span>

- <span data-ttu-id="3d472-334">`myImagesFolder`: `http://myserver/myapp/images`</span><span class="sxs-lookup"><span data-stu-id="3d472-334">`myImagesFolder`: `http://myserver/myapp/images`</span></span>
- <span data-ttu-id="3d472-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span><span class="sxs-lookup"><span data-stu-id="3d472-335">`myStyleSheet` : `http://myserver/myapp/styles/Stylesheet.css`</span></span>

<span data-ttu-id="3d472-336">（您實際上不會看到這些路徑做為變數的值，但，是它們的是，ASP.NET 會將路徑）。</span><span class="sxs-lookup"><span data-stu-id="3d472-336">(You won't actually see these paths as the values of the variable, but ASP.NET will treat the paths as if that's what they were.)</span></span>

<span data-ttu-id="3d472-337">您可以使用`~`運算子伺服端程式碼 （如上所述） 並在標記中，像這樣：</span><span class="sxs-lookup"><span data-stu-id="3d472-337">You can use the `~` operator both in server code (as above) and in markup, like this:</span></span>

[!code-html[Main](introducing-razor-syntax-vb/samples/sample41.html)]

<span data-ttu-id="3d472-338">在標記中，您可以使用`~`運算子來建立資源，例如映像檔案、 其他網頁和 CSS 檔案的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-338">In markup, you use the `~` operator to create paths to resources like image files, other web pages, and CSS files.</span></span> <span data-ttu-id="3d472-339">ASP.NET 頁面執行時，尋找整個頁面 （程式碼和標記），並解決所有`~`參考適當的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-339">When the page runs, ASP.NET looks through the page (both code and markup) and resolves all the `~` references to the appropriate path.</span></span>

## <a name="conditional-logic-and-loops"></a><span data-ttu-id="3d472-340">條件式邏輯和迴圈</span><span class="sxs-lookup"><span data-stu-id="3d472-340">Conditional Logic and Loops</span></span>

<span data-ttu-id="3d472-341">ASP.NET server 程式碼可讓您根據條件和撰寫程式碼重複特定次數也就是程式碼的陳述式執行迴圈執行工作）。</span><span class="sxs-lookup"><span data-stu-id="3d472-341">ASP.NET server code lets you perform tasks based on conditions and write code that repeats statements a specific number of times that is, code that runs a loop).</span></span>

### <a name="testing-conditions"></a><span data-ttu-id="3d472-342">測試條件</span><span class="sxs-lookup"><span data-stu-id="3d472-342">Testing conditions</span></span>

<span data-ttu-id="3d472-343">若要測試您所使用的簡單條件`If...Then`陳述式，它會傳回`True`或`False`根據您指定的測試：</span><span class="sxs-lookup"><span data-stu-id="3d472-343">To test a simple condition you use the `If...Then` statement, which returns `True` or `False` based on a test you specify:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample42.vbhtml)]

<span data-ttu-id="3d472-344">`If`關鍵字開始區塊。</span><span class="sxs-lookup"><span data-stu-id="3d472-344">The `If` keyword starts a block.</span></span> <span data-ttu-id="3d472-345">實際的測試 （條件） 遵循`If`關鍵字並傳回 true 或 false。</span><span class="sxs-lookup"><span data-stu-id="3d472-345">The actual test (condition) follows the `If` keyword and returns true or false.</span></span> <span data-ttu-id="3d472-346">`If`陳述式結尾`Then`。</span><span class="sxs-lookup"><span data-stu-id="3d472-346">The `If` statement ends with `Then`.</span></span> <span data-ttu-id="3d472-347">如果測試為 true，將執行的陳述式會加上`If`和`End If`。</span><span class="sxs-lookup"><span data-stu-id="3d472-347">The statements that will run if the test is true are enclosed by `If` and `End If`.</span></span> <span data-ttu-id="3d472-348">`If`陳述式可包含`Else`，指定當條件為 false 時要執行的陳述式區塊：</span><span class="sxs-lookup"><span data-stu-id="3d472-348">An `If` statement can include an `Else` block that specifies statements to run if the condition is false:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample43.vbhtml)]

<span data-ttu-id="3d472-349">如果`If`陳述式啟動的程式碼區塊時，您不需要使用 「 正常 」`Code...End Code`包含區塊的陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-349">If an `If` statement starts a code block, you don't have to use the normal `Code...End Code` statements to include the blocks.</span></span> <span data-ttu-id="3d472-350">您可以新增`@`區塊，它會運作。</span><span class="sxs-lookup"><span data-stu-id="3d472-350">You can just add `@` to the block, and it will work.</span></span> <span data-ttu-id="3d472-351">此方法適用於`If`以及其他的 Visual Basic 程式設計的關鍵字，後面接著程式碼區塊，包括`For`， `For Each`，`Do While`等等。</span><span class="sxs-lookup"><span data-stu-id="3d472-351">This approach works with `If` as well as other Visual Basic programming keywords that are followed by code blocks, including `For`, `For Each`, `Do While`, etc.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample44.vbhtml)]

<span data-ttu-id="3d472-352">您可以加入多個條件使用一或多個`ElseIf`區塊：</span><span class="sxs-lookup"><span data-stu-id="3d472-352">You can add multiple conditions using one or more `ElseIf` blocks:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample45.vbhtml)]

<span data-ttu-id="3d472-353">在此範例中，如果第一個條件中`If`區塊不是 true，`ElseIf`在檢查條件。</span><span class="sxs-lookup"><span data-stu-id="3d472-353">In this example, if the first condition in the `If` block is not true, the `ElseIf` condition is checked.</span></span> <span data-ttu-id="3d472-354">如果符合該條件中的陳述式`ElseIf`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="3d472-354">If that condition is met, the statements in the `ElseIf` block are executed.</span></span> <span data-ttu-id="3d472-355">如果條件符合中的陳述式`Else`區塊會執行。</span><span class="sxs-lookup"><span data-stu-id="3d472-355">If none of the conditions are met, the statements in the `Else` block are executed.</span></span> <span data-ttu-id="3d472-356">您可以新增任意數目的`ElseIf`區塊中使用，並關閉具有`Else`封鎖&quot;一切&quot;條件。</span><span class="sxs-lookup"><span data-stu-id="3d472-356">You can add any number of `ElseIf` blocks, and then close with an `Else` block as the &quot;everything else&quot; condition.</span></span>

<span data-ttu-id="3d472-357">若要測試大量的條件，請使用`Select Case`區塊：</span><span class="sxs-lookup"><span data-stu-id="3d472-357">To test a large number of conditions, use a `Select Case` block:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample46.vbhtml)]

<span data-ttu-id="3d472-358">（在範例中，weekday 變數） 的括號括住，是要測試的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-358">The value to test is in parentheses (in the example, the weekday variable).</span></span> <span data-ttu-id="3d472-359">每個個別的測試會使用`Case`列出值的陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-359">Each individual test uses a `Case` statement that lists a value.</span></span> <span data-ttu-id="3d472-360">如果值`Case`陳述式的測試值符合中的程式碼`Case`區塊執行。</span><span class="sxs-lookup"><span data-stu-id="3d472-360">If the value of a `Case` statement matches the test value, the code in that `Case` block is executed.</span></span>

<span data-ttu-id="3d472-361">在瀏覽器中顯示的最後兩個條件式區塊的結果：</span><span class="sxs-lookup"><span data-stu-id="3d472-361">The result of the last two conditional blocks displayed in a browser:</span></span>

![Razor-Img10](introducing-razor-syntax-vb/_static/image10.jpg)

### <a name="looping-code"></a><span data-ttu-id="3d472-363">迴圈的程式碼</span><span class="sxs-lookup"><span data-stu-id="3d472-363">Looping code</span></span>

<span data-ttu-id="3d472-364">您通常需要重複執行相同的陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-364">You often need to run the same statements repeatedly.</span></span> <span data-ttu-id="3d472-365">您可以進行迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-365">You do this by looping.</span></span> <span data-ttu-id="3d472-366">例如，您經常執行相同的陳述式，每個項目集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="3d472-366">For example, you often run the same statements for each item in a collection of data.</span></span> <span data-ttu-id="3d472-367">如果您知道確實多少次您想要執行迴圈，您可以使用`For`迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-367">If you know exactly how many times you want to loop, you can use a `For` loop.</span></span> <span data-ttu-id="3d472-368">這種迴圈是特別適用於計算或向下計數：</span><span class="sxs-lookup"><span data-stu-id="3d472-368">This kind of loop is especially useful for counting up or counting down:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample47.vbhtml)]

<span data-ttu-id="3d472-369">迴圈開頭`For`關鍵字，後面接著三個項目：</span><span class="sxs-lookup"><span data-stu-id="3d472-369">The loop begins with the `For` keyword, followed by three elements:</span></span>

- <span data-ttu-id="3d472-370">之後立即`For`陳述式，宣告的計數器變數 (您不必使用`Dim`)，接下來指明中的 [範圍] `i = 10 to 20`。</span><span class="sxs-lookup"><span data-stu-id="3d472-370">Immediately after the `For` statement, you declare a counter variable (you don't have to use `Dim`) and then indicate the range, as in `i = 10 to 20`.</span></span> <span data-ttu-id="3d472-371">這表示變數`i`將會在 10 開始計算，並繼續，直到它到達 20 （含）。</span><span class="sxs-lookup"><span data-stu-id="3d472-371">This means the variable `i` will start counting at 10 and continue until it reaches 20 (inclusive).</span></span>
- <span data-ttu-id="3d472-372">之間`For`和`Next`陳述式是在區塊的內容。</span><span class="sxs-lookup"><span data-stu-id="3d472-372">Between the `For` and `Next` statements is the content of the block.</span></span> <span data-ttu-id="3d472-373">這可以包含一或多個程式碼執行陳述式以及每個迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-373">This can contain one or more code statements that execute with each loop.</span></span>
- <span data-ttu-id="3d472-374">`Next i`陳述式會結束迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-374">The `Next i` statement ends the loop.</span></span> <span data-ttu-id="3d472-375">它會遞增計數器，並啟動迴圈的下一個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="3d472-375">It increments the counter and starts the next iteration of the loop.</span></span>

<span data-ttu-id="3d472-376">程式碼之間的一行`For`和`Next`行包含迴圈的每個反覆項目執行的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-376">The line of code between the `For` and `Next` lines contains the code that runs for each iteration of the loop.</span></span> <span data-ttu-id="3d472-377">標記會建立新的段落 (`<p>`項目) 每次，且輸出中顯示的值中加入一行 i （計數器）。</span><span class="sxs-lookup"><span data-stu-id="3d472-377">The markup creates a new paragraph (`<p>` element) each time and adds a line to the output, displaying the value of i (the counter).</span></span> <span data-ttu-id="3d472-378">當您執行此頁面時，此範例會建立 11 行顯示輸出，表示項目數目每一行中的文字。</span><span class="sxs-lookup"><span data-stu-id="3d472-378">When you run this page, the example creates 11 lines displaying the output, with the text in each line indicating the item number.</span></span>

![Razor-Img11](introducing-razor-syntax-vb/_static/image11.jpg)

<span data-ttu-id="3d472-380">如果您正在使用集合或陣列，您經常使用`For Each`迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-380">If you're working with a collection or array, you often use a `For Each` loop.</span></span> <span data-ttu-id="3d472-381">集合是一組類似的物件，而`For Each`迴圈可讓您執行的工作集合中的每個項目上。</span><span class="sxs-lookup"><span data-stu-id="3d472-381">A collection is a group of similar objects, and the `For Each` loop lets you carry out a task on each item in the collection.</span></span> <span data-ttu-id="3d472-382">這種類型的迴圈是方便的集合，因為不像`For`迴圈中，您不需要遞增計數器，或設定限制。</span><span class="sxs-lookup"><span data-stu-id="3d472-382">This type of loop is convenient for collections, because unlike a `For` loop, you don't have to increment the counter or set a limit.</span></span> <span data-ttu-id="3d472-383">相反地，`For Each`迴圈的程式碼會繼續透過集合直到完成為止。</span><span class="sxs-lookup"><span data-stu-id="3d472-383">Instead, the `For Each` loop code simply proceeds through the collection until it's finished.</span></span>

<span data-ttu-id="3d472-384">這個範例會傳回中的項目`Request.ServerVariables`集合 （其中包含您的 web 伺服器的相關資訊）。</span><span class="sxs-lookup"><span data-stu-id="3d472-384">This example returns the items in the `Request.ServerVariables` collection (which contains information about your web server).</span></span> <span data-ttu-id="3d472-385">它會使用`For Each`迴圈，以顯示每個項目的名稱來建立新`<li>`HTML 項目符號清單中的項目。</span><span class="sxs-lookup"><span data-stu-id="3d472-385">It uses a `For Each` loop to display the name of each item by creating a new `<li>` element in an HTML bulleted list.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample48.vbhtml)]

<span data-ttu-id="3d472-386">`For Each`關鍵字後面的變數，表示集合中的單一項目 (在範例中， `myItem`)，後面接著`In`關鍵字，後面接著您要循環的集合。</span><span class="sxs-lookup"><span data-stu-id="3d472-386">The `For Each` keyword is followed by a variable that represents a single item in the collection (in the example, `myItem`), followed by the `In` keyword, followed by the collection you want to loop through.</span></span> <span data-ttu-id="3d472-387">本文的`For Each`迴圈中，您可以存取目前的項目使用您稍早宣告的變數。</span><span class="sxs-lookup"><span data-stu-id="3d472-387">In the body of the `For Each` loop, you can access the current item using the variable that you declared earlier.</span></span>

![Razor-Img12](introducing-razor-syntax-vb/_static/image12.jpg)

<span data-ttu-id="3d472-389">若要建立更通用的迴圈，請使用`Do While`陳述式：</span><span class="sxs-lookup"><span data-stu-id="3d472-389">To create a more general-purpose loop, use the `Do While` statement:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample49.vbhtml)]

<span data-ttu-id="3d472-390">這個迴圈開頭`Do While`關鍵字，後面接著一項條件，後面接著要重複的區塊。</span><span class="sxs-lookup"><span data-stu-id="3d472-390">This loop begins with the `Do While` keyword, followed by a condition, followed by the block to repeat.</span></span> <span data-ttu-id="3d472-391">迴圈通常遞增 （加入） 或遞減 （減去） 的變數或用於計算的物件。</span><span class="sxs-lookup"><span data-stu-id="3d472-391">Loops typically increment (add to) or decrement (subtract from) a variable or object used for counting.</span></span> <span data-ttu-id="3d472-392">在範例中，`+=`運算子將 1 加到變數的值在每次執行迴圈。</span><span class="sxs-lookup"><span data-stu-id="3d472-392">In the example, the `+=` operator adds 1 to the value of a variable each time the loop runs.</span></span> <span data-ttu-id="3d472-393">(以遞減的向下計數的迴圈中的變數，您會使用遞減運算子`-=`。)</span><span class="sxs-lookup"><span data-stu-id="3d472-393">(To decrement a variable in a loop that counts down, you would use the decrement operator `-=`.)</span></span>

## <a name="objects-and-collections"></a><span data-ttu-id="3d472-394">物件和集合</span><span class="sxs-lookup"><span data-stu-id="3d472-394">Objects and Collections</span></span>

<span data-ttu-id="3d472-395">幾乎在 ASP.NET 網站中所有項目是物件，包括網頁本身。</span><span class="sxs-lookup"><span data-stu-id="3d472-395">Nearly everything in an ASP.NET website is an object, including the web page itself.</span></span> <span data-ttu-id="3d472-396">本章節將討論一些重要的物件，您將使用經常在您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-396">This section discusses some important objects you'll work with frequently in your code.</span></span>

### <a name="page-objects"></a><span data-ttu-id="3d472-397">頁面物件</span><span class="sxs-lookup"><span data-stu-id="3d472-397">Page objects</span></span>

<span data-ttu-id="3d472-398">在 ASP.NET 中最基本的物件是頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-398">The most basic object in ASP.NET is the page.</span></span> <span data-ttu-id="3d472-399">您可以存取任何合格的物件不直接的 page 物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="3d472-399">You can access properties of the page object directly without any qualifying object.</span></span> <span data-ttu-id="3d472-400">下列程式碼會取得頁面的檔案路徑，使用`Request`頁面物件：</span><span class="sxs-lookup"><span data-stu-id="3d472-400">The following code gets the page's file path, using the `Request` object of the page:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample50.vbhtml)]

<span data-ttu-id="3d472-401">您可以使用屬性`Page`物件，以取得大量資訊，例如：</span><span class="sxs-lookup"><span data-stu-id="3d472-401">You can use properties of the `Page` object to get a lot of information, such as:</span></span>

- <span data-ttu-id="3d472-402">`Request`.</span><span class="sxs-lookup"><span data-stu-id="3d472-402">`Request`.</span></span> <span data-ttu-id="3d472-403">如您所見，這是目前的要求，包括進行要求的頁面、 使用者身分識別等 URL 的瀏覽器的何種類型的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="3d472-403">As you've already seen, this is a collection of information about the current request, including what type of browser made the request, the URL of the page, the user identity, etc.</span></span>
- <span data-ttu-id="3d472-404">`Response`.</span><span class="sxs-lookup"><span data-stu-id="3d472-404">`Response`.</span></span> <span data-ttu-id="3d472-405">這是會在伺服器程式碼執行完成時傳送到瀏覽器的回應 （頁面） 的相關資訊的集合。</span><span class="sxs-lookup"><span data-stu-id="3d472-405">This is a collection of information about the response (page) that will be sent to the browser when the server code has finished running.</span></span> <span data-ttu-id="3d472-406">例如，您可以使用此屬性寫入至回應的資訊。</span><span class="sxs-lookup"><span data-stu-id="3d472-406">For example, you can use this property to write information into the response.</span></span>

    [!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample51.vbhtml)]

### <a name="collection-objects-arrays-and-dictionaries"></a><span data-ttu-id="3d472-407">（陣列和字典） 的集合物件</span><span class="sxs-lookup"><span data-stu-id="3d472-407">Collection objects (arrays and dictionaries)</span></span>

<span data-ttu-id="3d472-408">集合是一組相同的類型，例如的集合物件`Customer`資料庫的物件。</span><span class="sxs-lookup"><span data-stu-id="3d472-408">A collection is a group of objects of the same type, such as a collection of `Customer` objects from a database.</span></span> <span data-ttu-id="3d472-409">ASP.NET 包含許多的內建集合，例如`Request.Files`集合。</span><span class="sxs-lookup"><span data-stu-id="3d472-409">ASP.NET contains many built-in collections, like the `Request.Files` collection.</span></span>

<span data-ttu-id="3d472-410">您通常會使用在集合中的資料。</span><span class="sxs-lookup"><span data-stu-id="3d472-410">You'll often work with data in collections.</span></span> <span data-ttu-id="3d472-411">兩個常見的集合類型*陣列*並*字典*。</span><span class="sxs-lookup"><span data-stu-id="3d472-411">Two common collection types are the *array* and the *dictionary*.</span></span> <span data-ttu-id="3d472-412">當您想要儲存一組類似的項目，但不想要建立個別的變數來保存每個項目時，陣列是很有用：</span><span class="sxs-lookup"><span data-stu-id="3d472-412">An array is useful when you want to store a collection of similar items but don't want to create a separate variable to hold each item:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample52.vbhtml)]

<span data-ttu-id="3d472-413">使用陣列時，您宣告特定的資料類型，例如`String`， `Integer`，或`DateTime`。</span><span class="sxs-lookup"><span data-stu-id="3d472-413">With arrays, you declare a specific data type, such as `String`, `Integer`, or `DateTime`.</span></span> <span data-ttu-id="3d472-414">若要指出變數可以包含陣列，您在宣告中的變數名稱加上括號 (例如`Dim myVar() As String`)。</span><span class="sxs-lookup"><span data-stu-id="3d472-414">To indicate that the variable can contain an array, you add parentheses to the variable name in the declaration (such as `Dim myVar() As String`).</span></span> <span data-ttu-id="3d472-415">您可以存取項目中使用它們的位置 （索引） 的陣列，或使用`For Each`陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-415">You can access items in an array using their position (index) or by using the `For Each` statement.</span></span> <span data-ttu-id="3d472-416">陣列索引以零為起始&#8212;也就是第一個項目是在位置 0，第二個項目位於位置 1，依此類推。</span><span class="sxs-lookup"><span data-stu-id="3d472-416">Array indexes are zero-based &#8212; that is, the first item is at position 0, the second item is at position 1, and so on.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample53.vbhtml)]

<span data-ttu-id="3d472-417">您可以判斷陣列中的項目數，藉由取得其`Length`屬性。</span><span class="sxs-lookup"><span data-stu-id="3d472-417">You can determine the number of items in an array by getting its `Length` property.</span></span> <span data-ttu-id="3d472-418">若要取得特定項目的陣列中的位置 (也就是在陣列搜尋)，使用`Array.IndexOf`方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-418">To get the position of a specific item in the array (that is, to search the array), use the `Array.IndexOf` method.</span></span> <span data-ttu-id="3d472-419">您也可以執行像是反向陣列的內容 (`Array.Reverse`方法) 或排序內容 (`Array.Sort`方法)。</span><span class="sxs-lookup"><span data-stu-id="3d472-419">You can also do things like reverse the contents of an array (the `Array.Reverse` method) or sort the contents (the `Array.Sort` method).</span></span>

<span data-ttu-id="3d472-420">在瀏覽器中顯示的字串陣列程式碼的輸出：</span><span class="sxs-lookup"><span data-stu-id="3d472-420">The output of the string array code displayed in a browser:</span></span>

![Razor-Img13](introducing-razor-syntax-vb/_static/image13.jpg)

<span data-ttu-id="3d472-422">字典是索引鍵/值組的集合，您可在此提供金鑰 （或名稱），以設定或擷取對應的值：</span><span class="sxs-lookup"><span data-stu-id="3d472-422">A dictionary is a collection of key/value pairs, where you provide the key (or name) to set or retrieve the corresponding value:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample54.vbhtml)]

<span data-ttu-id="3d472-423">若要建立字典，您使用`New`關鍵字來表示，您要建立新`Dictionary`物件。</span><span class="sxs-lookup"><span data-stu-id="3d472-423">To create a dictionary, you use the `New` keyword to indicate that you're creating a new `Dictionary` object.</span></span> <span data-ttu-id="3d472-424">您可以將字典指派給變數使用`Dim`關鍵字。</span><span class="sxs-lookup"><span data-stu-id="3d472-424">You can assign a dictionary to a variable using the `Dim` keyword.</span></span> <span data-ttu-id="3d472-425">指出資料類型的字典使用括號中的項目 ( `( )` )。</span><span class="sxs-lookup"><span data-stu-id="3d472-425">You indicate the data types of the items in the dictionary using parentheses ( `( )` ).</span></span> <span data-ttu-id="3d472-426">宣告的結尾，在中，您必須新增另一組括號，因為這是實際的方法，建立新的字典。</span><span class="sxs-lookup"><span data-stu-id="3d472-426">At the end of the declaration, you must add another pair of parentheses, because this is actually a method that creates a new dictionary.</span></span>

<span data-ttu-id="3d472-427">若要新增至字典的項目，您可以呼叫`Add`字典變數的方法 (`myScores`在此情況下)，然後指定 索引鍵和值。</span><span class="sxs-lookup"><span data-stu-id="3d472-427">To add items to the dictionary, you can call the `Add` method of the dictionary variable (`myScores` in this case), and then specify a key and a value.</span></span> <span data-ttu-id="3d472-428">或者，您可以使用括號表示索引鍵，並執行簡單的指派，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="3d472-428">Alternatively, you can use parentheses to indicate the key and do a simple assignment, as in the following example:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample55.vbhtml)]

<span data-ttu-id="3d472-429">若要從字典取得值，您可以指定索引鍵括號括住：</span><span class="sxs-lookup"><span data-stu-id="3d472-429">To get a value from the dictionary, you specify the key in parentheses:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample56.vbhtml)]

## <a name="calling-methods-with-parameters"></a><span data-ttu-id="3d472-430">呼叫具有參數的方法</span><span class="sxs-lookup"><span data-stu-id="3d472-430">Calling Methods with Parameters</span></span>

<span data-ttu-id="3d472-431">如您所見稍早在本文中，您使用程式設計的物件具有方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-431">As you saw earlier in this article, the objects that you program with have methods.</span></span> <span data-ttu-id="3d472-432">例如，`Database`物件可能會有`Database.Connect`方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-432">For example, a `Database` object might have a `Database.Connect` method.</span></span> <span data-ttu-id="3d472-433">許多的方法也會有一或多個參數。</span><span class="sxs-lookup"><span data-stu-id="3d472-433">Many methods also have one or more parameters.</span></span> <span data-ttu-id="3d472-434">A*參數*是一個值傳遞至方法，若要啟用以完成其工作的方法。</span><span class="sxs-lookup"><span data-stu-id="3d472-434">A *parameter* is a value that you pass to a method to enable the method to complete its task.</span></span> <span data-ttu-id="3d472-435">例如，看看的宣告`Request.MapPath`方法，後者會採用三個參數：</span><span class="sxs-lookup"><span data-stu-id="3d472-435">For example, look at a declaration for the `Request.MapPath` method, which takes three parameters:</span></span>

[!code-vb[Main](introducing-razor-syntax-vb/samples/sample57.vb)]

<span data-ttu-id="3d472-436">這個方法會傳回對應的伺服器上的實體路徑，以指定的虛擬路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-436">This method returns the physical path on the server that corresponds to a specified virtual path.</span></span> <span data-ttu-id="3d472-437">方法的三個參數都`virtualPath`， `baseVirtualDir`，和`allowCrossAppMapping`。</span><span class="sxs-lookup"><span data-stu-id="3d472-437">The three parameters for the method are `virtualPath`, `baseVirtualDir`, and `allowCrossAppMapping`.</span></span> <span data-ttu-id="3d472-438">（請注意，在宣告中，列出參數的資料類型，它們會接受的資料）。當您呼叫這個方法時，您必須提供所有的三個參數的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-438">(Notice that in the declaration, the parameters are listed with the data types of the data that they'll accept.) When you call this method, you must supply values for all three parameters.</span></span>

<span data-ttu-id="3d472-439">當您使用 Visual Basic 含有 Razor 語法時，您會有兩個選項可將參數傳遞至方法：*位置參數*或是*具名參數*。</span><span class="sxs-lookup"><span data-stu-id="3d472-439">When you're using Visual Basic with the Razor syntax, you have two options for passing parameters to a method: *positional parameters* or *named parameters*.</span></span> <span data-ttu-id="3d472-440">若要呼叫使用位置參數的方法，您會以嚴格的順序，指定在方法宣告中傳遞參數。</span><span class="sxs-lookup"><span data-stu-id="3d472-440">To call a method using positional parameters, you pass the parameters in a strict order that's specified in the method declaration.</span></span> <span data-ttu-id="3d472-441">（您將通常知道此順序，請閱讀文件的方法）。您必須遵循的順序，以及您不能略過的任何參數&#8212;如果有必要，您傳遞空字串 (`""`) 或為 null 的位置參數，您不需要的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-441">(You would typically know this order by reading documentation for the method.) You must follow the order, and you can't skip any of the parameters &#8212; if necessary, you pass an empty string (`""`) or null for a positional parameter that you don't have a value for.</span></span>

<span data-ttu-id="3d472-442">下列範例假設您有一個名為的資料夾*指令碼*在您的網站。</span><span class="sxs-lookup"><span data-stu-id="3d472-442">The following example assumes you have a folder named *scripts* on your website.</span></span> <span data-ttu-id="3d472-443">程式碼會呼叫`Request.MapPath`方法，並傳遞正確的順序的三個參數值。</span><span class="sxs-lookup"><span data-stu-id="3d472-443">The code calls the `Request.MapPath` method and passes values for the three parameters in the correct order.</span></span> <span data-ttu-id="3d472-444">然後，它會顯示產生的對應的路徑。</span><span class="sxs-lookup"><span data-stu-id="3d472-444">It then displays the resulting mapped path.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample58.vbhtml)]

<span data-ttu-id="3d472-445">當有許多方法的參數時，您可以保留您的程式碼更簡潔且更容易閱讀使用具名參數。</span><span class="sxs-lookup"><span data-stu-id="3d472-445">When there are many parameters for a method, you can keep your code cleaner and more readable by using named parameters.</span></span> <span data-ttu-id="3d472-446">若要呼叫方法，使用具名的參數，指定參數名稱，後面接著`:=`，然後提供值。</span><span class="sxs-lookup"><span data-stu-id="3d472-446">To call a method using named parameters, specify the parameter name followed by `:=` and then provide the value.</span></span> <span data-ttu-id="3d472-447">具名參數的優點是您可以在任何您想要的順序新增它們。</span><span class="sxs-lookup"><span data-stu-id="3d472-447">An advantage of named parameters is that you can add them in any order you want.</span></span> <span data-ttu-id="3d472-448">（一項缺點是，在方法呼叫並不會精簡）。</span><span class="sxs-lookup"><span data-stu-id="3d472-448">(A disadvantage is that the method call is not as compact.)</span></span>

<span data-ttu-id="3d472-449">下列範例會呼叫與上述相同的方法，但使用具名參數，以提供值：</span><span class="sxs-lookup"><span data-stu-id="3d472-449">The following example calls the same method as above, but uses named parameters to supply the values:</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample59.vbhtml)]

<span data-ttu-id="3d472-450">如您所見，參數會傳遞不同的順序。</span><span class="sxs-lookup"><span data-stu-id="3d472-450">As you can see, the parameters are passed in a different order.</span></span> <span data-ttu-id="3d472-451">不過，如果您執行先前的範例和此範例中，它們會傳回相同的值。</span><span class="sxs-lookup"><span data-stu-id="3d472-451">However, if you run the previous example and this example, they'll return the same value.</span></span>

## <a name="handling-errors"></a><span data-ttu-id="3d472-452">處理錯誤</span><span class="sxs-lookup"><span data-stu-id="3d472-452">Handling Errors</span></span>

### <a name="try-catch-statements"></a><span data-ttu-id="3d472-453">Try Catch 陳述式</span><span class="sxs-lookup"><span data-stu-id="3d472-453">Try-Catch statements</span></span>

<span data-ttu-id="3d472-454">陳述式通常必須在程式碼中可能會失敗的原因，您無法控制。</span><span class="sxs-lookup"><span data-stu-id="3d472-454">You'll often have statements in your code that might fail for reasons outside your control.</span></span> <span data-ttu-id="3d472-455">例如: </span><span class="sxs-lookup"><span data-stu-id="3d472-455">For example:</span></span>

- <span data-ttu-id="3d472-456">如果您的程式碼會嘗試開啟、 建立、 讀取或寫入檔案，可能會發生各種錯誤。</span><span class="sxs-lookup"><span data-stu-id="3d472-456">If your code tries to open, create, read, or write a file, all sorts of errors might occur.</span></span> <span data-ttu-id="3d472-457">您想要的檔案可能不存在，它可能會遭到鎖定，程式碼可能不具有權限，等等。</span><span class="sxs-lookup"><span data-stu-id="3d472-457">The file you want might not exist, it might be locked, the code might not have permissions, and so on.</span></span>
- <span data-ttu-id="3d472-458">同樣地，如果您的程式碼會嘗試更新資料庫中的記錄，可以有權限問題，可能會卸除資料庫的連接，要儲存的資料可能無效，依此類推。</span><span class="sxs-lookup"><span data-stu-id="3d472-458">Similarly, if your code tries to update records in a database, there can be permissions issues, the connection to the database might be dropped, the data to save might be invalid, and so on.</span></span>

<span data-ttu-id="3d472-459">在程式設計的詞彙中，這些情況下則稱為*例外狀況*。</span><span class="sxs-lookup"><span data-stu-id="3d472-459">In programming terms, these situations are called *exceptions*.</span></span> <span data-ttu-id="3d472-460">如果您的程式碼發生例外狀況，則會產生 （擲回） 的錯誤訊息也就是，最惱人的使用者。</span><span class="sxs-lookup"><span data-stu-id="3d472-460">If your code encounters an exception, it generates (throws) an error message that is, at best, annoying to users.</span></span>

![Razor-Img14](introducing-razor-syntax-vb/_static/image14.jpg)

<span data-ttu-id="3d472-462">在您的程式碼可能會遇到例外狀況的情況下，並以避免此類型的錯誤訊息，您可以使用`Try/Catch`陳述式。</span><span class="sxs-lookup"><span data-stu-id="3d472-462">In situations where your code might encounter exceptions, and in order to avoid error messages of this type, you can use `Try/Catch` statements.</span></span> <span data-ttu-id="3d472-463">在 `Try`陳述式中，執行您的程式碼。</span><span class="sxs-lookup"><span data-stu-id="3d472-463">In the `Try` statement, you run the code that you're checking.</span></span> <span data-ttu-id="3d472-464">在一或多個`Catch`陳述式，您可以尋找特定可能發生的錯誤 （特定類型的例外狀況）。</span><span class="sxs-lookup"><span data-stu-id="3d472-464">In one or more `Catch` statements, you can look for specific errors (specific types of exceptions) that might have occurred.</span></span> <span data-ttu-id="3d472-465">您可以包含更多`Catch`陳述式，為您要尋找的預期的錯誤。</span><span class="sxs-lookup"><span data-stu-id="3d472-465">You can include as many `Catch` statements as you need to look for errors that you're anticipating.</span></span>

> [!NOTE]
> <span data-ttu-id="3d472-466">我們建議您避免使用`Response.Redirect`方法中的`Try/Catch`陳述式，因為它會在網頁中造成例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3d472-466">We recommend that you avoid using the `Response.Redirect` method in `Try/Catch` statements, because it can cause an exception in your page.</span></span>


<span data-ttu-id="3d472-467">下列範例顯示建立第一個要求上的文字檔案，然後顯示按鈕，讓使用者可以開啟檔案的頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-467">The following example shows a page that creates a text file on the first request and then displays a button that lets the user open the file.</span></span> <span data-ttu-id="3d472-468">此範例刻意使用不正確的檔案名稱，如此會導致例外狀況。</span><span class="sxs-lookup"><span data-stu-id="3d472-468">The example deliberately uses a bad file name so that it will cause an exception.</span></span> <span data-ttu-id="3d472-469">程式碼包含`Catch`陳述式中的兩個可能的例外狀況： `FileNotFoundException`，就會出現錯誤，檔案名稱是否與`DirectoryNotFoundException`，發生於 ASP.NET 即使找不到資料夾。</span><span class="sxs-lookup"><span data-stu-id="3d472-469">The code includes `Catch` statements for two possible exceptions: `FileNotFoundException`, which occurs if the file name is bad, and `DirectoryNotFoundException`, which occurs if ASP.NET can't even find the folder.</span></span> <span data-ttu-id="3d472-470">（您可以取消此範例中的陳述式以查看一切正常運作時，它的執行方式註解）。</span><span class="sxs-lookup"><span data-stu-id="3d472-470">(You can uncomment a statement in the example in order to see how it runs when everything works properly.)</span></span>

<span data-ttu-id="3d472-471">如果您的程式碼未處理例外狀況，您會看到先前的螢幕擷取畫面類似的錯誤頁面。</span><span class="sxs-lookup"><span data-stu-id="3d472-471">If your code didn't handle the exception, you would see an error page like the previous screen shot.</span></span> <span data-ttu-id="3d472-472">不過，`Try/Catch`一節可協助避免使用者看到這些錯誤類型。</span><span class="sxs-lookup"><span data-stu-id="3d472-472">However, the `Try/Catch` section helps prevent the user from seeing these types of errors.</span></span>

[!code-vbhtml[Main](introducing-razor-syntax-vb/samples/sample60.vbhtml)]

## <a name="additional-resources"></a><span data-ttu-id="3d472-473">其他資源</span><span class="sxs-lookup"><span data-stu-id="3d472-473">Additional Resources</span></span>

### <a name="reference-documentation"></a><span data-ttu-id="3d472-474">參考文件</span><span class="sxs-lookup"><span data-stu-id="3d472-474">Reference Documentation</span></span>

- [<span data-ttu-id="3d472-475">ASP.NET</span><span class="sxs-lookup"><span data-stu-id="3d472-475">ASP.NET</span></span>](https://msdn.microsoft.com/library/ee532866.aspx)
- [<span data-ttu-id="3d472-476">Visual Basic 語言</span><span class="sxs-lookup"><span data-stu-id="3d472-476">Visual Basic Language</span></span>](https://msdn.microsoft.com/library/2x7h1hfk.aspx)
