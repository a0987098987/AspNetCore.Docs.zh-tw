---
title: "ASP.NET Core MVC 中的模型驗證"
author: rachelappel
description: "深入了解 ASP.NET Core MVC 中的模型驗證。"
keywords: "ASP.NET Core，MVC、 驗證"
ms.author: riande
manager: wpickett
ms.date: 12/18/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.openlocfilehash: 7f641c247cb672934e76fa13bc7b7beb3990dd82
ms.sourcegitcommit: f5a7f0198628f0d152257d90dba6c3a0747a355a
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 12/19/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="8e15d-104">ASP.NET Core MVC 中的模型驗證的簡介</span><span class="sxs-lookup"><span data-stu-id="8e15d-104">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="8e15d-105">由[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8e15d-105">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="8e15d-106">模型驗證的簡介</span><span class="sxs-lookup"><span data-stu-id="8e15d-106">Introduction to model validation</span></span>

<span data-ttu-id="8e15d-107">應用程式將資料儲存在資料庫之前，應用程式必須驗證的資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-107">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="8e15d-108">資料必須進行檢查，確認它適當地格式化類型和大小，並必須符合規則的潛在安全性威脅。</span><span class="sxs-lookup"><span data-stu-id="8e15d-108">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="8e15d-109">驗證是必要的雖然它可以是重複而且單調實作。</span><span class="sxs-lookup"><span data-stu-id="8e15d-109">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="8e15d-110">在 MVC 中，驗證是用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8e15d-110">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="8e15d-111">幸運的是，.NET 具有抽象化驗證屬性的驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-111">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="8e15d-112">這些屬性包含驗證程式碼，藉此減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="8e15d-112">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="8e15d-113">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="8e15d-113">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="8e15d-114">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="8e15d-114">Validation Attributes</span></span>

<span data-ttu-id="8e15d-115">驗證屬性是設定模型驗證，因此它是在概念上類似資料庫資料表中的欄位上的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="8e15d-115">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="8e15d-116">這包括條件約束，例如指派必要的欄位或資料型別。</span><span class="sxs-lookup"><span data-stu-id="8e15d-116">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="8e15d-117">其他的驗證類型包括套用到要強制執行商務規則，例如信用卡、 電話號碼或電子郵件地址資料的模式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-117">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="8e15d-118">驗證屬性，請強制執行更簡單且更輕鬆地使用這些需求。</span><span class="sxs-lookup"><span data-stu-id="8e15d-118">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="8e15d-119">以下是 註解式`Movie`模型儲存電影的電視節目的相關資訊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-119">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="8e15d-120">大部分的屬性所需，且字串的數個屬性具有長度要求。</span><span class="sxs-lookup"><span data-stu-id="8e15d-120">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="8e15d-121">此外，還有數字範圍限制為就地`Price`從 0 至 $999.99，以及自訂驗證屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-121">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[Main](validation/sample/Movie.cs?range=6-29)]

<span data-ttu-id="8e15d-122">只讀取透過模型會顯示此應用程式，以便更容易維護的程式碼的資料相關的規則。</span><span class="sxs-lookup"><span data-stu-id="8e15d-122">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="8e15d-123">以下是幾個常用的內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="8e15d-123">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="8e15d-124">`[CreditCard]`： 會驗證此屬性有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-124">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="8e15d-125">`[Compare]`： 驗證模型比對中的兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-125">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="8e15d-126">`[EmailAddress]`： 會驗證此屬性有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-126">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="8e15d-127">`[Phone]`： 會驗證此屬性有電話格式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-127">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="8e15d-128">`[Range]`： 驗證就是屬性值落在指定範圍內。</span><span class="sxs-lookup"><span data-stu-id="8e15d-128">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="8e15d-129">`[RegularExpression]`： 會驗證指定的規則運算式符合的資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-129">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="8e15d-130">`[Required]`： 使所需的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-130">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="8e15d-131">`[StringLength]`： 驗證字串屬性最多具有指定的長度上限。</span><span class="sxs-lookup"><span data-stu-id="8e15d-131">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="8e15d-132">`[Url]`： 驗證屬性的 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-132">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="8e15d-133">MVC 支援的任何屬性，衍生自`ValidationAttribute`進行驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-133">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="8e15d-134">許多有用的驗證屬性位於[System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)命名空間。</span><span class="sxs-lookup"><span data-stu-id="8e15d-134">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="8e15d-135">可能需要更多的功能提供的內建屬性的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8e15d-135">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="8e15d-136">若是在這些時間點，您可以建立自訂的驗證屬性衍生自`ValidationAttribute`或變更您的模型來實作`IValidatableObject`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-136">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="notes-on-the-use-of-the-required-attribute"></a><span data-ttu-id="8e15d-137">使用必要的屬性資訊</span><span class="sxs-lookup"><span data-stu-id="8e15d-137">Notes on the use of the Required attribute</span></span>

<span data-ttu-id="8e15d-138">不可為 Null 的[實值類型](/dotnet/csharp/language-reference/keywords/value-types) (如 `decimal`、`int`、`float` 和 `DateTime`) 原本就是必要項目，而且不需要 `Required` 屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-138">Non-nullable [value types](/dotnet/csharp/language-reference/keywords/value-types) (such as `decimal`, `int`, `float`, and `DateTime`) are inherently required and don't need the `Required` attribute.</span></span> <span data-ttu-id="8e15d-139">應用程式不會執行伺服器端驗證檢查為非 null 的型別標示為`Required`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-139">The app performs no server-side validation checks for non-nullable types that are marked `Required`.</span></span>

<span data-ttu-id="8e15d-140">MVC 模型繫結，這並不關心驗證和驗證屬性，會拒絕包含遺漏值或空白字元不可為 null 類型的欄位送出表單。</span><span class="sxs-lookup"><span data-stu-id="8e15d-140">MVC model binding, which isn't concerned with validation and validation attributes, rejects a form field submission containing a missing value or whitespace for a non-nullable type.</span></span> <span data-ttu-id="8e15d-141">如果沒有`BindRequired`屬性針對目標屬性，模型繫結會忽略遺漏的資料，不可為 null 的類型，其中的表單欄位不存在的連入的表單資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-141">In the absence of a `BindRequired` attribute on the target property, model binding ignores missing data for non-nullable types, where the form field is absent from the incoming form data.</span></span>

<span data-ttu-id="8e15d-142">[BindRequired 屬性](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute)(另請參閱[自訂屬性的模型繫結行為](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) 來確保完成表單資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-142">The [BindRequired attribute](/aspnet/core/api/microsoft.aspnetcore.mvc.modelbinding.bindrequiredattribute) (also see [Customize model binding behavior with attributes](xref:mvc/models/model-binding#customize-model-binding-behavior-with-attributes)) is useful to ensure form data is complete.</span></span> <span data-ttu-id="8e15d-143">套用至屬性時，模型繫結系統需要該屬性的值。</span><span class="sxs-lookup"><span data-stu-id="8e15d-143">When applied to a property, the model binding system requires a value for that property.</span></span> <span data-ttu-id="8e15d-144">套用至類型時，模型繫結系統就會需要的所有該類型的屬性值。</span><span class="sxs-lookup"><span data-stu-id="8e15d-144">When applied to a type, the model binding system requires values for all of the properties of that type.</span></span>

<span data-ttu-id="8e15d-145">當您使用[Nullable\<T > 類型](/dotnet/csharp/programming-guide/nullable-types/)(例如，`decimal?`或`System.Nullable<decimal>`) 並將它標示`Required`，屬性就像標準的可為 null 型別 （適用於執行伺服器端驗證檢查範例中， `string`)。</span><span class="sxs-lookup"><span data-stu-id="8e15d-145">When you use a [Nullable\<T> type](/dotnet/csharp/programming-guide/nullable-types/) (for example, `decimal?` or `System.Nullable<decimal>`) and mark it `Required`, a server-side validation check is performed as if the property were a standard nullable type (for example, a `string`).</span></span>

<span data-ttu-id="8e15d-146">用戶端驗證需要對應到已標示的模型屬性的表單欄位的值`Required`和未標示為非 null 的型別屬性`Required`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-146">Client-side validation requires a value for a form field that corresponds to a model property that you've marked `Required` and for a non-nullable type property that you haven't marked `Required`.</span></span> <span data-ttu-id="8e15d-147">`Required`可用來控制用戶端驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-147">`Required` can be used to control the client-side validation error message.</span></span>

## <a name="model-state"></a><span data-ttu-id="8e15d-148">模型狀態</span><span class="sxs-lookup"><span data-stu-id="8e15d-148">Model State</span></span>

<span data-ttu-id="8e15d-149">模型狀態表示中提交的 HTML 表單值的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="8e15d-149">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="8e15d-150">MVC 會繼續直到到達驗證欄位的錯誤 (依預設為 200) 的數目上限。</span><span class="sxs-lookup"><span data-stu-id="8e15d-150">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="8e15d-151">您可以藉由將下列程式碼中設定這個數字`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="8e15d-151">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="8e15d-152">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="8e15d-152">Handling Model State Errors</span></span>

<span data-ttu-id="8e15d-153">模型驗證發生在之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。</span><span class="sxs-lookup"><span data-stu-id="8e15d-153">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="8e15d-154">在許多情況下，適當的反應就會傳回錯誤回應，在理想情況下，詳列出模型驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="8e15d-154">In many cases, the appropriate reaction is to return an error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="8e15d-155">某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤，其中案例篩選器可能會實作這類原則的適當位置。</span><span class="sxs-lookup"><span data-stu-id="8e15d-155">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="8e15d-156">您應該測試您的動作具有有效和無效的模型狀態的行為方式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-156">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="8e15d-157">手動驗證</span><span class="sxs-lookup"><span data-stu-id="8e15d-157">Manual validation</span></span>

<span data-ttu-id="8e15d-158">模型繫結和驗證完成後，您可能想要重複部分。</span><span class="sxs-lookup"><span data-stu-id="8e15d-158">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="8e15d-159">例如，使用者可能預期接收整數，欄位中輸入文字或您可能要計算的模型屬性的值。</span><span class="sxs-lookup"><span data-stu-id="8e15d-159">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="8e15d-160">您可能需要手動執行驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-160">You may need to run validation manually.</span></span> <span data-ttu-id="8e15d-161">若要這樣做，請呼叫`TryValidateModel`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e15d-161">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="8e15d-162">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="8e15d-162">Custom validation</span></span>

<span data-ttu-id="8e15d-163">驗證屬性適用於大多數的驗證需求。</span><span class="sxs-lookup"><span data-stu-id="8e15d-163">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="8e15d-164">不過，某些驗證規則僅適用於您的業務。</span><span class="sxs-lookup"><span data-stu-id="8e15d-164">However, some validation rules are specific to your business.</span></span> <span data-ttu-id="8e15d-165">您的規則可能不是一般資料驗證技術，例如確定欄位是必要或符合某個範圍的值。</span><span class="sxs-lookup"><span data-stu-id="8e15d-165">Your rules might not be common data validation techniques such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="8e15d-166">針對這些案例，自訂驗證屬性會是很好的解決方案。</span><span class="sxs-lookup"><span data-stu-id="8e15d-166">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="8e15d-167">在 MVC 中建立您自己的自訂驗證屬性十分簡單。</span><span class="sxs-lookup"><span data-stu-id="8e15d-167">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="8e15d-168">只會繼承`ValidationAttribute`，並覆寫`IsValid`方法。</span><span class="sxs-lookup"><span data-stu-id="8e15d-168">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="8e15d-169">`IsValid`方法接受兩個參數，第一個是名為物件*值*第二個是`ValidationContext`名為物件*validationContext*。</span><span class="sxs-lookup"><span data-stu-id="8e15d-169">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="8e15d-170">*值*正在驗證您的自訂驗證程式的欄位參考的實際值。</span><span class="sxs-lookup"><span data-stu-id="8e15d-170">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="8e15d-171">在下列範例中，商務規則表示使用者可能無法設定內容類型為*傳統*1960年之後發行的電影。</span><span class="sxs-lookup"><span data-stu-id="8e15d-171">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="8e15d-172">`[ClassicMovie]`屬性會先檢查內容類型，如果是典型，然後檢查是否晚於 1960年的發行日期。</span><span class="sxs-lookup"><span data-stu-id="8e15d-172">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="8e15d-173">它會釋放 1960年之後，如果驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="8e15d-173">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="8e15d-174">這個屬性接受整數參數代表年份可讓您驗證資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-174">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="8e15d-175">您可以擷取該屬性的建構函式，在參數的值如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e15d-175">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-29)]

<span data-ttu-id="8e15d-176">`movie`變數代表上方`Movie`物件，其中包含要驗證的送出表單的資料。</span><span class="sxs-lookup"><span data-stu-id="8e15d-176">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="8e15d-177">在此情況下，驗證程式碼會檢查日期和內容類型中的`IsValid`方法`ClassicMovieAttribute`依據規則的類別。</span><span class="sxs-lookup"><span data-stu-id="8e15d-177">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="8e15d-178">成功驗證後`IsValid`傳回`ValidationResult.Success`程式碼，以及當驗證失敗，`ValidationResult`並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-178">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="8e15d-179">當使用者修改`Genre`欄位及提交表單時，`IsValid`方法`ClassicMovieAttribute`將電影是否傳統驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-179">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="8e15d-180">任何內建屬性，例如套用`ClassicMovieAttribute`屬性例如`ReleaseDate`以確保驗證發生時，先前的程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="8e15d-180">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="8e15d-181">因為此範例僅適用於`Movie`型別，較好的選擇是使用`IValidatableObject`下列段落中所示。</span><span class="sxs-lookup"><span data-stu-id="8e15d-181">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="8e15d-182">或者，這個相同的程式碼可以架設在模型中藉由實作`Validate`方法`IValidatableObject`介面。</span><span class="sxs-lookup"><span data-stu-id="8e15d-182">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="8e15d-183">自訂的驗證屬性適用於驗證個別屬性，而實作`IValidatableObject`可以用來實作類別層級驗證，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8e15d-183">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=32-40)]

## <a name="client-side-validation"></a><span data-ttu-id="8e15d-184">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="8e15d-184">Client side validation</span></span>

<span data-ttu-id="8e15d-185">用戶端驗證是絕佳方便使用者。</span><span class="sxs-lookup"><span data-stu-id="8e15d-185">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="8e15d-186">它會將儲存它們要否則花的時間等候往返到伺服器。</span><span class="sxs-lookup"><span data-stu-id="8e15d-186">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="8e15d-187">商務詞彙表示，即使是幾個句號乘以達數百次每天將加入最多會有很多時間、 支出和挫折度。</span><span class="sxs-lookup"><span data-stu-id="8e15d-187">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="8e15d-188">直接和立即驗證可讓使用者更有效率地運作，並產生較佳的品質，輸入和輸出。</span><span class="sxs-lookup"><span data-stu-id="8e15d-188">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="8e15d-189">您必須具有適當的 JavaScript 指令碼參考的檢視以進行用戶端驗證，如下所示的工作。</span><span class="sxs-lookup"><span data-stu-id="8e15d-189">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-cshtml[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-cshtml[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="8e15d-190">[JQuery 不顯眼的驗證](https://github.com/aspnet/jquery-validation-unobtrusive)指令碼是自訂 Microsoft 前端的程式庫來建置熱門[jQuery 驗證](https://jqueryvalidation.org/)外掛程式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-190">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="8e15d-191">沒有 jQuery 不顯眼的驗證，您就必須撰寫程式碼相同的驗證邏輯，在兩個地方： 一次在伺服器端驗證屬性，在模型屬性，然後再次在用戶端指令碼 (jquery 驗證的範例[ `validate()`](https://jqueryvalidation.org/validate/)方法顯示如何複雜這可能會變得)。</span><span class="sxs-lookup"><span data-stu-id="8e15d-191">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server side validation attributes on model properties, and then again in client side scripts (the examples for jQuery Validate's [`validate()`](https://jqueryvalidation.org/validate/) method shows how complex this could become).</span></span> <span data-ttu-id="8e15d-192">相反地，MVC 的[標記協助程式](xref:mvc/views/tag-helpers/intro)和[HTML helper](xref:mvc/views/overview)能夠使用驗證屬性和型別中繼資料，從模型內容來呈現 HTML 5[資料屬性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)中需要驗證表單項目。</span><span class="sxs-lookup"><span data-stu-id="8e15d-192">Instead, MVC's [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) are able to use the validation attributes and type metadata from model properties to render HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation.</span></span> <span data-ttu-id="8e15d-193">MVC 會產生`data-`內建和自訂屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-193">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="8e15d-194">驗證不顯眼的 jQuery 然後剖析 thes`data-`屬性，並將邏輯傳遞給 jQuery 驗證，有效地 「 複製 」 的伺服器端驗證邏輯至用戶端。</span><span class="sxs-lookup"><span data-stu-id="8e15d-194">jQuery Unobtrusive Validation then parses thes `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server side validation logic to the client.</span></span> <span data-ttu-id="8e15d-195">您可以使用如下所示的相關標記協助程式的用戶端上顯示驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="8e15d-195">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-cshtml[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="8e15d-196">上述的標記協助程式會將下列 HTML 轉譯。</span><span class="sxs-lookup"><span data-stu-id="8e15d-196">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="8e15d-197">請注意，`data-`屬性的 html 輸出對應的驗證屬性`ReleaseDate`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-197">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="8e15d-198">`data-val-required`屬性包含使用者並不填入 [發行日期] 欄位時顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-198">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="8e15d-199">jQuery 不顯眼的驗證會將此值傳遞給 jQuery 驗證[ `required()` ](https://jqueryvalidation.org/required-method/)方法，然後顯示該訊息中隨附 **\<s p a n >**項目。</span><span class="sxs-lookup"><span data-stu-id="8e15d-199">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

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

<span data-ttu-id="8e15d-200">用戶端驗證可避免送出之前格式無效。</span><span class="sxs-lookup"><span data-stu-id="8e15d-200">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="8e15d-201">[提交] 按鈕會執行 JavaScript，或者送出表單會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-201">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="8e15d-202">MVC 判斷型別屬性，可能是覆寫使用的.NET 資料類型為基礎的屬性值`[DataType]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-202">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="8e15d-203">基底`[DataType]`屬性不會實際的伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-203">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="8e15d-204">瀏覽器選擇他們自己的錯誤訊息，並顯示這些錯誤，不過他們希望，不過 jQuery 驗證不顯眼封裝可以覆寫的訊息，並與其他以一致的方式顯示。</span><span class="sxs-lookup"><span data-stu-id="8e15d-204">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="8e15d-205">發生這種情況最明顯當使用者套用`[DataType]`子類別，例如`[EmailAddress]`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-205">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="8e15d-206">將驗證加入至動態表單</span><span class="sxs-lookup"><span data-stu-id="8e15d-206">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="8e15d-207">因為驗證不顯眼的 jQuery 驗證邏輯和傳遞參數給 jQuery 驗證第一次載入頁面時，動態產生的表單將不會自動會呈現驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-207">Because jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads, dynamically generated forms will not automatically exhibit validation.</span></span> <span data-ttu-id="8e15d-208">相反地，您必須告訴 jQuery 建立後立即剖析的動態表單不顯眼的驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-208">Instead, you must tell jQuery Unobtrusive Validation to parse the dynamic form immediately after creating it.</span></span> <span data-ttu-id="8e15d-209">例如，下列程式碼顯示您可以設定 AJAX 透過加入表單上的用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-209">For example, the code below shows how you might set up client side validation on a form added via AJAX.</span></span>

```js
$.get({
    url: "https://url/that/returns/a/form",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
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

<span data-ttu-id="8e15d-210">`$.validator.unobtrusive.parse()`方法會接受一個引數的 jQuery 選取器。</span><span class="sxs-lookup"><span data-stu-id="8e15d-210">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="8e15d-211">這個方法會告訴 jQuery 不顯眼的驗證，以剖析`data-`表單內的選取器的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-211">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="8e15d-212">這些屬性的值會接著傳遞至 jQuery 驗證外掛程式，表單表現用所需的用戶端驗證規則。</span><span class="sxs-lookup"><span data-stu-id="8e15d-212">The values of those attributes are then passed to the jQuery Validate plugin so that the form exhibits the desired client side validation rules.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="8e15d-213">將驗證加入至動態控制項</span><span class="sxs-lookup"><span data-stu-id="8e15d-213">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="8e15d-214">您也可以更新表單上的驗證規則，當個別控制項，例如`<input/>`s 和`<select/>`s，動態產生。</span><span class="sxs-lookup"><span data-stu-id="8e15d-214">You can also update the validation rules on a form when individual controls, such as `<input/>`s and `<select/>`s, are dynamically generated.</span></span> <span data-ttu-id="8e15d-215">您無法將傳遞至這些項目的選取器`parse()`方法直接因為周圍的表單已剖析，而且不會更新。</span><span class="sxs-lookup"><span data-stu-id="8e15d-215">You cannot pass selectors for these elements to the `parse()` method directly because the surrounding form has already been parsed and will not update.</span></span>  <span data-ttu-id="8e15d-216">相反地，您先移除現有的驗證資料，然後重新剖析整份表單，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e15d-216">Instead, you first remove the existing validation data, then reparse the entire form, as shown below:</span></span>

```js
$.get({
    url: "https://url/that/returns/a/control",
    dataType: "html",
    error: function(jqXHR, textStatus, errorThrown) {
        alert(textStatus + ": Could not add form. " + errorThrown);
    },
    success: function(newInputHTML) {
        var form = document.getElementById("my-form");
        form.insertAdjacentHTML("beforeend", newInputHTML);
        form.removeData("validator")    // Added by the raw jQuery Validate
            .removeData("unobtrusiveValidation");   // Added by jQuery Unobtrusive Validation
        $.validator.unobtrusive.parse(form);
    }
})
```

## <a name="iclientmodelvalidator"></a><span data-ttu-id="8e15d-217">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="8e15d-217">IClientModelValidator</span></span>

<span data-ttu-id="8e15d-218">您可能會對您的自訂屬性，建立用戶端端邏輯和[不顯眼的驗證](http://jqueryvalidation.org/documentation/)它為您自動一部分驗證用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="8e15d-218">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="8e15d-219">第一個步驟是控制哪些資料屬性加入實作`IClientModelValidator`介面如下所示：</span><span class="sxs-lookup"><span data-stu-id="8e15d-219">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="8e15d-220">實作這個介面的屬性可以將 HTML 屬性，加入產生的欄位。</span><span class="sxs-lookup"><span data-stu-id="8e15d-220">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="8e15d-221">檢查輸出`ReleaseDate`項目會顯示除了現在沒有外，類似於上述範例中，HTML`data-val-classicmovie`中所定義的屬性`AddValidation`方法`IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-221">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="8e15d-222">不顯眼的驗證會使用中的資料`data-`顯示錯誤訊息的屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-222">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="8e15d-223">不過，jQuery 不了解規則或訊息，直到您將它們加入 jQuery 的`validator`物件。</span><span class="sxs-lookup"><span data-stu-id="8e15d-223">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="8e15d-224">這顯示在下列範例中，加入名為方法`classicmovie`包含自訂用戶端驗證程式碼，加入 jQuery`validator`物件。</span><span class="sxs-lookup"><span data-stu-id="8e15d-224">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="8e15d-225">現在 jQuery 有執行自訂的 JavaScript 驗證，以及該驗證程式碼會傳回 false 時所顯示的錯誤訊息的資訊。</span><span class="sxs-lookup"><span data-stu-id="8e15d-225">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="8e15d-226">遠端驗證</span><span class="sxs-lookup"><span data-stu-id="8e15d-226">Remote validation</span></span>

<span data-ttu-id="8e15d-227">遠端驗證是很棒的功能，您需要驗證用戶端對伺服器上的資料上的資料時使用。</span><span class="sxs-lookup"><span data-stu-id="8e15d-227">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="8e15d-228">例如，可能需要您的應用程式，請確認是否有電子郵件或使用者名稱已在使用，而且它必須查詢大量的資料，若要這樣做。</span><span class="sxs-lookup"><span data-stu-id="8e15d-228">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="8e15d-229">下載大型的其中一個驗證的資料集或幾個欄位會耗用太多資源。</span><span class="sxs-lookup"><span data-stu-id="8e15d-229">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="8e15d-230">它可能也會公開機密資訊。</span><span class="sxs-lookup"><span data-stu-id="8e15d-230">It may also expose sensitive information.</span></span> <span data-ttu-id="8e15d-231">另一個方法是反覆存取要求驗證欄位。</span><span class="sxs-lookup"><span data-stu-id="8e15d-231">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="8e15d-232">您可以在兩個步驟的程序中實作遠端驗證。</span><span class="sxs-lookup"><span data-stu-id="8e15d-232">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="8e15d-233">首先，您必須加上註解您的模型與`[Remote]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-233">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="8e15d-234">`[Remote]`屬性接受多個多載可用來導向到適當的程式碼以呼叫的用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="8e15d-234">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="8e15d-235">下列範例會指向`VerifyEmail`動作方法的`Users`控制站。</span><span class="sxs-lookup"><span data-stu-id="8e15d-235">The example below points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=7-8)]

<span data-ttu-id="8e15d-236">第二個步驟將驗證程式碼中對應的動作方法中所定義`[Remote]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8e15d-236">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="8e15d-237">根據 jQuery 驗證[ `remote()` ](https://jqueryvalidation.org/remote-method/)方法的文件：</span><span class="sxs-lookup"><span data-stu-id="8e15d-237">According to the jQuery Validate [`remote()`](https://jqueryvalidation.org/remote-method/) method documentation:</span></span>

> <span data-ttu-id="8e15d-238">Serverside 回應必須是必須的 JSON 字串`"true"`針對有效的項目，而且可以是`"false"`， `undefined`，或`null`針對無效的項目，使用預設的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-238">The serverside response must be a JSON string that must be `"true"` for valid elements, and can be `"false"`, `undefined`, or `null` for invalid elements, using the default error message.</span></span> <span data-ttu-id="8e15d-239">如果 serverside 回應會是一個字串，例如。</span><span class="sxs-lookup"><span data-stu-id="8e15d-239">If the serverside response is a string, eg.</span></span> <span data-ttu-id="8e15d-240">`"That name is already taken, try peter123 instead"`這個字串將顯示為以取代預設的自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-240">`"That name is already taken, try peter123 instead"`, this string will be displayed as a custom error message in place of the default.</span></span>

<span data-ttu-id="8e15d-241">定義`VerifyEmail()`方法會依循這些規則，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8e15d-241">The definition of the `VerifyEmail()` method follows these rules, as shown below.</span></span> <span data-ttu-id="8e15d-242">它會傳回驗證錯誤訊息如果採取電子郵件，或`true`如果的電子郵件是免費的且會包裝在結果`JsonResult`物件。</span><span class="sxs-lookup"><span data-stu-id="8e15d-242">It returns a validation error message if the email is taken, or `true` if the email is free, and wraps the result in a `JsonResult` object.</span></span> <span data-ttu-id="8e15d-243">然後用戶端就可以使用傳回的值，以繼續，或顯示錯誤，如有需要。</span><span class="sxs-lookup"><span data-stu-id="8e15d-243">The client side can then use the returned value to proceed or display the error if needed.</span></span>

[!code-csharp[Main](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="8e15d-244">現在當使用者輸入電子郵件時，在檢視中的 JavaScript 可讓以查看該電子郵件已取用，如果是的話，會顯示錯誤訊息的遠端呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e15d-244">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken and, if so, displays the error message.</span></span> <span data-ttu-id="8e15d-245">否則，使用者可以像往常一樣送出表單。</span><span class="sxs-lookup"><span data-stu-id="8e15d-245">Otherwise, the user can submit the form as usual.</span></span>

<span data-ttu-id="8e15d-246">`AdditionalFields`屬性`[Remote]`屬性是適合用來驗證針對伺服器上的資料欄位的組合。</span><span class="sxs-lookup"><span data-stu-id="8e15d-246">The `AdditionalFields` property of the `[Remote]` attribute is useful for validating combinations of fields against data on the server.</span></span>  <span data-ttu-id="8e15d-247">例如，如果`User`上述的模型有兩個額外的屬性稱為`FirstName`和`LastName`，您可能想要確認沒有任何現有的使用者已經有該名稱的組。</span><span class="sxs-lookup"><span data-stu-id="8e15d-247">For example, if the `User` model from above had two additional properties called `FirstName` and `LastName`, you might want to verify that no existing users already have that pair of names.</span></span>  <span data-ttu-id="8e15d-248">下列程式碼所示，您會定義新屬性：</span><span class="sxs-lookup"><span data-stu-id="8e15d-248">You define the new properties as shown in the following code:</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=10-13)]

<span data-ttu-id="8e15d-249">`AdditionalFields`可能已明確設定為字串`"FirstName"`和`"LastName"`，但使用[ `nameof` ](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof)運算子，這樣可簡化稍後重構。</span><span class="sxs-lookup"><span data-stu-id="8e15d-249">`AdditionalFields` could have been set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/nameof) operator like this simplifies later refactoring.</span></span>  <span data-ttu-id="8e15d-250">要執行驗證的動作方法必須接受兩個引數，其中一個值的`FirstName`和值的其中一個`LastName`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-250">The action method to perform the validation must then accept two arguments, one for the value of `FirstName` and one for the value of `LastName`.</span></span>


[!code-csharp[Main](validation/sample/UsersController.cs?range=30-39)]

<span data-ttu-id="8e15d-251">現在當使用者輸入第一個和最後一個名稱時，JavaScript:</span><span class="sxs-lookup"><span data-stu-id="8e15d-251">Now when users enter a first and last name, JavaScript:</span></span>

* <span data-ttu-id="8e15d-252">會查看如果已取用的名稱配對的遠端呼叫。</span><span class="sxs-lookup"><span data-stu-id="8e15d-252">Makes a remote call to see if that pair of names has been taken.</span></span>
* <span data-ttu-id="8e15d-253">如果已取用配對，會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8e15d-253">If the pair has been taken, an error message is displayed.</span></span> 
* <span data-ttu-id="8e15d-254">如果不採用，使用者可以送出表單。</span><span class="sxs-lookup"><span data-stu-id="8e15d-254">If not taken, the user can submit the form.</span></span>

<span data-ttu-id="8e15d-255">如果您要驗證兩個或多個額外的欄位，與`[Remote]`屬性，您提供它們做為逗號分隔的清單。</span><span class="sxs-lookup"><span data-stu-id="8e15d-255">If you need to validate two or more additional fields with the `[Remote]` attribute, you provide them as a comma-delimited list.</span></span>  <span data-ttu-id="8e15d-256">例如，若要新增`MiddleName`屬性加入模型中，設定`[Remote]`屬性，如下列程式碼所示：</span><span class="sxs-lookup"><span data-stu-id="8e15d-256">For example, to add a  `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following code:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="8e15d-257">`AdditionalFields`像所有的屬性引數必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="8e15d-257">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span>  <span data-ttu-id="8e15d-258">因此，您必須使用[以內插值取代字串](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫[ `string.Join()` ](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx)初始化`AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="8e15d-258">Therefore, you must not use an [interpolated string](https://docs.microsoft.com/en-us/dotnet/csharp/language-reference/keywords/interpolated-strings) or call [`string.Join()`](https://msdn.microsoft.com/en-us/library/system.string.join(v=vs.110).aspx) to initialize `AdditionalFields`.</span></span> <span data-ttu-id="8e15d-259">每個其他欄位加入至`[Remote]`屬性，您必須將另一個引數加入至對應的控制器動作方法。</span><span class="sxs-lookup"><span data-stu-id="8e15d-259">For every additional field that you add to the `[Remote]` attribute, you must add another argument to the corresponding controller action method.</span></span>
