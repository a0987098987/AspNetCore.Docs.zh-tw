---
title: ASP.NET Core MVC 中的模型驗證
author: rachelappel
description: 了解 ASP.NET Core MVC 中的模型驗證。
manager: wpickett
ms.author: riande
ms.date: 12/18/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: mvc/models/validation
ms.openlocfilehash: 1ab19fad90eab9f2da58b4d62615a85d71894218
ms.sourcegitcommit: 5130b3034165f5cf49d829fe7475a84aa33d2693
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/03/2018
---
# <a name="model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="cc57b-103">ASP.NET Core MVC 中的模型驗證</span><span class="sxs-lookup"><span data-stu-id="cc57b-103">Model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="cc57b-104">作者：[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="cc57b-104">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="cc57b-105">模型驗證簡介</span><span class="sxs-lookup"><span data-stu-id="cc57b-105">Introduction to model validation</span></span>

<span data-ttu-id="cc57b-106">應用程式必須驗證資料，才能將資料儲存在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="cc57b-106">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="cc57b-107">資料必須檢查是否有潛在安全性威脅、確認其類型和大小是否適當地格式化，而且必須符合您的規則。</span><span class="sxs-lookup"><span data-stu-id="cc57b-107">Data must be checked for potential security threats, verified that it's appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="cc57b-108">驗證是必要的，但它在實作方面可能鎖碎繁複。</span><span class="sxs-lookup"><span data-stu-id="cc57b-108">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="cc57b-109">在 MVC 中，驗證會在用戶端和伺服器上進行。</span><span class="sxs-lookup"><span data-stu-id="cc57b-109">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="cc57b-110">但幸好 .NET 已將驗證抽象化為驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-110">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="cc57b-111">這些屬性包含驗證程式碼，藉此減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="cc57b-111">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="cc57b-112">[從 GitHub 檢視或下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-112">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="cc57b-113">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="cc57b-113">Validation Attributes</span></span>

<span data-ttu-id="cc57b-114">驗證屬性是設定模型驗證的方式，因此概念上類似於資料庫資料表中的欄位驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-114">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="cc57b-115">包括條件約束，例如指派資料類型或必要欄位。</span><span class="sxs-lookup"><span data-stu-id="cc57b-115">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="cc57b-116">其他驗證類型還包括將模式套用至資料以強制執行商務規則，例如信用卡、電話號碼或電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="cc57b-116">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="cc57b-117">驗證屬性可讓您更輕鬆地強制執行這些需求，而且使用起來更容易。</span><span class="sxs-lookup"><span data-stu-id="cc57b-117">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="cc57b-118">以下是應用程式中已註解的 `Movie` 模型，其儲存電影和電視節目的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="cc57b-118">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="cc57b-119">大部分屬性都是必要的，而且有幾個字串屬性具有長度需求。</span><span class="sxs-lookup"><span data-stu-id="cc57b-119">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="cc57b-120">此外，`Price` 屬性還有 0 至 $999.99 的數字範圍限制，以及自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-120">Additionally, there's a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="cc57b-121">只要透過模型讀取就會顯示此應用程式資料的相關規則，因此更容易維護程式碼。</span><span class="sxs-lookup"><span data-stu-id="cc57b-121">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="cc57b-122">以下是幾個常用的內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="cc57b-122">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="cc57b-123">`[CreditCard]`：驗證屬性具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-123">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="cc57b-124">`[Compare]`：驗證模型比對中的兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-124">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="cc57b-125">`[EmailAddress]`：驗證屬性具有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-125">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="cc57b-126">`[Phone]`：驗證屬性具有電話格式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-126">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="cc57b-127">`[Range]`：驗證屬性值落在指定範圍內。</span><span class="sxs-lookup"><span data-stu-id="cc57b-127">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="cc57b-128">`[RegularExpression]`：驗證資料符合指定的規則運算式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-128">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="cc57b-129">`[Required]`：讓屬性成為必要。</span><span class="sxs-lookup"><span data-stu-id="cc57b-129">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="cc57b-130">`[StringLength]`：驗證字串屬性的長度不超過指定的上限。</span><span class="sxs-lookup"><span data-stu-id="cc57b-130">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="cc57b-131">`[Url]`：驗證屬性具有 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-131">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="cc57b-132">MVC 支援將任何衍生自 `ValidationAttribute` 的屬性用於驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-132">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="cc57b-133">您可以在 [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) 命名空間中找到許多實用的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-133">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="cc57b-134">有時候您可能需要比內建屬性所提供更多的功能。</span><span class="sxs-lookup"><span data-stu-id="cc57b-134">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="cc57b-135">此時，您可以藉由衍生自 `ValidationAttribute` 或變更您的模型來實作 `IValidatableObject`，以建立自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-135">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="cc57b-136">使用必要屬性的注意事項</span><span class="sxs-lookup"><span data-stu-id="cc57b-136">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="cc57b-137">不可為 Null 的[實值類型](/dotnet/csharp/language-reference/keywords/value-types) (如 `decimal`、`int`、`float` 和 `DateTime`) 原本就是必要項目，而且不需要 `Required` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-137">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="cc57b-138">應用程式不會對標示為 `Required` 之不可為 Null 的型別執行伺服器端驗證檢查。</span><span class="sxs-lookup"><span data-stu-id="cc57b-138">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="cc57b-139">與驗證和驗證屬性無關的 MVC 模型繫結，會拒絕送出含有遺漏值或空白字元之不可為 Null 型別的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="cc57b-139">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="cc57b-140">如果目標屬性 (property) 上沒有 `BindRequired` 屬性 (attribute)，模型繫結會忽略不可為 Null 型別的遺漏資料 (傳入表單資料中不會有表單欄位)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-140">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="cc57b-141">[BindRequired 屬性](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (另請參閱[使用屬性自訂模型繫結行為](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) 有助於確保表單資料完整。</span><span class="sxs-lookup"><span data-stu-id="cc57b-141">The [BindRequired attribute](/dotnet/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="cc57b-142">套用至屬性時，模型繫結系統需要該屬性的值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-142">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="cc57b-143">套用至類型時，模型繫結系統需要該類型之所有屬性的值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-143">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="cc57b-144">當您使用 [Nullable\<T> 類型](/dotnet/csharp/programming-guide/nullable-types/) (例如 `decimal?` 或 `System.Nullable<decimal>`) 並將它標示為 `Required` 時，就會將屬性視為標準可為 Null 的型別 (例如 `string`) 來執行伺服器端驗證檢查。</span><span class="sxs-lookup"><span data-stu-id="cc57b-144">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="cc57b-145">用戶端驗證要求表單欄位的值必須對應至模型屬性 (若已標示為 `Required`)，以及不可為 Null 型別的屬性 (若未標示為 `Required`)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-145">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="cc57b-146">`Required` 可用來控制用戶端驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-146">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="cc57b-147">模型狀態</span><span class="sxs-lookup"><span data-stu-id="cc57b-147">Model State</span></span>

<span data-ttu-id="cc57b-148">模型狀態代表送出之 HTML 表單值中的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="cc57b-148">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="cc57b-149">MVC 會繼續驗證欄位，直到達到最大錯誤數目為止 (預設為 200)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-149">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="cc57b-150">您可以將下列程式碼插入 *Startup.cs* 檔案中的 `ConfigureServices` 方法，來設定這個數值：</span><span class="sxs-lookup"><span data-stu-id="cc57b-150">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="cc57b-151">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="cc57b-151">Handling Model State Errors</span></span>

<span data-ttu-id="cc57b-152">模型驗證會在叫用每個控制器動之前發生，而動作方法必須負責檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="cc57b-152">Model validation occurs prior to each controller action being invoked, and it's the action method's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="cc57b-153">在許多情況下，適當回應就是傳回錯誤回應，最好能夠詳細列出模型驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="cc57b-153">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="cc57b-154">某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤，在此情況下，篩選可能是實作這類原則的適當位置。</span><span class="sxs-lookup"><span data-stu-id="cc57b-154">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="cc57b-155">您應該測試動作在有效和無效模型狀態中的行為。</span><span class="sxs-lookup"><span data-stu-id="cc57b-155">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="cc57b-156">手動驗證</span><span class="sxs-lookup"><span data-stu-id="cc57b-156">Manual validation</span></span>

<span data-ttu-id="cc57b-157">模型繫結和驗證完成之後，您可能想要重複其中數個部分。</span><span class="sxs-lookup"><span data-stu-id="cc57b-157">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="cc57b-158">例如，使用者可能在預期整數的欄位中輸入文字，或是您可能需要計算模型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-158">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="cc57b-159">此時，您可能需要手動執行驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-159">You may need to run validation manually.</span></span> <span data-ttu-id="cc57b-160">若要執行這項操作，請呼叫 `TryValidateModel` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-160">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="cc57b-161">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="cc57b-161">Custom validation</span></span>

<span data-ttu-id="cc57b-162">驗證屬性符合大多數驗證需求。</span><span class="sxs-lookup"><span data-stu-id="cc57b-162">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="cc57b-163">不過，某些驗證規則是您業務特有的。</span><span class="sxs-lookup"><span data-stu-id="cc57b-163">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="cc57b-164">您的規則可能不是常用的資料驗證技術，例如確定欄位為必要或符合某個範圍的值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-164">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="cc57b-165">在此情況下，自訂驗證屬性會是很好的解決方案。</span><span class="sxs-lookup"><span data-stu-id="cc57b-165">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="cc57b-166">在 MVC 中建立您自己的自訂驗證屬性十分簡單。</span><span class="sxs-lookup"><span data-stu-id="cc57b-166">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="cc57b-167">只要繼承自 `ValidationAttribute` 並覆寫 `IsValid` 方法即可。</span><span class="sxs-lookup"><span data-stu-id="cc57b-167">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="cc57b-168">`IsValid` 方法接受兩個參數，第一個是名為 *value* 的物件，第二個是名為 *validationContext* 的 `ValidationContext` 物件。</span><span class="sxs-lookup"><span data-stu-id="cc57b-168">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="cc57b-169">*Value* 是指您的自訂驗證程式正在驗證之欄位中的實際值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-169">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="cc57b-170">在下列範例中，商務規則表示使用者可能未將 1960 年以後發行之電影的內容類型設定為 *Classic*。</span><span class="sxs-lookup"><span data-stu-id="cc57b-170">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="cc57b-171">`[ClassicMovie]` 屬性會先檢查內容類型，如果是 Classic，會再檢查發行日期是否晚於 1960 年。</span><span class="sxs-lookup"><span data-stu-id="cc57b-171">The `[ClassicMovie]` attribute checks the genre first, and if it's a classic, then it checks the release date to see that it's later than 1960.</span></span> <span data-ttu-id="cc57b-172">如果是在 1960 年以後發行，則驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="cc57b-172">If it's released after 1960, validation fails.</span></span> <span data-ttu-id="cc57b-173">用來驗證資料的屬性接受代表年份的整數參數。</span><span class="sxs-lookup"><span data-stu-id="cc57b-173">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="cc57b-174">您可以擷取屬性建構函式中的參數值，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-174">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

<span data-ttu-id="cc57b-175">上述 `movie` 變數代表 `Movie` 物件，其中包含送出待驗證之表單中的資料。</span><span class="sxs-lookup"><span data-stu-id="cc57b-175">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="cc57b-176">在本例中，驗證程式碼會根據規則，檢查 `ClassicMovieAttribute` 類別之 `IsValid` 方法中的日期和內容類型。</span><span class="sxs-lookup"><span data-stu-id="cc57b-176">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="cc57b-177">成功驗證時，`IsValid` 會傳回 `ValidationResult.Success` 代碼；驗證失敗時，則會傳回 `ValidationResult` 並顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-177">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="cc57b-178">當使用者修改 `Genre` 欄位並送出表單時，`ClassicMovieAttribute` 的 `IsValid` 方法會確認電影是否為 Classic。</span><span class="sxs-lookup"><span data-stu-id="cc57b-178">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="cc57b-179">如同任何內建屬性，將 `ClassicMovieAttribute` 套用至 `ReleaseDate` 等屬性可確保進行驗證，如上述程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="cc57b-179">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="cc57b-180">由於此範例僅適用於 `Movie` 類型，使用 `IValidatableObject` 會是更好的選擇，如下一個段落所示。</span><span class="sxs-lookup"><span data-stu-id="cc57b-180">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="cc57b-181">或者，您也可以透過在 `IValidatableObject` 介面上實作 `Validate` 方法，將此相同的程式碼放在模型中。</span><span class="sxs-lookup"><span data-stu-id="cc57b-181">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="cc57b-182">自訂驗證屬性適用於驗證個別屬性，而實作 `IValidatableObject` 則可用來實作類別層級驗證，如下所示。</span><span class="sxs-lookup"><span data-stu-id="cc57b-182">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="cc57b-183">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="cc57b-183">Client side validation</span></span>

<span data-ttu-id="cc57b-184">用戶端驗證對使用者而言很方便。</span><span class="sxs-lookup"><span data-stu-id="cc57b-184">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="cc57b-185">它為使用者省下花在等候伺服器往返的時間。</span><span class="sxs-lookup"><span data-stu-id="cc57b-185">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="cc57b-186">對商務而言，即便是幾秒，若每天乘上好幾百倍，也可能讓您花上許多時間和費用，而倍感挫折。</span><span class="sxs-lookup"><span data-stu-id="cc57b-186">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="cc57b-187">直接和立即驗證可讓使用者更有效率地工作，並產生更佳品質的輸入和輸出。</span><span class="sxs-lookup"><span data-stu-id="cc57b-187">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="cc57b-188">您必須具有適當 JavaScript 指令碼參考的檢視，以確保用戶端驗證如下所示正常運作。</span><span class="sxs-lookup"><span data-stu-id="cc57b-188">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="cc57b-189">[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。</span><span class="sxs-lookup"><span data-stu-id="cc57b-189">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="cc57b-190">若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (property) 上的伺服器端驗證屬性 (attribute)，另一次在用戶端指令碼 (jQuery 驗證的 [`validate()`](https://jqueryvalidation.org/validate/) 方法範例顯示這可能會變得多麼複雜)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-190">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="cc57b-191">相反地，MVC 的[標籤協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)能夠使用模型屬性 (property) 中的驗證屬性 (attribute) 和類型中繼資料，來轉譯需要驗證之表單項目中的 HTML 5 [data- 屬性 (attribute)](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-191">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="cc57b-192">MVC 針對內建和自訂屬性都會產生 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-192">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="cc57b-193">jQuery 低調驗證會接著剖析這些 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。</span><span class="sxs-lookup"><span data-stu-id="cc57b-193">jQuery Unobtrusive Validation then parses thes `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="cc57b-194">您可以使用相關的標籤協助程式，來顯示用戶端的驗證錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-194">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="cc57b-195">上述標籤協助程式會轉譯下列 HTML。</span><span class="sxs-lookup"><span data-stu-id="cc57b-195">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="cc57b-196">請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-196">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="cc57b-197">下面的 `data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-197">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="cc57b-198">jQuery 低調驗證會將此值傳遞至 jQuery 驗證的 [`required()`](https://jqueryvalidation.org/required-method/) 方法，然後在隨附的 **\<span>** 項目中顯示該訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-198">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

```html
<form action="/Movies/Create" method="post">
    <div class="form-horizontal">
        <h4>Movie</h4>
        <div class="text-danger"></div>
        <div class="form-group">
            <label class="col-md-2 control-label" for="ReleaseDate">ReleaseDate</label>
            <div class="col-md-10">
                <input class="form-control" type="datetime"
                data-val="true" data-val-required="The ReleaseDate field is required."
                id="ReleaseDate" name="ReleaseDate" value="" />
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="cc57b-199">用戶端驗證可避免送出無效的表單。</span><span class="sxs-lookup"><span data-stu-id="cc57b-199">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="cc57b-200">[送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-200">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="cc57b-201">MVC 會根據屬性 (property) 的 .NET 資料類型來決定類型屬性 (attribute) 值，並可能使用 `[DataType]` 屬性 (attribute) 加以覆寫。</span><span class="sxs-lookup"><span data-stu-id="cc57b-201">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="cc57b-202">基底 `[DataType]` 屬性不會執行任何實際伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-202">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="cc57b-203">瀏覽器會選擇自己的錯誤訊息，並按照想要的方式來顯示這些錯誤，不過 jQuery 低調驗證套件可能會覆寫這些訊息，並以與其他訊息一致的方式來顯示。</span><span class="sxs-lookup"><span data-stu-id="cc57b-203">Browsers choose their own error messages and display those errors as they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="cc57b-204">當使用者套用 `[DataType]` 子類別 (例如 `[EmailAddress]`) 時，最有可能發生此情況。</span><span class="sxs-lookup"><span data-stu-id="cc57b-204">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="cc57b-205">將驗證新增至動態表單</span><span class="sxs-lookup"><span data-stu-id="cc57b-205">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="cc57b-206">由於 jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證，因此動態產生的表單不會自動展示驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-206">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms won't automatically exhibit validation.</span></span> <span data-ttu-id="cc57b-207">相反地，您必須指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。</span><span class="sxs-lookup"><span data-stu-id="cc57b-207">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="cc57b-208">例如，下列程式碼示範如何在透過 AJAX 新增的表單上設定用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-208">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add form. " + errorThrown);
    },
    success: function(newFormHTML) {
        var container = document.getElementById("form-container");
        container.insertAdjacentHTML("beforeend", newFormHTML);
        var forms = container.getElementsByTagName("form");
        var newForm = forms[forms.length - 1];
        $.validator.unobtrusive.parse(newForm);
    }
})
```

<span data-ttu-id="cc57b-209">`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。</span><span class="sxs-lookup"><span data-stu-id="cc57b-209">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="cc57b-210">此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-210">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="cc57b-211">這些屬性的值會接著傳遞至 jQuery 驗證外掛程式，讓表單展示所需的用戶端驗證規則。</span><span class="sxs-lookup"><span data-stu-id="cc57b-211">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="cc57b-212">將驗證新增至動態控制項</span><span class="sxs-lookup"><span data-stu-id="cc57b-212">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="cc57b-213">您也可以在動態產生個別控制項 (例如 `<input/>` 和 `<select/>`) 時，更新表單上的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="cc57b-213">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="cc57b-214">您無法將這些項目的選取器直接傳遞至 `parse()` 方法，因為周圍的表單已經過剖析，因此不會更新。</span><span class="sxs-lookup"><span data-stu-id="cc57b-214">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and won't update.</span></span> <span data-ttu-id="cc57b-215">相反地，您會先移除現有的驗證資料，再重新剖析整份表單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-215">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Couldn't add control. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        $(form).removeData("validator")    // Added by jQuery Validate
               .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="cc57b-216">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="cc57b-216">IClientModelValidator</span></span>

<span data-ttu-id="cc57b-217">您可以建立自訂屬性的用戶端端邏輯，為 [jQuery Validation](http://jqueryvalidation.org/documentation/) 建立配接器的[低調驗證](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) 會將它當作驗證的一部分自動為您在用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="cc57b-217">You may create client side logic for your custom attribute, and [unobtrusive validation](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html) which creates an adapter to [jquery validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="cc57b-218">第一個步驟是實作 `IClientModelValidator` 介面，以控制要新增的 data- 屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-218">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="cc57b-219">實作此介面的屬性可以新增至 HTML 屬性來產生欄位。</span><span class="sxs-lookup"><span data-stu-id="cc57b-219">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="cc57b-220">檢查 `ReleaseDate` 項目的輸出會顯示類似於上述範例的 HTML，不同之處在於現在有 `IClientModelValidator` 的 `AddValidation` 方法中所定義的 `data-val-classicmovie` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-220">Examining the output for the `ReleaseDate` element reveals HTML that's similar to the previous example, except now there's a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="cc57b-221">低調驗證使用 `data-` 屬性中的資料來顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-221">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="cc57b-222">不過，在您將規則或訊息新增至 jQuery 的 `validator` 物件之前，jQuery 並不清楚有哪些規則或訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-222">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="cc57b-223">如下列範例所示，該範例會將名為 `classicmovie` 的方法新增至 jQuery `validator` 物件，其中包含自訂用戶端驗證程式碼。</span><span class="sxs-lookup"><span data-stu-id="cc57b-223">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span> <span data-ttu-id="cc57b-224">您可以在[這裡](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)找到低調配接器新增方法的說明</span><span class="sxs-lookup"><span data-stu-id="cc57b-224">An explanations of the unobtrusive.adapters.add method can be found [here](http://bradwilson.typepad.com/blog/2010/10/mvc3-unobtrusive-validation.html)</span></span>

[!code-javascript[](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="cc57b-225">現在，jQuery 具有執行自訂 JavaScript 驗證的資訊，以及該驗證程式碼傳回 false 時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-225">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="cc57b-226">遠端驗證</span><span class="sxs-lookup"><span data-stu-id="cc57b-226">Remote validation</span></span>

<span data-ttu-id="cc57b-227">遠端驗證功能適用於需要針對伺服器上的資料來驗證用戶端上的資料之情況。</span><span class="sxs-lookup"><span data-stu-id="cc57b-227">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="cc57b-228">例如，您的應用程式可能需要確認電子郵件或使用者名稱是否已在使用中；為了執行這項操作，它必須查詢大量資料。</span><span class="sxs-lookup"><span data-stu-id="cc57b-228">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="cc57b-229">為驗證一或多個欄位而下載大型資料集會耗用太多資源，</span><span class="sxs-lookup"><span data-stu-id="cc57b-229">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="cc57b-230">也可能會公開機密資訊。</span><span class="sxs-lookup"><span data-stu-id="cc57b-230">It may also expose sensitive information.</span></span> <span data-ttu-id="cc57b-231">替代方案是提出一個往返要求來驗證一個欄位。</span><span class="sxs-lookup"><span data-stu-id="cc57b-231">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="cc57b-232">您可以在兩個步驟的程序中實作遠端驗證。</span><span class="sxs-lookup"><span data-stu-id="cc57b-232">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="cc57b-233">首先，您必須為模型加註 `[Remote]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="cc57b-233">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="cc57b-234">`[Remote]` 屬性接受多個多載，您可以使用這些多載將用戶端 JavaScript 導向至適當的程式碼進行呼叫。</span><span class="sxs-lookup"><span data-stu-id="cc57b-234">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="cc57b-235">下列範例指向 `Users` 控制器的 `VerifyEmail` 動作方法。</span><span class="sxs-lookup"><span data-stu-id="cc57b-235">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="cc57b-236">第二個步驟將驗證程式碼放在 `[Remote]` 屬性中所定義的對應動作方法中。</span><span class="sxs-lookup"><span data-stu-id="cc57b-236">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="cc57b-237">根據 jQuery 驗證的 [`remote()`](https://jqueryvalidation.org/remote-method/) 方法文件：</span><span class="sxs-lookup"><span data-stu-id="cc57b-237">According to the jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) method documentation:</span></span>

> <span data-ttu-id="cc57b-238">伺服器端回應必須是使用預設錯誤訊息的 JSON 字串，若項目有效則必須是 `"true"`，若項目無效則可能是 `"false"`、`undefined` 或 `null`。</span><span class="sxs-lookup"><span data-stu-id="cc57b-238">The serverside response must be a JSON string that must be `"true"` for valid elements, and can be `"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span> <span data-ttu-id="cc57b-239">如果伺服器端回應是一個字串 (例如</span><span class="sxs-lookup"><span data-stu-id="cc57b-239">If the serverside response is a string, eg.</span></span> <span data-ttu-id="cc57b-240">`"That name is already taken, try peter123 instead"`)，則會顯示此字串以取代預設值作為自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-240">`"That name is already taken, try peter123 instead"`, this string will be displayed as a custom error message in place of the default.</span></span>

<span data-ttu-id="cc57b-241">`VerifyEmail()` 方法的定義遵循這些規則，如下所示。</span><span class="sxs-lookup"><span data-stu-id="cc57b-241">The definition of the `VerifyEmail()` method follows these rules, as shown below.</span></span> <span data-ttu-id="cc57b-242">如果電子郵件已在使用中，則會傳回驗證錯誤訊息；如果電子郵件可用，則會傳回 `true`，並將結果包裝在 `JsonResult` 物件中。</span><span class="sxs-lookup"><span data-stu-id="cc57b-242">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="cc57b-243">用戶端可接著使用此傳回值繼續進行，或顯示錯誤訊息 (如有需要)。</span><span class="sxs-lookup"><span data-stu-id="cc57b-243">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="cc57b-244">現在，當使用者輸入電子郵件時，檢視中的 JavaScript 會發出遠端呼叫，以查看該電子郵件是否已在使用中；若在使用中，則會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-244">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="cc57b-245">否則，使用者可以像往常一樣送出表單。</span><span class="sxs-lookup"><span data-stu-id="cc57b-245">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="cc57b-246">`[Remote]` 屬性 (attribute) 的 `AdditionalFields` 屬性 (property) 可用於針對伺服器上的資料來驗證欄位組合。</span><span class="sxs-lookup"><span data-stu-id="cc57b-246">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span> <span data-ttu-id="cc57b-247">例如，如果上述 `User` 模型有兩個額外的屬性 `FirstName` 和 `LastName`，您可能想要確認沒有任何現有的使用者已有該組名稱。</span><span class="sxs-lookup"><span data-stu-id="cc57b-247">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="cc57b-248">您可以定義新的屬性，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-248">You define the new properties as shown in the following code:</span></span>

[!code-csharp[](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="cc57b-249">`AdditionalFields` 可能已明確設定為 `"FirstName"` 和 `"LastName"` 字串，但使用上述 [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) 運算子可簡化稍後的重構。</span><span class="sxs-lookup"><span data-stu-id="cc57b-249">`AdditionalFields` could've been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span> <span data-ttu-id="cc57b-250">執行驗證的動作方法必須接受兩個引數，一個是 `FirstName` 的值，另一個是 `LastName` 的值。</span><span class="sxs-lookup"><span data-stu-id="cc57b-250">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>

[!code-csharp[](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="cc57b-251">現在，當使用者輸入名字和姓氏時，JavaScript 會：</span><span class="sxs-lookup"><span data-stu-id="cc57b-251">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="cc57b-252">發出遠端呼叫以查看該組名稱是否已在使用中。</span><span class="sxs-lookup"><span data-stu-id="cc57b-252">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="cc57b-253">如果已在使用中，則會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="cc57b-253">If the pair has been taken, an error message is displayed.</span></span> 
* <span data-ttu-id="cc57b-254">如果不在使用中，則使用者可以送出表單。</span><span class="sxs-lookup"><span data-stu-id="cc57b-254">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="cc57b-255">如果您需要驗證具有 `[Remote]` 屬性的兩個或多個額外欄位，請以逗號分隔清單來提供這些欄位。</span><span class="sxs-lookup"><span data-stu-id="cc57b-255">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span> <span data-ttu-id="cc57b-256">例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (attribute)，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="cc57b-256">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="cc57b-257">如同所有屬性引數，`AdditionalFields` 必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="cc57b-257">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="cc57b-258">因此，您不得使用[字串插值](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) 來初始化 `AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="cc57b-258">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="cc57b-259">針對每個新增 `[Remote]` 屬性的額外欄位，都必須另外新增一個引數至控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="cc57b-259">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
