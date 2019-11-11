---
title: ASP.NET Core MVC 中的模型驗證
author: rick-anderson
description: 了解 ASP.NET Core MVC 和 Razor Pages 中的模型驗證。
ms.author: riande
ms.custom: mvc
ms.date: 11/11/2019
uid: mvc/models/validation
ms.openlocfilehash: 8e9e588c8665962b2fe285b0feab977b16938154
ms.sourcegitcommit: 99cdd60a26ff0970880bb43c558d78b1ef17c237
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/11/2019
ms.locfileid: "73915518"
---
# <a name="model-validation-in-aspnet-core-mvc-and-razor-pages"></a><span data-ttu-id="e47a9-103">ASP.NET Core MVC 和 Razor Pages 中的模型驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-103">Model validation in ASP.NET Core MVC and Razor Pages</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e47a9-104">本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-104">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="e47a9-105">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e47a9-105">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="e47a9-106">模型狀態</span><span class="sxs-lookup"><span data-stu-id="e47a9-106">Model state</span></span>

<span data-ttu-id="e47a9-107">模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-107">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="e47a9-108">源自[模型繫結](model-binding.md)的錯誤通常是資料轉換錯誤 (例如在必須輸入整數的欄位中輸入 "x")。</span><span class="sxs-lookup"><span data-stu-id="e47a9-108">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="e47a9-109">模型驗證發生於模型繫結之後，並會在資料不符合商務規則時回報錯誤 (例如在必須輸入 1 到 5 之間評等的欄位中輸入 0)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-109">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="e47a9-110">模型繫結和驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-110">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="e47a9-111">針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="e47a9-111">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="e47a9-112">Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="e47a9-112">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="e47a9-113">如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-113">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="e47a9-114">在此情況下，當模型狀態無效時，會自動傳回 HTTP 400 回應並包含問題的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-114">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="e47a9-115">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-115">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="e47a9-116">重新執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-116">Rerun validation</span></span>

<span data-ttu-id="e47a9-117">驗證會自動進行，但建議您以手動方式重複進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-117">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="e47a9-118">例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-118">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="e47a9-119">若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-119">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="e47a9-120">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-120">Validation attributes</span></span>

<span data-ttu-id="e47a9-121">驗證屬性可讓您指定模型屬性的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="e47a9-121">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="e47a9-122">下列來自[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample)的範例，顯示以驗證屬性標註的模型類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-122">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="e47a9-123">`[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。</span><span class="sxs-lookup"><span data-stu-id="e47a9-123">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="e47a9-124">(未顯示的是 `[ClassicMovie2]`，其會顯示替代方式來實作自訂屬性。)</span><span class="sxs-lookup"><span data-stu-id="e47a9-124">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="e47a9-125">內建屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-125">Built-in attributes</span></span>

<span data-ttu-id="e47a9-126">下列是部分內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="e47a9-126">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="e47a9-127">`[CreditCard]`：驗證屬性是否具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-127">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="e47a9-128">`[Compare]`：驗證模型中的兩個屬性是否相符。</span><span class="sxs-lookup"><span data-stu-id="e47a9-128">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="e47a9-129">`[EmailAddress]`：驗證屬性是否具有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-129">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="e47a9-130">`[Phone]`：驗證屬性是否具有電話號碼格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-130">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="e47a9-131">`[Range]`：驗證屬性值是否落在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="e47a9-131">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="e47a9-132">`[RegularExpression]`：驗證屬性值符合指定的正則運算式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-132">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="e47a9-133">`[Required]`：驗證欄位不是 null。</span><span class="sxs-lookup"><span data-stu-id="e47a9-133">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="e47a9-134">請參閱 [[必要] 屬性](#required-attribute)以取得此屬性行為的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-134">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="e47a9-135">`[StringLength]`：驗證字串屬性值未超過指定的長度限制。</span><span class="sxs-lookup"><span data-stu-id="e47a9-135">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="e47a9-136">`[Url]`：驗證屬性是否具有 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-136">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="e47a9-137">`[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-137">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="e47a9-138">請參閱 [[遠端] 屬性](#remote-attribute)以取得此屬性行為的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-138">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="e47a9-139">您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-139">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="e47a9-140">錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="e47a9-140">Error messages</span></span>

<span data-ttu-id="e47a9-141">驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-141">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="e47a9-142">例如:</span><span class="sxs-lookup"><span data-stu-id="e47a9-142">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="e47a9-143">就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。</span><span class="sxs-lookup"><span data-stu-id="e47a9-143">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="e47a9-144">例如:</span><span class="sxs-lookup"><span data-stu-id="e47a9-144">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="e47a9-145">套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。</span><span class="sxs-lookup"><span data-stu-id="e47a9-145">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="e47a9-146">若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-146">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="e47a9-147">[必要] 屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-147">[Required] attribute</span></span>

<span data-ttu-id="e47a9-148">根據預設，驗證系統會將不可為 Null 的參數或屬性視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-148">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-149">例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="e47a9-149">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="e47a9-150">[必要] 在伺服器上執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-150">[Required] validation on the server</span></span>

<span data-ttu-id="e47a9-151">在伺服器上，如果必要的值屬性為 Null，則會視為遺失。</span><span class="sxs-lookup"><span data-stu-id="e47a9-151">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="e47a9-152">不可為 Null 的欄位一律有效，且一律不會顯示 [必要] 屬性的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-152">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="e47a9-153">不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-153">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="e47a9-154">若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="e47a9-154">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="e47a9-155">將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-155">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="e47a9-156">[可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-156">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="e47a9-157">指定模型繫結要使用的預設錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-157">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="e47a9-158">如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。</span><span class="sxs-lookup"><span data-stu-id="e47a9-158">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="e47a9-159">[必要] 在用戶端上執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-159">[Required] validation on the client</span></span>

<span data-ttu-id="e47a9-160">不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。</span><span class="sxs-lookup"><span data-stu-id="e47a9-160">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="e47a9-161">在用戶端上：</span><span class="sxs-lookup"><span data-stu-id="e47a9-161">On the client:</span></span>

* <span data-ttu-id="e47a9-162">只有在為值的輸入進行輸入後，才會將該值視為存在。</span><span class="sxs-lookup"><span data-stu-id="e47a9-162">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="e47a9-163">因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。</span><span class="sxs-lookup"><span data-stu-id="e47a9-163">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="e47a9-164">字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-164">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="e47a9-165">如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-165">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="e47a9-166">如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-166">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-167">這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-167">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-168">但是，如果您不使用該屬性，就會出現預設的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-168">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="e47a9-169">若要指定自訂錯誤訊息，請使用該屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-169">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="e47a9-170">[遠端] 屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-170">[Remote] attribute</span></span>

<span data-ttu-id="e47a9-171">`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-171">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="e47a9-172">例如，應用程式可能需要驗證使用者名稱是否已經在使用中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-172">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="e47a9-173">實作遠端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-173">To implement remote validation:</span></span>

1. <span data-ttu-id="e47a9-174">為 JavaScript 建立動作方法來呼叫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-174">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="e47a9-175">JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e47a9-175">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="e47a9-176">`"true"` 表示輸入資料有效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-176">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="e47a9-177">`"false"`、`undefined` 或 `null` 表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-177">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="e47a9-178">顯示預設錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-178">Display the default error message.</span></span>
   * <span data-ttu-id="e47a9-179">任何其他字串都表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-179">Any other string means the input is invalid.</span></span> <span data-ttu-id="e47a9-180">將字串顯示為自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-180">Display the string as a custom error message.</span></span>

   <span data-ttu-id="e47a9-181">自訂錯誤訊息的動作方法範例如下：</span><span class="sxs-lookup"><span data-stu-id="e47a9-181">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="e47a9-182">在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-182">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="e47a9-183">`[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="e47a9-183">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="e47a9-184">如不使用 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 中繼套件，請安裝 [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e47a9-184">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="e47a9-185">其他欄位</span><span class="sxs-lookup"><span data-stu-id="e47a9-185">Additional fields</span></span>

<span data-ttu-id="e47a9-186">`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。</span><span class="sxs-lookup"><span data-stu-id="e47a9-186">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="e47a9-187">例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。</span><span class="sxs-lookup"><span data-stu-id="e47a9-187">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="e47a9-188">下列範例顯示如何使用 `AdditionalFields`：</span><span class="sxs-lookup"><span data-stu-id="e47a9-188">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="e47a9-189">`AdditionalFields` 可能已明確設定為 `"FirstName"` 和 `"LastName"` 字串，但使用 [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) 運算子可簡化稍後的重構。</span><span class="sxs-lookup"><span data-stu-id="e47a9-189">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="e47a9-190">這項驗證的動作方法必須接受名字和姓氏引數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-190">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="e47a9-191">當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-191">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="e47a9-192">若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。</span><span class="sxs-lookup"><span data-stu-id="e47a9-192">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="e47a9-193">例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-193">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="e47a9-194">如同所有屬性引數，`AdditionalFields` 必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-194">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="e47a9-195">因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-195">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="e47a9-196">內建屬性的替代項目</span><span class="sxs-lookup"><span data-stu-id="e47a9-196">Alternatives to built-in attributes</span></span>

<span data-ttu-id="e47a9-197">如果您需要內建屬性未提供的驗證，您可以：</span><span class="sxs-lookup"><span data-stu-id="e47a9-197">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="e47a9-198">[建立自訂屬性](#custom-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-198">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="e47a9-199">[實作 IValidatableObject](#ivalidatableobject)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-199">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="e47a9-200">自訂屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-200">Custom attributes</span></span>

<span data-ttu-id="e47a9-201">在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-201">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="e47a9-202">建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。</span><span class="sxs-lookup"><span data-stu-id="e47a9-202">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="e47a9-203">`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-203">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="e47a9-204">多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。</span><span class="sxs-lookup"><span data-stu-id="e47a9-204">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="e47a9-205">下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。</span><span class="sxs-lookup"><span data-stu-id="e47a9-205">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="e47a9-206">`[ClassicMovie2]` 屬性會先檢查內容類型，如果是 *Classic* 才會繼續。</span><span class="sxs-lookup"><span data-stu-id="e47a9-206">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="e47a9-207">針對識別為 Classic 的電影，會檢查發行日期，以確定其不晚於傳遞至屬性建構函式的限制。)</span><span class="sxs-lookup"><span data-stu-id="e47a9-207">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="e47a9-208">上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-208">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="e47a9-209">`IsValid` 方法會檢查日期與內容類型。</span><span class="sxs-lookup"><span data-stu-id="e47a9-209">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="e47a9-210">驗證成功時，`IsValid` 會傳回 `ValidationResult.Success` 程式碼。</span><span class="sxs-lookup"><span data-stu-id="e47a9-210">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="e47a9-211">驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-211">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="e47a9-212">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="e47a9-212">IValidatableObject</span></span>

<span data-ttu-id="e47a9-213">上述範例僅適用於 `Movie` 型別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-213">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="e47a9-214">類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-214">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="e47a9-215">最上層節點驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-215">Top-level node validation</span></span>

<span data-ttu-id="e47a9-216">最上層節點包括：</span><span class="sxs-lookup"><span data-stu-id="e47a9-216">Top-level nodes include:</span></span>

* <span data-ttu-id="e47a9-217">動作參數</span><span class="sxs-lookup"><span data-stu-id="e47a9-217">Action parameters</span></span>
* <span data-ttu-id="e47a9-218">控制器屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-218">Controller properties</span></span>
* <span data-ttu-id="e47a9-219">頁面處理常式參數</span><span class="sxs-lookup"><span data-stu-id="e47a9-219">Page handler parameters</span></span>
* <span data-ttu-id="e47a9-220">頁面模型屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-220">Page model properties</span></span>

<span data-ttu-id="e47a9-221">除了驗證模型屬性，也會驗證模型所繫結的最上層節點。</span><span class="sxs-lookup"><span data-stu-id="e47a9-221">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="e47a9-222">在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-222">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="e47a9-223">最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-223">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="e47a9-224">在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-224">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="e47a9-225">在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-225">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="e47a9-226">第一個表單會提交 `Age` 值 `99` 作為查詢字串：`https://localhost:5001/Users/CheckAge?Age=99`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-226">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="e47a9-227">從查詢字串提交正確格式化的 `age` 時，即會驗證表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-227">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="e47a9-228">[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="e47a9-228">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="e47a9-229">由於 `age` 參數必須來自查詢字串，因此繫結會失敗。</span><span class="sxs-lookup"><span data-stu-id="e47a9-229">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="e47a9-230">執行 `CompatibilityVersion.Version_2_1` 或更新版本時，根據預設會啟用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-230">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="e47a9-231">否則，會停用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-231">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="e47a9-232">預設選項可以藉由設定 (`Startup.ConfigureServices`) 中的 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> 屬性來覆寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-232">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="e47a9-233">最大錯誤數</span><span class="sxs-lookup"><span data-stu-id="e47a9-233">Maximum errors</span></span>

<span data-ttu-id="e47a9-234">達到最大錯誤數 (預設為 200) 時，就會停止驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-234">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="e47a9-235">您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：</span><span class="sxs-lookup"><span data-stu-id="e47a9-235">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="e47a9-236">最大遞迴</span><span class="sxs-lookup"><span data-stu-id="e47a9-236">Maximum recursion</span></span>

<span data-ttu-id="e47a9-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e47a9-237"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="e47a9-238">針對非常深或無限遞迴的模型，驗證可能會導致堆疊溢位。</span><span class="sxs-lookup"><span data-stu-id="e47a9-238">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="e47a9-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-239">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="e47a9-240">執行 `CompatibilityVersion.Version_2_2` 或更新版本時，`MvcOptions.MaxValidationDepth` 的預設值是 200。</span><span class="sxs-lookup"><span data-stu-id="e47a9-240">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="e47a9-241">針對舊版，值為 Null，表示沒有深度條件約束。</span><span class="sxs-lookup"><span data-stu-id="e47a9-241">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="e47a9-242">自動最少運算</span><span class="sxs-lookup"><span data-stu-id="e47a9-242">Automatic short-circuit</span></span>

<span data-ttu-id="e47a9-243">如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-243">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="e47a9-244">執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e47a9-244">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="e47a9-245">停用驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-245">Disable validation</span></span>

<span data-ttu-id="e47a9-246">停用驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-246">To disable validation:</span></span>

1. <span data-ttu-id="e47a9-247">建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-247">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="e47a9-248">將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-248">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="e47a9-249">您仍可能會看到來自模型繫結的模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="e47a9-249">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="e47a9-250">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-250">Client-side validation</span></span>

<span data-ttu-id="e47a9-251">用戶端驗證可避免送出無效的表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-251">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="e47a9-252">[送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-252">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="e47a9-253">當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。</span><span class="sxs-lookup"><span data-stu-id="e47a9-253">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="e47a9-254">*_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-254">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="e47a9-255">[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-255">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="e47a9-256">若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-256">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="e47a9-257">反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-257">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="e47a9-258">jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。</span><span class="sxs-lookup"><span data-stu-id="e47a9-258">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="e47a9-259">您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-259">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="e47a9-260">上述標記協助程式會轉譯下列 HTML。</span><span class="sxs-lookup"><span data-stu-id="e47a9-260">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="e47a9-261">請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-261">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="e47a9-262">`data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-262">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="e47a9-263">jQuery 低調驗證會將此值傳遞至 jQuery 驗證的 [`required()`](https://jqueryvalidation.org/required-method/) 方法，然後在隨附的 **\<span>** 項目中顯示該訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-263">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="e47a9-264">資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-264">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="e47a9-265">瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-265">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="e47a9-266">`[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-266">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="e47a9-267">將驗證新增至動態表單</span><span class="sxs-lookup"><span data-stu-id="e47a9-267">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="e47a9-268">jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-268">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="e47a9-269">因此，驗證不會在動態產生的表單上自動運作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-269">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="e47a9-270">若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。</span><span class="sxs-lookup"><span data-stu-id="e47a9-270">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="e47a9-271">例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-271">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="e47a9-272">`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。</span><span class="sxs-lookup"><span data-stu-id="e47a9-272">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="e47a9-273">此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-273">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="e47a9-274">然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-274">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="e47a9-275">將驗證新增至動態控制項</span><span class="sxs-lookup"><span data-stu-id="e47a9-275">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="e47a9-276">`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-276">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="e47a9-277">若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-277">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="e47a9-278">自訂用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-278">Custom client-side validation</span></span>

<span data-ttu-id="e47a9-279">自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-279">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="e47a9-280">下列配接器程式碼範例，是針對本文稍早介紹的 `ClassicMovie` 和 `ClassicMovie2` 屬性所撰寫：</span><span class="sxs-lookup"><span data-stu-id="e47a9-280">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="e47a9-281">如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-281">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="e47a9-282">用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：</span><span class="sxs-lookup"><span data-stu-id="e47a9-282">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="e47a9-283">將欄位加上旗標為待驗證 (`data-val="true"`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-283">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="e47a9-284">識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-284">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="e47a9-285">提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-parm1="value"`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-285">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="e47a9-286">下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e47a9-286">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="e47a9-287">如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-287">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="e47a9-288">有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e47a9-288">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="e47a9-289">建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。</span><span class="sxs-lookup"><span data-stu-id="e47a9-289">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="e47a9-290">此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-290">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="e47a9-291">配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-291">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="e47a9-292">在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-292">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="e47a9-293">如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。</span><span class="sxs-lookup"><span data-stu-id="e47a9-293">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="e47a9-294">用戶端驗證的屬性配接器</span><span class="sxs-lookup"><span data-stu-id="e47a9-294">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="e47a9-295">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="e47a9-295">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="e47a9-296">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-296">To add client validation by using this method:</span></span>

1. <span data-ttu-id="e47a9-297">建立自訂驗證屬性的屬性配接器類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-297">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="e47a9-298">自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-298">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="e47a9-299">建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-299">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="e47a9-300">建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-300">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="e47a9-301">在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-301">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="e47a9-302">在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：</span><span class="sxs-lookup"><span data-stu-id="e47a9-302">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="e47a9-303">用戶端驗證的 IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="e47a9-303">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="e47a9-304">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie2` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="e47a9-304">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="e47a9-305">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-305">To add client validation by using this method:</span></span>

* <span data-ttu-id="e47a9-306">在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。</span><span class="sxs-lookup"><span data-stu-id="e47a9-306">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="e47a9-307">在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-307">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="e47a9-308">停用用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-308">Disable client-side validation</span></span>

<span data-ttu-id="e47a9-309">下列程式碼會停用 MVC 檢視中的用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-309">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="e47a9-310">以及在 Razor Pages 中：</span><span class="sxs-lookup"><span data-stu-id="e47a9-310">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="e47a9-311">停用用戶端驗證的另一個選項，是將 *.cshtml* 檔案中對 `_ValidationScriptsPartial` 的參考標記為註解。</span><span class="sxs-lookup"><span data-stu-id="e47a9-311">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e47a9-312">其他資源</span><span class="sxs-lookup"><span data-stu-id="e47a9-312">Additional resources</span></span>

* [<span data-ttu-id="e47a9-313">System.ComponentModel.DataAnnotations 命名空間</span><span class="sxs-lookup"><span data-stu-id="e47a9-313">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="e47a9-314">模型繫結</span><span class="sxs-lookup"><span data-stu-id="e47a9-314">Model Binding</span></span>](model-binding.md)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e47a9-315">本文說明如何在 ASP.NET Core MVC 或 Razor Pages 應用程式中驗證使用者輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-315">This article explains how to validate user input in an ASP.NET Core MVC or Razor Pages app.</span></span>

<span data-ttu-id="e47a9-316">[檢視或下載範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="e47a9-316">[View or download sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) ([how to download](xref:index#how-to-download-a-sample)).</span></span>

## <a name="model-state"></a><span data-ttu-id="e47a9-317">模型狀態</span><span class="sxs-lookup"><span data-stu-id="e47a9-317">Model state</span></span>

<span data-ttu-id="e47a9-318">模型狀態代表來自兩個子系統的錯誤：模型繫結和模型驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-318">Model state represents errors that come from two subsystems: model binding and model validation.</span></span> <span data-ttu-id="e47a9-319">源自[模型繫結](model-binding.md)的錯誤通常是資料轉換錯誤 (例如在必須輸入整數的欄位中輸入 "x")。</span><span class="sxs-lookup"><span data-stu-id="e47a9-319">Errors that originate from [model binding](model-binding.md) are generally data conversion errors (for example, an "x" is entered in a field that expects an integer).</span></span> <span data-ttu-id="e47a9-320">模型驗證發生於模型繫結之後，並會在資料不符合商務規則時回報錯誤 (例如在必須輸入 1 到 5 之間評等的欄位中輸入 0)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-320">Model validation occurs after model binding and reports errors where the data doesn't conform to business rules (for example, a 0 is entered in a field that expects a rating between 1 and 5).</span></span>

<span data-ttu-id="e47a9-321">模型繫結和驗證會在執行控制器動作或 Razor Pages 處理常式方法之前進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-321">Both model binding and validation occur before the execution of a controller action or a Razor Pages handler method.</span></span> <span data-ttu-id="e47a9-322">針對 Web 應用程式，此應用程式的責任為檢查 `ModelState.IsValid` 並做出適當回應。</span><span class="sxs-lookup"><span data-stu-id="e47a9-322">For web apps, it's the app's responsibility to inspect `ModelState.IsValid` and react appropriately.</span></span> <span data-ttu-id="e47a9-323">Web 應用程式通常會重新顯示頁面，並出現錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="e47a9-323">Web apps typically redisplay the page with an error message:</span></span>

[!code-csharp[](validation/sample_snapshot/Create.cshtml.cs?name=snippet&highlight=3-6)]

<span data-ttu-id="e47a9-324">如果 Web API 控制器具有 `[ApiController]` 屬性，則該控制器就不需要檢查 `ModelState.IsValid`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-324">Web API controllers don't have to check `ModelState.IsValid` if they have the `[ApiController]` attribute.</span></span> <span data-ttu-id="e47a9-325">在此情況下，當模型狀態無效時，會自動傳回 HTTP 400 回應並包含問題的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-325">In that case, an automatic HTTP 400 response containing issue details is returned when model state is invalid.</span></span> <span data-ttu-id="e47a9-326">如需詳細資訊，請參閱[自動 HTTP 400 回應](xref:web-api/index#automatic-http-400-responses)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-326">For more information, see [Automatic HTTP 400 responses](xref:web-api/index#automatic-http-400-responses).</span></span>

## <a name="rerun-validation"></a><span data-ttu-id="e47a9-327">重新執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-327">Rerun validation</span></span>

<span data-ttu-id="e47a9-328">驗證會自動進行，但建議您以手動方式重複進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-328">Validation is automatic, but you might want to repeat it manually.</span></span> <span data-ttu-id="e47a9-329">例如，您可能會計算屬性的值，並在將屬性設定為計算的值之後重新執行驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-329">For example, you might compute a value for a property and want to rerun validation after setting the property to the computed value.</span></span> <span data-ttu-id="e47a9-330">若要重新執行驗證，請呼叫 `TryValidateModel` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-330">To rerun validation, call the `TryValidateModel` method, as shown here:</span></span>

[!code-csharp[](validation/sample/Controllers/MoviesController.cs?name=snippet_TryValidateModel&highlight=11)]

## <a name="validation-attributes"></a><span data-ttu-id="e47a9-331">驗證屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-331">Validation attributes</span></span>

<span data-ttu-id="e47a9-332">驗證屬性可讓您指定模型屬性的驗證規則。</span><span class="sxs-lookup"><span data-stu-id="e47a9-332">Validation attributes let you specify validation rules for model properties.</span></span> <span data-ttu-id="e47a9-333">下列來自[範例應用程式](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample)的範例，顯示以驗證屬性標註的模型類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-333">The following example from [the sample app](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/mvc/models/validation/sample) shows a model class that is annotated with validation attributes.</span></span> <span data-ttu-id="e47a9-334">`[ClassicMovie]` 屬性是自訂驗證屬性，其他的則是內建。</span><span class="sxs-lookup"><span data-stu-id="e47a9-334">The `[ClassicMovie]` attribute is a custom validation attribute and the others are built-in.</span></span> <span data-ttu-id="e47a9-335">(未顯示的是 `[ClassicMovie2]`，其會顯示替代方式來實作自訂屬性。)</span><span class="sxs-lookup"><span data-stu-id="e47a9-335">(Not shown is `[ClassicMovie2]`, which shows an alternative way to implement a custom attribute.)</span></span>

[!code-csharp[](validation/sample/Models/Movie.cs?name=snippet_ModelClass)]

## <a name="built-in-attributes"></a><span data-ttu-id="e47a9-336">內建屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-336">Built-in attributes</span></span>

<span data-ttu-id="e47a9-337">下列是部分內建驗證屬性：</span><span class="sxs-lookup"><span data-stu-id="e47a9-337">Here are some of the built-in validation attributes:</span></span>

* <span data-ttu-id="e47a9-338">`[CreditCard]`：驗證屬性是否具有信用卡格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-338">`[CreditCard]`: Validates that the property has a credit card format.</span></span>
* <span data-ttu-id="e47a9-339">`[Compare]`：驗證模型中的兩個屬性是否相符。</span><span class="sxs-lookup"><span data-stu-id="e47a9-339">`[Compare]`: Validates that two properties in a model match.</span></span>
* <span data-ttu-id="e47a9-340">`[EmailAddress]`：驗證屬性是否具有電子郵件格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-340">`[EmailAddress]`: Validates that the property has an email format.</span></span>
* <span data-ttu-id="e47a9-341">`[Phone]`：驗證屬性是否具有電話號碼格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-341">`[Phone]`: Validates that the property has a telephone number format.</span></span>
* <span data-ttu-id="e47a9-342">`[Range]`：驗證屬性值是否落在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="e47a9-342">`[Range]`: Validates that the property value falls within a specified range.</span></span>
* <span data-ttu-id="e47a9-343">`[RegularExpression]`：驗證屬性值符合指定的正則運算式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-343">`[RegularExpression]`: Validates that the property value matches a specified regular expression.</span></span>
* <span data-ttu-id="e47a9-344">`[Required]`：驗證欄位不是 null。</span><span class="sxs-lookup"><span data-stu-id="e47a9-344">`[Required]`: Validates that the field is not null.</span></span> <span data-ttu-id="e47a9-345">請參閱 [[必要] 屬性](#required-attribute)以取得此屬性行為的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-345">See [[Required] attribute](#required-attribute) for details about this attribute's behavior.</span></span>
* <span data-ttu-id="e47a9-346">`[StringLength]`：驗證字串屬性值未超過指定的長度限制。</span><span class="sxs-lookup"><span data-stu-id="e47a9-346">`[StringLength]`: Validates that a string property value doesn't exceed a specified length limit.</span></span>
* <span data-ttu-id="e47a9-347">`[Url]`：驗證屬性是否具有 URL 格式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-347">`[Url]`: Validates that the property has a URL format.</span></span>
* <span data-ttu-id="e47a9-348">`[Remote]`：藉由在伺服器上呼叫動作方法，驗證用戶端上的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-348">`[Remote]`: Validates input on the client by calling an action method on the server.</span></span> <span data-ttu-id="e47a9-349">請參閱 [[遠端] 屬性](#remote-attribute)以取得此屬性行為的詳細資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-349">See [[Remote] attribute](#remote-attribute) for details about this attribute's behavior.</span></span>

<span data-ttu-id="e47a9-350">您可以在 [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) 命名空間中找到驗證屬性的完整清單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-350">A complete list of validation attributes can be found in the [System.ComponentModel.DataAnnotations](xref:System.ComponentModel.DataAnnotations) namespace.</span></span>

### <a name="error-messages"></a><span data-ttu-id="e47a9-351">錯誤訊息</span><span class="sxs-lookup"><span data-stu-id="e47a9-351">Error messages</span></span>

<span data-ttu-id="e47a9-352">驗證屬性可讓您指定要顯示的無效輸入錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-352">Validation attributes let you specify the error message to be displayed for invalid input.</span></span> <span data-ttu-id="e47a9-353">例如:</span><span class="sxs-lookup"><span data-stu-id="e47a9-353">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "Name length can't be more than 8.")]
```

<span data-ttu-id="e47a9-354">就內部而言，屬性會使用欄位名稱的預留位置呼叫 `String.Format`，有時候也會使用其他預留位置。</span><span class="sxs-lookup"><span data-stu-id="e47a9-354">Internally, the attributes call `String.Format` with a placeholder for the field name and sometimes additional placeholders.</span></span> <span data-ttu-id="e47a9-355">例如:</span><span class="sxs-lookup"><span data-stu-id="e47a9-355">For example:</span></span>

```csharp
[StringLength(8, ErrorMessage = "{0} length must be between {2} and {1}.", MinimumLength = 6)]
```

<span data-ttu-id="e47a9-356">套用至 `Name` 屬性時，透過上述程式碼建立的錯誤訊息會是 「名稱長度必須介於 6 和 8 之間」。</span><span class="sxs-lookup"><span data-stu-id="e47a9-356">When applied to a `Name` property, the error message created by the preceding code would be "Name length must be between 6 and 8.".</span></span>

<span data-ttu-id="e47a9-357">若要找出哪些參數傳遞給 `String.Format` 以取得特定屬性的錯誤訊息，請參閱 [DataAnnotations 原始程式碼](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-357">To find out which parameters are passed to `String.Format` for a particular attribute's error message, see the [DataAnnotations source code](https://github.com/dotnet/corefx/tree/master/src/System.ComponentModel.Annotations/src/System/ComponentModel/DataAnnotations).</span></span>

## <a name="required-attribute"></a><span data-ttu-id="e47a9-358">[必要] 屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-358">[Required] attribute</span></span>

<span data-ttu-id="e47a9-359">根據預設，驗證系統會將不可為 Null 的參數或屬性視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-359">By default, the validation system treats non-nullable parameters or properties as if they had a `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-360">例如 `decimal` 和 `int` 的[實值型別](/dotnet/csharp/language-reference/keywords/value-types)不可為 Null。</span><span class="sxs-lookup"><span data-stu-id="e47a9-360">[Value types](/dotnet/csharp/language-reference/keywords/value-types) such as `decimal` and `int` are non-nullable.</span></span>

### <a name="required-validation-on-the-server"></a><span data-ttu-id="e47a9-361">[必要] 在伺服器上執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-361">[Required] validation on the server</span></span>

<span data-ttu-id="e47a9-362">在伺服器上，如果必要的值屬性為 Null，則會視為遺失。</span><span class="sxs-lookup"><span data-stu-id="e47a9-362">On the server, a required value is considered missing if the property is null.</span></span> <span data-ttu-id="e47a9-363">不可為 Null 的欄位一律有效，且一律不會顯示 [必要] 屬性的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-363">A non-nullable field is always valid, and the [Required] attribute's error message is never displayed.</span></span>

<span data-ttu-id="e47a9-364">不過，不可為 Null 屬性的模型繫結可能會失敗，並產生一則錯誤訊息，例如 `The value '' is invalid`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-364">However, model binding for a non-nullable property may fail, resulting in an error message such as `The value '' is invalid`.</span></span> <span data-ttu-id="e47a9-365">若要針對不可為 Null 型別的伺服器端驗證指定自訂錯誤訊息，您可以使用下列選項：</span><span class="sxs-lookup"><span data-stu-id="e47a9-365">To specify a custom error message for server-side validation of non-nullable types, you have the following options:</span></span>

* <span data-ttu-id="e47a9-366">將欄位設成可為 Null (例如 `decimal?`，而不是 `decimal`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-366">Make the field nullable (for example, `decimal?` instead of `decimal`).</span></span> <span data-ttu-id="e47a9-367">[可為 Null\<T>](/dotnet/csharp/programming-guide/nullable-types/) 實值型別會視為如同標準的可為 Null 型別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-367">[Nullable\<T>](/dotnet/csharp/programming-guide/nullable-types/) value types are treated like standard nullable types.</span></span>
* <span data-ttu-id="e47a9-368">指定模型繫結要使用的預設錯誤訊息，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-368">Specify the default error message to be used by model binding, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=4-5)]

  <span data-ttu-id="e47a9-369">如需您可以為其設定預設訊息之模型繫結錯誤的詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>。</span><span class="sxs-lookup"><span data-stu-id="e47a9-369">For more information about model binding errors that you can set default messages for, see <xref:Microsoft.AspNetCore.Mvc.ModelBinding.Metadata.DefaultModelBindingMessageProvider#methods>.</span></span>

### <a name="required-validation-on-the-client"></a><span data-ttu-id="e47a9-370">[必要] 在用戶端上執行驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-370">[Required] validation on the client</span></span>

<span data-ttu-id="e47a9-371">不可為 Null 的型別和字串，會在用戶端上以不同於在伺服器上的方式處理。</span><span class="sxs-lookup"><span data-stu-id="e47a9-371">Non-nullable types and strings are handled differently on the client compared to the server.</span></span> <span data-ttu-id="e47a9-372">在用戶端上：</span><span class="sxs-lookup"><span data-stu-id="e47a9-372">On the client:</span></span>

* <span data-ttu-id="e47a9-373">只有在為值的輸入進行輸入後，才會將該值視為存在。</span><span class="sxs-lookup"><span data-stu-id="e47a9-373">A value is considered present only if input is entered for it.</span></span> <span data-ttu-id="e47a9-374">因此，用戶端驗證處理非 Null 型別的方式，會與可為 Null 型別的方式相同。</span><span class="sxs-lookup"><span data-stu-id="e47a9-374">Therefore, client-side validation handles non-nullable types the same as nullable types.</span></span>
* <span data-ttu-id="e47a9-375">字串欄位中的空白字元，會以 jQuery 驗證[必要](https://jqueryvalidation.org/required-method/)方法視為有效的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-375">Whitespace in a string field is considered valid input by the jQuery Validation [required](https://jqueryvalidation.org/required-method/) method.</span></span> <span data-ttu-id="e47a9-376">如果僅輸入空白字元，則伺服器端驗證會將必要的字串欄位視為無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-376">Server-side validation considers a required string field invalid if only whitespace is entered.</span></span>

<span data-ttu-id="e47a9-377">如先前所述，不可為 Null 的型別會視為具有 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-377">As noted earlier, non-nullable types are treated as though they had a `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-378">這表示即使您不套用 `[Required]` 屬性，也可以取得用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-378">That means you get client-side validation even if you don't apply the `[Required]` attribute.</span></span> <span data-ttu-id="e47a9-379">但是，如果您不使用該屬性，就會出現預設的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-379">But if you don't use the attribute, you get a default error message.</span></span> <span data-ttu-id="e47a9-380">若要指定自訂錯誤訊息，請使用該屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-380">To specify a custom error message, use the attribute.</span></span>

## <a name="remote-attribute"></a><span data-ttu-id="e47a9-381">[遠端] 屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-381">[Remote] attribute</span></span>

<span data-ttu-id="e47a9-382">`[Remote]` 屬性會實作用戶端驗證，該驗證需要呼叫伺服器上的方法來判斷欄位輸入是否有效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-382">The `[Remote]` attribute implements client-side validation that requires calling a method on the server to determine whether field input is valid.</span></span> <span data-ttu-id="e47a9-383">例如，應用程式可能需要驗證使用者名稱是否已經在使用中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-383">For example, the app may need to verify whether a user name is already in use.</span></span>

<span data-ttu-id="e47a9-384">實作遠端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-384">To implement remote validation:</span></span>

1. <span data-ttu-id="e47a9-385">為 JavaScript 建立動作方法來呼叫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-385">Create an action method for JavaScript to call.</span></span>  <span data-ttu-id="e47a9-386">JQuery 驗證[遠端](https://jqueryvalidation.org/remote-method/)方法會預期 JSON 回應：</span><span class="sxs-lookup"><span data-stu-id="e47a9-386">The jQuery Validate [remote](https://jqueryvalidation.org/remote-method/) method expects a JSON response:</span></span>

   * <span data-ttu-id="e47a9-387">`"true"` 表示輸入資料有效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-387">`"true"` means the input data is valid.</span></span>
   * <span data-ttu-id="e47a9-388">`"false"`、`undefined` 或 `null` 表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-388">`"false"`, `undefined`, or `null` means the input is invalid.</span></span>  <span data-ttu-id="e47a9-389">顯示預設錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-389">Display the default error message.</span></span>
   * <span data-ttu-id="e47a9-390">任何其他字串都表示輸入無效。</span><span class="sxs-lookup"><span data-stu-id="e47a9-390">Any other string means the input is invalid.</span></span> <span data-ttu-id="e47a9-391">將字串顯示為自訂錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-391">Display the string as a custom error message.</span></span>

   <span data-ttu-id="e47a9-392">自訂錯誤訊息的動作方法範例如下：</span><span class="sxs-lookup"><span data-stu-id="e47a9-392">Here's an example of an action method that returns a custom error message:</span></span>

   [!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyEmail)]

1. <span data-ttu-id="e47a9-393">在模型類別中，使用指向驗證動作方法的 `[Remote]` 屬性 (Attribute) 來標註屬性 (Property)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-393">In the model class, annotate the property with a `[Remote]` attribute that points to the validation action method, as shown in the following example:</span></span>

   [!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserEmailProperty)]
 
   <span data-ttu-id="e47a9-394">`[Remote]` 屬性位於 `Microsoft.AspNetCore.Mvc` 命名空間。</span><span class="sxs-lookup"><span data-stu-id="e47a9-394">The `[Remote]` attribute is in the `Microsoft.AspNetCore.Mvc` namespace.</span></span> <span data-ttu-id="e47a9-395">如不使用 `Microsoft.AspNetCore.App` 或 `Microsoft.AspNetCore.All` 中繼套件，請安裝 [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="e47a9-395">Install the [Microsoft.AspNetCore.Mvc.ViewFeatures](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.ViewFeatures) NuGet package if you're not using the `Microsoft.AspNetCore.App` or `Microsoft.AspNetCore.All` metapackage.</span></span>
   
### <a name="additional-fields"></a><span data-ttu-id="e47a9-396">其他欄位</span><span class="sxs-lookup"><span data-stu-id="e47a9-396">Additional fields</span></span>

<span data-ttu-id="e47a9-397">`[Remote]` 屬性 (Attribute) 的 `AdditionalFields` 屬性 (Property) 可讓您針對伺服器上的資料來驗證欄位組合。</span><span class="sxs-lookup"><span data-stu-id="e47a9-397">The `AdditionalFields` property of the `[Remote]` attribute lets you validate combinations of fields against data on the server.</span></span> <span data-ttu-id="e47a9-398">例如，如果 `User` 模型具 `FirstName` 和 `LastName` 屬性，建議您確認沒有任何現有使用者已具有該組名稱。</span><span class="sxs-lookup"><span data-stu-id="e47a9-398">For example, if the `User` model had `FirstName` and `LastName` properties, you might want to verify that no existing users already have that pair of names.</span></span> <span data-ttu-id="e47a9-399">下列範例顯示如何使用 `AdditionalFields`：</span><span class="sxs-lookup"><span data-stu-id="e47a9-399">The following example shows how to use `AdditionalFields`:</span></span>

[!code-csharp[](validation/sample/Models/User.cs?name=snippet_UserNameProperties)]

<span data-ttu-id="e47a9-400">`AdditionalFields` 可能已明確設定為 `"FirstName"` 和 `"LastName"` 字串，但使用 [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) 運算子可簡化稍後的重構。</span><span class="sxs-lookup"><span data-stu-id="e47a9-400">`AdditionalFields` could be set explicitly to the strings `"FirstName"` and `"LastName"`, but using the [`nameof`](/dotnet/csharp/language-reference/keywords/nameof) operator simplifies later refactoring.</span></span> <span data-ttu-id="e47a9-401">這項驗證的動作方法必須接受名字和姓氏引數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-401">The action method for this validation must accept both first name and last name arguments:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyName)]

<span data-ttu-id="e47a9-402">當使用者輸入名字或姓氏時，JavaScript 會進行遠端呼叫以查看該組名稱是否已在使用中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-402">When the user enters a first or last name, JavaScript makes a remote call to see if that pair of names has been taken.</span></span>

<span data-ttu-id="e47a9-403">若要驗證兩個或多個其他欄位，請以逗號分隔清單來提供這些欄位。</span><span class="sxs-lookup"><span data-stu-id="e47a9-403">To validate two or more additional fields, provide them as a comma-delimited list.</span></span> <span data-ttu-id="e47a9-404">例如，若要將 `MiddleName` 屬性 (Property) 新增至模型，請設定 `[Remote]` 屬性 (Attribute)，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-404">For example, to add a `MiddleName` property to the model, set the `[Remote]` attribute as shown in the following example:</span></span>

```cs
[Remote(action: "VerifyName", controller: "Users", AdditionalFields = nameof(FirstName) + "," + nameof(LastName))]
public string MiddleName { get; set; }
```

<span data-ttu-id="e47a9-405">如同所有屬性引數，`AdditionalFields` 必須是常數運算式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-405">`AdditionalFields`, like all attribute arguments, must be a constant expression.</span></span> <span data-ttu-id="e47a9-406">因此，請勿使用[內插字串](/dotnet/csharp/language-reference/keywords/interpolated-strings)或呼叫 <xref:System.String.Join*> 來初始化 `AdditionalFields`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-406">Therefore, don't use an [interpolated string](/dotnet/csharp/language-reference/keywords/interpolated-strings) or call <xref:System.String.Join*> to initialize `AdditionalFields`.</span></span>

## <a name="alternatives-to-built-in-attributes"></a><span data-ttu-id="e47a9-407">內建屬性的替代項目</span><span class="sxs-lookup"><span data-stu-id="e47a9-407">Alternatives to built-in attributes</span></span>

<span data-ttu-id="e47a9-408">如果您需要內建屬性未提供的驗證，您可以：</span><span class="sxs-lookup"><span data-stu-id="e47a9-408">If you need validation not provided by built-in attributes, you can:</span></span>

* <span data-ttu-id="e47a9-409">[建立自訂屬性](#custom-attributes)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-409">[Create custom attributes](#custom-attributes).</span></span>
* <span data-ttu-id="e47a9-410">[實作 IValidatableObject](#ivalidatableobject)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-410">[Implement IValidatableObject](#ivalidatableobject).</span></span>

## <a name="custom-attributes"></a><span data-ttu-id="e47a9-411">自訂屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-411">Custom attributes</span></span>

<span data-ttu-id="e47a9-412">在內建驗證屬性無法處理的情況下，您可以建立自訂驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-412">For scenarios that the built-in validation attributes don't handle, you can create custom validation attributes.</span></span> <span data-ttu-id="e47a9-413">建立繼承自 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute> 的類別，並覆寫 <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> 方法。</span><span class="sxs-lookup"><span data-stu-id="e47a9-413">Create a class that inherits from <xref:System.ComponentModel.DataAnnotations.ValidationAttribute>, and override the <xref:System.ComponentModel.DataAnnotations.ValidationAttribute.IsValid*> method.</span></span>

<span data-ttu-id="e47a9-414">`IsValid` 方法會接受名為 *value* 的物件，這是要驗證的輸入。</span><span class="sxs-lookup"><span data-stu-id="e47a9-414">The `IsValid` method accepts an object named *value*, which is the input to be validated.</span></span> <span data-ttu-id="e47a9-415">多載也會接受 `ValidationContext` 物件，該物件會提供其他資訊，例如模型繫結所建立的模型執行個體。</span><span class="sxs-lookup"><span data-stu-id="e47a9-415">An overload also accepts a `ValidationContext` object, which provides additional information, such as the model instance created by model binding.</span></span>

<span data-ttu-id="e47a9-416">下列範例會驗證 *Classic* 內容類型中電影的發行日期，不晚於指定的年份。</span><span class="sxs-lookup"><span data-stu-id="e47a9-416">The following example validates that the release date for a movie in the *Classic* genre isn't later than a specified year.</span></span> <span data-ttu-id="e47a9-417">`[ClassicMovie2]` 屬性會先檢查內容類型，如果是 *Classic* 才會繼續。</span><span class="sxs-lookup"><span data-stu-id="e47a9-417">The `[ClassicMovie2]` attribute checks the genre first and continues only if it's *Classic*.</span></span> <span data-ttu-id="e47a9-418">針對識別為 Classic 的電影，會檢查發行日期，以確定其不晚於傳遞至屬性建構函式的限制。)</span><span class="sxs-lookup"><span data-stu-id="e47a9-418">For movies identified as classics, it checks the release date to make sure it's not later than the limit passed to the attribute constructor.)</span></span>

[!code-csharp[](validation/sample/Attributes/ClassicMovieAttribute.cs?name=snippet_ClassicMovieAttribute)]

<span data-ttu-id="e47a9-419">上述範例中的 `movie` 變數代表 `Movie` 物件，該物件包含表單送出中的資料。</span><span class="sxs-lookup"><span data-stu-id="e47a9-419">The `movie` variable in the preceding example represents a `Movie` object that contains the data from the form submission.</span></span> <span data-ttu-id="e47a9-420">`IsValid` 方法會檢查日期與內容類型。</span><span class="sxs-lookup"><span data-stu-id="e47a9-420">The `IsValid` method checks the date and genre.</span></span> <span data-ttu-id="e47a9-421">驗證成功時，`IsValid` 會傳回 `ValidationResult.Success` 程式碼。</span><span class="sxs-lookup"><span data-stu-id="e47a9-421">Upon successful validation, `IsValid` returns a `ValidationResult.Success` code.</span></span> <span data-ttu-id="e47a9-422">驗證失敗時，會傳回 `ValidationResult` 和錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-422">When validation fails, a `ValidationResult` with an error message is returned.</span></span>

## <a name="ivalidatableobject"></a><span data-ttu-id="e47a9-423">IValidatableObject</span><span class="sxs-lookup"><span data-stu-id="e47a9-423">IValidatableObject</span></span>

<span data-ttu-id="e47a9-424">上述範例僅適用於 `Movie` 型別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-424">The preceding example works only with `Movie` types.</span></span> <span data-ttu-id="e47a9-425">類別層級驗證的另一個選項，是在模型類別中實作 `IValidatableObject`，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-425">Another option for class-level validation is to implement `IValidatableObject` in the model class, as shown in the following example:</span></span>

[!code-csharp[](validation/sample/Models/MovieIValidatable.cs?name=snippet&highlight=1,26-34)]

## <a name="top-level-node-validation"></a><span data-ttu-id="e47a9-426">最上層節點驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-426">Top-level node validation</span></span>

<span data-ttu-id="e47a9-427">最上層節點包括：</span><span class="sxs-lookup"><span data-stu-id="e47a9-427">Top-level nodes include:</span></span>

* <span data-ttu-id="e47a9-428">動作參數</span><span class="sxs-lookup"><span data-stu-id="e47a9-428">Action parameters</span></span>
* <span data-ttu-id="e47a9-429">控制器屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-429">Controller properties</span></span>
* <span data-ttu-id="e47a9-430">頁面處理常式參數</span><span class="sxs-lookup"><span data-stu-id="e47a9-430">Page handler parameters</span></span>
* <span data-ttu-id="e47a9-431">頁面模型屬性</span><span class="sxs-lookup"><span data-stu-id="e47a9-431">Page model properties</span></span>

<span data-ttu-id="e47a9-432">除了驗證模型屬性，也會驗證模型所繫結的最上層節點。</span><span class="sxs-lookup"><span data-stu-id="e47a9-432">Model-bound top-level nodes are validated in addition to validating model properties.</span></span> <span data-ttu-id="e47a9-433">在來自範例應用程式的下列範例中，`VerifyPhone` 方法會使用 <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> 來驗證 `phone` 動作參數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-433">In the following example from the sample app, the `VerifyPhone` method uses the <xref:System.ComponentModel.DataAnnotations.RegularExpressionAttribute> to validate the `phone` action parameter:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_VerifyPhone)]

<span data-ttu-id="e47a9-434">最上層節點可以搭配使用 <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> 和驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-434">Top-level nodes can use <xref:Microsoft.AspNetCore.Mvc.ModelBinding.BindRequiredAttribute> with validation attributes.</span></span> <span data-ttu-id="e47a9-435">在來自範例應用程式的下列範例中，`CheckAge` 方法會指定提交表單時必須從查詢字串繫結 `age` 參數：</span><span class="sxs-lookup"><span data-stu-id="e47a9-435">In the following example from the sample app, the `CheckAge` method specifies that the `age` parameter must be bound from the query string when the form is submitted:</span></span>

[!code-csharp[](validation/sample/Controllers/UsersController.cs?name=snippet_CheckAge)]

<span data-ttu-id="e47a9-436">在 [檢查年齡] 頁面 (*CheckAge.cshtml*) 中，有兩個表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-436">In the Check Age page (*CheckAge.cshtml*), there are two forms.</span></span> <span data-ttu-id="e47a9-437">第一個表單會提交 `Age` 值 `99` 作為查詢字串：`https://localhost:5001/Users/CheckAge?Age=99`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-437">The first form submits an `Age` value of `99` as a query string: `https://localhost:5001/Users/CheckAge?Age=99`.</span></span>

<span data-ttu-id="e47a9-438">從查詢字串提交正確格式化的 `age` 時，即會驗證表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-438">When a properly formatted `age` parameter from the query string is submitted, the form validates.</span></span>

<span data-ttu-id="e47a9-439">[檢查年齡] 頁面上的第二個表單會在要求本文中提交 `Age` 值，而且驗證會失敗。</span><span class="sxs-lookup"><span data-stu-id="e47a9-439">The second form on the Check Age page submits the `Age` value in the body of the request, and validation fails.</span></span> <span data-ttu-id="e47a9-440">由於 `age` 參數必須來自查詢字串，因此繫結會失敗。</span><span class="sxs-lookup"><span data-stu-id="e47a9-440">Binding fails because the `age` parameter must come from a query string.</span></span>

<span data-ttu-id="e47a9-441">執行 `CompatibilityVersion.Version_2_1` 或更新版本時，根據預設會啟用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-441">When running with `CompatibilityVersion.Version_2_1` or later, top-level node validation is enabled by default.</span></span> <span data-ttu-id="e47a9-442">否則，會停用最上層節點驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-442">Otherwise, top-level node validation is disabled.</span></span> <span data-ttu-id="e47a9-443">預設選項可以藉由設定 (`Startup.ConfigureServices`) 中的 <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> 屬性來覆寫，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-443">The default option can be overridden by setting the <xref:Microsoft.AspNetCore.Mvc.MvcOptions.AllowValidatingTopLevelNodes*> property in (`Startup.ConfigureServices`), as shown here:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup.cs?name=snippet_AddMvc&highlight=4)]

## <a name="maximum-errors"></a><span data-ttu-id="e47a9-444">最大錯誤數</span><span class="sxs-lookup"><span data-stu-id="e47a9-444">Maximum errors</span></span>

<span data-ttu-id="e47a9-445">達到最大錯誤數 (預設為 200) 時，就會停止驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-445">Validation stops when the maximum number of errors is reached (200 by default).</span></span> <span data-ttu-id="e47a9-446">您可以在 `Startup.ConfigureServices` 中使用下列程式碼來設定此數目：</span><span class="sxs-lookup"><span data-stu-id="e47a9-446">You can configure this number with the following code in `Startup.ConfigureServices`:</span></span>

[!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=3)]

## <a name="maximum-recursion"></a><span data-ttu-id="e47a9-447">最大遞迴</span><span class="sxs-lookup"><span data-stu-id="e47a9-447">Maximum recursion</span></span>

<span data-ttu-id="e47a9-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> 會周遊所正驗證之模型的物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e47a9-448"><xref:Microsoft.AspNetCore.Mvc.ModelBinding.Validation.ValidationVisitor> traverses the object graph of the model being validated.</span></span> <span data-ttu-id="e47a9-449">針對非常深或無限遞迴的模型，驗證可能會導致堆疊溢位。</span><span class="sxs-lookup"><span data-stu-id="e47a9-449">For models that are very deep or are infinitely recursive, validation may result in stack overflow.</span></span> <span data-ttu-id="e47a9-450">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) 可用來在訪客遞迴超過設定的深度時，提早停止驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-450">[MvcOptions.MaxValidationDepth](xref:Microsoft.AspNetCore.Mvc.MvcOptions.MaxValidationDepth) provides a way to stop validation early if the visitor recursion exceeds a configured depth.</span></span> <span data-ttu-id="e47a9-451">執行 `CompatibilityVersion.Version_2_2` 或更新版本時，`MvcOptions.MaxValidationDepth` 的預設值是 200。</span><span class="sxs-lookup"><span data-stu-id="e47a9-451">The default value of `MvcOptions.MaxValidationDepth` is 200 when running with `CompatibilityVersion.Version_2_2` or later.</span></span> <span data-ttu-id="e47a9-452">針對舊版，值為 Null，表示沒有深度條件約束。</span><span class="sxs-lookup"><span data-stu-id="e47a9-452">For earlier versions, the value is null, which means no depth constraint.</span></span>

## <a name="automatic-short-circuit"></a><span data-ttu-id="e47a9-453">自動最少運算</span><span class="sxs-lookup"><span data-stu-id="e47a9-453">Automatic short-circuit</span></span>

<span data-ttu-id="e47a9-454">如果模型圖形不需要驗證，則驗證會自動進行最少運算 (略過)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-454">Validation is automatically short-circuited (skipped) if the model graph doesn't require validation.</span></span> <span data-ttu-id="e47a9-455">執行階段會略過驗證的物件，包含基本集合 (例如 `byte[]`、`string[]`、`Dictionary<string, string>`) 和沒有任何驗證程式的複雜物件圖形。</span><span class="sxs-lookup"><span data-stu-id="e47a9-455">Objects that the runtime skips validation for include collections of primitives (such as `byte[]`, `string[]`, `Dictionary<string, string>`) and complex object graphs that don't have any validators.</span></span>

## <a name="disable-validation"></a><span data-ttu-id="e47a9-456">停用驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-456">Disable validation</span></span>

<span data-ttu-id="e47a9-457">停用驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-457">To disable validation:</span></span>

1. <span data-ttu-id="e47a9-458">建立不會將任何欄位標記為無效的 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-458">Create an implementation of `IObjectModelValidator` that doesn't mark any fields as invalid.</span></span>

   [!code-csharp[](validation/sample/Attributes/NullObjectModelValidator.cs?name=snippet_DisableValidation)]

1. <span data-ttu-id="e47a9-459">將下列程式碼新增至 `Startup.ConfigureServices`，以取代相依性插入容器中的預設 `IObjectModelValidator` 實作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-459">Add the following code to `Startup.ConfigureServices` to replace the default `IObjectModelValidator` implementation in the dependency injection container.</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_DisableValidation)]

<span data-ttu-id="e47a9-460">您仍可能會看到來自模型繫結的模型狀態錯誤。</span><span class="sxs-lookup"><span data-stu-id="e47a9-460">You might still see model state errors that originate from model binding.</span></span>

## <a name="client-side-validation"></a><span data-ttu-id="e47a9-461">用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-461">Client-side validation</span></span>

<span data-ttu-id="e47a9-462">用戶端驗證可避免送出無效的表單。</span><span class="sxs-lookup"><span data-stu-id="e47a9-462">Client-side validation prevents submission until the form is valid.</span></span> <span data-ttu-id="e47a9-463">[送出] 按鈕會執行 JavaScript 以送出表單或顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-463">The Submit button runs JavaScript that either submits the form or displays error messages.</span></span>

<span data-ttu-id="e47a9-464">當表單上存在輸入錯誤時，用戶端驗證可避免伺服器上不必要的來回往返。</span><span class="sxs-lookup"><span data-stu-id="e47a9-464">Client-side validation avoids an unnecessary round trip to the server when there are input errors on a form.</span></span> <span data-ttu-id="e47a9-465">*_Layout.cshtml* 和 *_ValidationScriptsPartial.cshtml* 中的下列指令碼參考支援用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-465">The following script references in *_Layout.cshtml* and *_ValidationScriptsPartial.cshtml* support client-side validation:</span></span>

[!code-cshtml[](validation/sample/Views/Shared/_Layout.cshtml?name=snippet_ScriptTag)]

[!code-cshtml[](validation/sample/Views/Shared/_ValidationScriptsPartial.cshtml?name=snippet_ScriptTags)]

<span data-ttu-id="e47a9-466">[jQuery 低調驗證](https://github.com/aspnet/jquery-validation-unobtrusive) (jQuery Unobtrusive Validation) 指令碼是建置在熱門 [jQuery 驗證](https://jqueryvalidation.org/) 外掛程式上的自訂 Microsoft 前端程式庫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-466">The [jQuery Unobtrusive Validation](https://github.com/aspnet/jquery-validation-unobtrusive) script is a custom Microsoft front-end library that builds on the popular [jQuery Validate](https://jqueryvalidation.org/) plugin.</span></span> <span data-ttu-id="e47a9-467">若沒有 jQuery 低調驗證，您就必須在兩個地方撰寫相同的驗證邏輯程式碼：一次在模型屬性 (Property) 上的伺服器端驗證屬性 (Attribute)，另一次在用戶端指令碼中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-467">Without jQuery Unobtrusive Validation, you would have to code the same validation logic in two places: once in the server-side validation attributes on model properties, and then again in client-side scripts.</span></span> <span data-ttu-id="e47a9-468">反之，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview)使用模型屬性 (Property) 中的驗證屬性 (Attribute) 和類型中繼資料，來轉譯需要驗證之表單項目的 HTML 5 `data-` 屬性 (Attribute)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-468">Instead, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use the validation attributes and type metadata from model properties to render HTML 5 `data-` attributes for the form elements that need validation.</span></span> <span data-ttu-id="e47a9-469">jQuery 低調驗證會剖析 `data-` 屬性並將邏輯傳遞至 jQuery 驗證，以有效地將伺服器端驗證邏輯「複製」到用戶端。</span><span class="sxs-lookup"><span data-stu-id="e47a9-469">jQuery Unobtrusive Validation parses the `data-` attributes and passes the logic to jQuery Validate, effectively "copying" the server-side validation logic to the client.</span></span> <span data-ttu-id="e47a9-470">您可以使用標記協助程式來顯示用戶端的驗證錯誤，如下所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-470">You can display validation errors on the client using tag helpers as shown here:</span></span>

[!code-cshtml[](validation/sample/Views/Movies/Create.cshtml?name=snippet_ReleaseDate&highlight=4-5)]

<span data-ttu-id="e47a9-471">上述標記協助程式會轉譯下列 HTML。</span><span class="sxs-lookup"><span data-stu-id="e47a9-471">The preceding tag helpers render the following HTML.</span></span>

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

<span data-ttu-id="e47a9-472">請注意，HTML 輸出中的 `data-` 屬性 (attribute) 會對應至 `ReleaseDate` 屬性 (property) 的驗證屬性 (attribute)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-472">Notice that the `data-` attributes in the HTML output correspond to the validation attributes for the `ReleaseDate` property.</span></span> <span data-ttu-id="e47a9-473">`data-val-required` 屬性包含使用者未填入發行日期欄位時所要顯示的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-473">The `data-val-required` attribute contains an error message to display if the user doesn't fill in the release date field.</span></span> <span data-ttu-id="e47a9-474">jQuery 低調驗證會將此值傳遞至 jQuery 驗證的 [`required()`](https://jqueryvalidation.org/required-method/) 方法，然後在隨附的 **\<span>** 項目中顯示該訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-474">jQuery Unobtrusive Validation passes this value to the jQuery Validate [`required()`](https://jqueryvalidation.org/required-method/) method, which then displays that message in the accompanying **\<span>** element.</span></span>

<span data-ttu-id="e47a9-475">資料型別驗證是根據屬性的 .NET 型別，除非是由 `[DataType]` 屬性覆寫。</span><span class="sxs-lookup"><span data-stu-id="e47a9-475">Data type validation is based on the .NET type of a property, unless that is overridden by a `[DataType]` attribute.</span></span> <span data-ttu-id="e47a9-476">瀏覽器具有自己的預設錯誤訊息，但 jQuery 驗證低調驗證套件可以覆寫這些訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-476">Browsers have their own default error messages, but the jQuery Validation Unobtrusive Validation package can override those messages.</span></span> <span data-ttu-id="e47a9-477">`[DataType]` 屬性和子類別 (例如 `[EmailAddress]`) 可讓您指定錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e47a9-477">`[DataType]` attributes and subclasses such as `[EmailAddress]` let you specify the error message.</span></span>

### <a name="add-validation-to-dynamic-forms"></a><span data-ttu-id="e47a9-478">將驗證新增至動態表單</span><span class="sxs-lookup"><span data-stu-id="e47a9-478">Add Validation to Dynamic Forms</span></span>

<span data-ttu-id="e47a9-479">jQuery 低調驗證會在第一次載入頁面時將驗證邏輯和傳遞參數至 jQuery 驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-479">jQuery Unobtrusive Validation passes validation logic and parameters to jQuery Validate when the page first loads.</span></span> <span data-ttu-id="e47a9-480">因此，驗證不會在動態產生的表單上自動運作。</span><span class="sxs-lookup"><span data-stu-id="e47a9-480">Therefore, validation doesn't work automatically on dynamically generated forms.</span></span> <span data-ttu-id="e47a9-481">若要啟用驗證，請指示 jQuery 低調驗證在建立動態表單之後立即進行剖析。</span><span class="sxs-lookup"><span data-stu-id="e47a9-481">To enable validation, tell jQuery Unobtrusive Validation to parse the dynamic form immediately after you create it.</span></span> <span data-ttu-id="e47a9-482">例如，下列程式碼會在透過 AJAX 新增的表單上設定用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e47a9-482">For example, the following code sets up client-side validation on a form added via AJAX.</span></span>

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

<span data-ttu-id="e47a9-483">`$.validator.unobtrusive.parse()` 方法接受 jQuery 選取器的一個引數。</span><span class="sxs-lookup"><span data-stu-id="e47a9-483">The `$.validator.unobtrusive.parse()` method accepts a jQuery selector for its one argument.</span></span> <span data-ttu-id="e47a9-484">此方法會指示 jQuery 低調驗證在該選取器內剖析表單的 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-484">This method tells jQuery Unobtrusive Validation to parse the `data-` attributes of forms within that selector.</span></span> <span data-ttu-id="e47a9-485">然後，這些屬性的值會傳遞至 jQuery 驗證外掛程式。</span><span class="sxs-lookup"><span data-stu-id="e47a9-485">The values of those attributes are then passed to the jQuery Validate plugin.</span></span>

### <a name="add-validation-to-dynamic-controls"></a><span data-ttu-id="e47a9-486">將驗證新增至動態控制項</span><span class="sxs-lookup"><span data-stu-id="e47a9-486">Add Validation to Dynamic Controls</span></span>

<span data-ttu-id="e47a9-487">`$.validator.unobtrusive.parse()` 方法適用於整個表單，而非動態產生的個別控制項 (例如 `<input>` 和 `<select/>`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-487">The `$.validator.unobtrusive.parse()` method works on an entire form, not on individual dynamically generated controls, such as `<input>` and `<select/>`.</span></span> <span data-ttu-id="e47a9-488">若要重新剖析表單，請移除先前剖析表單時新增的驗證資料，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-488">To reparse the form, remove the validation data that was added when the form was parsed earlier, as shown in the following example:</span></span>

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

## <a name="custom-client-side-validation"></a><span data-ttu-id="e47a9-489">自訂用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-489">Custom client-side validation</span></span>

<span data-ttu-id="e47a9-490">自訂用戶端驗證是透過產生使用自訂 jQuery 驗證配接器的 `data-` HTML 屬性來進行。</span><span class="sxs-lookup"><span data-stu-id="e47a9-490">Custom client-side validation is done by generating `data-` HTML attributes that work with a custom jQuery Validate adapter.</span></span> <span data-ttu-id="e47a9-491">下列配接器程式碼範例，是針對本文稍早介紹的 `ClassicMovie` 和 `ClassicMovie2` 屬性所撰寫：</span><span class="sxs-lookup"><span data-stu-id="e47a9-491">The following sample adapter code was written for the `ClassicMovie` and `ClassicMovie2` attributes that were introduced earlier in this article:</span></span>

[!code-javascript[](validation/sample/wwwroot/js/classicMovieValidator.js?name=snippet_UnobtrusiveValidation)]

<span data-ttu-id="e47a9-492">如需如何撰寫配接器的資訊，請參閱 [jQuery 驗證文件](https://jqueryvalidation.org/documentation/)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-492">For information about how to write adapters, see the [jQuery Validate documentation](https://jqueryvalidation.org/documentation/).</span></span>

<span data-ttu-id="e47a9-493">用於指定欄位之配接器的使用，是由 `data-` 屬性觸發，因此會：</span><span class="sxs-lookup"><span data-stu-id="e47a9-493">The use of an adapter for a given field is triggered by `data-` attributes that:</span></span>

* <span data-ttu-id="e47a9-494">將欄位加上旗標為待驗證 (`data-val="true"`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-494">Flag the field as being subject to validation (`data-val="true"`).</span></span>
* <span data-ttu-id="e47a9-495">識別驗證規則名稱和錯誤訊息文字 (例如 `data-val-rulename="Error message."`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-495">Identify a validation rule name and error message text (for example, `data-val-rulename="Error message."`).</span></span>
* <span data-ttu-id="e47a9-496">提供驗證程式需要的任何其他參數 (例如 `data-val-rulename-parm1="value"`)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-496">Provide any additional parameters the validator needs (for example, `data-val-rulename-parm1="value"`).</span></span>

<span data-ttu-id="e47a9-497">下列範例顯示範例應用程式之 `ClassicMovie` 屬性的 `data-` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e47a9-497">The following example shows the `data-` attributes for the sample app's `ClassicMovie` attribute:</span></span>

```html
<input class="form-control" type="datetime"
    data-val="true"
    data-val-classicmovie1="Classic movies must have a release year earlier than 1960."
    data-val-classicmovie1-year="1960"
    data-val-required="The ReleaseDate field is required."
    id="ReleaseDate" name="ReleaseDate" value="">
```

<span data-ttu-id="e47a9-498">如前文所述，[標記協助程式](xref:mvc/views/tag-helpers/intro)和 [HTML 協助程式](xref:mvc/views/overview) 使用來自驗證屬性的資訊來轉譯 `data-` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e47a9-498">As noted earlier, [Tag Helpers](xref:mvc/views/tag-helpers/intro) and [HTML helpers](xref:mvc/views/overview) use information from validation attributes to render `data-` attributes.</span></span> <span data-ttu-id="e47a9-499">有二個選項可撰寫用於建立自訂 `data-` HTML 屬性的程式碼：</span><span class="sxs-lookup"><span data-stu-id="e47a9-499">There are two options for writing code that results in the creation of custom `data-` HTML attributes:</span></span>

* <span data-ttu-id="e47a9-500">建立衍生自 `AttributeAdapterBase<TAttribute>` 的類別，以及可實作 `IValidationAttributeAdapterProvider` 的類別，並在 DI 中註冊您的屬性及其配接器。</span><span class="sxs-lookup"><span data-stu-id="e47a9-500">Create a class that derives from `AttributeAdapterBase<TAttribute>` and a class that implements `IValidationAttributeAdapterProvider`, and register your attribute and its adapter in DI.</span></span> <span data-ttu-id="e47a9-501">此方法遵循[單一職責原則](https://wikipedia.org/wiki/Single_responsibility_principle)，其中伺服器相關與用戶端相關的驗證程式碼位於不同類別中。</span><span class="sxs-lookup"><span data-stu-id="e47a9-501">This method follows the [single responsibility principal](https://wikipedia.org/wiki/Single_responsibility_principle) in that server-related and client-related validation code is in separate classes.</span></span> <span data-ttu-id="e47a9-502">配接器也具有優點，由於其在 DI 中註冊，因此可以使用 DI 中的其他服務 (如需要)。</span><span class="sxs-lookup"><span data-stu-id="e47a9-502">The adapter also has the advantage that since it is registered in DI, other services in DI are available to it if needed.</span></span>
* <span data-ttu-id="e47a9-503">在您的 `ValidationAttribute` 類別中實作 `IClientModelValidator`。</span><span class="sxs-lookup"><span data-stu-id="e47a9-503">Implement `IClientModelValidator` in your `ValidationAttribute` class.</span></span> <span data-ttu-id="e47a9-504">如果屬性不執行任何伺服器端驗證，且不需要任何來自 DI 的服務，則此方法可能適合。</span><span class="sxs-lookup"><span data-stu-id="e47a9-504">This method might be appropriate if the attribute doesn't do any server-side validation and doesn't need any services from DI.</span></span>

### <a name="attributeadapter-for-client-side-validation"></a><span data-ttu-id="e47a9-505">用戶端驗證的屬性配接器</span><span class="sxs-lookup"><span data-stu-id="e47a9-505">AttributeAdapter for client-side validation</span></span>

<span data-ttu-id="e47a9-506">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="e47a9-506">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie` attribute in the sample app.</span></span> <span data-ttu-id="e47a9-507">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-507">To add client validation by using this method:</span></span>

1. <span data-ttu-id="e47a9-508">建立自訂驗證屬性的屬性配接器類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-508">Create an attribute adapter class for the custom validation attribute.</span></span> <span data-ttu-id="e47a9-509">自 [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2) 衍生類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-509">Derive the class from [AttributeAdapterBase\<T>](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.attributeadapterbase-1?view=aspnetcore-2.2).</span></span> <span data-ttu-id="e47a9-510">建立會將 `data-` 屬性新增至轉譯輸出的 `AddValidation` 方法，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-510">Create an `AddValidation` method that adds `data-` attributes to the rendered output, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/ClassicMovieAttributeAdapter.cs?name=snippet_ClassicMovieAttributeAdapter)]

1. <span data-ttu-id="e47a9-511">建立實作 <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider> 的配接器提供者類別。</span><span class="sxs-lookup"><span data-stu-id="e47a9-511">Create an adapter provider class that implements <xref:Microsoft.AspNetCore.Mvc.DataAnnotations.IValidationAttributeAdapterProvider>.</span></span> <span data-ttu-id="e47a9-512">在 `GetAttributeAdapter` 方法中將自訂屬性傳遞至配接器的建構函式，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-512">In the `GetAttributeAdapter` method pass in the custom attribute to the adapter's constructor, as shown in this example:</span></span>

   [!code-csharp[](validation/sample/Attributes/CustomValidationAttributeAdapterProvider.cs?name=snippet_CustomValidationAttributeAdapterProvider)]

1. <span data-ttu-id="e47a9-513">在 `Startup.ConfigureServices` 中註冊 DI 的配接器提供者：</span><span class="sxs-lookup"><span data-stu-id="e47a9-513">Register the adapter provider for DI in `Startup.ConfigureServices`:</span></span>

   [!code-csharp[](validation/sample/Startup.cs?name=snippet_MaxModelValidationErrors&highlight=8-10)]

### <a name="iclientmodelvalidator-for-client-side-validation"></a><span data-ttu-id="e47a9-514">用戶端驗證的 IClientModelValidator</span><span class="sxs-lookup"><span data-stu-id="e47a9-514">IClientModelValidator for client-side validation</span></span>

<span data-ttu-id="e47a9-515">這種在 HTML 中轉譯 `data-` 屬性的方法，由範例應用程式中的 `ClassicMovie2` 屬性使用。</span><span class="sxs-lookup"><span data-stu-id="e47a9-515">This method of rendering `data-` attributes in HTML is used by the `ClassicMovie2` attribute in the sample app.</span></span> <span data-ttu-id="e47a9-516">使用此方法來新增用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-516">To add client validation by using this method:</span></span>

* <span data-ttu-id="e47a9-517">在自訂驗證屬性中，實作 `IClientModelValidator` 介面並建立 `AddValidation` 方法。</span><span class="sxs-lookup"><span data-stu-id="e47a9-517">In the custom validation attribute, implement the `IClientModelValidator` interface and create an `AddValidation` method.</span></span> <span data-ttu-id="e47a9-518">在 `AddValidation` 方法中，為驗證新增 `data-` 屬性，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="e47a9-518">In the `AddValidation` method, add `data-` attributes for validation, as shown in the following example:</span></span>

  [!code-csharp[](validation/sample/Attributes/ClassicMovie2Attribute.cs?name=snippet_ClassicMovie2Attribute)]

## <a name="disable-client-side-validation"></a><span data-ttu-id="e47a9-519">停用用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="e47a9-519">Disable client-side validation</span></span>

<span data-ttu-id="e47a9-520">下列程式碼會停用 MVC 檢視中的用戶端驗證：</span><span class="sxs-lookup"><span data-stu-id="e47a9-520">The following code disables client validation in MVC views:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup2.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="e47a9-521">以及在 Razor Pages 中：</span><span class="sxs-lookup"><span data-stu-id="e47a9-521">And in Razor Pages:</span></span>

[!code-csharp[](validation/sample_snapshot/Startup3.cs?name=snippet_DisableClientValidation)]

<span data-ttu-id="e47a9-522">停用用戶端驗證的另一個選項，是將 *.cshtml* 檔案中對 `_ValidationScriptsPartial` 的參考標記為註解。</span><span class="sxs-lookup"><span data-stu-id="e47a9-522">Another option for disabling client validation is to comment out the reference to `_ValidationScriptsPartial` in your *.cshtml* file.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e47a9-523">其他資源</span><span class="sxs-lookup"><span data-stu-id="e47a9-523">Additional resources</span></span>

* [<span data-ttu-id="e47a9-524">System.ComponentModel.DataAnnotations 命名空間</span><span class="sxs-lookup"><span data-stu-id="e47a9-524">System.ComponentModel.DataAnnotations namespace</span></span>](xref:System.ComponentModel.DataAnnotations)
* [<span data-ttu-id="e47a9-525">模型繫結</span><span class="sxs-lookup"><span data-stu-id="e47a9-525">Model Binding</span></span>](model-binding.md)

::: moniker-end
