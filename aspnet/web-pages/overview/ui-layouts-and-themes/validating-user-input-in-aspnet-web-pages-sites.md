---
uid: web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
title: "驗證使用者輸入，在 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件"
author: tfitzmac
description: "本文將討論如何驗證您從使用者取得的資訊&mdash;也就是以確定使用者輸入有效 HTML 中的資訊中 form 存新檔..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2014
ms.topic: article
ms.assetid: 4eb060cc-cf14-41ae-bab1-14a2c15332d0
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/ui-layouts-and-themes/validating-user-input-in-aspnet-web-pages-sites
msc.type: authoredcontent
ms.openlocfilehash: 3bde2a4ea69577ebcbe3e9e89a7ee07e6ece8dd1
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="validating-user-input-in-aspnet-web-pages-razor-sites"></a><span data-ttu-id="3dc3f-103">ASP.NET Web Pages (Razor) 網站中驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="3dc3f-103">Validating User Input in ASP.NET Web Pages (Razor) Sites</span></span>
====================
<span data-ttu-id="3dc3f-104">由[Tom FitzMacken](https://github.com/tfitzmac)</span><span class="sxs-lookup"><span data-stu-id="3dc3f-104">by [Tom FitzMacken](https://github.com/tfitzmac)</span></span>

> <span data-ttu-id="3dc3f-105">本文將討論如何驗證您從使用者取得的資訊&mdash;亦即，若要確定使用者輸入有效 HTML 中的資訊表單 ASP.NET Web Pages (Razor) 網站中。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-105">This article discusses how to validate information you get from users &mdash; that is, to make sure that users enter valid information in HTML forms in an ASP.NET Web Pages (Razor) site.</span></span>
> 
> <span data-ttu-id="3dc3f-106">您將學習：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-106">What you'll learn:</span></span>
> 
> - <span data-ttu-id="3dc3f-107">如何檢查使用者的輸入符合您定義的驗證準則。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-107">How to check that a user's input matches validation criteria that you define.</span></span>
> - <span data-ttu-id="3dc3f-108">如何判斷是否已通過所有驗證測試。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-108">How to determine whether all validation tests have passed.</span></span>
> - <span data-ttu-id="3dc3f-109">如何顯示驗證錯誤 （以及如何將它們格式化）。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-109">How to display validation errors (and how to format them).</span></span>
> - <span data-ttu-id="3dc3f-110">如何驗證不會直接來自使用者的資料。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-110">How to validate data that doesn't come directly from users.</span></span>
> 
> <span data-ttu-id="3dc3f-111">這些是 ASP.NET 程式設計概念，文章中：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-111">These are the ASP.NET programming concepts introduced in the article:</span></span>
> 
> - <span data-ttu-id="3dc3f-112">`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-112">The `Validation` helper.</span></span>
> - <span data-ttu-id="3dc3f-113">`Html.ValidationSummary`和`Html.ValidationMessage`方法。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-113">The `Html.ValidationSummary` and `Html.ValidationMessage` methods.</span></span>
>   
> 
> ## <a name="software-versions-used-in-the-tutorial"></a><span data-ttu-id="3dc3f-114">在本教學課程中使用的軟體版本</span><span class="sxs-lookup"><span data-stu-id="3dc3f-114">Software versions used in the tutorial</span></span>
> 
> 
> - <span data-ttu-id="3dc3f-115">ASP.NET Web Pages (Razor) 3</span><span class="sxs-lookup"><span data-stu-id="3dc3f-115">ASP.NET Web Pages (Razor) 3</span></span>
>   
> 
> <span data-ttu-id="3dc3f-116">本教學課程也適用於 ASP.NET Web Pages 2。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-116">This tutorial also works with ASP.NET Web Pages 2.</span></span>


<span data-ttu-id="3dc3f-117">本文包含下列章節：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-117">This article contains the following sections:</span></span>

- [<span data-ttu-id="3dc3f-118">使用者輸入驗證的概觀</span><span class="sxs-lookup"><span data-stu-id="3dc3f-118">Overview of User Input Validation</span></span>](#Overview_of_User_Input_Validation)
- [<span data-ttu-id="3dc3f-119">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="3dc3f-119">Validating User Input</span></span>](#Validating_User_Input)
- [<span data-ttu-id="3dc3f-120">加入用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="3dc3f-120">Adding Client-Side Validation</span></span>](#Adding_Client-Side_Validation)
- [<span data-ttu-id="3dc3f-121">格式化的驗證錯誤</span><span class="sxs-lookup"><span data-stu-id="3dc3f-121">Formatting Validation Errors</span></span>](#Formatting_Validation_Errors)
- [<span data-ttu-id="3dc3f-122">驗證不會直接來自使用者的資料</span><span class="sxs-lookup"><span data-stu-id="3dc3f-122">Validating Data That Doesn't Come Directly from Users</span></span>](#Validating_Data_That_Doesnt_Come_Directly_from_Users)

<a id="Overview_of_User_Input_Validation"></a>
## <a name="overview-of-user-input-validation"></a><span data-ttu-id="3dc3f-123">使用者輸入驗證的概觀</span><span class="sxs-lookup"><span data-stu-id="3dc3f-123">Overview of User Input Validation</span></span>

<span data-ttu-id="3dc3f-124">如果您要求使用者在頁面中輸入資訊 — 比方說，在表單，請務必確定使用者輸入的值是有效。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-124">If you ask users to enter information in a page — for example, into a form — it's important to make sure that the values that they enter are valid.</span></span> <span data-ttu-id="3dc3f-125">例如，您不想處理遺失重要資訊的表單。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-125">For example, you don't want to process a form that's missing critical information.</span></span>

<span data-ttu-id="3dc3f-126">當使用者輸入 HTML 表單的值時，這些輸入的值是字串。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-126">When users enter values into an HTML form, the values that they enter are strings.</span></span> <span data-ttu-id="3dc3f-127">在許多情況下，您需要的值會是其他資料類型，例如整數或日期。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-127">In many cases, the values you need are some other data types, like integers or dates.</span></span> <span data-ttu-id="3dc3f-128">因此，您也必須確定使用者輸入的值可以正確地轉換成適當的資料類型。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-128">Therefore, you also have to make sure that the values that users enter can be correctly converted to the appropriate data types.</span></span>

<span data-ttu-id="3dc3f-129">您也可能會有一些限制的值。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-129">You might also have certain restrictions on the values.</span></span> <span data-ttu-id="3dc3f-130">即使使用者正確輸入的整數，例如，您可能需要確定值落在特定範圍內。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-130">Even if users correctly enter an integer, for example, you might need to make sure that the value falls within a certain range.</span></span>

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image1.png)

> [!NOTE] 
> 
> <span data-ttu-id="3dc3f-132">**重要**驗證使用者輸入也是很重要的安全性。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-132">**Important** Validating user input is also important for security.</span></span> <span data-ttu-id="3dc3f-133">當您限制使用者可以在表單中輸入的值時，會減少某人可以輸入值，可能會危害您的站台安全性的機會。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-133">When you restrict the values that users can enter in forms, you reduce the chance that someone can enter a value that can compromise the security of your site.</span></span>


<a id="Validating_User_Input"></a>
## <a name="validating-user-input"></a><span data-ttu-id="3dc3f-134">驗證使用者輸入</span><span class="sxs-lookup"><span data-stu-id="3dc3f-134">Validating User Input</span></span>

<span data-ttu-id="3dc3f-135">在 ASP.NET Web Pages 2 中，您可以使用`Validator`測試使用者輸入的協助程式。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-135">In ASP.NET Web Pages 2, you can use the `Validator` helper to test user input.</span></span> <span data-ttu-id="3dc3f-136">基本的方法是執行下列作業：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-136">The basic approach is to do the following:</span></span>

1. <span data-ttu-id="3dc3f-137">判斷哪個輸入您想要驗證項目 （欄位）。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-137">Determine which input elements (fields) you want to validate.</span></span>

    <span data-ttu-id="3dc3f-138">您通常會驗證中的值`<input>`表單中的項目。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-138">You typically validate values in `<input>` elements in a form.</span></span> <span data-ttu-id="3dc3f-139">不過，最好以驗證所有輸入，即使輸入來自受條件約束的項目，例如`<select>`清單。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-139">However, it's a good practice to validate all input, even input that comes from a constrained element like a `<select>` list.</span></span> <span data-ttu-id="3dc3f-140">這有助於確保使用者不略過網頁上的控制項，並送出表單。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-140">This helps to make sure that users don't bypass the controls on a page and submit a form.</span></span>
2. <span data-ttu-id="3dc3f-141">網頁程式碼中加入個別的驗證檢查，每個輸入項目所使用的方法`Validation`協助程式。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-141">In the page code, add individual validation checks for each input element by using methods of the `Validation` helper.</span></span>

    <span data-ttu-id="3dc3f-142">若要檢查必要的欄位，請使用`Validation.RequireField(field, [error message])`（適用於個別的欄位） 或`Validation.RequireFields(field1, field2, ...))`（如欄位的清單）。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-142">To check for required fields, use `Validation.RequireField(field, [error message])` (for an individual field) or `Validation.RequireFields(field1, field2, ...))` (for a list of fields).</span></span> <span data-ttu-id="3dc3f-143">對於其他類型的驗證，使用`Validation.Add(field, ValidationType)`。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-143">For other types of validation, use `Validation.Add(field, ValidationType)`.</span></span> <span data-ttu-id="3dc3f-144">如`ValidationType`，您可以使用這些選項：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-144">For `ValidationType`, you can use these options:</span></span>

    `Validator.DateTime ([error message])`  
`Validator.Decimal([error message])`  
`Validator.EqualsTo(otherField [, error message])`  
`Validator.Float([error message])`  
`Validator.Integer([error message])`  
`Validator.Range(min, max [, error message])`  
`Validator.RegEx(pattern [, error message])`  
`Validator.Required([error message])`  
`Validator.StringLength(length)`  
`Validator.Url([error message])`
3. <span data-ttu-id="3dc3f-145">當提交頁面時，請檢查驗證是否已通過檢查`Validation.IsValid`:</span><span class="sxs-lookup"><span data-stu-id="3dc3f-145">When the page is submitted, check whether validation has passed by checking `Validation.IsValid`:</span></span>

    [!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample1.cs)]

    <span data-ttu-id="3dc3f-146">如果有任何驗證錯誤，則會略過正常網頁處理。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-146">If there are any validation errors, you skip normal page processing.</span></span> <span data-ttu-id="3dc3f-147">比方說，如果頁的目的是要更新資料庫，您沒有進行，直到所有驗證錯誤已獲得都修正。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-147">For example, if the purpose of the page is to update a database, you don't do that until all validation errors have been fixed.</span></span>
4. <span data-ttu-id="3dc3f-148">如果有驗證錯誤，在網頁標記中顯示錯誤訊息使用`Html.ValidationSummary`或`Html.ValidationMessage`，或兩者。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-148">If there are validation errors, display error messages in the page's markup by using `Html.ValidationSummary` or `Html.ValidationMessage`, or both.</span></span>

<span data-ttu-id="3dc3f-149">下列範例將說明這些步驟的頁面。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-149">The following example shows a page that illustrates these steps.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample2.cshtml)]

<span data-ttu-id="3dc3f-150">若要驗證的運作方式，請執行此頁面並故意出錯。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-150">To see how validation works, run this page and deliberately make mistakes.</span></span> <span data-ttu-id="3dc3f-151">例如，以下是頁面看起來像如果您忘記輸入課程名稱，如果您輸入，而且如果您輸入無效的日期：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-151">For example, here's what the page looks like if you forget to enter a course name, if you enter an, and if you enter an invalid date:</span></span>

![呈現的網頁中的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image2.png)

<a id="Adding_Client-Side_Validation"></a>
## <a name="adding-client-side-validation"></a><span data-ttu-id="3dc3f-153">加入用戶端驗證</span><span class="sxs-lookup"><span data-stu-id="3dc3f-153">Adding Client-Side Validation</span></span>

<span data-ttu-id="3dc3f-154">根據預設，使用者送出頁面之後，會驗證使用者輸入，也就是執行伺服器程式碼中的驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-154">By default, user input is validated after users submit the page — that is, the validation is performed in server code.</span></span> <span data-ttu-id="3dc3f-155">這種方法的缺點是使用者不知道它們所做錯誤直到後的送出此頁面。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-155">A disadvantage of this approach is that users don't know that they've made an error until after they submit the page.</span></span> <span data-ttu-id="3dc3f-156">如果表單是長或太複雜，報告錯誤，只有在送出頁面之後，才可以對使用者很不方便。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-156">If a form is long or complex, reporting errors only after the page is submitted can be inconvenient to the user.</span></span>

<span data-ttu-id="3dc3f-157">您可以新增支援，以在用戶端指令碼中執行驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-157">You can add support to perform validation in client script.</span></span> <span data-ttu-id="3dc3f-158">在此情況下，使用者在瀏覽器中，會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-158">In that case, the validation is performed as users work in the browser.</span></span> <span data-ttu-id="3dc3f-159">例如，假設您指定的值應為整數。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-159">For example, suppose you specify that a value should be an integer.</span></span> <span data-ttu-id="3dc3f-160">如果使用者輸入非整數值，只要使用者離開項目欄位被報告的錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-160">If a user enters a non-integer value, the error is reported as soon as the user leaves the entry field.</span></span> <span data-ttu-id="3dc3f-161">使用者會立即回應，方便的時候。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-161">Users get immediate feedback, which is convenient for them.</span></span> <span data-ttu-id="3dc3f-162">用戶端為基礎的驗證也可以減少使用者已送出多個錯誤，更正表單的次數。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-162">Client-based validation can also reduce the number of times that the user has to submit the form to correct multiple errors.</span></span>

> [!NOTE]
> <span data-ttu-id="3dc3f-163">即使您使用用戶端驗證時，在伺服器程式碼永遠也執行驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-163">Even if you use client-side validation, validation is always also performed in server code.</span></span> <span data-ttu-id="3dc3f-164">在伺服端程式碼中執行驗證是安全性措施，以防使用者略過用戶端為基礎的驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-164">Performing validation in server code is a security measure, in case users bypass client-based validation.</span></span>


1. <span data-ttu-id="3dc3f-165">註冊頁面中的下列 JavaScript 程式庫：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-165">Register the following JavaScript libraries in the page:</span></span>  

    [!code-html[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample3.html)]

 <span data-ttu-id="3dc3f-166">兩個程式庫是內容傳遞網路 (CDN)，從可載入，因此您不一定需要對您的電腦或伺服器。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-166">Two of the libraries are loadable from a content delivery network (CDN), so you don't necessarily have to have them on your computer or server.</span></span> <span data-ttu-id="3dc3f-167">不過，您必須擁有的本機副本*jquery.validate.unobtrusive.js*。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-167">However, you must have a local copy of *jquery.validate.unobtrusive.js*.</span></span> <span data-ttu-id="3dc3f-168">如果您沒有已正在使用 WebMatrix 範本 (例如**入門網站**)，其中包含文件庫，請建立 Web Pages 站台中，取決於**入門網站**。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-168">If you are not already working with a WebMatrix template (like **Starter Site** ) that includes the library, create a Web Pages site that's based on **Starter Site**.</span></span> <span data-ttu-id="3dc3f-169">然後將複製*.js*檔案目前的站台。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-169">Then copy the *.js* file to your current site.</span></span>
2. <span data-ttu-id="3dc3f-170">在標記中，針對您正在驗證時，每個項目加入至呼叫`Validation.For(field)`。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-170">In markup, for each element that you're validating, add a call to `Validation.For(field)`.</span></span> <span data-ttu-id="3dc3f-171">這個方法會發出用戶端驗證所使用的屬性。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-171">This method emits attributes that are used by client-side validation.</span></span> <span data-ttu-id="3dc3f-172">(而不是發出實際的 JavaScript 程式碼，方法就會發出屬性，例如`data-val-...`。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-172">(Rather than emitting actual JavaScript code, the method emits attributes like `data-val-...`.</span></span> <span data-ttu-id="3dc3f-173">這些屬性支援用來執行工作的 jQuery 不顯眼的用戶端驗證）。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-173">These attributes support unobtrusive client validation that uses jQuery to do the work.)</span></span>

<span data-ttu-id="3dc3f-174">下列頁面會顯示如何將用戶端驗證功能新增至先前範例所示。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-174">The following page shows how to add client validation features to the example shown earlier.</span></span>

[!code-cshtml[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample4.cshtml?highlight=35-39,51,61,71)]

<span data-ttu-id="3dc3f-175">並非所有用戶端上執行的驗證檢查。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-175">Not all validation checks run on the client.</span></span> <span data-ttu-id="3dc3f-176">特別是，不會在用戶端上執行 （整數、 日期和等等） 的資料類型驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-176">In particular, data-type validation (integer, date, and so on) don't run on the client.</span></span> <span data-ttu-id="3dc3f-177">在用戶端和伺服器運作下列檢查：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-177">The following checks work on both the client and server:</span></span>

- `Required`
- `Range(minValue, maxValue)`
- `StringLength(maxLength[, minLength])`
- `Regex(pattern)`
- `EqualsTo(otherField)`

<span data-ttu-id="3dc3f-178">在此範例中，用戶端程式碼不適用於測試的是有效的日期。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-178">In this example, the test for a valid date won't work in client code.</span></span> <span data-ttu-id="3dc3f-179">不過，測試會執行伺服器程式碼中。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-179">However, the test will be performed in server code.</span></span>

<a id="Formatting_Validation_Errors"></a>
## <a name="formatting-validation-errors"></a><span data-ttu-id="3dc3f-180">格式化的驗證錯誤</span><span class="sxs-lookup"><span data-stu-id="3dc3f-180">Formatting Validation Errors</span></span>

<span data-ttu-id="3dc3f-181">您可以控制如何藉由定義 CSS 類別具有下列保留的名稱會顯示驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-181">You can control how validation errors are displayed by defining CSS classes that have the following reserved names:</span></span>

- <span data-ttu-id="3dc3f-182">`field-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-182">`field-validation-error`.</span></span> <span data-ttu-id="3dc3f-183">定義的輸出`Html.ValidationMessage`方法時顯示錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-183">Defines the output of the `Html.ValidationMessage` method when it's displaying an error.</span></span>
- <span data-ttu-id="3dc3f-184">`field-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-184">`field-validation-valid`.</span></span> <span data-ttu-id="3dc3f-185">定義的輸出`Html.ValidationMessage`方法時沒有發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-185">Defines the output of the `Html.ValidationMessage` method when there is no error.</span></span>
- <span data-ttu-id="3dc3f-186">`input-validation-error`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-186">`input-validation-error`.</span></span> <span data-ttu-id="3dc3f-187">定義如何`<input>`發生錯誤時，會呈現項目。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-187">Defines how `<input>` elements are rendered when there's an error.</span></span> <span data-ttu-id="3dc3f-188">(例如，您可以使用這個類別來設定的背景色彩&lt;輸入&gt;為不同的色彩如果其值為無效的項目。)只有在用戶端驗證 （在 ASP.NET Web Pages 2) 會使用這個 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-188">(For example, you can use this class to set the background color of an &lt;input&gt; element to a different color if its value is invalid.) This CSS class is used only during client validation (in ASP.NET Web Pages 2).</span></span>
- <span data-ttu-id="3dc3f-189">`input-validation-valid`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-189">`input-validation-valid`.</span></span> <span data-ttu-id="3dc3f-190">定義的外觀`<input>`時沒有發生錯誤的項目。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-190">Defines the appearance of `<input>` elements when there is no error.</span></span>
- <span data-ttu-id="3dc3f-191">`validation-summary-errors`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-191">`validation-summary-errors`.</span></span> <span data-ttu-id="3dc3f-192">定義的輸出`Html.ValidationSummary`顯示錯誤清單的方法。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-192">Defines the output of the `Html.ValidationSummary` method it's displaying a list of errors.</span></span>
- <span data-ttu-id="3dc3f-193">`validation-summary-valid`.</span><span class="sxs-lookup"><span data-stu-id="3dc3f-193">`validation-summary-valid`.</span></span> <span data-ttu-id="3dc3f-194">定義的輸出`Html.ValidationSummary`方法時沒有發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-194">Defines the output of the `Html.ValidationSummary` method when there is no error.</span></span>

<span data-ttu-id="3dc3f-195">下列`<style>`區塊顯示錯誤條件的規則。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-195">The following `<style>` block shows rules for error conditions.</span></span>

[!code-css[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample5.css)]

<span data-ttu-id="3dc3f-196">如果您從文章中稍早範例頁面中包含這個樣式區塊，錯誤顯示看起來像下圖：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-196">If you include this style block in the example pages from earlier in the article, the error display will look like the following illustration:</span></span>

![使用 CSS 樣式類別的驗證錯誤](validating-user-input-in-aspnet-web-pages-sites/_static/image3.png)

> [!NOTE]
> <span data-ttu-id="3dc3f-198">如果您不使用用戶端驗證在 ASP.NET Web Pages 2，CSS 類別的`<input>`項目 (`input-validation-error`和`input-validation-valid`沒有任何作用。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-198">If you're not using client validation in ASP.NET Web Pages 2, the CSS classes for the `<input>` elements (`input-validation-error` and `input-validation-valid` don't have any effect.</span></span>


### <a name="static-and-dynamic-error-display"></a><span data-ttu-id="3dc3f-199">靜態和動態錯誤顯示</span><span class="sxs-lookup"><span data-stu-id="3dc3f-199">Static and Dynamic Error Display</span></span>

<span data-ttu-id="3dc3f-200">CSS 規則成對出現，例如`validation-summary-errors`和`validation-summary-valid`。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-200">The CSS rules come in pairs, such as `validation-summary-errors` and `validation-summary-valid`.</span></span> <span data-ttu-id="3dc3f-201">這些配對可讓您定義這兩個條件的規則： 錯誤狀況和 「 標準 」 （非錯誤） 條件。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-201">These pairs let you define rules for both conditions: an error condition and a "normal" (non-error) condition.</span></span> <span data-ttu-id="3dc3f-202">請務必了解，會一律會呈現錯誤顯示的標記，即使沒有任何錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-202">It's important to understand that the markup for the error display is always rendered, even if there are no errors.</span></span> <span data-ttu-id="3dc3f-203">例如，若有`Html.ValidationSummary`方法標記中的，第一次要求頁面時，即使網頁原始檔會包含下列標記：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-203">For example, if a page has an `Html.ValidationSummary` method in the markup, the page source will contain the following markup even when the page is requested for the first time:</span></span>

`<div class="validation-summary-valid" data-valmsg-summary="true"><ul></ul></div>`

<span data-ttu-id="3dc3f-204">換句話說，`Html.ValidationSummary`方法一律會呈現`<div>`項目和清單，即使錯誤清單是空的。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-204">In other words, the `Html.ValidationSummary` method always renders a `<div>` element and a list, even if the error list is empty.</span></span> <span data-ttu-id="3dc3f-205">同樣地，`Html.ValidationMessage`方法一律會呈現`<span>`項目做為個別欄位錯誤，即使沒有任何錯誤的預留位置。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-205">Similarly, the `Html.ValidationMessage` method always renders a `<span>` element as a placeholder for an individual field error, even if there is no error.</span></span>

<span data-ttu-id="3dc3f-206">在某些情況下，顯示錯誤訊息可能會造成頁面以重新排列而且可能會導致元素在頁面上四處移動。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-206">In some situations, displaying an error message can cause the page to reflow and can cause elements on the page to move around.</span></span> <span data-ttu-id="3dc3f-207">結尾的 CSS 規則`-valid`可讓您定義的版面配置，有助於防止這個問題。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-207">The CSS rules that end in `-valid` let you define a layout that can help prevent this problem.</span></span> <span data-ttu-id="3dc3f-208">例如，您可以定義`field-validation-error`和`field-validation-valid`兩者有相同固定大小。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-208">For example, you can define `field-validation-error` and `field-validation-valid` to both have the same fixed size.</span></span> <span data-ttu-id="3dc3f-209">這樣一來，欄位的顯示區域是靜態，不會變更頁面流程，如果會顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-209">That way, the display area for the field is static and won't change the page flow if an error message is displayed.</span></span>

<a id="Validating_Data_That_Doesnt_Come_Directly_from_Users"></a>
## <a name="validating-data-that-doesnt-come-directly-from-users"></a><span data-ttu-id="3dc3f-210">驗證不會直接來自使用者的資料</span><span class="sxs-lookup"><span data-stu-id="3dc3f-210">Validating Data That Doesn't Come Directly from Users</span></span>

<span data-ttu-id="3dc3f-211">有時候，您必須驗證不會直接來自 HTML 表單的資訊。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-211">Sometimes you have to validate information that doesn't come directly from an HTML form.</span></span> <span data-ttu-id="3dc3f-212">典型的範例是的頁面，其中一個值傳入查詢字串，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="3dc3f-212">A typical example is a page where a value is passed in a query string, as in the following example:</span></span>

`http://server/myapp/EditClassInformation?classid=1022`

<span data-ttu-id="3dc3f-213">您想要在此情況下，請確定傳遞至頁面的值 (這裡的值為 1022年`classid`) 有效。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-213">In this case, you want to make sure that the value that's passed to the page (here, 1022 for the value of `classid`) is valid.</span></span> <span data-ttu-id="3dc3f-214">您無法直接使用`Validation`協助程式執行這項驗證。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-214">You can't directly use the `Validation` helper to perform this validation.</span></span> <span data-ttu-id="3dc3f-215">不過，您可以使用其他功能的驗證系統，例如顯示驗證錯誤訊息的能力。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-215">However, you can use other features of the validation system, like the ability to display validation error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="3dc3f-216">**重要**一律驗證您從取得的值*任何*來源，包括表單欄位值、 查詢字串值和 cookie 值。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-216">**Important** Always validate values that you get from *any* source, including form-field values, query-string values, and cookie values.</span></span> <span data-ttu-id="3dc3f-217">很容易讓人變更這些值 （可能是適用於惡意用途）。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-217">It's easy for people to change these values (perhaps for malicious purposes).</span></span> <span data-ttu-id="3dc3f-218">因此您必須檢查這些值，以保護您的應用程式。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-218">So you must check these values in order to protect your application.</span></span>


<span data-ttu-id="3dc3f-219">下列範例會示範如何，您可能會驗證傳遞查詢字串中的值。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-219">The following example shows how you might validate a value that's passed in a query string.</span></span> <span data-ttu-id="3dc3f-220">中的程式碼測試的值不是空白，且它是整數。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-220">The code tests that the value is not empty and that it's an integer.</span></span>

[!code-csharp[Main](validating-user-input-in-aspnet-web-pages-sites/samples/sample6.cs)]

<span data-ttu-id="3dc3f-221">請注意，要求不是送出表單時，會執行測試 (`if(!IsPost)`)。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-221">Notice that the test is performed when the request is not a form submission (`if(!IsPost)`).</span></span> <span data-ttu-id="3dc3f-222">這項測試會傳送要求頁面時，第一次，但不是要求時送出表單。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-222">This test would pass the first time that the page is requested, but not when the request is a form submission.</span></span>

<span data-ttu-id="3dc3f-223">若要顯示這個錯誤，您可以將錯誤加入至驗證錯誤清單藉由呼叫`Validation.AddFormError("message")`。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-223">To display this error, you can add the error to the list of validation errors by calling `Validation.AddFormError("message")`.</span></span> <span data-ttu-id="3dc3f-224">如果頁面包含呼叫`Html.ValidationSummary`方法時，錯誤會顯示，如同使用者輸入驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="3dc3f-224">If the page contains a call to the `Html.ValidationSummary` method, the error is displayed there, just like a user-input validation error.</span></span>

<a id="AdditionalResources"></a>
## <a name="additional-resources"></a><span data-ttu-id="3dc3f-225">其他資源</span><span class="sxs-lookup"><span data-stu-id="3dc3f-225">Additional Resources</span></span>

[<span data-ttu-id="3dc3f-226">使用在 ASP.NET Web Pages 站台中的 HTML 表單</span><span class="sxs-lookup"><span data-stu-id="3dc3f-226">Working with HTML Forms in ASP.NET Web Pages Sites</span></span>](https://go.microsoft.com/fwlink/?LinkID=202892)
