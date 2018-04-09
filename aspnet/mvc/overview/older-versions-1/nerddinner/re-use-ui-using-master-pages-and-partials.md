---
uid: mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
title: 重複使用 UI 使用主版頁面和 Partials |Microsoft 文件
author: microsoft
description: 步驟 7 會查看我們排除程式碼重複，使用部分檢視的範本和主版頁面的檢視範本內的方式，我們可以使用最低 ' 乾 '。
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: d4243a4a-e91c-4116-9ae0-5c08e5285677
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/re-use-ui-using-master-pages-and-partials
msc.type: authoredcontent
ms.openlocfilehash: ade655f3a4a429360b678d02fb564ac9cf255d42
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
---
<a name="re-use-ui-using-master-pages-and-partials"></a><span data-ttu-id="8af4f-103">重複使用使用主版頁面和 Partials UI</span><span class="sxs-lookup"><span data-stu-id="8af4f-103">Re-use UI Using Master Pages and Partials</span></span>
====================
<span data-ttu-id="8af4f-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="8af4f-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="8af4f-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="8af4f-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="8af4f-106">這是一套免費的步驟 7 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="8af4f-106">This is step 7 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="8af4f-107">步驟 7 會查看我們排除程式碼重複，使用部分檢視的範本和主版頁面的檢視範本內的方式，我們可以使用最低"乾"。</span><span class="sxs-lookup"><span data-stu-id="8af4f-107">Step 7 looks at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication, using partial view templates and master pages.</span></span>
> 
> <span data-ttu-id="8af4f-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="8af4f-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-7-partials-and-master-pages"></a><span data-ttu-id="8af4f-109">NerdDinner 步驟 7: Partials 和主版頁面</span><span class="sxs-lookup"><span data-stu-id="8af4f-109">NerdDinner Step 7: Partials and Master Pages</span></span>

<span data-ttu-id="8af4f-110">ASP.NET MVC 滿足設計原理的其中一個是 「 沒有不重複自行 」 原則 （通常稱為 「 乾"）。</span><span class="sxs-lookup"><span data-stu-id="8af4f-110">One of the design philosophies ASP.NET MVC embraces is the "Do Not Repeat Yourself" principle (commonly referred to as "DRY").</span></span> <span data-ttu-id="8af4f-111">乾設計可協助您排除重複的程式碼和邏輯，最終讓應用程式更快、 更容易維護。</span><span class="sxs-lookup"><span data-stu-id="8af4f-111">A DRY design helps eliminate the duplication of code and logic, which ultimately makes applications faster to build and easier to maintain.</span></span>

<span data-ttu-id="8af4f-112">我們已經看過乾幾個我們 NerdDinner 案例中套用的原則。</span><span class="sxs-lookup"><span data-stu-id="8af4f-112">We've already seen the DRY principle applied in several of our NerdDinner scenarios.</span></span> <span data-ttu-id="8af4f-113">一些範例： 我們驗證邏輯中我們讓它能夠跨兩個編輯強制執行，並建立案例，我們控制器; 中的模型層實作我們重新跨編輯、 詳細資料和 Delete 動作方法; 使用"NotFound"檢視範本我們使用慣例的命名模式我們檢視範本，就不需要明確指定名稱，當我們呼叫 View() helper 方法。我們要重複使用這兩個編輯 DinnerFormViewModel 類別，並建立動作的案例。</span><span class="sxs-lookup"><span data-stu-id="8af4f-113">A few examples: our validation logic is implemented within our model layer, which enables it to be enforced across both edit and create scenarios in our controller; we are re-using the "NotFound" view template across the Edit, Details and Delete action methods; we are using a convention- naming pattern with our view templates, which eliminates the need to explicitly specify the name when we call the View() helper method; and we are re-using the DinnerFormViewModel class for both Edit and Create action scenarios.</span></span>

<span data-ttu-id="8af4f-114">我們現在看我們可以套用 「 乾原則 」 的方式排除程式碼重複那里以及我們檢視範本中。</span><span class="sxs-lookup"><span data-stu-id="8af4f-114">Let's now look at ways we can apply the "DRY Principle" within our view templates to eliminate code duplication there as well.</span></span>

### <a name="re-visiting-our-edit-and-create-view-templates"></a><span data-ttu-id="8af4f-115">重新瀏覽我們的編輯和建立檢視範本</span><span class="sxs-lookup"><span data-stu-id="8af4f-115">Re-visiting our Edit and Create View Templates</span></span>

<span data-ttu-id="8af4f-116">目前我們使用兩個不同的檢視範本 –"Edit.aspx"和"Create.aspx"– 來顯示 Dinner 表單 UI。</span><span class="sxs-lookup"><span data-stu-id="8af4f-116">Currently we are using two different view templates – "Edit.aspx" and "Create.aspx" – to display our Dinner form UI.</span></span> <span data-ttu-id="8af4f-117">其中的快速視覺比較反白顯示它們是類似。</span><span class="sxs-lookup"><span data-stu-id="8af4f-117">A quick visual comparison of them highlights how similar they are.</span></span> <span data-ttu-id="8af4f-118">以下是建立表單的外觀：</span><span class="sxs-lookup"><span data-stu-id="8af4f-118">Below is what the create form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image1.png)

<span data-ttu-id="8af4f-119">以下是我們的 [編輯] 表單的外觀：</span><span class="sxs-lookup"><span data-stu-id="8af4f-119">And here is what our "Edit" form looks like:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image2.png)

<span data-ttu-id="8af4f-120">不是多差異是否有？</span><span class="sxs-lookup"><span data-stu-id="8af4f-120">Not much of a difference is there?</span></span> <span data-ttu-id="8af4f-121">標題、 標頭文字以外表單版面配置和輸入控制項都相同。</span><span class="sxs-lookup"><span data-stu-id="8af4f-121">Other than the title and header text, the form layout and input controls are identical.</span></span>

<span data-ttu-id="8af4f-122">如果我們開啟 「 Edit.aspx"和"Create.aspx 」 檢視範本，我們會發現它們包含相同的表單版面配置和輸入控制項的程式碼。</span><span class="sxs-lookup"><span data-stu-id="8af4f-122">If we open up the "Edit.aspx" and "Create.aspx" view templates we'll find that they contain identical form layout and input control code.</span></span> <span data-ttu-id="8af4f-123">這種重複狀況表示我們最後會有進行變更，我們導入，或變更新 Dinner 屬性-不是好每當兩次。</span><span class="sxs-lookup"><span data-stu-id="8af4f-123">This duplication means we end up having to make changes twice anytime we introduce or change a new Dinner property - which is not good.</span></span>

### <a name="using-partial-view-templates"></a><span data-ttu-id="8af4f-124">使用部分檢視範本</span><span class="sxs-lookup"><span data-stu-id="8af4f-124">Using Partial View Templates</span></span>

<span data-ttu-id="8af4f-125">ASP.NET MVC 支援定義可以用來封裝頁面子部分檢視轉譯邏輯的 「 部分檢視 」 範本的能力。</span><span class="sxs-lookup"><span data-stu-id="8af4f-125">ASP.NET MVC supports the ability to define "partial view" templates that can be used to encapsulate view rendering logic for a sub-portion of a page.</span></span> <span data-ttu-id="8af4f-126">"Partials 」 提供有用的方式來定義檢視轉譯邏輯一次，然後再重新使用它在多個位置整個應用程式。</span><span class="sxs-lookup"><span data-stu-id="8af4f-126">"Partials" provide a useful way to define view rendering logic once, and then re-use it in multiple places across an application.</span></span>

<span data-ttu-id="8af4f-127">若要協助"乾總 「 我們 Edit.aspx 及 Create.aspx 檢視範本重複，我們可以建立名為"DinnerForm.ascx 」 封裝表單的版面配置和輸入的項目，同時的部分檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-127">To help "DRY-up" our Edit.aspx and Create.aspx view template duplication, we can create a partial view template named "DinnerForm.ascx" that encapsulates the form layout and input elements common to both.</span></span> <span data-ttu-id="8af4f-128">將執行此作業，我們/檢視表/Dinners 目錄上按一下滑鼠右鍵，然後選擇 「 加入-&gt;檢視 」 功能表命令：</span><span class="sxs-lookup"><span data-stu-id="8af4f-128">We'll do this by right-clicking on our /Views/Dinners directory and choosing the "Add-&gt;View" menu command:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image3.png)

<span data-ttu-id="8af4f-129">這會顯示 [新增檢視] 對話方塊。</span><span class="sxs-lookup"><span data-stu-id="8af4f-129">This will display the "Add View" dialog.</span></span> <span data-ttu-id="8af4f-130">我們將我們想要建立 「 DinnerForm 」，選取在對話方塊中，「 建立部分檢視 」 核取方塊，並指出，我們會將它傳遞 DinnerFormViewModel 類別的新檢視的名稱：</span><span class="sxs-lookup"><span data-stu-id="8af4f-130">We'll name the new view we want to create "DinnerForm", select the "Create a partial view" checkbox within the dialog, and indicate that we will pass it a DinnerFormViewModel class:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image4.png)

<span data-ttu-id="8af4f-131">當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄中就讓我們建立新的"DinnerForm.ascx 」 檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-131">When we click the "Add" button, Visual Studio will create a new "DinnerForm.ascx" view template for us within the "\Views\Dinners" directory.</span></span>

<span data-ttu-id="8af4f-132">我們可以再複製/貼上重複的表單配置 / 將我們新的 「 DinnerForm.ascx"部分檢視範本輸入控制項從我們 Edit.aspx/ Create.aspx 檢視範本的程式碼：</span><span class="sxs-lookup"><span data-stu-id="8af4f-132">We can then copy/paste the duplicate form layout / input control code from our Edit.aspx/ Create.aspx view templates into our new "DinnerForm.ascx" partial view template:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample1.aspx)]

<span data-ttu-id="8af4f-133">然後，我們可以更新呼叫 DinnerForm 部分範本，並排除表單重複我們編輯和建立檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-133">We can then update our Edit and Create view templates to call the DinnerForm partial template and eliminate the form duplication.</span></span> <span data-ttu-id="8af4f-134">我們這樣可以呼叫 Html.RenderPartial("DinnerForm") 我們檢視的範本內：</span><span class="sxs-lookup"><span data-stu-id="8af4f-134">We can do this by calling Html.RenderPartial("DinnerForm") within our view templates:</span></span>

##### <a name="createaspx"></a><span data-ttu-id="8af4f-135">Create.aspx</span><span class="sxs-lookup"><span data-stu-id="8af4f-135">Create.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample2.aspx)]

##### <a name="editaspx"></a><span data-ttu-id="8af4f-136">Edit.aspx</span><span class="sxs-lookup"><span data-stu-id="8af4f-136">Edit.aspx</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample3.aspx)]

<span data-ttu-id="8af4f-137">您可以明確限定您想要呼叫 Html.RenderPartial 部分範本的路徑 (例如: ~ Views/Dinners/DinnerForm.ascx")。</span><span class="sxs-lookup"><span data-stu-id="8af4f-137">You can explicitly qualify the path of the partial template you want when calling Html.RenderPartial (for example: ~Views/Dinners/DinnerForm.ascx").</span></span> <span data-ttu-id="8af4f-138">在上面的程式碼，不過，我們會運用 ASP.NET MVC 中，以慣例為基礎的命名模式，僅指定"DinnerForm"做為呈現部分的名稱。</span><span class="sxs-lookup"><span data-stu-id="8af4f-138">In our code above, though, we are taking advantage of the convention-based naming pattern within ASP.NET MVC, and just specifying "DinnerForm" as the name of the partial to render.</span></span> <span data-ttu-id="8af4f-139">當執行此作業，ASP.NET MVC 會尋找以慣例為基礎的檢視目錄中的第一個 （這會是/檢視表/Dinners DinnersController)。</span><span class="sxs-lookup"><span data-stu-id="8af4f-139">When we do this ASP.NET MVC will look first in the convention-based views directory (for DinnersController this would be /Views/Dinners).</span></span> <span data-ttu-id="8af4f-140">如果找不到部分樣板那里它會接著尋找它 /Views/Shared 目錄中。</span><span class="sxs-lookup"><span data-stu-id="8af4f-140">If it doesn't find the partial template there it will then look for it in the /Views/Shared directory.</span></span>

<span data-ttu-id="8af4f-141">Html.RenderPartial() 部分檢視的名稱呼叫時，ASP.NET MVC 會傳遞至部分檢視相同模型和別的 ViewData 字典所使用的物件呼叫檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-141">When Html.RenderPartial() is called with just the name of the partial view, ASP.NET MVC will pass to the partial view the same Model and ViewData dictionary objects used by the calling view template.</span></span> <span data-ttu-id="8af4f-142">或者，有多載可讓您將其他模型物件和 （或） 要使用的部分檢視的別的 ViewData 字典 Html.RenderPartial() 版本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-142">Alternatively, there are overloaded versions of Html.RenderPartial() that enable you to pass an alternate Model object and/or ViewData dictionary for the partial view to use.</span></span> <span data-ttu-id="8af4f-143">這是適用於您只想傳遞完整的模型/ViewModel 子集的案例。</span><span class="sxs-lookup"><span data-stu-id="8af4f-143">This is useful for scenarios where you only want to pass a subset of the full Model/ViewModel.</span></span>

| <span data-ttu-id="8af4f-144">**側邊主題內容： 為什麼&lt;%%&gt;而不是&lt;%= %&gt;嗎？**</span><span class="sxs-lookup"><span data-stu-id="8af4f-144">**Side Topic: Why &lt;% %&gt; instead of &lt;%= %&gt;?**</span></span> |
| --- |
| <span data-ttu-id="8af4f-145">您可能已注意到上述程式碼難以察覺的事情之一是我們使用&lt;%%&gt;而不是封鎖&lt;%= %&gt;封鎖呼叫 Html.RenderPartial() 時。</span><span class="sxs-lookup"><span data-stu-id="8af4f-145">One of the subtle things you might have noticed with the code above is that we are using a &lt;% %&gt; block instead of a &lt;%= %&gt; block when calling Html.RenderPartial().</span></span> <span data-ttu-id="8af4f-146">&lt;%= %&gt; ASP.NET 中的區塊，表示開發人員要呈現指定的值 (例如： &lt;%="Hello"%&gt;呈現"Hello")。</span><span class="sxs-lookup"><span data-stu-id="8af4f-146">&lt;%= %&gt; blocks in ASP.NET indicate that a developer wants to render a specified value (for example: &lt;%= "Hello" %&gt; would render "Hello").</span></span> <span data-ttu-id="8af4f-147">&lt;%%&gt;區塊而是表示，開發人員想要執行程式碼，且任何轉譯輸出中的必須進行明確 (例如： &lt;%response.write("hello")&gt;。</span><span class="sxs-lookup"><span data-stu-id="8af4f-147">&lt;% %&gt; blocks instead indicate that the developer wants to execute code, and that any rendered output within them must be done explicitly (for example: &lt;% Response.Write("Hello") %&gt;.</span></span> <span data-ttu-id="8af4f-148">我們將使用的原因&lt;%%&gt;具有上述我們 Html.RenderPartial 程式碼區塊是因為 Html.RenderPartial() 方法不會傳回字串，並改為將輸出直接要呼叫的檢視表範本的內容的輸出資料流。</span><span class="sxs-lookup"><span data-stu-id="8af4f-148">The reason we are using a &lt;% %&gt; block with our Html.RenderPartial code above is because the Html.RenderPartial() method doesn't return a string, and instead outputs the content directly to the calling view template's output stream.</span></span> <span data-ttu-id="8af4f-149">它會基於效能的效率，並藉由讓它可避免需要建立暫存的 （可能會有極的大型） 字串物件。</span><span class="sxs-lookup"><span data-stu-id="8af4f-149">It does this for performance efficiency reasons, and by doing so it avoids the need to create a (potentially very large) temporary string object.</span></span> <span data-ttu-id="8af4f-150">這會減少記憶體使用量，並改善整體應用程式輸送量。</span><span class="sxs-lookup"><span data-stu-id="8af4f-150">This reduces memory usage and improves overall application throughput.</span></span> <span data-ttu-id="8af4f-151">一個常見的錯誤時使用 Html.RenderPartial() 忘記內時，呼叫的結尾加入分號&lt;%%&gt;區塊。</span><span class="sxs-lookup"><span data-stu-id="8af4f-151">One common mistake when using Html.RenderPartial() is to forget to add a semi-colon at the end of the call when it is within a &lt;% %&gt; block.</span></span> <span data-ttu-id="8af4f-152">例如，此程式碼會造成編譯器錯誤： &lt;%html.renderpartial("dinnerform")&gt;相反地，您需要撰寫： &lt;%html.renderpartial("dinnerform"); %&gt;這是因為&lt;%%&gt;區塊是獨立的程式碼陳述式，並使用 C# 程式碼必須以分號結束陳述式時。</span><span class="sxs-lookup"><span data-stu-id="8af4f-152">For example, this code will cause a compiler error: &lt;% Html.RenderPartial("DinnerForm") %&gt; You instead need to write: &lt;% Html.RenderPartial("DinnerForm"); %&gt; This is because &lt;% %&gt; blocks are self-contained code statements, and when using C# code statements need to be terminated with a semi-colon.</span></span> |

### <a name="using-partial-view-templates-to-clarify-code"></a><span data-ttu-id="8af4f-153">若要釐清程式碼使用的部分檢視範本</span><span class="sxs-lookup"><span data-stu-id="8af4f-153">Using Partial View Templates to Clarify Code</span></span>

<span data-ttu-id="8af4f-154">我們建立 「 DinnerForm"部分檢視範本，以避免重複的多個位置中的檢視轉譯邏輯。</span><span class="sxs-lookup"><span data-stu-id="8af4f-154">We created the "DinnerForm" partial view template to avoid duplicating view rendering logic in multiple places.</span></span> <span data-ttu-id="8af4f-155">這是要建立部分檢視範本的最常見原因。</span><span class="sxs-lookup"><span data-stu-id="8af4f-155">This is the most common reason to create partial view templates.</span></span>

<span data-ttu-id="8af4f-156">有時仍合理建立部分檢視，即使它們只會呼叫在單一位置。</span><span class="sxs-lookup"><span data-stu-id="8af4f-156">Sometimes it still makes sense to create partial views even when they are only being called in a single place.</span></span> <span data-ttu-id="8af4f-157">非常複雜的檢視範本很容易就會更容易閱讀時擷取和分割成一個或多也名為樣板部分其檢視呈現邏輯。</span><span class="sxs-lookup"><span data-stu-id="8af4f-157">Very complicated view templates can often become much easier to read when their view rendering logic is extracted and partitioned into one or more well named partial templates.</span></span>

<span data-ttu-id="8af4f-158">例如，請考慮下列程式碼片段從 Site.master 檔我們專案中 （我們將會尋找在短時間內）。</span><span class="sxs-lookup"><span data-stu-id="8af4f-158">For example, consider the below code-snippet from the Site.master file in our project (which we will be looking at shortly).</span></span> <span data-ttu-id="8af4f-159">程式碼是相當直截了當讀取 – 這是因為頂端] 連結以顯示 [登入/登出邏輯螢幕右邊封裝在 「 LogOnUserControl"部分：</span><span class="sxs-lookup"><span data-stu-id="8af4f-159">The code is relatively straight-forward to read – partly because the logic to display a login/logout link at the top right of the screen is encapsulated within the "LogOnUserControl" partial:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample4.aspx)]

<span data-ttu-id="8af4f-160">每當您發現自己混淆嘗試了解檢視範本內的 html/程式碼標記，請考慮是否也不是更清楚，如果某些從中擷取及重構為妥善命名的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="8af4f-160">Whenever you find yourself getting confused trying to understand the html/code markup within a view-template, consider whether it wouldn't be clearer if some of it was extracted and refactored into well-named partial views.</span></span>

### <a name="master-pages"></a><span data-ttu-id="8af4f-161">主版頁面</span><span class="sxs-lookup"><span data-stu-id="8af4f-161">Master Pages</span></span>

<span data-ttu-id="8af4f-162">除了支援部分檢視，ASP.NET MVC 也支援建立可用來定義一般版面配置和站台的最上層 html 的"master page"範本的能力。</span><span class="sxs-lookup"><span data-stu-id="8af4f-162">In addition to supporting partial views, ASP.NET MVC also supports the ability to create "master page" templates that can be used to define the common layout and top-level html of a site.</span></span> <span data-ttu-id="8af4f-163">內容的預留位置然後可以控制項加入至主版頁面會識別出可取代可以覆寫或 「 填入 」 檢視的區域。</span><span class="sxs-lookup"><span data-stu-id="8af4f-163">Content placeholder controls can then be added to the master page to identify replaceable regions that can be overridden or "filled in" by views.</span></span> <span data-ttu-id="8af4f-164">這提供非常有效率 （且乾） 方法，將整個應用程式套用一般的版面配置。</span><span class="sxs-lookup"><span data-stu-id="8af4f-164">This provides a very effective (and DRY) way to apply a common layout across an application.</span></span>

<span data-ttu-id="8af4f-165">根據預設，新的 ASP.NET MVC 專案已自動加入至它們的主版頁面範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-165">By default, new ASP.NET MVC projects have a master page template automatically added to them.</span></span> <span data-ttu-id="8af4f-166">「 Site.master"和生活 \Views\Shared\ 資料夾內名為此主版頁面：</span><span class="sxs-lookup"><span data-stu-id="8af4f-166">This master page is named "Site.master" and lives within the \Views\Shared\ folder:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image5.png)

<span data-ttu-id="8af4f-167">預設 Site.master 檔看起來像下面。</span><span class="sxs-lookup"><span data-stu-id="8af4f-167">The default Site.master file looks like below.</span></span> <span data-ttu-id="8af4f-168">它會定義外部站台，以及在頂端導覽功能表的 html。</span><span class="sxs-lookup"><span data-stu-id="8af4f-168">It defines the outer html of the site, along with a menu for navigation at the top.</span></span> <span data-ttu-id="8af4f-169">它包含兩個可取代內容預留位置控制項 – 個標題，而另一個則用於主頁面的內容應該取代其中一個：</span><span class="sxs-lookup"><span data-stu-id="8af4f-169">It contains two replaceable content placeholder controls – one for the title, and the other for where the primary content of a page should be replaced:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample5.aspx)]

<span data-ttu-id="8af4f-170">我們已為我們 NerdDinner 的應用程式 （"List"、 詳細資料 」、 「 編輯 」、 「 建立 」、"NotFound"等等） 建立的檢視範本的所有已根據此 Site.master 範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-170">All of the view templates we've created for our NerdDinner application ("List", "Details", "Edit", "Create", "NotFound", etc) have been based on this Site.master template.</span></span> <span data-ttu-id="8af4f-171">這表示透過 「 MasterPageFile"屬性，依預設加入至頂端&lt;%@ 頁面 %&gt;當我們建立我們使用 [新增檢視] 對話方塊的檢視時的指示詞：</span><span class="sxs-lookup"><span data-stu-id="8af4f-171">This is indicated via the "MasterPageFile" attribute that was added by default to the top &lt;% @ Page %&gt; directive when we created our views using the "Add View" dialog:</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample6.aspx)]

<span data-ttu-id="8af4f-172">這表示是，我們可以變更 Site.master 內容，而且有所做的變更會自動套用與我們轉譯任何我們檢視的範本時使用。</span><span class="sxs-lookup"><span data-stu-id="8af4f-172">What this means is that we can change the Site.master content, and have the changes automatically be applied and used when we render any of our view templates.</span></span>

<span data-ttu-id="8af4f-173">讓我們來更新我們 Site.master 標頭區段，讓我們的應用程式的標頭是"NerdDinner"，而不是 「 我的 MVC 應用程式 」。</span><span class="sxs-lookup"><span data-stu-id="8af4f-173">Let's update our Site.master's header section so that the header of our application is "NerdDinner" instead of "My MVC Application".</span></span> <span data-ttu-id="8af4f-174">我們也更新我們的導覽功能表上，因此第一個索引標籤 」 尋找 Dinner"（由 HomeController index 動作方法處理），且讓我們加入新的索引標籤，稱為 「 主機 Dinner"（由 DinnersController create （） 動作方法處理）：</span><span class="sxs-lookup"><span data-stu-id="8af4f-174">Let's also update our navigation menu so that the first tab is "Find a Dinner" (handled by the HomeController's Index() action method), and let's add a new tab called "Host a Dinner" (handled by the DinnersController's Create() action method):</span></span>

[!code-aspx[Main](re-use-ui-using-master-pages-and-partials/samples/sample7.aspx)]

<span data-ttu-id="8af4f-175">當我們儲存 Site.master 檔並重新整理瀏覽器我們會看到我們標頭變更顯示跨應用程式中的所有檢視。</span><span class="sxs-lookup"><span data-stu-id="8af4f-175">When we save the Site.master file and refresh our browser we'll see our header changes show up across all views within our application.</span></span> <span data-ttu-id="8af4f-176">例如: </span><span class="sxs-lookup"><span data-stu-id="8af4f-176">For example:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image6.png)

<span data-ttu-id="8af4f-177">與*/Dinners/編輯 / [id]* URL:</span><span class="sxs-lookup"><span data-stu-id="8af4f-177">And with the */Dinners/Edit/[id]* URL:</span></span>

![](re-use-ui-using-master-pages-and-partials/_static/image7.png)

### <a name="next-step"></a><span data-ttu-id="8af4f-178">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="8af4f-178">Next Step</span></span>

<span data-ttu-id="8af4f-179">Partials 和主版頁面提供富彈性，可讓您完全組織檢視的選項。</span><span class="sxs-lookup"><span data-stu-id="8af4f-179">Partials and master pages provide very flexible options that enable you to cleanly organize views.</span></span> <span data-ttu-id="8af4f-180">您可以找到還可協助您避免重複的檢視內容 / 程式碼，並讓您更容易閱讀和維護檢視範本。</span><span class="sxs-lookup"><span data-stu-id="8af4f-180">You'll find that they help you avoid duplicating view content/ code, and make your view templates easier to read and maintain.</span></span>

<span data-ttu-id="8af4f-181">我們現在再次瀏覽我們稍早建立的清單案例，並啟用可擴充的分頁支援。</span><span class="sxs-lookup"><span data-stu-id="8af4f-181">Let's now revisit the listing scenario we built earlier and enable scalable paging support.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="8af4f-182">[上一頁](use-viewdata-and-implement-viewmodel-classes.md)
> [下一頁](implement-efficient-data-paging.md)</span><span class="sxs-lookup"><span data-stu-id="8af4f-182">[Previous](use-viewdata-and-implement-viewmodel-classes.md)
[Next](implement-efficient-data-paging.md)</span></span>
