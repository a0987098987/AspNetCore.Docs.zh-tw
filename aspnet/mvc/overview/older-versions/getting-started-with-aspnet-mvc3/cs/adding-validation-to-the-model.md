---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
title: "將驗證加入至模型 (C#) |Microsoft 文件"
author: Rick-Anderson
description: "建立控制站"
ms.author: aspnetcontent
manager: wpickett
ms.date: 01/12/2011
ms.topic: article
ms.assetid: 9af927e2-1c3b-43d9-917d-1df75be3c482
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc3/cs/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: a1d6a6468a39f31c3af8779abbbced093288773c
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="adding-validation-to-the-model-c"></a><span data-ttu-id="0c678-103">將驗證加入至模型 (C#)</span><span class="sxs-lookup"><span data-stu-id="0c678-103">Adding Validation to the Model (C#)</span></span>
====================
<span data-ttu-id="0c678-104">由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="0c678-104">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="0c678-105">本教學課程的更新的版本時使用[這裡](../../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="0c678-105">An updated version of this tutorial is available [here](../../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="0c678-106">它更安全、 容易遵循，及示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="0c678-106">It's more secure, much simpler to follow and demonstrates more features.</span></span>
> 
> 
> <span data-ttu-id="0c678-107">本教學課程將告訴您建置使用 Microsoft Visual Web Developer 2010 Express Service Pack 1，這是 Microsoft Visual Studio 的免費版本的 ASP.NET MVC Web 應用程式的基本概念。</span><span class="sxs-lookup"><span data-stu-id="0c678-107">This tutorial will teach you the basics of building an ASP.NET MVC Web application using Microsoft Visual Web Developer 2010 Express Service Pack 1, which is a free version of Microsoft Visual Studio.</span></span> <span data-ttu-id="0c678-108">開始之前，請確定您已安裝下面所列的必要條件。</span><span class="sxs-lookup"><span data-stu-id="0c678-108">Before you start, make sure you've installed the prerequisites listed below.</span></span> <span data-ttu-id="0c678-109">您可以安裝全部都按下列連結： [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="0c678-109">You can install all of them by clicking the following link: [Web Platform Installer](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack).</span></span> <span data-ttu-id="0c678-110">或者，您可以個別安裝的必要條件，使用下列連結：</span><span class="sxs-lookup"><span data-stu-id="0c678-110">Alternatively, you can individually install the prerequisites using the following links:</span></span>
> 
> - [<span data-ttu-id="0c678-111">Visual Studio Web Developer Express SP1 必要條件</span><span class="sxs-lookup"><span data-stu-id="0c678-111">Visual Studio Web Developer Express SP1 prerequisites</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appid=VWD2010SP1Pack)
> - [<span data-ttu-id="0c678-112">ASP.NET MVC 3 Tools Update</span><span class="sxs-lookup"><span data-stu-id="0c678-112">ASP.NET MVC 3 Tools Update</span></span>](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=MVC3)
> - <span data-ttu-id="0c678-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)（執行階段 + 工具支援）</span><span class="sxs-lookup"><span data-stu-id="0c678-113">[SQL Server Compact 4.0](https://www.microsoft.com/web/gallery/install.aspx?appid=SQLCE;SQLCEVSTools_4_0)(runtime + tools support)</span></span>
> 
> <span data-ttu-id="0c678-114">如果您使用 Visual Studio 2010 而不 Visual Web Developer 2010，安裝必要元件，請按一下下列連結： [Visual Studio 2010 必要條件](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack)。</span><span class="sxs-lookup"><span data-stu-id="0c678-114">If you're using Visual Studio 2010 instead of Visual Web Developer 2010, install the prerequisites by clicking the following link: [Visual Studio 2010 prerequisites](https://www.microsoft.com/web/gallery/install.aspx?appsxml=&amp;appid=VS2010SP1Pack).</span></span>
> 
> <span data-ttu-id="0c678-115">使用本主題隨附在 Visual Web Developer 專案中的使用 C# 原始程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c678-115">A Visual Web Developer project with C# source code is available to accompany this topic.</span></span> <span data-ttu-id="0c678-116">[下載的 C# 版本](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098)。</span><span class="sxs-lookup"><span data-stu-id="0c678-116">[Download the C# version](https://code.msdn.microsoft.com/Introduction-to-MVC-3-10d1b098).</span></span> <span data-ttu-id="0c678-117">如果您偏好 Visual Basic，切換至[Visual Basic 版本](../vb/intro-to-aspnet-mvc-3.md)本教學課程。</span><span class="sxs-lookup"><span data-stu-id="0c678-117">If you prefer Visual Basic, switch to the [Visual Basic version](../vb/intro-to-aspnet-mvc-3.md) of this tutorial.</span></span>


<span data-ttu-id="0c678-118">這一節中，您會將驗證邏輯加入`Movie`模型，而您要確保驗證規則會強制執行任何使用者嘗試建立或編輯電影使用應用程式的時間。</span><span class="sxs-lookup"><span data-stu-id="0c678-118">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="0c678-119">保留項目乾</span><span class="sxs-lookup"><span data-stu-id="0c678-119">Keeping Things DRY</span></span>

<span data-ttu-id="0c678-120">其中一個核心設計原則，ASP.NET mvc 是乾 （「 不重複自行"）。</span><span class="sxs-lookup"><span data-stu-id="0c678-120">One of the core design tenets of ASP.NET MVC is DRY ("Don't Repeat Yourself").</span></span> <span data-ttu-id="0c678-121">ASP.NET MVC 鼓勵您一次，指定的功能或行為，然後讓它 everywhere 反映應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0c678-121">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="0c678-122">這可減少您需要撰寫程式碼數量，並讓您更容易撰寫維護的程式碼。</span><span class="sxs-lookup"><span data-stu-id="0c678-122">This reduces the amount of code you need to write and makes the code you do write much easier to maintain.</span></span>

<span data-ttu-id="0c678-123">ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援是在動作中的乾主體的絕佳範例。</span><span class="sxs-lookup"><span data-stu-id="0c678-123">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="0c678-124">您可以以宣告方式指定在單一位置 （以模型類別） 的驗證規則，然後這些規則會強制執行所有應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0c678-124">You can declaratively specify validation rules in one place (in the model class) and then those rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="0c678-125">讓我們看看如何您可以利用這項驗證支援的電影應用程式中。</span><span class="sxs-lookup"><span data-stu-id="0c678-125">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="0c678-126">將驗證規則加入至電影模型</span><span class="sxs-lookup"><span data-stu-id="0c678-126">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="0c678-127">藉由新增一些驗證邏輯，以開始，您將`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="0c678-127">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="0c678-128">開啟 *Movie.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0c678-128">Open the *Movie.cs* file.</span></span> <span data-ttu-id="0c678-129">新增`using`在參考的檔案最上方的陳述式[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間：</span><span class="sxs-lookup"><span data-stu-id="0c678-129">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="0c678-130">命名空間是.NET Framework 的一部分。</span><span class="sxs-lookup"><span data-stu-id="0c678-130">The namespace is part of the .NET Framework.</span></span> <span data-ttu-id="0c678-131">它提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-131">It provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="0c678-132">立即更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，和[ `Range` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性.</span><span class="sxs-lookup"><span data-stu-id="0c678-132">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="0c678-133">使用下列程式碼做為要套用屬性的範例。</span><span class="sxs-lookup"><span data-stu-id="0c678-133">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs)]

<span data-ttu-id="0c678-134">驗證屬性會指定您想要強制執行模型屬性套用的行為。</span><span class="sxs-lookup"><span data-stu-id="0c678-134">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="0c678-135">`Required`屬性會指出屬性必須有一個值; 在此範例中，影片必須有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。</span><span class="sxs-lookup"><span data-stu-id="0c678-135">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="0c678-136">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="0c678-136">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="0c678-137">`StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。</span><span class="sxs-lookup"><span data-stu-id="0c678-137">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>

<span data-ttu-id="0c678-138">程式碼第一次可確保您的模型類別指定的驗證規則會強制執行之前會先應用程式資料庫中儲存的變更。</span><span class="sxs-lookup"><span data-stu-id="0c678-138">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="0c678-139">例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值為遺失，價格為零 （亦即超出有效範圍）。</span><span class="sxs-lookup"><span data-stu-id="0c678-139">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample3.cs)]

<span data-ttu-id="0c678-140">需要.NET framework 自動強制執行的驗證規則可協助讓您的應用程式更穩固。</span><span class="sxs-lookup"><span data-stu-id="0c678-140">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="0c678-141">它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。</span><span class="sxs-lookup"><span data-stu-id="0c678-141">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="0c678-142">以下是完整的程式碼已更新*Movie.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="0c678-142">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="0c678-143">驗證錯誤 ASP.NET MVC 中的 UI</span><span class="sxs-lookup"><span data-stu-id="0c678-143">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="0c678-144">重新執行應用程式，並瀏覽至*/Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="0c678-144">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="0c678-145">按一下**建立電影**連結加入新的電影。</span><span class="sxs-lookup"><span data-stu-id="0c678-145">Click the **Create Movie** link to add a new movie.</span></span> <span data-ttu-id="0c678-146">填寫表單具有一些無效的值，然後按一下 [**建立**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="0c678-146">Fill out the form with some invalid values and then click the **Create** button.</span></span>

<span data-ttu-id="0c678-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="0c678-147">[![8_validationErrors](adding-validation-to-the-model/_static/image2.png)](adding-validation-to-the-model/_static/image1.png)</span></span>

<span data-ttu-id="0c678-148">請注意如何表單已自動使用背景色彩來反白顯示包含無效的資料，以及發出適當的驗證錯誤訊息，每個旁邊的文字方塊。</span><span class="sxs-lookup"><span data-stu-id="0c678-148">Notice how the form has automatically used a background color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="0c678-149">錯誤訊息會比對您指定當您的註解錯誤字串`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="0c678-149">The error messages match the error strings you specified when you annotated the `Movie` class.</span></span> <span data-ttu-id="0c678-150">錯誤會強制執行 （使用 JavaScript） 用戶端和伺服器端 （如果使用者已停用 JavaScript）。</span><span class="sxs-lookup"><span data-stu-id="0c678-150">The errors are enforced both client-side (using JavaScript) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="0c678-151">實際的好處是： 您不需要變更單一行中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。</span><span class="sxs-lookup"><span data-stu-id="0c678-151">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="0c678-152">控制站建立和檢視您稍早在本教學課程中會自動挑選驗證規則，您會指定使用屬性的總`Movie`模型類別。</span><span class="sxs-lookup"><span data-stu-id="0c678-152">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified using attributes on the `Movie` model class.</span></span>

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="0c678-153">如何驗證會在中建立檢視，並建立動作方法</span><span class="sxs-lookup"><span data-stu-id="0c678-153">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="0c678-154">您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。</span><span class="sxs-lookup"><span data-stu-id="0c678-154">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="0c678-155">下一步的清單顯示什麼`Create`方法`MovieController`類別看起來像。</span><span class="sxs-lookup"><span data-stu-id="0c678-155">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="0c678-156">變更就會維持與您建立的方式加以稍早在本教學課程。</span><span class="sxs-lookup"><span data-stu-id="0c678-156">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

<span data-ttu-id="0c678-157">第一個動作方法會顯示在初始建立表單。</span><span class="sxs-lookup"><span data-stu-id="0c678-157">The first action method displays the initial Create form.</span></span> <span data-ttu-id="0c678-158">第二個處理表單 post。</span><span class="sxs-lookup"><span data-stu-id="0c678-158">The second handles the form post.</span></span> <span data-ttu-id="0c678-159">第二個`Create`方法呼叫`ModelState.IsValid`，檢查電影是否有任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="0c678-159">The second `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="0c678-160">呼叫此方法會評估已套用至物件的所有驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-160">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="0c678-161">如果物件有驗證錯誤`Create`方法重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="0c678-161">If the object has validation errors, the `Create` method redisplays the form.</span></span> <span data-ttu-id="0c678-162">如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="0c678-162">If there are no errors, the method saves the new movie in the database.</span></span>

<span data-ttu-id="0c678-163">以下是*Create.cshtml*您稍早在本教學課程中建立結構的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="0c678-163">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="0c678-164">上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。</span><span class="sxs-lookup"><span data-stu-id="0c678-164">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="0c678-165">請注意程式碼如何使用`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-165">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="0c678-166">此協助程式旁是呼叫`Html.ValidationMessageFor`helper 方法。</span><span class="sxs-lookup"><span data-stu-id="0c678-166">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="0c678-167">這兩個 helper 方法會使用控制器的傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。</span><span class="sxs-lookup"><span data-stu-id="0c678-167">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="0c678-168">它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-168">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="0c678-169">這種方法的優點是，控制器和建立檢視表範本都不知道的任何項目有關強制執行的實際驗證規則或顯示的特定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="0c678-169">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="0c678-170">驗證規則和錯誤字串只在 `Movie` 類別中指定。</span><span class="sxs-lookup"><span data-stu-id="0c678-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span>

<span data-ttu-id="0c678-171">如果您想要稍後變更驗證邏輯，您可以在一個地方。</span><span class="sxs-lookup"><span data-stu-id="0c678-171">If you want to change the validation logic later, you can do so in exactly one place.</span></span> <span data-ttu-id="0c678-172">您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。</span><span class="sxs-lookup"><span data-stu-id="0c678-172">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="0c678-173">這會讓程式碼非常整齊乾淨，容易維護及發展。</span><span class="sxs-lookup"><span data-stu-id="0c678-173">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="0c678-174">這表示，您可以完全接受乾原則。</span><span class="sxs-lookup"><span data-stu-id="0c678-174">And it means that that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="0c678-175">加入影片模型格式設定</span><span class="sxs-lookup"><span data-stu-id="0c678-175">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="0c678-176">開啟 *Movie.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="0c678-176">Open the *Movie.cs* file.</span></span> <span data-ttu-id="0c678-177">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx)命名空間提供除了一組內建的驗證屬性的格式屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-177">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="0c678-178">您將套用[ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性和[ `DataType` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。</span><span class="sxs-lookup"><span data-stu-id="0c678-178">You'll apply the [`DisplayFormat`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute and a [`DataType`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="0c678-179">下列程式碼會示範`ReleaseDate`和`Price`具有適當的屬性[ `DisplayFormat` ](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="0c678-179">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/en-us/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs)]

<span data-ttu-id="0c678-180">或者，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/en-us/library/system.string.format.aspx)值。</span><span class="sxs-lookup"><span data-stu-id="0c678-180">Alternatively, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/en-us/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="0c678-181">下列程式碼將示範使用日期的格式字串的發行日期屬性 (也就是"d")。</span><span class="sxs-lookup"><span data-stu-id="0c678-181">The following code shows the release date property with a date format string (namely, "d").</span></span> <span data-ttu-id="0c678-182">您會使用這個指定不希望時間做為發行日期的一部分。</span><span class="sxs-lookup"><span data-stu-id="0c678-182">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample8.cs)]

<span data-ttu-id="0c678-183">下列程式碼格式`Price`屬性做為貨幣。</span><span class="sxs-lookup"><span data-stu-id="0c678-183">The following code formats the `Price` property as currency.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="0c678-184">完整`Movie`類別如下所示。</span><span class="sxs-lookup"><span data-stu-id="0c678-184">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="0c678-185">執行應用程式，並瀏覽至`Movies`控制站。</span><span class="sxs-lookup"><span data-stu-id="0c678-185">Run the application and browse to the `Movies` controller.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image3.png)

<span data-ttu-id="0c678-187">在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。</span><span class="sxs-lookup"><span data-stu-id="0c678-187">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

>[!div class="step-by-step"]
<span data-ttu-id="0c678-188">[上一頁](adding-a-new-field.md)
[下一頁](improving-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="0c678-188">[Previous](adding-a-new-field.md)
[Next](improving-the-details-and-delete-methods.md)</span></span>
