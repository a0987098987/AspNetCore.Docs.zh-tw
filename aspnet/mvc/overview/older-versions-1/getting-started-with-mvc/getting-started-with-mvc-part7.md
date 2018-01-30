---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: "將驗證加入至模型 |Microsoft 文件"
author: shanselman
description: "這是初學者教學課程介紹基本概念的 ASP.NET MVC。 建立簡單的 web 應用程式可讀取和寫入資料庫中。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 08/14/2010
ms.topic: article
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: 5616c3c3bc77be0a770540d04cc2ae48ba9eedff
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="fa37d-104">將驗證加入至模型</span><span class="sxs-lookup"><span data-stu-id="fa37d-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="fa37d-105">由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="fa37d-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="fa37d-106">這是初學者教學課程介紹基本概念的 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="fa37d-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="fa37d-107">您將建立簡單的 web 應用程式可讀取和寫入資料庫中。</span><span class="sxs-lookup"><span data-stu-id="fa37d-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="fa37d-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)教學課程和範例，請尋找其他 ASP.NET MVC。</span><span class="sxs-lookup"><span data-stu-id="fa37d-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="fa37d-109">本節中，我們即將實作啟用應用程式中的輸入的驗證所需的支援。</span><span class="sxs-lookup"><span data-stu-id="fa37d-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="fa37d-110">我們會確保我們的資料庫內容永遠正確，且再試一次，且輸入電影資料無效時，提供有用的錯誤訊息給使用者。</span><span class="sxs-lookup"><span data-stu-id="fa37d-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="fa37d-111">我們一開始將極小的驗證邏輯加入至電影類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="fa37d-112">模型 資料夾上按一下滑鼠右鍵並選取 加入類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="fa37d-113">命名您電影的類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-113">Name your class Movie.</span></span>

<span data-ttu-id="fa37d-114">當我們稍早建立的電影實體模型時，IDE 就會建立影片類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="fa37d-115">事實上，影片類別部分可以是一個檔案與另一個部分。</span><span class="sxs-lookup"><span data-stu-id="fa37d-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="fa37d-116">這稱為部分類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-116">This is called a Partial Class.</span></span> <span data-ttu-id="fa37d-117">我們將電影類別延伸從另一個檔案。</span><span class="sxs-lookup"><span data-stu-id="fa37d-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="fa37d-118">我們將建立部分影片類別指向 < 協同類別 >，以某些屬性，系統將讓驗證提示。</span><span class="sxs-lookup"><span data-stu-id="fa37d-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="fa37d-119">我們會標記為必要項、 價格和標題和也堅持這個價格是特定範圍內。</span><span class="sxs-lookup"><span data-stu-id="fa37d-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="fa37d-120">以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。</span><span class="sxs-lookup"><span data-stu-id="fa37d-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="fa37d-121">命名您電影的類別，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="fa37d-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="fa37d-122">以下是什麼我們部分影片類別類似。</span><span class="sxs-lookup"><span data-stu-id="fa37d-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="fa37d-123">重新執行您的應用程式，並嘗試輸入電影的價格超過 100。</span><span class="sxs-lookup"><span data-stu-id="fa37d-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="fa37d-124">您已送出表單之後，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="fa37d-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="fa37d-125">錯誤在伺服器端會遭到攔截，而表單張貼之後，就會發生。</span><span class="sxs-lookup"><span data-stu-id="fa37d-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="fa37d-126">請注意如何 ASP.NET MVC 的內建的 HTML helper 已聰明，可以顯示錯誤訊息，並為我們保留值，在文字方塊中的項目內：</span><span class="sxs-lookup"><span data-stu-id="fa37d-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="fa37d-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="fa37d-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="fa37d-128">這非常適合，但會是很好如果我們無法告訴使用者在用戶端，立即取得涉及伺服器之前。</span><span class="sxs-lookup"><span data-stu-id="fa37d-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="fa37d-129">讓我們啟用 JavaScript 的某些用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="fa37d-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="fa37d-130">加入用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="fa37d-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="fa37d-131">我們的影片類別已經有一些驗證屬性，因為我們只需要將一些 JavaScript 檔案加入至我們 Create.aspx 檢視的範本，並加入一行程式碼，以啟用用戶端驗證，才能進行。</span><span class="sxs-lookup"><span data-stu-id="fa37d-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="fa37d-132">從 VWD 移我們影片檢視/資料夾，並開啟 Create.aspx。</span><span class="sxs-lookup"><span data-stu-id="fa37d-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="fa37d-133">開啟 方案總管 中的指令碼 資料夾，並拖曳的下列三個指令碼內&lt;head&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="fa37d-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="fa37d-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="fa37d-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="fa37d-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="fa37d-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="fa37d-136">您想要依此順序顯示這些指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="fa37d-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="fa37d-137">此外，請加入 Html.BeginForm 上述這一行：</span><span class="sxs-lookup"><span data-stu-id="fa37d-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="fa37d-138">以下是顯示在 IDE 中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="fa37d-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="fa37d-139">[![影片-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="fa37d-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="fa37d-140">執行您的應用程式和造訪 /Movies/Create 一次，並按一下 建立不需要輸入任何資料。</span><span class="sxs-lookup"><span data-stu-id="fa37d-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="fa37d-141">錯誤訊息會立即出現快閃，我們將關聯傳送資料的頁面不回溯到伺服器。</span><span class="sxs-lookup"><span data-stu-id="fa37d-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="fa37d-142">這是因為 ASP.NET MVC 現在驗證的輸入上兩個 （使用 JavaScript） 用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="fa37d-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="fa37d-143">[![建立-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="fa37d-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="fa37d-144">正在尋找良好 ！</span><span class="sxs-lookup"><span data-stu-id="fa37d-144">This is looking good!</span></span> <span data-ttu-id="fa37d-145">我們現在將一個額外的資料行加入資料庫。</span><span class="sxs-lookup"><span data-stu-id="fa37d-145">Let's now add one additional column to the database.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="fa37d-146">[上一頁](getting-started-with-mvc-part6.md)
[下一頁](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="fa37d-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
