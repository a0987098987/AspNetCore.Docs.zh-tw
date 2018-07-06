---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: 將驗證加入至模型 (C#) |Microsoft Docs
author: Rick-Anderson
description: 建立控制器
ms.author: aspnetcontent
ms.date: 01/12/2011
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: fc5c3d2afcb6fa5b1983de1d09476a2a1e09d302
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37804448"
---
<a name="adding-validation-to-the-model-c"></a><span data-ttu-id="06f0d-103">將驗證加入至模型 (C#)</span><span class="sxs-lookup"><span data-stu-id="06f0d-103">Adding Validation to the Model (C#)</span></span>
====================
<span data-ttu-id="06f0d-104">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="06f0d-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="06f0d-105">本教學課程中的更新的版本可[此處](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="06f0d-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="06f0d-106">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="06f0d-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="06f0d-107">本教學課程將教導您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，也就是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="06f0d-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="06f0d-108">在開始之前，請確定您已安裝符合下列先決條件。</span><span class="sxs-lookup"><span data-stu-id="06f0d-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="06f0d-109">您可以安裝所有人都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="06f0d-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="06f0d-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="06f0d-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="06f0d-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="06f0d-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="06f0d-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="06f0d-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="06f0d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="06f0d-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="06f0d-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，請按一下下列連結安裝必要的： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="06f0d-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="06f0d-115">使用本主題隨附了 C# 原始程式碼的 Visual Web Developer 專案。</span><span class="sxs-lookup"><span data-stu-id="06f0d-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="06f0d-116">[下載 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="06f0d-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="06f0d-117">如果您偏好 Visual Basic，切換到[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="06f0d-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="06f0d-118">在本節中，您會將驗證邏輯加入`Movie`模型，而您將確保的每當使用者嘗試建立或編輯電影，使用應用程式執行驗證規則。</span><span class="sxs-lookup"><span data-stu-id="06f0d-118">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="06f0d-119">項目保持 DRY</span><span class="sxs-lookup"><span data-stu-id="06f0d-119">Keeping Things DRY</span></span>

<span data-ttu-id="06f0d-120">ASP.NET mvc 的核心設計原則之一是 DRY （「 不自行重複 」）。</span><span class="sxs-lookup"><span data-stu-id="06f0d-120">One of the core design tenets of ASP.NET MVC is DRY ("Don't Repeat Yourself").</span></span> <span data-ttu-id="06f0d-121">ASP.NET MVC 鼓勵您一次，指定功能或行為，然後讓它會反映在應用程式中的所有位置。</span><span class="sxs-lookup"><span data-stu-id="06f0d-121">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="06f0d-122">這可減少您需要撰寫的程式碼數量，並可讓您維護更容易撰寫的程式碼。</span><span class="sxs-lookup"><span data-stu-id="06f0d-122">This reduces the amount of code you need to write and makes the code you do write much easier to maintain.</span></span>

<span data-ttu-id="06f0d-123">ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援就是執行 DRY 準則的動作的絕佳範例。</span><span class="sxs-lookup"><span data-stu-id="06f0d-123">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="06f0d-124">您可以以宣告方式指定在單一位置 （在模型類別中） 中的驗證規則，然後這些規則是任何位置強制執行應用程式中。</span><span class="sxs-lookup"><span data-stu-id="06f0d-124">You can declaratively specify validation rules in one place (in the model class) and then those rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="06f0d-125">讓我們看看如何您可以利用這項驗證支援的電影應用程式中。</span><span class="sxs-lookup"><span data-stu-id="06f0d-125">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="06f0d-126">將驗證規則新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="06f0d-126">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="06f0d-127">一開始要先新增一些驗證邏輯，以`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="06f0d-127">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="06f0d-128">開啟 *Movie.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="06f0d-128">Open the *Movie.cs* file.</span></span> <span data-ttu-id="06f0d-129">新增`using`陳述式參考的檔案頂端[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間：</span><span class="sxs-lookup"><span data-stu-id="06f0d-129">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="06f0d-130">命名空間是.NET Framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="06f0d-130">The namespace is part of the .NET Framework.</span></span> <span data-ttu-id="06f0d-131">它提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-131">It provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="06f0d-132">現在更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，並[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性.</span><span class="sxs-lookup"><span data-stu-id="06f0d-132">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="06f0d-133">使用下列程式碼作為範例，要套用屬性的位置。</span><span class="sxs-lookup"><span data-stu-id="06f0d-133">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

<span data-ttu-id="06f0d-134">驗證屬性會指定您想要強制執行模型屬性套用的行為。</span><span class="sxs-lookup"><span data-stu-id="06f0d-134">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="06f0d-135">`Required`屬性會指出屬性必須有值; 在此範例中，電影必須具有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。</span><span class="sxs-lookup"><span data-stu-id="06f0d-135">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="06f0d-136">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="06f0d-136">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="06f0d-137">`StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。</span><span class="sxs-lookup"><span data-stu-id="06f0d-137">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>

<span data-ttu-id="06f0d-138">程式碼會先確認您的模型類別指定的驗證規則會強制執行，才能應用程式會將變更儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="06f0d-138">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="06f0d-139">例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值遺漏，且價格為零 （這是超出有效範圍）。</span><span class="sxs-lookup"><span data-stu-id="06f0d-139">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

<span data-ttu-id="06f0d-140">具有自動強制執行的.NET Framework 的驗證規則有助於讓您的應用程式更穩固。</span><span class="sxs-lookup"><span data-stu-id="06f0d-140">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="06f0d-141">它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。</span><span class="sxs-lookup"><span data-stu-id="06f0d-141">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="06f0d-142">以下是完整的程式碼，更新清單*Movie.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="06f0d-142">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="06f0d-143">驗證錯誤 ASP.NET MVC 中的 UI</span><span class="sxs-lookup"><span data-stu-id="06f0d-143">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="06f0d-144">重新執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="06f0d-144">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="06f0d-145">按一下 **建立的電影**連結，可新增一部新電影。</span><span class="sxs-lookup"><span data-stu-id="06f0d-145">Click the **Create Movie** link to add a new movie.</span></span> <span data-ttu-id="06f0d-146">填妥表單中使用某些無效的值，然後按一下 [**建立**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="06f0d-146">Fill out the form with some invalid values and then click the **Create** button.</span></span>

<span data-ttu-id="06f0d-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="06f0d-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span></span>

<span data-ttu-id="06f0d-148">請注意如何表單已自動使用背景色彩來反白顯示的文字方塊，其中包含無效的資料，並發出每一個旁適當的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="06f0d-148">Notice how the form has automatically used a background color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="06f0d-149">錯誤訊息符合您指定當您的註解的錯誤字串`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="06f0d-149">The error messages match the error strings you specified when you annotated the `Movie` class.</span></span> <span data-ttu-id="06f0d-150">錯誤會強制執行 （使用 JavaScript） 用戶端和伺服器端 （如果使用者已停用 JavaScript 時）。</span><span class="sxs-lookup"><span data-stu-id="06f0d-150">The errors are enforced both client-side (using JavaScript) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="06f0d-151">實質的好處是，您不需要變更任何程式中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。</span><span class="sxs-lookup"><span data-stu-id="06f0d-151">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="06f0d-152">控制器和檢視您稍早在本教學課程中自動建立拾取驗證規則，您指定的資料上使用屬性`Movie`模型類別。</span><span class="sxs-lookup"><span data-stu-id="06f0d-152">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified using attributes on the `Movie` model class.</span></span>

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="06f0d-153">如何驗證發生在 建立檢視，以及建立動作方法</span><span class="sxs-lookup"><span data-stu-id="06f0d-153">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="06f0d-154">您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。</span><span class="sxs-lookup"><span data-stu-id="06f0d-154">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="06f0d-155">下一步 的清單顯示什麼`Create`中的方法`MovieController`類別看起來像。</span><span class="sxs-lookup"><span data-stu-id="06f0d-155">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="06f0d-156">它們與您建立的方式加以稍早在本教學課程中相同。</span><span class="sxs-lookup"><span data-stu-id="06f0d-156">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

<span data-ttu-id="06f0d-157">第一個動作方法會顯示初始建立表單。</span><span class="sxs-lookup"><span data-stu-id="06f0d-157">The first action method displays the initial Create form.</span></span> <span data-ttu-id="06f0d-158">第二個處理表單張貼。</span><span class="sxs-lookup"><span data-stu-id="06f0d-158">The second handles the form post.</span></span> <span data-ttu-id="06f0d-159">第二個`Create`方法呼叫`ModelState.IsValid`檢查電影是否有任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="06f0d-159">The second `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="06f0d-160">呼叫此方法會評估已套用至物件的所有驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-160">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="06f0d-161">如果物件有驗證錯誤`Create`方法會重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="06f0d-161">If the object has validation errors, the `Create` method redisplays the form.</span></span> <span data-ttu-id="06f0d-162">如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="06f0d-162">If there are no errors, the method saves the new movie in the database.</span></span>

<span data-ttu-id="06f0d-163">以下是*Create.cshtml*您稍早在本教學課程中包含 scaffold 的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="06f0d-163">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="06f0d-164">上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。</span><span class="sxs-lookup"><span data-stu-id="06f0d-164">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="06f0d-165">請注意程式碼的使用方式`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-165">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="06f0d-166">此協助程式旁會呼叫`Html.ValidationMessageFor`helper 方法。</span><span class="sxs-lookup"><span data-stu-id="06f0d-166">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="06f0d-167">這兩個 helper 方法會使用控制器傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。</span><span class="sxs-lookup"><span data-stu-id="06f0d-167">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="06f0d-168">它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-168">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="06f0d-169">什麼是這種方法的優點在於控制器和 Create 檢視範本都不知道的任何項目顯示的特定錯誤訊息或強制執行的實際驗證規則相關。</span><span class="sxs-lookup"><span data-stu-id="06f0d-169">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="06f0d-170">驗證規則和錯誤字串只在 `Movie` 類別中指定。</span><span class="sxs-lookup"><span data-stu-id="06f0d-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span>

<span data-ttu-id="06f0d-171">如果您想要稍後變更驗證邏輯，則可以只在一個位置。</span><span class="sxs-lookup"><span data-stu-id="06f0d-171">If you want to change the validation logic later, you can do so in exactly one place.</span></span> <span data-ttu-id="06f0d-172">您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。</span><span class="sxs-lookup"><span data-stu-id="06f0d-172">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="06f0d-173">這會讓程式碼非常整齊乾淨，容易維護及發展。</span><span class="sxs-lookup"><span data-stu-id="06f0d-173">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="06f0d-174">這表示您會完全接受 DRY 原則。</span><span class="sxs-lookup"><span data-stu-id="06f0d-174">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="06f0d-175">加入至電影模型格式設定</span><span class="sxs-lookup"><span data-stu-id="06f0d-175">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="06f0d-176">開啟 *Movie.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="06f0d-176">Open the *Movie.cs* file.</span></span> <span data-ttu-id="06f0d-177">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間會提供除了一組內建的驗證屬性的格式化屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-177">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="06f0d-178">您將會套用[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性並[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。</span><span class="sxs-lookup"><span data-stu-id="06f0d-178">You'll apply the [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute and a [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="06f0d-179">下列程式碼示範`ReleaseDate`並`Price`屬性以適當[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="06f0d-179">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

<span data-ttu-id="06f0d-180">或者，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)值。</span><span class="sxs-lookup"><span data-stu-id="06f0d-180">Alternatively, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="06f0d-181">下列程式碼顯示以日期格式字串的發行日期屬性 (也就是"d")。</span><span class="sxs-lookup"><span data-stu-id="06f0d-181">The following code shows the release date property with a date format string (namely, "d").</span></span> <span data-ttu-id="06f0d-182">您會使用此對話方塊來指定您不想要時間發行日期的一部分。</span><span class="sxs-lookup"><span data-stu-id="06f0d-182">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

<span data-ttu-id="06f0d-183">下列程式碼格式`Price`屬性做為貨幣。</span><span class="sxs-lookup"><span data-stu-id="06f0d-183">The following code formats the `Price` property as currency.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="06f0d-184">完整`Movie`類別如下所示。</span><span class="sxs-lookup"><span data-stu-id="06f0d-184">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="06f0d-185">執行應用程式，並瀏覽至`Movies`控制站。</span><span class="sxs-lookup"><span data-stu-id="06f0d-185">Run the application and browse to the `Movies` controller.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

<span data-ttu-id="06f0d-187">在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。</span><span class="sxs-lookup"><span data-stu-id="06f0d-187">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="06f0d-188">[上一頁](adding-a-new-field.md)
> [下一頁](improving-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="06f0d-188">[Previous](adding-a-new-field.md)
[Next](improving-the-details-and-delete-methods.md)</span></span>
