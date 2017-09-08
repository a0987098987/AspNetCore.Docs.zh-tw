---
title: "在表單的 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "描述搭配表單的標記協助程式的內建。"
keywords: "ASP.NET Core，標記 Helper TagHelper，HTML 表單，表單"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: 25595059-4fac-4785-8152-f88590e3169b
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: bd69e008a81abc4f6785d93b89823c03e1a7df83
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="938f9-104">在表單的 ASP.NET Core 使用標記協助程式簡介</span><span class="sxs-lookup"><span data-stu-id="938f9-104">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="938f9-105">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Dave Paquette](https://twitter.com/Dave_Paquette)，和[Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="938f9-105">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="938f9-106">本文件將示範使用表單和常用在表單上的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-106">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="938f9-107">HTML[表單](https://www.w3.org/TR/html401/interact/forms.html)項目會提供張貼至伺服器的備份資料的主要機制 web 應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="938f9-107">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="938f9-108">大部分的這份文件描述[標記協助程式](tag-helpers/intro.md)及其如何協助您有效率地建立強大的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="938f9-108">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="938f9-109">我們建議您先閱讀[標記協助程式簡介](tag-helpers/intro.md)閱讀這份文件之前。</span><span class="sxs-lookup"><span data-stu-id="938f9-109">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="938f9-110">在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="938f9-110">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="938f9-111">替代的 HTML Helper 存在時，它被提及。</span><span class="sxs-lookup"><span data-stu-id="938f9-111">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name=my-asp-route-param-ref-label></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="938f9-112">表單標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-112">The Form Tag Helper</span></span>

<span data-ttu-id="938f9-113">[表單](https://www.w3.org/TR/html401/interact/forms.html)標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="938f9-113">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="938f9-114">會產生 HTML [\<表單 >](https://www.w3.org/TR/html401/interact/forms.html) `action`之 MVC 控制器動作或具名的路由的屬性值</span><span class="sxs-lookup"><span data-stu-id="938f9-114">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="938f9-115">產生隱藏[要求驗證語彙基元](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)</span><span class="sxs-lookup"><span data-stu-id="938f9-115">Generates a hidden [Request Verification Token](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="938f9-116">提供`asp-route-<Parameter Name>`屬性，其中`<Parameter Name>`新增至路由值。</span><span class="sxs-lookup"><span data-stu-id="938f9-116">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="938f9-117">`routeValues`參數`Html.BeginForm`和`Html.BeginRouteForm`提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="938f9-117">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="938f9-118">有替代的 HTML Helper`Html.BeginForm`和`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="938f9-118">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="938f9-119">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-119">Sample:</span></span>

<span data-ttu-id="938f9-120">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-120">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]</span></span>

<span data-ttu-id="938f9-121">上述表單標記協助程式會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-121">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

<span data-ttu-id="938f9-122">MVC 執行階段產生`action`屬性值的表單標記協助程式屬性`asp-controller`和`asp-action`。</span><span class="sxs-lookup"><span data-stu-id="938f9-122">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="938f9-123">表單標記協助程式也會產生隱藏[要求驗證語彙基元](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)。</span><span class="sxs-lookup"><span data-stu-id="938f9-123">The Form Tag Helper also generates a hidden [Request Verification Token](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="938f9-124">防止跨站台要求偽造的純 HTML 表單是很困難，表單標記協助程式提供此服務。</span><span class="sxs-lookup"><span data-stu-id="938f9-124">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="938f9-125">使用具名的路由</span><span class="sxs-lookup"><span data-stu-id="938f9-125">Using a named route</span></span>

<span data-ttu-id="938f9-126">`asp-route`標記協助程式屬性也可以產生標記的 html`action`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-126">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="938f9-127">應用程式，但[路由](../../fundamentals/routing.md)名為`register`可以使用 [註冊] 頁面的下列標記：</span><span class="sxs-lookup"><span data-stu-id="938f9-127">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

<span data-ttu-id="938f9-128">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-128">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]</span></span>

<span data-ttu-id="938f9-129">中的檢視表的許多*檢視/帳戶*資料夾 (當您建立新的 web 應用程式，以產生*個別使用者帳戶*) 包含[asp-路由-returnurl](http://docs.asp.net/en/latest/mvc/views/working-with-forms.html#the-form-tag-helper)屬性：</span><span class="sxs-lookup"><span data-stu-id="938f9-129">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](http://docs.asp.net/en/latest/mvc/views/working-with-forms.html#the-form-tag-helper) attribute:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "none", "highlight_args": {"hl_lines": [2]}} -->

```none
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
   ```

>[!NOTE]
><span data-ttu-id="938f9-130">內建的範本， `returnUrl` ，才會填入自動當您嘗試存取授權的資源，但不是驗證或授權。</span><span class="sxs-lookup"><span data-stu-id="938f9-130">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="938f9-131">當您嘗試未經授權的存取時，安全性的中介軟體您重新導向到的登入頁面`returnUrl`設定。</span><span class="sxs-lookup"><span data-stu-id="938f9-131">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="938f9-132">輸入的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-132">The Input Tag Helper</span></span>

<span data-ttu-id="938f9-133">輸入標記協助程式繫結 HTML [\<輸入 >](https://www.w3.org/wiki/HTML/Elements/input)模型中的運算式 razor 檢視的元素。</span><span class="sxs-lookup"><span data-stu-id="938f9-133">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="938f9-134">語法：</span><span class="sxs-lookup"><span data-stu-id="938f9-134">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
   ```

<span data-ttu-id="938f9-135">輸入的標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="938f9-135">The Input Tag Helper:</span></span>

* <span data-ttu-id="938f9-136">會產生`id`和`name`HTML 屬性中指定的運算式名稱`asp-for`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-136">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="938f9-137">`asp-for="Property1.Property2"` 相當於 `m => m.Property1.Property2`。</span><span class="sxs-lookup"><span data-stu-id="938f9-137">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="938f9-138">運算式的名稱是用途`asp-for`屬性值。</span><span class="sxs-lookup"><span data-stu-id="938f9-138">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="938f9-139">請參閱[運算式名稱](#expression-names)一節以取得其他資訊。</span><span class="sxs-lookup"><span data-stu-id="938f9-139">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="938f9-140">設定的 HTML`type`屬性值為基礎的模型型別和[資料註解](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)屬性套用至模型屬性</span><span class="sxs-lookup"><span data-stu-id="938f9-140">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes applied to the model property</span></span>

* <span data-ttu-id="938f9-141">不會覆寫 HTML`type`時指定的其中一個屬性值</span><span class="sxs-lookup"><span data-stu-id="938f9-141">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="938f9-142">會產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)驗證屬性從[資料註解](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)屬性套用至模型屬性</span><span class="sxs-lookup"><span data-stu-id="938f9-142">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes applied to model properties</span></span>

* <span data-ttu-id="938f9-143">具有重疊的 HTML Helper 功能`Html.TextBoxFor`和`Html.EditorFor`。</span><span class="sxs-lookup"><span data-stu-id="938f9-143">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="938f9-144">請參閱**輸入標記協助程式的 HTML Helper 替代**如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="938f9-144">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="938f9-145">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="938f9-145">Provides strong typing.</span></span> <span data-ttu-id="938f9-146">如果名稱屬性變更，而且您沒有更新標記協助程式會出現的錯誤如下所示：</span><span class="sxs-lookup"><span data-stu-id="938f9-146">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="938f9-147">`Input`標記協助程式設定的 HTML`type`屬性根據.NET 類型。</span><span class="sxs-lookup"><span data-stu-id="938f9-147">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="938f9-148">下表列出一些常見的.NET 型別和產生的 HTML 型別 （不是每個.NET 型別列出）。</span><span class="sxs-lookup"><span data-stu-id="938f9-148">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="938f9-149">.NET 型別</span><span class="sxs-lookup"><span data-stu-id="938f9-149">.NET type</span></span>|<span data-ttu-id="938f9-150">輸入的類型</span><span class="sxs-lookup"><span data-stu-id="938f9-150">Input Type</span></span>|
|---|---|
|<span data-ttu-id="938f9-151">Bool</span><span class="sxs-lookup"><span data-stu-id="938f9-151">Bool</span></span>|<span data-ttu-id="938f9-152">類型 ="checkbox"</span><span class="sxs-lookup"><span data-stu-id="938f9-152">type=”checkbox”</span></span>|
|<span data-ttu-id="938f9-153">字串</span><span class="sxs-lookup"><span data-stu-id="938f9-153">String</span></span>|<span data-ttu-id="938f9-154">類型 ="text"</span><span class="sxs-lookup"><span data-stu-id="938f9-154">type=”text”</span></span>|
|<span data-ttu-id="938f9-155">DateTime</span><span class="sxs-lookup"><span data-stu-id="938f9-155">DateTime</span></span>|<span data-ttu-id="938f9-156">類型 ="datetime"</span><span class="sxs-lookup"><span data-stu-id="938f9-156">type=”datetime”</span></span>|
|<span data-ttu-id="938f9-157">Byte</span><span class="sxs-lookup"><span data-stu-id="938f9-157">Byte</span></span>|<span data-ttu-id="938f9-158">類型 ="number"</span><span class="sxs-lookup"><span data-stu-id="938f9-158">type=”number”</span></span>|
|<span data-ttu-id="938f9-159">Int</span><span class="sxs-lookup"><span data-stu-id="938f9-159">Int</span></span>|<span data-ttu-id="938f9-160">類型 ="number"</span><span class="sxs-lookup"><span data-stu-id="938f9-160">type=”number”</span></span>|
|<span data-ttu-id="938f9-161">Single、Double</span><span class="sxs-lookup"><span data-stu-id="938f9-161">Single, Double</span></span>|<span data-ttu-id="938f9-162">類型 ="number"</span><span class="sxs-lookup"><span data-stu-id="938f9-162">type=”number”</span></span>|


<span data-ttu-id="938f9-163">下表顯示一些常見[資料註解](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)屬性輸入的標記協助程式將會對應至特定的輸入類型 （並非所有驗證屬性會都列出）：</span><span class="sxs-lookup"><span data-stu-id="938f9-163">The following table shows some common [data annotations](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="938f9-164">屬性</span><span class="sxs-lookup"><span data-stu-id="938f9-164">Attribute</span></span>|<span data-ttu-id="938f9-165">輸入的類型</span><span class="sxs-lookup"><span data-stu-id="938f9-165">Input Type</span></span>|
|---|---|
|<span data-ttu-id="938f9-166">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="938f9-166">[EmailAddress]</span></span>|<span data-ttu-id="938f9-167">類型 ="email"</span><span class="sxs-lookup"><span data-stu-id="938f9-167">type=”email”</span></span>|
|<span data-ttu-id="938f9-168">[Url]</span><span class="sxs-lookup"><span data-stu-id="938f9-168">[Url]</span></span>|<span data-ttu-id="938f9-169">類型 ="url"</span><span class="sxs-lookup"><span data-stu-id="938f9-169">type=”url”</span></span>|
|<span data-ttu-id="938f9-170">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="938f9-170">[HiddenInput]</span></span>|<span data-ttu-id="938f9-171">類型 ="hidden"</span><span class="sxs-lookup"><span data-stu-id="938f9-171">type=”hidden”</span></span>|
|<span data-ttu-id="938f9-172">[Phone]</span><span class="sxs-lookup"><span data-stu-id="938f9-172">[Phone]</span></span>|<span data-ttu-id="938f9-173">類型 = 「 電話 」</span><span class="sxs-lookup"><span data-stu-id="938f9-173">type=”tel”</span></span>|
|<span data-ttu-id="938f9-174">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="938f9-174">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="938f9-175">類型 = 「 密碼 」</span><span class="sxs-lookup"><span data-stu-id="938f9-175">type=”password”</span></span>|
|<span data-ttu-id="938f9-176">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="938f9-176">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="938f9-177">類型 = 「 日期 」</span><span class="sxs-lookup"><span data-stu-id="938f9-177">type=”date”</span></span>|
|<span data-ttu-id="938f9-178">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="938f9-178">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="938f9-179">類型 = 「 時間 」</span><span class="sxs-lookup"><span data-stu-id="938f9-179">type=”time”</span></span>|


<span data-ttu-id="938f9-180">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-180">Sample:</span></span>

<span data-ttu-id="938f9-181">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-181">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span></span>

<span data-ttu-id="938f9-182">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-182">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]</span></span>

<span data-ttu-id="938f9-183">上述程式碼會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-183">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid e-mail address."
              data-val-required="The Email Address field is required."
              id="Email" name="Email" value="" /> <br>
       Password:
       <input type="password" data-val="true"
              data-val-required="The Password field is required."
              id="Password" name="Password" /><br>
       <button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

<span data-ttu-id="938f9-184">套用至資料註解`Email`和`Password`屬性產生模型的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="938f9-184">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="938f9-185">輸入標記協助程式會消耗模型中繼資料，並產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`屬性 (請參閱[模型驗證](../models/validation.md))。</span><span class="sxs-lookup"><span data-stu-id="938f9-185">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="938f9-186">這些屬性描述附加至輸入欄位的驗證。</span><span class="sxs-lookup"><span data-stu-id="938f9-186">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="938f9-187">這會提供不顯眼的 HTML5 和[jQuery](https://jquery.com/)驗證。</span><span class="sxs-lookup"><span data-stu-id="938f9-187">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="938f9-188">不顯眼的屬性具有格式`data-val-rule="Error Message"`，其中的規則是驗證規則的名稱 (例如`data-val-required`， `data-val-email`，`data-val-maxlength`等。)如果屬性中提供的錯誤訊息，就會顯示的值為`data-val-rule`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-188">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="938f9-189">另外還有的表單屬性`data-val-ruleName-argumentName="argumentValue"`可提供其他詳細資料的規則，比方說， `data-val-maxlength-max="1024"` 。</span><span class="sxs-lookup"><span data-stu-id="938f9-189">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="938f9-190">輸入標記協助程式的 HTML Helper 替代項目</span><span class="sxs-lookup"><span data-stu-id="938f9-190">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="938f9-191">`Html.TextBox``Html.TextBoxFor`，`Html.Editor`和`Html.EditorFor`具有重疊的功能與輸入標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="938f9-191">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="938f9-192">輸入標記協助程式將會自動設定`type`屬性;`Html.TextBox`和`Html.TextBoxFor`則不會。</span><span class="sxs-lookup"><span data-stu-id="938f9-192">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="938f9-193">`Html.Editor`和`Html.EditorFor`處理集合、 複雜的物件和範本，並不會輸入標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="938f9-193">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="938f9-194">輸入標記協助程式`Html.EditorFor`和`Html.TextBoxFor`強型別 （使用 lambda 運算式）。`Html.TextBox`和`Html.Editor`不 （其使用的運算式名稱）。</span><span class="sxs-lookup"><span data-stu-id="938f9-194">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="938f9-195">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="938f9-195">HtmlAttributes</span></span>

<span data-ttu-id="938f9-196">`@Html.Editor()`和`@Html.EditorFor()`使用特殊`ViewDataDictionary`名為項目`htmlAttributes`時執行它們的預設範本。</span><span class="sxs-lookup"><span data-stu-id="938f9-196">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="938f9-197">此行為選擇性使用來增強`additionalViewData`參數。</span><span class="sxs-lookup"><span data-stu-id="938f9-197">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="938f9-198">「 HtmlAttributes"的索引鍵不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="938f9-198">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="938f9-199">索引鍵 」 htmlAttributes 」 類似於處理`htmlAttributes`物件傳遞到輸入像 helper `@Html.TextBox()`。</span><span class="sxs-lookup"><span data-stu-id="938f9-199">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="938f9-200">運算式名稱</span><span class="sxs-lookup"><span data-stu-id="938f9-200">Expression names</span></span>

<span data-ttu-id="938f9-201">`asp-for`屬性值是`ModelExpression`和 lambda 運算式的右邊。</span><span class="sxs-lookup"><span data-stu-id="938f9-201">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="938f9-202">因此，`asp-for="Property1"`變成`m => m.Property1`中產生的程式碼，這也是為什麼您不需要加上前置詞`Model`。</span><span class="sxs-lookup"><span data-stu-id="938f9-202">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="938f9-203">您可以使用"@"字元在內嵌運算式的開頭，並移動後，再`m.`:</span><span class="sxs-lookup"><span data-stu-id="938f9-203">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="938f9-204">會產生下列訊息：</span><span class="sxs-lookup"><span data-stu-id="938f9-204">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="938f9-205">集合屬性時，與`asp-for="CollectionProperty[23].Member"`會產生相同的名稱`asp-for="CollectionProperty[i].Member"`時`i`值`23`。</span><span class="sxs-lookup"><span data-stu-id="938f9-205">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="938f9-206">瀏覽子屬性</span><span class="sxs-lookup"><span data-stu-id="938f9-206">Navigating child properties</span></span>

<span data-ttu-id="938f9-207">您也可以瀏覽至使用檢視模型的屬性路徑的子屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-207">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="938f9-208">請考慮更複雜的模型類別，其中包含子系`Address`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-208">Consider a more complex model class that contains a child `Address` property.</span></span>

<span data-ttu-id="938f9-209">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]</span><span class="sxs-lookup"><span data-stu-id="938f9-209">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]</span></span>

<span data-ttu-id="938f9-210">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]</span><span class="sxs-lookup"><span data-stu-id="938f9-210">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]</span></span>

<span data-ttu-id="938f9-211">在檢視中，我們將繫結至`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="938f9-211">In the view, we bind to `Address.AddressLine1`:</span></span>

<span data-ttu-id="938f9-212">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="938f9-212">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]</span></span>

<span data-ttu-id="938f9-213">下列 HTML 會產生`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="938f9-213">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
   ```

### <a name="expression-names-and-collections"></a><span data-ttu-id="938f9-214">運算式名稱和集合</span><span class="sxs-lookup"><span data-stu-id="938f9-214">Expression names and Collections</span></span>

<span data-ttu-id="938f9-215">範例中，包含陣列的模型`Colors`:</span><span class="sxs-lookup"><span data-stu-id="938f9-215">Sample, a model containing an array of `Colors`:</span></span>

<span data-ttu-id="938f9-216">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]</span><span class="sxs-lookup"><span data-stu-id="938f9-216">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]</span></span>

<span data-ttu-id="938f9-217">動作方法：</span><span class="sxs-lookup"><span data-stu-id="938f9-217">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
   ```

<span data-ttu-id="938f9-218">下列的 Razor 示範如何存取特定`Color`項目：</span><span class="sxs-lookup"><span data-stu-id="938f9-218">The following Razor shows how you access a specific `Color` element:</span></span>

<span data-ttu-id="938f9-219">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-219">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]</span></span>

<span data-ttu-id="938f9-220">*Views/Shared/EditorTemplates/String.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="938f9-220">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

<span data-ttu-id="938f9-221">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-221">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]</span></span>

<span data-ttu-id="938f9-222">範例使用`List<T>`:</span><span class="sxs-lookup"><span data-stu-id="938f9-222">Sample using `List<T>`:</span></span>

<span data-ttu-id="938f9-223">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]</span><span class="sxs-lookup"><span data-stu-id="938f9-223">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]</span></span>

<span data-ttu-id="938f9-224">下列的 Razor 顯示如何反覆查看的集合：</span><span class="sxs-lookup"><span data-stu-id="938f9-224">The following Razor shows how to iterate over a collection:</span></span>

<span data-ttu-id="938f9-225">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-225">[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]</span></span>

<span data-ttu-id="938f9-226">*Views/Shared/EditorTemplates/ToDoItem.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="938f9-226">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

<span data-ttu-id="938f9-227">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-227">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]</span></span>


>[!NOTE]
><span data-ttu-id="938f9-228">一律使用`for`(和*不* `foreach`) 來逐一查看清單。</span><span class="sxs-lookup"><span data-stu-id="938f9-228">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="938f9-229">運算式評估在 LINQ 中的索引子可能會耗費資源，應該降到最低。</span><span class="sxs-lookup"><span data-stu-id="938f9-229">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="938f9-230">上述加上註解的範例程式碼會示範如何，您必須取代的 lambda 運算式`@`運算子來存取每個`ToDoItem`清單中。</span><span class="sxs-lookup"><span data-stu-id="938f9-230">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="938f9-231">Textarea 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-231">The Textarea Tag Helper</span></span>

<span data-ttu-id="938f9-232">`Textarea Tag Helper`標記協助程式是輸入標記協助程式類似。</span><span class="sxs-lookup"><span data-stu-id="938f9-232">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="938f9-233">會產生`id`和`name`屬性和資料的驗證屬性的模型從[ \<textarea >](http://www.w3.org/wiki/HTML/Elements/textarea)項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-233">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](http://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="938f9-234">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="938f9-234">Provides strong typing.</span></span>

* <span data-ttu-id="938f9-235">HTML Helper 替代程序：`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="938f9-235">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="938f9-236">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-236">Sample:</span></span>

<span data-ttu-id="938f9-237">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-237">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]</span></span>

<span data-ttu-id="938f9-238">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-238">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]</span></span>

<span data-ttu-id="938f9-239">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-239">The following HTML is generated:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6, 7, 8]}} -->

```HTML
<form method="post" action="/Demo/RegisterTextArea">
  <textarea data-val="true"
   data-val-maxlength="The field Description must be a string or array type with a maximum length of &#x27;1024&#x27;."
   data-val-maxlength-max="1024"
   data-val-minlength="The field Description must be a string or array type with a minimum length of &#x27;5&#x27;."
   data-val-minlength-min="5"
   id="Description" name="Description">
  </textarea>
  <button type="submit">Test</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-label-tag-helper"></a><span data-ttu-id="938f9-240">標籤標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-240">The Label Tag Helper</span></span>

* <span data-ttu-id="938f9-241">會產生標籤標題和`for`屬性[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)運算式名稱的項目</span><span class="sxs-lookup"><span data-stu-id="938f9-241">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="938f9-242">HTML Helper 的替代方式： `Html.LabelFor`。</span><span class="sxs-lookup"><span data-stu-id="938f9-242">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="938f9-243">`Label Tag Helper`純 HTML label 項目透過提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="938f9-243">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="938f9-244">您會自動取得的描述性標籤值`Display`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-244">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="938f9-245">預定的顯示名稱可能會隨著時間和的組合`Display`屬性和標籤標記協助程式會套用`Display`地方使用它。</span><span class="sxs-lookup"><span data-stu-id="938f9-245">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="938f9-246">更少原始程式碼中的標記</span><span class="sxs-lookup"><span data-stu-id="938f9-246">Less markup in source code</span></span>

* <span data-ttu-id="938f9-247">強型別與模型屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-247">Strong typing with the model property.</span></span>

<span data-ttu-id="938f9-248">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-248">Sample:</span></span>

<span data-ttu-id="938f9-249">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-249">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]</span></span>

<span data-ttu-id="938f9-250">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-250">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]</span></span>

<span data-ttu-id="938f9-251">下列 HTML 會產生`<label>`項目：</span><span class="sxs-lookup"><span data-stu-id="938f9-251">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
   ```

<span data-ttu-id="938f9-252">產生的標籤標記協助程式`for`與相關聯的屬性值，"Email"，這是識別碼`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-252">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="938f9-253">標記協助程式產生一致`id`和`for`以便能夠正確地建立關聯的項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-253">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="938f9-254">在此範例中的標題來自`Display`屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-254">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="938f9-255">如果模型未包含`Display`屬性標題會是運算式的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="938f9-255">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="938f9-256">驗證標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-256">The Validation Tag Helpers</span></span>

<span data-ttu-id="938f9-257">有兩個驗證標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="938f9-257">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="938f9-258">`Validation Message Tag Helper` （這會顯示單一屬性的驗證訊息上您的模型），而`Validation Summary Tag Helper`（這會顯示驗證錯誤的摘要）。</span><span class="sxs-lookup"><span data-stu-id="938f9-258">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="938f9-259">`Input Tag Helper`新增 HTML5 用戶端端輸入元素為基礎的資料模型類別上的註釋屬性的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-259">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="938f9-260">伺服器上，也會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="938f9-260">Validation is also performed on the server.</span></span> <span data-ttu-id="938f9-261">驗證標記協助程式在發生驗證錯誤時，會顯示這些錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="938f9-261">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="938f9-262">驗證訊息標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-262">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="938f9-263">新增[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)元素，其會附加在指定的模型屬性的輸入欄位的驗證錯誤訊息。  </span><span class="sxs-lookup"><span data-stu-id="938f9-263">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="938f9-264">用戶端端驗證錯誤時， [jQuery](https://jquery.com/)顯示中的錯誤訊息`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-264">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="938f9-265">驗證也會在伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="938f9-265">Validation also takes place on the server.</span></span> <span data-ttu-id="938f9-266">用戶端可能具有停用 JavaScript 和一些驗證只能在伺服器端。</span><span class="sxs-lookup"><span data-stu-id="938f9-266">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="938f9-267">HTML Helper 替代程序：`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="938f9-267">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="938f9-268">`Validation Message Tag Helper`搭配`asp-validation-for`上的 HTML 屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-268">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
   ```

<span data-ttu-id="938f9-269">驗證訊息標記協助程式將會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-269">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="938f9-270">您通常會使用`Validation Message Tag Helper`之後`Input`標記協助程式針對相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="938f9-270">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="938f9-271">這麼做會顯示附近造成錯誤的輸入任何驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="938f9-271">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="938f9-272">您必須使用正確的 JavaScript 檢視和[jQuery](https://jquery.com/)指令碼參考在用戶端驗證的位置。</span><span class="sxs-lookup"><span data-stu-id="938f9-272">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="938f9-273">請參閱[模型驗證](../models/validation.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="938f9-273">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="938f9-274">伺服器端驗證錯誤，就會發生 （例如當您自訂伺服器端驗證用戶端驗證已停用），MVC 會放在該錯誤訊息的主體為`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-274">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="938f9-275">驗證摘要的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-275">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="938f9-276">目標`<div>`的項目`asp-validation-summary`屬性</span><span class="sxs-lookup"><span data-stu-id="938f9-276">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="938f9-277">HTML Helper 替代程序：`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="938f9-277">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="938f9-278">`Validation Summary Tag Helper`用來顯示驗證訊息的摘要。</span><span class="sxs-lookup"><span data-stu-id="938f9-278">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="938f9-279">`asp-validation-summary`屬性值可以是下列任一項：</span><span class="sxs-lookup"><span data-stu-id="938f9-279">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="938f9-280">asp 驗證摘要</span><span class="sxs-lookup"><span data-stu-id="938f9-280">asp-validation-summary</span></span>|<span data-ttu-id="938f9-281">顯示驗證訊息</span><span class="sxs-lookup"><span data-stu-id="938f9-281">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="938f9-282">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="938f9-282">ValidationSummary.All</span></span>|<span data-ttu-id="938f9-283">屬性和模型層級</span><span class="sxs-lookup"><span data-stu-id="938f9-283">Property and model level</span></span>|
|<span data-ttu-id="938f9-284">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="938f9-284">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="938f9-285">型號</span><span class="sxs-lookup"><span data-stu-id="938f9-285">Model</span></span>|
|<span data-ttu-id="938f9-286">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="938f9-286">ValidationSummary.None</span></span>|<span data-ttu-id="938f9-287">無</span><span class="sxs-lookup"><span data-stu-id="938f9-287">None</span></span>|

### <a name="sample"></a><span data-ttu-id="938f9-288">範例</span><span class="sxs-lookup"><span data-stu-id="938f9-288">Sample</span></span>

<span data-ttu-id="938f9-289">在下列範例中，資料模型以裝飾`DataAnnotation`產生驗證錯誤訊息的屬性`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-289">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="938f9-290">發生驗證錯誤時，驗證標記協助程式會顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="938f9-290">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

<span data-ttu-id="938f9-291">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-291">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]</span></span>

<span data-ttu-id="938f9-292">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]</span><span class="sxs-lookup"><span data-stu-id="938f9-292">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]</span></span>

<span data-ttu-id="938f9-293">產生的 HTML （如果模型有效）：</span><span class="sxs-lookup"><span data-stu-id="938f9-293">The generated HTML (when the model is valid):</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 8, 9, 12, 13]}} -->

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid e-mail address."
   data-val="true"> <br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Email"></span><br>
  Password: <input name="Password" id="Password" type="password"
   data-val-required="The Password field is required." data-val="true"><br>
  <span class="field-validation-valid" data-valmsg-replace="true"
   data-valmsg-for="Password"></span><br>
  <button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

## <a name="the-select-tag-helper"></a><span data-ttu-id="938f9-294">選取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-294">The Select Tag Helper</span></span>

* <span data-ttu-id="938f9-295">會產生[選取](https://www.w3.org/wiki/HTML/Elements/select)以及相關聯[選項](https://www.w3.org/wiki/HTML/Elements/option)屬性的模型項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-295">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="938f9-296">有替代的 HTML Helper`Html.DropDownListFor`和`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="938f9-296">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="938f9-297">`Select Tag Helper` `asp-for`指定模型的屬性名稱，如[選取](https://www.w3.org/wiki/HTML/Elements/select)項目和`asp-items`指定[選項](https://www.w3.org/wiki/HTML/Elements/option)項目。</span><span class="sxs-lookup"><span data-stu-id="938f9-297">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="938f9-298">例如: </span><span class="sxs-lookup"><span data-stu-id="938f9-298">For example:</span></span>

<span data-ttu-id="938f9-299">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-299">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span></span>

<span data-ttu-id="938f9-300">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-300">Sample:</span></span>

<span data-ttu-id="938f9-301">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-301">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]</span></span>

<span data-ttu-id="938f9-302">`Index`方法初始化`CountryViewModel`、 設定選取的國家/地區，並將其傳遞給`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="938f9-302">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

<span data-ttu-id="938f9-303">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span><span class="sxs-lookup"><span data-stu-id="938f9-303">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span></span>

<span data-ttu-id="938f9-304">HTTP POST`Index`方法會顯示選取項目：</span><span class="sxs-lookup"><span data-stu-id="938f9-304">The HTTP POST `Index` method displays the selection:</span></span>

<span data-ttu-id="938f9-305">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]</span><span class="sxs-lookup"><span data-stu-id="938f9-305">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]</span></span>

<span data-ttu-id="938f9-306">`Index`檢視：</span><span class="sxs-lookup"><span data-stu-id="938f9-306">The `Index` view:</span></span>

<span data-ttu-id="938f9-307">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-307">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]</span></span>

<span data-ttu-id="938f9-308">如此就會產生 （具有"CA"選取) 下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-308">Which generates the following HTML (with "CA" selected):</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [2, 3, 4, 5, 6]}} -->

```HTML
<form method="post" action="/">
     <select id="Country" name="Country">
       <option value="MX">Mexico</option>
       <option selected="selected" value="CA">Canada</option>
       <option value="US">USA</option>
     </select>
       <br /><button type="submit">Register</button>
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
   </form>
   ```

> [!NOTE]
> <span data-ttu-id="938f9-309">我們不建議使用`ViewBag`或`ViewData`與選取的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="938f9-309">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="938f9-310">檢視模型是較為提供 MVC 中繼資料以及通常較不容易發生問題。</span><span class="sxs-lookup"><span data-stu-id="938f9-310">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="938f9-311">`asp-for`屬性值是特殊案例，而不需要`Model`首碼，其他標記協助程式屬性般 (例如`asp-items`)</span><span class="sxs-lookup"><span data-stu-id="938f9-311">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

<span data-ttu-id="938f9-312">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-312">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]</span></span>

### <a name="enum-binding"></a><span data-ttu-id="938f9-313">列舉繫結</span><span class="sxs-lookup"><span data-stu-id="938f9-313">Enum binding</span></span>

<span data-ttu-id="938f9-314">通常很方便使用`<select>`與`enum`屬性，並產生`SelectListItem`項目從`enum`值。</span><span class="sxs-lookup"><span data-stu-id="938f9-314">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="938f9-315">範例：</span><span class="sxs-lookup"><span data-stu-id="938f9-315">Sample:</span></span>

<span data-ttu-id="938f9-316">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]</span><span class="sxs-lookup"><span data-stu-id="938f9-316">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]</span></span>

<span data-ttu-id="938f9-317">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]</span><span class="sxs-lookup"><span data-stu-id="938f9-317">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]</span></span>

<span data-ttu-id="938f9-318">`GetEnumSelectList`方法會產生`SelectList`列舉的物件。</span><span class="sxs-lookup"><span data-stu-id="938f9-318">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

<span data-ttu-id="938f9-319">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="938f9-319">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]</span></span>

<span data-ttu-id="938f9-320">您可以裝飾的列舉值清單`Display`屬性，以取得更豐富的 UI:</span><span class="sxs-lookup"><span data-stu-id="938f9-320">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

<span data-ttu-id="938f9-321">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]</span><span class="sxs-lookup"><span data-stu-id="938f9-321">[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]</span></span>

<span data-ttu-id="938f9-322">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-322">The following HTML is generated:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [4, 5]}} -->

```HTML
  <form method="post" action="/Home/IndexEnum">
         <select data-val="true" data-val-required="The EnumCountry field is required."
                 id="EnumCountry" name="EnumCountry">
             <option value="0">United Mexican States</option>
             <option value="1">United States of America</option>
             <option value="2">Canada</option>
             <option value="3">France</option>
             <option value="4">Germany</option>
             <option selected="selected" value="5">Spain</option>
         </select>
         <br /><button type="submit">Register</button>
         <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
   ```

### <a name="option-group"></a><span data-ttu-id="938f9-323">群組選項</span><span class="sxs-lookup"><span data-stu-id="938f9-323">Option Group</span></span>

<span data-ttu-id="938f9-324">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)檢視模型包含一或多個時，會產生元素`SelectListGroup`物件。</span><span class="sxs-lookup"><span data-stu-id="938f9-324">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="938f9-325">`CountryViewModelGroup`群組`SelectListItem`"北美"和"Europe"分組的項目：</span><span class="sxs-lookup"><span data-stu-id="938f9-325">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

<span data-ttu-id="938f9-326">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]</span><span class="sxs-lookup"><span data-stu-id="938f9-326">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]</span></span>

<span data-ttu-id="938f9-327">兩個群組如下所示：</span><span class="sxs-lookup"><span data-stu-id="938f9-327">The two groups are shown below:</span></span>

![選項群組範例](working-with-forms/_static/grp.png)

<span data-ttu-id="938f9-329">產生的 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-329">The generated HTML:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3, 4, 5, 6, 7, 8, 9, 10, 11, 12]}} -->

```HTML
 <form method="post" action="/Home/IndexGroup">
      <select id="Country" name="Country">
          <optgroup label="North America">
              <option value="MEX">Mexico</option>
              <option value="CAN">Canada</option>
              <option value="US">USA</option>
          </optgroup>
          <optgroup label="Europe">
              <option value="FR">France</option>
              <option value="ES">Spain</option>
              <option value="DE">Germany</option>
          </optgroup>
      </select>
      <br /><button type="submit">Register</button>
      <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
```

### <a name="multiple-select"></a><span data-ttu-id="938f9-330">多個選取</span><span class="sxs-lookup"><span data-stu-id="938f9-330">Multiple select</span></span>

<span data-ttu-id="938f9-331">選取標記協助程式會自動產生[多個 ="多個 「](http://w3c.github.io/html-reference/select.html)屬性中指定的屬性，如果`asp-for`屬性是`IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="938f9-331">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="938f9-332">例如，假設下列模型：</span><span class="sxs-lookup"><span data-stu-id="938f9-332">For example, given the following model:</span></span>

<span data-ttu-id="938f9-333">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]</span><span class="sxs-lookup"><span data-stu-id="938f9-333">[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]</span></span>

<span data-ttu-id="938f9-334">使用下列檢視：</span><span class="sxs-lookup"><span data-stu-id="938f9-334">With the following view:</span></span>

<span data-ttu-id="938f9-335">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]</span><span class="sxs-lookup"><span data-stu-id="938f9-335">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]</span></span>

<span data-ttu-id="938f9-336">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-336">Generates the following HTML:</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [3]}} -->

```HTML
<form method="post" action="/Home/IndexMultiSelect">
    <select id="CountryCodes"
    multiple="multiple"
    name="CountryCodes"><option value="MX">Mexico</option>
<option value="CA">Canada</option>
<option value="US">USA</option>
<option value="FR">France</option>
<option value="ES">Spain</option>
<option value="DE">Germany</option>
</select>
    <br /><button type="submit">Register</button>
  <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

### <a name="no-selection"></a><span data-ttu-id="938f9-337">沒有選取項目</span><span class="sxs-lookup"><span data-stu-id="938f9-337">No selection</span></span>

<span data-ttu-id="938f9-338">如果您發現自己使用多個頁面中的 「 未指定"選項，您可以建立範本，以便消除重複的 HTML:</span><span class="sxs-lookup"><span data-stu-id="938f9-338">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

<span data-ttu-id="938f9-339">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]</span><span class="sxs-lookup"><span data-stu-id="938f9-339">[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]</span></span>

<span data-ttu-id="938f9-340">*Views/Shared/EditorTemplates/CountryViewModel.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="938f9-340">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

<span data-ttu-id="938f9-341">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-341">[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]</span></span>

<span data-ttu-id="938f9-342">將 HTML 加入[\<選項 >](https://www.w3.org/wiki/HTML/Elements/option)項目並不限於*沒有選取項目*案例。</span><span class="sxs-lookup"><span data-stu-id="938f9-342">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="938f9-343">例如，下列檢視和動作方法會產生 HTML 類似於上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="938f9-343">For example, the following view and action method will generate HTML similar to the code above:</span></span>

<span data-ttu-id="938f9-344">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span><span class="sxs-lookup"><span data-stu-id="938f9-344">[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]</span></span>

<span data-ttu-id="938f9-345">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]</span><span class="sxs-lookup"><span data-stu-id="938f9-345">[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]</span></span>

<span data-ttu-id="938f9-346">正確`<option>`將選取項目 (包含`selected="selected"`屬性) 根據目前`Country`值。</span><span class="sxs-lookup"><span data-stu-id="938f9-346">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

<!-- literal_block {"ids": [], "linenos": false, "xml:space": "preserve", "language": "HTML", "highlight_args": {"hl_lines": [5]}} -->

```HTML
 <form method="post" action="/Home/IndexEmpty">
      <select id="Country" name="Country">
          <option value="">&lt;none&gt;</option>
          <option value="MX">Mexico</option>
          <option value="CA" selected="selected">Canada</option>
          <option value="US">USA</option>
      </select>
      <br /><button type="submit">Register</button>
   <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
 </form>
 ```

## <a name="additional-resources"></a><span data-ttu-id="938f9-347">其他資源</span><span class="sxs-lookup"><span data-stu-id="938f9-347">Additional Resources</span></span>

* [<span data-ttu-id="938f9-348">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="938f9-348">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="938f9-349">HTML 表單元素</span><span class="sxs-lookup"><span data-stu-id="938f9-349">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="938f9-350">要求驗證語彙基元</span><span class="sxs-lookup"><span data-stu-id="938f9-350">Request Verification Token</span></span>](http://www.asp.net/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="938f9-351">模型繫結</span><span class="sxs-lookup"><span data-stu-id="938f9-351">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="938f9-352">模型驗證</span><span class="sxs-lookup"><span data-stu-id="938f9-352">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="938f9-353">資料註解</span><span class="sxs-lookup"><span data-stu-id="938f9-353">data annotations</span></span>](https://msdn.microsoft.com/library/system.componentmodel.dataannotations.aspx)

* <span data-ttu-id="938f9-354">[程式碼片段，這份文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)。</span><span class="sxs-lookup"><span data-stu-id="938f9-354">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
