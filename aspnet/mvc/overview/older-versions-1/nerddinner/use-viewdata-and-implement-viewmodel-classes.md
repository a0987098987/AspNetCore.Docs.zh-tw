---
uid: mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
title: 使用別的 ViewData 和實作 ViewModel 類別 |Microsoft 文件
author: microsoft
description: 步驟 6 顯示如何啟用更豐富格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法:...
ms.author: aspnetcontent
manager: wpickett
ms.date: 07/27/2010
ms.topic: article
ms.assetid: 5755ec4c-60f1-4057-9ec0-3a5de3a20e23
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/nerddinner/use-viewdata-and-implement-viewmodel-classes
msc.type: authoredcontent
ms.openlocfilehash: 9ba8758bd6524f3e300f3fd91ef68cfe8a3587a7
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30879422"
---
<a name="use-viewdata-and-implement-viewmodel-classes"></a><span data-ttu-id="d96d3-103">使用別的 ViewData 和實作 ViewModel 類別</span><span class="sxs-lookup"><span data-stu-id="d96d3-103">Use ViewData and Implement ViewModel Classes</span></span>
====================
<span data-ttu-id="d96d3-104">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d96d3-104">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="d96d3-105">下載 PDF</span><span class="sxs-lookup"><span data-stu-id="d96d3-105">Download PDF</span></span>](http://aspnetmvcbook.s3.amazonaws.com/aspnetmvc-nerdinner_v1.pdf)

> <span data-ttu-id="d96d3-106">這是一套免費的步驟 6 ["NerdDinner 」 應用程式教學課程](introducing-the-nerddinner-tutorial.md)，會逐步引導式如何建置小，但是完成，web 應用程式使用 ASP.NET MVC 1。</span><span class="sxs-lookup"><span data-stu-id="d96d3-106">This is step 6 of a free ["NerdDinner" application tutorial](introducing-the-nerddinner-tutorial.md) that walks-through how to build a small, but complete, web application using ASP.NET MVC 1.</span></span>
> 
> <span data-ttu-id="d96d3-107">步驟 6 顯示如何啟用更豐富格式編輯案例中，支援，同時也討論可用來將資料從控制器傳遞至檢視的兩種方法： 別的 ViewData 和 ViewModel。</span><span class="sxs-lookup"><span data-stu-id="d96d3-107">Step 6 shows how enable support for richer form editing scenarios, and also discusses two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>
> 
> <span data-ttu-id="d96d3-108">如果您使用 ASP.NET MVC 3，我們建議您遵循[取得啟動與 MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md)或[MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md)教學課程。</span><span class="sxs-lookup"><span data-stu-id="d96d3-108">If you are using ASP.NET MVC 3, we recommend you follow the [Getting Started With MVC 3](../../older-versions/getting-started-with-aspnet-mvc3/cs/intro-to-aspnet-mvc-3.md) or [MVC Music Store](../../older-versions/mvc-music-store/mvc-music-store-part-1.md) tutorials.</span></span>


## <a name="nerddinner-step-6-viewdata-and-viewmodel"></a><span data-ttu-id="d96d3-109">NerdDinner 步驟 6： 別的 ViewData 和 ViewModel</span><span class="sxs-lookup"><span data-stu-id="d96d3-109">NerdDinner Step 6: ViewData and ViewModel</span></span>

<span data-ttu-id="d96d3-110">我們已涵蓋了數個表單張貼案例，並討論如何實作建立、 更新和刪除 (CRUD) 支援。</span><span class="sxs-lookup"><span data-stu-id="d96d3-110">We've covered a number of form post scenarios, and discussed how to implement create, update and delete (CRUD) support.</span></span> <span data-ttu-id="d96d3-111">我們現在將會採取進一步的 DinnersController 實作，並啟用更豐富格式編輯案例的支援。</span><span class="sxs-lookup"><span data-stu-id="d96d3-111">We'll now take our DinnersController implementation further and enable support for richer form editing scenarios.</span></span> <span data-ttu-id="d96d3-112">執行此動作時，我們將討論可用來將資料從控制器傳遞至檢視的兩種方法： 別的 ViewData 和 ViewModel。</span><span class="sxs-lookup"><span data-stu-id="d96d3-112">While doing this we'll discuss two approaches that can be used to pass data from controllers to views: ViewData and ViewModel.</span></span>

### <a name="passing-data-from-controllers-to-view-templates"></a><span data-ttu-id="d96d3-113">將資料從控制器傳遞至檢視範本</span><span class="sxs-lookup"><span data-stu-id="d96d3-113">Passing Data from Controllers to View-Templates</span></span>

<span data-ttu-id="d96d3-114">MVC 模式的定義特性的其中一個是 strict"的重要性分離 」 可協助應用程式的不同元件之間強制執行。</span><span class="sxs-lookup"><span data-stu-id="d96d3-114">One of the defining characteristics of the MVC pattern is the strict "separation of concerns" it helps enforce between the different components of an application.</span></span> <span data-ttu-id="d96d3-115">模型、 控制器和檢視每個已正確定義的角色和責任，並以妥善定義的方式彼此通訊。</span><span class="sxs-lookup"><span data-stu-id="d96d3-115">Models, Controllers and Views each have well defined roles and responsibilities, and they communicate amongst each other in well defined ways.</span></span> <span data-ttu-id="d96d3-116">這有助於升級可測試性並重複使用程式碼。</span><span class="sxs-lookup"><span data-stu-id="d96d3-116">This helps promote testability and code reuse.</span></span>

<span data-ttu-id="d96d3-117">當控制器類別會決定要呈現的 HTML 回應傳回給用戶端時，它會負責明確傳遞至檢視範本的所有資料需要回應。</span><span class="sxs-lookup"><span data-stu-id="d96d3-117">When a Controller class decides to render an HTML response back to a client, it is responsible for explicitly passing to the view template all of the data needed to render the response.</span></span> <span data-ttu-id="d96d3-118">檢視範本應該永遠不會執行任何的資料擷取或應用程式邏輯 –，而且應該改為限制只會有驅動開控制器所收到的模型/資料的轉譯程式碼本身。</span><span class="sxs-lookup"><span data-stu-id="d96d3-118">View templates should never perform any data retrieval or application logic – and should instead limit themselves to only have rendering code that is driven off of the model/data passed to it by the controller.</span></span>

<span data-ttu-id="d96d3-119">現在模型的資料傳入我們 DinnersController 我們檢視範本的類別是簡單明瞭 – index （），在 Dinner 物件的清單以及單一 Dinner 物件在 Details()、 Edit()、 create （） 和 delete 的情況下。</span><span class="sxs-lookup"><span data-stu-id="d96d3-119">Right now the model data being passed by our DinnersController class to our view templates is simple and straight-forward – a list of Dinner objects in the case of Index(), and a single Dinner object in the case of Details(), Edit(), Create() and Delete().</span></span> <span data-ttu-id="d96d3-120">因為我們將更多的 UI 功能加入至我們的應用程式時，我們通常會需要傳遞不只是這項資料來呈現 HTML 回應中我們檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d96d3-120">As we add more UI capabilities to our application, we are often going to need to pass more than just this data to render HTML responses within our view templates.</span></span> <span data-ttu-id="d96d3-121">比方說，我們可能會想要變更的 「 國家/地區 」 欄位中在我們的編輯，並防止 dropdownlist 以 HTML 文字方塊中建立檢視。</span><span class="sxs-lookup"><span data-stu-id="d96d3-121">For example, we might want to change the "Country" field within our Edit and Create views from being an HTML textbox to a dropdownlist.</span></span> <span data-ttu-id="d96d3-122">而不是硬式編碼檢視範本中的國家 （地區） 名稱的下拉式清單，我們可能會想要產生從支援國家 （地區），我們以動態方式填入的清單。</span><span class="sxs-lookup"><span data-stu-id="d96d3-122">Rather than hard-code the dropdown list of country names in the view template, we might want to generate it from a list of supported countries that we populate dynamically.</span></span> <span data-ttu-id="d96d3-123">我們將會需要能夠傳遞 Dinner 物件*和*支援國家/地區從 controller 我們檢視範本清單。</span><span class="sxs-lookup"><span data-stu-id="d96d3-123">We will need a way to pass both the Dinner object *and* the list of supported countries from our controller to our view templates.</span></span>

<span data-ttu-id="d96d3-124">讓我們看看我們可以完成這兩種方式。</span><span class="sxs-lookup"><span data-stu-id="d96d3-124">Let's look at two ways we can accomplish this.</span></span>

### <a name="using-the-viewdata-dictionary"></a><span data-ttu-id="d96d3-125">使用別的 ViewData 字典</span><span class="sxs-lookup"><span data-stu-id="d96d3-125">Using the ViewData Dictionary</span></span>

<span data-ttu-id="d96d3-126">控制器的基底類別會公開 「 別的 ViewData"字典屬性可以用來將其他資料項目控制站從傳遞至檢視。</span><span class="sxs-lookup"><span data-stu-id="d96d3-126">The Controller base class exposes a "ViewData" dictionary property that can be used to pass additional data items from Controllers to Views.</span></span>

<span data-ttu-id="d96d3-127">例如，若要支援的案例，我們要變更 「 國家/地區 」 文字方塊中，我們的編輯檢視中進行 dropdownlist 以 HTML 文字方塊中，我們可以更新來傳送電子郵件 （除了 Dinner 物件） SelectList 物件，可用來當作 m 我們 Edit() 動作方法odel 的國家/地區 dropdownlist。</span><span class="sxs-lookup"><span data-stu-id="d96d3-127">For example, to support the scenario where we want to change the "Country" textbox within our Edit view from being an HTML textbox to a dropdownlist, we can update our Edit() action method to pass (in addition to a Dinner object) a SelectList object that can be used as the model of a countries dropdownlist.</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample1.cs)]

<span data-ttu-id="d96d3-128">上述 SelectList 的建構函式可接受郡來填入下拉式 downlist，為目前選取的值的清單。</span><span class="sxs-lookup"><span data-stu-id="d96d3-128">The constructor of the SelectList above is accepting a list of counties to populate the drop-downlist with, as well as the currently selected value.</span></span>

<span data-ttu-id="d96d3-129">然後，我們可以更新使用 Html.DropDownList() helper 方法，而不是我們先前使用 Html.TextBox() helper 方法我們 Edit.aspx 檢視範本：</span><span class="sxs-lookup"><span data-stu-id="d96d3-129">We can then update our Edit.aspx view template to use the Html.DropDownList() helper method instead of the Html.TextBox() helper method we used previously:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample2.aspx)]

<span data-ttu-id="d96d3-130">上述的 Html.DropDownList() helper 方法會採用兩個參數。</span><span class="sxs-lookup"><span data-stu-id="d96d3-130">The Html.DropDownList() helper method above takes two parameters.</span></span> <span data-ttu-id="d96d3-131">第一個是輸出將 HTML 表單項目的名稱。</span><span class="sxs-lookup"><span data-stu-id="d96d3-131">The first is the name of the HTML form element to output.</span></span> <span data-ttu-id="d96d3-132">第二個是我們透過別的 ViewData 字典傳遞"SelectList 」 模型。</span><span class="sxs-lookup"><span data-stu-id="d96d3-132">The second is the "SelectList" model we passed via the ViewData dictionary.</span></span> <span data-ttu-id="d96d3-133">我們使用 C#"關鍵字 as"轉型為 SelectList 字典內的型別。</span><span class="sxs-lookup"><span data-stu-id="d96d3-133">We are using the C# "as" keyword to cast the type within the dictionary as a SelectList.</span></span>

<span data-ttu-id="d96d3-134">現在，當我們執行我們的應用程式，以及存取和 */Dinners/Edit/1* URL 我們瀏覽器中我們會看到我們編輯 UI，已更新以顯示國家/地區，而不是 textbox 的 dropdownlist:</span><span class="sxs-lookup"><span data-stu-id="d96d3-134">And now when we run our application and access the */Dinners/Edit/1* URL within our browser we'll see that our edit UI has been updated to display a dropdownlist of countries instead of a textbox:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image1.png)

<span data-ttu-id="d96d3-135">我們也會呈現編輯檢視範本，從編輯 HTTP POST 方法 （在案例中發生錯誤時），因為我們會想要確定我們也更新這個方法時可加入 SelectList 別的 ViewData 在錯誤狀況中呈現的檢視表範本：</span><span class="sxs-lookup"><span data-stu-id="d96d3-135">Because we also render the Edit view template from the HTTP-POST Edit method (in scenarios when errors occur), we'll want to make sure that we also update this method to add the SelectList to ViewData when the view template is rendered in error scenarios:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample3.cs)]

<span data-ttu-id="d96d3-136">和我們 DinnersController 編輯案例現在支援 DropDownList。</span><span class="sxs-lookup"><span data-stu-id="d96d3-136">And now our DinnersController edit scenario supports a DropDownList.</span></span>

### <a name="using-a-viewmodel-pattern"></a><span data-ttu-id="d96d3-137">使用 ViewModel 模式</span><span class="sxs-lookup"><span data-stu-id="d96d3-137">Using a ViewModel Pattern</span></span>

<span data-ttu-id="d96d3-138">別的 ViewData 字典方法有優點就是相當快速而輕鬆地實作。</span><span class="sxs-lookup"><span data-stu-id="d96d3-138">The ViewData dictionary approach has the benefit of being fairly fast and easy to implement.</span></span> <span data-ttu-id="d96d3-139">有些開發人員不喜歡使用字串為基礎的字典，不過，因為拼字錯誤可能會導致不會攔截在編譯時期的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d96d3-139">Some developers don't like using string-based dictionaries, though, since typos can lead to errors that will not be caught at compile-time.</span></span> <span data-ttu-id="d96d3-140">不具型別的別的 ViewData 字典也需要使用"為"運算子或轉換時使用強型別 C# 等語言中檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d96d3-140">The un-typed ViewData dictionary also requires using the "as" operator or casting when using a strongly-typed language like C# in a view template.</span></span>

<span data-ttu-id="d96d3-141">我們可以使用替代方法是其中一個通常稱為 「 ViewModel 」 模式。</span><span class="sxs-lookup"><span data-stu-id="d96d3-141">An alternative approach that we could use is one often referred to as the "ViewModel" pattern.</span></span> <span data-ttu-id="d96d3-142">使用此模式時我們建立強型別的類別，最佳化的特定檢視案例中，以及其公開之動態值/內容我們檢視範本所需的屬性。</span><span class="sxs-lookup"><span data-stu-id="d96d3-142">When using this pattern we create strongly-typed classes that are optimized for our specific view scenarios, and which expose properties for the dynamic values/content needed by our view templates.</span></span> <span data-ttu-id="d96d3-143">我們控制器類別可以填入並將這些檢視最佳化類別傳遞給我們要使用的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d96d3-143">Our controller classes can then populate and pass these view-optimized classes to our view template to use.</span></span> <span data-ttu-id="d96d3-144">這可讓類型安全、 編譯時期檢查，以及編輯器 intellisense 中檢視範本。</span><span class="sxs-lookup"><span data-stu-id="d96d3-144">This enables type-safety, compile-time checking, and editor intellisense within view templates.</span></span>

<span data-ttu-id="d96d3-145">例如，若要啟用編輯案例，我們可以建立 「 DinnerFormViewModel"像下面類別會公開兩個強型別屬性 dinner 表單： Dinner 物件，並填入國家 （地區) dropdownlist 需 SelectList 模型：</span><span class="sxs-lookup"><span data-stu-id="d96d3-145">For example, to enable dinner form editing scenarios we can create a "DinnerFormViewModel" class like below that exposes two strongly-typed properties: a Dinner object, and the SelectList model needed to populate the countries dropdownlist:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample4.cs)]

<span data-ttu-id="d96d3-146">我們可以然後更新 若要建立使用 Dinner 物件我們從我們的儲存機制擷取 DinnerFormViewModel 我們 Edit() 動作方法，並將它傳遞至我們檢視範本：</span><span class="sxs-lookup"><span data-stu-id="d96d3-146">We can then update our Edit() action method to create the DinnerFormViewModel using the Dinner object we retrieve from our repository, and then pass it to our view template:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample5.cs)]

<span data-ttu-id="d96d3-147">我們會接著我們檢視範本，因此它必須要有 「 DinnerFormViewModel"而不是 「 Dinner"物件"inherits"屬性變更 edit.aspx 頁面頂端的更新，就像這樣：</span><span class="sxs-lookup"><span data-stu-id="d96d3-147">We'll then update our view template so that it expects a "DinnerFormViewModel" instead of a "Dinner" object by changing the "inherits" attribute at the top of the edit.aspx page like so:</span></span>

[!code-cshtml[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample6.cshtml)]

<span data-ttu-id="d96d3-148">一旦我們這樣做時，「 模型 」 屬性，我們檢視範本中的 intellisense 將會更新以反映我們傳遞 DinnerFormViewModel 類型的物件模型：</span><span class="sxs-lookup"><span data-stu-id="d96d3-148">Once we do this, the intellisense of the "Model" property within our view template will be updated to reflect the object model of the DinnerFormViewModel type we are passing it:</span></span>

![](use-viewdata-and-implement-viewmodel-classes/_static/image2.png)

![](use-viewdata-and-implement-viewmodel-classes/_static/image3.png)

<span data-ttu-id="d96d3-149">然後，我們可以更新檢視程式碼中使用它的索引。</span><span class="sxs-lookup"><span data-stu-id="d96d3-149">We can then update our view code to work off of it.</span></span> <span data-ttu-id="d96d3-150">請注意以下方式我們並不會變更輸入項目的名稱，我們會建立 （form 項目將仍被命名為"Title"，「 國家/地區 」） – 但我們正在更新的 HTML Helper 方法來擷取使用 DinnerFormViewModel 類別的值：</span><span class="sxs-lookup"><span data-stu-id="d96d3-150">Notice below how we are not changing the names of the input elements we are creating (the form elements will still be named "Title", "Country") – but we are updating the HTML Helper methods to retrieve the values using the DinnerFormViewModel class:</span></span>

[!code-aspx[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample7.aspx)]

<span data-ttu-id="d96d3-151">我們也會將更新的轉譯錯誤時，使用 DinnerFormViewModel 類別我們編輯 post 方法：</span><span class="sxs-lookup"><span data-stu-id="d96d3-151">We'll also update our Edit post method to use the DinnerFormViewModel class when rendering errors:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample8.cs)]

<span data-ttu-id="d96d3-152">我們也可以更新重複使用的確切我們 create （） 動作方法相同*DinnerFormViewModel*類別，讓的國家/地區 DropDownList 內那些以及。</span><span class="sxs-lookup"><span data-stu-id="d96d3-152">We can also update our Create() action methods to re-use the exact same *DinnerFormViewModel* class to enable the countries DropDownList within those as well.</span></span> <span data-ttu-id="d96d3-153">以下是 HTTP GET 實作：</span><span class="sxs-lookup"><span data-stu-id="d96d3-153">Below is the HTTP-GET implementation:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample9.cs)]

<span data-ttu-id="d96d3-154">以下是建立 HTTP POST 方法的實作：</span><span class="sxs-lookup"><span data-stu-id="d96d3-154">Below is the implementation of the HTTP-POST Create method:</span></span>

[!code-csharp[Main](use-viewdata-and-implement-viewmodel-classes/samples/sample10.cs)]

<span data-ttu-id="d96d3-155">以及我們編輯和建立的畫面現在支援拖放在此選擇國家/地區。</span><span class="sxs-lookup"><span data-stu-id="d96d3-155">And now both our Edit and Create screens support drop-downlists for picking the country.</span></span>

### <a name="custom-shaped-viewmodel-classes"></a><span data-ttu-id="d96d3-156">自訂形狀 ViewModel 類別</span><span class="sxs-lookup"><span data-stu-id="d96d3-156">Custom-shaped ViewModel classes</span></span>

<span data-ttu-id="d96d3-157">在上述案例中，我們 DinnerFormViewModel 類別直接公開 Dinner 模型物件為屬性，以及支援 SelectList 模型屬性。</span><span class="sxs-lookup"><span data-stu-id="d96d3-157">In the scenario above, our DinnerFormViewModel class directly exposes the Dinner model object as a property, along with a supporting SelectList model property.</span></span> <span data-ttu-id="d96d3-158">這種方法其中我們想要建立我們檢視範本的 HTML UI 對應相當接近我們網域模型物件的情況下運作正常。</span><span class="sxs-lookup"><span data-stu-id="d96d3-158">This approach works fine for scenarios where the HTML UI we want to create within our view template corresponds relatively closely to our domain model objects.</span></span>

<span data-ttu-id="d96d3-159">這不是其中的案例的情況下，您可以使用的其中一個選項是建立自訂形狀 ViewModel 類別的物件模型更適合耗用量檢視 – 所且這看起來可能完全不同的基礎網域模型物件。</span><span class="sxs-lookup"><span data-stu-id="d96d3-159">For scenarios where this isn't the case, one option that you can use is to create a custom-shaped ViewModel class whose object model is more optimized for consumption by the view – and which might look completely different from the underlying domain model object.</span></span> <span data-ttu-id="d96d3-160">例如，它可能無法公開不同的屬性名稱和 （或） 從多個模型物件收集到的彙總屬性。</span><span class="sxs-lookup"><span data-stu-id="d96d3-160">For example, it could potentially expose different property names and/or aggregate properties collected from multiple model objects.</span></span>

<span data-ttu-id="d96d3-161">您可以為自訂形狀 ViewModel 類別用於兩者中控制站的資料傳遞至轉譯，以及協助處理回傳至控制器動作方法的表單資料的檢視。</span><span class="sxs-lookup"><span data-stu-id="d96d3-161">Custom-shaped ViewModel classes can be used both to pass data from controllers to views to render, as well as to help handle form data posted back to a controller's action method.</span></span> <span data-ttu-id="d96d3-162">此更新的案例中，您可能必須表單張貼的資料，更新的 ViewModel 物件，然後使用 ViewModel 執行個體對應，或擷取實際的網域模型物件的動作方法。</span><span class="sxs-lookup"><span data-stu-id="d96d3-162">For this later scenario, you might have the action method update a ViewModel object with the form-posted data, and then use the ViewModel instance to map or retrieve an actual domain model object.</span></span>

<span data-ttu-id="d96d3-163">自訂形狀 ViewModel 類別可以提供極大的彈性，而且調查隨時檢視範本或表單張貼程式碼中尋找轉譯程式碼在您開始得太複雜的動作方法內的項目。</span><span class="sxs-lookup"><span data-stu-id="d96d3-163">Custom-shaped ViewModel classes can provide a great deal of flexibility, and are something to investigate any time you find the rendering code within your view templates or the form-posting code inside your action methods starting to get too complicated.</span></span> <span data-ttu-id="d96d3-164">這通常是正負號的網域模型不完全地對應至您產生的 UI 中繼自訂形狀 ViewModel 類別可以幫助。</span><span class="sxs-lookup"><span data-stu-id="d96d3-164">This is often a sign that your domain models don't cleanly correspond to the UI you are generating, and that an intermediate custom-shaped ViewModel class can help.</span></span>

### <a name="next-step"></a><span data-ttu-id="d96d3-165">下一個步驟</span><span class="sxs-lookup"><span data-stu-id="d96d3-165">Next Step</span></span>

<span data-ttu-id="d96d3-166">讓我們現在看看如何我們可以使用 partials 和主版頁面來重複使用，並在我們的應用程式之間共用 UI。</span><span class="sxs-lookup"><span data-stu-id="d96d3-166">Let's now look at how we can use partials and master-pages to re-use and share UI across our application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="d96d3-167">[上一頁](provide-crud-create-read-update-delete-data-form-entry-support.md)
> [下一頁](re-use-ui-using-master-pages-and-partials.md)</span><span class="sxs-lookup"><span data-stu-id="d96d3-167">[Previous](provide-crud-create-read-update-delete-data-form-entry-support.md)
[Next](re-use-ui-using-master-pages-and-partials.md)</span></span>
