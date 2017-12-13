---
uid: mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
title: "提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援 |Microsoft 文件"
author: microsoft
description: "步驟 5 會示範如何啟用的編輯、 建立及刪除 Dinners 它也支援接受更我們 DinnersController 類別。"
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: bbb976e5-6150-4283-a374-c22fbafe29f5
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/provide-crud-create-read-update-delete-data-form-entry-support
msc.type: authoredcontent
ms.openlocfilehash: 5a314a1761527d8a2273166a743e3deac012a557
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="provide-crud-create-read-update-delete-data-form-entry-support"></a><span data-ttu-id="16ef1-103">提供 CRUD （建立、 讀取、 更新、 刪除） 資料組成項目支援</span><span class="sxs-lookup"><span data-stu-id="16ef1-103">Provide CRUD (Create, Read, Update, Delete) Data Form Entry Support</span></span>
====================
<span data-ttu-id="16ef1-104">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="16ef1-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="16ef1-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="16ef1-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="16ef1-106">這是一套免費的步驟 5 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="16ef1-106">This is step 5 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="16ef1-107">步驟 5 會示範如何啟用的編輯、 建立及刪除 Dinners 它也支援接受更我們 DinnersController 類別。</span><span class="sxs-lookup"><span data-stu-id="16ef1-107">Step 5 shows how to take our DinnersController class further by enable support for editing, creating and deleting Dinners with it as well.</span></span>
> 
> <span data-ttu-id="16ef1-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="16ef1-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-5-create-update-delete-form-scenarios"></a><span data-ttu-id="16ef1-109">NerdDinner 步驟 5： 建立、 更新、 刪除表單案例</span><span class="sxs-lookup"><span data-stu-id="16ef1-109">NerdDinner Step 5: Create, Update, Delete Form Scenarios</span></span>

<span data-ttu-id="16ef1-110">我們已導入控制器和檢視，並涵蓋如何使用它們來實作 Dinners 經驗的詳細資料清單/站台上。</span><span class="sxs-lookup"><span data-stu-id="16ef1-110">We've introduced controllers and views, and covered how to use them to implement a listing/details experience for Dinners on site.</span></span> <span data-ttu-id="16ef1-111">下一步是進一步採取 DinnersController 類別，並啟用編輯、 建立和刪除 Dinners 它也支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-111">Our next step will be to take our DinnersController class further and enable support for editing, creating and deleting Dinners with it as well.</span></span>

### <a name="urls-handled-by-dinnerscontroller"></a><span data-ttu-id="16ef1-112">Url 由 DinnersController</span><span class="sxs-lookup"><span data-stu-id="16ef1-112">URLs handled by DinnersController</span></span>

<span data-ttu-id="16ef1-113">我們先前加入動作方法實作支援兩個 Url 的 DinnersController: */Dinners*和*/Dinners/詳細資料 / [id]*。</span><span class="sxs-lookup"><span data-stu-id="16ef1-113">We previously added action methods to DinnersController that implemented support for two URLs: */Dinners* and */Dinners/Details/[id]*.</span></span>

| <span data-ttu-id="16ef1-114">**URL**</span><span class="sxs-lookup"><span data-stu-id="16ef1-114">**URL**</span></span> | <span data-ttu-id="16ef1-115">**動詞命令**</span><span class="sxs-lookup"><span data-stu-id="16ef1-115">**VERB**</span></span> | <span data-ttu-id="16ef1-116">**目的**</span><span class="sxs-lookup"><span data-stu-id="16ef1-116">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="16ef1-117">*/Dinners/*</span><span class="sxs-lookup"><span data-stu-id="16ef1-117">*/Dinners/*</span></span> | <span data-ttu-id="16ef1-118">GET</span><span class="sxs-lookup"><span data-stu-id="16ef1-118">GET</span></span> | <span data-ttu-id="16ef1-119">顯示即將 dinners HTML 清單。</span><span class="sxs-lookup"><span data-stu-id="16ef1-119">Display an HTML list of upcoming dinners.</span></span> |
| <span data-ttu-id="16ef1-120">*/Dinners/詳細資料 / [id]*</span><span class="sxs-lookup"><span data-stu-id="16ef1-120">*/Dinners/Details/[id]*</span></span> | <span data-ttu-id="16ef1-121">GET</span><span class="sxs-lookup"><span data-stu-id="16ef1-121">GET</span></span> | <span data-ttu-id="16ef1-122">顯示有關特定 dinner 詳細資料。</span><span class="sxs-lookup"><span data-stu-id="16ef1-122">Display details about a specific dinner.</span></span> |

<span data-ttu-id="16ef1-123">我們現在會將動作方法，來實作三個額外的 Url: */Dinners/編輯 / [id]、 / Dinners/建立、*和*/Dinners/刪除 / [id]*。</span><span class="sxs-lookup"><span data-stu-id="16ef1-123">We will now add action methods to implement three additional URLs: */Dinners/Edit/[id], /Dinners/Create,*and*/Dinners/Delete/[id]*.</span></span> <span data-ttu-id="16ef1-124">這些 Url 將會啟用編輯現有的 Dinners，建立新的 Dinners 和刪除 Dinners 支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-124">These URLs will enable support for editing existing Dinners, creating new Dinners, and deleting Dinners.</span></span>

<span data-ttu-id="16ef1-125">我們將會支援這些新的 Url 使用 HTTP GET 和 HTTP POST 動詞命令互動。</span><span class="sxs-lookup"><span data-stu-id="16ef1-125">We will support both HTTP GET and HTTP POST verb interactions with these new URLs.</span></span> <span data-ttu-id="16ef1-126">HTTP GET 要求到這些 Url 會顯示初始的 HTML 檢視的資料 （表單，以在"edit"的情況下 Dinner 資料填入、 空白表單在 「 建立 」 和 「 刪除 」 在刪除確認 畫面）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-126">HTTP GET requests to these URLs will display the initial HTML view of the data (a form populated with the Dinner data in the case of "edit", a blank form in the case of "create", and a delete confirmation screen in the case of "delete").</span></span> <span data-ttu-id="16ef1-127">HTTP POST 要求到這些 Url 會儲存/更新/刪除 Dinner 資料中我們 DinnerRepository （及從該處資料庫）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-127">HTTP POST requests to these URLs will save/update/delete the Dinner data in our DinnerRepository (and from there to the database).</span></span>

| <span data-ttu-id="16ef1-128">**URL**</span><span class="sxs-lookup"><span data-stu-id="16ef1-128">**URL**</span></span> | <span data-ttu-id="16ef1-129">**動詞命令**</span><span class="sxs-lookup"><span data-stu-id="16ef1-129">**VERB**</span></span> | <span data-ttu-id="16ef1-130">**目的**</span><span class="sxs-lookup"><span data-stu-id="16ef1-130">**Purpose**</span></span> |
| --- | --- | --- |
| <span data-ttu-id="16ef1-131">*/Dinners/編輯 / [id]*</span><span class="sxs-lookup"><span data-stu-id="16ef1-131">*/Dinners/Edit/[id]*</span></span> | <span data-ttu-id="16ef1-132">GET</span><span class="sxs-lookup"><span data-stu-id="16ef1-132">GET</span></span> | <span data-ttu-id="16ef1-133">顯示可編輯 Dinner 資料以填入 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="16ef1-133">Display an editable HTML form populated with Dinner data.</span></span> |
| <span data-ttu-id="16ef1-134">POST</span><span class="sxs-lookup"><span data-stu-id="16ef1-134">POST</span></span> | <span data-ttu-id="16ef1-135">儲存至資料庫的特定 Dinner 表單變更。</span><span class="sxs-lookup"><span data-stu-id="16ef1-135">Save the form changes for a particular Dinner to the database.</span></span> |
| <span data-ttu-id="16ef1-136">*/ Dinners/建立*</span><span class="sxs-lookup"><span data-stu-id="16ef1-136">*/Dinners/Create*</span></span> | <span data-ttu-id="16ef1-137">GET</span><span class="sxs-lookup"><span data-stu-id="16ef1-137">GET</span></span> | <span data-ttu-id="16ef1-138">顯示空的 HTML 表單，好讓使用者定義新 Dinners。</span><span class="sxs-lookup"><span data-stu-id="16ef1-138">Display an empty HTML form that allows users to define new Dinners.</span></span> |
| <span data-ttu-id="16ef1-139">POST</span><span class="sxs-lookup"><span data-stu-id="16ef1-139">POST</span></span> | <span data-ttu-id="16ef1-140">建立新的 Dinner，並將它儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-140">Create a new Dinner and save it in the database.</span></span> |
| <span data-ttu-id="16ef1-141">*/Dinners/刪除 / [id]*</span><span class="sxs-lookup"><span data-stu-id="16ef1-141">*/Dinners/Delete/[id]*</span></span> | <span data-ttu-id="16ef1-142">GET</span><span class="sxs-lookup"><span data-stu-id="16ef1-142">GET</span></span> | <span data-ttu-id="16ef1-143">顯示刪除確認 畫面中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-143">Display delete confirmation screen.</span></span> |
| <span data-ttu-id="16ef1-144">POST</span><span class="sxs-lookup"><span data-stu-id="16ef1-144">POST</span></span> | <span data-ttu-id="16ef1-145">從資料庫刪除指定的 dinner。</span><span class="sxs-lookup"><span data-stu-id="16ef1-145">Deletes the specified dinner from the database.</span></span> |

### <a name="edit-support"></a><span data-ttu-id="16ef1-146">編輯支援</span><span class="sxs-lookup"><span data-stu-id="16ef1-146">Edit Support</span></span>

<span data-ttu-id="16ef1-147">讓我們先實作"edit"案例。</span><span class="sxs-lookup"><span data-stu-id="16ef1-147">Let's begin by implementing the "edit" scenario.</span></span>

#### <a name="the-http-get-edit-action-method"></a><span data-ttu-id="16ef1-148">HTTP GET 編輯動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-148">The HTTP-GET Edit Action Method</span></span>

<span data-ttu-id="16ef1-149">我們會先實作我們編輯動作方法的 HTTP 「 取得 」 行為。</span><span class="sxs-lookup"><span data-stu-id="16ef1-149">We'll start by implementing the HTTP "GET" behavior of our edit action method.</span></span> <span data-ttu-id="16ef1-150">這個方法會叫用時*/Dinners/編輯 / [id]*要求 URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-150">This method will be invoked when the */Dinners/Edit/[id]* URL is requested.</span></span> <span data-ttu-id="16ef1-151">我們實作看起來像：</span><span class="sxs-lookup"><span data-stu-id="16ef1-151">Our implementation will look like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample1.cs)]

<span data-ttu-id="16ef1-152">上述程式碼會使用 DinnerRepository 擷取 Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="16ef1-152">The code above uses the DinnerRepository to retrieve a Dinner object.</span></span> <span data-ttu-id="16ef1-153">然後，它會呈現檢視的範本，使用 Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="16ef1-153">It then renders a View template using the Dinner object.</span></span> <span data-ttu-id="16ef1-154">因為我們尚未明確傳遞至範本名稱*View()* helper 方法，它將使用的慣例為基礎的預設路徑解析檢視範本： /Views/Dinners/Edit.aspx。</span><span class="sxs-lookup"><span data-stu-id="16ef1-154">Because we haven't explicitly passed a template name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Edit.aspx.</span></span>

<span data-ttu-id="16ef1-155">我們現在來建立此檢視範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-155">Let's now create this view template.</span></span> <span data-ttu-id="16ef1-156">我們會以編輯方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令來這樣做：</span><span class="sxs-lookup"><span data-stu-id="16ef1-156">We will do this by right-clicking within the Edit method and selecting the "Add View" context menu command:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image1.png)

<span data-ttu-id="16ef1-157">在 [新增檢視] 對話方塊中，我們會指出我們 Dinner 物件傳遞至為其模型中，我們檢視的範本，並選擇 [編輯] 的範本自動 scaffold:</span><span class="sxs-lookup"><span data-stu-id="16ef1-157">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to auto-scaffold an "Edit" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image2.png)

<span data-ttu-id="16ef1-158">當我們按一下 [新增] 按鈕時，Visual Studio 會將新的 「 Edit.aspx 」 檢視的範本檔之我們"\Views\Dinners"目錄中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-158">When we click the "Add" button, Visual Studio will add a new "Edit.aspx" view template file for us within the "\Views\Dinners" directory.</span></span> <span data-ttu-id="16ef1-159">它也會開啟新的"Edit.aspx 」 檢視範本內程式碼編輯器 – 填入初始 「 編輯 」 scaffold 實作類似如下：</span><span class="sxs-lookup"><span data-stu-id="16ef1-159">It will also open up the new "Edit.aspx" view template within the code-editor – populated with an initial "Edit" scaffold implementation like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image3.png)

<span data-ttu-id="16ef1-160">讓我們進行一些變更以預設 「 編輯 」 scaffold 產生，並更新 編輯檢視範本的內容下 （這會移除一些不想要公開的屬性）：</span><span class="sxs-lookup"><span data-stu-id="16ef1-160">Let's make a few changes to the default "edit" scaffold generated, and update the edit view template to have the content below (which removes a few of the properties we don't want to expose):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample2.aspx)]

<span data-ttu-id="16ef1-161">當我們執行的應用程式和要求*"/ 1/編輯 Dinners /"*我們會看到下列頁面的 URL:</span><span class="sxs-lookup"><span data-stu-id="16ef1-161">When we run the application and request the *"/Dinners/Edit/1"* URL we will see the following page:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image4.png)

<span data-ttu-id="16ef1-162">我們檢視所產生的 HTML 標記看起來像下面。</span><span class="sxs-lookup"><span data-stu-id="16ef1-162">The HTML markup generated by our view looks like below.</span></span> <span data-ttu-id="16ef1-163">它是標準 HTML – 具有&lt;表單&gt;項目，執行 HTTP POST 來*/Dinners/Edit/1* URL 時 儲存&lt;輸入類型 = 「 送出"/&gt;推入 按鈕。</span><span class="sxs-lookup"><span data-stu-id="16ef1-163">It is standard HTML – with a &lt;form&gt; element that performs an HTTP POST to the */Dinners/Edit/1* URL when the "Save" &lt;input type="submit"/&gt; button is pushed.</span></span> <span data-ttu-id="16ef1-164">HTML&lt;輸入類型 ="text"/&gt;項目已為每個可編輯屬性的輸出：</span><span class="sxs-lookup"><span data-stu-id="16ef1-164">A HTML &lt;input type="text"/&gt; element has been output for each editable property:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image5.png)

#### <a name="htmlbeginform-and-htmltextbox-html-helper-methods"></a><span data-ttu-id="16ef1-165">Html.BeginForm() 和 Html.TextBox() Html Helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-165">Html.BeginForm() and Html.TextBox() Html Helper Methods</span></span>

<span data-ttu-id="16ef1-166">我們 「 Edit.aspx 」 檢視的範本使用數種 「 Html Helper"方法： Html.ValidationSummary()、 Html.BeginForm()、 Html.TextBox() 和 Html.ValidationMessage()。</span><span class="sxs-lookup"><span data-stu-id="16ef1-166">Our "Edit.aspx" view template is using several "Html Helper" methods: Html.ValidationSummary(), Html.BeginForm(), Html.TextBox(), and Html.ValidationMessage().</span></span> <span data-ttu-id="16ef1-167">除了為我們產生 HTML 標記中，這些 helper 方法會提供內建的錯誤處理和驗證支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-167">In addition to generating HTML markup for us, these helper methods provide built-in error handling and validation support.</span></span>

##### <a name="htmlbeginform-helper-method"></a><span data-ttu-id="16ef1-168">Html.BeginForm() helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-168">Html.BeginForm() helper method</span></span>

<span data-ttu-id="16ef1-169">Html.BeginForm() helper 方法是什麼輸出 HTML&lt;表單&gt;我們標記中的項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-169">The Html.BeginForm() helper method is what output the HTML &lt;form&gt; element in our markup.</span></span> <span data-ttu-id="16ef1-170">我們 Edit.aspx 檢視範本中，您會發現，我們要套用以 C#"using"陳述式時使用此方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-170">In our Edit.aspx view template you'll notice that we are applying a C# "using" statement when using this method.</span></span> <span data-ttu-id="16ef1-171">左大括號表示開頭&lt;表單&gt;內容和右大括號就會指出結尾&lt;/形成&gt;項目：</span><span class="sxs-lookup"><span data-stu-id="16ef1-171">The open curly brace indicates the beginning of the &lt;form&gt; content, and the closing curly brace is what indicates the end of the &lt;/form&gt; element:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample3.cs)]

<span data-ttu-id="16ef1-172">或者，如果您找到的"using"陳述式方式比較不自然，像這樣的案例，您可以使用 Html.BeginForm() 和 Html.EndForm() 組合 （它會進行相同的工作）：</span><span class="sxs-lookup"><span data-stu-id="16ef1-172">Alternatively, if you find the "using" statement approach unnatural for a scenario like this, you can use a Html.BeginForm() and Html.EndForm() combination (which does the same thing):</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample4.aspx)]

<span data-ttu-id="16ef1-173">不含任何參數呼叫 Html.BeginForm() 會導致輸出至目前要求 URL 中沒有 HTTP POST 的表單項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-173">Calling Html.BeginForm() without any parameters will cause it to output a form element that does an HTTP-POST to the current request's URL.</span></span> <span data-ttu-id="16ef1-174">也就是為什麼我們編輯檢視產生*&lt;形成動作 ="/ 1/編輯 Dinners /"方法 ="post"&gt;* 項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-174">That is why our Edit view generates a *&lt;form action="/Dinners/Edit/1" method="post"&gt;* element.</span></span> <span data-ttu-id="16ef1-175">我們可能具有或者明確參數傳送至 Html.BeginForm() 如果我們想要張貼至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-175">We could have alternatively passed explicit parameters to Html.BeginForm() if we wanted to post to a different URL.</span></span>

##### <a name="htmltextbox-helper-method"></a><span data-ttu-id="16ef1-176">Html.TextBox() helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-176">Html.TextBox() helper method</span></span>

<span data-ttu-id="16ef1-177">我們 Edit.aspx 檢視會使用 Html.TextBox() helper 方法來輸出&lt;輸入類型 ="text"/&gt;項目：</span><span class="sxs-lookup"><span data-stu-id="16ef1-177">Our Edit.aspx view uses the Html.TextBox() helper method to output &lt;input type="text"/&gt; elements:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample5.aspx)]

<span data-ttu-id="16ef1-178">Html.TextBox() 上述方法，會採用單一參數 – 用來指定這兩個識別碼/名稱的屬性&lt;輸入類型 ="text"/&gt;輸出，以及要填入的文字方塊值的模型屬性的項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-178">The Html.TextBox() method above takes a single parameter – which is being used to specify both the id/name attributes of the &lt;input type="text"/&gt; element to output, as well as the model property to populate the textbox value from.</span></span> <span data-ttu-id="16ef1-179">比方說，我們傳遞到編輯檢視的 Dinner 物件有 「.NET 未來"，"Title"屬性值，因此我們 Html.TextBox("Title") 方法呼叫輸出： *&lt;輸入的識別碼 ="Title"name ="Title"type ="text"value =".NET 未來"/&gt;*.</span><span class="sxs-lookup"><span data-stu-id="16ef1-179">For example, the Dinner object we passed to the Edit view had a "Title" property value of ".NET Futures", and so our Html.TextBox("Title") method call output: *&lt;input id="Title" name="Title" type="text" value=".NET Futures" /&gt;*.</span></span>

<span data-ttu-id="16ef1-180">或者，我們可以使用第一個 Html.TextBox() 參數指定識別碼/名稱的項目，並明確傳遞中要做為第二個參數的值：</span><span class="sxs-lookup"><span data-stu-id="16ef1-180">Alternatively, we can use the first Html.TextBox() parameter to specify the id/name of the element, and then explicitly pass in the value to use as a second parameter:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample6.aspx)]

<span data-ttu-id="16ef1-181">通常我們會想要執行的輸出值的自訂格式。</span><span class="sxs-lookup"><span data-stu-id="16ef1-181">Often we'll want to perform custom formatting on the value that is output.</span></span> <span data-ttu-id="16ef1-182">String.format （) 內建.NET 靜態方法是適用於這些案例。</span><span class="sxs-lookup"><span data-stu-id="16ef1-182">The String.Format() static method built-into .NET is useful for these scenarios.</span></span> <span data-ttu-id="16ef1-183">我們 Edit.aspx 檢視範本使用這個來格式化 EventDate 值 （這是 DateTime 類型的），因此它不會顯示時間 （秒）：</span><span class="sxs-lookup"><span data-stu-id="16ef1-183">Our Edit.aspx view template is using this to format the EventDate value (which is of type DateTime) so that it doesn't show seconds for the time:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample7.aspx)]

<span data-ttu-id="16ef1-184">Html.TextBox() 的第三個參數可以選擇性地用於輸出其他的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-184">A third parameter to Html.TextBox() can optionally be used to output additional HTML attributes.</span></span> <span data-ttu-id="16ef1-185">下列程式碼的片段會示範如何轉譯的其他大小 ="30"的屬性和類別上 ="mycssclass"屬性&lt;輸入類型 ="text"/&gt;項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-185">The code-snippet below demonstrates how to render an additional size="30" attribute and a class="mycssclass" attribute on the &lt;input type="text"/&gt; element.</span></span> <span data-ttu-id="16ef1-186">請注意如何我們會逸出的類別屬性使用名稱"@" character because "類別 」 是保留的關鍵字在 C# 中：</span><span class="sxs-lookup"><span data-stu-id="16ef1-186">Note how we are escaping the name of the class attribute using a "@" character because "class" is a reserved keyword in C#:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample8.aspx)]

#### <a name="implementing-the-http-post-edit-action-method"></a><span data-ttu-id="16ef1-187">實作 HTTP POST 的編輯動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-187">Implementing the HTTP-POST Edit Action Method</span></span>

<span data-ttu-id="16ef1-188">我們現在有我們實作的編輯動作方法的 HTTP GET 版本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-188">We now have the HTTP-GET version of our Edit action method implemented.</span></span> <span data-ttu-id="16ef1-189">當使用者要求*/Dinners/Edit/1*他們會收到類似下列的 HTML 網頁的 URL:</span><span class="sxs-lookup"><span data-stu-id="16ef1-189">When a user requests the */Dinners/Edit/1* URL they receive an HTML page like the following:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image6.png)

<span data-ttu-id="16ef1-190">按下 [儲存] 按鈕會導致表單張貼至*/Dinners/Edit/1* URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。</span><span class="sxs-lookup"><span data-stu-id="16ef1-190">Pressing the "Save" button causes a form post to the */Dinners/Edit/1* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span> <span data-ttu-id="16ef1-191">我們現在可實作我們編輯動作方法 – 將會處理儲存 Dinner 的 HTTP POST 行為。</span><span class="sxs-lookup"><span data-stu-id="16ef1-191">Let's now implement the HTTP POST behavior of our edit action method – which will handle saving the Dinner.</span></span>

<span data-ttu-id="16ef1-192">我們一開始會將多載的 「 編輯 」 動作方法加入至我們 DinnersController 其上已有 「 AcceptVerbs"屬性，表示它會處理 HTTP POST 案例：</span><span class="sxs-lookup"><span data-stu-id="16ef1-192">We'll begin by adding an overloaded "Edit" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample9.cs)]

<span data-ttu-id="16ef1-193">當 [AcceptVerbs] 屬性套用至多載的動作方法時，ASP.NET MVC 會自動處理分派至適當的動作方法會根據傳入的 HTTP 指令動詞的要求。</span><span class="sxs-lookup"><span data-stu-id="16ef1-193">When the [AcceptVerbs] attribute is applied to overloaded action methods, ASP.NET MVC automatically handles dispatching requests to the appropriate action method depending on the incoming HTTP verb.</span></span> <span data-ttu-id="16ef1-194">HTTP POST 要求到*/Dinners/編輯 / [id]* Url 將會移至上述的編輯方法，對所有其他 HTTP 動詞命令要求時*/Dinners/編輯 / [id]*Url 將會移至第一個編輯方法我們實作 （這並未沒有 [AcceptVerbs] 屬性）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-194">HTTP POST requests to */Dinners/Edit/[id]* URLs will go to the above Edit method, while all other HTTP verb requests to */Dinners/Edit/[id]*URLs will go to the first Edit method we implemented (which did not have an [AcceptVerbs] attribute).</span></span>

| <span data-ttu-id="16ef1-195">**側邊主題內容： 原因區分透過 HTTP 指令動詞嗎？**</span><span class="sxs-lookup"><span data-stu-id="16ef1-195">**Side Topic: Why differentiate via HTTP verbs?**</span></span> |
| --- |
| <span data-ttu-id="16ef1-196">您可能會要求 – 我們為何使用單一的 URL 和區隔其行為，透過 HTTP 指令動詞嗎？</span><span class="sxs-lookup"><span data-stu-id="16ef1-196">You might ask – why are we using a single URL and differentiating its behavior via the HTTP verb?</span></span> <span data-ttu-id="16ef1-197">為什麼不只需要兩個不同的 Url，以處理載入及儲存所做的變更嗎？</span><span class="sxs-lookup"><span data-stu-id="16ef1-197">Why not just have two separate URLs to handle loading and saving edit changes?</span></span> <span data-ttu-id="16ef1-198">例如： /Dinners/編輯 / [id] 顯示的初始表單和 /Dinners/Save / [id] 以處理表單 post 要求來儲存它嗎？</span><span class="sxs-lookup"><span data-stu-id="16ef1-198">For example: /Dinners/Edit/[id] to display the initial form and /Dinners/Save/[id] to handle the form post to save it?</span></span> <span data-ttu-id="16ef1-199">發行的兩個不同 Url 的缺點是，在我們公佈到 /Dinners/Save/2，並需要重新顯示 HTML 表單，因為發生輸入錯誤的情況下，使用者將最後會有 Dinners/Save/2 URL 其瀏覽器的網址列中 （因為這是URL 表單張貼到）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-199">The downside with publishing two separate URLs is that in cases where we post to /Dinners/Save/2, and then need to redisplay the HTML form because of an input error, the end-user will end up having the /Dinners/Save/2 URL in their browser's address bar (since that was the URL the form posted to).</span></span> <span data-ttu-id="16ef1-200">如果使用者設定為書籤 redisplayed 的本頁一致瀏覽器的我的最愛清單，或複製/貼上 URL 和電子郵件寄給給朋友，它們將最後儲存會無法運作，未來 （因為該 URL 張貼值而定） 的 URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-200">If the end-user bookmarks this redisplayed page to their browser favorites list, or copy/pastes the URL and emails it to a friend, they will end up saving a URL that won't work in the future (since that URL depends on post values).</span></span> <span data-ttu-id="16ef1-201">藉由公開單一 URL (例如： /Dinners/Edit/[id]) 並處理它的 HTTP 動詞命令，並加入書籤 編輯頁面及 （或） 將 URL 傳送給其他人的使用者和安全。</span><span class="sxs-lookup"><span data-stu-id="16ef1-201">By exposing a single URL (like: /Dinners/Edit/[id]) and differentiating the processing of it by HTTP verb, it is safe for end-users to bookmark the edit page and/or send the URL to others.</span></span> |

#### <a name="retrieving-form-post-values"></a><span data-ttu-id="16ef1-202">擷取表單張貼值</span><span class="sxs-lookup"><span data-stu-id="16ef1-202">Retrieving Form Post Values</span></span>

<span data-ttu-id="16ef1-203">有多種方式可以存取公佈在我們的 HTTP POST [編輯] 方法中的表單參數。</span><span class="sxs-lookup"><span data-stu-id="16ef1-203">There are a variety of ways we can access posted form parameters within our HTTP POST "Edit" method.</span></span> <span data-ttu-id="16ef1-204">一個簡單的作法是只控制站的基底類別上使用 要求屬性來存取表單集合和直接擷取已張貼的值：</span><span class="sxs-lookup"><span data-stu-id="16ef1-204">One simple approach is to just use the Request property on the Controller base class to access the form collection and retrieve the posted values directly:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample10.cs)]

<span data-ttu-id="16ef1-205">我們加入錯誤處理邏輯之後，特別是，會，更詳細資訊，上述的方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-205">The above approach is a little verbose, though, especially once we add error handling logic.</span></span>

<span data-ttu-id="16ef1-206">A 更接近此案例中是利用內建*UpdateModel()*控制器的基底類別的 helper 方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-206">A better approach for this scenario is to leverage the built-in *UpdateModel()* helper method on the Controller base class.</span></span> <span data-ttu-id="16ef1-207">它支援更新，我們將它使用的連入的表單參數傳遞的物件的屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-207">It supports updating the properties of an object we pass it using the incoming form parameters.</span></span> <span data-ttu-id="16ef1-208">使用反映來判斷物件上的屬性名稱，然後自動轉換並指派值給變數根據用戶端所送出的輸入值。</span><span class="sxs-lookup"><span data-stu-id="16ef1-208">It uses reflection to determine the property names on the object, and then automatically converts and assigns values to them based on the input values submitted by the client.</span></span>

<span data-ttu-id="16ef1-209">我們可以使用 UpdateModel() 方法，以簡化我們 HTTP POST 編輯動作使用此程式碼：</span><span class="sxs-lookup"><span data-stu-id="16ef1-209">We could use the UpdateModel() method to simplify our HTTP-POST Edit Action using this code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample11.cs)]

<span data-ttu-id="16ef1-210">我們現在可以瀏覽*/Dinners/Edit/1* URL，然後變更我們 Dinner 標題：</span><span class="sxs-lookup"><span data-stu-id="16ef1-210">We can now visit the */Dinners/Edit/1* URL, and change the title of our Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image7.png)

<span data-ttu-id="16ef1-211">當我們按一下 [儲存] 按鈕時，我們會執行表單張貼到我們的編輯動作，並已更新的值將會保存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-211">When we click the "Save" button, we'll perform a form post to our Edit action, and the updated values will be persisted in the database.</span></span> <span data-ttu-id="16ef1-212">我們將然後被重新導向至詳細資料的 URL 為 Dinner （將會顯示最近儲存的值）：</span><span class="sxs-lookup"><span data-stu-id="16ef1-212">We will then be redirected to the Details URL for the Dinner (which will display the newly saved values):</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image8.png)

#### <a name="handling-edit-errors"></a><span data-ttu-id="16ef1-213">處理編輯錯誤</span><span class="sxs-lookup"><span data-stu-id="16ef1-213">Handling Edit Errors</span></span>

<span data-ttu-id="16ef1-214">我們目前 HTTP POST 實作可以正常運作 – 除非有錯誤。</span><span class="sxs-lookup"><span data-stu-id="16ef1-214">Our current HTTP-POST implementation works fine – except when there are errors.</span></span>

<span data-ttu-id="16ef1-215">當使用者編輯表單的錯誤，我們需要確定表單會重新顯示使用資訊的錯誤訊息，將引導他們修正此問題。</span><span class="sxs-lookup"><span data-stu-id="16ef1-215">When a user makes a mistake editing a form, we need to make sure that the form is redisplayed with an informative error message that guides them to fix it.</span></span> <span data-ttu-id="16ef1-216">這包括使用者要將張貼輸入不正確的情況下 (例如： 的格式不正確的日期字串)，就如同狀況下以及輸入的格式是否有效，但沒有商務規則違規。</span><span class="sxs-lookup"><span data-stu-id="16ef1-216">This includes cases where an end-user posts incorrect input (for example: a malformed date string), as well as cases where the input format is valid but there is a business rule violation.</span></span> <span data-ttu-id="16ef1-217">發生錯誤時表單應該保留原本輸入，讓他們不必手動將其變更重新填滿使用者輸入的資料。</span><span class="sxs-lookup"><span data-stu-id="16ef1-217">When errors occur the form should preserve the input data the user originally entered so that they don't have to refill their changes manually.</span></span> <span data-ttu-id="16ef1-218">表單已成功完成之前，應該會為視需要多次重複此程序。</span><span class="sxs-lookup"><span data-stu-id="16ef1-218">This process should repeat as many times as necessary until the form successfully completes.</span></span>

<span data-ttu-id="16ef1-219">ASP.NET MVC 包括一些不錯的內建功能可簡化錯誤處理和形式顯示。</span><span class="sxs-lookup"><span data-stu-id="16ef1-219">ASP.NET MVC includes some nice built-in features that make error handling and form redisplay easy.</span></span> <span data-ttu-id="16ef1-220">若要查看這些功能的動作，讓我們以下列程式碼更新我們的編輯動作方法：</span><span class="sxs-lookup"><span data-stu-id="16ef1-220">To see these features in action let's update our Edit action method with the following code:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample12.cs)]

<span data-ttu-id="16ef1-221">上述程式碼是類似於先前實作中 –，不同之處在於我們現在會周圍的工作中包裝 try/catch 區塊中的錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="16ef1-221">The above code is similar to our previous implementation – except that we are now wrapping a try/catch error handling block around our work.</span></span> <span data-ttu-id="16ef1-222">如果發生例外狀況呼叫 UpdateModel()，或是當我們再試一次，並儲存 DinnerRepository （這會引發例外狀況，如果我們嘗試儲存的 Dinner 物件無效因為我們的模型內的規則違規的緣故），我們 catch 錯誤處理區塊就會將執行。</span><span class="sxs-lookup"><span data-stu-id="16ef1-222">If an exception occurs either when calling UpdateModel(), or when we try and save the DinnerRepository (which will raise an exception if the Dinner object we are trying to save is invalid because of a rule violation within our model), our catch error handling block will execute.</span></span> <span data-ttu-id="16ef1-223">在其中我們迴圈 Dinner 物件存在於任何規則違規時，並加入 ModelState 物件 （我們將討論）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-223">Within it we loop over any rule violations that exist in the Dinner object and add them to a ModelState object (which we'll discuss shortly).</span></span> <span data-ttu-id="16ef1-224">然後，我們重新顯示檢視。</span><span class="sxs-lookup"><span data-stu-id="16ef1-224">We then redisplay the view.</span></span>

<span data-ttu-id="16ef1-225">若要查看此工作讓我們重新執行應用程式，請編輯 Dinner，並將它變更為有空白的標題，EventDate 的"BOGUS"，，美國國家 （地區） 值會使用英式電話號碼。</span><span class="sxs-lookup"><span data-stu-id="16ef1-225">To see this working let's re-run the application, edit a Dinner, and change it to have an empty Title, an EventDate of "BOGUS", and use a UK phone number with a country value of USA.</span></span> <span data-ttu-id="16ef1-226">當我們按下 [儲存] 按鈕時編輯的 HTTP POST 方法將無法儲存 Dinner （因為沒有錯誤），將會重新顯示表單：</span><span class="sxs-lookup"><span data-stu-id="16ef1-226">When we press the "Save" button our HTTP POST Edit method will not be able to save the Dinner (because there are errors) and will redisplay the form:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image9.png)

<span data-ttu-id="16ef1-227">我們的應用程式擁有不錯錯誤體驗。</span><span class="sxs-lookup"><span data-stu-id="16ef1-227">Our application has a decent error experience.</span></span> <span data-ttu-id="16ef1-228">具有無效的輸入的文字項目會以紅色反白顯示和其相關的使用者會顯示驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="16ef1-228">The text elements with the invalid input are highlighted in red, and validation error messages are displayed to the end user about them.</span></span> <span data-ttu-id="16ef1-229">表單也會保留原本輸入的使用者 – 輸入的資料，讓他們不必重新填滿任何項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-229">The form is also preserving the input data the user originally entered – so that they don't have to refill anything.</span></span>

<span data-ttu-id="16ef1-230">如何，您可能會問，這會發生？</span><span class="sxs-lookup"><span data-stu-id="16ef1-230">How, you might ask, did this occur?</span></span> <span data-ttu-id="16ef1-231">如何未標題、 EventDate 和 ContactPhone 文字方塊以紅色強調本身和知道輸出原本輸入的使用者值？</span><span class="sxs-lookup"><span data-stu-id="16ef1-231">How did the Title, EventDate, and ContactPhone textboxes highlight themselves in red and know to output the originally entered user values?</span></span> <span data-ttu-id="16ef1-232">以及如何未顯示錯誤訊息取得上方清單中？</span><span class="sxs-lookup"><span data-stu-id="16ef1-232">And how did error messages get displayed in the list at the top?</span></span> <span data-ttu-id="16ef1-233">好消息是，這不會發生魔術-而不是已因為我們使用的一些內建 ASP.NET MVC 功能可簡化輸入的驗證和錯誤處理案例。</span><span class="sxs-lookup"><span data-stu-id="16ef1-233">The good news is that this didn't occur by magic - rather it was because we used some of the built-in ASP.NET MVC features that make input validation and error handling scenarios easy.</span></span>

#### <a name="understanding-modelstate-and-the-validation-html-helper-methods"></a><span data-ttu-id="16ef1-234">了解 ModelState 和驗證的 HTML Helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-234">Understanding ModelState and the Validation HTML Helper Methods</span></span>

<span data-ttu-id="16ef1-235">控制器類別有 「 ModelState"屬性集合會提供一個方法來指示與正在傳遞至檢視的模型物件有錯誤。</span><span class="sxs-lookup"><span data-stu-id="16ef1-235">Controller classes have a "ModelState" property collection which provides a way to indicate that errors exist with a model object being passed to a View.</span></span> <span data-ttu-id="16ef1-236">錯誤 ModelState 集合內的項目會識別模型屬性的名稱與問題 (例如:"Title"、"EventDate，"或"ContactPhone")，並允許指定人類看得方便的錯誤訊息 (例如: 「 標題 」 需要)。</span><span class="sxs-lookup"><span data-stu-id="16ef1-236">Error entries within the ModelState collection identify the name of the model property with the issue (for example: "Title", "EventDate", or "ContactPhone"), and allow a human-friendly error message to be specified (for example: "Title is required").</span></span>

<span data-ttu-id="16ef1-237">*UpdateModel()* helper 方法會自動填入 ModelState 集合，當它嘗試將表單值指派給模型物件上的內容時遇到錯誤。</span><span class="sxs-lookup"><span data-stu-id="16ef1-237">The *UpdateModel()* helper method automatically populates the ModelState collection when it encounters errors while trying to assign form values to properties on the model object.</span></span> <span data-ttu-id="16ef1-238">例如，我們的 Dinner 物件 EventDate 屬性是 DateTime 類型。</span><span class="sxs-lookup"><span data-stu-id="16ef1-238">For example, our Dinner object's EventDate property is of type DateTime.</span></span> <span data-ttu-id="16ef1-239">無法將上述案例中，指派給它的字串值"BOGUS"UpdateModel() 方法時，UpdateModel() 方法所加入至指出作業錯誤 ModelState 集合的項目發生與屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-239">When the UpdateModel() method was unable to assign the string value "BOGUS" to it in the scenario above, the UpdateModel() method added an entry to the ModelState collection indicating an assignment error had occurred with that property.</span></span>

<span data-ttu-id="16ef1-240">開發人員也可以撰寫程式碼以明確地加入錯誤 ModelState 集合內的項目，像我們做以下我們 「 攔截 」 錯誤處理在區塊內，這 ModelState 集合填入中作用中的規則違規的項目Dinner 物件：</span><span class="sxs-lookup"><span data-stu-id="16ef1-240">Developers can also write code to explicitly add error entries into the ModelState collection like we are doing below within our "catch" error handling block, which is populating the ModelState collection with entries based on the active Rule Violations in the Dinner object:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample13.cs)]

#### <a name="html-helper-integration-with-modelstate"></a><span data-ttu-id="16ef1-241">與 ModelState 的 html Helper 整合</span><span class="sxs-lookup"><span data-stu-id="16ef1-241">Html Helper Integration with ModelState</span></span>

<span data-ttu-id="16ef1-242">HTML helper 方法-例如 Html.TextBox()-轉譯輸出時，請檢查 ModelState 集合。</span><span class="sxs-lookup"><span data-stu-id="16ef1-242">HTML helper methods - like Html.TextBox() - check the ModelState collection when rendering output.</span></span> <span data-ttu-id="16ef1-243">如果發生錯誤的項目存在，它們會呈現使用者輸入的值和 CSS 錯誤類別。</span><span class="sxs-lookup"><span data-stu-id="16ef1-243">If an error for the item exists, they render the user-entered value and a CSS error class.</span></span>

<span data-ttu-id="16ef1-244">例如，在我們的 [編輯] 檢視中我們使用 Html.TextBox() helper 方法轉譯我們 Dinner 物件的 EventDate:</span><span class="sxs-lookup"><span data-stu-id="16ef1-244">For example, in our "Edit" view we are using the Html.TextBox() helper method to render the EventDate of our Dinner object:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample14.aspx)]

<span data-ttu-id="16ef1-245">當錯誤狀況中呈現的檢視時，Html.TextBox() 方法會檢查 ModelState 集合，以查看只有 「 EventDate"我們 Dinner 物件屬性相關聯的任何錯誤時。</span><span class="sxs-lookup"><span data-stu-id="16ef1-245">When the view was rendered in the error scenario, the Html.TextBox() method checked the ModelState collection to see if there were any errors associated with the "EventDate" property of our Dinner object.</span></span> <span data-ttu-id="16ef1-246">判斷已發生錯誤時呈現送出的使用者輸入 (「 BOGUS") 的值，並加入至 css 錯誤類別&lt;輸入類型 ="textbox"/&gt;它產生的標記：</span><span class="sxs-lookup"><span data-stu-id="16ef1-246">When it determined that there was an error it rendered the submitted user input ("BOGUS") as the value, and added a css error class to the &lt;input type="textbox"/&gt; markup it generated:</span></span>

[!code-html[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample15.html)]

<span data-ttu-id="16ef1-247">您可以自訂 css 錯誤類別，尋找但是您希望的外觀。</span><span class="sxs-lookup"><span data-stu-id="16ef1-247">You can customize the appearance of the css error class to look however you want.</span></span> <span data-ttu-id="16ef1-248">中已定義預設 CSS 錯誤類別 – 「 輸入-驗證-錯誤 」 – *\content\site.css*樣式表，看起來類似下面的：</span><span class="sxs-lookup"><span data-stu-id="16ef1-248">The default CSS error class – "input-validation-error" – is defined in the *\content\site.css* stylesheet and looks like below:</span></span>

[!code-css[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample16.css)]

<span data-ttu-id="16ef1-249">這個 CSS 規則是什麼造成我們無效的輸入項目以反白顯示類似如下：</span><span class="sxs-lookup"><span data-stu-id="16ef1-249">This CSS rule is what caused our invalid input elements to be highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image10.png)

##### <a name="htmlvalidationmessage-helper-method"></a><span data-ttu-id="16ef1-250">Html.ValidationMessage() Helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-250">Html.ValidationMessage() Helper Method</span></span>

<span data-ttu-id="16ef1-251">Html.ValidationMessage() helper 方法可以用來輸出與特定模型屬性相關聯的 ModelState 錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="16ef1-251">The Html.ValidationMessage() helper method can be used to output the ModelState error message associated with a particular model property:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample17.aspx)]

<span data-ttu-id="16ef1-252">上述程式碼的輸出：  *&lt;/><span class ="欄位的驗證錯誤的 「&gt; 'BOGUS' 的值無效 &lt; /s p&gt;*</span><span class="sxs-lookup"><span data-stu-id="16ef1-252">The above code outputs: *&lt;span class="field-validation-error"&gt; The value ‘BOGUS' is invalid&lt;/span&gt;*</span></span>

<span data-ttu-id="16ef1-253">Html.ValidationMessage() 協助程式方法也支援可讓開發人員覆寫會顯示錯誤文字訊息的第二個參數：</span><span class="sxs-lookup"><span data-stu-id="16ef1-253">The Html.ValidationMessage() helper method also supports a second parameter that allows developers to override the error text message that is displayed:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample18.aspx)]

<span data-ttu-id="16ef1-254">上述程式碼的輸出：  *&lt;/><span class ="欄位的驗證錯誤的 「&gt;\*&lt;/s p a&gt;*而不是針對有錯誤時的預設錯誤文字EventDate 屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-254">The above code outputs: *&lt;span class="field-validation-error"&gt;\*&lt;/span&gt;*instead of the default error text when an error is present for the EventDate property.</span></span>

##### <a name="htmlvalidationsummary-helper-method"></a><span data-ttu-id="16ef1-255">Html.ValidationSummary() Helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-255">Html.ValidationSummary() Helper Method</span></span>

<span data-ttu-id="16ef1-256">Html.ValidationSummary() helper 方法可以用來呈現摘要錯誤訊息，伴隨著&lt;ul&gt;&lt;li /&gt;&lt;/u l&gt;之所有詳細錯誤訊息清單中ModelState 集合：</span><span class="sxs-lookup"><span data-stu-id="16ef1-256">The Html.ValidationSummary() helper method can be used to render a summary error message, accompanied by a &lt;ul&gt;&lt;li/&gt;&lt;/ul&gt; list of all detailed error messages in the ModelState collection:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image11.png)

<span data-ttu-id="16ef1-257">Html.ValidationSummary() helper 方法會接受選擇性的字串參數 – 定義詳細的錯誤清單上方顯示的摘要錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="16ef1-257">The Html.ValidationSummary() helper method takes an optional string parameter – which defines a summary error message to display above the list of detailed errors:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample19.aspx)]

<span data-ttu-id="16ef1-258">您可以選擇覆寫 錯誤清單看起來像使用 CSS。</span><span class="sxs-lookup"><span data-stu-id="16ef1-258">You can optionally use CSS to override what the error list looks like.</span></span>

#### <a name="using-a-addruleviolations-helper-method"></a><span data-ttu-id="16ef1-259">使用 AddRuleViolations Helper 方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-259">Using a AddRuleViolations Helper Method</span></span>

<span data-ttu-id="16ef1-260">初始 HTTP POST 編輯實作會使用 foreach 陳述式的 catch 區塊中迴圈 Dinner 物件的規則違規時，並將它們新增至控制器的 ModelState 集合：</span><span class="sxs-lookup"><span data-stu-id="16ef1-260">Our initial HTTP-POST Edit implementation used a foreach statement within its catch block to loop over the Dinner object's Rule Violations and add them to the controller's ModelState collection:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample20.cs)]

<span data-ttu-id="16ef1-261">我們可以讓此程式碼，加上"ControllerHelpers"小清潔器 NerdDinner 專案中，類別，並實作加入至 ASP.NET MVC ModelStateDictionary 類別的 helper 方法的"AddRuleViolations 」 擴充方法中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-261">We can make this code a little cleaner by adding a "ControllerHelpers" class to the NerdDinner project, and implement an "AddRuleViolations" extension method within it that adds a helper method to the ASP.NET MVC ModelStateDictionary class.</span></span> <span data-ttu-id="16ef1-262">這個擴充方法可以將封裝以填入 ModelStateDictionary RuleViolation 錯誤的清單所需的邏輯：</span><span class="sxs-lookup"><span data-stu-id="16ef1-262">This extension method can encapsulate the logic necessary to populate the ModelStateDictionary with a list of RuleViolation errors:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample21.cs)]

<span data-ttu-id="16ef1-263">然後，我們可以更新 HTTP POST 編輯動作方法使用這個擴充方法來填入我們 Dinner 規則違規的 ModelState 集合。</span><span class="sxs-lookup"><span data-stu-id="16ef1-263">We can then update our HTTP-POST Edit action method to use this extension method to populate the ModelState collection with our Dinner Rule Violations.</span></span>

#### <a name="complete-edit-action-method-implementations"></a><span data-ttu-id="16ef1-264">完成編輯動作方法的實作</span><span class="sxs-lookup"><span data-stu-id="16ef1-264">Complete Edit Action Method Implementations</span></span>

<span data-ttu-id="16ef1-265">下列程式碼會實作所有控制器所需的邏輯我們的案例中編輯：</span><span class="sxs-lookup"><span data-stu-id="16ef1-265">The code below implements all of the controller logic necessary for our Edit scenario:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample22.cs)]

<span data-ttu-id="16ef1-266">編輯實作的好處就是我們控制器類別都我們檢視的範本必須知道的任何項目有關的特定驗證或我們的 Dinner 模型中強制執行商務規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-266">The nice thing about our Edit implementation is that neither our Controller class nor our View template has to know anything about the specific validation or business rules being enforced by our Dinner model.</span></span> <span data-ttu-id="16ef1-267">我們可以將其他規則加入我們的模型在未來，不需要對我們的控制器或檢視中，都必須支援的任何程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="16ef1-267">We can add additional rules to our model in the future and do not have to make any code changes to our controller or view in order for them to be supported.</span></span> <span data-ttu-id="16ef1-268">這會讓我們有彈性地輕鬆發展我們應用程式的需求在未來，最少的程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="16ef1-268">This provides us with the flexibility to easily evolve our application requirements in the future with a minimum of code changes.</span></span>

### <a name="create-support"></a><span data-ttu-id="16ef1-269">建立的支援</span><span class="sxs-lookup"><span data-stu-id="16ef1-269">Create Support</span></span>

<span data-ttu-id="16ef1-270">我們已完成實作 DinnersController 類別的 「 編輯 」 行為。</span><span class="sxs-lookup"><span data-stu-id="16ef1-270">We've finished implementing the "Edit" behavior of our DinnersController class.</span></span> <span data-ttu-id="16ef1-271">我們現在移到它-這可讓使用者加入新 Dinners 上實作的 「 建立 」 支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-271">Let's now move on to implement the "Create" support on it – which will enable users to add new Dinners.</span></span>

#### <a name="the-http-get-create-action-method"></a><span data-ttu-id="16ef1-272">HTTP GET 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-272">The HTTP-GET Create Action Method</span></span>

<span data-ttu-id="16ef1-273">我們一開始會藉由實作的 HTTP 「 取得 」 行為我們建立動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-273">We'll begin by implementing the HTTP "GET" behavior of our create action method.</span></span> <span data-ttu-id="16ef1-274">會呼叫這個方法，當有人造訪*/Dinners/建立*URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-274">This method will be called when someone visits the */Dinners/Create* URL.</span></span> <span data-ttu-id="16ef1-275">我們實作如下所示：</span><span class="sxs-lookup"><span data-stu-id="16ef1-275">Our implementation looks like:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample23.cs)]

<span data-ttu-id="16ef1-276">上述程式碼會建立新的 Dinner 物件，並指派其 EventDate 屬性是在未來一週。</span><span class="sxs-lookup"><span data-stu-id="16ef1-276">The code above creates a new Dinner object, and assigns its EventDate property to be one week in the future.</span></span> <span data-ttu-id="16ef1-277">然後，它會呈現新 Dinner 物件為基礎的檢視。</span><span class="sxs-lookup"><span data-stu-id="16ef1-277">It then renders a View that is based on the new Dinner object.</span></span> <span data-ttu-id="16ef1-278">因為我們尚未明確傳遞的名稱*View()* helper 方法，它將使用的慣例為基礎的預設路徑解析檢視範本： /Views/Dinners/Create.aspx。</span><span class="sxs-lookup"><span data-stu-id="16ef1-278">Because we haven't explicitly passed a name to the *View()* helper method, it will use the convention based default path to resolve the view template: /Views/Dinners/Create.aspx.</span></span>

<span data-ttu-id="16ef1-279">我們現在來建立此檢視範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-279">Let's now create this view template.</span></span> <span data-ttu-id="16ef1-280">建立動作方法內按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令，我們可以執行這項操作。</span><span class="sxs-lookup"><span data-stu-id="16ef1-280">We can do this by right-clicking within the Create action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="16ef1-281">在 [新增檢視] 對話方塊中，我們會指出我們會將 Dinner 物件傳遞至檢視的範本，並選擇自動 scaffold 「 建立 」 範本：</span><span class="sxs-lookup"><span data-stu-id="16ef1-281">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to the view template, and choose to auto-scaffold a "Create" template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image12.png)

<span data-ttu-id="16ef1-282">當我們按一下 [新增] 按鈕時，Visual Studio 將"\Views\Dinners"目錄中，儲存新的 scaffold 基礎"Create.aspx 」 檢視，並在 IDE 中開啟它：</span><span class="sxs-lookup"><span data-stu-id="16ef1-282">When we click the "Add" button, Visual Studio will save a new scaffold-based "Create.aspx" view to the "\Views\Dinners" directory, and open it up within the IDE:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image13.png)

<span data-ttu-id="16ef1-283">讓我們來變更幾個預設 「 建立 」 scaffold 檔案所產生的是，，並向上修改看起來如下：</span><span class="sxs-lookup"><span data-stu-id="16ef1-283">Let's make a few changes to the default "create" scaffold file that was generated for us, and modify it up to look like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample24.aspx)]

<span data-ttu-id="16ef1-284">現在，當我們執行我們的應用程式，以及存取和*"/ Dinners/建立 「*瀏覽器，它會呈現 UI 類似下面我們建立動作實作中的 URL:</span><span class="sxs-lookup"><span data-stu-id="16ef1-284">And now when we run our application and access the *"/Dinners/Create"* URL within the browser it will render UI like below from our Create action implementation:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image14.png)

#### <a name="implementing-the-http-post-create-action-method"></a><span data-ttu-id="16ef1-285">實作 HTTP POST 建立動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-285">Implementing the HTTP-POST Create Action Method</span></span>

<span data-ttu-id="16ef1-286">我們有實作我們建立動作方法的 HTTP GET 版本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-286">We have the HTTP-GET version of our Create action method implemented.</span></span> <span data-ttu-id="16ef1-287">當使用者按一下 [儲存] 按鈕會執行的表單 post */Dinners/建立*URL，並送出 HTML&lt;輸入&gt;形成使用 HTTP POST 動詞命令的值。</span><span class="sxs-lookup"><span data-stu-id="16ef1-287">When a user clicks the "Save" button it performs a form post to the */Dinners/Create* URL, and submits the HTML &lt;input&gt; form values using the HTTP POST verb.</span></span>

<span data-ttu-id="16ef1-288">現在讓我們實作的 HTTP POST 行為我們建立動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-288">Let's now implement the HTTP POST behavior of our create action method.</span></span> <span data-ttu-id="16ef1-289">我們一開始會將多載的 「 建立 」 動作方法加入至我們 DinnersController 其上已有 「 AcceptVerbs"屬性，表示它會處理 HTTP POST 案例：</span><span class="sxs-lookup"><span data-stu-id="16ef1-289">We'll begin by adding an overloaded "Create" action method to our DinnersController that has an "AcceptVerbs" attribute on it that indicates it handles HTTP POST scenarios:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample25.cs)]

<span data-ttu-id="16ef1-290">有各種不同的方式，所以我們可以存取的已張貼的表單參數中啟用 HTTP POST 「 建立 」 方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-290">There are a variety of ways we can access the posted form parameters within our HTTP-POST enabled "Create" method.</span></span>

<span data-ttu-id="16ef1-291">其中一個方法是建立新的 Dinner 物件，然後使用*UpdateModel()* helper 方法 （例如我們所做的編輯動作） 要填入的已張貼的表單值。</span><span class="sxs-lookup"><span data-stu-id="16ef1-291">One approach is to create a new Dinner object and then use the *UpdateModel()* helper method (like we did with the Edit action) to populate it with the posted form values.</span></span> <span data-ttu-id="16ef1-292">我們可以將它加入至我們 DinnerRepository、 將它保存到資料庫，並將使用者重新導向至我們的詳細資料動作以顯示新建立的 Dinner 使用下列程式碼：</span><span class="sxs-lookup"><span data-stu-id="16ef1-292">We can then add it to our DinnerRepository, persist it to the database, and redirect the user to our Details action to show the newly created Dinner using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample26.cs)]

<span data-ttu-id="16ef1-293">或者，我們可以使用的方法有我們採取 Dinner 物件做為方法參數的 create （） 的動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-293">Alternatively, we can use an approach where we have our Create() action method take a Dinner object as a method parameter.</span></span> <span data-ttu-id="16ef1-294">ASP.NET MVC 接著自動具現化新的 Dinner 物件我們、 填入其屬性使用的格式輸入，並將它傳遞至動作方法：</span><span class="sxs-lookup"><span data-stu-id="16ef1-294">ASP.NET MVC will then automatically instantiate a new Dinner object for us, populate its properties using the form inputs, and pass it to our action method:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample27.cs)]

<span data-ttu-id="16ef1-295">我們上面的動作方法會驗證，Dinner 物件已成功填入表單張貼值藉由檢查 ModelState.IsValid 屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-295">Our action method above verifies that the Dinner object has been successfully populated with the form post values by checking the ModelState.IsValid property.</span></span> <span data-ttu-id="16ef1-296">這會傳回 false，如果沒有輸入轉換問題 (例如:"BOGUS"EventDate 屬性的字串)，並有任何問題，如果我們動作方法重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="16ef1-296">This will return false if there are input conversion issues (for example: a string of "BOGUS" for the EventDate property), and if there are any issues our action method redisplays the form.</span></span>

<span data-ttu-id="16ef1-297">如果輸入的值是有效的動作方法就會嘗試加入及儲存 DinnerRepository 新 Dinner。</span><span class="sxs-lookup"><span data-stu-id="16ef1-297">If the input values are valid, then the action method attempts to add and save the new Dinner to the DinnerRepository.</span></span> <span data-ttu-id="16ef1-298">它會包裝在 try/catch 區塊內的這個工作，並重新顯示表單，如果沒有任何商務規則違規 （這會導致引發例外狀況的 dinnerRepository.Save() 方法）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-298">It wraps this work within a try/catch block and redisplays the form if there are any business rule violations (which would cause the dinnerRepository.Save() method to raise an exception).</span></span>

<span data-ttu-id="16ef1-299">若要查看此錯誤處理動作中的行為，我們可以要求*/Dinners/建立*URL 和填滿出新 Dinner 相關詳細資料。</span><span class="sxs-lookup"><span data-stu-id="16ef1-299">To see this error handling behavior in action, we can request the */Dinners/Create* URL and fill out details about a new Dinner.</span></span> <span data-ttu-id="16ef1-300">不正確的輸入值，會導致建立表單，會重新顯示具有反白顯示類似如下的錯誤：</span><span class="sxs-lookup"><span data-stu-id="16ef1-300">Incorrect input or values will cause the create form to be redisplayed with the errors highlighted like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image15.png)

<span data-ttu-id="16ef1-301">請注意，我們建立表單的完全相同驗證和商務規則為我們的編輯表單中的特殊區分。</span><span class="sxs-lookup"><span data-stu-id="16ef1-301">Notice how our Create form is honoring the exact same validation and business rules as our Edit form.</span></span> <span data-ttu-id="16ef1-302">這是因為我們驗證和商務規則已定義在模型中，而且沒有 UI 或控制站的應用程式中嵌入。</span><span class="sxs-lookup"><span data-stu-id="16ef1-302">This is because our validation and business rules were defined in the model, and were not embedded within the UI or controller of the application.</span></span> <span data-ttu-id="16ef1-303">這表示我們可以稍後變更/改良我們的驗證，或在單一的商務規則將和已將它們套用在我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="16ef1-303">This means we can later change/evolve our validation or business rules in a single place and have them apply throughout our application.</span></span> <span data-ttu-id="16ef1-304">我們不會變更任何程式碼內我們編輯或建立要自動接受任何新的規則或修改現有的動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-304">We will not have to change any code within either our Edit or Create action methods to automatically honor any new rules or modifications to existing ones.</span></span>

<span data-ttu-id="16ef1-305">當我們修正 輸入的值，然後按一下 儲存 按鈕同樣地，我們除了 DinnerRepository 會成功，而且新 Dinner 會加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="16ef1-305">When we fix the input values and click the "Save" button again, our addition to the DinnerRepository will succeed, and a new Dinner will be added to the database.</span></span> <span data-ttu-id="16ef1-306">我們會接著會重新導向至*/Dinners/詳細資料 / [id]* URL-我們看到新建立的 Dinner 的相關詳細資料：</span><span class="sxs-lookup"><span data-stu-id="16ef1-306">We will then be redirected to the */Dinners/Details/[id]* URL – where we will be presented with details about the newly created Dinner:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image16.png)

### <a name="delete-support"></a><span data-ttu-id="16ef1-307">刪除支援</span><span class="sxs-lookup"><span data-stu-id="16ef1-307">Delete Support</span></span>

<span data-ttu-id="16ef1-308">我們現在加入 「 刪除 」 支援我們 DinnersController。</span><span class="sxs-lookup"><span data-stu-id="16ef1-308">Let's now add "Delete" support to our DinnersController.</span></span>

#### <a name="the-http-get-delete-action-method"></a><span data-ttu-id="16ef1-309">HTTP GET Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-309">The HTTP-GET Delete Action Method</span></span>

<span data-ttu-id="16ef1-310">我們一開始會藉由實作我們 delete 動作方法的 HTTP GET 行為。</span><span class="sxs-lookup"><span data-stu-id="16ef1-310">We'll begin by implementing the HTTP GET behavior of our delete action method.</span></span> <span data-ttu-id="16ef1-311">這個方法會呼叫當有人造訪*/Dinners/刪除 / [id]* URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-311">This method will get called when someone visits the */Dinners/Delete/[id]* URL.</span></span> <span data-ttu-id="16ef1-312">以下是實作：</span><span class="sxs-lookup"><span data-stu-id="16ef1-312">Below is the implementation:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample28.cs)]

<span data-ttu-id="16ef1-313">擷取要刪除 Dinner 嘗試的動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-313">The action method attempts to retrieve the Dinner to be deleted.</span></span> <span data-ttu-id="16ef1-314">如果 Dinner 已存在，則會呈現檢視會根據 Dinner 物件。</span><span class="sxs-lookup"><span data-stu-id="16ef1-314">If the Dinner exists it renders a View based on the Dinner object.</span></span> <span data-ttu-id="16ef1-315">如果物件不存在 （或已刪除） 其傳回"NotFound"會呈現的檢視會檢視我們稍早為 [詳細資料] 動作方法建立的範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-315">If the object doesn't exist (or has already been deleted) it returns a View that renders the "NotFound" view template we created earlier for our "Details" action method.</span></span>

<span data-ttu-id="16ef1-316">我們可以建立 「 刪除 」 檢視範本內的 Delete 動作方法上按一下滑鼠右鍵，然後選取 [新增檢視] 內容功能表命令。</span><span class="sxs-lookup"><span data-stu-id="16ef1-316">We can create the "Delete" view template by right-clicking within the Delete action method and selecting the "Add View" context menu command.</span></span> <span data-ttu-id="16ef1-317">在 [新增檢視] 對話方塊中，我們會表示我們要將 Dinner 物件傳遞給為其模型中，我們檢視的範本，並選擇建立空白的範本：</span><span class="sxs-lookup"><span data-stu-id="16ef1-317">Within the "Add View" dialog we'll indicate that we are passing a Dinner object to our view template as its model, and choose to create an empty template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image17.png)

<span data-ttu-id="16ef1-318">當我們按一下 [新增] 按鈕時，Visual Studio 會將新的 「 Delete.aspx 」 檢視的範本檔之我們我們"\Views\Dinners"目錄中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-318">When we click the "Add" button, Visual Studio will add a new "Delete.aspx" view template file for us within our "\Views\Dinners" directory.</span></span> <span data-ttu-id="16ef1-319">我們會將某些 HTML 和程式碼新增至範本中，實作 [刪除確認] 畫面中類似下面的：</span><span class="sxs-lookup"><span data-stu-id="16ef1-319">We'll add some HTML and code to the template to implement a delete confirmation screen like below:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample29.aspx)]

<span data-ttu-id="16ef1-320">上述程式碼顯示要刪除的 Dinner 和輸出的標題&lt;表單&gt;未 /Dinners/刪除 / [id] url 的 POST，如果使用者按一下 [刪除] 按鈕，在其中的項目。</span><span class="sxs-lookup"><span data-stu-id="16ef1-320">The code above displays the title of the Dinner to be deleted, and outputs a &lt;form&gt; element that does a POST to the /Dinners/Delete/[id] URL if the end-user clicks the "Delete" button within it.</span></span>

<span data-ttu-id="16ef1-321">當我們執行我們的應用程式和存取*"/ Dinners/刪除 / [id]"* URL 有效 Dinner 物件它呈現 UI 類似下面的：</span><span class="sxs-lookup"><span data-stu-id="16ef1-321">When we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it renders UI like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image18.png)

| <span data-ttu-id="16ef1-322">**側邊主題內容： 為什麼我們所做的 POST？**</span><span class="sxs-lookup"><span data-stu-id="16ef1-322">**Side Topic: Why are we doing a POST?**</span></span> |
| --- |
| <span data-ttu-id="16ef1-323">您可能會問： 為什麼我們未瀏覽建立的投入時間&lt;表單&gt;我們刪除確認 畫面中？</span><span class="sxs-lookup"><span data-stu-id="16ef1-323">You might ask – why did we go through the effort of creating a &lt;form&gt; within our Delete confirmation screen?</span></span> <span data-ttu-id="16ef1-324">為何不直接使用標準的超連結來連結至動作方法會執行實際的刪除作業？</span><span class="sxs-lookup"><span data-stu-id="16ef1-324">Why not just use a standard hyperlink to link to an action method that does the actual delete operation?</span></span> <span data-ttu-id="16ef1-325">原因是因為我們想要非常小心，防範網頁自動尋檢及搜尋引擎探索我們的 Url 和不小心造成它們遵循下列連結時要刪除的資料。</span><span class="sxs-lookup"><span data-stu-id="16ef1-325">The reason is because we want to be careful to guard against web-crawlers and search engines discovering our URLs and inadvertently causing data to be deleted when they follow the links.</span></span> <span data-ttu-id="16ef1-326">HTTP GET Url 會被視為 「 安全 」，才能存取/搜耙，而且應該遵循 HTTP POST 的。</span><span class="sxs-lookup"><span data-stu-id="16ef1-326">HTTP-GET based URLs are considered "safe" for them to access/crawl, and they are supposed to not follow HTTP-POST ones.</span></span> <span data-ttu-id="16ef1-327">請確定您一律將破壞型或 HTTP POST 要求背後的資料修改作業是理想的規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-327">A good rule is to make sure you always put destructive or data modifying operations behind HTTP-POST requests.</span></span> |

#### <a name="implementing-the-http-post-delete-action-method"></a><span data-ttu-id="16ef1-328">實作 HTTP POST 的 Delete 動作方法</span><span class="sxs-lookup"><span data-stu-id="16ef1-328">Implementing the HTTP-POST Delete Action Method</span></span>

<span data-ttu-id="16ef1-329">我們現在有我們實作的刪除動作方法的 HTTP GET 版本以顯示 [刪除確認] 畫面中。</span><span class="sxs-lookup"><span data-stu-id="16ef1-329">We now have the HTTP-GET version of our Delete action method implemented which displays a delete confirmation screen.</span></span> <span data-ttu-id="16ef1-330">當使用者按一下 [刪除] 按鈕就會執行的表單 post */Dinners/Dinner / [id]* URL。</span><span class="sxs-lookup"><span data-stu-id="16ef1-330">When an end user clicks the "Delete" button it will perform a form post to the */Dinners/Dinner/[id]* URL.</span></span>

<span data-ttu-id="16ef1-331">讓我們實作了使用下列程式碼的 delete 動作方法的 HTTP"POST"行為：</span><span class="sxs-lookup"><span data-stu-id="16ef1-331">Let's now implement the HTTP "POST" behavior of the delete action method using the code below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample30.cs)]

<span data-ttu-id="16ef1-332">擷取要刪除的 dinner 物件嘗試刪除動作方法的 HTTP POST 版本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-332">The HTTP-POST version of our Delete action method attempts to retrieve the dinner object to delete.</span></span> <span data-ttu-id="16ef1-333">如果它找不到 （因為它已經被刪除） 呈現我們"NotFound"的範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-333">If it can't find it (because it has already been deleted) it renders our "NotFound" template.</span></span> <span data-ttu-id="16ef1-334">如果找到 Dinner，它從 DinnerRepository 刪除它。</span><span class="sxs-lookup"><span data-stu-id="16ef1-334">If it finds the Dinner, it deletes it from the DinnerRepository.</span></span> <span data-ttu-id="16ef1-335">然後，它會呈現 [已刪除] 範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-335">It then renders a "Deleted" template.</span></span>

<span data-ttu-id="16ef1-336">若要實作我們將動作方法中以滑鼠右鍵按一下並選擇 [新增檢視] 內容功能表的 [已刪除] 範本。</span><span class="sxs-lookup"><span data-stu-id="16ef1-336">To implement the "Deleted" template we'll right-click in the action method and choose the "Add View" context menu.</span></span> <span data-ttu-id="16ef1-337">我們將命名我們檢視 [已刪除]，並讓它為空白的範本 （而且不接受強型別模型物件）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-337">We'll name our view "Deleted" and have it be an empty template (and not take a strongly-typed model object).</span></span> <span data-ttu-id="16ef1-338">然後，我們會將一些 HTML 內容新增至它：</span><span class="sxs-lookup"><span data-stu-id="16ef1-338">We'll then add some HTML content to it:</span></span>

[!code-aspx[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample31.aspx)]

<span data-ttu-id="16ef1-339">現在，當我們執行我們的應用程式，以及存取和*"/ Dinners/刪除 / [id]"* URL 有效 Dinner，它會呈現我們 Dinner 物件刪除確認畫面上類似如下：</span><span class="sxs-lookup"><span data-stu-id="16ef1-339">And now when we run our application and access the *"/Dinners/Delete/[id]"* URL for a valid Dinner object it will render our Dinner delete confirmation screen like below:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image19.png)

<span data-ttu-id="16ef1-340">當我們按一下 [刪除] 按鈕就會執行至 HTTP POST */Dinners/刪除 / [id]* URL，這將刪除 Dinner 從我們的資料庫，並顯示我們的 [已刪除] 檢視範本：</span><span class="sxs-lookup"><span data-stu-id="16ef1-340">When we click the "Delete" button it will perform an HTTP-POST to the */Dinners/Delete/[id]* URL, which will delete the Dinner from our database, and display our "Deleted" view template:</span></span>

![](provide-crud-create-read-update-delete-data-form-entry-support/_static/image20.png)

### <a name="model-binding-security"></a><span data-ttu-id="16ef1-341">模型繫結安全性</span><span class="sxs-lookup"><span data-stu-id="16ef1-341">Model Binding Security</span></span>

<span data-ttu-id="16ef1-342">我們已討論過兩個不同的方式，使用內建 ASP.NET MVC 模型繫結功能。</span><span class="sxs-lookup"><span data-stu-id="16ef1-342">We've discussed two different ways to use the built-in model-binding features of ASP.NET MVC.</span></span> <span data-ttu-id="16ef1-343">第一次使用 UpdateModel() 方法來更新現有的模型物件的屬性和第二個使用 ASP.NET MVC 支援傳遞做為動作方法參數中的模型物件。</span><span class="sxs-lookup"><span data-stu-id="16ef1-343">The first using the UpdateModel() method to update properties on an existing model object, and the second using ASP.NET MVC's support for passing model objects in as action method parameters.</span></span> <span data-ttu-id="16ef1-344">這兩種技巧是非常強大且非常有用。</span><span class="sxs-lookup"><span data-stu-id="16ef1-344">Both of these techniques are very powerful and extremely useful.</span></span>

<span data-ttu-id="16ef1-345">這項強大也會伴隨著責任。</span><span class="sxs-lookup"><span data-stu-id="16ef1-345">This power also brings with it responsibility.</span></span> <span data-ttu-id="16ef1-346">請務必一律是不合理，關於安全性接受使用者輸入時，這一點也成立時物件繫結至表單的輸入。</span><span class="sxs-lookup"><span data-stu-id="16ef1-346">It is important to always be paranoid about security when accepting any user input, and this is also true when binding objects to form input.</span></span> <span data-ttu-id="16ef1-347">您應該要特別注意一律 HTML 編碼可避免 HTML 和 JavaScript 插入式攻擊，並注意 SQL 資料隱碼攻擊的任何使用者輸入的值 (注意： 我們會使用 LINQ to SQL 應用程式，將會自動編碼參數以避免這些類型的攻擊）。</span><span class="sxs-lookup"><span data-stu-id="16ef1-347">You should be careful to always HTML encode any user-entered values to avoid HTML and JavaScript injection attacks, and be careful of SQL injection attacks (note: we are using LINQ to SQL for our application, which automatically encodes parameters to prevent these types of attacks).</span></span> <span data-ttu-id="16ef1-348">永遠不應該依賴用戶端驗證時，一律使用伺服器端驗證，以防止駭客嘗試傳送您假的值。</span><span class="sxs-lookup"><span data-stu-id="16ef1-348">You should never rely on client-side validation alone, and always employ server-side validation to guard against hackers attempting to send you bogus values.</span></span>

<span data-ttu-id="16ef1-349">先確定您的想法時使用 ASP.NET MVC 的繫結功能的一個額外的安全性項目是您要繫結物件的領域。</span><span class="sxs-lookup"><span data-stu-id="16ef1-349">One additional security item to make sure you think about when using the binding features of ASP.NET MVC is the scope of the objects you are binding.</span></span> <span data-ttu-id="16ef1-350">具體來說，您會想要確定您了解您允許將可繫結，並請確定您只允許實際上應該由要更新的使用者可更新這些屬性的屬性的安全性含意。</span><span class="sxs-lookup"><span data-stu-id="16ef1-350">Specifically, you want to make sure you understand the security implications of the properties you are allowing to be bound, and make sure you only allow those properties that really should be updatable by an end-user to be updated.</span></span>

<span data-ttu-id="16ef1-351">根據預設，UpdateModel() 方法會嘗試更新的模型物件上符合連入的表單參數值的所有屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-351">By default, the UpdateModel() method will attempt to update all properties on the model object that match incoming form parameter values.</span></span> <span data-ttu-id="16ef1-352">同樣地，根據預設也傳遞做為動作方法參數的物件可以有所有表單參數透過設定其屬性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-352">Likewise, objects passed as action method parameters also by default can have all of their properties set via form parameters.</span></span>

#### <a name="locking-down-binding-on-a-per-usage-basis"></a><span data-ttu-id="16ef1-353">鎖定每次使用量為基礎的繫結</span><span class="sxs-lookup"><span data-stu-id="16ef1-353">Locking down binding on a per-usage basis</span></span>

<span data-ttu-id="16ef1-354">您可以提供明確"include 清單 」 可以更新的內容，以鎖定每次使用量為基礎的繫結原則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-354">You can lock down the binding policy on a per-usage basis by providing an explicit "include list" of properties that can be updated.</span></span> <span data-ttu-id="16ef1-355">將額外的字串陣列參數傳遞至像下面 UpdateModel() 方法可以達成此目的：</span><span class="sxs-lookup"><span data-stu-id="16ef1-355">This can be done by passing an extra string array parameter to the UpdateModel() method like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample32.cs)]

<span data-ttu-id="16ef1-356">物件傳遞為動作方法參數也支援可讓 「 include 清單 」 的允許屬性，以相同方式來指定以下的 [繫結] 屬性：</span><span class="sxs-lookup"><span data-stu-id="16ef1-356">Objects passed as action method parameters also support a [Bind] attribute that enables an "include list" of allowed properties to be specified like below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample33.cs)]

#### <a name="locking-down-binding-on-a-type-basis"></a><span data-ttu-id="16ef1-357">鎖定類型為基礎的繫結</span><span class="sxs-lookup"><span data-stu-id="16ef1-357">Locking down binding on a type basis</span></span>

<span data-ttu-id="16ef1-358">您也可以鎖定個別類型為基礎的繫結規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-358">You can also lock down the binding rules on a per-type basis.</span></span> <span data-ttu-id="16ef1-359">這可讓您一次，指定繫結規則，然後再將它們套用在所有案例 （包括 UpdateModel 和動作方法參數的案例） 中跨所有控制器和動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-359">This allows you to specify the binding rules once, and then have them apply in all scenarios (including both UpdateModel and action method parameter scenarios) across all controllers and action methods.</span></span>

<span data-ttu-id="16ef1-360">加入至類型，[繫結] 屬性，或藉由註冊 （適用於案例，您並不擁有型別） 的應用程式 Global.asax 檔案中，您可以自訂每個型別繫結規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-360">You can customize the per-type binding rules by adding a [Bind] attribute onto a type, or by registering it within the Global.asax file of the application (useful for scenarios where you don't own the type).</span></span> <span data-ttu-id="16ef1-361">然後，您可以使用該繫結屬性的包含和排除的屬性，以控制哪些屬性是可繫結的特定類別或介面。</span><span class="sxs-lookup"><span data-stu-id="16ef1-361">You can then use the Bind attribute's Include and Exclude properties to control which properties are bindable for the particular class or interface.</span></span>

<span data-ttu-id="16ef1-362">我們將 Dinner 類別的這項技術用於 NerdDinner 應用程式中，並加入 [繫結] 屬性，以限制可繫結的屬性清單所示：</span><span class="sxs-lookup"><span data-stu-id="16ef1-362">We'll use this technique for the Dinner class in our NerdDinner application, and add a [Bind] attribute to it that restricts the list of bindable properties to the following:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample34.cs)]

<span data-ttu-id="16ef1-363">請注意，我們不允許繫結，透過可操作的與會者集合，也不我們允許透過繫結來設定 DinnerID 或 HostedBy 的內容。</span><span class="sxs-lookup"><span data-stu-id="16ef1-363">Notice we are not allowing the RSVPs collection to be manipulated via binding, nor are we allowing the DinnerID or HostedBy properties to be set via binding.</span></span> <span data-ttu-id="16ef1-364">基於安全性考量我們將改為只操作這些特定的屬性，使用明確的程式碼，在我們的動作方法。</span><span class="sxs-lookup"><span data-stu-id="16ef1-364">For security reasons we'll instead only manipulate these particular properties using explicit code within our action methods.</span></span>

### <a name="crud-wrap-up"></a><span data-ttu-id="16ef1-365">CRUD Wrap-Up</span><span class="sxs-lookup"><span data-stu-id="16ef1-365">CRUD Wrap-Up</span></span>

<span data-ttu-id="16ef1-366">ASP.NET MVC 包含數個內建功能可協助完成實作表單張貼案例。</span><span class="sxs-lookup"><span data-stu-id="16ef1-366">ASP.NET MVC includes a number of built-in features that help with implementing form posting scenarios.</span></span> <span data-ttu-id="16ef1-367">我們使用各種不同的這些功能將在我們 DinnerRepository 之上提供 CRUD UI 支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-367">We used a variety of these features to provide CRUD UI support on top of our DinnerRepository.</span></span>

<span data-ttu-id="16ef1-368">我們使用模型已取得焦點的方法來實作我們的應用程式。</span><span class="sxs-lookup"><span data-stu-id="16ef1-368">We are using a model-focused approach to implement our application.</span></span> <span data-ttu-id="16ef1-369">這表示，我們所有的驗證和我們的模型層 – 內和不在我們的控制器或檢視表中定義邏輯的商務規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-369">This means that all our validation and business rule logic is defined within our model layer – and not within our controllers or views.</span></span> <span data-ttu-id="16ef1-370">控制器類別都我們檢視的範本有任何了解 Dinner 模型類別所強制執行特定的商務規則。</span><span class="sxs-lookup"><span data-stu-id="16ef1-370">Neither our Controller class nor our View templates know anything about the specific business rules being enforced by our Dinner model class.</span></span>

<span data-ttu-id="16ef1-371">這將保留我們的應用程式架構的初始狀態，並更輕鬆地測試。</span><span class="sxs-lookup"><span data-stu-id="16ef1-371">This will keep our application architecture clean and make it easier to test.</span></span> <span data-ttu-id="16ef1-372">我們可以新增其他商務規則到我們的模型層未來和*不需要變更任何程式碼*到我們的控制器或檢視中，都必須支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-372">We can add additional business rules to our model layer in the future and *not have to make any code changes* to our Controller or View in order for them to be supported.</span></span> <span data-ttu-id="16ef1-373">這要提供更多的發展及變更應用程式在未來的靈活性。</span><span class="sxs-lookup"><span data-stu-id="16ef1-373">This is going to provide us with a great deal of agility to evolve and change our application in the future.</span></span>

<span data-ttu-id="16ef1-374">我們 DinnersController 現在可讓 Dinner 清單/詳細資料，以及建立、 編輯和刪除支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-374">Our DinnersController now enables Dinner listings/details, as well as create, edit and delete support.</span></span> <span data-ttu-id="16ef1-375">此類別的完整程式碼可以找到如下：</span><span class="sxs-lookup"><span data-stu-id="16ef1-375">The complete code for the class can be found below:</span></span>

[!code-csharp[Main](provide-crud-create-read-update-delete-data-form-entry-support/samples/sample35.cs)]

### <a name="next-step"></a><span data-ttu-id="16ef1-376">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="16ef1-376">Next Step</span></span>

<span data-ttu-id="16ef1-377">我們現在有我們 DinnersController 類別內實作的基本 CRUD （建立、 讀取、 更新和刪除） 支援。</span><span class="sxs-lookup"><span data-stu-id="16ef1-377">We now have basic CRUD (Create, Read, Update and Delete) support implement within our DinnersController class.</span></span>

<span data-ttu-id="16ef1-378">我們現在看我們如何使用我們的表單上啟用更豐富的 UI 別的 ViewData 和 ViewModel 類別。</span><span class="sxs-lookup"><span data-stu-id="16ef1-378">Let's now look at how we can use ViewData and ViewModel classes to enable even richer UI on our forms.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="16ef1-379">[上一頁](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[下一頁](use-viewdata-and-implement-viewmodel-classes.md)</span><span class="sxs-lookup"><span data-stu-id="16ef1-379">[Previous](use-controllers-and-views-to-implement-a-listingdetails-ui.md)
[Next](use-viewdata-and-implement-viewmodel-classes.md)</span></span>
