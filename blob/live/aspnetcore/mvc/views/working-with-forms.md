---
title: "在表單的 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "描述搭配表單的標記協助程式的內建。"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/working-with-forms
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 9fbe2c5cb495aabee0e1f0bdb3871641efa03599
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="introduction-to-using-tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="a91de-103">在表單的 ASP.NET Core 使用標記協助程式簡介</span><span class="sxs-lookup"><span data-stu-id="a91de-103">Introduction to using tag helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="a91de-104">由[Rick Anderson](https://twitter.com/RickAndMSFT)， [Dave Paquette](https://twitter.com/Dave_Paquette)，和[Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="a91de-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="a91de-105">本文件將示範使用表單和常用在表單上的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="a91de-106">HTML[表單](https://www.w3.org/TR/html401/interact/forms.html)項目會提供張貼至伺服器的備份資料的主要機制 web 應用程式使用。</span><span class="sxs-lookup"><span data-stu-id="a91de-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="a91de-107">大部分的這份文件描述[標記協助程式](tag-helpers/intro.md)及其如何協助您有效率地建立強大的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="a91de-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="a91de-108">我們建議您先閱讀[標記協助程式簡介](tag-helpers/intro.md)閱讀這份文件之前。</span><span class="sxs-lookup"><span data-stu-id="a91de-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="a91de-109">在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="a91de-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers do not replace HTML Helpers and there is not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="a91de-110">替代的 HTML Helper 存在時，它被提及。</span><span class="sxs-lookup"><span data-stu-id="a91de-110">When an HTML Helper alternative exists, it is mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="a91de-111">表單標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-111">The Form Tag Helper</span></span>

<span data-ttu-id="a91de-112">[表單](https://www.w3.org/TR/html401/interact/forms.html)標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="a91de-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="a91de-113">會產生 HTML [\<表單 >](https://www.w3.org/TR/html401/interact/forms.html) `action`之 MVC 控制器動作或具名的路由的屬性值</span><span class="sxs-lookup"><span data-stu-id="a91de-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="a91de-114">產生隱藏[要求驗證語彙基元](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)</span><span class="sxs-lookup"><span data-stu-id="a91de-114">Generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="a91de-115">提供`asp-route-<Parameter Name>`屬性，其中`<Parameter Name>`新增至路由值。</span><span class="sxs-lookup"><span data-stu-id="a91de-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="a91de-116">`routeValues`參數`Html.BeginForm`和`Html.BeginRouteForm`提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="a91de-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="a91de-117">有替代的 HTML Helper`Html.BeginForm`和`Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="a91de-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="a91de-118">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-118">Sample:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="a91de-119">上述表單標記協助程式會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
     <!-- Input and Submit elements -->
     <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
    </form>
```

<span data-ttu-id="a91de-120">MVC 執行階段產生`action`屬性值的表單標記協助程式屬性`asp-controller`和`asp-action`。</span><span class="sxs-lookup"><span data-stu-id="a91de-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="a91de-121">表單標記協助程式也會產生隱藏[要求驗證語彙基元](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)以防止跨站台要求偽造 (搭配使用時`[ValidateAntiForgeryToken]`HTTP Post 動作方法中的屬性)。</span><span class="sxs-lookup"><span data-stu-id="a91de-121">The Form Tag Helper also generates a hidden [Request Verification Token](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="a91de-122">防止跨站台要求偽造的純 HTML 表單是很困難，表單標記協助程式提供此服務。</span><span class="sxs-lookup"><span data-stu-id="a91de-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="a91de-123">使用具名的路由</span><span class="sxs-lookup"><span data-stu-id="a91de-123">Using a named route</span></span>

<span data-ttu-id="a91de-124">`asp-route`標記協助程式屬性也可以產生標記的 html`action`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="a91de-125">應用程式，但[路由](../../fundamentals/routing.md)名為`register`可以使用 [註冊] 頁面的下列標記：</span><span class="sxs-lookup"><span data-stu-id="a91de-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="a91de-126">中的檢視表的許多*檢視/帳戶*資料夾 (當您建立新的 web 應用程式，以產生*個別使用者帳戶*) 包含[asp-路由-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms)屬性：</span><span class="sxs-lookup"><span data-stu-id="a91de-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](https://docs.microsoft.com/aspnet/core/mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="a91de-127">內建的範本， `returnUrl` ，才會填入自動當您嘗試存取授權的資源，但不是驗證或授權。</span><span class="sxs-lookup"><span data-stu-id="a91de-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="a91de-128">當您嘗試未經授權的存取時，安全性的中介軟體您重新導向到的登入頁面`returnUrl`設定。</span><span class="sxs-lookup"><span data-stu-id="a91de-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-input-tag-helper"></a><span data-ttu-id="a91de-129">輸入的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-129">The Input Tag Helper</span></span>

<span data-ttu-id="a91de-130">輸入標記協助程式繫結 HTML [\<輸入 >](https://www.w3.org/wiki/HTML/Elements/input)模型中的運算式 razor 檢視的元素。</span><span class="sxs-lookup"><span data-stu-id="a91de-130">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="a91de-131">語法：</span><span class="sxs-lookup"><span data-stu-id="a91de-131">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="a91de-132">輸入的標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="a91de-132">The Input Tag Helper:</span></span>

* <span data-ttu-id="a91de-133">會產生`id`和`name`HTML 屬性中指定的運算式名稱`asp-for`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-133">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="a91de-134">`asp-for="Property1.Property2"` 相當於 `m => m.Property1.Property2`。</span><span class="sxs-lookup"><span data-stu-id="a91de-134">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="a91de-135">運算式的名稱是用途`asp-for`屬性值。</span><span class="sxs-lookup"><span data-stu-id="a91de-135">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="a91de-136">請參閱[運算式名稱](#expression-names)一節以取得其他資訊。</span><span class="sxs-lookup"><span data-stu-id="a91de-136">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="a91de-137">設定的 HTML`type`屬性值為基礎的模型型別和[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性套用至模型屬性</span><span class="sxs-lookup"><span data-stu-id="a91de-137">Sets the HTML `type` attribute value based on the model type and  [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="a91de-138">不會覆寫 HTML`type`時指定的其中一個屬性值</span><span class="sxs-lookup"><span data-stu-id="a91de-138">Will not overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="a91de-139">會產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)驗證屬性從[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性套用至模型屬性</span><span class="sxs-lookup"><span data-stu-id="a91de-139">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="a91de-140">具有重疊的 HTML Helper 功能`Html.TextBoxFor`和`Html.EditorFor`。</span><span class="sxs-lookup"><span data-stu-id="a91de-140">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="a91de-141">請參閱**輸入標記協助程式的 HTML Helper 替代**如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a91de-141">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="a91de-142">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="a91de-142">Provides strong typing.</span></span> <span data-ttu-id="a91de-143">如果名稱屬性變更，而且您沒有更新標記協助程式會出現的錯誤如下所示：</span><span class="sxs-lookup"><span data-stu-id="a91de-143">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="a91de-144">`Input`標記協助程式設定的 HTML`type`屬性根據.NET 類型。</span><span class="sxs-lookup"><span data-stu-id="a91de-144">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="a91de-145">下表列出一些常見的.NET 型別和產生的 HTML 型別 （不是每個.NET 型別列出）。</span><span class="sxs-lookup"><span data-stu-id="a91de-145">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="a91de-146">.NET 型別</span><span class="sxs-lookup"><span data-stu-id="a91de-146">.NET type</span></span>|<span data-ttu-id="a91de-147">輸入的類型</span><span class="sxs-lookup"><span data-stu-id="a91de-147">Input Type</span></span>|
|---|---|
|<span data-ttu-id="a91de-148">Bool</span><span class="sxs-lookup"><span data-stu-id="a91de-148">Bool</span></span>|<span data-ttu-id="a91de-149">type=”checkbox”</span><span class="sxs-lookup"><span data-stu-id="a91de-149">type=”checkbox”</span></span>|
|<span data-ttu-id="a91de-150">String</span><span class="sxs-lookup"><span data-stu-id="a91de-150">String</span></span>|<span data-ttu-id="a91de-151">type=”text”</span><span class="sxs-lookup"><span data-stu-id="a91de-151">type=”text”</span></span>|
|<span data-ttu-id="a91de-152">DateTime</span><span class="sxs-lookup"><span data-stu-id="a91de-152">DateTime</span></span>|<span data-ttu-id="a91de-153">type=”datetime”</span><span class="sxs-lookup"><span data-stu-id="a91de-153">type=”datetime”</span></span>|
|<span data-ttu-id="a91de-154">Byte</span><span class="sxs-lookup"><span data-stu-id="a91de-154">Byte</span></span>|<span data-ttu-id="a91de-155">type=”number”</span><span class="sxs-lookup"><span data-stu-id="a91de-155">type=”number”</span></span>|
|<span data-ttu-id="a91de-156">Int</span><span class="sxs-lookup"><span data-stu-id="a91de-156">Int</span></span>|<span data-ttu-id="a91de-157">type=”number”</span><span class="sxs-lookup"><span data-stu-id="a91de-157">type=”number”</span></span>|
|<span data-ttu-id="a91de-158">Single、Double</span><span class="sxs-lookup"><span data-stu-id="a91de-158">Single, Double</span></span>|<span data-ttu-id="a91de-159">type=”number”</span><span class="sxs-lookup"><span data-stu-id="a91de-159">type=”number”</span></span>|


<span data-ttu-id="a91de-160">下表顯示一些常見[資料註解](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性輸入的標記協助程式將會對應至特定的輸入類型 （並非所有驗證屬性會都列出）：</span><span class="sxs-lookup"><span data-stu-id="a91de-160">The following table shows some common [data annotations](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="a91de-161">屬性</span><span class="sxs-lookup"><span data-stu-id="a91de-161">Attribute</span></span>|<span data-ttu-id="a91de-162">輸入的類型</span><span class="sxs-lookup"><span data-stu-id="a91de-162">Input Type</span></span>|
|---|---|
|<span data-ttu-id="a91de-163">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="a91de-163">[EmailAddress]</span></span>|<span data-ttu-id="a91de-164">type=”email”</span><span class="sxs-lookup"><span data-stu-id="a91de-164">type=”email”</span></span>|
|<span data-ttu-id="a91de-165">[Url]</span><span class="sxs-lookup"><span data-stu-id="a91de-165">[Url]</span></span>|<span data-ttu-id="a91de-166">type=”url”</span><span class="sxs-lookup"><span data-stu-id="a91de-166">type=”url”</span></span>|
|<span data-ttu-id="a91de-167">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="a91de-167">[HiddenInput]</span></span>|<span data-ttu-id="a91de-168">type=”hidden”</span><span class="sxs-lookup"><span data-stu-id="a91de-168">type=”hidden”</span></span>|
|<span data-ttu-id="a91de-169">[Phone]</span><span class="sxs-lookup"><span data-stu-id="a91de-169">[Phone]</span></span>|<span data-ttu-id="a91de-170">type=”tel”</span><span class="sxs-lookup"><span data-stu-id="a91de-170">type=”tel”</span></span>|
|<span data-ttu-id="a91de-171">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="a91de-171">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="a91de-172">type=”password”</span><span class="sxs-lookup"><span data-stu-id="a91de-172">type=”password”</span></span>|
|<span data-ttu-id="a91de-173">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="a91de-173">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="a91de-174">type=”date”</span><span class="sxs-lookup"><span data-stu-id="a91de-174">type=”date”</span></span>|
|<span data-ttu-id="a91de-175">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="a91de-175">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="a91de-176">type=”time”</span><span class="sxs-lookup"><span data-stu-id="a91de-176">type=”time”</span></span>|


<span data-ttu-id="a91de-177">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-177">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="a91de-178">上述程式碼會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-178">The code above generates the following HTML:</span></span>

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

<span data-ttu-id="a91de-179">套用至資料註解`Email`和`Password`屬性產生模型的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="a91de-179">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="a91de-180">輸入標記協助程式會消耗模型中繼資料，並產生[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*`屬性 (請參閱[模型驗證](../models/validation.md))。</span><span class="sxs-lookup"><span data-stu-id="a91de-180">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="a91de-181">這些屬性描述附加至輸入欄位的驗證。</span><span class="sxs-lookup"><span data-stu-id="a91de-181">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="a91de-182">這會提供不顯眼的 HTML5 和[jQuery](https://jquery.com/)驗證。</span><span class="sxs-lookup"><span data-stu-id="a91de-182">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="a91de-183">不顯眼的屬性具有格式`data-val-rule="Error Message"`，其中的規則是驗證規則的名稱 (例如`data-val-required`， `data-val-email`，`data-val-maxlength`等。)如果屬性中提供的錯誤訊息，就會顯示的值為`data-val-rule`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-183">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it is displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="a91de-184">另外還有的表單屬性`data-val-ruleName-argumentName="argumentValue"`可提供其他詳細資料的規則，比方說， `data-val-maxlength-max="1024"` 。</span><span class="sxs-lookup"><span data-stu-id="a91de-184">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="a91de-185">輸入標記協助程式的 HTML Helper 替代項目</span><span class="sxs-lookup"><span data-stu-id="a91de-185">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="a91de-186">`Html.TextBox``Html.TextBoxFor`，`Html.Editor`和`Html.EditorFor`具有重疊的功能與輸入標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="a91de-186">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="a91de-187">輸入標記協助程式將會自動設定`type`屬性;`Html.TextBox`和`Html.TextBoxFor`則不會。</span><span class="sxs-lookup"><span data-stu-id="a91de-187">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` will not.</span></span> <span data-ttu-id="a91de-188">`Html.Editor`和`Html.EditorFor`處理集合、 複雜的物件和範本，並不會輸入標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="a91de-188">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper does not.</span></span> <span data-ttu-id="a91de-189">輸入標記協助程式`Html.EditorFor`和`Html.TextBoxFor`強型別 （使用 lambda 運算式）。`Html.TextBox`和`Html.Editor`不 （其使用的運算式名稱）。</span><span class="sxs-lookup"><span data-stu-id="a91de-189">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="a91de-190">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="a91de-190">HtmlAttributes</span></span>

<span data-ttu-id="a91de-191">`@Html.Editor()`和`@Html.EditorFor()`使用特殊`ViewDataDictionary`名為項目`htmlAttributes`時執行它們的預設範本。</span><span class="sxs-lookup"><span data-stu-id="a91de-191">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="a91de-192">此行為選擇性使用來增強`additionalViewData`參數。</span><span class="sxs-lookup"><span data-stu-id="a91de-192">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="a91de-193">「 HtmlAttributes"的索引鍵不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="a91de-193">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="a91de-194">索引鍵 」 htmlAttributes 」 類似於處理`htmlAttributes`物件傳遞到輸入像 helper `@Html.TextBox()`。</span><span class="sxs-lookup"><span data-stu-id="a91de-194">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="a91de-195">運算式名稱</span><span class="sxs-lookup"><span data-stu-id="a91de-195">Expression names</span></span>

<span data-ttu-id="a91de-196">`asp-for`屬性值是`ModelExpression`和 lambda 運算式的右邊。</span><span class="sxs-lookup"><span data-stu-id="a91de-196">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="a91de-197">因此，`asp-for="Property1"`變成`m => m.Property1`中產生的程式碼，這也是為什麼您不需要加上前置詞`Model`。</span><span class="sxs-lookup"><span data-stu-id="a91de-197">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="a91de-198">您可以使用"@"字元在內嵌運算式的開頭，並移動後，再`m.`:</span><span class="sxs-lookup"><span data-stu-id="a91de-198">You can use the "@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="a91de-199">會產生下列訊息：</span><span class="sxs-lookup"><span data-stu-id="a91de-199">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="a91de-200">集合屬性時，與`asp-for="CollectionProperty[23].Member"`會產生相同的名稱`asp-for="CollectionProperty[i].Member"`時`i`值`23`。</span><span class="sxs-lookup"><span data-stu-id="a91de-200">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="a91de-201">瀏覽子屬性</span><span class="sxs-lookup"><span data-stu-id="a91de-201">Navigating child properties</span></span>

<span data-ttu-id="a91de-202">您也可以瀏覽至使用檢視模型的屬性路徑的子屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-202">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="a91de-203">請考慮更複雜的模型類別，其中包含子系`Address`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-203">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="a91de-204">在檢視中，我們將繫結至`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="a91de-204">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="a91de-205">下列 HTML 會產生`Address.AddressLine1`:</span><span class="sxs-lookup"><span data-stu-id="a91de-205">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="a91de-206">運算式名稱和集合</span><span class="sxs-lookup"><span data-stu-id="a91de-206">Expression names and Collections</span></span>

<span data-ttu-id="a91de-207">範例中，包含陣列的模型`Colors`:</span><span class="sxs-lookup"><span data-stu-id="a91de-207">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="a91de-208">動作方法：</span><span class="sxs-lookup"><span data-stu-id="a91de-208">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="a91de-209">下列的 Razor 示範如何存取特定`Color`項目：</span><span class="sxs-lookup"><span data-stu-id="a91de-209">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="a91de-210">*Views/Shared/EditorTemplates/String.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="a91de-210">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="a91de-211">範例使用`List<T>`:</span><span class="sxs-lookup"><span data-stu-id="a91de-211">Sample using `List<T>`:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="a91de-212">下列的 Razor 顯示如何反覆查看的集合：</span><span class="sxs-lookup"><span data-stu-id="a91de-212">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="a91de-213">*Views/Shared/EditorTemplates/ToDoItem.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="a91de-213">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]


>[!NOTE]
><span data-ttu-id="a91de-214">一律使用`for`(和*不* `foreach`) 來逐一查看清單。</span><span class="sxs-lookup"><span data-stu-id="a91de-214">Always use `for` (and *not* `foreach`) to iterate over a list.</span></span> <span data-ttu-id="a91de-215">運算式評估在 LINQ 中的索引子可能會耗費資源，應該降到最低。</span><span class="sxs-lookup"><span data-stu-id="a91de-215">Evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="a91de-216">上述加上註解的範例程式碼會示範如何，您必須取代的 lambda 運算式`@`運算子來存取每個`ToDoItem`清單中。</span><span class="sxs-lookup"><span data-stu-id="a91de-216">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="a91de-217">Textarea 標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-217">The Textarea Tag Helper</span></span>

<span data-ttu-id="a91de-218">`Textarea Tag Helper`標記協助程式是輸入標記協助程式類似。</span><span class="sxs-lookup"><span data-stu-id="a91de-218">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="a91de-219">會產生`id`和`name`屬性和資料的驗證屬性的模型從[ \<textarea >](https://www.w3.org/wiki/HTML/Elements/textarea)項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-219">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="a91de-220">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="a91de-220">Provides strong typing.</span></span>

* <span data-ttu-id="a91de-221">HTML Helper 替代程序：`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="a91de-221">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="a91de-222">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-222">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="a91de-223">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-223">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="a91de-224">標籤標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-224">The Label Tag Helper</span></span>

* <span data-ttu-id="a91de-225">會產生標籤標題和`for`屬性[ <label> ](https://www.w3.org/wiki/HTML/Elements/label)運算式名稱的項目</span><span class="sxs-lookup"><span data-stu-id="a91de-225">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="a91de-226">HTML Helper 的替代方式： `Html.LabelFor`。</span><span class="sxs-lookup"><span data-stu-id="a91de-226">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="a91de-227">`Label Tag Helper`純 HTML label 項目透過提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="a91de-227">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="a91de-228">您會自動取得的描述性標籤值`Display`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-228">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="a91de-229">預定的顯示名稱可能會隨著時間和的組合`Display`屬性和標籤標記協助程式會套用`Display`地方使用它。</span><span class="sxs-lookup"><span data-stu-id="a91de-229">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="a91de-230">更少原始程式碼中的標記</span><span class="sxs-lookup"><span data-stu-id="a91de-230">Less markup in source code</span></span>

* <span data-ttu-id="a91de-231">強型別與模型屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-231">Strong typing with the model property.</span></span>

<span data-ttu-id="a91de-232">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-232">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="a91de-233">下列 HTML 會產生`<label>`項目：</span><span class="sxs-lookup"><span data-stu-id="a91de-233">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="a91de-234">產生的標籤標記協助程式`for`與相關聯的屬性值，"Email"，這是識別碼`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-234">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="a91de-235">標記協助程式產生一致`id`和`for`以便能夠正確地建立關聯的項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-235">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="a91de-236">在此範例中的標題來自`Display`屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-236">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="a91de-237">如果模型未包含`Display`屬性標題會是運算式的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="a91de-237">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="a91de-238">驗證標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-238">The Validation Tag Helpers</span></span>

<span data-ttu-id="a91de-239">有兩個驗證標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="a91de-239">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="a91de-240">`Validation Message Tag Helper` （這會顯示單一屬性的驗證訊息上您的模型），而`Validation Summary Tag Helper`（這會顯示驗證錯誤的摘要）。</span><span class="sxs-lookup"><span data-stu-id="a91de-240">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="a91de-241">`Input Tag Helper`新增 HTML5 用戶端端輸入元素為基礎的資料模型類別上的註釋屬性的驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-241">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="a91de-242">伺服器上，也會執行驗證。</span><span class="sxs-lookup"><span data-stu-id="a91de-242">Validation is also performed on the server.</span></span> <span data-ttu-id="a91de-243">驗證標記協助程式在發生驗證錯誤時，會顯示這些錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a91de-243">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="a91de-244">驗證訊息標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-244">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="a91de-245">新增[HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-valmsg-for="property"`屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)元素，其會附加在指定的模型屬性的輸入欄位的驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a91de-245">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="a91de-246">用戶端端驗證錯誤時， [jQuery](https://jquery.com/)顯示中的錯誤訊息`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-246">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="a91de-247">驗證也會在伺服器上的位置。</span><span class="sxs-lookup"><span data-stu-id="a91de-247">Validation also takes place on the server.</span></span> <span data-ttu-id="a91de-248">用戶端可能具有停用 JavaScript 和一些驗證只能在伺服器端。</span><span class="sxs-lookup"><span data-stu-id="a91de-248">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="a91de-249">HTML Helper 替代程序：`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="a91de-249">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="a91de-250">`Validation Message Tag Helper`搭配`asp-validation-for`上的 HTML 屬性[跨越](https://developer.mozilla.org/docs/Web/HTML/Element/span)項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-250">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="a91de-251">驗證訊息標記協助程式將會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-251">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="a91de-252">您通常會使用`Validation Message Tag Helper`之後`Input`標記協助程式針對相同的屬性。</span><span class="sxs-lookup"><span data-stu-id="a91de-252">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="a91de-253">這麼做會顯示附近造成錯誤的輸入任何驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="a91de-253">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="a91de-254">您必須使用正確的 JavaScript 檢視和[jQuery](https://jquery.com/)指令碼參考在用戶端驗證的位置。</span><span class="sxs-lookup"><span data-stu-id="a91de-254">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="a91de-255">請參閱[模型驗證](../models/validation.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="a91de-255">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="a91de-256">伺服器端驗證錯誤，就會發生 （例如當您自訂伺服器端驗證用戶端驗證已停用），MVC 會放在該錯誤訊息的主體為`<span>`項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-256">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="a91de-257">驗證摘要的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-257">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="a91de-258">目標`<div>`的項目`asp-validation-summary`屬性</span><span class="sxs-lookup"><span data-stu-id="a91de-258">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="a91de-259">HTML Helper 替代程序：`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="a91de-259">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="a91de-260">`Validation Summary Tag Helper`用來顯示驗證訊息的摘要。</span><span class="sxs-lookup"><span data-stu-id="a91de-260">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="a91de-261">`asp-validation-summary`屬性值可以是下列任一項：</span><span class="sxs-lookup"><span data-stu-id="a91de-261">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="a91de-262">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="a91de-262">asp-validation-summary</span></span>|<span data-ttu-id="a91de-263">顯示驗證訊息</span><span class="sxs-lookup"><span data-stu-id="a91de-263">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="a91de-264">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="a91de-264">ValidationSummary.All</span></span>|<span data-ttu-id="a91de-265">屬性和模型層級</span><span class="sxs-lookup"><span data-stu-id="a91de-265">Property and model level</span></span>|
|<span data-ttu-id="a91de-266">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="a91de-266">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="a91de-267">型號</span><span class="sxs-lookup"><span data-stu-id="a91de-267">Model</span></span>|
|<span data-ttu-id="a91de-268">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="a91de-268">ValidationSummary.None</span></span>|<span data-ttu-id="a91de-269">無</span><span class="sxs-lookup"><span data-stu-id="a91de-269">None</span></span>|

### <a name="sample"></a><span data-ttu-id="a91de-270">範例</span><span class="sxs-lookup"><span data-stu-id="a91de-270">Sample</span></span>

<span data-ttu-id="a91de-271">在下列範例中，資料模型以裝飾`DataAnnotation`產生驗證錯誤訊息的屬性`<input>`項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-271">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="a91de-272">發生驗證錯誤時，驗證標記協助程式會顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="a91de-272">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="a91de-273">產生的 HTML （如果模型有效）：</span><span class="sxs-lookup"><span data-stu-id="a91de-273">The generated HTML (when the model is valid):</span></span>

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

## <a name="the-select-tag-helper"></a><span data-ttu-id="a91de-274">選取標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-274">The Select Tag Helper</span></span>

* <span data-ttu-id="a91de-275">會產生[選取](https://www.w3.org/wiki/HTML/Elements/select)以及相關聯[選項](https://www.w3.org/wiki/HTML/Elements/option)屬性的模型項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-275">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="a91de-276">有替代的 HTML Helper`Html.DropDownListFor`和`Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="a91de-276">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="a91de-277">`Select Tag Helper` `asp-for`指定模型的屬性名稱，如[選取](https://www.w3.org/wiki/HTML/Elements/select)項目和`asp-items`指定[選項](https://www.w3.org/wiki/HTML/Elements/option)項目。</span><span class="sxs-lookup"><span data-stu-id="a91de-277">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="a91de-278">例如: </span><span class="sxs-lookup"><span data-stu-id="a91de-278">For example:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="a91de-279">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-279">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="a91de-280">`Index`方法初始化`CountryViewModel`、 設定選取的國家/地區，並將其傳遞給`Index`檢視。</span><span class="sxs-lookup"><span data-stu-id="a91de-280">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

<span data-ttu-id="a91de-281">HTTP POST`Index`方法會顯示選取項目：</span><span class="sxs-lookup"><span data-stu-id="a91de-281">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="a91de-282">`Index`檢視：</span><span class="sxs-lookup"><span data-stu-id="a91de-282">The `Index` view:</span></span>

[!code-cshtml[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="a91de-283">如此就會產生 （具有"CA"選取) 下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-283">Which generates the following HTML (with "CA" selected):</span></span>

```html
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
> <span data-ttu-id="a91de-284">我們不建議使用`ViewBag`或`ViewData`與選取的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="a91de-284">We do not recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="a91de-285">檢視模型是較為提供 MVC 中繼資料以及通常較不容易發生問題。</span><span class="sxs-lookup"><span data-stu-id="a91de-285">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="a91de-286">`asp-for`屬性值是特殊案例，而不需要`Model`首碼，其他標記協助程式屬性般 (例如`asp-items`)</span><span class="sxs-lookup"><span data-stu-id="a91de-286">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="a91de-287">列舉繫結</span><span class="sxs-lookup"><span data-stu-id="a91de-287">Enum binding</span></span>

<span data-ttu-id="a91de-288">通常很方便使用`<select>`與`enum`屬性，並產生`SelectListItem`項目從`enum`值。</span><span class="sxs-lookup"><span data-stu-id="a91de-288">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="a91de-289">範例：</span><span class="sxs-lookup"><span data-stu-id="a91de-289">Sample:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="a91de-290">`GetEnumSelectList`方法會產生`SelectList`列舉的物件。</span><span class="sxs-lookup"><span data-stu-id="a91de-290">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="a91de-291">您可以裝飾的列舉值清單`Display`屬性，以取得更豐富的 UI:</span><span class="sxs-lookup"><span data-stu-id="a91de-291">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="a91de-292">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-292">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="a91de-293">群組選項</span><span class="sxs-lookup"><span data-stu-id="a91de-293">Option Group</span></span>

<span data-ttu-id="a91de-294">HTML [ \<optgroup >](https://www.w3.org/wiki/HTML/Elements/optgroup)檢視模型包含一或多個時，會產生元素`SelectListGroup`物件。</span><span class="sxs-lookup"><span data-stu-id="a91de-294">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="a91de-295">`CountryViewModelGroup`群組`SelectListItem`"北美"和"Europe"分組的項目：</span><span class="sxs-lookup"><span data-stu-id="a91de-295">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="a91de-296">兩個群組如下所示：</span><span class="sxs-lookup"><span data-stu-id="a91de-296">The two groups are shown below:</span></span>

![選項群組範例](working-with-forms/_static/grp.png)

<span data-ttu-id="a91de-298">產生的 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-298">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="a91de-299">多個選取</span><span class="sxs-lookup"><span data-stu-id="a91de-299">Multiple select</span></span>

<span data-ttu-id="a91de-300">選取標記協助程式會自動產生[多個 ="多個 「](http://w3c.github.io/html-reference/select.html)屬性中指定的屬性，如果`asp-for`屬性是`IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="a91de-300">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="a91de-301">例如，假設下列模型：</span><span class="sxs-lookup"><span data-stu-id="a91de-301">For example, given the following model:</span></span>

[!code-csharp[Main](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="a91de-302">使用下列檢視：</span><span class="sxs-lookup"><span data-stu-id="a91de-302">With the following view:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="a91de-303">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-303">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="a91de-304">沒有選取項目</span><span class="sxs-lookup"><span data-stu-id="a91de-304">No selection</span></span>

<span data-ttu-id="a91de-305">如果您發現自己使用多個頁面中的 「 未指定"選項，您可以建立範本，以便消除重複的 HTML:</span><span class="sxs-lookup"><span data-stu-id="a91de-305">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[Main](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="a91de-306">*Views/Shared/EditorTemplates/CountryViewModel.cshtml*範本：</span><span class="sxs-lookup"><span data-stu-id="a91de-306">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[Main](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="a91de-307">將 HTML 加入[\<選項 >](https://www.w3.org/wiki/HTML/Elements/option)項目並不限於*沒有選取項目*案例。</span><span class="sxs-lookup"><span data-stu-id="a91de-307">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements is not limited to the *No selection* case.</span></span> <span data-ttu-id="a91de-308">例如，下列檢視和動作方法會產生 HTML 類似於上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="a91de-308">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[Main](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[Main](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="a91de-309">正確`<option>`將選取項目 (包含`selected="selected"`屬性) 根據目前`Country`值。</span><span class="sxs-lookup"><span data-stu-id="a91de-309">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="a91de-310">其他資源</span><span class="sxs-lookup"><span data-stu-id="a91de-310">Additional Resources</span></span>

* [<span data-ttu-id="a91de-311">標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a91de-311">Tag Helpers</span></span>](tag-helpers/intro.md)

* [<span data-ttu-id="a91de-312">HTML 表單元素</span><span class="sxs-lookup"><span data-stu-id="a91de-312">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)

* [<span data-ttu-id="a91de-313">要求驗證語彙基元</span><span class="sxs-lookup"><span data-stu-id="a91de-313">Request Verification Token</span></span>](https://docs.microsoft.com/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)

* [<span data-ttu-id="a91de-314">模型繫結</span><span class="sxs-lookup"><span data-stu-id="a91de-314">Model Binding</span></span>](../models/model-binding.md)

* [<span data-ttu-id="a91de-315">模型驗證</span><span class="sxs-lookup"><span data-stu-id="a91de-315">Model Validation</span></span>](../models/validation.md)

* [<span data-ttu-id="a91de-316">資料註解</span><span class="sxs-lookup"><span data-stu-id="a91de-316">data annotations</span></span>](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)

* <span data-ttu-id="a91de-317">[程式碼片段，這份文件](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample)。</span><span class="sxs-lookup"><span data-stu-id="a91de-317">[Code snippets for this document](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/forms/sample).</span></span>
