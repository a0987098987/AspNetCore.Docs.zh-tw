---
title: 將驗證新增至 ASP.NET Core Razor頁面
author: rick-anderson
description: 探索如何將驗證新增至 ASP.NET Core Razor中的頁面。
ms.author: riande
ms.custom: mvc
ms.date: 7/23/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: tutorials/razor-pages/validation
ms.openlocfilehash: 91f0ac5fcd607f2423f9fc4647413b2bbb2336fc
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82773771"
---
# <a name="add-validation-to-an-aspnet-core-razor-page"></a><span data-ttu-id="c05b8-103">將驗證新增至 ASP.NET Core Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="c05b8-103">Add validation to an ASP.NET Core Razor Page</span></span>

<span data-ttu-id="c05b8-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c05b8-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c05b8-105">在本節中將驗證邏輯新增至 `Movie` 模型。</span><span class="sxs-lookup"><span data-stu-id="c05b8-105">In this section, validation logic is added to the `Movie` model.</span></span> <span data-ttu-id="c05b8-106">使用者建立或編輯電影時，就會強制執行驗證規則。</span><span class="sxs-lookup"><span data-stu-id="c05b8-106">The validation rules are enforced any time a user creates or edits a movie.</span></span>

## <a name="validation"></a><span data-ttu-id="c05b8-107">驗證</span><span class="sxs-lookup"><span data-stu-id="c05b8-107">Validation</span></span>

<span data-ttu-id="c05b8-108">軟體開發的核心原則稱為 [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself)("**D**on't **R**epeat **Y**ourself", 不重複原則)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-108">A key tenet of software development is called [DRY](https://wikipedia.org/wiki/Don%27t_repeat_yourself) ("**D**on't **R**epeat **Y**ourself").</span></span> <span data-ttu-id="c05b8-109">Razor Pages可促進開發，只要指定功能一次，就能在整個應用程式中運用。</span><span class="sxs-lookup"><span data-stu-id="c05b8-109">Razor Pages encourages development where functionality is specified once, and it's reflected throughout the app.</span></span> <span data-ttu-id="c05b8-110">DRY 有助於：</span><span class="sxs-lookup"><span data-stu-id="c05b8-110">DRY can help:</span></span>

* <span data-ttu-id="c05b8-111">降低應用程式中的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="c05b8-111">Reduce the amount of code in an app.</span></span>
* <span data-ttu-id="c05b8-112">使程式碼較少出現錯誤，而且更容易進行測試和維護。</span><span class="sxs-lookup"><span data-stu-id="c05b8-112">Make the code less error prone, and easier to test and maintain.</span></span>

<span data-ttu-id="c05b8-113">Razor 頁面和 Entity Framework 所提供的驗證支援就是 DRY 準則的絶佳範例。</span><span class="sxs-lookup"><span data-stu-id="c05b8-113">The validation support provided by Razor Pages and Entity Framework is a good example of the DRY principle.</span></span> <span data-ttu-id="c05b8-114">驗證規則是在單一位置 (在模型類別中) 以宣告方式指定，而規則可在應用程式的任何位置強制執行。</span><span class="sxs-lookup"><span data-stu-id="c05b8-114">Validation rules are declaratively specified in one place (in the model class), and the rules are enforced everywhere in the app.</span></span>

## <a name="add-validation-rules-to-the-movie-model"></a><span data-ttu-id="c05b8-115">將驗證規則新增至電影模型</span><span class="sxs-lookup"><span data-stu-id="c05b8-115">Add validation rules to the movie model</span></span>

<span data-ttu-id="c05b8-116">DataAnnotations 命名空間提供一組內建的驗證屬性 (attribute)，其以宣告方式套用至類別或屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-116">The DataAnnotations namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="c05b8-117">DataAnnotations 也包含格式化屬性 (如 `DataType`)，可協助進行格式化，但不提供任何驗證。</span><span class="sxs-lookup"><span data-stu-id="c05b8-117">DataAnnotations also contains formatting attributes like `DataType` that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="c05b8-118">更新 `Movie` 類別，以充分利用內建的 `Required`、`StringLength`、`RegularExpression` 和 `Range` 驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-118">Update the `Movie` class to take advantage of the built-in `Required`, `StringLength`, `RegularExpression`, and `Range` validation attributes.</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="c05b8-119">驗證屬性會指定您想要對套用目標模型屬性強制執行的行為：</span><span class="sxs-lookup"><span data-stu-id="c05b8-119">The validation attributes specify behavior that you want to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="c05b8-120">`Required` 和 `MinimumLength` 屬性 (attribute) 指出屬性 (property) 必須是值；但無法防止使用者輸入空格以滿足此驗證。</span><span class="sxs-lookup"><span data-stu-id="c05b8-120">The `Required` and `MinimumLength` attributes indicate that a property must have a value; but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="c05b8-121">`RegularExpression` 屬性則用來限制可輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="c05b8-121">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="c05b8-122">在上述程式碼中，"Genre"：</span><span class="sxs-lookup"><span data-stu-id="c05b8-122">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="c05b8-123">必須指使用字母。</span><span class="sxs-lookup"><span data-stu-id="c05b8-123">Must only use letters.</span></span>
  * <span data-ttu-id="c05b8-124">第一個字母必須是大寫。</span><span class="sxs-lookup"><span data-stu-id="c05b8-124">The first letter is required to be uppercase.</span></span> <span data-ttu-id="c05b8-125">不允許使用空格、數字和特殊字元。</span><span class="sxs-lookup"><span data-stu-id="c05b8-125">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="c05b8-126">`RegularExpression` "Rating"：</span><span class="sxs-lookup"><span data-stu-id="c05b8-126">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="c05b8-127">第一個字元必須為大寫字母。</span><span class="sxs-lookup"><span data-stu-id="c05b8-127">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="c05b8-128">允許後續空格中的特殊字元和數位。</span><span class="sxs-lookup"><span data-stu-id="c05b8-128">Allows special characters and numbers in  subsequent spaces.</span></span> <span data-ttu-id="c05b8-129">"PG-13" 對分級而言有效，但不適用於 "Genre"。</span><span class="sxs-lookup"><span data-stu-id="c05b8-129">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="c05b8-130">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="c05b8-130">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="c05b8-131">`StringLength` 屬性可讓您設定字串屬性的最大長度，並選擇性設定其最小長度。</span><span class="sxs-lookup"><span data-stu-id="c05b8-131">The `StringLength` attribute lets you set the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="c05b8-132">實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-132">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="c05b8-133">擁有 ASP.NET Core 自動強制執行的驗證規則有助於讓您的應用程式更穩固。</span><span class="sxs-lookup"><span data-stu-id="c05b8-133">Having validation rules automatically enforced by ASP.NET Core helps make your app more robust.</span></span> <span data-ttu-id="c05b8-134">它也確保您不會忘記要驗證某些項目，不小心讓不正確的資料進入資料庫。</span><span class="sxs-lookup"><span data-stu-id="c05b8-134">It also ensures that you can't forget to validate something and inadvertently let bad data into the database.</span></span>

### <a name="validation-error-ui-in-razor-pages"></a><span data-ttu-id="c05b8-135">Razor 頁面中的驗證錯誤 UI</span><span class="sxs-lookup"><span data-stu-id="c05b8-135">Validation Error UI in Razor Pages</span></span>

<span data-ttu-id="c05b8-136">執行應用程式，並巡覽至 Pages/Movies。</span><span class="sxs-lookup"><span data-stu-id="c05b8-136">Run the app and navigate to Pages/Movies.</span></span>

<span data-ttu-id="c05b8-137">選取 **Create New** 連結。</span><span class="sxs-lookup"><span data-stu-id="c05b8-137">Select the **Create New** link.</span></span> <span data-ttu-id="c05b8-138">使用某些無效值完成表單。</span><span class="sxs-lookup"><span data-stu-id="c05b8-138">Complete the form with some invalid values.</span></span> <span data-ttu-id="c05b8-139">當 jQuery 用戶端驗證偵測到錯誤時，它會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="c05b8-139">When jQuery client-side validation detects the error, it displays an error message.</span></span>

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](validation/_static/val.png)

[!INCLUDE[](~/includes/localization/currency.md)]

<span data-ttu-id="c05b8-141">請注意表單在包含無效值的每個欄位中自動呈現驗證錯誤訊息的方式。</span><span class="sxs-lookup"><span data-stu-id="c05b8-141">Notice how the form has automatically rendered a validation error message in each field containing an invalid value.</span></span> <span data-ttu-id="c05b8-142">用戶端 (使用 JavaScript 和 jQuery) 與伺服器端 (當使用者已停用 JavaScript 時) 都會強制執行這些錯誤。</span><span class="sxs-lookup"><span data-stu-id="c05b8-142">The errors are enforced both client-side (using JavaScript and jQuery) and server-side (when a user has JavaScript disabled).</span></span>

<span data-ttu-id="c05b8-143">明顯的好處是：**不**需要在 Create 或 Edit 頁面中進行程式碼變更。</span><span class="sxs-lookup"><span data-stu-id="c05b8-143">A significant benefit is that **no** code changes were necessary in the Create  or Edit pages.</span></span> <span data-ttu-id="c05b8-144">一旦 DataAnnotations 套用至模型，就會啟用驗證 UI。</span><span class="sxs-lookup"><span data-stu-id="c05b8-144">Once DataAnnotations were applied to the model, the validation UI was enabled.</span></span> <span data-ttu-id="c05b8-145">本教學課程中建立的 Razor Pages 會自動拾取驗證規則 (在 `Movie` 模型類別的屬性 (property) 上使用驗證屬性 (attribute))。</span><span class="sxs-lookup"><span data-stu-id="c05b8-145">The Razor Pages created in this tutorial automatically picked up the validation rules (using validation attributes on the properties of the `Movie` model class).</span></span> <span data-ttu-id="c05b8-146">使用 Edit 頁面測試驗證，會套用相同的驗證。</span><span class="sxs-lookup"><span data-stu-id="c05b8-146">Test validation using the Edit page, the same validation is applied.</span></span>

<span data-ttu-id="c05b8-147">要一直到沒有任何用戶端驗證錯誤之後，才會將表單資料發佈到伺服器。</span><span class="sxs-lookup"><span data-stu-id="c05b8-147">The form data isn't posted to the server until there are no client-side validation errors.</span></span> <span data-ttu-id="c05b8-148">請確認表單資料不會經由下列一或多種方式發佈：</span><span class="sxs-lookup"><span data-stu-id="c05b8-148">Verify form data isn't posted by one or more of the following approaches:</span></span>

* <span data-ttu-id="c05b8-149">將中斷點放置在 `OnPostAsync` 方法中。</span><span class="sxs-lookup"><span data-stu-id="c05b8-149">Put a break point in the `OnPostAsync` method.</span></span> <span data-ttu-id="c05b8-150">提交表單 (選取 [建立]\*\*\*\* 或 [儲存]\*\*\*\*)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-150">Submit the form (select **Create** or **Save**).</span></span> <span data-ttu-id="c05b8-151">永遠不會叫用中斷點。</span><span class="sxs-lookup"><span data-stu-id="c05b8-151">The break point is never hit.</span></span>
* <span data-ttu-id="c05b8-152">使用 [Fiddler 工具](https://www.telerik.com/fiddler)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-152">Use the [Fiddler tool](https://www.telerik.com/fiddler).</span></span>
* <span data-ttu-id="c05b8-153">使用瀏覽器開發人員工具來監視網路流量。</span><span class="sxs-lookup"><span data-stu-id="c05b8-153">Use the browser developer tools to monitor network traffic.</span></span>

### <a name="server-side-validation"></a><span data-ttu-id="c05b8-154">伺服器端驗證</span><span class="sxs-lookup"><span data-stu-id="c05b8-154">Server-side validation</span></span>

<span data-ttu-id="c05b8-155">在瀏覽器中停用 JavaScript 時，提交含有錯誤的表單將發佈到伺服器。</span><span class="sxs-lookup"><span data-stu-id="c05b8-155">When JavaScript is disabled in the browser, submitting the form with errors will post to the server.</span></span>

<span data-ttu-id="c05b8-156">選擇性地測試伺服器端驗證：</span><span class="sxs-lookup"><span data-stu-id="c05b8-156">Optional, test server-side validation:</span></span>

* <span data-ttu-id="c05b8-157">在瀏覽器中停用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c05b8-157">Disable JavaScript in the browser.</span></span> <span data-ttu-id="c05b8-158">您可以使用瀏覽器的開發人員工具來停用 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="c05b8-158">You can disable JavaScript using browser's developer tools.</span></span> <span data-ttu-id="c05b8-159">如果您無法在該瀏覽器中停用 JavaScript，請嘗試另一個瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="c05b8-159">If you can't disable JavaScript in the browser, try another browser.</span></span>
* <span data-ttu-id="c05b8-160">在 Create 或 Edit 頁面的 `OnPostAsync` 方法中設定中斷點。</span><span class="sxs-lookup"><span data-stu-id="c05b8-160">Set a break point in the `OnPostAsync` method of the Create or Edit page.</span></span>
* <span data-ttu-id="c05b8-161">提交含有無效資料的表單。</span><span class="sxs-lookup"><span data-stu-id="c05b8-161">Submit a form with invalid data.</span></span>
* <span data-ttu-id="c05b8-162">確認模型狀態無效：</span><span class="sxs-lookup"><span data-stu-id="c05b8-162">Verify the model state is invalid:</span></span>

  ```csharp
   if (!ModelState.IsValid)
   {
      return Page();
   }
  ```
  
<span data-ttu-id="c05b8-163">或者，您可以[停用伺服器上的用戶端驗證](xref:mvc/models/validation#disable-client-side-validation)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-163">Alternatively, you can [Disable client-side validation on the server](xref:mvc/models/validation#disable-client-side-validation).</span></span>

<span data-ttu-id="c05b8-164">下列程式碼顯示稍早在此教學課程中包含 Scaffold 的部分 *Create.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="c05b8-164">The following code shows a portion of the *Create.cshtml* page scaffolded earlier in the tutorial.</span></span> <span data-ttu-id="c05b8-165">Create 和 Edit 頁面會使用它來顯示初始表單，以及在發生錯誤時重新顯示格式。</span><span class="sxs-lookup"><span data-stu-id="c05b8-165">It's used by the Create and Edit pages to display the initial form and to redisplay the form in the event of an error.</span></span>

[!code-cshtml[](razor-pages-start/sample/RazorPagesMovie/Pages/Movies/Create.cshtml?range=14-20)]

<span data-ttu-id="c05b8-166">[輸入標記協助程式](xref:mvc/views/working-with-forms)會使用 [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) 屬性，並產生在用戶端上進行 jQuery 驗證所需的 HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-166">The [Input Tag Helper](xref:mvc/views/working-with-forms) uses the [DataAnnotations](/aspnet/mvc/overview/older-versions/mvc-music-store/mvc-music-store-part-6) attributes and produces HTML attributes needed for jQuery Validation on the client-side.</span></span> <span data-ttu-id="c05b8-167">[驗證標記協助程式](xref:mvc/views/working-with-forms#the-validation-tag-helpers)會顯示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="c05b8-167">The [Validation Tag Helper](xref:mvc/views/working-with-forms#the-validation-tag-helpers) displays validation errors.</span></span> <span data-ttu-id="c05b8-168">如需詳細資訊，請參閱[驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-168">See [Validation](xref:mvc/models/validation) for more information.</span></span>

<span data-ttu-id="c05b8-169">Create 和 Edit 頁面中沒有任何驗證規則。</span><span class="sxs-lookup"><span data-stu-id="c05b8-169">The Create and Edit pages have no validation rules in them.</span></span> <span data-ttu-id="c05b8-170">只有在 `Movie` 類別中才能指定驗證規則和錯誤字串。</span><span class="sxs-lookup"><span data-stu-id="c05b8-170">The validation rules and the error strings are specified only in the `Movie` class.</span></span> <span data-ttu-id="c05b8-171">這些驗證規則會自動套用至編輯 `Movie` 模型的 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="c05b8-171">These validation rules are automatically applied to Razor Pages that edit the `Movie` model.</span></span>

<span data-ttu-id="c05b8-172">當驗證邏輯需要變更時，它只會在模型中進行。</span><span class="sxs-lookup"><span data-stu-id="c05b8-172">When validation logic needs to change, it's done only in the model.</span></span> <span data-ttu-id="c05b8-173">驗證會一致地套用在整個應用程式中(驗證邏輯定義於一個位置)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-173">Validation is applied consistently throughout the application (validation logic is defined in one place).</span></span> <span data-ttu-id="c05b8-174">位於一個位置的驗證有助於讓程式碼保持整潔，並可讓您更容易進行維護和更新。</span><span class="sxs-lookup"><span data-stu-id="c05b8-174">Validation in one place helps keep the code clean, and makes it easier to maintain and update.</span></span>

## <a name="using-datatype-attributes"></a><span data-ttu-id="c05b8-175">使用 DataType 屬性</span><span class="sxs-lookup"><span data-stu-id="c05b8-175">Using DataType Attributes</span></span>

<span data-ttu-id="c05b8-176">檢查 `Movie` 類別。</span><span class="sxs-lookup"><span data-stu-id="c05b8-176">Examine the `Movie` class.</span></span> <span data-ttu-id="c05b8-177">除了一組內建的驗證屬性之外，`System.ComponentModel.DataAnnotations` 命名空間還提供了格式屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-177">The `System.ComponentModel.DataAnnotations` namespace provides formatting attributes in addition to the built-in set of validation attributes.</span></span> <span data-ttu-id="c05b8-178">`DataType` 屬性 (attribute) 會套用至 `ReleaseDate` 和 `Price` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-178">The `DataType` attribute is applied to the `ReleaseDate` and `Price` properties.</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie/Models/MovieDateRatingDA.cs?highlight=2,6&name=snippet2)]

<span data-ttu-id="c05b8-179">`DataType` 屬性只提供檢視引擎將資料格式化的提示 (以及提供一些屬性，例如用於 URL 的 `<a>` 和用於電子郵件的 `<a href="mailto:EmailAddress.com">`)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-179">The `DataType` attributes only provide hints for the view engine to format the data (and supplies attributes such as `<a>` for URL's and `<a href="mailto:EmailAddress.com">` for email).</span></span> <span data-ttu-id="c05b8-180">使用 `RegularExpression` 屬性來驗證資料的格式。</span><span class="sxs-lookup"><span data-stu-id="c05b8-180">Use the `RegularExpression` attribute to validate the format of the data.</span></span> <span data-ttu-id="c05b8-181">`DataType` 屬性可用於指定比資料庫內建類型更特定的資料類型。</span><span class="sxs-lookup"><span data-stu-id="c05b8-181">The `DataType` attribute is used to specify a data type that's more specific than the database intrinsic type.</span></span> <span data-ttu-id="c05b8-182">`DataType` 屬性不是驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-182">`DataType` attributes are not validation attributes.</span></span> <span data-ttu-id="c05b8-183">在範例應用程式中，只會顯示日期，而不含時間。</span><span class="sxs-lookup"><span data-stu-id="c05b8-183">In the sample application, only the date is displayed, without time.</span></span>

<span data-ttu-id="c05b8-184">`DataType` 列舉可提供給許多資料類型，例如 Date、Time、PhoneNumber、Currency、EmailAddress 等等。</span><span class="sxs-lookup"><span data-stu-id="c05b8-184">The `DataType` Enumeration provides for many data types, such as Date, Time, PhoneNumber, Currency, EmailAddress, and more.</span></span> <span data-ttu-id="c05b8-185">`DataType` 屬性也可讓應用程式自動提供類型的特定功能。</span><span class="sxs-lookup"><span data-stu-id="c05b8-185">The `DataType` attribute can also enable the application to automatically provide type-specific features.</span></span> <span data-ttu-id="c05b8-186">例如，可針對 `DataType.EmailAddress` 建立 `mailto:` 連結。</span><span class="sxs-lookup"><span data-stu-id="c05b8-186">For example, a `mailto:` link can be created for `DataType.EmailAddress`.</span></span> <span data-ttu-id="c05b8-187">在支援 HTML5 的瀏覽器中，可以為 `DataType.Date` 提供日期選取器。</span><span class="sxs-lookup"><span data-stu-id="c05b8-187">A date selector can be provided for `DataType.Date` in browsers that support HTML5.</span></span> <span data-ttu-id="c05b8-188">`DataType` 屬性會發出 HTML 5 瀏覽器使用的 HTML 5 `data-` (唸成的 data dash) 屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-188">The `DataType` attributes emits HTML 5 `data-` (pronounced data dash) attributes that HTML 5 browsers consume.</span></span> <span data-ttu-id="c05b8-189">`DataType` 屬性**不**會提供任何驗證。</span><span class="sxs-lookup"><span data-stu-id="c05b8-189">The `DataType` attributes do **not** provide any validation.</span></span>

<span data-ttu-id="c05b8-190">`DataType.Date` 未指定顯示日期的格式。</span><span class="sxs-lookup"><span data-stu-id="c05b8-190">`DataType.Date` doesn't specify the format of the date that's displayed.</span></span> <span data-ttu-id="c05b8-191">根據預設，將依據以伺服器 `CultureInfo` 為基礎的預設格式顯示資料欄位。</span><span class="sxs-lookup"><span data-stu-id="c05b8-191">By default, the data field is displayed according to the default formats based on the server's `CultureInfo`.</span></span>

<span data-ttu-id="c05b8-192">`[Column(TypeName = "decimal(18, 2)")]` 資料註解為必要項，因此 Entity Framework Core 可將 `Price` 正確對應到資料庫中的貨幣。</span><span class="sxs-lookup"><span data-stu-id="c05b8-192">The `[Column(TypeName = "decimal(18, 2)")]` data annotation is required so Entity Framework Core can correctly map `Price` to currency in the database.</span></span> <span data-ttu-id="c05b8-193">如需詳細資訊，請參閱[資料類型](/ef/core/modeling/relational/data-types)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-193">For more information, see [Data Types](/ef/core/modeling/relational/data-types).</span></span>

<span data-ttu-id="c05b8-194">`DisplayFormat` 屬性用來明確指定日期格式：</span><span class="sxs-lookup"><span data-stu-id="c05b8-194">The `DisplayFormat` attribute is used to explicitly specify the date format:</span></span>

```csharp
[DisplayFormat(DataFormatString = "{0:yyyy-MM-dd}", ApplyFormatInEditMode = true)]
public DateTime ReleaseDate { get; set; }
```

<span data-ttu-id="c05b8-195">`ApplyFormatInEditMode` 設定可指定當顯示值以供編輯時，應該套用的格式。</span><span class="sxs-lookup"><span data-stu-id="c05b8-195">The `ApplyFormatInEditMode` setting specifies that the formatting should be applied when the value is displayed for editing.</span></span> <span data-ttu-id="c05b8-196">但在某些欄位中，您可能不想要套用該行為。</span><span class="sxs-lookup"><span data-stu-id="c05b8-196">You might not want that behavior for some fields.</span></span> <span data-ttu-id="c05b8-197">例如，在貨幣值中，應該不會希望編輯 UI 中出現貨幣符號。</span><span class="sxs-lookup"><span data-stu-id="c05b8-197">For example, in currency values, you probably don't want the currency symbol in the edit UI.</span></span>

<span data-ttu-id="c05b8-198">`DisplayFormat` 屬性可以單獨使用，但通常最好是使用 `DataType` 屬性。</span><span class="sxs-lookup"><span data-stu-id="c05b8-198">The `DisplayFormat` attribute can be used by itself, but it's generally a good idea to use the `DataType` attribute.</span></span> <span data-ttu-id="c05b8-199">`DataType` 屬性會傳逹資料的語意，而不是在螢幕上呈現資料的方式，並提供下列使用 DisplayFormat 無法取得的優點：</span><span class="sxs-lookup"><span data-stu-id="c05b8-199">The `DataType` attribute conveys the semantics of the data as opposed to how to render it on a screen, and provides the following benefits that you don't get with DisplayFormat:</span></span>

* <span data-ttu-id="c05b8-200">瀏覽器可以啟用 HTML5 功能 (例如顯示日曆控制項、適合地區設定的貨幣符號、電子郵件連結等等)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-200">The browser can enable HTML5 features (for example to show a calendar control, the locale-appropriate currency symbol, email links, etc.)</span></span>
* <span data-ttu-id="c05b8-201">根據預設，瀏覽器將根據您的地區設定，使用正確的格式呈現資料。</span><span class="sxs-lookup"><span data-stu-id="c05b8-201">By default, the browser will render data using the correct format based on your locale.</span></span>
* <span data-ttu-id="c05b8-202">`DataType` 屬性可以啟用 ASP.NET Core 架構，來選擇用於呈現資料的正確欄位範本。</span><span class="sxs-lookup"><span data-stu-id="c05b8-202">The `DataType` attribute can enable the ASP.NET Core framework to choose the right field template to render the data.</span></span> <span data-ttu-id="c05b8-203">`DisplayFormat` 若是單獨使用，則會使用字串範本。</span><span class="sxs-lookup"><span data-stu-id="c05b8-203">The `DisplayFormat` if used by itself uses the string template.</span></span>

<span data-ttu-id="c05b8-204">請注意：jQuery 驗證不適用於 `Range` 屬性與 `DateTime`。</span><span class="sxs-lookup"><span data-stu-id="c05b8-204">Note: jQuery validation doesn't work with the `Range` attribute and `DateTime`.</span></span> <span data-ttu-id="c05b8-205">例如，下列程式碼一律會顯示用戶端驗證錯誤，即使當日期位在指定範圍中也一樣：</span><span class="sxs-lookup"><span data-stu-id="c05b8-205">For example, the following code will always display a client-side validation error, even when the date is in the specified range:</span></span>

```csharp
[Range(typeof(DateTime), "1/1/1966", "1/1/2020")]
   ```

<span data-ttu-id="c05b8-206">編譯模型中的硬性日期通常不是良好的做法，因此不建議使用 `Range` 屬性和 `DateTime`。</span><span class="sxs-lookup"><span data-stu-id="c05b8-206">It's generally not a good practice to compile hard dates in your models, so using the `Range` attribute and `DateTime` is discouraged.</span></span>

<span data-ttu-id="c05b8-207">下列程式碼會顯示一行上的結合屬性：</span><span class="sxs-lookup"><span data-stu-id="c05b8-207">The following code shows combining attributes on one line:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDAmult.cs?name=snippet1)]

<span data-ttu-id="c05b8-208">[Razor Pages 和 EF Core 使用者入門](xref:data/ef-rp/intro)說明使用 Razor Pages 執行進階 EF Core 作業。</span><span class="sxs-lookup"><span data-stu-id="c05b8-208">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) shows advanced EF Core operations with Razor Pages.</span></span>

### <a name="apply-migrations"></a><span data-ttu-id="c05b8-209">套用移轉</span><span class="sxs-lookup"><span data-stu-id="c05b8-209">Apply migrations</span></span>

<span data-ttu-id="c05b8-210">套用至類別的 DataAnnotations 會變更結構描述。</span><span class="sxs-lookup"><span data-stu-id="c05b8-210">The DataAnnotations applied to the class change the schema.</span></span> <span data-ttu-id="c05b8-211">例如，套用至 `Title` 欄位的 DataAnnotations：</span><span class="sxs-lookup"><span data-stu-id="c05b8-211">For example, the DataAnnotations applied to the `Title` field:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet11)]

* <span data-ttu-id="c05b8-212">將字元限制為 60 個。</span><span class="sxs-lookup"><span data-stu-id="c05b8-212">Limits the characters to 60.</span></span>
* <span data-ttu-id="c05b8-213">不允許 `null` 值。</span><span class="sxs-lookup"><span data-stu-id="c05b8-213">Doesn't allow a `null` value.</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="c05b8-214">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="c05b8-214">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="c05b8-215">`Movie` 資料表目前具有下列結構描述：</span><span class="sxs-lookup"><span data-stu-id="c05b8-215">The `Movie` table currently has the following schema:</span></span>

```sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (MAX)  NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (MAX)  NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (MAX)  NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

<span data-ttu-id="c05b8-216">上述結構描述變更不會造成 EF 擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="c05b8-216">The preceding schema changes don't cause EF to throw an exception.</span></span> <span data-ttu-id="c05b8-217">不過，請建立移轉，讓結構描述與模型一致。</span><span class="sxs-lookup"><span data-stu-id="c05b8-217">However, create a migration so the schema is consistent with the model.</span></span>

<span data-ttu-id="c05b8-218"> 從 [工具\]\*\*\** 功能表中，選取 [NuGet 套件管理員] > [套件管理員主控台\]\*\*\**。</span><span class="sxs-lookup"><span data-stu-id="c05b8-218">From the **Tools** menu, select **NuGet Package Manager > Package Manager Console**.</span></span>
<span data-ttu-id="c05b8-219">在 PMC 中，輸入下列命令：</span><span class="sxs-lookup"><span data-stu-id="c05b8-219">In the PMC, enter the following commands:</span></span>

```powershell
Add-Migration New_DataAnnotations
Update-Database
```

<span data-ttu-id="c05b8-220">`Update-Database` 會執行 `New_DataAnnotations` 類別的 `Up` 方法。</span><span class="sxs-lookup"><span data-stu-id="c05b8-220">`Update-Database` runs the `Up` methods of the `New_DataAnnotations` class.</span></span> <span data-ttu-id="c05b8-221">檢查 `Up` 方法：</span><span class="sxs-lookup"><span data-stu-id="c05b8-221">Examine the `Up` method:</span></span>

[!code-csharp[](razor-pages-start/sample/RazorPagesMovie30/Migrations/20190724163003_New_DataAnnotations.cs?name=snippet)]

<span data-ttu-id="c05b8-222">已更新的 `Movie` 資料表具有下列結構描述：</span><span class="sxs-lookup"><span data-stu-id="c05b8-222">The updated `Movie` table has the following schema:</span></span>

```sql
CREATE TABLE [dbo].[Movie] (
    [ID]          INT             IDENTITY (1, 1) NOT NULL,
    [Title]       NVARCHAR (60)   NOT NULL,
    [ReleaseDate] DATETIME2 (7)   NOT NULL,
    [Genre]       NVARCHAR (30)   NOT NULL,
    [Price]       DECIMAL (18, 2) NOT NULL,
    [Rating]      NVARCHAR (5)    NOT NULL,
    CONSTRAINT [PK_Movie] PRIMARY KEY CLUSTERED ([ID] ASC)
);
```

# <a name="visual-studio-code--visual-studio-for-mac"></a>[<span data-ttu-id="c05b8-223">Visual Studio Code / Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="c05b8-223">Visual Studio Code / Visual Studio for Mac</span></span>](#tab/visual-studio-code+visual-studio-mac)

<span data-ttu-id="c05b8-224">SQLite 不需要移轉。</span><span class="sxs-lookup"><span data-stu-id="c05b8-224">Migrations are not required for SQLite.</span></span>

---

### <a name="publish-to-azure"></a><span data-ttu-id="c05b8-225">發佈至 Azure</span><span class="sxs-lookup"><span data-stu-id="c05b8-225">Publish to Azure</span></span>

<span data-ttu-id="c05b8-226">如需部署至 Azure 的詳細資訊，請參閱[教學課程：在 Azure 中使用 SQL Database 建立 ASP.NET Core 應用程式](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb)。</span><span class="sxs-lookup"><span data-stu-id="c05b8-226">For information on deploying to Azure, see [Tutorial: Build an ASP.NET Core app in Azure with SQL Database](/azure/app-service/app-service-web-tutorial-dotnetcore-sqldb).</span></span>

<span data-ttu-id="c05b8-227">感謝您完成這篇Razor頁面簡介。</span><span class="sxs-lookup"><span data-stu-id="c05b8-227">Thanks for completing this introduction to Razor Pages.</span></span> <span data-ttu-id="c05b8-228">[開始使用Razor頁面和 EF Core](xref:data/ef-rp/intro)是本教學課程的絕佳追蹤。</span><span class="sxs-lookup"><span data-stu-id="c05b8-228">[Get started with Razor Pages and EF Core](xref:data/ef-rp/intro) is an excellent follow up to this tutorial.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c05b8-229">其他資源</span><span class="sxs-lookup"><span data-stu-id="c05b8-229">Additional resources</span></span>

* <xref:mvc/views/working-with-forms>
* <xref:fundamentals/localization>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/authoring>
* [<span data-ttu-id="c05b8-230">本教學課程的 YouTube 版本</span><span class="sxs-lookup"><span data-stu-id="c05b8-230">YouTube version of this tutorial</span></span>](https://youtu.be/b63m66eu7us)

> [!div class="step-by-step"]
> [<span data-ttu-id="c05b8-231">上一步：新增欄位</span><span class="sxs-lookup"><span data-stu-id="c05b8-231">Previous: Adding a new field</span></span>](xref:tutorials/razor-pages/new-field)
