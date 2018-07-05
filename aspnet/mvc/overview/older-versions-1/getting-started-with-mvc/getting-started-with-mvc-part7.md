---
uid: mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
title: 將驗證加入至模型 |Microsoft Docs
author: shanselman
description: 這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。 建立簡單的 web 應用程式，從資料庫讀取與寫入。
ms.author: aspnetcontent
ms.date: 08/14/2010
ms.assetid: aa7b3e8e-e23d-49f1-b160-f99a7f2982bd
msc.legacyurl: /mvc/overview/older-versions-1/getting-started-with-mvc/getting-started-with-mvc-part7
msc.type: authoredcontent
ms.openlocfilehash: f8172b63fcaa7599f5dd733271d9ea7570e3bcb1
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37802782"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="6926f-104">將驗證加入至模型</span><span class="sxs-lookup"><span data-stu-id="6926f-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="6926f-105">藉由[Scott Hanselman](https://github.com/shanselman)</span><span class="sxs-lookup"><span data-stu-id="6926f-105">by [Scott Hanselman](https://github.com/shanselman)</span></span>

> <span data-ttu-id="6926f-106">這是初學者教學課程中，將會介紹 ASP.NET MVC 的基本概念。</span><span class="sxs-lookup"><span data-stu-id="6926f-106">This is a beginner tutorial that introduces the basics of ASP.NET MVC.</span></span> <span data-ttu-id="6926f-107">您將建立簡單 web 應用程式，從資料庫讀取與寫入。</span><span class="sxs-lookup"><span data-stu-id="6926f-107">You'll create a simple web application that reads and writes from a database.</span></span> <span data-ttu-id="6926f-108">請瀏覽[ASP.NET MVC 學習中心](../../../index.md)來尋找其他 ASP.NET MVC 教學課程和範例。</span><span class="sxs-lookup"><span data-stu-id="6926f-108">Visit the [ASP.NET MVC learning center](../../../index.md) to find other ASP.NET MVC tutorials and samples.</span></span>


<span data-ttu-id="6926f-109">在這一節我們即將實作啟用我們的應用程式內的輸入的驗證所需的支援。</span><span class="sxs-lookup"><span data-stu-id="6926f-109">In this section we are going to implement the support necessary to enable input validation within our application.</span></span> <span data-ttu-id="6926f-110">我們會確保我們的資料庫內容永遠正確，且很有幫助的錯誤訊息提供給使用者，當他們嘗試，並輸入電影資料無效。</span><span class="sxs-lookup"><span data-stu-id="6926f-110">We'll ensure that our database content is always correct, and provide helpful error messages to end users when they try and enter Movie data which is not valid.</span></span> <span data-ttu-id="6926f-111">我們開始要先將一些驗證邏輯新增至電影類別。</span><span class="sxs-lookup"><span data-stu-id="6926f-111">We'll begin by adding a little validation logic to the Movie class.</span></span>

<span data-ttu-id="6926f-112">以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。</span><span class="sxs-lookup"><span data-stu-id="6926f-112">Right click on the Model folder and select Add Class.</span></span> <span data-ttu-id="6926f-113">影片將類別的名稱。</span><span class="sxs-lookup"><span data-stu-id="6926f-113">Name your class Movie.</span></span>

<span data-ttu-id="6926f-114">當我們稍早建立的電影實體模型時，IDE 就會建立 Movie 類別。</span><span class="sxs-lookup"><span data-stu-id="6926f-114">When we created the Movie Entity Model earlier, the IDE created a Movie class.</span></span> <span data-ttu-id="6926f-115">事實上，電影類別的部分可以是一個檔案與另一個部分。</span><span class="sxs-lookup"><span data-stu-id="6926f-115">In fact, part of the Movie class can be in one file and part in another.</span></span> <span data-ttu-id="6926f-116">這就叫做部分類別。</span><span class="sxs-lookup"><span data-stu-id="6926f-116">This is called a Partial Class.</span></span> <span data-ttu-id="6926f-117">我們要將電影類別延伸從另一個檔案。</span><span class="sxs-lookup"><span data-stu-id="6926f-117">We're going to extend the Movie class from another file.</span></span>

<span data-ttu-id="6926f-118">我們將建立部分影片類別所指向的 「 朋友類別 」，與一些屬性，可讓系統驗證提示。</span><span class="sxs-lookup"><span data-stu-id="6926f-118">We'll create a partial movie class that points to a "buddy class" with some attributes that will give validation hints to the system.</span></span> <span data-ttu-id="6926f-119">我們將會標示的標題和價格為必要項目，並也堅持的價格會在特定範圍內。</span><span class="sxs-lookup"><span data-stu-id="6926f-119">We'll mark the Title and Price as Required, and also insist that the Price be within a certain range.</span></span> <span data-ttu-id="6926f-120">以滑鼠右鍵按一下 模型 資料夾，然後選取 加入類別。</span><span class="sxs-lookup"><span data-stu-id="6926f-120">Right click the Models folder and select Add Class.</span></span> <span data-ttu-id="6926f-121">影片將類別的名稱，然後按一下 [確定] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="6926f-121">Name your class Movie and Click the OK button.</span></span> <span data-ttu-id="6926f-122">以下是我們部分的電影類別看起來像什麼。</span><span class="sxs-lookup"><span data-stu-id="6926f-122">Here's what our partial Movie class looks like.</span></span>

[!code-csharp[Main](getting-started-with-mvc-part7/samples/sample1.cs)]

<span data-ttu-id="6926f-123">重新執行您的應用程式，然後再次嘗試輸入影片價格超過 100。</span><span class="sxs-lookup"><span data-stu-id="6926f-123">Re-Run your application and try to enter a movie with a price over 100.</span></span> <span data-ttu-id="6926f-124">在提交表單之後，您會收到錯誤。</span><span class="sxs-lookup"><span data-stu-id="6926f-124">You'll get an error after you've submitted the form.</span></span> <span data-ttu-id="6926f-125">錯誤會攔截在伺服器端，並在張貼表單之後，就會發生。</span><span class="sxs-lookup"><span data-stu-id="6926f-125">The error is caught on the server side and occurs after the Form is POSTed.</span></span> <span data-ttu-id="6926f-126">請注意如何 ASP.NET MVC 的內建 HTML 協助程式是聰明，足以顯示錯誤訊息，並為我們保留值，在文字方塊中的項目內：</span><span class="sxs-lookup"><span data-stu-id="6926f-126">Notice how ASP.NET MVC's built-in HTML helpers were smart enough to display the error message and maintain the values for us within the textbox elements:</span></span>

<span data-ttu-id="6926f-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="6926f-127">[![CreateMovieWithValidation](getting-started-with-mvc-part7/_static/image2.png)](getting-started-with-mvc-part7/_static/image1.png)</span></span>

<span data-ttu-id="6926f-128">這非常適合，但如果我們還可以告訴使用者用戶端上立即執行，才能取得有關伺服器應該會不錯。</span><span class="sxs-lookup"><span data-stu-id="6926f-128">This works great, but it'd be nice if we could tell the user on the client-side, immediately, before the server gets involved.</span></span>

<span data-ttu-id="6926f-129">我們就讓有些使用 JavaScript 的用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="6926f-129">Let's enable some client-side validation with JavaScript.</span></span>

## <a name="adding-client-side-validation"></a><span data-ttu-id="6926f-130">新增用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="6926f-130">Adding Client-Side Validation</span></span>

<span data-ttu-id="6926f-131">因為我們的影片類別已經有一些驗證屬性，我們將只需要 Create.aspx 檢視範本中加入幾個 JavaScript 檔案，並新增一行程式碼，以便進行用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="6926f-131">Since our Movie class already has some validation attributes, we'll just need to add a few JavaScript files to our Create.aspx View template and add a line of code to enable client-side validation to take place.</span></span>

<span data-ttu-id="6926f-132">從內 VWD 移的電影檢視/資料夾，然後開啟 Create.aspx。</span><span class="sxs-lookup"><span data-stu-id="6926f-132">From within VWD go our Views/Movie folder and open up Create.aspx.</span></span>

<span data-ttu-id="6926f-133">指令碼 資料夾，在 方案總管 中開啟，並將下列三個指令碼內&lt;head&gt;標記。</span><span class="sxs-lookup"><span data-stu-id="6926f-133">Open up the Scripts folder in the Solution Explorer and drag the following three scripts to within the &lt;head&gt; tag.</span></span>

- <span data-ttu-id="6926f-134">MicrosoftAjax.js</span><span class="sxs-lookup"><span data-stu-id="6926f-134">MicrosoftAjax.js</span></span>
- <span data-ttu-id="6926f-135">MicrosoftMvcValidation.js</span><span class="sxs-lookup"><span data-stu-id="6926f-135">MicrosoftMvcValidation.js</span></span>

<span data-ttu-id="6926f-136">您想要以此順序顯示這些指令碼檔案。</span><span class="sxs-lookup"><span data-stu-id="6926f-136">You want these script files to appear in this order.</span></span>

[!code-html[Main](getting-started-with-mvc-part7/samples/sample2.html)]

<span data-ttu-id="6926f-137">此外，新增上述 Html.BeginForm 這一行：</span><span class="sxs-lookup"><span data-stu-id="6926f-137">Also, add this single line above the Html.BeginForm:</span></span>

[!code-aspx[Main](getting-started-with-mvc-part7/samples/sample3.aspx)]

<span data-ttu-id="6926f-138">以下是顯示在 IDE 中的程式碼。</span><span class="sxs-lookup"><span data-stu-id="6926f-138">Here's the code shown within the IDE.</span></span>

<span data-ttu-id="6926f-139">[![影片-Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="6926f-139">[![Movies - Microsoft Visual Web Developer 2010 Express (10)](getting-started-with-mvc-part7/_static/image4.png)](getting-started-with-mvc-part7/_static/image3.png)</span></span>

<span data-ttu-id="6926f-140">執行您的應用程式和瀏覽 /Movies/Create 同樣地，然後按一下建立不需要輸入任何資料。</span><span class="sxs-lookup"><span data-stu-id="6926f-140">Run your application and visit /Movies/Create again, and click Create without entering any data.</span></span> <span data-ttu-id="6926f-141">錯誤訊息會立即顯示沒有 [flash 我們相關聯傳送資料] 頁面上，一路回溯到伺服器。</span><span class="sxs-lookup"><span data-stu-id="6926f-141">The error messages appear immediately without the page flash that we associate with sending data all the way back to the server.</span></span> <span data-ttu-id="6926f-142">這是因為 ASP.NET MVC 現在驗證的輸入上同時 （使用 JavaScript） 的用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="6926f-142">This is because ASP.NET MVC is now validating the input on both the client (using JavaScript) and on the server.</span></span>

<span data-ttu-id="6926f-143">[![建立-Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="6926f-143">[![Create - Windows Internet Explorer](getting-started-with-mvc-part7/_static/image6.png)](getting-started-with-mvc-part7/_static/image5.png)</span></span>

<span data-ttu-id="6926f-144">這狀況良好 ！</span><span class="sxs-lookup"><span data-stu-id="6926f-144">This is looking good!</span></span> <span data-ttu-id="6926f-145">讓我們現在新增一個額外的資料行的資料庫。</span><span class="sxs-lookup"><span data-stu-id="6926f-145">Let's now add one additional column to the database.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="6926f-146">[上一頁](getting-started-with-mvc-part6.md)
> [下一頁](getting-started-with-mvc-part8.md)</span><span class="sxs-lookup"><span data-stu-id="6926f-146">[Previous](getting-started-with-mvc-part6.md)
[Next](getting-started-with-mvc-part8.md)</span></span>
