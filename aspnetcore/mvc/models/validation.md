---
title: ASP.NET Core MVC 中的模型驗證
author: rick-anderson
description: 了解 ASP.NET Core MVC 和 Razor Pages 中的模型驗證。
ms.author: riande
ms.custom: mvc
ms.date: 12/15/2019
uid: mvc/models/validation
ms.openlocfilehash: b697f02183c76b9a96471a748a86c144fde47bb0
ms.sourcegitcommit: f259889044d1fc0f0c7e3882df0008157ced4915
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/18/2020
ms.locfileid: "76268747"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="bad4d-103">ASP.NET Core MVC 和 Razor Pages 中的模型驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="bad4d-104">依[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="bad4d-104">By [Kirk Larkin](https://github.com/serpent5)</span></span>

<span data-ttu-id="bad4d-105">本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-105">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="bad4d-106">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bad4d-106">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/samples) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="bad4d-107">模型狀態</span><span class="sxs-lookup"><span data-stu-id="bad4d-107">Model state</span></span>

<span data-ttu-id="bad4d-108">模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-108">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="bad4d-109">源自[模型](model-binding.md)系結的錯誤通常是資料轉換錯誤。</span><span class="sxs-lookup"><span data-stu-id="bad4d-109">Errors that originate from [model binding](model-binding.md) are generally data conversion errors.</span></span> <span data-ttu-id="bad4d-110">例如，在整數位段中輸入 "x"。</span><span class="sxs-lookup"><span data-stu-id="bad4d-110">For example, an "x" is entered in an integer field.</span></span> <span data-ttu-id="bad4d-111">模型驗證會在模型系結之後發生，並報告資料不符合商務規則的錯誤。</span><span class="sxs-lookup"><span data-stu-id="bad4d-111">Model validation occurs after model binding and reports errors where data doesn't conform to business rules.</span></span> <span data-ttu-id="bad4d-112">例如，在欄位中輸入0時，預期會有1到5之間的評等。</span><span class="sxs-lookup"><span data-stu-id="bad4d-112">For example, a 0 is entered in a field that expects a rating between 1 and 5.</span></span>

<span data-ttu-id="bad4d-113">模型系結和模型驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-113">Both model binding and model validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="bad4d-114">針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="bad4d-114">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="bad4d-115">Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="bad4d-115">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

<span data-ttu-id="bad4d-116">如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-116">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="bad4d-117">在此情況下，當模型狀態無效時，會傳回包含錯誤詳細資料的自動 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="bad4d-117">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="bad4d-118">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-118">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="bad4d-119">重新執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-119">Rerun validation</span></span>

<span data-ttu-id="bad4d-120">驗證會自動進行，但建議您以手動方式重複進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-120">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="bad4d-121">例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-121">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="bad4d-122">若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-122">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml.cs?name=snippet_TryValidate&highlight=3-6)]

## <a name="validation-attributes"></a><span data-ttu-id="bad4d-123">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-123">Validation attributes</span></span>

<span data-ttu-id="bad4d-124">驗證屬性可讓您指定模型屬性的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="bad4d-124">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="bad4d-125">範例應用程式的下列範例顯示以驗證屬性標注的模型類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-125">The following example from the sample app shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="bad4d-126">`[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。</span><span class="sxs-lookup"><span data-stu-id="bad4d-126">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="bad4d-127">未顯示 `[ClassicMovieWithClientValidator]`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-127">Not shown is `[ClassicMovieWithClientValidator]`.</span></span> <span data-ttu-id="bad4d-128">`[ClassicMovieWithClientValidator]` 會顯示執行自訂屬性的替代方式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-128">`[ClassicMovieWithClientValidator]` shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/Movie.cs?name=snippet_Class)]

## <a name="built-in-attributes"></a><span data-ttu-id="bad4d-129">內建屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-129">Built-in attributes</span></span>

<span data-ttu-id="bad4d-130">下列是部分內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="bad4d-130">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="bad4d-131">`[CreditCard]`：驗證屬性是否具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-131">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="bad4d-132">`[Compare]`：驗證模型中的兩個屬性是否相符。</span><span class="sxs-lookup"><span data-stu-id="bad4d-132">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="bad4d-133">`[EmailAddress]`：驗證屬性是否具有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-133">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="bad4d-134">`[Phone]`：驗證屬性是否具有電話號碼格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-134">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="bad4d-135">`[Range]`：驗證屬性值是否落在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="bad4d-135">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="bad4d-136">`[RegularExpression]`：驗證屬性值符合指定的正則運算式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-136">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="bad4d-137">`[Required]`：驗證欄位不是 null。</span><span class="sxs-lookup"><span data-stu-id="bad4d-137">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="bad4d-138">如需此屬性行為的詳細資料，請參閱[`[Required]` 屬性](#required-attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-138">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="bad4d-139">`[StringLength]`：驗證字串屬性值未超過指定的長度限制。</span><span class="sxs-lookup"><span data-stu-id="bad4d-139">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="bad4d-140">`[Url]`：驗證屬性是否具有 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-140">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="bad4d-141">`[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-141">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="bad4d-142">如需此屬性行為的詳細資料，請參閱[`[Remote]` 屬性](#remote-attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-142">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="bad4d-143">您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-143">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="bad4d-144">錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="bad4d-144">Error messages</span></span>

<span data-ttu-id="bad4d-145">驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-145">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="bad4d-146">例如：</span><span class="sxs-lookup"><span data-stu-id="bad4d-146">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="bad4d-147">就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。</span><span class="sxs-lookup"><span data-stu-id="bad4d-147">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="bad4d-148">例如：</span><span class="sxs-lookup"><span data-stu-id="bad4d-148">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="bad4d-149">套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。</span><span class="sxs-lookup"><span data-stu-id="bad4d-149">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="bad4d-150">若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-150">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="bad4d-151">[必要] 屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-151">[Required] attribute</span></span>

<span data-ttu-id="bad4d-152">.NET Core 3.0 和更新版本中的驗證系統會將不可為 null 的參數或系結屬性視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-152">The validation system in .NET Core 3.0 and later treats non-nullable parameters or bound properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-153">例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="bad4d-153">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span> <span data-ttu-id="bad4d-154">藉由設定 `Startup.ConfigureServices`中的 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes>，即可停用此行為：</span><span class="sxs-lookup"><span data-stu-id="bad4d-154">This behavior can be disabled by configuring <xref:Microsoft.AspNetCore.Mvc.MvcOptions.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes> in `Startup.ConfigureServices`:</span></span>

<span data-ttu-id="bad4d-155">' ' csharp 服務。AddControllers （options = > 選項。SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true）;...</span><span class="sxs-lookup"><span data-stu-id="bad4d-155">\`\`csharp services.AddControllers(options => options.SuppressImplicitRequiredAttributeForNonNullableReferenceTypes = true); ...</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="bad4d-156">[必要] 在伺服器上執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-156">[Required] validation on the server</span></span>

<span data-ttu-id="bad4d-157">在伺服器上，如果必要的值屬性為 Null，則會視為遺失。</span><span class="sxs-lookup"><span data-stu-id="bad4d-157">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="bad4d-158">不可為 null 的欄位一律有效，而且永遠不會顯示 `[Required]` 屬性的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-158">A non-nullable field is always valid, and the `[Required]` attribute's error message is never displayed.</span></span>

<span data-ttu-id="bad4d-159">不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-159">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="bad4d-160">若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="bad4d-160">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="bad4d-161">將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-161">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="bad4d-162">[可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-162">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="bad4d-163">指定模型繫結要使用的預設錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-163">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=5-6)]

  <span data-ttu-id="bad4d-164">如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。</span><span class="sxs-lookup"><span data-stu-id="bad4d-164">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="bad4d-165">[必要] 在用戶端上執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-165">[Required] validation on the client</span></span>

<span data-ttu-id="bad4d-166">不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。</span><span class="sxs-lookup"><span data-stu-id="bad4d-166">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="bad4d-167">在用戶端上：</span><span class="sxs-lookup"><span data-stu-id="bad4d-167">On the client:</span></span>

* <span data-ttu-id="bad4d-168">只有在為值的輸入進行輸入後，才會將該值視為存在。</span><span class="sxs-lookup"><span data-stu-id="bad4d-168">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="bad4d-169">因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。</span><span class="sxs-lookup"><span data-stu-id="bad4d-169">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="bad4d-170">字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-170">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="bad4d-171">如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-171">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="bad4d-172">如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-172">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-173">這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-173">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-174">但是，如果您不使用該屬性，就會出現預設的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-174">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="bad4d-175">若要指定自訂錯誤訊息，請使用該屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-175">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="bad4d-176">[遠端] 屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-176">[Remote] attribute</span></span>

<span data-ttu-id="bad4d-177">`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-177">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="bad4d-178">例如，應用程式可能需要驗證使用者名稱是否已經在使用中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-178">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="bad4d-179">實作遠端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-179">To implement remote validation:</span></span>

1. <span data-ttu-id="bad4d-180">為 JavaScript 建立動作方法來呼叫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-180">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="bad4d-181">JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="bad4d-181">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="bad4d-182">`true` 表示輸入資料有效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-182">`true` means the input data is valid.</span></span>
   * <span data-ttu-id="bad4d-183">`false`、`undefined` 或 `null` 表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-183">`false`, `undefined`, or `null` means the input is invalid.</span></span> <span data-ttu-id="bad4d-184">顯示預設錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-184">Display the default error message.</span></span>
   * <span data-ttu-id="bad4d-185">任何其他字串都表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-185">Any other string means the input is invalid.</span></span> <span data-ttu-id="bad4d-186">將字串顯示為自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-186">Display the string as a custom error message.</span></span>

   <span data-ttu-id="bad4d-187">自訂錯誤訊息的動作方法範例如下：</span><span class="sxs-lookup"><span data-stu-id="bad4d-187">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="bad4d-188">在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-188">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Email)]
 
   <span data-ttu-id="bad4d-189">`[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="bad4d-189">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="bad4d-190">其他欄位</span><span class="sxs-lookup"><span data-stu-id="bad4d-190">Additional fields</span></span>

<span data-ttu-id="bad4d-191">`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。</span><span class="sxs-lookup"><span data-stu-id="bad4d-191">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="bad4d-192">例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。</span><span class="sxs-lookup"><span data-stu-id="bad4d-192">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="bad4d-193">下列範例顯示如何使用 `AdditionalFields`：</span><span class="sxs-lookup"><span data-stu-id="bad4d-193">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/User.cs?name=snippet_Name&highlight=1,5)]

<span data-ttu-id="bad4d-194">`AdditionalFields` 可以明確設定為字串 "FirstName" 和 "LastName"，但使用[nameof](/dotnet/csharp/language-reference/keywords/nameof)運算子可簡化稍後的重構。</span><span class="sxs-lookup"><span data-stu-id="bad4d-194">`AdditionalFields` could be set explicitly to the strings "FirstName" and "LastName", but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="bad4d-195">此驗證的動作方法必須同時接受 `firstName` 和 `lastName` 引數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-195">The action method for this validation must accept both `firstName` and `lastName` arguments:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="bad4d-196">當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-196">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="bad4d-197">若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。</span><span class="sxs-lookup"><span data-stu-id="bad4d-197">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="bad4d-198">例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-198">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="bad4d-199">如同所有屬性引數，`AdditionalFields` 必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-199">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="bad4d-200">因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-200">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="bad4d-201">內建屬性的替代項目</span><span class="sxs-lookup"><span data-stu-id="bad4d-201">Alternatives to built-in attributes</span></span>

<span data-ttu-id="bad4d-202">如果您需要內建屬性未提供的驗證，您可以：</span><span class="sxs-lookup"><span data-stu-id="bad4d-202">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="bad4d-203">[建立自訂屬性](#custom-attributes)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-203">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="bad4d-204">[實作 IValidatableObject](#ivalidatableobject)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-204">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="bad4d-205">自訂屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-205">Custom attributes</span></span>

<span data-ttu-id="bad4d-206">在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-206">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="bad4d-207">建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。</span><span class="sxs-lookup"><span data-stu-id="bad4d-207">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="bad4d-208">`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-208">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="bad4d-209">多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。</span><span class="sxs-lookup"><span data-stu-id="bad4d-209">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="bad4d-210">下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。</span><span class="sxs-lookup"><span data-stu-id="bad4d-210">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="bad4d-211">`[ClassicMovie]` 屬性：</span><span class="sxs-lookup"><span data-stu-id="bad4d-211">The `[ClassicMovie]` attribute:</span></span>

* <span data-ttu-id="bad4d-212">只在伺服器上執行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-212">Is only run on the server.</span></span>
* <span data-ttu-id="bad4d-213">若為傳統電影，會驗證發行日期：</span><span class="sxs-lookup"><span data-stu-id="bad4d-213">For Classic movies, validates the release date:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttribute.cs?name=snippet_Class)]

<span data-ttu-id="bad4d-214">上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。</span><span class="sxs-lookup"><span data-stu-id="bad4d-214">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="bad4d-215">驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-215">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="bad4d-216">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="bad4d-216">IValidatableObject</span></span>

<span data-ttu-id="bad4d-217">上述範例僅適用於 `Movie` 型別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-217">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="bad4d-218">類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-218">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Models/ValidatableMovie.cs?name=snippet_Class&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="bad4d-219">最上層節點驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-219">Top-level node validation</span></span>

<span data-ttu-id="bad4d-220">最上層節點包括：</span><span class="sxs-lookup"><span data-stu-id="bad4d-220">Top-level nodes include:</span></span>

* <span data-ttu-id="bad4d-221">動作參數</span><span class="sxs-lookup"><span data-stu-id="bad4d-221">Action parameters</span></span>
* <span data-ttu-id="bad4d-222">控制器屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-222">Controller properties</span></span>
* <span data-ttu-id="bad4d-223">頁面處理常式參數</span><span class="sxs-lookup"><span data-stu-id="bad4d-223">Page handler parameters</span></span>
* <span data-ttu-id="bad4d-224">頁面模型屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-224">Page model properties</span></span>

<span data-ttu-id="bad4d-225">除了驗證模型屬性，也會驗證模型所繫結的最上層節點。</span><span class="sxs-lookup"><span data-stu-id="bad4d-225">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="bad4d-226">在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-226">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="bad4d-227">最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-227">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="bad4d-228">在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-228">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAgeSignature)]

<span data-ttu-id="bad4d-229">在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-229">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="bad4d-230">第一個表單會提交 `99` 的 `Age` 值做為查詢字串參數： `https://localhost:5001/Users/CheckAge?Age=99`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-230">The first form submits an `Age` value of `99` as a query string parameter: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="bad4d-231">從查詢字串提交正確格式化的 `age` 時，即會驗證表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-231">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="bad4d-232">[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="bad4d-232">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="bad4d-233">由於 `age` 參數必須來自查詢字串，因此繫結會失敗。</span><span class="sxs-lookup"><span data-stu-id="bad4d-233">Binding fails because the `age` parameter must come from a query string.</span></span>

## <a name="maximum-errors"></a><span data-ttu-id="bad4d-234">最大錯誤數</span><span class="sxs-lookup"><span data-stu-id="bad4d-234">Maximum errors</span></span>

<span data-ttu-id="bad4d-235">達到最大錯誤數 (預設為 200) 時，就會停止驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-235">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="bad4d-236">您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：</span><span class="sxs-lookup"><span data-stu-id="bad4d-236">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=4)]

## <a name="maximum-recursion"></a><span data-ttu-id="bad4d-237">最大遞迴</span><span class="sxs-lookup"><span data-stu-id="bad4d-237">Maximum recursion</span></span>

<span data-ttu-id="bad4d-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="bad4d-238"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="bad4d-239">對於深度或無限遞迴的模型，驗證可能會導致堆疊溢位。</span><span class="sxs-lookup"><span data-stu-id="bad4d-239">For models that are deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="bad4d-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-240">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="bad4d-241">`MvcOptions.MaxValidationDepth` 的預設值為32。</span><span class="sxs-lookup"><span data-stu-id="bad4d-241">The default value of `MvcOptions.MaxValidationDepth` is 32.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="bad4d-242">自動最少運算</span><span class="sxs-lookup"><span data-stu-id="bad4d-242">Automatic short-circuit</span></span>

<span data-ttu-id="bad4d-243">如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="bad4d-244">執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。</span><span class="sxs-lookup"><span data-stu-id="bad4d-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="bad4d-245">停用驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-245">Disable validation</span></span>

<span data-ttu-id="bad4d-246">停用驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-246">To disable validation:</span></span>

1. <span data-ttu-id="bad4d-247">建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/NullObjectModelValidator.cs?name=snippet_Class)]

1. <span data-ttu-id="bad4d-248">將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="bad4d-249">您仍可能會看到來自模型繫結的模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="bad4d-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="bad4d-250">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-250">Client-side validation</span></span>

<span data-ttu-id="bad4d-251">用戶端驗證可避免送出無效的表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="bad4d-252">[送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="bad4d-253">當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。</span><span class="sxs-lookup"><span data-stu-id="bad4d-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="bad4d-254">*_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_Scripts)]

[!code-cshtml[](validation/samples/3.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_Scripts)]

<span data-ttu-id="bad4d-255">[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="bad4d-256">若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="bad4d-257">反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="bad4d-258">jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。</span><span class="sxs-lookup"><span data-stu-id="bad4d-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="bad4d-259">您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/3.x/ValidationSample/Pages/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=3-4)]

<span data-ttu-id="bad4d-260">上述標記協助程式會轉譯下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="bad4d-260">The preceding tag helpers render the following HTML:</span></span>

```html
<div class="form-group">
    <label class="control-label" for="Movie_ReleaseDate">Release Date</label>
    <input class="form-control" type="date" data-val="true"
        data-val-required="The Release Date field is required."
        id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
    <span class="text-danger field-validation-valid"
        data-valmsg-for="Movie.ReleaseDate" data-valmsg-replace="true"></span>
</div>
```

<span data-ttu-id="bad4d-261">請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `Movie.ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `Movie.ReleaseDate` property.</span></span> <span data-ttu-id="bad4d-262">`data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="bad4d-263">jQuery 不顯眼的驗證會將此值傳遞至 jQuery Validate [required （）](https://jqueryvalidation.org/required-method/)方法，然後在伴隨的 **\<範圍 >** 元素中顯示該訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="bad4d-264">資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="bad4d-265">瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="bad4d-266">`[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

## <a name="unobtrusive-validation"></a><span data-ttu-id="bad4d-267">不顯眼的驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-267">Unobtrusive validation</span></span>

<span data-ttu-id="bad4d-268">如需不顯眼驗證的相關資訊，請參閱[此 GitHub 問題](https://github.com/aspnet/AspNetCore.Docs/issues/1111)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-268">For information on unobtrusive validation, see [this GitHub issue](https://github.com/aspnet/AspNetCore.Docs/issues/1111).</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="bad4d-269">將驗證新增至動態表單</span><span class="sxs-lookup"><span data-stu-id="bad4d-269">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="bad4d-270">jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-270">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="bad4d-271">因此，驗證不會在動態產生的表單上自動運作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-271">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="bad4d-272">若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。</span><span class="sxs-lookup"><span data-stu-id="bad4d-272">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="bad4d-273">例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-273">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="bad4d-274">`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。</span><span class="sxs-lookup"><span data-stu-id="bad4d-274">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="bad4d-275">此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-275">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="bad4d-276">然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-276">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="bad4d-277">將驗證新增至動態控制項</span><span class="sxs-lookup"><span data-stu-id="bad4d-277">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="bad4d-278">`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-278">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="bad4d-279">若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-279">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="bad4d-280">自訂用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-280">Custom client-side validation</span></span>

<span data-ttu-id="bad4d-281">自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-281">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="bad4d-282">下列配接器程式碼範例，是針對本文稍早介紹的 `[ClassicMovie]` 和 `[ClassicMovieWithClientValidator]` 屬性所撰寫：</span><span class="sxs-lookup"><span data-stu-id="bad4d-282">The following sample adapter code was written for the `[ClassicMovie]` and `[ClassicMovieWithClientValidator]` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/3.x/ValidationSample/wwwroot/js/classicMovieValidator.js)]

<span data-ttu-id="bad4d-283">如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-283">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="bad4d-284">用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：</span><span class="sxs-lookup"><span data-stu-id="bad4d-284">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="bad4d-285">將欄位加上旗標為待驗證 (`data-val="true"`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-285">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="bad4d-286">識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-286">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="bad4d-287">提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-param1="value"`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-287">Provide any additional parameters the validator needs (for example, `data-val-rulename-param1="value"`).</span></span>

<span data-ttu-id="bad4d-288">下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：</span><span class="sxs-lookup"><span data-stu-id="bad4d-288">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="date"
    data-val="true"
    data-val-classicmovie="Classic movies must have a release year no later than 1960."
    data-val-classicmovie-year="1960"
    data-val-required="The Release Date field is required."
    id="Movie_ReleaseDate" name="Movie.ReleaseDate" value="">
```

<span data-ttu-id="bad4d-289">如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-289">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="bad4d-290">有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bad4d-290">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="bad4d-291">建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。</span><span class="sxs-lookup"><span data-stu-id="bad4d-291">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="bad4d-292">此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-292">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="bad4d-293">配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-293">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="bad4d-294">在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-294">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="bad4d-295">如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。</span><span class="sxs-lookup"><span data-stu-id="bad4d-295">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="bad4d-296">用戶端驗證的屬性配接器</span><span class="sxs-lookup"><span data-stu-id="bad4d-296">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="bad4d-297">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="bad4d-297">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="bad4d-298">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-298">To add client validation by using this method:</span></span>

1. <span data-ttu-id="bad4d-299">建立自訂驗證屬性的屬性配接器類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-299">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="bad4d-300">自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-300">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="bad4d-301">建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-301">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieAttributeAdapter.cs?name=snippet_Class)]

1. <span data-ttu-id="bad4d-302">建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-302">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="bad4d-303">在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-303">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/CustomValidationAttributeAdapterProvider.cs?name=snippet_Class)]

1. <span data-ttu-id="bad4d-304">在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：</span><span class="sxs-lookup"><span data-stu-id="bad4d-304">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_Configuration&highlight=9-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="bad4d-305">用戶端驗證的 IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="bad4d-305">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="bad4d-306">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovieWithClientValidator` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="bad4d-306">This method of rendering `data-` attributes in HTML is used by the `ClassicMovieWithClientValidator` attribute in the sample app.</span></span> <span data-ttu-id="bad4d-307">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-307">To add client validation by using this method:</span></span>

* <span data-ttu-id="bad4d-308">在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。</span><span class="sxs-lookup"><span data-stu-id="bad4d-308">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="bad4d-309">在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-309">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/3.x/ValidationSample/Validation/ClassicMovieWithClientValidatorAttribute.cs?name=snippet_Class)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="bad4d-310">停用用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-310">Disable client-side validation</span></span>

<span data-ttu-id="bad4d-311">下列程式碼會停用 Razor Pages 中的用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-311">The following code disables client validation in Razor Pages:</span></span>

[!code-csharp[](validation/samples/3.x/ValidationSample/Startup.cs?name=snippet_DisableClientValidation&highlight=2-5)]

<span data-ttu-id="bad4d-312">停用用戶端驗證的其他選項：</span><span class="sxs-lookup"><span data-stu-id="bad4d-312">Other options to disable client-side validation:</span></span>

* <span data-ttu-id="bad4d-313">將所有 *. cshtml*檔案中 `_ValidationScriptsPartial` 的參考批註在一起。</span><span class="sxs-lookup"><span data-stu-id="bad4d-313">Comment out the reference to `_ValidationScriptsPartial` in all the *.cshtml* files.</span></span>
* <span data-ttu-id="bad4d-314">移除*Pages\Shared\_ValidationScriptsPartial*的內容。</span><span class="sxs-lookup"><span data-stu-id="bad4d-314">Remove the contents of the *Pages\Shared\_ValidationScriptsPartial.cshtml* file.</span></span>

<span data-ttu-id="bad4d-315">上述方法不會防止用戶端驗證 ASP.NET Core 身分識別 Razor 類別庫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-315">The preceding approach won't prevent client side validation of ASP.NET Core Identity Razor Class Library.</span></span> <span data-ttu-id="bad4d-316">如需詳細資訊，請參閱<xref:security/authentication/scaffold-identity>。</span><span class="sxs-lookup"><span data-stu-id="bad4d-316">For more information, see <xref:security/authentication/scaffold-identity>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bad4d-317">其他資源</span><span class="sxs-lookup"><span data-stu-id="bad4d-317">Additional resources</span></span>

* [<span data-ttu-id="bad4d-318">System.ComponentModel.DataAnnotations 命名空間</span><span class="sxs-lookup"><span data-stu-id="bad4d-318">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="bad4d-319">模型繫結</span><span class="sxs-lookup"><span data-stu-id="bad4d-319">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="bad4d-320">本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-320">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="bad4d-321">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="bad4d-321">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="bad4d-322">模型狀態</span><span class="sxs-lookup"><span data-stu-id="bad4d-322">Model state</span></span>

<span data-ttu-id="bad4d-323">模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-323">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="bad4d-324">源自[模型繫結](model-binding.md)的錯誤通常是資料轉換錯誤 (例如在必須輸入整數的欄位中輸入 "x")。</span><span class="sxs-lookup"><span data-stu-id="bad4d-324">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="bad4d-325">模型驗證發生於模型繫結之後，並會在資料不符合商務規則時回報錯誤 (例如在必須輸入 1 到 5 之間評等的欄位中輸入 0)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-325">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="bad4d-326">模型繫結和驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-326">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="bad4d-327">針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="bad4d-327">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="bad4d-328">Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="bad4d-328">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="bad4d-329">如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-329">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="bad4d-330">在此情況下，當模型狀態無效時，會傳回包含錯誤詳細資料的自動 HTTP 400 回應。</span><span class="sxs-lookup"><span data-stu-id="bad4d-330">In that case, an automatic HTTP 400 response containing error details is returned when model state is invalid.</span></span> <span data-ttu-id="bad4d-331">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-331">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="bad4d-332">重新執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-332">Rerun validation</span></span>

<span data-ttu-id="bad4d-333">驗證會自動進行，但建議您以手動方式重複進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-333">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="bad4d-334">例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-334">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="bad4d-335">若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-335">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="bad4d-336">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-336">Validation attributes</span></span>

<span data-ttu-id="bad4d-337">驗證屬性可讓您指定模型屬性的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="bad4d-337">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="bad4d-338">下列來自[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample)的範例，顯示以驗證屬性標註的模型類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-338">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="bad4d-339">`[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。</span><span class="sxs-lookup"><span data-stu-id="bad4d-339">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="bad4d-340">[未顯示] 是 `[ClassicMovie2]`，這會顯示執行自訂屬性的替代方式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-340">Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="bad4d-341">內建屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-341">Built-in attributes</span></span>

<span data-ttu-id="bad4d-342">內建驗證屬性包括：</span><span class="sxs-lookup"><span data-stu-id="bad4d-342">Built-in validation attributes include:</span></span>

* <span data-ttu-id="bad4d-343">`[CreditCard]`：驗證屬性是否具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-343">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="bad4d-344">`[Compare]`：驗證模型中的兩個屬性是否相符。</span><span class="sxs-lookup"><span data-stu-id="bad4d-344">`[Compare]`: Validates that two properties in a model match.</span></span> <span data-ttu-id="bad4d-345">例如， *Register.cshtml.cs*檔案會使用 `[Compare]` 來驗證兩個輸入的密碼相符。</span><span class="sxs-lookup"><span data-stu-id="bad4d-345">For example, the *Register.cshtml.cs* file uses `[Compare]` to validate the two entered passwords match.</span></span> <span data-ttu-id="bad4d-346">[Scaffold 身分識別](xref:security/authentication/scaffold-identity) 以查看註冊程式碼。</span><span class="sxs-lookup"><span data-stu-id="bad4d-346">[Scaffold Identity](xref:security/authentication/scaffold-identity) to see the Register code.</span></span>
* <span data-ttu-id="bad4d-347">`[EmailAddress]`：驗證屬性是否具有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-347">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="bad4d-348">`[Phone]`：驗證屬性是否具有電話號碼格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-348">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="bad4d-349">`[Range]`：驗證屬性值是否落在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="bad4d-349">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="bad4d-350">`[RegularExpression]`：驗證屬性值符合指定的正則運算式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-350">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="bad4d-351">`[Required]`：驗證欄位不是 null。</span><span class="sxs-lookup"><span data-stu-id="bad4d-351">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="bad4d-352">如需此屬性行為的詳細資料，請參閱[`[Required]` 屬性](#required-attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-352">See [`[Required]` attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="bad4d-353">`[StringLength]`：驗證字串屬性值未超過指定的長度限制。</span><span class="sxs-lookup"><span data-stu-id="bad4d-353">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="bad4d-354">`[Url]`：驗證屬性是否具有 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-354">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="bad4d-355">`[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-355">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="bad4d-356">如需此屬性行為的詳細資料，請參閱[`[Remote]` 屬性](#remote-attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-356">See [`[Remote]` attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="bad4d-357">當您使用 `[RegularExpression]` 屬性搭配用戶端驗證時，會在用戶端上以 JavaScript 執行 RegEx。</span><span class="sxs-lookup"><span data-stu-id="bad4d-357">When using the `[RegularExpression]` attribute with client-side validation, the regex is executed in JavaScript on the client.</span></span> <span data-ttu-id="bad4d-358">這表示將會使用[ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior)比對行為。</span><span class="sxs-lookup"><span data-stu-id="bad4d-358">This means [ECMAScript](/dotnet/standard/base-types/regular-expression-options#ecmascript-matching-behavior) matching behavior will be used.</span></span> <span data-ttu-id="bad4d-359">如需詳細資訊，請參閱[這個 GitHub 問題](https://github.com/dotnet/corefx/issues/42487) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-359">For more information, see [this GitHub issue](https://github.com/dotnet/corefx/issues/42487).</span></span>

<span data-ttu-id="bad4d-360">您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-360">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="bad4d-361">錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="bad4d-361">Error messages</span></span>

<span data-ttu-id="bad4d-362">驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-362">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="bad4d-363">例如：</span><span class="sxs-lookup"><span data-stu-id="bad4d-363">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="bad4d-364">就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。</span><span class="sxs-lookup"><span data-stu-id="bad4d-364">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="bad4d-365">例如：</span><span class="sxs-lookup"><span data-stu-id="bad4d-365">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="bad4d-366">套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。</span><span class="sxs-lookup"><span data-stu-id="bad4d-366">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="bad4d-367">若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-367">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="bad4d-368">[必要] 屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-368">[Required] attribute</span></span>

<span data-ttu-id="bad4d-369">根據預設，驗證系統會將不可為 Null 的參數或屬性視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-369">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-370">例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="bad4d-370">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="bad4d-371">[必要] 在伺服器上執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-371">[Required] validation on the server</span></span>

<span data-ttu-id="bad4d-372">在伺服器上，如果必要的值屬性為 Null，則會視為遺失。</span><span class="sxs-lookup"><span data-stu-id="bad4d-372">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="bad4d-373">不可為 Null 的欄位一律有效，且一律不會顯示 [必要] 屬性的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-373">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="bad4d-374">不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-374">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="bad4d-375">若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="bad4d-375">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="bad4d-376">將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-376">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="bad4d-377">[可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-377">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="bad4d-378">指定模型繫結要使用的預設錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-378">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="bad4d-379">如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。</span><span class="sxs-lookup"><span data-stu-id="bad4d-379">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="bad4d-380">[必要] 在用戶端上執行驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-380">[Required] validation on the client</span></span>

<span data-ttu-id="bad4d-381">不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。</span><span class="sxs-lookup"><span data-stu-id="bad4d-381">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="bad4d-382">在用戶端上：</span><span class="sxs-lookup"><span data-stu-id="bad4d-382">On the client:</span></span>

* <span data-ttu-id="bad4d-383">只有在為值的輸入進行輸入後，才會將該值視為存在。</span><span class="sxs-lookup"><span data-stu-id="bad4d-383">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="bad4d-384">因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。</span><span class="sxs-lookup"><span data-stu-id="bad4d-384">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="bad4d-385">字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-385">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="bad4d-386">如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-386">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="bad4d-387">如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-387">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-388">這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-388">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="bad4d-389">但是，如果您不使用該屬性，就會出現預設的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-389">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="bad4d-390">若要指定自訂錯誤訊息，請使用該屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-390">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="bad4d-391">[遠端] 屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-391">[Remote] attribute</span></span>

<span data-ttu-id="bad4d-392">`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-392">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="bad4d-393">例如，應用程式可能需要驗證使用者名稱是否已經在使用中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-393">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="bad4d-394">實作遠端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-394">To implement remote validation:</span></span>

1. <span data-ttu-id="bad4d-395">為 JavaScript 建立動作方法來呼叫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-395">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="bad4d-396">JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="bad4d-396">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="bad4d-397">`"true"` 表示輸入資料有效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-397">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="bad4d-398">`"false"`、`undefined` 或 `null` 表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-398">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="bad4d-399">顯示預設錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-399">Display the default error message.</span></span>
   * <span data-ttu-id="bad4d-400">任何其他字串都表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="bad4d-400">Any other string means the input is invalid.</span></span> <span data-ttu-id="bad4d-401">將字串顯示為自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-401">Display the string as a custom error message.</span></span>

   <span data-ttu-id="bad4d-402">自訂錯誤訊息的動作方法範例如下：</span><span class="sxs-lookup"><span data-stu-id="bad4d-402">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="bad4d-403">在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-403">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="bad4d-404">`[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="bad4d-404">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="bad4d-405">如不使用 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 中繼套件，請安裝 [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="bad4d-405">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="bad4d-406">其他欄位</span><span class="sxs-lookup"><span data-stu-id="bad4d-406">Additional fields</span></span>

<span data-ttu-id="bad4d-407">`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。</span><span class="sxs-lookup"><span data-stu-id="bad4d-407">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="bad4d-408">例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。</span><span class="sxs-lookup"><span data-stu-id="bad4d-408">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="bad4d-409">下列範例顯示如何使用 `AdditionalFields`：</span><span class="sxs-lookup"><span data-stu-id="bad4d-409">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="bad4d-410">`AdditionalFields` 可以明確設定為 `"FirstName"` 和 `"LastName"`的字串，但使用[nameof](/dotnet/csharp/language-reference/keywords/nameof)運算子可簡化稍後的重構。</span><span class="sxs-lookup"><span data-stu-id="bad4d-410">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [nameof](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="bad4d-411">這項驗證的動作方法必須接受名字和姓氏引數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-411">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="bad4d-412">當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-412">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="bad4d-413">若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。</span><span class="sxs-lookup"><span data-stu-id="bad4d-413">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="bad4d-414">例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-414">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="bad4d-415">如同所有屬性引數，`AdditionalFields` 必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-415">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="bad4d-416">因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-416">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="bad4d-417">內建屬性的替代項目</span><span class="sxs-lookup"><span data-stu-id="bad4d-417">Alternatives to built-in attributes</span></span>

<span data-ttu-id="bad4d-418">如果您需要內建屬性未提供的驗證，您可以：</span><span class="sxs-lookup"><span data-stu-id="bad4d-418">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="bad4d-419">[建立自訂屬性](#custom-attributes)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-419">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="bad4d-420">[實作 IValidatableObject](#ivalidatableobject)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-420">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="bad4d-421">自訂屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-421">Custom attributes</span></span>

<span data-ttu-id="bad4d-422">在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-422">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="bad4d-423">建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。</span><span class="sxs-lookup"><span data-stu-id="bad4d-423">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="bad4d-424">`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。</span><span class="sxs-lookup"><span data-stu-id="bad4d-424">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="bad4d-425">多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。</span><span class="sxs-lookup"><span data-stu-id="bad4d-425">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="bad4d-426">下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。</span><span class="sxs-lookup"><span data-stu-id="bad4d-426">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="bad4d-427">`[ClassicMovie2]` 屬性會先檢查內容類型，如果是 *Classic* 才會繼續。</span><span class="sxs-lookup"><span data-stu-id="bad4d-427">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="bad4d-428">針對識別為 Classic 的電影，會檢查發行日期，以確定其不晚於傳遞至屬性建構函式的限制。)</span><span class="sxs-lookup"><span data-stu-id="bad4d-428">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="bad4d-429">上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。</span><span class="sxs-lookup"><span data-stu-id="bad4d-429">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="bad4d-430">`IsValid` 方法會檢查日期與內容類型。</span><span class="sxs-lookup"><span data-stu-id="bad4d-430">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="bad4d-431">驗證成功時，`IsValid` 會傳回 `ValidationResult.Success` 程式碼。</span><span class="sxs-lookup"><span data-stu-id="bad4d-431">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="bad4d-432">驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-432">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="bad4d-433">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="bad4d-433">IValidatableObject</span></span>

<span data-ttu-id="bad4d-434">上述範例僅適用於 `Movie` 型別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-434">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="bad4d-435">類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-435">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="bad4d-436">最上層節點驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-436">Top-level node validation</span></span>

<span data-ttu-id="bad4d-437">最上層節點包括：</span><span class="sxs-lookup"><span data-stu-id="bad4d-437">Top-level nodes include:</span></span>

* <span data-ttu-id="bad4d-438">動作參數</span><span class="sxs-lookup"><span data-stu-id="bad4d-438">Action parameters</span></span>
* <span data-ttu-id="bad4d-439">控制器屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-439">Controller properties</span></span>
* <span data-ttu-id="bad4d-440">頁面處理常式參數</span><span class="sxs-lookup"><span data-stu-id="bad4d-440">Page handler parameters</span></span>
* <span data-ttu-id="bad4d-441">頁面模型屬性</span><span class="sxs-lookup"><span data-stu-id="bad4d-441">Page model properties</span></span>

<span data-ttu-id="bad4d-442">除了驗證模型屬性，也會驗證模型所繫結的最上層節點。</span><span class="sxs-lookup"><span data-stu-id="bad4d-442">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="bad4d-443">在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-443">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="bad4d-444">最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-444">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="bad4d-445">在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：</span><span class="sxs-lookup"><span data-stu-id="bad4d-445">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="bad4d-446">在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-446">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="bad4d-447">第一個表單會提交 `Age` 值 `99` 作為查詢字串：`https://localhost:5001/Users/CheckAge?Age=99`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-447">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="bad4d-448">從查詢字串提交正確格式化的 `age` 時，即會驗證表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-448">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="bad4d-449">[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="bad4d-449">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="bad4d-450">由於 `age` 參數必須來自查詢字串，因此繫結會失敗。</span><span class="sxs-lookup"><span data-stu-id="bad4d-450">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="bad4d-451">執行 `CompatibilityVersion.Version_2_1` 或更新版本時，根據預設會啟用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-451">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="bad4d-452">否則，會停用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-452">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="bad4d-453">預設選項可以藉由設定 (`Startup.ConfigureServices`) 中的 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> 屬性來覆寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-453">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="bad4d-454">最大錯誤數</span><span class="sxs-lookup"><span data-stu-id="bad4d-454">Maximum errors</span></span>

<span data-ttu-id="bad4d-455">達到最大錯誤數 (預設為 200) 時，就會停止驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-455">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="bad4d-456">您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：</span><span class="sxs-lookup"><span data-stu-id="bad4d-456">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="bad4d-457">最大遞迴</span><span class="sxs-lookup"><span data-stu-id="bad4d-457">Maximum recursion</span></span>

<span data-ttu-id="bad4d-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="bad4d-458"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="bad4d-459">針對非常深或無限遞迴的模型，驗證可能會導致堆疊溢位。</span><span class="sxs-lookup"><span data-stu-id="bad4d-459">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="bad4d-460">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-460">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="bad4d-461">使用 `CompatibilityVersion.Version_2_2` 或更新版本執行時，`MvcOptions.MaxValidationDepth` 的預設值是32。</span><span class="sxs-lookup"><span data-stu-id="bad4d-461">The default value of `MvcOptions.MaxValidationDepth` is 32 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="bad4d-462">針對舊版，值為 Null，表示沒有深度條件約束。</span><span class="sxs-lookup"><span data-stu-id="bad4d-462">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="bad4d-463">自動最少運算</span><span class="sxs-lookup"><span data-stu-id="bad4d-463">Automatic short-circuit</span></span>

<span data-ttu-id="bad4d-464">如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-464">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="bad4d-465">執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。</span><span class="sxs-lookup"><span data-stu-id="bad4d-465">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="bad4d-466">停用驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-466">Disable validation</span></span>

<span data-ttu-id="bad4d-467">停用驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-467">To disable validation:</span></span>

1. <span data-ttu-id="bad4d-468">建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-468">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="bad4d-469">將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-469">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="bad4d-470">您仍可能會看到來自模型繫結的模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="bad4d-470">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="bad4d-471">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-471">Client-side validation</span></span>

<span data-ttu-id="bad4d-472">用戶端驗證可避免送出無效的表單。</span><span class="sxs-lookup"><span data-stu-id="bad4d-472">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="bad4d-473">[送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-473">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="bad4d-474">當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。</span><span class="sxs-lookup"><span data-stu-id="bad4d-474">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="bad4d-475">*_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-475">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="bad4d-476">[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-476">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="bad4d-477">若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-477">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="bad4d-478">反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-478">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="bad4d-479">jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。</span><span class="sxs-lookup"><span data-stu-id="bad4d-479">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="bad4d-480">您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-480">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/samples/2.x/ValidationSample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="bad4d-481">上述標記協助程式會轉譯下列 HTML。</span><span class="sxs-lookup"><span data-stu-id="bad4d-481">The preceding tag helpers render the following HTML.</span></span>

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
                id="ReleaseDate" name="ReleaseDate" value="">
                <span class="text-danger field-validation-valid"
                data-valmsg-for="ReleaseDate" data-valmsg-replace="true"></span>
            </div>
        </div>
    </div>
</form>
```

<span data-ttu-id="bad4d-482">請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-482">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="bad4d-483">`data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-483">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="bad4d-484">jQuery 不顯眼的驗證會將此值傳遞至 jQuery Validate [required （）](https://jqueryvalidation.org/required-method/)方法，然後在伴隨的 **\<範圍 >** 元素中顯示該訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-484">jQuery Unobtrusive Validation passes this value to the jQuery Validate [required()](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="bad4d-485">資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。</span><span class="sxs-lookup"><span data-stu-id="bad4d-485">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="bad4d-486">瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-486">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="bad4d-487">`[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="bad4d-487">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="bad4d-488">將驗證新增至動態表單</span><span class="sxs-lookup"><span data-stu-id="bad4d-488">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="bad4d-489">jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-489">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="bad4d-490">因此，驗證不會在動態產生的表單上自動運作。</span><span class="sxs-lookup"><span data-stu-id="bad4d-490">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="bad4d-491">若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。</span><span class="sxs-lookup"><span data-stu-id="bad4d-491">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="bad4d-492">例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="bad4d-492">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="bad4d-493">`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。</span><span class="sxs-lookup"><span data-stu-id="bad4d-493">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="bad4d-494">此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-494">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="bad4d-495">然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。</span><span class="sxs-lookup"><span data-stu-id="bad4d-495">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="bad4d-496">將驗證新增至動態控制項</span><span class="sxs-lookup"><span data-stu-id="bad4d-496">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="bad4d-497">`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-497">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="bad4d-498">若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-498">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="bad4d-499">自訂用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-499">Custom client-side validation</span></span>

<span data-ttu-id="bad4d-500">自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。</span><span class="sxs-lookup"><span data-stu-id="bad4d-500">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="bad4d-501">下列配接器程式碼範例，是針對本文稍早介紹的 `ClassicMovie` 和 `ClassicMovie2` 屬性所撰寫：</span><span class="sxs-lookup"><span data-stu-id="bad4d-501">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/samples/2.x/ValidationSample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="bad4d-502">如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-502">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="bad4d-503">用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：</span><span class="sxs-lookup"><span data-stu-id="bad4d-503">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="bad4d-504">將欄位加上旗標為待驗證 (`data-val="true"`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-504">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="bad4d-505">識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-505">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="bad4d-506">提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-parm1="value"`)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-506">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="bad4d-507">下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：</span><span class="sxs-lookup"><span data-stu-id="bad4d-507">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="bad4d-508">如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="bad4d-508">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="bad4d-509">有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：</span><span class="sxs-lookup"><span data-stu-id="bad4d-509">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="bad4d-510">建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。</span><span class="sxs-lookup"><span data-stu-id="bad4d-510">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="bad4d-511">此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。</span><span class="sxs-lookup"><span data-stu-id="bad4d-511">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="bad4d-512">配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。</span><span class="sxs-lookup"><span data-stu-id="bad4d-512">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="bad4d-513">在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="bad4d-513">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="bad4d-514">如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。</span><span class="sxs-lookup"><span data-stu-id="bad4d-514">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="bad4d-515">用戶端驗證的屬性配接器</span><span class="sxs-lookup"><span data-stu-id="bad4d-515">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="bad4d-516">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="bad4d-516">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="bad4d-517">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-517">To add client validation by using this method:</span></span>

1. <span data-ttu-id="bad4d-518">建立自訂驗證屬性的屬性配接器類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-518">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="bad4d-519">自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-519">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="bad4d-520">建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-520">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="bad4d-521">建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。</span><span class="sxs-lookup"><span data-stu-id="bad4d-521">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="bad4d-522">在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-522">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="bad4d-523">在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：</span><span class="sxs-lookup"><span data-stu-id="bad4d-523">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/samples/2.x/ValidationSample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="bad4d-524">用戶端驗證的 IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="bad4d-524">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="bad4d-525">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie2` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="bad4d-525">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="bad4d-526">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-526">To add client validation by using this method:</span></span>

* <span data-ttu-id="bad4d-527">在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。</span><span class="sxs-lookup"><span data-stu-id="bad4d-527">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="bad4d-528">在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="bad4d-528">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/samples/2.x/ValidationSample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="bad4d-529">停用用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="bad4d-529">Disable client-side validation</span></span>

<span data-ttu-id="bad4d-530">下列程式碼會停用 MVC 檢視中的用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="bad4d-530">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="bad4d-531">以及在 Razor Pages 中：</span><span class="sxs-lookup"><span data-stu-id="bad4d-531">And in Razor Pages:</span></span>

[!code-csharp[](validation/samples_snapshot/2.x/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="bad4d-532">停用用戶端驗證的另一個選項，是將 *.cshtml* 檔案中對 `_ValidationScriptsPartial` 的參考標記為註解。</span><span class="sxs-lookup"><span data-stu-id="bad4d-532">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="bad4d-533">其他資源</span><span class="sxs-lookup"><span data-stu-id="bad4d-533">Additional resources</span></span>

* [<span data-ttu-id="bad4d-534">System.ComponentModel.DataAnnotations 命名空間</span><span class="sxs-lookup"><span data-stu-id="bad4d-534">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="bad4d-535">模型繫結</span><span class="sxs-lookup"><span data-stu-id="bad4d-535">Model Binding</span></span>](model-binding.md)

::: moniker-end
