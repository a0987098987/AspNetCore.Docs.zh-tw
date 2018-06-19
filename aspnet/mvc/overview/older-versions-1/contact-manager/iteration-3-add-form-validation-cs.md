---
uid: mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
title: '反覆項目 #3-加入表單驗證 (C#) |Microsoft 文件'
author: microsoft
description: 第三個反覆項目中，我們會加入基本表單驗證。 我們可以防止使用者提交表單，而不會完成必要的表單欄位。 我們也會驗證 emai...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/20/2009
ms.topic: article
ms.assetid: 51a0d175-913b-43d8-95e3-840fb96ad1a9
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/older-versions-1/contact-manager/iteration-3-add-form-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: b9353c32b2839fd760513982c5742bb8f521e94a
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2018
ms.locfileid: "30872360"
---
<a name="iteration-3--add-form-validation-c"></a><span data-ttu-id="28879-105">反覆項目 #3-加入表單驗證 (C#)</span><span class="sxs-lookup"><span data-stu-id="28879-105">Iteration #3 – Add form validation (C#)</span></span>
====================
<span data-ttu-id="28879-106">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="28879-106">by [Microsoft](https://github.com/microsoft)</span></span>

[<span data-ttu-id="28879-107">下載程式碼</span><span class="sxs-lookup"><span data-stu-id="28879-107">Download Code</span></span>](iteration-3-add-form-validation-cs/_static/contactmanager_3_cs1.zip)

> <span data-ttu-id="28879-108">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="28879-108">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="28879-109">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="28879-109">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="28879-110">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="28879-110">We also validate email addresses and phone numbers.</span></span>


## <a name="building-a-contact-management-aspnet-mvc-application-c"></a><span data-ttu-id="28879-111">建立連絡人管理 ASP.NET MVC 應用程式 (C#)</span><span class="sxs-lookup"><span data-stu-id="28879-111">Building a Contact Management ASP.NET MVC Application (C#)</span></span>
  

<span data-ttu-id="28879-112">在這一系列的教學課程中，我們會建置整個連絡人管理應用程式從開始到完成。</span><span class="sxs-lookup"><span data-stu-id="28879-112">In this series of tutorials, we build an entire Contact Management application from start to finish.</span></span> <span data-ttu-id="28879-113">請連絡系統管理員應用程式可讓您商店連絡資訊的名稱、 電話號碼和電子郵件地址-的人員清單。</span><span class="sxs-lookup"><span data-stu-id="28879-113">The Contact Manager application enables you to store contact information - names, phone numbers and email addresses - for a list of people.</span></span>

<span data-ttu-id="28879-114">我們會建置應用程式透過多個反覆項目。</span><span class="sxs-lookup"><span data-stu-id="28879-114">We build the application over multiple iterations.</span></span> <span data-ttu-id="28879-115">與每個反覆項目，我們會逐漸改善應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-115">With each iteration, we gradually improve the application.</span></span> <span data-ttu-id="28879-116">這個多個反覆項目方法的目標是要讓您了解每個變更的原因。</span><span class="sxs-lookup"><span data-stu-id="28879-116">The goal of this multiple iteration approach is to enable you to understand the reason for each change.</span></span>

- <span data-ttu-id="28879-117">反覆項目 #1-建立應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-117">Iteration #1 - Create the application.</span></span> <span data-ttu-id="28879-118">在第一次反覆運算中，我們建立連絡人管理員簡單的方式可能。</span><span class="sxs-lookup"><span data-stu-id="28879-118">In the first iteration, we create the Contact Manager in the simplest way possible.</span></span> <span data-ttu-id="28879-119">我們加入基本資料庫作業的支援： 建立、 讀取、 更新和刪除 (CRUD)。</span><span class="sxs-lookup"><span data-stu-id="28879-119">We add support for basic database operations: Create, Read, Update, and Delete (CRUD).</span></span>

- <span data-ttu-id="28879-120">反覆項目 #2-請看起來很棒的應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-120">Iteration #2 - Make the application look nice.</span></span> <span data-ttu-id="28879-121">在這個反覆項目，我們可以改進應用程式的外觀修改 ASP.NET MVC 檢視主版頁面的預設值和階層式樣式表。</span><span class="sxs-lookup"><span data-stu-id="28879-121">In this iteration, we improve the appearance of the application by modifying the default ASP.NET MVC view master page and cascading style sheet.</span></span>

- <span data-ttu-id="28879-122">反覆項目 #3-加入表單驗證。</span><span class="sxs-lookup"><span data-stu-id="28879-122">Iteration #3 - Add form validation.</span></span> <span data-ttu-id="28879-123">第三個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="28879-123">In the third iteration, we add basic form validation.</span></span> <span data-ttu-id="28879-124">我們可以防止使用者提交表單，而不會完成必要的表單欄位。</span><span class="sxs-lookup"><span data-stu-id="28879-124">We prevent people from submitting a form without completing required form fields.</span></span> <span data-ttu-id="28879-125">此外，我們也會驗證電子郵件地址和電話號碼。</span><span class="sxs-lookup"><span data-stu-id="28879-125">We also validate email addresses and phone numbers.</span></span>

- <span data-ttu-id="28879-126">反覆項目 #4-請鬆散偶合的應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-126">Iteration #4 - Make the application loosely coupled.</span></span> <span data-ttu-id="28879-127">在此第三個反覆項目中，我們利用數種軟體設計模式，讓您更輕鬆地維護及修改連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-127">In this third iteration, we take advantage of several software design patterns to make it easier to maintain and modify the Contact Manager application.</span></span> <span data-ttu-id="28879-128">例如，我們可以重構應用程式使用的儲存機制模式和相依性插入模式。</span><span class="sxs-lookup"><span data-stu-id="28879-128">For example, we refactor our application to use the Repository pattern and the Dependency Injection pattern.</span></span>

- <span data-ttu-id="28879-129">反覆項目 #5-建立單元測試。</span><span class="sxs-lookup"><span data-stu-id="28879-129">Iteration #5 - Create unit tests.</span></span> <span data-ttu-id="28879-130">第五個反覆項目中，我們在我們的應用程式更輕鬆地維護及修改加入單元測試。</span><span class="sxs-lookup"><span data-stu-id="28879-130">In the fifth iteration, we make our application easier to maintain and modify by adding unit tests.</span></span> <span data-ttu-id="28879-131">我們模擬資料模型類別，並建立單元測試，我們的控制器和驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="28879-131">We mock our data model classes and build unit tests for our controllers and validation logic.</span></span>

- <span data-ttu-id="28879-132">反覆項目 #6-使用測試為導向的開發。</span><span class="sxs-lookup"><span data-stu-id="28879-132">Iteration #6 - Use test-driven development.</span></span> <span data-ttu-id="28879-133">這個第六個反覆項目中我們將新功能加入我們的應用程式藉由撰寫單元測試的第一次，並撰寫單元測試的程式碼。</span><span class="sxs-lookup"><span data-stu-id="28879-133">In this sixth iteration, we add new functionality to our application by writing unit tests first and writing code against the unit tests.</span></span> <span data-ttu-id="28879-134">在這個反覆項目，我們會加入連絡人群組。</span><span class="sxs-lookup"><span data-stu-id="28879-134">In this iteration, we add contact groups.</span></span>

- <span data-ttu-id="28879-135">反覆項目 #7： 加入 Ajax 功能。</span><span class="sxs-lookup"><span data-stu-id="28879-135">Iteration #7 - Add Ajax functionality.</span></span> <span data-ttu-id="28879-136">在第七個反覆項目，改善回應性和效能的應用程式藉由新增 Ajax 支援。</span><span class="sxs-lookup"><span data-stu-id="28879-136">In the seventh iteration, we improve the responsiveness and performance of our application by adding support for Ajax.</span></span>


## <a name="this-iteration"></a><span data-ttu-id="28879-137">這個反覆項目</span><span class="sxs-lookup"><span data-stu-id="28879-137">This Iteration</span></span>

<span data-ttu-id="28879-138">連絡人管理員應用程式的這個第二個反覆項目中，我們會加入基本表單驗證。</span><span class="sxs-lookup"><span data-stu-id="28879-138">In this second iteration of the Contact Manager application, we add basic form validation.</span></span> <span data-ttu-id="28879-139">我們可以防止使用者提交連絡人，而不需輸入必要的表單欄位的值。</span><span class="sxs-lookup"><span data-stu-id="28879-139">We prevent people from submitting a contact without entering values for required form fields.</span></span> <span data-ttu-id="28879-140">此外，我們也會驗證電話號碼和電子郵件地址 （請參閱圖 1）。</span><span class="sxs-lookup"><span data-stu-id="28879-140">We also validate phone numbers and email addresses (see Figure 1).</span></span>


<span data-ttu-id="28879-141">[![新增專案 對話方塊](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="28879-141">[![The New Project dialog box](iteration-3-add-form-validation-cs/_static/image1.jpg)](iteration-3-add-form-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="28879-142">**圖 01**： 驗證表單 ([按一下以檢視完整大小的影像](iteration-3-add-form-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="28879-142">**Figure 01**: A form with validation ([Click to view full-size image](iteration-3-add-form-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="28879-143">在這個反覆項目，我們將驗證邏輯直接加入至控制器的動作。</span><span class="sxs-lookup"><span data-stu-id="28879-143">In this iteration, we add the validation logic directly to the controller actions.</span></span> <span data-ttu-id="28879-144">一般情況下，這不是建議用來將驗證加入至 ASP.NET MVC 應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-144">In general, this is not the recommended way to add validation to an ASP.NET MVC application.</span></span> <span data-ttu-id="28879-145">更好的方法是將應用程式的驗證邏輯放在個別[服務層](http://martinfowler.com/eaaCatalog/serviceLayer.html)。</span><span class="sxs-lookup"><span data-stu-id="28879-145">A better approach is to place an application s validation logic in a separate [service layer](http://martinfowler.com/eaaCatalog/serviceLayer.html).</span></span> <span data-ttu-id="28879-146">中的下一個反覆項目中，我們可以重構連絡人管理員應用程式，讓應用程式更容易維護。</span><span class="sxs-lookup"><span data-stu-id="28879-146">In the next iteration, we refactor the Contact Manager application to make the application more maintainable.</span></span>

<span data-ttu-id="28879-147">在這個反覆項目，為了讓事情變簡單，我們撰寫所有驗證程式碼以手動方式。</span><span class="sxs-lookup"><span data-stu-id="28879-147">In this iteration, to keep things simple, we write all of the validation code by hand.</span></span> <span data-ttu-id="28879-148">而不必自行撰寫驗證程式碼，我們無法利用驗證架構。</span><span class="sxs-lookup"><span data-stu-id="28879-148">Instead of writing the validation code ourselves, we could take advantage of a validation framework.</span></span> <span data-ttu-id="28879-149">例如，您可以使用 Microsoft 企業程式庫驗證應用程式區塊 (VAB) 來實作 ASP.NET MVC 應用程式的驗證邏輯。</span><span class="sxs-lookup"><span data-stu-id="28879-149">For example, you can use the Microsoft Enterprise Library Validation Application Block (VAB) to implement the validation logic for your ASP.NET MVC application.</span></span> <span data-ttu-id="28879-150">若要深入了解驗證應用程式區塊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="28879-150">To learn more about the Validation Application Block, see:</span></span>

[*http://msdn.microsoft.com/library/dd203099.aspx*](https://msdn.microsoft.com/library/dd203099.aspx)

## <a name="adding-validation-to-the-create-view"></a><span data-ttu-id="28879-151">將驗證加入至建立檢視</span><span class="sxs-lookup"><span data-stu-id="28879-151">Adding Validation to the Create View</span></span>

<span data-ttu-id="28879-152">可讓 s 開始將驗證邏輯加入至建立檢視。</span><span class="sxs-lookup"><span data-stu-id="28879-152">Let s start by adding validation logic to the Create view.</span></span> <span data-ttu-id="28879-153">幸運的是，由於我們產生與 Visual Studio 建立檢視時，建立檢視已經包含所有必要的使用者介面邏輯，顯示驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="28879-153">Fortunately, because we generated the Create view with Visual Studio, the Create view already contains all of the necessary user interface logic to display validation messages.</span></span> <span data-ttu-id="28879-154">建立檢視包含在程式碼範例 1。</span><span class="sxs-lookup"><span data-stu-id="28879-154">The Create view is contained in Listing 1.</span></span>

<span data-ttu-id="28879-155">**列出 1-\Views\Contact\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="28879-155">**Listing 1 - \Views\Contact\Create.aspx**</span></span>

[!code-aspx[Main](iteration-3-add-form-validation-cs/samples/sample1.aspx)]

<span data-ttu-id="28879-156">請注意，就會立即顯示 HTML 表單上方 Html.ValidationSummary() helper 方法的呼叫。</span><span class="sxs-lookup"><span data-stu-id="28879-156">Notice the call to the Html.ValidationSummary() helper method that appears immediately above the HTML form.</span></span> <span data-ttu-id="28879-157">如果有驗證錯誤訊息，這個方法會顯示驗證訊息中項目符號清單。</span><span class="sxs-lookup"><span data-stu-id="28879-157">If there are validation error messages, then this method displays validation messages in a bulleted list.</span></span>

<span data-ttu-id="28879-158">請注意，此外，每個表單欄位旁出現 Html.ValidationMessage() 呼叫。</span><span class="sxs-lookup"><span data-stu-id="28879-158">Notice, furthermore, the calls to Html.ValidationMessage() that appear next to each form field.</span></span> <span data-ttu-id="28879-159">ValidationMessage() helper 顯示個別的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="28879-159">The ValidationMessage() helper displays an individual validation error message.</span></span> <span data-ttu-id="28879-160">在程式碼範例 1，發生驗證錯誤時，會顯示一個星號。</span><span class="sxs-lookup"><span data-stu-id="28879-160">In the case of Listing 1, an asterisk is displayed when there is a validation error.</span></span>

<span data-ttu-id="28879-161">最後，Html.TextBox() helper 會自動轉譯的階層式樣式表類別協助程式 」 所顯示的屬性相關聯的驗證錯誤時。</span><span class="sxs-lookup"><span data-stu-id="28879-161">Finally, the Html.TextBox() helper automatically renders a Cascading Style Sheet class when there is a validation error associated with the property displayed by the helper.</span></span> <span data-ttu-id="28879-162">Html.TextBox() helper 呈現類別的**輸入驗證錯誤**。</span><span class="sxs-lookup"><span data-stu-id="28879-162">The Html.TextBox() helper renders a class named **input-validation-error**.</span></span>

<span data-ttu-id="28879-163">當您建立新的 ASP.NET MVC 應用程式時，名為 Site.css 樣式表會自動建立的內容資料夾中。</span><span class="sxs-lookup"><span data-stu-id="28879-163">When you create a new ASP.NET MVC application, a style sheet named Site.css is created in the Content folder automatically.</span></span> <span data-ttu-id="28879-164">這個樣式表包含驗證錯誤訊息的外觀與相關的 CSS 類別的下列定義：</span><span class="sxs-lookup"><span data-stu-id="28879-164">This style sheet contains the following definitions for CSS classes related to the appearance of validation error messages:</span></span>

[!code-css[Main](iteration-3-add-form-validation-cs/samples/sample2.css)]

<span data-ttu-id="28879-165">欄位驗證錯誤類別用於 Html.ValidationMessage() 協助程式 」 所呈現的輸出樣式。</span><span class="sxs-lookup"><span data-stu-id="28879-165">The field-validation-error class is used to style the output rendered by the Html.ValidationMessage() helper.</span></span> <span data-ttu-id="28879-166">輸入驗證錯誤類別用來設定文字方塊中 （輸入） Html.TextBox() 協助程式 」 所呈現的樣式。</span><span class="sxs-lookup"><span data-stu-id="28879-166">The input-validation-error class is used to style the textbox (input) rendered by the Html.TextBox() helper.</span></span> <span data-ttu-id="28879-167">驗證摘要錯誤類別用來設定樣式 Html.ValidationSummary() 協助程式 」 所呈現的未排序的清單。</span><span class="sxs-lookup"><span data-stu-id="28879-167">The validation-summary-errors class is used to style the unordered list rendered by the Html.ValidationSummary() helper.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="28879-168">您可以修改自訂的驗證錯誤訊息的外觀，本節所描述的樣式表類別。</span><span class="sxs-lookup"><span data-stu-id="28879-168">You can modify the style sheet classes described in this section to customize the appearance of validation error messages.</span></span>


## <a name="adding-validation-logic-to-the-create-action"></a><span data-ttu-id="28879-169">加入驗證邏輯，以建立動作</span><span class="sxs-lookup"><span data-stu-id="28879-169">Adding Validation Logic to the Create Action</span></span>

<span data-ttu-id="28879-170">現在，建立檢視永遠不會顯示驗證錯誤訊息，因為我們沒有產生任何訊息的邏輯寫入。</span><span class="sxs-lookup"><span data-stu-id="28879-170">Right now, the Create view never displays validation error messages because we have not written the logic to generate any messages.</span></span> <span data-ttu-id="28879-171">若要顯示驗證錯誤訊息，您要加入 ModelState 錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="28879-171">In order to display validation error messages, you need to add the error messages to ModelState.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="28879-172">UpdateModel() 方法將錯誤訊息加入 ModelState 自動指派給屬性的表單欄位的值發生錯誤時。</span><span class="sxs-lookup"><span data-stu-id="28879-172">The UpdateModel() method adds error messages to ModelState automatically when there is an error assigning the value of a form field to a property.</span></span> <span data-ttu-id="28879-173">例如，如果您嘗試將字串"apple"指派給 BirthDate 屬性可接受的日期時間值，然後 UpdateModel() 方法將錯誤加入至 ModelState。</span><span class="sxs-lookup"><span data-stu-id="28879-173">For example, if you attempt to assign the string "apple" to a BirthDate property that accepts DateTime values, then the UpdateModel() method adds an error to ModelState.</span></span>


<span data-ttu-id="28879-174">修改 create （） 方法中列出的 2 包含新的區段，新的連絡人插入資料庫之前會驗證連絡人類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="28879-174">The modified Create() method in Listing 2 contains a new section that validates the properties of the Contact class before the new contact is inserted into the database.</span></span>

<span data-ttu-id="28879-175">**列出 2-Controllers\ContactController.cs （建立與驗證）**</span><span class="sxs-lookup"><span data-stu-id="28879-175">**Listing 2 - Controllers\ContactController.cs (Create with validation)**</span></span>

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample3.cs)]

<span data-ttu-id="28879-176">[驗證] 區段中，會強制執行四個不同的驗證規則：</span><span class="sxs-lookup"><span data-stu-id="28879-176">The validate section enforces four distinct validation rules:</span></span>

- <span data-ttu-id="28879-177">FirstName 屬性長度必須小於或等於零 （而且它不能只包含空格）</span><span class="sxs-lookup"><span data-stu-id="28879-177">The FirstName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="28879-178">LastName 屬性長度必須小於或等於零 （而且它不能只包含空格）</span><span class="sxs-lookup"><span data-stu-id="28879-178">The LastName property must have a length greater than zero (and it cannot consist solely of spaces)</span></span>
- <span data-ttu-id="28879-179">如果電話屬性的值 （長度大於 0） 則電話屬性必須符合規則運算式。</span><span class="sxs-lookup"><span data-stu-id="28879-179">If the Phone property has a value (has a length greater than 0) then the Phone property must match a regular expression.</span></span>
- <span data-ttu-id="28879-180">如果電子郵件屬性的值 （長度大於 0） 則電子郵件屬性必須符合規則運算式。</span><span class="sxs-lookup"><span data-stu-id="28879-180">If the Email property has a value (has a length greater than 0) then the Email property must match a regular expression.</span></span>

<span data-ttu-id="28879-181">違反驗證規則時，錯誤訊息會加入到 ModelState AddModelError() 方法的說明。</span><span class="sxs-lookup"><span data-stu-id="28879-181">When there is a validation rule violation, an error message is added to ModelState with the help of the AddModelError() method.</span></span> <span data-ttu-id="28879-182">當您將訊息加入 ModelState 時，您可以提供屬性的名稱和驗證錯誤訊息文字。</span><span class="sxs-lookup"><span data-stu-id="28879-182">When you add a message to ModelState, you provide the name of a property and the text of a validation error message.</span></span> <span data-ttu-id="28879-183">這則錯誤訊息會顯示在檢視中 Html.ValidationSummary() 和 Html.ValidationMessage() helper 方法。</span><span class="sxs-lookup"><span data-stu-id="28879-183">This error message is displayed in the view by the Html.ValidationSummary() and Html.ValidationMessage() helper methods.</span></span>

<span data-ttu-id="28879-184">驗證規則會執行後，會檢查 ModelState 的 IsValid 屬性。</span><span class="sxs-lookup"><span data-stu-id="28879-184">After the validation rules are executed, the IsValid property of ModelState is checked.</span></span> <span data-ttu-id="28879-185">當任何驗證錯誤訊息已加入至 ModelState IsValid 屬性會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="28879-185">The IsValid property returns false when any validation error messages have been added to ModelState.</span></span> <span data-ttu-id="28879-186">如果驗證失敗，錯誤訊息重新顯示建立表單。</span><span class="sxs-lookup"><span data-stu-id="28879-186">If validation fails, the Create form is redisplayed with the error messages.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="28879-187">我收到驗證電話號碼和電子郵件地址的規則運算式儲存機制從規則的運算式 [*http://regexlib.com*](http://regexlib.com)</span><span class="sxs-lookup"><span data-stu-id="28879-187">I got the regular expressions for validating the phone number and email address from the regular expression repository at [*http://regexlib.com*](http://regexlib.com)</span></span>


## <a name="adding-validation-logic-to-the-edit-action"></a><span data-ttu-id="28879-188">將驗證邏輯加入至 編輯動作</span><span class="sxs-lookup"><span data-stu-id="28879-188">Adding Validation Logic to the Edit Action</span></span>

<span data-ttu-id="28879-189">Edit() 動作來更新連絡人。</span><span class="sxs-lookup"><span data-stu-id="28879-189">The Edit() action updates a Contact.</span></span> <span data-ttu-id="28879-190">Edit() 動作需要執行 create （） 動作完全相同的驗證。</span><span class="sxs-lookup"><span data-stu-id="28879-190">The Edit() action needs to perform exactly the same validation as the Create() action.</span></span> <span data-ttu-id="28879-191">而不必重複相同的驗證程式碼，我們應該重構連絡控制器，以便在 create （） 和 Edit() 動作呼叫相同的驗證方法。</span><span class="sxs-lookup"><span data-stu-id="28879-191">Instead of duplicating the same validation code, we should refactor the Contact controller so that both the Create() and Edit() actions call the same validation method.</span></span>

<span data-ttu-id="28879-192">修改過的連絡人控制器類別包含在列出的 3。</span><span class="sxs-lookup"><span data-stu-id="28879-192">The modified Contact controller class is contained in Listing 3.</span></span> <span data-ttu-id="28879-193">這個類別具有新 ValidateContact() 方法呼叫內的 create （） 」 和 「 Edit() 動作。</span><span class="sxs-lookup"><span data-stu-id="28879-193">This class has a new ValidateContact() method that is called within both the Create() and Edit() actions.</span></span>

<span data-ttu-id="28879-194">**Listing 3 - Controllers\ContactController.cs**</span><span class="sxs-lookup"><span data-stu-id="28879-194">**Listing 3 - Controllers\ContactController.cs**</span></span>

[!code-csharp[Main](iteration-3-add-form-validation-cs/samples/sample4.cs)]

## <a name="summary"></a><span data-ttu-id="28879-195">總結</span><span class="sxs-lookup"><span data-stu-id="28879-195">Summary</span></span>

<span data-ttu-id="28879-196">在這個反覆項目，我們加入的基本表單驗證連絡人管理員應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-196">In this iteration, we added basic form validation to our Contact Manager application.</span></span> <span data-ttu-id="28879-197">我們將驗證邏輯可防止使用者提交新的連絡人或編輯現有連絡人，而不需要提供 FirstName 和 LastName 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="28879-197">Our validation logic prevents users from submitting a new contact or editing an existing contact without supplying values for the FirstName and LastName properties.</span></span> <span data-ttu-id="28879-198">此外，使用者必須提供有效的電話號碼和電子郵件地址。</span><span class="sxs-lookup"><span data-stu-id="28879-198">Furthermore, users must supply valid phone numbers and email addresses.</span></span>

<span data-ttu-id="28879-199">在這個反覆項目，我們加入驗證邏輯為連絡人管理員應用程式中最簡單的方式。</span><span class="sxs-lookup"><span data-stu-id="28879-199">In this iteration, we added the validation logic to our Contact Manager application in the easiest way possible.</span></span> <span data-ttu-id="28879-200">不過，我們控制器邏輯至混合我們驗證邏輯會產生問題為我們長期來看。</span><span class="sxs-lookup"><span data-stu-id="28879-200">However, mixing our validation logic into our controller logic will create problems for us in the long term.</span></span> <span data-ttu-id="28879-201">我們的應用程式將會更難維護及修改一段時間。</span><span class="sxs-lookup"><span data-stu-id="28879-201">Our application will be more difficult to maintain and modify over time.</span></span>

<span data-ttu-id="28879-202">中的下一個反覆項目中，我們將我們控制器重構我們的驗證邏輯和資料庫存取邏輯。</span><span class="sxs-lookup"><span data-stu-id="28879-202">In the next iteration, we will refactor our validation logic and database access logic out of our controllers.</span></span> <span data-ttu-id="28879-203">我們將利用數種軟體設計原則，讓我們來建立更多彈性，且更易於維護，應用程式。</span><span class="sxs-lookup"><span data-stu-id="28879-203">We'll take advantage of several software design principles to enable us to create a more loosely coupled, and more maintainable, application.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="28879-204">[上一頁](iteration-2-make-the-application-look-nice-cs.md)
> [下一頁](iteration-4-make-the-application-loosely-coupled-cs.md)</span><span class="sxs-lookup"><span data-stu-id="28879-204">[Previous](iteration-2-make-the-application-look-nice-cs.md)
[Next](iteration-4-make-the-application-loosely-coupled-cs.md)</span></span>
