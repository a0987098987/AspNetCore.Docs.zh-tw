---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: 重複使用使用主版頁面及部分的 UI |Microsoft Docs
author: microsoft
description: 步驟 7 會探討在我們的檢視範本，以排除程式碼重複，使用部分檢視範本和主版頁面的方式，我們可以套用 ' DRY 準則 '。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: f96d3f3059a442f977d4422797c872b865a55695
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37391427"
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="82baf-103">重複使用使用主版頁面及部分的 UI</span><span class="sxs-lookup"><span data-stu-id="82baf-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="82baf-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="82baf-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="82baf-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="82baf-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="82baf-106">這是一套免費的步驟 7 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，逐步解說如何建置一個小型的但在完成時，使用 ASP.NET MVC 1 的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="82baf-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="82baf-107">步驟 7 會探討在我們的檢視範本，以排除程式碼重複，使用部分檢視範本和主版頁面的方式，我們可以套用 「 DRY 準則 」。</span><span class="sxs-lookup"><span data-stu-id="82baf-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="82baf-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得開始使用 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或是[MVC Music 市集](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="82baf-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="82baf-109">NerdDinner 步驟 7： 部分和主版頁面</span><span class="sxs-lookup"><span data-stu-id="82baf-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="82baf-110">ASP.NET MVC 應用了絕佳的設計原理的其中一個是 「 執行不自行重複 」 原則 （通常稱為 「 試"）。</span><span class="sxs-lookup"><span data-stu-id="82baf-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="82baf-111">DRY 的設計可協助消除重複的程式碼和邏輯，最終讓應用程式更快速地建置和維護變得更容易。</span><span class="sxs-lookup"><span data-stu-id="82baf-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="82baf-112">我們已經看過在數個我們 NerdDinner 的案例中套用 DRY 準則。</span><span class="sxs-lookup"><span data-stu-id="82baf-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="82baf-113">一些範例： 我們的模型層，讓它能夠跨這兩個編輯強制執行，並建立控制器; 中的案例中實作我們的驗證邏輯我們要重複使用的編輯、 詳細資料和刪除動作方法中，跨"NotFound"檢視範本我們使用我們的檢視範本，就不需要明確指定名稱，當我們呼叫 View() 協助程式方法; 中的慣例-命名模式和我們會重複使用這兩個編輯 DinnerFormViewModel 類別，並建立動作的案例。</span><span class="sxs-lookup"><span data-stu-id="82baf-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="82baf-114">現在來看看我們可以套用 「 DRY 準則 」 的方式在我們的檢視範本，以及消除那里程式碼重複。</span><span class="sxs-lookup"><span data-stu-id="82baf-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="82baf-115">重新瀏覽我們的編輯和建立檢視範本</span><span class="sxs-lookup"><span data-stu-id="82baf-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="82baf-116">目前我們會使用兩個不同的檢視範本 –"Edit.aspx 」 和 「 Create.aspx"– 顯示 Dinner 表單 UI。</span><span class="sxs-lookup"><span data-stu-id="82baf-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="82baf-117">快速的視覺比較，其中反白顯示它們是類似。</span><span class="sxs-lookup"><span data-stu-id="82baf-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="82baf-118">以下是建立表單的外觀：</span><span class="sxs-lookup"><span data-stu-id="82baf-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="82baf-119">此外，以下是我們的 [編輯] 表單的外觀：</span><span class="sxs-lookup"><span data-stu-id="82baf-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="82baf-120">不是大的差別有這樣做嗎？</span><span class="sxs-lookup"><span data-stu-id="82baf-120">Not much of a difference is there?</span></span> <span data-ttu-id="82baf-121">以外的標題和標頭的文字，表單版面配置和輸入控制項都相同。</span><span class="sxs-lookup"><span data-stu-id="82baf-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="82baf-122">如果我們開啟 「 Edit.aspx"和"Create.aspx 」 檢視範本，我們會發現它們包含相同的表單版面配置和輸入控制項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="82baf-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="82baf-123">這種重複狀況，表示我們最後會有進行變更，我們導入，或變更新的 Dinner 屬性--也就是不好每當，兩次。</span><span class="sxs-lookup"><span data-stu-id="82baf-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="82baf-124">使用部分檢視範本</span><span class="sxs-lookup"><span data-stu-id="82baf-124">Using Partial View Templates</span></span>

<span data-ttu-id="82baf-125">ASP.NET MVC 支援定義可用來封裝頁面子部分檢視轉譯邏輯的 「 部分檢視 」 範本的能力。</span><span class="sxs-lookup"><span data-stu-id="82baf-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="82baf-126">「 部分 」 提供實用的方式，來定義檢視轉譯邏輯一次，然後再重新使用它在多個位置整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="82baf-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="82baf-127">為了協助 「 試啟動 」 我們 Edit.aspx 和 Create.aspx 檢視範本重複資料刪除，我們可以建立名為"DinnerForm.ascx 」 封裝的表單版面配置和輸入的項目，同時的部分檢視範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="82baf-128">我們這樣我們/檢視/Dinners 目錄上按一下滑鼠右鍵，然後選擇 「 Add-&gt;檢視 」 功能表命令：</span><span class="sxs-lookup"><span data-stu-id="82baf-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="82baf-129">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="82baf-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="82baf-130">我們將會命名為我們想要建立 「 DinnerForm"，選取 [建立部分檢視] 的核取方塊在對話方塊中，並指出，我們會將它傳遞 DinnerFormViewModel 類別的新檢視：</span><span class="sxs-lookup"><span data-stu-id="82baf-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="82baf-131">當我們按一下 [新增] 按鈕時，Visual Studio 會"\Views\Dinners 」 目錄內就讓我們建立新的 「 DinnerForm.ascx 」 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="82baf-132">我們可以接著複製/貼上重複的表單版面配置 / 我們新的 「 DinnerForm.ascx 」 部分檢視範本中所輸入的控制碼，從我們 Edit.aspx/ Create.aspx 檢視範本：</span><span class="sxs-lookup"><span data-stu-id="82baf-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="82baf-133">然後，我們可以更新我們的編輯和建立檢視範本呼叫 DinnerForm 部分範本，並排除與表單重複。</span><span class="sxs-lookup"><span data-stu-id="82baf-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="82baf-134">我們可以執行這項操作所呼叫的 Html.RenderPartial("DinnerForm") 我們檢視的範本內：</span><span class="sxs-lookup"><span data-stu-id="82baf-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="82baf-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="82baf-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="82baf-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="82baf-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="82baf-137">您可以明確地限定時呼叫 Html.RenderPartial 所要的部分範本的路徑 (例如: ~ Views/Dinners/DinnerForm.ascx")。</span><span class="sxs-lookup"><span data-stu-id="82baf-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="82baf-138">在上述程式碼，不過，我們利用 ASP.NET MVC 中，以慣例為基礎的命名模式並且僅做為呈現部分的名稱中指定 」 DinnerForm"。</span><span class="sxs-lookup"><span data-stu-id="82baf-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="82baf-139">當我們執行此動作 ASP.NET MVC 會尋找以慣例為基礎的檢視目錄中的第一個 （這會是/檢視/Dinners DinnersController)。</span><span class="sxs-lookup"><span data-stu-id="82baf-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="82baf-140">如果找不到部分範本那里它會接著尋找它 /Views/Shared 目錄中。</span><span class="sxs-lookup"><span data-stu-id="82baf-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="82baf-141">Html.RenderPartial() 呼叫時與部分檢視的名稱，ASP.NET MVC 會將傳遞給部分檢視相同模型和 ViewData 字典所使用的物件呼叫的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="82baf-142">或者，有多載可讓您傳遞的替代模型物件和/或要使用的部分檢視的 ViewData 字典 Html.RenderPartial() 版本。</span><span class="sxs-lookup"><span data-stu-id="82baf-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="82baf-143">這非常有用的情況下，您只想將完整的模型/ViewModel 的子集。</span><span class="sxs-lookup"><span data-stu-id="82baf-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="82baf-144">**側邊的主題內容： 為什麼&lt;%%&gt;而不是&lt;%= %&gt;嗎？**</span><span class="sxs-lookup"><span data-stu-id="82baf-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="82baf-145">您可能已經注意到上述程式碼難以察覺的事情之一是，我們會使用&lt;%%&gt;而不是封鎖&lt;%= %&gt;封鎖呼叫 Html.RenderPartial() 時。</span><span class="sxs-lookup"><span data-stu-id="82baf-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="82baf-146">&lt;%= %&gt;在 ASP.NET 中的區塊表示開發人員想要呈現指定的值 (例如： &lt;%="Hello"%&gt;會轉譯"Hello")。</span><span class="sxs-lookup"><span data-stu-id="82baf-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="82baf-147">&lt;%%&gt;區塊，改為指出的開發人員想要執行程式碼，而任何呈現其中的輸出必須明確 (例如： &lt;%response.write("hello")&gt;。</span><span class="sxs-lookup"><span data-stu-id="82baf-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="82baf-148">我們將使用的原因&lt;%%&gt;上述 Html.RenderPartial 程式碼區塊是因為 Html.RenderPartial() 方法不會傳回字串，並改為將輸出直接向呼叫端檢視範本內容的輸出資料流。</span><span class="sxs-lookup"><span data-stu-id="82baf-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="82baf-149">它會基於效能效率，並藉由讓它可避免需要建立一個 （可能會有極的大型） 的暫存字串物件。</span><span class="sxs-lookup"><span data-stu-id="82baf-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="82baf-150">這樣可減少記憶體使用量，並改善整體的應用程式輸送量。</span><span class="sxs-lookup"><span data-stu-id="82baf-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="82baf-151">一個常見的錯誤時使用 Html.RenderPartial() 忘記內時，呼叫的結尾加入分號&lt;%%&gt;區塊。</span><span class="sxs-lookup"><span data-stu-id="82baf-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="82baf-152">比方說，這段程式碼會造成編譯器錯誤： &lt;%html.renderpartial("dinnerform")&gt;相反地，您需要撰寫： &lt;%html.renderpartial("dinnerform"); %&gt;這是因為&lt;%%&gt;區塊是獨立的程式碼陳述式，並且使用 C# 程式碼陳述式必須以分號結束時。</span><span class="sxs-lookup"><span data-stu-id="82baf-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="82baf-153">若要釐清程式碼中使用部分檢視範本</span><span class="sxs-lookup"><span data-stu-id="82baf-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="82baf-154">我們建立 「 DinnerForm 」 部分檢視範本，以避免複製多個位置中的檢視轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="82baf-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="82baf-155">這是最常見的原因，若要建立部分檢視範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="82baf-156">有時候它仍然可以合理地建立部分檢視，即使他們只會在呼叫在單一位置。</span><span class="sxs-lookup"><span data-stu-id="82baf-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="82baf-157">非常複雜的檢視範本很容易就會更容易閱讀時擷取和分割成一或多也名為樣板部分其檢視呈現邏輯。</span><span class="sxs-lookup"><span data-stu-id="82baf-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="82baf-158">例如，請考慮下列程式碼片段從 Site.master 檔案，在我們的專案 （這我們將查看短時間內）。</span><span class="sxs-lookup"><span data-stu-id="82baf-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="82baf-159">程式碼是相當簡單讀取 – 這是因為頂端的 [邏輯，以顯示登入/登出] 連結時，才在畫面右邊封裝在"LogOnUserControl 」 部分：</span><span class="sxs-lookup"><span data-stu-id="82baf-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="82baf-160">每當您發現自己知道嘗試了解檢視範本內的 html/程式碼標記，請考慮是否就不能更清楚的某些部分擷取及重構為妥善具名的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="82baf-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="82baf-161">主版頁面</span><span class="sxs-lookup"><span data-stu-id="82baf-161">Master Pages</span></span>

<span data-ttu-id="82baf-162">除了支援部分檢視，ASP.NET MVC 也支援建立可用來定義一般版面配置和站台的最上層 html 的 「 主版頁面 」 範本的能力。</span><span class="sxs-lookup"><span data-stu-id="82baf-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="82baf-163">內容控制項則可以新增至主版頁面，即可識別可取代的區域，可以覆寫或 「 填入 「 檢視的預留位置。</span><span class="sxs-lookup"><span data-stu-id="82baf-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="82baf-164">這提供非常有效 （且 DRY） 的方式，將整個應用程式的常見的版面配置。</span><span class="sxs-lookup"><span data-stu-id="82baf-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="82baf-165">根據預設，新的 ASP.NET MVC 專案會有自動新增至它們的主版頁面範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="82baf-166">「 Site.master"及生活 \Views\Shared\ 資料夾內名為此主版頁面：</span><span class="sxs-lookup"><span data-stu-id="82baf-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="82baf-167">預設 Site.master 檔如下所示。</span><span class="sxs-lookup"><span data-stu-id="82baf-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="82baf-168">它會定義外部站台，以及在頂端導覽功能表的 html。</span><span class="sxs-lookup"><span data-stu-id="82baf-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="82baf-169">它包含兩個可取代內容預留位置控制項 – 一個標題，而另一個則用於其中應該取代主頁面的內容：</span><span class="sxs-lookup"><span data-stu-id="82baf-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="82baf-170">我們建立我們 NerdDinner 的應用程式 （"List"、"Details"、 「 編輯 」、 「 建立 」、"NotFound"等等） 的檢視範本的所有已取決於此 Site.master 範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="82baf-171">這經由 「 MasterPageFile"屬性，依預設加入至頂端&lt;%@ %頁&gt;指示詞，當我們建立我們使用 [新增檢視] 對話方塊的檢視：</span><span class="sxs-lookup"><span data-stu-id="82baf-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="82baf-172">意思就是說，我們可以變更 Site.master 內容，而且有所做的變更會自動套用與我們呈現任何我們檢視範本時使用。</span><span class="sxs-lookup"><span data-stu-id="82baf-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="82baf-173">讓我們更新我們 Site.master 標頭區段，使我們的應用程式的標頭 」 NerdDinner"，而不是 「 我的 MVC 應用程式 」。</span><span class="sxs-lookup"><span data-stu-id="82baf-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="82baf-174">我們也更新我們瀏覽功能表，讓第一個索引標籤是 「 尋找 Dinner"（由 HomeController 的 index （） 動作方法），並讓我們加入新的索引標籤，稱為 「 主機 Dinner 」 （DinnersController create （） 動作方法處理）：</span><span class="sxs-lookup"><span data-stu-id="82baf-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="82baf-175">當我們儲存 Site.master 檔並重新整理瀏覽器就會看到我們的標頭變更顯現到我們的應用程式內的所有檢視。</span><span class="sxs-lookup"><span data-stu-id="82baf-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="82baf-176">例如: </span><span class="sxs-lookup"><span data-stu-id="82baf-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="82baf-177">與 */Dinners/編輯 / [id]* URL:</span><span class="sxs-lookup"><span data-stu-id="82baf-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="82baf-178">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="82baf-178">Next Step</span></span>

<span data-ttu-id="82baf-179">部分及主版頁面提供極具彈性的選項，可讓您完全組織檢視。</span><span class="sxs-lookup"><span data-stu-id="82baf-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="82baf-180">您會發現這些報告可協助您避免重複的檢視內容 / 程式碼，並讓您更容易閱讀和維護您的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="82baf-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="82baf-181">讓我們現在再次瀏覽我們稍早建立的清單案例，並啟用可調整的分頁支援。</span><span class="sxs-lookup"><span data-stu-id="82baf-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="82baf-182">[上一頁](use-viewdata-and-implement-viewmodel-classes.md)
> [下一頁](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="82baf-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>
