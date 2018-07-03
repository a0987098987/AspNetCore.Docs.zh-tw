---
uid: mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
title: 執行簡單的驗證 (C#) |Microsoft Docs
author: StephenWalther
description: 了解如何在 ASP.NET MVC 應用程式中執行驗證。 在本教學課程中，Stephen Walther 會介紹您至模型狀態，並驗證 HTML 協助程式...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/02/2009
ms.topic: article
ms.assetid: 21383c9d-6aea-4bad-a99b-b5f2c9d6503f
ms.technology: dotnet-mvc
msc.legacyurl: /mvc/overview/older-versions-1/models-data/performing-simple-validation-cs
msc.type: authoredcontent
ms.openlocfilehash: 622d4bc40b2f403c0a4efe0ee799a68f1adbe4fd
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37377411"
---
<a name="performing-simple-validation-c"></a><span data-ttu-id="19179-104">執行簡單的驗證 (C#)</span><span class="sxs-lookup"><span data-stu-id="19179-104">Performing Simple Validation (C#)</span></span>
====================
<span data-ttu-id="19179-105">藉由[Stephen Walther](https://github.com/StephenWalther)</span><span class="sxs-lookup"><span data-stu-id="19179-105">by [Stephen Walther](https://github.com/StephenWalther)</span></span>

> <span data-ttu-id="19179-106">了解如何在 ASP.NET MVC 應用程式中執行驗證。</span><span class="sxs-lookup"><span data-stu-id="19179-106">Learn how to perform validation in an ASP.NET MVC application.</span></span> <span data-ttu-id="19179-107">在本教學課程中，Stephen Walther 會介紹您模型狀態，以及驗證 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="19179-107">In this tutorial, Stephen Walther introduces you to model state and the validation HTML helpers.</span></span>


<span data-ttu-id="19179-108">本教學課程的目標是要說明如何執行驗證的 ASP.NET MVC 應用程式內。</span><span class="sxs-lookup"><span data-stu-id="19179-108">The goal of this tutorial is to explain how you can perform validation within an ASP.NET MVC application.</span></span> <span data-ttu-id="19179-109">比方說，您會了解如何防止有人提交表單不包含必要的欄位的值。</span><span class="sxs-lookup"><span data-stu-id="19179-109">For example, you learn how to prevent someone from submitting a form that does not contain a value for a required field.</span></span> <span data-ttu-id="19179-110">您了解如何使用模型的狀態和驗證 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="19179-110">You learn how to use model state and the validation HTML helpers.</span></span>

## <a name="understanding-model-state"></a><span data-ttu-id="19179-111">了解模型狀態</span><span class="sxs-lookup"><span data-stu-id="19179-111">Understanding Model State</span></span>

<span data-ttu-id="19179-112">您可以使用模型狀態-或更精確地說，位於模型狀態字典中-來表示驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="19179-112">You use model state - or more accurately, the model state dictionary - to represent validation errors.</span></span> <span data-ttu-id="19179-113">比方說，列表 1 中的 create （） 動作會加入資料庫中的 Product 類別之前驗證產品類別的屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-113">For example, the Create() action in Listing 1 validates the properties of a Product class before adding the Product class to a database.</span></span>


<span data-ttu-id="19179-114">我不建議將您的資料庫或驗證邏輯加入至控制器。</span><span class="sxs-lookup"><span data-stu-id="19179-114">I'm not recommending that you add your validation or database logic to a controller.</span></span> <span data-ttu-id="19179-115">控制器應該包含應用程式流程控制相關的邏輯。</span><span class="sxs-lookup"><span data-stu-id="19179-115">A controller should contain only logic related to application flow control.</span></span> <span data-ttu-id="19179-116">我們正在採取捷徑，為了簡單起見。</span><span class="sxs-lookup"><span data-stu-id="19179-116">We are taking a shortcut to keep things simple.</span></span>


<span data-ttu-id="19179-117">**列表 1-Controllers\ProductController.cs**</span><span class="sxs-lookup"><span data-stu-id="19179-117">**Listing 1 - Controllers\ProductController.cs**</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample1.cs)]

<span data-ttu-id="19179-118">在 [列表 1] 的產品類別的名稱、 描述和 UnitsInStock 屬性會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="19179-118">In Listing 1, the Name, Description, and UnitsInStock properties of the Product class are validated.</span></span> <span data-ttu-id="19179-119">如果其中任何一個屬性的驗證測試失敗的錯誤會加入 （控制器類別的 ModelState 屬性所表示） 的模型狀態字典。</span><span class="sxs-lookup"><span data-stu-id="19179-119">If any of these properties fail a validation test then an error is added to the model state dictionary (represented by the ModelState property of the Controller class).</span></span>

<span data-ttu-id="19179-120">如果模型狀態中有任何錯誤則 ModelState.IsValid 屬性會傳回 false。</span><span class="sxs-lookup"><span data-stu-id="19179-120">If there are any errors in model state then the ModelState.IsValid property returns false.</span></span> <span data-ttu-id="19179-121">在此情況下，會重新顯示的 HTML 表單建立新的產品。</span><span class="sxs-lookup"><span data-stu-id="19179-121">In that case, the HTML form for creating a new product is redisplayed.</span></span> <span data-ttu-id="19179-122">否則，如果不有任何驗證錯誤，新的產品被加入至資料庫。</span><span class="sxs-lookup"><span data-stu-id="19179-122">Otherwise, if there are no validation errors, the new Product is added to the database.</span></span>

## <a name="using-the-validation-helpers"></a><span data-ttu-id="19179-123">使用驗證協助程式</span><span class="sxs-lookup"><span data-stu-id="19179-123">Using the Validation Helpers</span></span>

<span data-ttu-id="19179-124">ASP.NET MVC 架構包括兩個驗證協助程式： Html.ValidationMessage() helper 和 Html.ValidationSummary() 協助程式。</span><span class="sxs-lookup"><span data-stu-id="19179-124">The ASP.NET MVC framework includes two validation helpers: the Html.ValidationMessage() helper and the Html.ValidationSummary() helper.</span></span> <span data-ttu-id="19179-125">您可以在檢視中，使用這些兩個協助程式來顯示驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="19179-125">You use these two helpers in a view to display validation error messages.</span></span>

<span data-ttu-id="19179-126">Html.ValidationMessage() 和 Html.ValidationSummary() 的協助程式由 ASP.NET MVC scaffolding 自動產生的建立與編輯檢視中使用。</span><span class="sxs-lookup"><span data-stu-id="19179-126">The Html.ValidationMessage() and Html.ValidationSummary() helpers are used in the Create and Edit views that are generated automatically by the ASP.NET MVC scaffolding.</span></span> <span data-ttu-id="19179-127">請遵循下列步驟來產生建立檢視：</span><span class="sxs-lookup"><span data-stu-id="19179-127">Follow these steps to generate the Create view:</span></span>

1. <span data-ttu-id="19179-128">以滑鼠右鍵按一下 create （） 動作，在 Product 控制器，然後選取功能表選項**加入檢視**（請參閱 圖 1）。</span><span class="sxs-lookup"><span data-stu-id="19179-128">Right-click the Create() action in the Product controller and select the menu option **Add View** (see Figure 1).</span></span>
2. <span data-ttu-id="19179-129">在**加入檢視**] 對話方塊中，選取標示的核取方塊**建立強型別檢視**（請參閱 [圖 2）。</span><span class="sxs-lookup"><span data-stu-id="19179-129">In the **Add View** dialog, check the checkbox labeled **Create a strongly-typed view** (see Figure 2).</span></span>
3. <span data-ttu-id="19179-130">從**檢視資料類別**下拉式清單中選取產品類別。</span><span class="sxs-lookup"><span data-stu-id="19179-130">From the **View data class** dropdown list, select the Product class.</span></span>
4. <span data-ttu-id="19179-131">從**檢視內容**下拉式清單中，選取建立。</span><span class="sxs-lookup"><span data-stu-id="19179-131">From the **View content** dropdown list, select Create.</span></span>
5. <span data-ttu-id="19179-132">按一下 [新增] 按鈕。</span><span class="sxs-lookup"><span data-stu-id="19179-132">Click the **Add** button.</span></span>


<span data-ttu-id="19179-133">請確定您建置應用程式，再加入的檢視。</span><span class="sxs-lookup"><span data-stu-id="19179-133">Make sure that you build your application before adding a view.</span></span> <span data-ttu-id="19179-134">否則，清單中的類別不會出現在**檢視資料類別**下拉式清單中。</span><span class="sxs-lookup"><span data-stu-id="19179-134">Otherwise, the list of classes won't appear in the **View data class** dropdown list.</span></span>


<span data-ttu-id="19179-135">[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span><span class="sxs-lookup"><span data-stu-id="19179-135">[![The New Project dialog box](performing-simple-validation-cs/_static/image1.jpg)](performing-simple-validation-cs/_static/image1.png)</span></span>

<span data-ttu-id="19179-136">**圖 01**： 新增檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image2.png))</span><span class="sxs-lookup"><span data-stu-id="19179-136">**Figure 01**: Adding a view([Click to view full-size image](performing-simple-validation-cs/_static/image2.png))</span></span>


<span data-ttu-id="19179-137">[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span><span class="sxs-lookup"><span data-stu-id="19179-137">[![The New Project dialog box](performing-simple-validation-cs/_static/image2.jpg)](performing-simple-validation-cs/_static/image3.png)</span></span>

<span data-ttu-id="19179-138">**圖 02**： 建立強型別檢視 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image4.png))</span><span class="sxs-lookup"><span data-stu-id="19179-138">**Figure 02**: Creating a strongly-typed view ([Click to view full-size image](performing-simple-validation-cs/_static/image4.png))</span></span>


<span data-ttu-id="19179-139">完成這些步驟之後，您會取得列表 2 中建立檢視。</span><span class="sxs-lookup"><span data-stu-id="19179-139">After you complete these steps, you get the Create view in Listing 2.</span></span>

<span data-ttu-id="19179-140">**列表 2-Views\Product\Create.aspx**</span><span class="sxs-lookup"><span data-stu-id="19179-140">**Listing 2 - Views\Product\Create.aspx**</span></span>

[!code-aspx[Main](performing-simple-validation-cs/samples/sample2.aspx)]

<span data-ttu-id="19179-141">在列表 2 中，Html.ValidationSummary() helper 稱為立即上方的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="19179-141">In Listing 2, the Html.ValidationSummary() helper is called immediately above the HTML form.</span></span> <span data-ttu-id="19179-142">此協助程式用來顯示驗證錯誤訊息的清單。</span><span class="sxs-lookup"><span data-stu-id="19179-142">This helper is used to display a list of validation error messages.</span></span> <span data-ttu-id="19179-143">Html.ValidationSummary() helper 會呈現項目符號清單中的錯誤。</span><span class="sxs-lookup"><span data-stu-id="19179-143">The Html.ValidationSummary() helper renders the errors in a bulleted list.</span></span>

<span data-ttu-id="19179-144">Html.ValidationMessage() 協助程式會呼叫每個 HTML 表單欄位旁邊。</span><span class="sxs-lookup"><span data-stu-id="19179-144">The Html.ValidationMessage() helper is called next to each of the HTML form fields.</span></span> <span data-ttu-id="19179-145">此協助程式用來顯示錯誤訊息的表單欄位的旁邊。</span><span class="sxs-lookup"><span data-stu-id="19179-145">This helper is used to display an error message right next to a form field.</span></span> <span data-ttu-id="19179-146">在列表 2 的情況下 Html.ValidationMessage() 協助程式會顯示星號，當發生錯誤。</span><span class="sxs-lookup"><span data-stu-id="19179-146">In the case of Listing 2, the Html.ValidationMessage() helper displays an asterisk when there is an error.</span></span>

<span data-ttu-id="19179-147">[圖 3] 頁面會說明遺漏的欄位與無效的值送出表單時，驗證協助程式所呈現的錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="19179-147">The page in Figure 3 illustrates the error messages rendered by the validation helpers when the form is submitted with missing fields and invalid values.</span></span>


<span data-ttu-id="19179-148">[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span><span class="sxs-lookup"><span data-stu-id="19179-148">[![The New Project dialog box](performing-simple-validation-cs/_static/image3.jpg)](performing-simple-validation-cs/_static/image5.png)</span></span>

<span data-ttu-id="19179-149">**圖 03**: 建立檢視提交問題 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image6.png))</span><span class="sxs-lookup"><span data-stu-id="19179-149">**Figure 03**: The Create view submitted with problems ([Click to view full-size image](performing-simple-validation-cs/_static/image6.png))</span></span>


<span data-ttu-id="19179-150">請注意外觀的 HTML 輸入驗證錯誤時，也會修改欄位。</span><span class="sxs-lookup"><span data-stu-id="19179-150">Notice that the appearance of the HTML input fields are also modified when there is a validation error.</span></span> <span data-ttu-id="19179-151">Html.TextBox() helper 轉譯*類別 ="輸入的驗證錯誤的 「* Html.TextBox() 協助程式所呈現屬性相關聯的驗證錯誤時的屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-151">The Html.TextBox() helper renders a *class="input-validation-error"* attribute when there is a validation error associated with the property rendered by the Html.TextBox() helper.</span></span>

<span data-ttu-id="19179-152">有三個階層式的樣式表類別用來控制驗證錯誤的外觀：</span><span class="sxs-lookup"><span data-stu-id="19179-152">There are three cascading style sheet classes used to control the appearance of validation errors:</span></span>

- <span data-ttu-id="19179-153">輸入驗證錯誤-套用至&lt;輸入&gt;Html.TextBox() 協助程式所呈現的標記。</span><span class="sxs-lookup"><span data-stu-id="19179-153">input-validation-error - Applied to the &lt;input&gt; tag rendered by Html.TextBox() helper.</span></span>
- <span data-ttu-id="19179-154">欄位驗證錯誤-套用至&lt;跨越&gt;Html.ValidationMessage() 協助程式所呈現的標記。</span><span class="sxs-lookup"><span data-stu-id="19179-154">field-validation-error - Applied to the &lt;span&gt; tag rendered by the Html.ValidationMessage() helper.</span></span>
- <span data-ttu-id="19179-155">驗證摘要錯誤-套用至&lt;ul&gt; Html.ValidationSumamry() 協助程式所呈現的標記。</span><span class="sxs-lookup"><span data-stu-id="19179-155">validation-summary-errors - Applied to the &lt;ul&gt; tag rendered by the Html.ValidationSumamry() helper.</span></span>

<span data-ttu-id="19179-156">您可以修改這些階層式樣式表類別，並因此修改藉由修改 Site.css 檔案內容的資料夾中的 驗證錯誤的外觀。</span><span class="sxs-lookup"><span data-stu-id="19179-156">You can modify these cascading style sheet classes, and therefore modify the appearance of the validation errors, by modifying the Site.css file located in the Content folder.</span></span>

> [!NOTE] 
> 
> <span data-ttu-id="19179-157">HtmlHelper 類別包含唯讀的靜態屬性，如需擷取驗證的名稱相關的 CSS 類別。</span><span class="sxs-lookup"><span data-stu-id="19179-157">The HtmlHelper class includes read-only static properties for retrieving the names of the validation related CSS classes.</span></span> <span data-ttu-id="19179-158">這些靜態屬性會命名為 ValidationInputCssClassName、 ValidationFieldCssClassName 和 ValidationSummaryCssClassName。</span><span class="sxs-lookup"><span data-stu-id="19179-158">These static properties are named ValidationInputCssClassName, ValidationFieldCssClassName, and ValidationSummaryCssClassName.</span></span>


## <a name="prebinding-validation-and-postbinding-validation"></a><span data-ttu-id="19179-159">Prebinding 驗證和 Postbinding 驗證</span><span class="sxs-lookup"><span data-stu-id="19179-159">Prebinding Validation and Postbinding Validation</span></span>

<span data-ttu-id="19179-160">如果您送出的 HTML 表單，建立一項產品，且 [價格] 欄位和 [UnitsInStock] 欄位沒有值輸入了無效的值時，您就會得到 [圖 4] 中顯示的驗證訊息。</span><span class="sxs-lookup"><span data-stu-id="19179-160">If you submit the HTML form for creating a Product, and you enter an invalid value for the price field and no value for the UnitsInStock field, then you'll get the validation messages displayed in Figure 4.</span></span> <span data-ttu-id="19179-161">這些驗證錯誤訊息來自何處？</span><span class="sxs-lookup"><span data-stu-id="19179-161">Where do these validation error messages come from?</span></span>


<span data-ttu-id="19179-162">[![[新增專案] 對話方塊](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span><span class="sxs-lookup"><span data-stu-id="19179-162">[![The New Project dialog box](performing-simple-validation-cs/_static/image4.jpg)](performing-simple-validation-cs/_static/image7.png)</span></span>

<span data-ttu-id="19179-163">**圖 04**: Prebinding 驗證錯誤 ([按一下以檢視完整大小的影像](performing-simple-validation-cs/_static/image8.png))</span><span class="sxs-lookup"><span data-stu-id="19179-163">**Figure 04**: Prebinding Validation Errors([Click to view full-size image](performing-simple-validation-cs/_static/image8.png))</span></span>


<span data-ttu-id="19179-164">有實際上有兩種類型的驗證錯誤訊息-產生之前的 HTML 表單欄位，繫結至類別和表單欄位繫結至類別之後，所產生。</span><span class="sxs-lookup"><span data-stu-id="19179-164">There are actually two types of validation error messages - those generated before the HTML form fields are bound to a class and those generated after the form fields are bound to the class.</span></span> <span data-ttu-id="19179-165">換句話說，有 prebinding 驗證錯誤和 postbinding 驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="19179-165">In other words, there are prebinding validation errors and postbinding validation errors.</span></span>

<span data-ttu-id="19179-166">產品中的控制站列表 1 所公開的 create （） 動作接受產品類別的執行個體。</span><span class="sxs-lookup"><span data-stu-id="19179-166">The Create() action exposed by the Product controller in Listing 1 accepts an instance of the Product class.</span></span> <span data-ttu-id="19179-167">建立方法的簽章看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="19179-167">The signature of the Create method looks like this:</span></span>

[!code-csharp[Main](performing-simple-validation-cs/samples/sample3.cs)]

<span data-ttu-id="19179-168">建立表單的 HTML 表單欄位的值會繫結至 productToCreate 類別，所謂的模型繫結器。</span><span class="sxs-lookup"><span data-stu-id="19179-168">The values of the HTML form fields from the Create form are bound to the productToCreate class by something called a model binder.</span></span> <span data-ttu-id="19179-169">預設模型繫結器錯誤訊息至模型狀態會自動新增當它無法將表單欄位繫結至表單的屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-169">The default model binder adds an error message to model state automatically when it cannot bind a form field to a form property.</span></span>

<span data-ttu-id="19179-170">預設模型繫結器無法將字串"apple"繫結至 Product 類別中的 [價格] 屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-170">The default model binder cannot bind the string "apple" to the Price property of the Product class.</span></span> <span data-ttu-id="19179-171">您無法將字串指派給十進位屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-171">You can't assign a string to a decimal property.</span></span> <span data-ttu-id="19179-172">因此，模型繫結會將錯誤加入至模型狀態。</span><span class="sxs-lookup"><span data-stu-id="19179-172">Therefore, the model binder adds an error to model state.</span></span>

<span data-ttu-id="19179-173">預設模型繫結器也無法將 null 值指派給不接受 null 值的屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-173">The default model binder also cannot assign a null value to a property that does not accept nulls.</span></span> <span data-ttu-id="19179-174">特別是，模型繫結器無法將 null 值指派給 [UnitsInStock] 屬性。</span><span class="sxs-lookup"><span data-stu-id="19179-174">In particular, the model binder cannot assign a null value to the UnitsInStock property.</span></span> <span data-ttu-id="19179-175">同樣地，模型繫結會放棄，並將錯誤訊息加入至模型狀態。</span><span class="sxs-lookup"><span data-stu-id="19179-175">Once again, the model binder gives up and adds an error message to model state.</span></span>

<span data-ttu-id="19179-176">如果您想要自訂這些外觀 prebinding 錯誤訊息您需要建立這些訊息的資源字串。</span><span class="sxs-lookup"><span data-stu-id="19179-176">If you want to customize the appearance of these prebinding error messages then you need to create resource strings for these messages.</span></span>

## <a name="summary"></a><span data-ttu-id="19179-177">總結</span><span class="sxs-lookup"><span data-stu-id="19179-177">Summary</span></span>

<span data-ttu-id="19179-178">本教學課程的目標是驗證的要說明基本的 ASP.NET MVC 架構中機制。</span><span class="sxs-lookup"><span data-stu-id="19179-178">The goal of this tutorial was to describe the basic mechanics of validation in the ASP.NET MVC framework.</span></span> <span data-ttu-id="19179-179">您已了解如何使用模型狀態和驗證 HTML 協助程式。</span><span class="sxs-lookup"><span data-stu-id="19179-179">You learned how to use model state and the validation HTML helpers.</span></span> <span data-ttu-id="19179-180">我們也會討論 prebinding 和 postbinding 驗證之間的差異。</span><span class="sxs-lookup"><span data-stu-id="19179-180">We also discussed the distinction between prebinding and postbinding validation.</span></span> <span data-ttu-id="19179-181">在其他教學課程中，我們將討論各種策略可用來移動驗證程式碼出您的控制器並進入您的模型類別。</span><span class="sxs-lookup"><span data-stu-id="19179-181">In other tutorials, we'll discuss various strategies for moving your validation code out of your controllers and into your model classes.</span></span>

> [!div class="step-by-step"]
> <span data-ttu-id="19179-182">[上一頁](displaying-a-table-of-database-data-cs.md)
> [下一頁](validating-with-the-idataerrorinfo-interface-cs.md)</span><span class="sxs-lookup"><span data-stu-id="19179-182">[Previous](displaying-a-table-of-database-data-cs.md)
[Next](validating-with-the-idataerrorinfo-interface-cs.md)</span></span>
