---
title: "ASP.NET Core MVC 中的模型驗證"
author: rick-anderson
description: "導入了 ASP.NET Core MVC 中的模型驗證。"
keywords: "ASP.NET Core，MVC、 驗證"
ms.author: riande
manager: wpickett
ms.date: 10/14/2016
ms.topic: article
ms.assetid: 3a8676dd-7ed8-4a05-bca2-44e288ab99ee
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/models/validation
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 0874d3b677cee2859da9eb85b0573811abbed12a
ms.sourcegitcommit: 78d28178345a0eea91556e4cd1adad98b1446db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/22/2017
---
# <a name="introduction-to-model-validation-in-aspnet-core-mvc"></a><span data-ttu-id="8c408-104">ASP.NET Core MVC 中的模型驗證的簡介</span><span class="sxs-lookup"><span data-stu-id="8c408-104">Introduction to model validation in ASP.NET Core MVC</span></span>

<span data-ttu-id="8c408-105">由[Rachel Appel](https://github.com/rachelappel)</span><span class="sxs-lookup"><span data-stu-id="8c408-105">By [Rachel Appel](https://github.com/rachelappel)</span></span>

## <a name="introduction-to-model-validation"></a><span data-ttu-id="8c408-106">模型驗證的簡介</span><span class="sxs-lookup"><span data-stu-id="8c408-106">Introduction to model validation</span></span>

<span data-ttu-id="8c408-107">應用程式將資料儲存在資料庫之前，應用程式必須驗證的資料。</span><span class="sxs-lookup"><span data-stu-id="8c408-107">Before an app stores data in a database, the app must validate the data.</span></span> <span data-ttu-id="8c408-108">資料必須進行檢查，確認它適當地格式化類型和大小，並必須符合規則的潛在安全性威脅。</span><span class="sxs-lookup"><span data-stu-id="8c408-108">Data must be checked for potential security threats, verified that it is appropriately formatted by type and size, and it must conform to your rules.</span></span> <span data-ttu-id="8c408-109">驗證是必要的雖然它可以是重複而且單調實作。</span><span class="sxs-lookup"><span data-stu-id="8c408-109">Validation is necessary although it can be redundant and tedious to implement.</span></span> <span data-ttu-id="8c408-110">在 MVC 中，驗證是用戶端和伺服器上。</span><span class="sxs-lookup"><span data-stu-id="8c408-110">In MVC, validation happens on both the client and server.</span></span>

<span data-ttu-id="8c408-111">幸運的是，.NET 具有抽象化驗證屬性的驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-111">Fortunately, .NET has abstracted validation into validation attributes.</span></span> <span data-ttu-id="8c408-112">這些屬性包含驗證程式碼，藉此減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="8c408-112">These attributes contain validation code, thereby reducing the amount of code you must write.</span></span>

<span data-ttu-id="8c408-113">[檢視或從 GitHub 下載範例](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample)。</span><span class="sxs-lookup"><span data-stu-id="8c408-113">[View or download sample from GitHub](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/models/validation/sample).</span></span>

## <a name="validation-attributes"></a><span data-ttu-id="8c408-114">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="8c408-114">Validation Attributes</span></span>

<span data-ttu-id="8c408-115">驗證屬性是設定模型驗證，因此它是在概念上類似資料庫資料表中的欄位上的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="8c408-115">Validation attributes are a way to configure model validation so it's similar conceptually to validation on fields in database tables.</span></span> <span data-ttu-id="8c408-116">這包括條件約束，例如指派必要的欄位或資料型別。</span><span class="sxs-lookup"><span data-stu-id="8c408-116">This includes constraints such as assigning data types or required fields.</span></span> <span data-ttu-id="8c408-117">其他的驗證類型包括套用到要強制執行商務規則，例如信用卡、 電話號碼或電子郵件地址資料的模式。</span><span class="sxs-lookup"><span data-stu-id="8c408-117">Other types of validation include applying patterns to data to enforce business rules, such as a credit card, phone number, or email address.</span></span> <span data-ttu-id="8c408-118">驗證屬性，請強制執行更簡單且更輕鬆地使用這些需求。</span><span class="sxs-lookup"><span data-stu-id="8c408-118">Validation attributes make enforcing these requirements much simpler and easier to use.</span></span>

<span data-ttu-id="8c408-119">以下是 註解式`Movie`模型儲存電影的電視節目的相關資訊的應用程式。</span><span class="sxs-lookup"><span data-stu-id="8c408-119">Below is an annotated `Movie` model from an app that stores information about movies and TV shows.</span></span> <span data-ttu-id="8c408-120">大部分的屬性所需，且字串的數個屬性具有長度要求。</span><span class="sxs-lookup"><span data-stu-id="8c408-120">Most of the properties are required and several string properties have length requirements.</span></span> <span data-ttu-id="8c408-121">此外，還有數字範圍限制為就地`Price`從 0 至 $999.99，以及自訂驗證屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-121">Additionally, there is a numeric range restriction in place for the `Price` property from 0 to $999.99, along with a custom validation attribute.</span></span>

[!code-csharp[Main](validation/sample/Movie.cs?range=6-31)]

<span data-ttu-id="8c408-122">只讀取透過模型會顯示此應用程式，以便更容易維護的程式碼的資料相關的規則。</span><span class="sxs-lookup"><span data-stu-id="8c408-122">Simply reading through the model reveals the rules about data for this app, making it easier to maintain the code.</span></span> <span data-ttu-id="8c408-123">以下是幾個常用的內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="8c408-123">Below are several popular built-in validation attributes:</span></span>

* <span data-ttu-id="8c408-124">`[CreditCard]`： 會驗證此屬性有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="8c408-124">`[CreditCard]`: Validates the property has a credit card format.</span></span>

* <span data-ttu-id="8c408-125">`[Compare]`： 驗證模型比對中的兩個屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-125">`[Compare]`: Validates two properties in a model match.</span></span>

* <span data-ttu-id="8c408-126">`[EmailAddress]`： 會驗證此屬性有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="8c408-126">`[EmailAddress]`: Validates the property has an email format.</span></span>

* <span data-ttu-id="8c408-127">`[Phone]`： 會驗證此屬性有電話格式。</span><span class="sxs-lookup"><span data-stu-id="8c408-127">`[Phone]`: Validates the property has a telephone format.</span></span>

* <span data-ttu-id="8c408-128">`[Range]`： 驗證就是屬性值落在指定範圍內。</span><span class="sxs-lookup"><span data-stu-id="8c408-128">`[Range]`: Validates the property value falls within the given range.</span></span>

* <span data-ttu-id="8c408-129">`[RegularExpression]`： 會驗證指定的規則運算式符合的資料。</span><span class="sxs-lookup"><span data-stu-id="8c408-129">`[RegularExpression]`: Validates that the data matches the specified regular expression.</span></span>

* <span data-ttu-id="8c408-130">`[Required]`： 使所需的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-130">`[Required]`: Makes a property required.</span></span>

* <span data-ttu-id="8c408-131">`[StringLength]`： 驗證字串屬性最多具有指定的長度上限。</span><span class="sxs-lookup"><span data-stu-id="8c408-131">`[StringLength]`: Validates that a string property has at most the given maximum length.</span></span>

* <span data-ttu-id="8c408-132">`[Url]`： 驗證屬性的 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="8c408-132">`[Url]`: Validates the property has a URL format.</span></span>

<span data-ttu-id="8c408-133">MVC 支援的任何屬性，衍生自`ValidationAttribute`進行驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-133">MVC supports any attribute that derives from `ValidationAttribute` for validation purposes.</span></span> <span data-ttu-id="8c408-134">許多有用的驗證屬性位於[System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations)命名空間。</span><span class="sxs-lookup"><span data-stu-id="8c408-134">Many useful validation attributes can be found in the [System.ComponentModel.DataAnnotations](https://docs.microsoft.com/dotnet/api/system.componentmodel.dataannotations) namespace.</span></span>

<span data-ttu-id="8c408-135">可能需要更多的功能提供的內建屬性的執行個體。</span><span class="sxs-lookup"><span data-stu-id="8c408-135">There may be instances where you need more features than built-in attributes provide.</span></span> <span data-ttu-id="8c408-136">若是在這些時間點，您可以建立自訂的驗證屬性衍生自`ValidationAttribute`或變更您的模型來實作`IValidatableObject`。</span><span class="sxs-lookup"><span data-stu-id="8c408-136">For those times, you can create custom validation attributes by deriving from `ValidationAttribute` or changing your model to implement `IValidatableObject`.</span></span>

## <a name="model-state"></a><span data-ttu-id="8c408-137">模型狀態</span><span class="sxs-lookup"><span data-stu-id="8c408-137">Model State</span></span>

<span data-ttu-id="8c408-138">模型狀態表示中提交的 HTML 表單值的驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="8c408-138">Model state represents validation errors in submitted HTML form values.</span></span>

<span data-ttu-id="8c408-139">MVC 會繼續直到到達驗證欄位的錯誤 (依預設為 200) 的數目上限。</span><span class="sxs-lookup"><span data-stu-id="8c408-139">MVC will continue validating fields until reaches the maximum number of errors (200 by default).</span></span> <span data-ttu-id="8c408-140">您可以藉由將下列程式碼中設定這個數字`ConfigureServices`方法中的*Startup.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="8c408-140">You can configure this number by inserting the following code into the `ConfigureServices` method in the *Startup.cs* file:</span></span>

[!code-csharp[Main](validation/sample/Startup.cs?range=27)]

## <a name="handling-model-state-errors"></a><span data-ttu-id="8c408-141">處理模型狀態錯誤</span><span class="sxs-lookup"><span data-stu-id="8c408-141">Handling Model State Errors</span></span>

<span data-ttu-id="8c408-142">模型驗證發生在之前所叫用每個控制器動作和動作方法必須負責檢查`ModelState.IsValid`和作出適當回應。</span><span class="sxs-lookup"><span data-stu-id="8c408-142">Model validation occurs prior to each controller action being invoked, and it is the action method’s responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="8c408-143">在許多情況下，適當的反應就會傳回某種類型的錯誤回應，在理想情況下，詳列出模型驗證失敗的原因。</span><span class="sxs-lookup"><span data-stu-id="8c408-143">In many cases, the appropriate reaction is to return some kind of error response, ideally detailing the reason why model validation failed.</span></span>

<span data-ttu-id="8c408-144">某些應用程式會選擇遵循標準慣例來處理模型驗證錯誤，其中案例篩選器可能會實作這類原則的適當位置。</span><span class="sxs-lookup"><span data-stu-id="8c408-144">Some apps will choose to follow a standard convention for dealing with model validation errors, in which case a filter may be an appropriate place to implement such a policy.</span></span> <span data-ttu-id="8c408-145">您應該測試您的動作具有有效和無效的模型狀態的行為方式。</span><span class="sxs-lookup"><span data-stu-id="8c408-145">You should test how your actions behave with valid and invalid model states.</span></span>

## <a name="manual-validation"></a><span data-ttu-id="8c408-146">手動驗證</span><span class="sxs-lookup"><span data-stu-id="8c408-146">Manual validation</span></span>

<span data-ttu-id="8c408-147">模型繫結和驗證完成後，您可能想要重複部分。</span><span class="sxs-lookup"><span data-stu-id="8c408-147">After model binding and validation are complete, you may want to repeat parts of it.</span></span> <span data-ttu-id="8c408-148">例如，使用者可能預期接收整數，欄位中輸入文字或您可能要計算的模型屬性的值。</span><span class="sxs-lookup"><span data-stu-id="8c408-148">For example, a user may have entered text in a field expecting an integer, or you may need to compute a value for a model's property.</span></span>

<span data-ttu-id="8c408-149">您可能需要手動執行驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-149">You may need to run validation manually.</span></span> <span data-ttu-id="8c408-150">若要這樣做，請呼叫`TryValidateModel`方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="8c408-150">To do so, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[Main](validation/sample/MoviesController.cs?range=52)]

## <a name="custom-validation"></a><span data-ttu-id="8c408-151">自訂驗證</span><span class="sxs-lookup"><span data-stu-id="8c408-151">Custom validation</span></span>

<span data-ttu-id="8c408-152">驗證屬性適用於大多數的驗證需求。</span><span class="sxs-lookup"><span data-stu-id="8c408-152">Validation attributes work for most validation needs.</span></span> <span data-ttu-id="8c408-153">不過，一些驗證規則專屬於您企業中，因為它們並不是只需要確認欄位的一般資料驗證，或符合某個範圍的值。</span><span class="sxs-lookup"><span data-stu-id="8c408-153">However, some validation rules are specific to your business, as they're not just generic data validation such as ensuring a field is required or that it conforms to a range of values.</span></span> <span data-ttu-id="8c408-154">針對這些案例，自訂驗證屬性會是很好的解決方案。</span><span class="sxs-lookup"><span data-stu-id="8c408-154">For these scenarios, custom validation attributes are a great solution.</span></span> <span data-ttu-id="8c408-155">在 MVC 中建立您自己的自訂驗證屬性十分簡單。</span><span class="sxs-lookup"><span data-stu-id="8c408-155">Creating your own custom validation attributes in MVC is easy.</span></span> <span data-ttu-id="8c408-156">只會繼承`ValidationAttribute`，並覆寫`IsValid`方法。</span><span class="sxs-lookup"><span data-stu-id="8c408-156">Just inherit from the `ValidationAttribute`, and override the `IsValid` method.</span></span> <span data-ttu-id="8c408-157">`IsValid`方法接受兩個參數，第一個是名為物件*值*第二個是`ValidationContext`名為物件*validationContext*。</span><span class="sxs-lookup"><span data-stu-id="8c408-157">The `IsValid` method accepts two parameters, the first is an object named *value* and the second is a `ValidationContext` object named *validationContext*.</span></span> <span data-ttu-id="8c408-158">*值*正在驗證您的自訂驗證程式的欄位參考的實際值。</span><span class="sxs-lookup"><span data-stu-id="8c408-158">*Value* refers to the actual value from the field that your custom validator is validating.</span></span>

<span data-ttu-id="8c408-159">在下列範例中，商務規則表示使用者可能無法設定內容類型為*傳統*1960年之後發行的電影。</span><span class="sxs-lookup"><span data-stu-id="8c408-159">In the following sample, a business rule states that users may not set the genre to *Classic* for a movie released after 1960.</span></span> <span data-ttu-id="8c408-160">`[ClassicMovie]`屬性會先檢查內容類型，如果是典型，然後檢查是否晚於 1960年的發行日期。</span><span class="sxs-lookup"><span data-stu-id="8c408-160">The `[ClassicMovie]` attribute checks the genre first, and if it is a classic, then it checks the release date to see that it is later than 1960.</span></span> <span data-ttu-id="8c408-161">它會釋放 1960年之後，如果驗證失敗。</span><span class="sxs-lookup"><span data-stu-id="8c408-161">If it is released after 1960, validation fails.</span></span> <span data-ttu-id="8c408-162">這個屬性接受整數參數代表年份可讓您驗證資料。</span><span class="sxs-lookup"><span data-stu-id="8c408-162">The attribute accepts an integer parameter representing the year that you can use to validate data.</span></span> <span data-ttu-id="8c408-163">您可以擷取該屬性的建構函式，在參數的值如下所示：</span><span class="sxs-lookup"><span data-stu-id="8c408-163">You can capture the value of the parameter in the attribute's constructor, as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=9-28)]

<span data-ttu-id="8c408-164">`movie`變數代表上方`Movie`物件，其中包含要驗證的送出表單的資料。</span><span class="sxs-lookup"><span data-stu-id="8c408-164">The `movie` variable above represents a `Movie` object that contains the data from the form submission to validate.</span></span> <span data-ttu-id="8c408-165">在此情況下，驗證程式碼會檢查日期和內容類型中的`IsValid`方法`ClassicMovieAttribute`依據規則的類別。</span><span class="sxs-lookup"><span data-stu-id="8c408-165">In this case, the validation code checks the date and genre in the `IsValid` method of the `ClassicMovieAttribute` class as per the rules.</span></span> <span data-ttu-id="8c408-166">成功驗證後`IsValid`傳回`ValidationResult.Success`程式碼，以及當驗證失敗，`ValidationResult`並出現錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8c408-166">Upon successful validation `IsValid` returns a `ValidationResult.Success` code, and when validation fails, a `ValidationResult` with an error message.</span></span> <span data-ttu-id="8c408-167">當使用者修改`Genre`欄位及提交表單時，`IsValid`方法`ClassicMovieAttribute`將電影是否傳統驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-167">When a user modifies the `Genre` field and submits the form, the `IsValid` method of the `ClassicMovieAttribute` will verify whether the movie is a classic.</span></span> <span data-ttu-id="8c408-168">任何內建屬性，例如套用`ClassicMovieAttribute`屬性例如`ReleaseDate`以確保驗證發生時，先前的程式碼範例所示。</span><span class="sxs-lookup"><span data-stu-id="8c408-168">Like any built-in attribute, apply the `ClassicMovieAttribute` to a property such as `ReleaseDate` to ensure validation happens, as shown in the previous code sample.</span></span> <span data-ttu-id="8c408-169">因為此範例僅適用於`Movie`型別，較好的選擇是使用`IValidatableObject`下列段落中所示。</span><span class="sxs-lookup"><span data-stu-id="8c408-169">Since the example works only with `Movie` types, a better option is to use `IValidatableObject` as shown in the following paragraph.</span></span>

<span data-ttu-id="8c408-170">或者，這個相同的程式碼可以架設在模型中藉由實作`Validate`方法`IValidatableObject`介面。</span><span class="sxs-lookup"><span data-stu-id="8c408-170">Alternatively, this same code could be placed in the model by implementing the `Validate` method on the `IValidatableObject` interface.</span></span> <span data-ttu-id="8c408-171">自訂的驗證屬性適用於驗證個別屬性，而實作`IValidatableObject`可以用來實作類別層級驗證，如下所示。</span><span class="sxs-lookup"><span data-stu-id="8c408-171">While custom validation attributes work well for validating individual properties, implementing `IValidatableObject` can be used to implement class-level validation as seen here.</span></span>

[!code-csharp[Main](validation/sample/MovieIValidatable.cs?range=33-41)]

## <a name="client-side-validation"></a><span data-ttu-id="8c408-172">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="8c408-172">Client side validation</span></span>

<span data-ttu-id="8c408-173">用戶端驗證是絕佳方便使用者。</span><span class="sxs-lookup"><span data-stu-id="8c408-173">Client side validation is a great convenience for users.</span></span> <span data-ttu-id="8c408-174">它會將儲存它們要否則花的時間等候往返到伺服器。</span><span class="sxs-lookup"><span data-stu-id="8c408-174">It saves time they would otherwise spend waiting for a round trip to the server.</span></span> <span data-ttu-id="8c408-175">商務詞彙表示，即使是幾個句號乘以達數百次每天將加入最多會有很多時間、 支出和挫折度。</span><span class="sxs-lookup"><span data-stu-id="8c408-175">In business terms, even a few fractions of seconds multiplied hundreds of times each day adds up to be a lot of time, expense, and frustration.</span></span> <span data-ttu-id="8c408-176">直接和立即驗證可讓使用者更有效率地運作，並產生較佳的品質，輸入和輸出。</span><span class="sxs-lookup"><span data-stu-id="8c408-176">Straightforward and immediate validation enables users to work more efficiently and produce better quality input and output.</span></span>

<span data-ttu-id="8c408-177">您必須具有適當的 JavaScript 指令碼參考的檢視以進行用戶端驗證，如下所示的工作。</span><span class="sxs-lookup"><span data-stu-id="8c408-177">You must have a view with the proper JavaScript script references in place for client side validation to work as you see here.</span></span>

[!code-html[Main](validation/sample/Views/Shared/_Layout.cshtml?range=37)]

[!code-html[Main](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml)]

<span data-ttu-id="8c408-178">要驗證資料並顯示任何錯誤訊息，使用 JavaScript 則 MVC 會使用驗證屬性，除了從 模型屬性的型別中繼資料。</span><span class="sxs-lookup"><span data-stu-id="8c408-178">MVC uses validation attributes in addition to type metadata from model properties to validate data and display any error messages using JavaScript.</span></span> <span data-ttu-id="8c408-179">當您使用 MVC 來呈現模型使用從表單元素[標記協助程式](xref:mvc/views/tag-helpers/intro)或[HTML helper](xref:mvc/views/overview)將 HTML 5[資料屬性](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes)需要驗證，做為表單項目中如下所示。</span><span class="sxs-lookup"><span data-stu-id="8c408-179">When you use MVC to render form elements from a model using [Tag Helpers](xref:mvc/views/tag-helpers/intro) or [HTML helpers](xref:mvc/views/overview) it will add HTML 5 [data- attributes](http://w3c.github.io/html/dom.html#embedding-custom-non-visible-data-with-the-data-attributes) in the form elements that need validation, as shown below.</span></span> <span data-ttu-id="8c408-180">MVC 會產生`data-`內建和自訂屬性的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-180">MVC generates the `data-` attributes for both built-in and custom attributes.</span></span> <span data-ttu-id="8c408-181">您可以使用如下所示的相關標記協助程式的用戶端上顯示驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="8c408-181">You can display validation errors on the client using the relevant tag helpers as shown here:</span></span>

[!code-html[Main](validation/sample/Views/Movies/Create.cshtml?highlight=4,5&range=19-25)]

<span data-ttu-id="8c408-182">上述的標記協助程式會將下列 HTML 轉譯。</span><span class="sxs-lookup"><span data-stu-id="8c408-182">The tag helpers above render the HTML below.</span></span> <span data-ttu-id="8c408-183">請注意，`data-`屬性的 html 輸出對應的驗證屬性`ReleaseDate`屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-183">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="8c408-184">`data-val-required`屬性包含錯誤訊息顯示如果使用者不會填入 [發行日期] 欄位中，而且該訊息會顯示隨附`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="8c408-184">The `data-val-required` attribute below contains an error message to display if the user doesn't fill in the release date field, and that message displays in the accompanying `<span>` element.</span></span>

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

<span data-ttu-id="8c408-185">用戶端驗證可避免送出之前格式無效。</span><span class="sxs-lookup"><span data-stu-id="8c408-185">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="8c408-186">[提交] 按鈕會執行 JavaScript，或者送出表單會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8c408-186">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="8c408-187">MVC 判斷型別屬性，可能是覆寫使用的.NET 資料類型為基礎的屬性值`[DataType]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-187">MVC determines type attribute values based on the .NET data type of a property, possibly overridden using `[DataType]` attributes.</span></span> <span data-ttu-id="8c408-188">基底`[DataType]`屬性不會實際的伺服器端驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-188">The base `[DataType]` attribute does no real server-side validation.</span></span> <span data-ttu-id="8c408-189">瀏覽器選擇他們自己的錯誤訊息，並顯示這些錯誤，不過他們希望，不過 jQuery 驗證不顯眼封裝可以覆寫的訊息，並與其他以一致的方式顯示。</span><span class="sxs-lookup"><span data-stu-id="8c408-189">Browsers choose their own error messages and display those errors however they wish, however the jQuery Validation Unobtrusive package can override the messages and display them consistently with others.</span></span> <span data-ttu-id="8c408-190">發生這種情況最明顯當使用者套用`[DataType]`子類別，例如`[EmailAddress]`。</span><span class="sxs-lookup"><span data-stu-id="8c408-190">This happens most obviously when users apply `[DataType]` subclasses such as `[EmailAddress]`.</span></span>

## <a name="iclientmodelvalidator"></a><span data-ttu-id="8c408-191">IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="8c408-191">IClientModelValidator</span></span>

<span data-ttu-id="8c408-192">您可能會對您的自訂屬性，建立用戶端端邏輯和[不顯眼的驗證](http://jqueryvalidation.org/documentation/)它為您自動一部分驗證用戶端上執行。</span><span class="sxs-lookup"><span data-stu-id="8c408-192">You may create client side logic for your custom attribute, and [unobtrusive validation](http://jqueryvalidation.org/documentation/) will execute it on the client for you automatically as part of validation.</span></span> <span data-ttu-id="8c408-193">第一個步驟是控制哪些資料屬性加入實作`IClientModelValidator`介面如下所示：</span><span class="sxs-lookup"><span data-stu-id="8c408-193">The first step is to control what data- attributes are added by implementing the `IClientModelValidator` interface as shown here:</span></span>

[!code-csharp[Main](validation/sample/ClassicMovieAttribute.cs?range=30-42)]

<span data-ttu-id="8c408-194">實作這個介面的屬性可以將 HTML 屬性，加入產生的欄位。</span><span class="sxs-lookup"><span data-stu-id="8c408-194">Attributes that implement this interface can add HTML attributes to generated fields.</span></span> <span data-ttu-id="8c408-195">檢查輸出`ReleaseDate`項目會顯示除了現在沒有外，類似於上述範例中，HTML`data-val-classicmovie`中所定義的屬性`AddValidation`方法`IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="8c408-195">Examining the output for the `ReleaseDate` element reveals HTML that is similar to the previous example, except now there is a `data-val-classicmovie` attribute that was defined in the `AddValidation` method of `IClientModelValidator`.</span></span>

```html
<input class="form-control" type="datetime"
data-val="true"
data-val-classicmovie="Classic movies must have a release year earlier than 1960."
data-val-classicmovie-year="1960"
data-val-required="The ReleaseDate field is required."
id="ReleaseDate" name="ReleaseDate" value="" />
```

<span data-ttu-id="8c408-196">不顯眼的驗證會使用中的資料`data-`顯示錯誤訊息的屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-196">Unobtrusive validation uses the data in the `data-` attributes to display error messages.</span></span> <span data-ttu-id="8c408-197">不過，jQuery 不了解規則或訊息，直到您將它們加入 jQuery 的`validator`物件。</span><span class="sxs-lookup"><span data-stu-id="8c408-197">However, jQuery doesn't know about rules or messages until you add them to jQuery's `validator` object.</span></span> <span data-ttu-id="8c408-198">這顯示在下列範例中，加入名為方法`classicmovie`包含自訂用戶端驗證程式碼，加入 jQuery`validator`物件。</span><span class="sxs-lookup"><span data-stu-id="8c408-198">This is shown in the example below that adds a method named `classicmovie` containing custom client validation code to the jQuery `validator` object.</span></span>

[!code-javascript[Main](validation/sample/Views/Movies/Create.cshtml?range=71-93)]

<span data-ttu-id="8c408-199">現在 jQuery 有執行自訂的 JavaScript 驗證，以及該驗證程式碼會傳回 false 時所顯示的錯誤訊息的資訊。</span><span class="sxs-lookup"><span data-stu-id="8c408-199">Now jQuery has the information to execute the custom JavaScript validation as well as the error message to display if that validation code returns false.</span></span>

## <a name="remote-validation"></a><span data-ttu-id="8c408-200">遠端驗證</span><span class="sxs-lookup"><span data-stu-id="8c408-200">Remote validation</span></span>

<span data-ttu-id="8c408-201">遠端驗證是很棒的功能，您需要驗證用戶端對伺服器上的資料上的資料時使用。</span><span class="sxs-lookup"><span data-stu-id="8c408-201">Remote validation is a great feature to use when you need to validate data on the client against data on the server.</span></span> <span data-ttu-id="8c408-202">例如，可能需要您的應用程式，請確認是否有電子郵件或使用者名稱已在使用，而且它必須查詢大量的資料，若要這樣做。</span><span class="sxs-lookup"><span data-stu-id="8c408-202">For example, your app may need to verify whether an email or user name is already in use, and it must query a large amount of data to do so.</span></span> <span data-ttu-id="8c408-203">下載大型的其中一個驗證的資料集或幾個欄位會耗用太多資源。</span><span class="sxs-lookup"><span data-stu-id="8c408-203">Downloading large sets of data for validating one or a few fields consumes too many resources.</span></span> <span data-ttu-id="8c408-204">它可能也會公開機密資訊。</span><span class="sxs-lookup"><span data-stu-id="8c408-204">It may also expose sensitive information.</span></span> <span data-ttu-id="8c408-205">另一個方法是反覆存取要求驗證欄位。</span><span class="sxs-lookup"><span data-stu-id="8c408-205">An alternative is to make a round-trip request to validate a field.</span></span>

<span data-ttu-id="8c408-206">您可以在兩個步驟的程序中實作遠端驗證。</span><span class="sxs-lookup"><span data-stu-id="8c408-206">You can implement remote validation in a two step process.</span></span> <span data-ttu-id="8c408-207">首先，您必須加上註解您的模型與`[Remote]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-207">First, you must annotate your model with the `[Remote]` attribute.</span></span> <span data-ttu-id="8c408-208">`[Remote]`屬性接受多個多載可用來導向到適當的程式碼以呼叫的用戶端 JavaScript。</span><span class="sxs-lookup"><span data-stu-id="8c408-208">The `[Remote]` attribute accepts multiple overloads you can use to direct client side JavaScript to the appropriate code to call.</span></span> <span data-ttu-id="8c408-209">此範例會指向`VerifyEmail`動作方法的`Users`控制站。</span><span class="sxs-lookup"><span data-stu-id="8c408-209">The example points to the `VerifyEmail` action method of the `Users` controller.</span></span>

[!code-csharp[Main](validation/sample/User.cs?range=5-9)]

<span data-ttu-id="8c408-210">第二個步驟將驗證程式碼中對應的動作方法中所定義`[Remote]`屬性。</span><span class="sxs-lookup"><span data-stu-id="8c408-210">The second step is putting the validation code in the corresponding action method as defined in the `[Remote]` attribute.</span></span> <span data-ttu-id="8c408-211">它會傳回`JsonResult`用戶端可以用來繼續執行或是暫停並顯示一則錯誤，如有需要。</span><span class="sxs-lookup"><span data-stu-id="8c408-211">It returns a `JsonResult` that the client side can use to proceed or pause and display an error if needed.</span></span>

[!code-none[Main](validation/sample/UsersController.cs?range=19-28)]

<span data-ttu-id="8c408-212">現在當使用者輸入的電子郵件，檢視中的 JavaScript 進行遠端呼叫是否已佔用該電子郵件，請參閱，而且如果是，接著會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="8c408-212">Now when users enter an email, JavaScript in the view makes a remote call to see if that email has been taken, and if so, then displays the error message.</span></span> <span data-ttu-id="8c408-213">否則，使用者可以像往常一樣送出表單。</span><span class="sxs-lookup"><span data-stu-id="8c408-213">Otherwise, the user can submit the form as usual.</span></span>
