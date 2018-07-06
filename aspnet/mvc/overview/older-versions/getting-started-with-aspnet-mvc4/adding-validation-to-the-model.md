---
uid: mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
title: 將驗證加入至模型 |Microsoft Docs
author: Rick-Anderson
description: 注意： 本教學課程中的更新的版本就可以使用這裡使用 ASP.NET MVC 5 和 Visual Studio 2013。 這是更安全、 更容易遵循，並示範...
ms.author: aspnetcontent
ms.date: 08/28/2012
ms.assetid: 5d9a2999-fcc4-4c45-a018-271fddf74a3b
msc.legacyurl: /mvc/overview/older-versions/getting-started-with-aspnet-mvc4/adding-validation-to-the-model
msc.type: authoredcontent
ms.openlocfilehash: 50911920ed80f09033cfc53a137a6bbba3fc62f3
ms.sourcegitcommit: b28cd0313af316c051c2ff8549865bff67f2fbb4
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/05/2018
ms.locfileid: "37834487"
---
<a name="adding-validation-to-the-model"></a><span data-ttu-id="25efb-104">將驗證加入至模型</span><span class="sxs-lookup"><span data-stu-id="25efb-104">Adding Validation to the Model</span></span>
====================
<span data-ttu-id="25efb-105">藉由[Rick Anderson](https://github.com/Rick-Anderson)</span><span class="sxs-lookup"><span data-stu-id="25efb-105">by [Rick Anderson](https://github.com/Rick-Anderson)</span></span>

> > [!NOTE]
> > <span data-ttu-id="25efb-106">本教學課程中的更新的版本可[此處](../../getting-started/introduction/getting-started.md)使用 ASP.NET MVC 5 和 Visual Studio 2013。</span><span class="sxs-lookup"><span data-stu-id="25efb-106">An updated version of this tutorial is available [here](../../getting-started/introduction/getting-started.md) that uses ASP.NET MVC 5 and Visual Studio 2013.</span></span> <span data-ttu-id="25efb-107">它更安全、 更容易遵循，並示範更多的功能。</span><span class="sxs-lookup"><span data-stu-id="25efb-107">It's more secure, much simpler to follow and demonstrates more features.</span></span>


<span data-ttu-id="25efb-108">在本節中，您會將驗證邏輯加入`Movie`模型，而您將確保的每當使用者嘗試建立或編輯電影，使用應用程式執行驗證規則。</span><span class="sxs-lookup"><span data-stu-id="25efb-108">In this section you'll add validation logic to the `Movie` model, and you'll ensure that the validation rules are enforced any time a user attempts to create or edit a movie using the application.</span></span>

## <a name="keeping-things-dry"></a><span data-ttu-id="25efb-109">項目保持 DRY</span><span class="sxs-lookup"><span data-stu-id="25efb-109">Keeping Things DRY</span></span>

<span data-ttu-id="25efb-110">ASP.NET mvc 的核心設計原則之一，是 DRY (&quot;Don't Repeat Yourself&quot;)。</span><span class="sxs-lookup"><span data-stu-id="25efb-110">One of the core design tenets of ASP.NET MVC is DRY (&quot;Don't Repeat Yourself&quot;).</span></span> <span data-ttu-id="25efb-111">ASP.NET MVC 鼓勵您一次，指定功能或行為，然後讓它會反映在應用程式中的所有位置。</span><span class="sxs-lookup"><span data-stu-id="25efb-111">ASP.NET MVC encourages you to specify functionality or behavior only once, and then have it be reflected everywhere in an application.</span></span> <span data-ttu-id="25efb-112">這可減少您需要撰寫的程式碼數量，並可讓您撰寫較少的錯誤，很容易出錯且更容易維護的程式碼。</span><span class="sxs-lookup"><span data-stu-id="25efb-112">This reduces the amount of code you need to write and makes the code you do write less error prone and easier to maintain.</span></span>

<span data-ttu-id="25efb-113">ASP.NET MVC 和 Entity Framework Code First 所提供的驗證支援就是執行 DRY 準則的動作的絕佳範例。</span><span class="sxs-lookup"><span data-stu-id="25efb-113">The validation support provided by ASP.NET MVC and Entity Framework Code First is a great example of the DRY principle in action.</span></span> <span data-ttu-id="25efb-114">您可以以宣告方式指定在單一位置 （在模型類別中） 中的驗證規則和規則是任何位置強制執行應用程式中。</span><span class="sxs-lookup"><span data-stu-id="25efb-114">You can declaratively specify validation rules in one place (in the model class) and the rules are enforced everywhere in the application.</span></span>

<span data-ttu-id="25efb-115">讓我們看看如何您可以利用這項驗證支援的電影應用程式中。</span><span class="sxs-lookup"><span data-stu-id="25efb-115">Let's look at how you can take advantage of this validation support in the movie application.</span></span>

## <a name="adding-validation-rules-to-the-movie-model"></a><span data-ttu-id="25efb-116">將驗證規則新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="25efb-116">Adding Validation Rules to the Movie Model</span></span>

<span data-ttu-id="25efb-117">一開始要先新增一些驗證邏輯，以`Movie`類別。</span><span class="sxs-lookup"><span data-stu-id="25efb-117">You'll begin by adding some validation logic to the `Movie` class.</span></span>

<span data-ttu-id="25efb-118">開啟 *Movie.cs* 檔案。</span><span class="sxs-lookup"><span data-stu-id="25efb-118">Open the *Movie.cs* file.</span></span> <span data-ttu-id="25efb-119">新增`using`陳述式參考的檔案頂端[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間：</span><span class="sxs-lookup"><span data-stu-id="25efb-119">Add a `using` statement at the top of the file that references the [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample1.cs)]

<span data-ttu-id="25efb-120">請注意命名空間不包含`System.Web`。</span><span class="sxs-lookup"><span data-stu-id="25efb-120">Notice the namespace does not contain `System.Web`.</span></span> <span data-ttu-id="25efb-121">DataAnnotations 提供一組內建的驗證屬性，您可以以宣告方式套用至任何類別或屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-121">DataAnnotations provides a built-in set of validation attributes that you can apply declaratively to any class or property.</span></span>

<span data-ttu-id="25efb-122">現在更新`Movie`類別，以充分利用內建[ `Required` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx)， [ `StringLength` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx)，並[ `Range` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx)驗證屬性.</span><span class="sxs-lookup"><span data-stu-id="25efb-122">Now update the `Movie` class to take advantage of the built-in [`Required`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.requiredattribute.aspx), [`StringLength`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.stringlengthattribute.aspx), and [`Range`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.rangeattribute.aspx) validation attributes.</span></span> <span data-ttu-id="25efb-123">使用下列程式碼作為範例，要套用屬性的位置。</span><span class="sxs-lookup"><span data-stu-id="25efb-123">Use the following code as an example of where to apply the attributes.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample2.cs?highlight=4,10,13,17)]

<span data-ttu-id="25efb-124">執行應用程式，您仍會收到下列執行的階段錯誤：</span><span class="sxs-lookup"><span data-stu-id="25efb-124">Run the application and you will again get the following run time error:</span></span>

<span data-ttu-id="25efb-125">***備份 'MovieDBContext' 內容的模型已變更，因為所建立的資料庫。請考慮使用 Code First 移轉更新資料庫 ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269))。***</span><span class="sxs-lookup"><span data-stu-id="25efb-125">***The model backing the 'MovieDBContext' context has changed since the database was created. Consider using Code First Migrations to update the database ([https://go.microsoft.com/fwlink/?LinkId=238269](https://go.microsoft.com/fwlink/?LinkId=238269)).***</span></span>

<span data-ttu-id="25efb-126">我們將使用移轉來更新結構描述。</span><span class="sxs-lookup"><span data-stu-id="25efb-126">We will use migrations to update the schema.</span></span> <span data-ttu-id="25efb-127">建置方案，然後再開啟**Package Manager Console**視窗並輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="25efb-127">Build the solution, and then open the **Package Manager Console** window and enter the following commands:</span></span>

[!code-console[Main](adding-validation-to-the-model/samples/sample3.cmd)]

<span data-ttu-id="25efb-128">此命令完成時，Visual Studio 會開啟可定義新的類別檔案`DbMIgration`衍生類別指定名稱 (*AddDataAnnotationsMig*)，然後在`Up`方法，您可以看到更新的程式碼結構描述條件約束。</span><span class="sxs-lookup"><span data-stu-id="25efb-128">When this command finishes, Visual Studio opens the class file that defines the new `DbMIgration` derived class with the name specified (*AddDataAnnotationsMig*), and in the `Up` method you can see the code that updates the schema constraints.</span></span> <span data-ttu-id="25efb-129">`Title`並`Genre`欄位不再是可為 null （也就是您必須輸入的值） 和`Rating`欄位的最大長度為 5。</span><span class="sxs-lookup"><span data-stu-id="25efb-129">The `Title` and `Genre` fields are no longer nullable (that is, you must enter a value) and the `Rating` field has a maximum length of 5.</span></span>

<span data-ttu-id="25efb-130">驗證屬性會指定您想要強制執行模型屬性套用的行為。</span><span class="sxs-lookup"><span data-stu-id="25efb-130">The validation attributes specify behavior that you want to enforce on the model properties they are applied to.</span></span> <span data-ttu-id="25efb-131">`Required`屬性會指出屬性必須有值; 在此範例中，電影必須具有值`Title`， `ReleaseDate`， `Genre`，和`Price`屬性才會生效。</span><span class="sxs-lookup"><span data-stu-id="25efb-131">The `Required` attribute indicates that a property must have a value; in this sample, a movie has to have values for the `Title`, `ReleaseDate`, `Genre`, and `Price` properties in order to be valid.</span></span> <span data-ttu-id="25efb-132">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="25efb-132">The `Range` attribute constrains a value to within a specified range.</span></span> <span data-ttu-id="25efb-133">`StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。</span><span class="sxs-lookup"><span data-stu-id="25efb-133">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span> <span data-ttu-id="25efb-134">內建類型 (例如`decimal, int, float, DateTime`) 預設為必要項，而且不需要`Required`屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-134">Intrinsic types (such as `decimal, int, float, DateTime`) are required by default and don't need the `Required` attribute.</span></span>

<span data-ttu-id="25efb-135">程式碼會先確認您的模型類別指定的驗證規則會強制執行，才能應用程式會將變更儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="25efb-135">Code First ensures that the validation rules you specify on a model class are enforced before the application saves changes in the database.</span></span> <span data-ttu-id="25efb-136">例如，下列程式碼將會擲回例外狀況時`SaveChanges`呼叫方法，因為數個需要`Movie`屬性值遺漏，且價格為零 （這是超出有效範圍）。</span><span class="sxs-lookup"><span data-stu-id="25efb-136">For example, the code below will throw an exception when the `SaveChanges` method is called, because several required `Movie` property values are missing and the price is zero (which is out of the valid range).</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample4.cs?highlight=7-8)]

<span data-ttu-id="25efb-137">具有自動強制執行的.NET Framework 的驗證規則有助於讓您的應用程式更穩固。</span><span class="sxs-lookup"><span data-stu-id="25efb-137">Having validation rules automatically enforced by the .NET Framework helps make your application more robust.</span></span> <span data-ttu-id="25efb-138">它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。</span><span class="sxs-lookup"><span data-stu-id="25efb-138">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

<span data-ttu-id="25efb-139">以下是完整的程式碼，更新清單*Movie.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="25efb-139">Here's a complete code listing for the updated *Movie.cs* file:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample5.cs)]

## <a name="validation-error-ui-in-aspnet-mvc"></a><span data-ttu-id="25efb-140">驗證錯誤 ASP.NET MVC 中的 UI</span><span class="sxs-lookup"><span data-stu-id="25efb-140">Validation Error UI in ASP.NET MVC</span></span>

<span data-ttu-id="25efb-141">重新執行應用程式，並瀏覽至 */Movies* URL。</span><span class="sxs-lookup"><span data-stu-id="25efb-141">Re-run the application and navigate to the */Movies* URL.</span></span>

<span data-ttu-id="25efb-142">按一下 **新建**連結，可新增一部新電影。</span><span class="sxs-lookup"><span data-stu-id="25efb-142">Click the **Create New** link to add a new movie.</span></span> <span data-ttu-id="25efb-143">填妥表單中使用某些無效的值，然後按一下 [**建立**] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="25efb-143">Fill out the form with some invalid values and then click the **Create** button.</span></span>

![8_validationErrors](adding-validation-to-the-model/_static/image1.png)

> [!NOTE]
> <span data-ttu-id="25efb-145">以 jQuery 驗證支援非英文的地區設定，請使用逗號 (&quot;，&quot;) 的小數點，您必須加入*globalize.js*和您的特定*cultures/globalize.cultures.js*檔案 (從[ https://github.com/jquery/globalize ](https://github.com/jquery/globalize) ) 和使用的 JavaScript `Globalize.parseFloat`。</span><span class="sxs-lookup"><span data-stu-id="25efb-145">to support jQuery validation for non-English locales that use a comma (&quot;,&quot;) for a decimal point, you must include *globalize.js* and your specific *cultures/globalize.cultures.js* file(from [https://github.com/jquery/globalize](https://github.com/jquery/globalize) ) and JavaScript to use `Globalize.parseFloat`.</span></span> <span data-ttu-id="25efb-146">下列程式碼會顯示在 Views\Movies\Edit.cshtml 檔案，才能使用已修改&quot;FR-FR&quot;文化特性：</span><span class="sxs-lookup"><span data-stu-id="25efb-146">The following code shows the modifications to the Views\Movies\Edit.cshtml file to work with the &quot;fr-FR&quot; culture:</span></span>


[!code-cshtml[Main](adding-validation-to-the-model/samples/sample6.cshtml)]

<span data-ttu-id="25efb-147">請注意如何表單已自動使用紅色外框色彩來反白顯示的文字方塊，其中包含無效的資料，並發出每一個旁適當的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="25efb-147">Notice how the form has automatically used a red border color to highlight the text boxes that contain invalid data and has emitted an appropriate validation error message next to each one.</span></span> <span data-ttu-id="25efb-148">用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (若使用者已停用 JavaScript 時) 都會強制執行這些錯誤。</span><span class="sxs-lookup"><span data-stu-id="25efb-148">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (in case a user has JavaScript disabled).</span></span>

<span data-ttu-id="25efb-149">實質的好處是，您不需要變更任何程式中的程式碼`MoviesController`類別或*Create.cshtml*才能啟用這項驗證 UI 的檢視。</span><span class="sxs-lookup"><span data-stu-id="25efb-149">A real benefit is that you didn't need to change a single line of code in the `MoviesController` class or in the *Create.cshtml* view in order to enable this validation UI.</span></span> <span data-ttu-id="25efb-150">您稍早在本教學課程中建立的控制器和檢視會自動拾取您指定的驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。</span><span class="sxs-lookup"><span data-stu-id="25efb-150">The controller and views you created earlier in this tutorial automatically picked up the validation rules that you specified by using validation attributes on the properties of the `Movie` model class.</span></span>

<span data-ttu-id="25efb-151">您可能已經注意到的屬性`Title`並`Genre`，必要的屬性不會強制執行直到您提交表單 (點擊**建立**按鈕)，或輸入欄位中輸入文字，並將其移除。</span><span class="sxs-lookup"><span data-stu-id="25efb-151">You might have noticed for the properties `Title` and `Genre`, the required attribute is not enforced until you submit the form (hit the **Create** button), or enter text into the input field and removed it.</span></span> <span data-ttu-id="25efb-152">欄位的最初是空的 （例如 [建立] 檢視中的欄位），且具有必要的屬性與其他任何驗證屬性，您可以執行下列命令來觸發驗證：</span><span class="sxs-lookup"><span data-stu-id="25efb-152">For a field which is initially empty (such as the fields on the Create view) and which has only the required attribute and no other validation attributes, you can do the following to trigger validation:</span></span>

1. <span data-ttu-id="25efb-153">在欄位的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="25efb-153">Tab into the field.</span></span>
2. <span data-ttu-id="25efb-154">輸入一些文字。</span><span class="sxs-lookup"><span data-stu-id="25efb-154">Enter some text.</span></span>
3. <span data-ttu-id="25efb-155">按下 Tab 鍵切換至下一個欄位。</span><span class="sxs-lookup"><span data-stu-id="25efb-155">Tab out.</span></span>
4. <span data-ttu-id="25efb-156">回欄位的索引標籤。</span><span class="sxs-lookup"><span data-stu-id="25efb-156">Tab back into the field.</span></span>
5. <span data-ttu-id="25efb-157">移除文字。</span><span class="sxs-lookup"><span data-stu-id="25efb-157">Remove the text.</span></span>
6. <span data-ttu-id="25efb-158">按下 Tab 鍵切換至下一個欄位。</span><span class="sxs-lookup"><span data-stu-id="25efb-158">Tab out.</span></span>

<span data-ttu-id="25efb-159">上述的序列將會觸發必要的驗證，而不叫用 [提交] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="25efb-159">The above sequence will trigger the required validation without hitting the submit button.</span></span> <span data-ttu-id="25efb-160">只要不需要輸入任何欄位中按 [提交] 按鈕，將會觸發用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="25efb-160">Simply hitting the submit button without entering any of the fields will trigger client side validation.</span></span> <span data-ttu-id="25efb-161">直到沒有任何用戶端驗證錯誤後，表單資料才會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="25efb-161">The form data is not sent to the server until there are no client side validation errors.</span></span> <span data-ttu-id="25efb-162">您可以將中斷點放在 HTTP Post 方法，或使用測試[fiddler 工具](http://fiddler2.com/fiddler2/)或 IE 9 [F12 開發人員工具](https://msdn.microsoft.com/ie/aa740478)。</span><span class="sxs-lookup"><span data-stu-id="25efb-162">You can test this by putting a break point in the HTTP Post method or using the [fiddler tool](http://fiddler2.com/fiddler2/) or the IE 9 [F12 developer tools](https://msdn.microsoft.com/ie/aa740478).</span></span>

![](adding-validation-to-the-model/_static/image2.png)

## <a name="how-validation-occurs-in-the-create-view-and-create-action-method"></a><span data-ttu-id="25efb-163">如何驗證發生在 建立檢視，以及建立動作方法</span><span class="sxs-lookup"><span data-stu-id="25efb-163">How Validation Occurs in the Create View and Create Action Method</span></span>

<span data-ttu-id="25efb-164">您可能奇怪如何在不更新控制器或檢視程式碼的狀況下產生驗證 UI。</span><span class="sxs-lookup"><span data-stu-id="25efb-164">You might wonder how the validation UI was generated without any updates to the code in the controller or views.</span></span> <span data-ttu-id="25efb-165">下一步 的清單顯示什麼`Create`中的方法`MovieController`類別看起來像。</span><span class="sxs-lookup"><span data-stu-id="25efb-165">The next listing shows what the `Create` methods in the `MovieController` class look like.</span></span> <span data-ttu-id="25efb-166">它們與您建立的方式加以稍早在本教學課程中相同。</span><span class="sxs-lookup"><span data-stu-id="25efb-166">They're unchanged from how you created them earlier in this tutorial.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample7.cs?highlight=12,15)]

<span data-ttu-id="25efb-167">第一個 (HTTP GET)`Create` 動作方法會顯示初始建立表單。</span><span class="sxs-lookup"><span data-stu-id="25efb-167">The first (HTTP GET) `Create` action method displays the initial Create form.</span></span> <span data-ttu-id="25efb-168">第二個 (`[HttpPost]`) 版本處理表單張貼。</span><span class="sxs-lookup"><span data-stu-id="25efb-168">The second (`[HttpPost]`) version handles the form post.</span></span> <span data-ttu-id="25efb-169">第二個 `Create` 方法 (`HttpPost` 版本) 會呼叫`ModelState.IsValid` 檢查電影是否有任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="25efb-169">The second `Create` method (The `HttpPost` version) calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span> <span data-ttu-id="25efb-170">呼叫此方法會評估已套用至物件的所有驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-170">Calling this method evaluates any validation attributes that have been applied to the object.</span></span> <span data-ttu-id="25efb-171">如果物件有驗證錯誤，則 `Create` 方法會重新顯示表單。</span><span class="sxs-lookup"><span data-stu-id="25efb-171">If the object has validation errors, the `Create` method re-displays the form.</span></span> <span data-ttu-id="25efb-172">如果沒有任何錯誤，方法即會將新的電影儲存到資料庫。</span><span class="sxs-lookup"><span data-stu-id="25efb-172">If there are no errors, the method saves the new movie in the database.</span></span> <span data-ttu-id="25efb-173">在影片範例中我們使用，**表單不張貼至伺服器的用戶端; 上偵測到驗證錯誤時，第二個** `Create`**絕不會呼叫方法**。</span><span class="sxs-lookup"><span data-stu-id="25efb-173">In our movie example we are using, **the form is not posted to the server when their are validation errors detected on the client side; the second** `Create`**method is never called**.</span></span> <span data-ttu-id="25efb-174">如果您停用 JavaScript 瀏覽器中，用戶端驗證已停用與 HTTP POST`Create`方法呼叫`ModelState.IsValid`檢查電影是否有任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="25efb-174">If you disable JavaScript in your browser, client validation is disabled and the HTTP POST `Create` method calls `ModelState.IsValid` to check whether the movie has any validation errors.</span></span>

<span data-ttu-id="25efb-175">您可以在 `HttpPost Create` 方法中設定中斷點，並確認永遠不會呼叫該方法，用戶端驗證就不會在偵測到驗證錯誤時，提交表單資料。</span><span class="sxs-lookup"><span data-stu-id="25efb-175">You can set a break point in the `HttpPost Create` method and verify the method is never called, client side validation will not submit the form data when validation errors are detected.</span></span> <span data-ttu-id="25efb-176">如果您停用瀏覽器的 JavaScript，然後提交有錯誤的表單，就會叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="25efb-176">If you disable JavaScript in your browser, then submit the form with errors, the break point will be hit.</span></span> <span data-ttu-id="25efb-177">您仍可使用沒有 JavaScript 的完整驗證。</span><span class="sxs-lookup"><span data-stu-id="25efb-177">You still get full validation without JavaScript.</span></span> <span data-ttu-id="25efb-178">下圖顯示如何停用 Internet Explorer 中的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="25efb-178">The following image shows how to disable JavaScript in Internet Explorer.</span></span>

![](adding-validation-to-the-model/_static/image3.png)

![](adding-validation-to-the-model/_static/image4.png)

<span data-ttu-id="25efb-179">下圖顯示如何停用 FireFox 瀏覽器的 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="25efb-179">The following image shows how to disable JavaScript in the FireFox browser.</span></span>

![](adding-validation-to-the-model/_static/image5.png)

<span data-ttu-id="25efb-180">下圖顯示如何搭配 Chrome 瀏覽器中停用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="25efb-180">The following image shows how to disable JavaScript with the Chrome browser.</span></span>

![](adding-validation-to-the-model/_static/image6.png)

<span data-ttu-id="25efb-181">以下是*Create.cshtml*您稍早在本教學課程中包含 scaffold 的檢視範本。</span><span class="sxs-lookup"><span data-stu-id="25efb-181">Below is the *Create.cshtml* view template that you scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="25efb-182">上述動作方法會使用它來顯示初始表單，以及在發生錯誤時重新顯示它。</span><span class="sxs-lookup"><span data-stu-id="25efb-182">It's used by the action methods shown above both to display the initial form and to redisplay it in the event of an error.</span></span>

[!code-cshtml[Main](adding-validation-to-the-model/samples/sample8.cshtml?highlight=22-23,30-31,38-39,46-47)]

<span data-ttu-id="25efb-183">請注意程式碼的使用方式`Html.EditorFor`輸出的協助專家`<input>`每個項目`Movie`屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-183">Notice how the code uses an `Html.EditorFor` helper to output the `<input>` element for each `Movie` property.</span></span> <span data-ttu-id="25efb-184">此協助程式旁會呼叫`Html.ValidationMessageFor`helper 方法。</span><span class="sxs-lookup"><span data-stu-id="25efb-184">Next to this helper is a call to the `Html.ValidationMessageFor` helper method.</span></span> <span data-ttu-id="25efb-185">這兩個 helper 方法會使用控制器傳遞至檢視的模型物件 (在此情況下，`Movie`物件)。</span><span class="sxs-lookup"><span data-stu-id="25efb-185">These two helper methods work with the model object that's passed by the controller to the view (in this case, a `Movie` object).</span></span> <span data-ttu-id="25efb-186">它們會自動尋找適當的模型並顯示錯誤訊息中指定的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-186">They automatically look for validation attributes specified on the model and display error messages as appropriate.</span></span>

<span data-ttu-id="25efb-187">什麼是這種方法的優點在於控制器和 Create 檢視範本都不知道的任何項目顯示的特定錯誤訊息或強制執行的實際驗證規則相關。</span><span class="sxs-lookup"><span data-stu-id="25efb-187">What's really nice about this approach is that neither the controller nor the Create view template knows anything about the actual validation rules being enforced or about the specific error messages displayed.</span></span> <span data-ttu-id="25efb-188">驗證規則和錯誤字串只在 `Movie` 類別中指定。</span><span class="sxs-lookup"><span data-stu-id="25efb-188">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="25efb-189">這些相同的驗證規則會自動套用 [編輯] 檢視和任何其他檢視範本可能會建立以編輯模型。</span><span class="sxs-lookup"><span data-stu-id="25efb-189">These same validation rules are automatically applied to the Edit view and any other views templates you might create that edit your model.</span></span>

<span data-ttu-id="25efb-190">如果您想要稍後變更驗證邏輯，則可以只在一個位置新增至模型的驗證屬性 (在此範例中，`movie`類別)。</span><span class="sxs-lookup"><span data-stu-id="25efb-190">If you want to change the validation logic later, you can do so in exactly one place by adding validation attributes to the model (in this example, the `movie` class).</span></span> <span data-ttu-id="25efb-191">您不必擔心應用程式的不同部分會與規則強制執行的方式不一致，所有的驗證邏輯都是在同一個地方定義，用於所有位置。</span><span class="sxs-lookup"><span data-stu-id="25efb-191">You won't have to worry about different parts of the application being inconsistent with how the rules are enforced — all validation logic will be defined in one place and used everywhere.</span></span> <span data-ttu-id="25efb-192">這會讓程式碼非常整齊乾淨，容易維護及發展。</span><span class="sxs-lookup"><span data-stu-id="25efb-192">This keeps the code very clean, and makes it easy to maintain and evolve.</span></span> <span data-ttu-id="25efb-193">這表示您會完全接受 DRY 原則。</span><span class="sxs-lookup"><span data-stu-id="25efb-193">And it means that you'll be fully honoring the DRY principle.</span></span>

## <a name="adding-formatting-to-the-movie-model"></a><span data-ttu-id="25efb-194">加入至電影模型格式設定</span><span class="sxs-lookup"><span data-stu-id="25efb-194">Adding Formatting to the Movie Model</span></span>

<span data-ttu-id="25efb-195">開啟 *Movie.cs* 檔案並檢查 `Movie` 類別。</span><span class="sxs-lookup"><span data-stu-id="25efb-195">Open the *Movie.cs* file and examine the `Movie` class.</span></span> <span data-ttu-id="25efb-196">[ `System.ComponentModel.DataAnnotations` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)命名空間會提供除了一組內建的驗證屬性的格式化屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-196">The [`System.ComponentModel.DataAnnotations`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="25efb-197">我們已套用[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)發行日期和價格欄位的列舉值。</span><span class="sxs-lookup"><span data-stu-id="25efb-197">We've already applied a [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) enumeration value to the release date and to the price fields.</span></span> <span data-ttu-id="25efb-198">下列程式碼示範`ReleaseDate`並`Price`屬性以適當[ `DisplayFormat` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx)屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-198">The following code shows the `ReleaseDate` and `Price` properties with the appropriate [`DisplayFormat`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.displayformatattribute.aspx) attribute.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample9.cs)]

<span data-ttu-id="25efb-199">[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性不是驗證屬性，它們用來指示檢視引擎如何呈現 HTML。</span><span class="sxs-lookup"><span data-stu-id="25efb-199">The [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributes are not validation attributes, they are used to tell the view engine how to render the HTML.</span></span> <span data-ttu-id="25efb-200">在上面的範例，`DataType.Date`屬性會顯示影片日期為日期，而不需要的時間。</span><span class="sxs-lookup"><span data-stu-id="25efb-200">In the example above, the `DataType.Date` attribute displays the movie dates as dates only, without time.</span></span> <span data-ttu-id="25efb-201">例如，下列[ `DataType` ](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx)屬性不會驗證資料的格式：</span><span class="sxs-lookup"><span data-stu-id="25efb-201">For example, the following [`DataType`](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.datatype.aspx) attributes don't validate the format of the data:</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample10.cs)]

<span data-ttu-id="25efb-202">以上所列的屬性只會提供檢視引擎格式化資料的提示 (並提供屬性，例如&lt;&gt;用於 URL 並&lt;a =&quot;mailto:EmailAddress.com&quot; &gt;電子郵件。</span><span class="sxs-lookup"><span data-stu-id="25efb-202">The attributes listed above only provide hints for the view engine to format the data (and supply attributes such as &lt;a&gt; for URL's and &lt;a href=&quot;mailto:EmailAddress.com&quot;&gt; for email.</span></span> <span data-ttu-id="25efb-203">您可以使用[RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx)来驗證的資料格式屬性。</span><span class="sxs-lookup"><span data-stu-id="25efb-203">You can use the [RegularExpression](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.regularexpressionattribute.aspx) attribute to validate the format of the data.</span></span>

<span data-ttu-id="25efb-204">若要使用的替代方法`DataType`屬性，您也可以明確設定[ `DataFormatString` ](https://msdn.microsoft.com/library/system.string.format.aspx)值。</span><span class="sxs-lookup"><span data-stu-id="25efb-204">An alternative approach to using the `DataType` attributes, you could explicitly set a [`DataFormatString`](https://msdn.microsoft.com/library/system.string.format.aspx) value.</span></span> <span data-ttu-id="25efb-205">下列程式碼顯示以日期格式字串的發行日期屬性 (亦即&quot;d&quot;)。</span><span class="sxs-lookup"><span data-stu-id="25efb-205">The following code shows the release date property with a date format string (namely, &quot;d&quot;).</span></span> <span data-ttu-id="25efb-206">您會使用此對話方塊來指定您不想要時間發行日期的一部分。</span><span class="sxs-lookup"><span data-stu-id="25efb-206">You'd use this to specify that you don't want to time as part of the release date.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample11.cs)]

<span data-ttu-id="25efb-207">完整`Movie`類別如下所示。</span><span class="sxs-lookup"><span data-stu-id="25efb-207">The complete `Movie` class is shown below.</span></span>

[!code-csharp[Main](adding-validation-to-the-model/samples/sample12.cs)]

<span data-ttu-id="25efb-208">執行應用程式，並瀏覽至`Movies`控制站。</span><span class="sxs-lookup"><span data-stu-id="25efb-208">Run the application and browse to the `Movies` controller.</span></span> <span data-ttu-id="25efb-209">適當格式化的發行日期和價格。</span><span class="sxs-lookup"><span data-stu-id="25efb-209">The release date and price are nicely formatted.</span></span> <span data-ttu-id="25efb-210">下圖顯示 發行日期和價格使用&quot;FR-FR&quot;與文化特性。</span><span class="sxs-lookup"><span data-stu-id="25efb-210">The image below shows the release date and price using &quot;fr-FR&quot; as the culture.</span></span>

![8_format_SM](adding-validation-to-the-model/_static/image7.png)

<span data-ttu-id="25efb-212">下圖顯示相同的資料顯示的預設文化特性 （美式英文）。</span><span class="sxs-lookup"><span data-stu-id="25efb-212">The image below shows the same data displayed with the default culture (English US).</span></span>

![](adding-validation-to-the-model/_static/image8.png)

<span data-ttu-id="25efb-213">在數列的下一個部分中，我們會檢閱應用程式，並對自動產生的 `Details` 和 `Delete` 方法進行一些改良。</span><span class="sxs-lookup"><span data-stu-id="25efb-213">In the next part of the series, we'll review the application and make some improvements to the automatically generated `Details` and `Delete` methods.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="25efb-214">[上一頁](adding-a-new-field-to-the-movie-model-and-table.md)
> [下一頁](examining-the-details-and-delete-methods.md)</span><span class="sxs-lookup"><span data-stu-id="25efb-214">[Previous](adding-a-new-field-to-the-movie-model-and-table.md)
[Next](examining-the-details-and-delete-methods.md)</span></span>
