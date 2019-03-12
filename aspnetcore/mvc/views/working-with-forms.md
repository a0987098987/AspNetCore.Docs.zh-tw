---
title: ASP.NET Core 表單中的標籤協助程式
author: rick-anderson
description: 描述搭配表單使用的內建標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 02/27/2019
uid: mvc/views/working-with-forms
ms.openlocfilehash: a0fbeac51bd1bfbc50c4d369a479ce5f3091358b
ms.sourcegitcommit: 036d4b03fd86ca5bb378198e29ecf2704257f7b2
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/05/2019
ms.locfileid: "57346251"
---
# <a name="tag-helpers-in-forms-in-aspnet-core"></a><span data-ttu-id="24416-103">ASP.NET Core 表單中的標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-103">Tag Helpers in forms in ASP.NET Core</span></span>

<span data-ttu-id="24416-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)、[N. Taylor Mullen](https://github.com/NTaylorMullen)、[Dave Paquette](https://twitter.com/Dave_Paquette) 和 [Jerrie Pelser](https://github.com/jerriep)</span><span class="sxs-lookup"><span data-stu-id="24416-104">By [Rick Anderson](https://twitter.com/RickAndMSFT), [N. Taylor Mullen](https://github.com/NTaylorMullen), [Dave Paquette](https://twitter.com/Dave_Paquette), and [Jerrie Pelser](https://github.com/jerriep)</span></span>

<span data-ttu-id="24416-105">此文件示範使用表單和常用在表單上的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="24416-105">This document demonstrates working with Forms and the HTML elements commonly used on a Form.</span></span> <span data-ttu-id="24416-106">HTML [表單](https://www.w3.org/TR/html401/interact/forms.html)項目提供用來將資料張貼回伺服器的主要機制 Web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="24416-106">The HTML [Form](https://www.w3.org/TR/html401/interact/forms.html) element provides the primary mechanism web apps use to post back data to the server.</span></span> <span data-ttu-id="24416-107">此文件的大部分在描述[標籤協助程式](tag-helpers/intro.md)，以及它們如何協助您有效率地建立強大的 HTML 表單。</span><span class="sxs-lookup"><span data-stu-id="24416-107">Most of this document describes [Tag Helpers](tag-helpers/intro.md) and how they can help you productively create robust HTML forms.</span></span> <span data-ttu-id="24416-108">我們建議您先閱讀[標籤協助程式簡介](tag-helpers/intro.md)，然後才閱讀這份文件。</span><span class="sxs-lookup"><span data-stu-id="24416-108">We recommend you read [Introduction to Tag Helpers](tag-helpers/intro.md) before you read this document.</span></span>

<span data-ttu-id="24416-109">在許多情況下，HTML 協助程式都會提供特定標籤協助程式的替代方式，但請務必辨識標籤協助程式未取代 HTML 協助程式，而且每個 HTML 協助程式都沒有標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="24416-109">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="24416-110">有 HTML 協助程式替代存在時，便會予以提及。</span><span class="sxs-lookup"><span data-stu-id="24416-110">When an HTML Helper alternative exists, it's mentioned.</span></span>

<a name="my-asp-route-param-ref-label"></a>

## <a name="the-form-tag-helper"></a><span data-ttu-id="24416-111">表單標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-111">The Form Tag Helper</span></span>

<span data-ttu-id="24416-112">[表單](https://www.w3.org/TR/html401/interact/forms.html)標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="24416-112">The [Form](https://www.w3.org/TR/html401/interact/forms.html) Tag Helper:</span></span>

* <span data-ttu-id="24416-113">產生 MVC 控制器動作或具名路由的 HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` 屬性值</span><span class="sxs-lookup"><span data-stu-id="24416-113">Generates the HTML [\<FORM>](https://www.w3.org/TR/html401/interact/forms.html) `action` attribute value for a MVC controller action or named route</span></span>

* <span data-ttu-id="24416-114">產生隱藏的[要求驗證權杖](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)，以防止跨站台要求偽造 (搭配 HTTP Post 動作方法中的 `[ValidateAntiForgeryToken]` 屬性使用時)</span><span class="sxs-lookup"><span data-stu-id="24416-114">Generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method)</span></span>

* <span data-ttu-id="24416-115">提供 `asp-route-<Parameter Name>` 屬性，其中 `<Parameter Name>` 新增至路由值。</span><span class="sxs-lookup"><span data-stu-id="24416-115">Provides the `asp-route-<Parameter Name>` attribute, where `<Parameter Name>` is added to the route values.</span></span> <span data-ttu-id="24416-116">`Html.BeginForm` 和 `Html.BeginRouteForm` 的 `routeValues` 參數提供類似的功能。</span><span class="sxs-lookup"><span data-stu-id="24416-116">The  `routeValues` parameters to `Html.BeginForm` and `Html.BeginRouteForm` provide similar functionality.</span></span>

* <span data-ttu-id="24416-117">有 HTML 協助程式的替代 `Html.BeginForm` 和 `Html.BeginRouteForm`</span><span class="sxs-lookup"><span data-stu-id="24416-117">Has an HTML Helper alternative `Html.BeginForm` and `Html.BeginRouteForm`</span></span>

<span data-ttu-id="24416-118">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-118">Sample:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterFormOnly.cshtml)]

<span data-ttu-id="24416-119">上述表單標籤協助程式會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-119">The Form Tag Helper above generates the following HTML:</span></span>

```HTML
<form method="post" action="/Demo/Register">
    <!-- Input and Submit elements -->
    <input name="__RequestVerificationToken" type="hidden" value="<removed for brevity>" />
</form>
```

<span data-ttu-id="24416-120">MVC 執行階段會從表單標籤協助程式屬性 `asp-controller` 和 `asp-action` 產生 `action` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="24416-120">The MVC runtime generates the `action` attribute value from the Form Tag Helper attributes `asp-controller` and `asp-action`.</span></span> <span data-ttu-id="24416-121">表單標籤協助程式也會產生隱藏的[要求驗證權杖](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)，以防止跨站台要求偽造 (搭配 HTTP Post 動作方法中的 `[ValidateAntiForgeryToken]` 屬性使用時)。</span><span class="sxs-lookup"><span data-stu-id="24416-121">The Form Tag Helper also generates a hidden [Request Verification Token](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages) to prevent cross-site request forgery (when used with the `[ValidateAntiForgeryToken]` attribute in the HTTP Post action method).</span></span> <span data-ttu-id="24416-122">保護純粹的 HTML 表單抵禦跨站台要求偽造很困難，而表單標籤協助程式為您提供此服務。</span><span class="sxs-lookup"><span data-stu-id="24416-122">Protecting a pure HTML Form from cross-site request forgery is difficult, the Form Tag Helper provides this service for you.</span></span>

### <a name="using-a-named-route"></a><span data-ttu-id="24416-123">使用具名路由</span><span class="sxs-lookup"><span data-stu-id="24416-123">Using a named route</span></span>

<span data-ttu-id="24416-124">`asp-route` 標籤協助程式屬性也可以產生 HTML `action` 屬性的標記。</span><span class="sxs-lookup"><span data-stu-id="24416-124">The `asp-route` Tag Helper attribute can also generate markup for the HTML `action` attribute.</span></span> <span data-ttu-id="24416-125">具有名為 `register` 之[路由](../../fundamentals/routing.md)的應用程式，可能針對註冊頁面使用下列標記：</span><span class="sxs-lookup"><span data-stu-id="24416-125">An app with a [route](../../fundamentals/routing.md)  named `register` could use the following markup for the registration page:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterRoute.cshtml)]

<span data-ttu-id="24416-126">*Views/Account* 資料夾 (當您建立具有「個別使用者帳戶」的新 Web 應用程式時產生) 中的許多檢視表，包含 [asp-route-returnurl](xref:mvc/views/working-with-forms) 屬性：</span><span class="sxs-lookup"><span data-stu-id="24416-126">Many of the views in the *Views/Account* folder (generated when you create a new web app with *Individual User Accounts*) contain the [asp-route-returnurl](xref:mvc/views/working-with-forms) attribute:</span></span>

```cshtml
<form asp-controller="Account" asp-action="Login"
     asp-route-returnurl="@ViewData["ReturnUrl"]"
     method="post" class="form-horizontal" role="form">
```

>[!NOTE]
><span data-ttu-id="24416-127">使用內建的範本，`returnUrl` 只會在您嘗試存取授權的資源，但未經過驗證或授權時才自動填入。</span><span class="sxs-lookup"><span data-stu-id="24416-127">With the built in templates, `returnUrl` is only populated automatically when you try to access an authorized resource but are not authenticated or authorized.</span></span> <span data-ttu-id="24416-128">當您嘗試未經授權的存取時，安全性中介軟體會將您重新導向到登入頁面，並設定 `returnUrl`。</span><span class="sxs-lookup"><span data-stu-id="24416-128">When you attempt an unauthorized access, the security middleware redirects you to the login page with the `returnUrl` set.</span></span>

## <a name="the-form-action-tag-helper"></a><span data-ttu-id="24416-129">表單動作標記協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-129">The Form Action Tag Helper</span></span>

<span data-ttu-id="24416-130">表單動作標記協助程式會在產生的 `<button ...>` 或 `<input type="image" ...>` 標記上產生 `formaction` 屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-130">The Form Action Tag Helper generates the `formaction` attribute on the generated `<button ...>` or `<input type="image" ...>` tag.</span></span> <span data-ttu-id="24416-131">`formaction` 屬性可讓您控制表單提交其資料的位置。</span><span class="sxs-lookup"><span data-stu-id="24416-131">The `formaction` attribute controls where a form submits its data.</span></span> <span data-ttu-id="24416-132">它會繫結至類型 `image` 的 [\<input>](https://www.w3.org/wiki/HTML/Elements/input) 元素和 [\<button>](https://www.w3.org/wiki/HTML/Elements/button) 元素。</span><span class="sxs-lookup"><span data-stu-id="24416-132">It binds to [\<input>](https://www.w3.org/wiki/HTML/Elements/input) elements of type `image` and [\<button>](https://www.w3.org/wiki/HTML/Elements/button) elements.</span></span> <span data-ttu-id="24416-133">表單動作標記協助程式允許使用多個 [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` 屬性來控制會為相應元素產生哪個 `formaction` 連結。</span><span class="sxs-lookup"><span data-stu-id="24416-133">The Form Action Tag Helper enables the usage of several [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) `asp-` attributes to control what `formaction` link is generated for the corresponding element.</span></span>

<span data-ttu-id="24416-134">支援 [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) 屬性來控制 `formaction` 的值：</span><span class="sxs-lookup"><span data-stu-id="24416-134">Supported [AnchorTagHelper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) attributes to control the value of `formaction`:</span></span>

|<span data-ttu-id="24416-135">屬性</span><span class="sxs-lookup"><span data-stu-id="24416-135">Attribute</span></span>|<span data-ttu-id="24416-136">說明</span><span class="sxs-lookup"><span data-stu-id="24416-136">Description</span></span>|
|---|---|
|[<span data-ttu-id="24416-137">asp-controller</span><span class="sxs-lookup"><span data-stu-id="24416-137">asp-controller</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-controller)|<span data-ttu-id="24416-138">控制器的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-138">The name of the controller.</span></span>|
|[<span data-ttu-id="24416-139">asp-action</span><span class="sxs-lookup"><span data-stu-id="24416-139">asp-action</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-action)|<span data-ttu-id="24416-140">動作方法的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-140">The name of the action method.</span></span>|
|[<span data-ttu-id="24416-141">asp-area</span><span class="sxs-lookup"><span data-stu-id="24416-141">asp-area</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-area)|<span data-ttu-id="24416-142">區域的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-142">The name of the area.</span></span>|
|[<span data-ttu-id="24416-143">asp-page</span><span class="sxs-lookup"><span data-stu-id="24416-143">asp-page</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page)|<span data-ttu-id="24416-144">Razor 頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-144">The name of the Razor page.</span></span>|
|[<span data-ttu-id="24416-145">asp-page-handler</span><span class="sxs-lookup"><span data-stu-id="24416-145">asp-page-handler</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-page-handler)|<span data-ttu-id="24416-146">Razor 頁面處理常式的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-146">The name of the Razor page handler.</span></span>|
|[<span data-ttu-id="24416-147">asp-route</span><span class="sxs-lookup"><span data-stu-id="24416-147">asp-route</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route)|<span data-ttu-id="24416-148">路由的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-148">The name of the route.</span></span>|
|[<span data-ttu-id="24416-149">asp-route-{value}</span><span class="sxs-lookup"><span data-stu-id="24416-149">asp-route-{value}</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-route-value)|<span data-ttu-id="24416-150">單一 URL 路由值。</span><span class="sxs-lookup"><span data-stu-id="24416-150">A single URL route value.</span></span> <span data-ttu-id="24416-151">例如，`asp-route-id="1234"`。</span><span class="sxs-lookup"><span data-stu-id="24416-151">For example, `asp-route-id="1234"`.</span></span>|
|[<span data-ttu-id="24416-152">asp-all-route-data</span><span class="sxs-lookup"><span data-stu-id="24416-152">asp-all-route-data</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-all-route-data)|<span data-ttu-id="24416-153">所有路由值。</span><span class="sxs-lookup"><span data-stu-id="24416-153">All route values.</span></span>|
|[<span data-ttu-id="24416-154">asp-fragment</span><span class="sxs-lookup"><span data-stu-id="24416-154">asp-fragment</span></span>](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper#asp-fragment)|<span data-ttu-id="24416-155">URL 片段。</span><span class="sxs-lookup"><span data-stu-id="24416-155">The URL fragment.</span></span>|

### <a name="submit-to-controller-example"></a><span data-ttu-id="24416-156">提交至控制器範例</span><span class="sxs-lookup"><span data-stu-id="24416-156">Submit to controller example</span></span>

<span data-ttu-id="24416-157">輸入或選取按鈕時，下列標記會將表單提交到 `HomeController` 的 `Index` 動作：</span><span class="sxs-lookup"><span data-stu-id="24416-157">The following markup submits the form to the `Index` action of `HomeController` when the input or button are selected:</span></span>

```cshtml
<form method="post">
    <button asp-controller="Home" asp-action="Index">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-controller="Home" 
                                asp-action="Index" />
</form>
```

<span data-ttu-id="24416-158">上述標記會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-158">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/Home">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home" />
</form>
```

### <a name="submit-to-page-example"></a><span data-ttu-id="24416-159">提交至頁面範例</span><span class="sxs-lookup"><span data-stu-id="24416-159">Submit to page example</span></span>

<span data-ttu-id="24416-160">下列標記會將表單提交到 `About` Razor 頁面：</span><span class="sxs-lookup"><span data-stu-id="24416-160">The following markup submits the form to the `About` Razor Page:</span></span>

```cshtml
<form method="post">
    <button asp-page="About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-page="About" />
</form>
```

<span data-ttu-id="24416-161">上述標記會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-161">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/About">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/About" />
</form>
```

### <a name="submit-to-route-example"></a><span data-ttu-id="24416-162">提交至路由範例</span><span class="sxs-lookup"><span data-stu-id="24416-162">Submit to route example</span></span>

<span data-ttu-id="24416-163">請考量 `/Home/Test` 端點：</span><span class="sxs-lookup"><span data-stu-id="24416-163">Consider the `/Home/Test` endpoint:</span></span>

```csharp
public class HomeController : Controller
{
    [Route("/Home/Test", Name = "Custom")]
    public string Test()
    {
        return "This is the test page";
    }
}
```

<span data-ttu-id="24416-164">下列標記會將表單提交到 `/Home/Test` 端點。</span><span class="sxs-lookup"><span data-stu-id="24416-164">The following markup submits the form to the `/Home/Test` endpoint.</span></span>

```cshtml
<form method="post">
    <button asp-route="Custom">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" asp-route="Custom" />
</form>
```

<span data-ttu-id="24416-165">上述標記會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-165">The previous markup generates following HTML:</span></span>

```html
<form method="post">
    <button formaction="/Home/Test">Click Me</button>
    <input type="image" src="..." alt="Or Click Me" formaction="/Home/Test" />
</form>
```

## <a name="the-input-tag-helper"></a><span data-ttu-id="24416-166">輸入標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-166">The Input Tag Helper</span></span>

<span data-ttu-id="24416-167">輸入標籤協助程式會將 HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) 項目繫結到 Razor 檢視中的模型運算式。</span><span class="sxs-lookup"><span data-stu-id="24416-167">The Input Tag Helper binds an HTML [\<input>](https://www.w3.org/wiki/HTML/Elements/input) element to a model expression in your razor view.</span></span>

<span data-ttu-id="24416-168">語法：</span><span class="sxs-lookup"><span data-stu-id="24416-168">Syntax:</span></span>

```HTML
<input asp-for="<Expression Name>" />
```

<span data-ttu-id="24416-169">輸入標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="24416-169">The Input Tag Helper:</span></span>

* <span data-ttu-id="24416-170">會為 `asp-for` 屬性中指定的運算式名稱產生 `id` 和 `name` HTML 屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-170">Generates the `id` and `name` HTML attributes for the expression name specified in the `asp-for` attribute.</span></span> <span data-ttu-id="24416-171">`asp-for="Property1.Property2"` 相當於 `m => m.Property1.Property2`。</span><span class="sxs-lookup"><span data-stu-id="24416-171">`asp-for="Property1.Property2"` is equivalent to `m => m.Property1.Property2`.</span></span> <span data-ttu-id="24416-172">運算式的名稱用於 `asp-for` 屬性值。</span><span class="sxs-lookup"><span data-stu-id="24416-172">The name of the expression is what is used for the `asp-for` attribute value.</span></span> <span data-ttu-id="24416-173">請參閱[運算式名稱](#expression-names)一節以取得其他資訊。</span><span class="sxs-lookup"><span data-stu-id="24416-173">See the [Expression names](#expression-names) section for additional information.</span></span>

* <span data-ttu-id="24416-174">根據套用至模型屬性的模型類型和[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性，設定 HTML `type` 屬性值</span><span class="sxs-lookup"><span data-stu-id="24416-174">Sets the HTML `type` attribute value based on the model type and  [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to the model property</span></span>

* <span data-ttu-id="24416-175">已指定 HTML `type` 屬性值時不會予以覆寫</span><span class="sxs-lookup"><span data-stu-id="24416-175">Won't overwrite the HTML `type` attribute value when one is specified</span></span>

* <span data-ttu-id="24416-176">從套用至模型屬性的[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性產生 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) 驗證屬性</span><span class="sxs-lookup"><span data-stu-id="24416-176">Generates [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  validation attributes from [data annotation](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes applied to model properties</span></span>

* <span data-ttu-id="24416-177">`Html.TextBoxFor` 和 `Html.EditorFor` 具有 HTML 協助程式功能重疊。</span><span class="sxs-lookup"><span data-stu-id="24416-177">Has an HTML Helper feature overlap with `Html.TextBoxFor` and `Html.EditorFor`.</span></span> <span data-ttu-id="24416-178">如需詳細資訊，請參閱**輸入標籤協助程式的 HTML 協助程式替代**。</span><span class="sxs-lookup"><span data-stu-id="24416-178">See the **HTML Helper alternatives to Input Tag Helper** section for details.</span></span>

* <span data-ttu-id="24416-179">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="24416-179">Provides strong typing.</span></span> <span data-ttu-id="24416-180">如果屬性的名稱變更，而您未更新標籤協助程式，則會出現如下所示的錯誤：</span><span class="sxs-lookup"><span data-stu-id="24416-180">If the name of the property changes and you don't update the Tag Helper you'll get an error similar to the following:</span></span>

```HTML
An error occurred during the compilation of a resource required to process
this request. Please review the following specific error details and modify
your source code appropriately.

Type expected
 'RegisterViewModel' does not contain a definition for 'Email' and no
 extension method 'Email' accepting a first argument of type 'RegisterViewModel'
 could be found (are you missing a using directive or an assembly reference?)
```

<span data-ttu-id="24416-181">`Input` 標籤協助程式會根據 .NET 型別設定 HTML `type` 屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-181">The `Input` Tag Helper sets the HTML `type` attribute based on the .NET type.</span></span> <span data-ttu-id="24416-182">下表列出一些常見的 .NET 型別和產生的 HTML 型別 (不是每個 .NET 類型都列出)。</span><span class="sxs-lookup"><span data-stu-id="24416-182">The following table lists some common .NET types and generated HTML type (not every .NET type is listed).</span></span>

|<span data-ttu-id="24416-183">.NET 型別</span><span class="sxs-lookup"><span data-stu-id="24416-183">.NET type</span></span>|<span data-ttu-id="24416-184">輸入類型</span><span class="sxs-lookup"><span data-stu-id="24416-184">Input Type</span></span>|
|---|---|
|<span data-ttu-id="24416-185">Bool</span><span class="sxs-lookup"><span data-stu-id="24416-185">Bool</span></span>|<span data-ttu-id="24416-186">type="checkbox"</span><span class="sxs-lookup"><span data-stu-id="24416-186">type="checkbox"</span></span>|
|<span data-ttu-id="24416-187">String</span><span class="sxs-lookup"><span data-stu-id="24416-187">String</span></span>|<span data-ttu-id="24416-188">type="text"</span><span class="sxs-lookup"><span data-stu-id="24416-188">type="text"</span></span>|
|<span data-ttu-id="24416-189">DateTime</span><span class="sxs-lookup"><span data-stu-id="24416-189">DateTime</span></span>|<span data-ttu-id="24416-190">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span><span class="sxs-lookup"><span data-stu-id="24416-190">type=["datetime-local"](https://developer.mozilla.org/docs/Web/HTML/Element/input/datetime-local)</span></span>|
|<span data-ttu-id="24416-191">Byte</span><span class="sxs-lookup"><span data-stu-id="24416-191">Byte</span></span>|<span data-ttu-id="24416-192">type="number"</span><span class="sxs-lookup"><span data-stu-id="24416-192">type="number"</span></span>|
|<span data-ttu-id="24416-193">Int</span><span class="sxs-lookup"><span data-stu-id="24416-193">Int</span></span>|<span data-ttu-id="24416-194">type="number"</span><span class="sxs-lookup"><span data-stu-id="24416-194">type="number"</span></span>|
|<span data-ttu-id="24416-195">Single、Double</span><span class="sxs-lookup"><span data-stu-id="24416-195">Single, Double</span></span>|<span data-ttu-id="24416-196">type="number"</span><span class="sxs-lookup"><span data-stu-id="24416-196">type="number"</span></span>|


<span data-ttu-id="24416-197">下表顯示輸入標籤協助程式將對應至特定的輸入類型的一些常見[資料註解](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter)屬性 (不是每個驗證屬性都列出)：</span><span class="sxs-lookup"><span data-stu-id="24416-197">The following table shows some common [data annotations](/dotnet/api/microsoft.aspnetcore.mvc.dataannotations.iattributeadapter) attributes that the input tag helper will map to specific input types (not every validation attribute is listed):</span></span>


|<span data-ttu-id="24416-198">屬性</span><span class="sxs-lookup"><span data-stu-id="24416-198">Attribute</span></span>|<span data-ttu-id="24416-199">輸入類型</span><span class="sxs-lookup"><span data-stu-id="24416-199">Input Type</span></span>|
|---|---|
|<span data-ttu-id="24416-200">[EmailAddress]</span><span class="sxs-lookup"><span data-stu-id="24416-200">[EmailAddress]</span></span>|<span data-ttu-id="24416-201">type="email"</span><span class="sxs-lookup"><span data-stu-id="24416-201">type="email"</span></span>|
|<span data-ttu-id="24416-202">[Url]</span><span class="sxs-lookup"><span data-stu-id="24416-202">[Url]</span></span>|<span data-ttu-id="24416-203">type="url"</span><span class="sxs-lookup"><span data-stu-id="24416-203">type="url"</span></span>|
|<span data-ttu-id="24416-204">[HiddenInput]</span><span class="sxs-lookup"><span data-stu-id="24416-204">[HiddenInput]</span></span>|<span data-ttu-id="24416-205">type="hidden"</span><span class="sxs-lookup"><span data-stu-id="24416-205">type="hidden"</span></span>|
|<span data-ttu-id="24416-206">[Phone]</span><span class="sxs-lookup"><span data-stu-id="24416-206">[Phone]</span></span>|<span data-ttu-id="24416-207">type="tel"</span><span class="sxs-lookup"><span data-stu-id="24416-207">type="tel"</span></span>|
|<span data-ttu-id="24416-208">[DataType(DataType.Password)]</span><span class="sxs-lookup"><span data-stu-id="24416-208">[DataType(DataType.Password)]</span></span>| <span data-ttu-id="24416-209">type="password"</span><span class="sxs-lookup"><span data-stu-id="24416-209">type="password"</span></span>|
|<span data-ttu-id="24416-210">[DataType(DataType.Date)]</span><span class="sxs-lookup"><span data-stu-id="24416-210">[DataType(DataType.Date)]</span></span>| <span data-ttu-id="24416-211">type="date"</span><span class="sxs-lookup"><span data-stu-id="24416-211">type="date"</span></span>|
|<span data-ttu-id="24416-212">[DataType(DataType.Time)]</span><span class="sxs-lookup"><span data-stu-id="24416-212">[DataType(DataType.Time)]</span></span>| <span data-ttu-id="24416-213">type="time"</span><span class="sxs-lookup"><span data-stu-id="24416-213">type="time"</span></span>|


<span data-ttu-id="24416-214">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-214">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](working-with-forms/sample/final/Views/Demo/RegisterInput.cshtml)]

<span data-ttu-id="24416-215">上述程式碼會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-215">The code above generates the following HTML:</span></span>

```HTML
  <form method="post" action="/Demo/RegisterInput">
       Email:
       <input type="email" data-val="true"
              data-val-email="The Email Address field is not a valid email address."
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

<span data-ttu-id="24416-216">套用至 `Email` 和 `Password` 屬性的資料註解會產生模型的中繼資料。</span><span class="sxs-lookup"><span data-stu-id="24416-216">The data annotations applied to the `Email` and `Password` properties generate metadata on the model.</span></span> <span data-ttu-id="24416-217">輸入標籤協助程式會取用模型中繼資料，並產生 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` 屬性 (請參閱[模型驗證](../models/validation.md))。</span><span class="sxs-lookup"><span data-stu-id="24416-217">The Input Tag Helper consumes the model metadata and produces [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5) `data-val-*` attributes (see [Model Validation](../models/validation.md)).</span></span> <span data-ttu-id="24416-218">這些屬性描述要附加至輸入欄位的驗證程式。</span><span class="sxs-lookup"><span data-stu-id="24416-218">These attributes describe the validators to attach to the input fields.</span></span> <span data-ttu-id="24416-219">這提供低調的 HTML5 和 [jQuery](https://jquery.com/) 驗證。</span><span class="sxs-lookup"><span data-stu-id="24416-219">This provides unobtrusive HTML5 and [jQuery](https://jquery.com/) validation.</span></span> <span data-ttu-id="24416-220">低調屬性具有格式 `data-val-rule="Error Message"`，其中 rule 是驗證規則的名稱 (例如 `data-val-required`、`data-val-email`、`data-val-maxlength` 等。)如果屬性中提供錯誤訊息，就會顯示為 `data-val-rule` 屬性的值。</span><span class="sxs-lookup"><span data-stu-id="24416-220">The unobtrusive attributes have the format `data-val-rule="Error Message"`, where rule is the name of the validation rule (such as `data-val-required`, `data-val-email`, `data-val-maxlength`, etc.) If an error message is provided in the attribute, it's displayed as the value for the `data-val-rule` attribute.</span></span> <span data-ttu-id="24416-221">另外還有 `data-val-ruleName-argumentName="argumentValue"` 格式的屬性，提供規則的其他詳細資料，例如 `data-val-maxlength-max="1024"`。</span><span class="sxs-lookup"><span data-stu-id="24416-221">There are also attributes of the form `data-val-ruleName-argumentName="argumentValue"` that provide additional details about the rule, for example, `data-val-maxlength-max="1024"` .</span></span>

### <a name="html-helper-alternatives-to-input-tag-helper"></a><span data-ttu-id="24416-222">輸入標籤協助程式的 HTML 標記替代</span><span class="sxs-lookup"><span data-stu-id="24416-222">HTML Helper alternatives to Input Tag Helper</span></span>

<span data-ttu-id="24416-223">`Html.TextBox`、`Html.TextBoxFor`、`Html.Editor` 和 `Html.EditorFor` 具有與輸入標籤協助程式重疊的功能。</span><span class="sxs-lookup"><span data-stu-id="24416-223">`Html.TextBox`, `Html.TextBoxFor`, `Html.Editor` and `Html.EditorFor` have overlapping features with the Input Tag Helper.</span></span> <span data-ttu-id="24416-224">輸入標籤協助程式將會自動設定 `type` 屬性；而 `Html.TextBox` 和 `Html.TextBoxFor` 不會。</span><span class="sxs-lookup"><span data-stu-id="24416-224">The Input Tag Helper will automatically set the `type` attribute; `Html.TextBox` and `Html.TextBoxFor` won't.</span></span> <span data-ttu-id="24416-225">`Html.Editor` 和 `Html.EditorFor` 會處理集合、複雜的物件和範本；而輸入標籤協助程式不會。</span><span class="sxs-lookup"><span data-stu-id="24416-225">`Html.Editor` and `Html.EditorFor` handle collections, complex objects and templates; the Input Tag Helper doesn't.</span></span> <span data-ttu-id="24416-226">輸入標籤協助程式、`Html.EditorFor` 和 `Html.TextBoxFor` 為強型別 (它們使用 Lambda 運算式)；而 `Html.TextBox` 和 `Html.Editor` 不是 (它們使用運算式名稱)。</span><span class="sxs-lookup"><span data-stu-id="24416-226">The Input Tag Helper, `Html.EditorFor`  and  `Html.TextBoxFor` are strongly typed (they use lambda expressions); `Html.TextBox` and `Html.Editor` are not (they use expression names).</span></span>

### <a name="htmlattributes"></a><span data-ttu-id="24416-227">HtmlAttributes</span><span class="sxs-lookup"><span data-stu-id="24416-227">HtmlAttributes</span></span>

<span data-ttu-id="24416-228">`@Html.Editor()` 和 `@Html.EditorFor()` 執行它們的預設範本時，使用特殊的 `ViewDataDictionary` 項目，名為 `htmlAttributes`。</span><span class="sxs-lookup"><span data-stu-id="24416-228">`@Html.Editor()` and `@Html.EditorFor()` use a special `ViewDataDictionary` entry named `htmlAttributes` when executing their default templates.</span></span> <span data-ttu-id="24416-229">此行為會選擇性地使用 `additionalViewData` 參數來增強。</span><span class="sxs-lookup"><span data-stu-id="24416-229">This behavior is optionally augmented using `additionalViewData` parameters.</span></span> <span data-ttu-id="24416-230">索引鍵 "htmlAttributes" 不區分大小寫。</span><span class="sxs-lookup"><span data-stu-id="24416-230">The key "htmlAttributes" is case-insensitive.</span></span> <span data-ttu-id="24416-231">索引鍵 "htmlAttributes" 的處理方式類似於傳遞給輸入協助程式 (例如 `@Html.TextBox()`)的 `htmlAttributes` 物件。</span><span class="sxs-lookup"><span data-stu-id="24416-231">The key "htmlAttributes" is handled similarly to the `htmlAttributes` object passed to input helpers like `@Html.TextBox()`.</span></span>

```HTML
@Html.EditorFor(model => model.YourProperty, 
  new { htmlAttributes = new { @class="myCssClass", style="Width:100px" } })
```

### <a name="expression-names"></a><span data-ttu-id="24416-232">運算式名稱</span><span class="sxs-lookup"><span data-stu-id="24416-232">Expression names</span></span>

<span data-ttu-id="24416-233">`asp-for` 屬性值是 `ModelExpression` 和 Lambda 運算式的右邊。</span><span class="sxs-lookup"><span data-stu-id="24416-233">The `asp-for` attribute value is a `ModelExpression` and the right hand side of a lambda expression.</span></span> <span data-ttu-id="24416-234">因此，`asp-for="Property1"` 在產生的程式碼中變成 `m => m.Property1`，這也是為什麼您不需要加上前置詞 `Model` 的原因。</span><span class="sxs-lookup"><span data-stu-id="24416-234">Therefore, `asp-for="Property1"` becomes `m => m.Property1` in the generated code which is why you don't need to prefix with `Model`.</span></span> <span data-ttu-id="24416-235">您可以使用 "\@" 字元來起始內嵌運算式並將它移到 `m.` 前面：</span><span class="sxs-lookup"><span data-stu-id="24416-235">You can use the "\@" character to start an inline expression and move before the `m.`:</span></span>

```HTML
@{
       var joe = "Joe";
   }
   <input asp-for="@joe" />
```

<span data-ttu-id="24416-236">產生下列內容：</span><span class="sxs-lookup"><span data-stu-id="24416-236">Generates the following:</span></span>

```HTML
<input type="text" id="joe" name="joe" value="Joe" />
```

<span data-ttu-id="24416-237">搭配集合屬性，當 `i` 的值為 `23` 時，`asp-for="CollectionProperty[23].Member"` 會產生與 `asp-for="CollectionProperty[i].Member"` 相同的名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-237">With collection properties, `asp-for="CollectionProperty[23].Member"` generates the same name as `asp-for="CollectionProperty[i].Member"` when `i` has the value `23`.</span></span>

<span data-ttu-id="24416-238">當 ASP.NET Core MVC 計算 `ModelExpression` 的值時，會檢查幾個來源，其中包括`ModelState`。</span><span class="sxs-lookup"><span data-stu-id="24416-238">When ASP.NET Core MVC calculates the value of `ModelExpression`, it inspects several sources, including `ModelState`.</span></span> <span data-ttu-id="24416-239">請考慮 `<input type="text" asp-for="@Name" />`。</span><span class="sxs-lookup"><span data-stu-id="24416-239">Consider `<input type="text" asp-for="@Name" />`.</span></span> <span data-ttu-id="24416-240">導出的 `value` 屬性是第一個非 null 的值，來自：</span><span class="sxs-lookup"><span data-stu-id="24416-240">The calculated `value` attribute is the first non-null value from:</span></span>

* <span data-ttu-id="24416-241">索引鍵為 "Name" 的 `ModelState` 項目。</span><span class="sxs-lookup"><span data-stu-id="24416-241">`ModelState` entry with key "Name".</span></span>
* <span data-ttu-id="24416-242">運算式 `Model.Name` 的結果。</span><span class="sxs-lookup"><span data-stu-id="24416-242">Result of the expression `Model.Name`.</span></span>

### <a name="navigating-child-properties"></a><span data-ttu-id="24416-243">巡覽子屬性</span><span class="sxs-lookup"><span data-stu-id="24416-243">Navigating child properties</span></span>

<span data-ttu-id="24416-244">您也可以使用檢視模型的屬性路徑巡覽至子屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-244">You can also navigate to child properties using the property path of the view model.</span></span> <span data-ttu-id="24416-245">請考慮更複雜的模型類別，其中包含子系 `Address` 屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-245">Consider a more complex model class that contains a child `Address` property.</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/AddressViewModel.cs?highlight=1,2,3,4&range=5-8)]

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/RegisterAddressViewModel.cs?highlight=8&range=5-13)]

<span data-ttu-id="24416-246">在檢視中，我們繫結至 `Address.AddressLine1`：</span><span class="sxs-lookup"><span data-stu-id="24416-246">In the view, we bind to `Address.AddressLine1`:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterAddress.cshtml?highlight=6)]

<span data-ttu-id="24416-247">會為 `Address.AddressLine1` 產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-247">The following HTML is generated for `Address.AddressLine1`:</span></span>

```HTML
<input type="text" id="Address_AddressLine1" name="Address.AddressLine1" value="" />
```

### <a name="expression-names-and-collections"></a><span data-ttu-id="24416-248">運算式名稱和集合</span><span class="sxs-lookup"><span data-stu-id="24416-248">Expression names and Collections</span></span>

<span data-ttu-id="24416-249">範例，包含 `Colors` 陣列的模型：</span><span class="sxs-lookup"><span data-stu-id="24416-249">Sample, a model containing an array of `Colors`:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/Person.cs?highlight=3&range=5-10)]

<span data-ttu-id="24416-250">動作方法：</span><span class="sxs-lookup"><span data-stu-id="24416-250">The action method:</span></span>

```csharp
public IActionResult Edit(int id, int colorIndex)
   {
       ViewData["Index"] = colorIndex;
       return View(GetPerson(id));
   }
```

<span data-ttu-id="24416-251">下列 Razor 示範如何存取特定 `Color` 項目：</span><span class="sxs-lookup"><span data-stu-id="24416-251">The following Razor shows how you access a specific `Color` element:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/EditColor.cshtml)]

<span data-ttu-id="24416-252">*Views/Shared/EditorTemplates/String.cshtml* 範本：</span><span class="sxs-lookup"><span data-stu-id="24416-252">The *Views/Shared/EditorTemplates/String.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/String.cshtml)]

<span data-ttu-id="24416-253">使用 `List<T>` 的範例：</span><span class="sxs-lookup"><span data-stu-id="24416-253">Sample using `List<T>`:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/ToDoItem.cs?range=3-8)]

<span data-ttu-id="24416-254">下列 Razor 示範如何逐一查看集合：</span><span class="sxs-lookup"><span data-stu-id="24416-254">The following Razor shows how to iterate over a collection:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Demo/Edit.cshtml)]

<span data-ttu-id="24416-255">*Views/Shared/EditorTemplates/ToDoItem.cshtml* 範本：</span><span class="sxs-lookup"><span data-stu-id="24416-255">The *Views/Shared/EditorTemplates/ToDoItem.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/ToDoItem.cshtml)]

<span data-ttu-id="24416-256">當值將在 `asp-for` 或 `Html.DisplayFor` 相當內容中使用時，應該使用 `foreach` (如果可能的話)。</span><span class="sxs-lookup"><span data-stu-id="24416-256">`foreach` should be used if possible when the value is going to be used in an `asp-for` or `Html.DisplayFor` equivalent context.</span></span> <span data-ttu-id="24416-257">一般而言，`for` 比 `foreach` 好 (若案例允許的話)，因為它不需要配置列舉程式；不過，評估 LINQ 運算式中的索引子可能成本高昂且應該儘可能避免。</span><span class="sxs-lookup"><span data-stu-id="24416-257">In general, `for` is better than `foreach` (if the scenario allows it) because it doesn't need to allocate an enumerator; however, evaluating an indexer in a LINQ expression can be expensive and should be minimized.</span></span>

&nbsp;

>[!NOTE]
><span data-ttu-id="24416-258">上述加上註解的範例程式碼示範如何將 Lambda 運算式取代為 `@` 運算子，來存取清單中的每個 `ToDoItem`。</span><span class="sxs-lookup"><span data-stu-id="24416-258">The commented sample code above shows how you would replace the lambda expression with the `@` operator to access each `ToDoItem` in the list.</span></span>

## <a name="the-textarea-tag-helper"></a><span data-ttu-id="24416-259">Textarea 標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-259">The Textarea Tag Helper</span></span>

<span data-ttu-id="24416-260">`Textarea Tag Helper` 標籤協助程式類似於輸入標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="24416-260">The `Textarea Tag Helper` tag helper is  similar to the Input Tag Helper.</span></span>

* <span data-ttu-id="24416-261">會從 [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) 項目的模型產生 `id` 和 `name` 屬性，以及資料驗證屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-261">Generates the `id` and `name` attributes, and the data validation attributes from the model for a [\<textarea>](https://www.w3.org/wiki/HTML/Elements/textarea) element.</span></span>

* <span data-ttu-id="24416-262">提供強型別。</span><span class="sxs-lookup"><span data-stu-id="24416-262">Provides strong typing.</span></span>

* <span data-ttu-id="24416-263">HTML 協助程式替代：`Html.TextAreaFor`</span><span class="sxs-lookup"><span data-stu-id="24416-263">HTML Helper alternative: `Html.TextAreaFor`</span></span>

<span data-ttu-id="24416-264">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-264">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/DescriptionViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterTextArea.cshtml?highlight=4)]

<span data-ttu-id="24416-265">會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-265">The following HTML is generated:</span></span>

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

## <a name="the-label-tag-helper"></a><span data-ttu-id="24416-266">標籤標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-266">The Label Tag Helper</span></span>

* <span data-ttu-id="24416-267">會針對運算式名稱產生標籤標題和 [<label>](https://www.w3.org/wiki/HTML/Elements/label) 項目的 `for` 屬性</span><span class="sxs-lookup"><span data-stu-id="24416-267">Generates the label caption and `for` attribute on a [<label>](https://www.w3.org/wiki/HTML/Elements/label) element for an expression name</span></span>

* <span data-ttu-id="24416-268">HTML 協助程式替代：`Html.LabelFor`。</span><span class="sxs-lookup"><span data-stu-id="24416-268">HTML Helper alternative: `Html.LabelFor`.</span></span>

<span data-ttu-id="24416-269">`Label Tag Helper` 相較於純粹的 HTML標籤項目，提供下列優點：</span><span class="sxs-lookup"><span data-stu-id="24416-269">The `Label Tag Helper`  provides the following benefits over a pure HTML label element:</span></span>

* <span data-ttu-id="24416-270">您會自動從 `Display` 屬性取得描述性的標籤值。</span><span class="sxs-lookup"><span data-stu-id="24416-270">You automatically get the descriptive label value from the `Display` attribute.</span></span> <span data-ttu-id="24416-271">預定的顯示名稱可能會隨著時間而改變，`Display` 屬性和標籤標籤協助程式的組合會在使用它的每個地方都套用 `Display`。</span><span class="sxs-lookup"><span data-stu-id="24416-271">The intended display name might change over time, and the combination of `Display` attribute and Label Tag Helper will apply the `Display` everywhere it's used.</span></span>

* <span data-ttu-id="24416-272">原始程式碼中較少標記</span><span class="sxs-lookup"><span data-stu-id="24416-272">Less markup in source code</span></span>

* <span data-ttu-id="24416-273">強型別與模型屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-273">Strong typing with the model property.</span></span>

<span data-ttu-id="24416-274">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-274">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/SimpleViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterLabel.cshtml?highlight=4)]

<span data-ttu-id="24416-275">會為 `<label>` 項目產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-275">The following HTML is generated for the `<label>` element:</span></span>

```HTML
<label for="Email">Email Address</label>
```

<span data-ttu-id="24416-276">標籤標籤協助程式產生了 "Email" 的 `for` 屬性值，這是與 `<input>` 項目建立關聯的識別碼。</span><span class="sxs-lookup"><span data-stu-id="24416-276">The Label Tag Helper generated the `for` attribute value of "Email", which is the ID associated with the `<input>` element.</span></span> <span data-ttu-id="24416-277">標籤協助程式會產生一致的 `id` 和 `for` 項目，所以能夠正確地相關聯。</span><span class="sxs-lookup"><span data-stu-id="24416-277">The Tag Helpers generate consistent `id` and `for` elements so they can be correctly associated.</span></span> <span data-ttu-id="24416-278">在此範例中的標題來自 `Display` 屬性。</span><span class="sxs-lookup"><span data-stu-id="24416-278">The caption in this sample comes from the `Display` attribute.</span></span> <span data-ttu-id="24416-279">如果模型未包含 `Display` 屬性，標題會是運算式的屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="24416-279">If the model didn't contain a `Display` attribute, the caption would be the property name of the expression.</span></span>

## <a name="the-validation-tag-helpers"></a><span data-ttu-id="24416-280">驗證標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-280">The Validation Tag Helpers</span></span>

<span data-ttu-id="24416-281">有兩個驗證標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="24416-281">There are two Validation Tag Helpers.</span></span> <span data-ttu-id="24416-282">`Validation Message Tag Helper` (這會顯示模型上單一屬性的驗證訊息)，和 `Validation Summary Tag Helper` (這會顯示驗證錯誤的摘要)。</span><span class="sxs-lookup"><span data-stu-id="24416-282">The `Validation Message Tag Helper` (which displays a validation message for a single property on your model), and the `Validation Summary Tag Helper` (which displays a summary of validation errors).</span></span> <span data-ttu-id="24416-283">`Input Tag Helper` 會根據您模型類別上的資料註釋屬性，將 HTML5 用戶端端驗證屬性新增至輸入項目。</span><span class="sxs-lookup"><span data-stu-id="24416-283">The `Input Tag Helper` adds HTML5 client side validation attributes to input elements based on data annotation attributes on your model classes.</span></span> <span data-ttu-id="24416-284">也會在伺服器上執行驗證。</span><span class="sxs-lookup"><span data-stu-id="24416-284">Validation is also performed on the server.</span></span> <span data-ttu-id="24416-285">驗證標籤協助程式會在發生驗證錯誤時顯示這些錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24416-285">The Validation Tag Helper displays these error messages when a validation error occurs.</span></span>

### <a name="the-validation-message-tag-helper"></a><span data-ttu-id="24416-286">驗證訊息標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-286">The Validation Message Tag Helper</span></span>

* <span data-ttu-id="24416-287">新增 [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)`data-valmsg-for="property"` 屬性至 [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 項目，它會將驗證錯誤訊息附加在指定模型屬性的輸入欄位。</span><span class="sxs-lookup"><span data-stu-id="24416-287">Adds the [HTML5](https://developer.mozilla.org/docs/Web/Guide/HTML/HTML5)  `data-valmsg-for="property"` attribute to the [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element, which attaches the validation error messages on the input field of the specified model property.</span></span> <span data-ttu-id="24416-288">發生用戶端驗證錯誤時，[jQuery](https://jquery.com/) 會在 `<span>` 項目顯示錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24416-288">When a client side validation error occurs, [jQuery](https://jquery.com/) displays the error message in the `<span>` element.</span></span>

* <span data-ttu-id="24416-289">也會在伺服器上發生驗證。</span><span class="sxs-lookup"><span data-stu-id="24416-289">Validation also takes place on the server.</span></span> <span data-ttu-id="24416-290">用戶端可能停用 JavaScript，而一些驗證只能在伺服器端進行。</span><span class="sxs-lookup"><span data-stu-id="24416-290">Clients may have JavaScript disabled and some validation can only be done on the server side.</span></span>

* <span data-ttu-id="24416-291">HTML 協助程式替代：`Html.ValidationMessageFor`</span><span class="sxs-lookup"><span data-stu-id="24416-291">HTML Helper alternative: `Html.ValidationMessageFor`</span></span>

<span data-ttu-id="24416-292">`Validation Message Tag Helper` 與 HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) 項目上的 `asp-validation-for` 屬性搭配使用。</span><span class="sxs-lookup"><span data-stu-id="24416-292">The `Validation Message Tag Helper`  is used with the `asp-validation-for` attribute on a HTML [span](https://developer.mozilla.org/docs/Web/HTML/Element/span) element.</span></span>

```HTML
<span asp-validation-for="Email"></span>
```

<span data-ttu-id="24416-293">驗證訊息標籤協助程式將會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-293">The Validation Message Tag Helper will generate the following HTML:</span></span>

```HTML
<span class="field-validation-valid"
  data-valmsg-for="Email"
  data-valmsg-replace="true"></span>
```

<span data-ttu-id="24416-294">您通常會在 `Input` 標籤協助程式之後針對相同的屬性使用 `Validation Message Tag Helper`。</span><span class="sxs-lookup"><span data-stu-id="24416-294">You generally use the `Validation Message Tag Helper`  after an `Input` Tag Helper for the same property.</span></span> <span data-ttu-id="24416-295">這麼做會在造成錯誤的輸入附近顯示任何驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24416-295">Doing so displays any validation error messages near the input that caused the error.</span></span>

> [!NOTE]
> <span data-ttu-id="24416-296">您必須具有正確 JavaScript 和 [jQuery](https://jquery.com/) 指令碼參考的檢視。</span><span class="sxs-lookup"><span data-stu-id="24416-296">You must have a view with the correct JavaScript and [jQuery](https://jquery.com/) script references in place for client side validation.</span></span> <span data-ttu-id="24416-297">如需詳細資訊，請參閱[模型驗證](../models/validation.md)。</span><span class="sxs-lookup"><span data-stu-id="24416-297">See [Model Validation](../models/validation.md) for more information.</span></span>

<span data-ttu-id="24416-298">發生伺服器端驗證錯誤時 (例如當您有自訂伺服器端驗證或是已停用用戶端驗證時)，MVC 會將該錯誤訊息放置為 `<span>` 項目的主體。</span><span class="sxs-lookup"><span data-stu-id="24416-298">When a server side validation error occurs (for example when you have custom server side validation or client-side validation is disabled), MVC places that error message as the body of the `<span>` element.</span></span>

```HTML
<span class="field-validation-error" data-valmsg-for="Email"
            data-valmsg-replace="true">
   The Email Address field is required.
</span>
```

### <a name="the-validation-summary-tag-helper"></a><span data-ttu-id="24416-299">驗證摘要標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-299">The Validation Summary Tag Helper</span></span>

* <span data-ttu-id="24416-300">使用 `asp-validation-summary` 屬性設定 `<div>` 項目的目標</span><span class="sxs-lookup"><span data-stu-id="24416-300">Targets `<div>` elements with the `asp-validation-summary` attribute</span></span>

* <span data-ttu-id="24416-301">HTML 協助程式替代：`@Html.ValidationSummary`</span><span class="sxs-lookup"><span data-stu-id="24416-301">HTML Helper alternative: `@Html.ValidationSummary`</span></span>

<span data-ttu-id="24416-302">`Validation Summary Tag Helper` 用來顯示驗證訊息的摘要。</span><span class="sxs-lookup"><span data-stu-id="24416-302">The `Validation Summary Tag Helper`  is used to display a summary of validation messages.</span></span> <span data-ttu-id="24416-303">`asp-validation-summary` 屬性值可以是下列任一個：</span><span class="sxs-lookup"><span data-stu-id="24416-303">The `asp-validation-summary` attribute value can be any of the following:</span></span>

|<span data-ttu-id="24416-304">asp-validation-summary</span><span class="sxs-lookup"><span data-stu-id="24416-304">asp-validation-summary</span></span>|<span data-ttu-id="24416-305">顯示的驗證訊息</span><span class="sxs-lookup"><span data-stu-id="24416-305">Validation messages displayed</span></span>|
|--- |--- |
|<span data-ttu-id="24416-306">ValidationSummary.All</span><span class="sxs-lookup"><span data-stu-id="24416-306">ValidationSummary.All</span></span>|<span data-ttu-id="24416-307">屬性和模型層級</span><span class="sxs-lookup"><span data-stu-id="24416-307">Property and model level</span></span>|
|<span data-ttu-id="24416-308">ValidationSummary.ModelOnly</span><span class="sxs-lookup"><span data-stu-id="24416-308">ValidationSummary.ModelOnly</span></span>|<span data-ttu-id="24416-309">型號</span><span class="sxs-lookup"><span data-stu-id="24416-309">Model</span></span>|
|<span data-ttu-id="24416-310">ValidationSummary.None</span><span class="sxs-lookup"><span data-stu-id="24416-310">ValidationSummary.None</span></span>|<span data-ttu-id="24416-311">無</span><span class="sxs-lookup"><span data-stu-id="24416-311">None</span></span>|

### <a name="sample"></a><span data-ttu-id="24416-312">範例</span><span class="sxs-lookup"><span data-stu-id="24416-312">Sample</span></span>

<span data-ttu-id="24416-313">在下列範例中，資料模型裝飾了 `DataAnnotation` 屬性，它會在 `<input>` 項目產生驗證錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="24416-313">In the following example, the data model is decorated with `DataAnnotation` attributes, which generates validation error messages on the `<input>` element.</span></span>  <span data-ttu-id="24416-314">在發生驗證錯誤時，驗證標籤協助程式會顯示錯誤訊息：</span><span class="sxs-lookup"><span data-stu-id="24416-314">When a validation error occurs, the Validation Tag Helper displays the error message:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/RegisterViewModel.cs)]

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Demo/RegisterValidation.cshtml?highlight=4,6,8&range=1-10)]

<span data-ttu-id="24416-315">產生的 HTML (當模型有效時)：</span><span class="sxs-lookup"><span data-stu-id="24416-315">The generated HTML (when the model is valid):</span></span>

```HTML
<form action="/DemoReg/Register" method="post">
  <div class="validation-summary-valid" data-valmsg-summary="true">
  <ul><li style="display:none"></li></ul></div>
  Email:  <input name="Email" id="Email" type="email" value=""
   data-val-required="The Email field is required."
   data-val-email="The Email field is not a valid email address."
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

## <a name="the-select-tag-helper"></a><span data-ttu-id="24416-316">選取標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="24416-316">The Select Tag Helper</span></span>

* <span data-ttu-id="24416-317">為模型的屬性產生 [select](https://www.w3.org/wiki/HTML/Elements/select) 以及相關聯的 [option](https://www.w3.org/wiki/HTML/Elements/option) 項目。</span><span class="sxs-lookup"><span data-stu-id="24416-317">Generates [select](https://www.w3.org/wiki/HTML/Elements/select) and associated [option](https://www.w3.org/wiki/HTML/Elements/option) elements for properties of your model.</span></span>

* <span data-ttu-id="24416-318">有 HTML 協助程式的替代 `Html.DropDownListFor` 和 `Html.ListBoxFor`</span><span class="sxs-lookup"><span data-stu-id="24416-318">Has an HTML Helper alternative `Html.DropDownListFor` and `Html.ListBoxFor`</span></span>

<span data-ttu-id="24416-319">`Select Tag Helper` `asp-for` 指定 [select](https://www.w3.org/wiki/HTML/Elements/select) 項目的模型屬性名稱，而 `asp-items` 指定 [option](https://www.w3.org/wiki/HTML/Elements/option) 項目。</span><span class="sxs-lookup"><span data-stu-id="24416-319">The `Select Tag Helper` `asp-for` specifies the model property  name for the [select](https://www.w3.org/wiki/HTML/Elements/select) element  and `asp-items` specifies the [option](https://www.w3.org/wiki/HTML/Elements/option) elements.</span></span>  <span data-ttu-id="24416-320">例如：</span><span class="sxs-lookup"><span data-stu-id="24416-320">For example:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

<span data-ttu-id="24416-321">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-321">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryViewModel.cs)]

<span data-ttu-id="24416-322">`Index` 方法會初始化 `CountryViewModel`、設定選取的國家/地區，並將其傳遞給 `Index` 檢視。</span><span class="sxs-lookup"><span data-stu-id="24416-322">The `Index` method initializes the `CountryViewModel`, sets the selected country and passes it to the `Index` view.</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=8-13)]

<span data-ttu-id="24416-323">HTTP POST `Index` 方法會顯示選取項目：</span><span class="sxs-lookup"><span data-stu-id="24416-323">The HTTP POST `Index` method displays the selection:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=15-27)]

<span data-ttu-id="24416-324">`Index` 檢視：</span><span class="sxs-lookup"><span data-stu-id="24416-324">The `Index` view:</span></span>

[!code-cshtml[](working-with-forms/sample/final/Views/Home/Index.cshtml?highlight=4)]

<span data-ttu-id="24416-325">它會產生下列 HTML (並選取 "CA")：</span><span class="sxs-lookup"><span data-stu-id="24416-325">Which generates the following HTML (with "CA" selected):</span></span>

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
> <span data-ttu-id="24416-326">我們不建議使用 `ViewBag` 或 `ViewData` 搭配選取標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="24416-326">We don't recommend using `ViewBag` or `ViewData` with the Select Tag Helper.</span></span> <span data-ttu-id="24416-327">檢視模型在提供 MVC 中繼資料方面比較強大，且通常較不容易發生問題。</span><span class="sxs-lookup"><span data-stu-id="24416-327">A view model is more robust at providing MVC metadata and generally less problematic.</span></span>

<span data-ttu-id="24416-328">`asp-for` 屬性值是特殊案例，不需要 `Model` 前置詞，其他標籤協助程式屬性則需要 (例如 `asp-items`)</span><span class="sxs-lookup"><span data-stu-id="24416-328">The `asp-for` attribute value is a special case and doesn't require a `Model` prefix, the other Tag Helper attributes do (such as `asp-items`)</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Home/Index.cshtml?range=4)]

### <a name="enum-binding"></a><span data-ttu-id="24416-329">列舉繫結</span><span class="sxs-lookup"><span data-stu-id="24416-329">Enum binding</span></span>

<span data-ttu-id="24416-330">使用 `<select>` 搭配 `enum` 屬性，並從 `enum` 值產生 `SelectListItem` 項目通常很方便。</span><span class="sxs-lookup"><span data-stu-id="24416-330">It's often convenient to use `<select>` with an `enum` property and generate the `SelectListItem` elements from the `enum` values.</span></span>

<span data-ttu-id="24416-331">範例：</span><span class="sxs-lookup"><span data-stu-id="24416-331">Sample:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnumViewModel.cs?range=3-7)]

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs)]

<span data-ttu-id="24416-332">`GetEnumSelectList` 方法會產生列舉的 `SelectList` 物件。</span><span class="sxs-lookup"><span data-stu-id="24416-332">The `GetEnumSelectList` method generates a `SelectList` object for an enum.</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEnum.cshtml?highlight=5)]

<span data-ttu-id="24416-333">您可以用 `Display` 屬性裝飾列舉值清單，以取得更豐富的 UI：</span><span class="sxs-lookup"><span data-stu-id="24416-333">You can decorate your enumerator list with the `Display` attribute to get a richer UI:</span></span>

[!code-csharp[](working-with-forms/sample/final/ViewModels/CountryEnum.cs?highlight=5,7)]

<span data-ttu-id="24416-334">會產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-334">The following HTML is generated:</span></span>

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

### <a name="option-group"></a><span data-ttu-id="24416-335">選項群組</span><span class="sxs-lookup"><span data-stu-id="24416-335">Option Group</span></span>

<span data-ttu-id="24416-336">當檢視模型包含一或多個 `SelectListGroup` 物件時，會產生 HTML [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) 項目。</span><span class="sxs-lookup"><span data-stu-id="24416-336">The HTML  [\<optgroup>](https://www.w3.org/wiki/HTML/Elements/optgroup) element is generated when the view model contains one or more `SelectListGroup` objects.</span></span>

<span data-ttu-id="24416-337">`CountryViewModelGroup` 將 `SelectListItem` 項目分組成「北美洲」和「歐洲」群組：</span><span class="sxs-lookup"><span data-stu-id="24416-337">The `CountryViewModelGroup` groups the `SelectListItem` elements into the "North America" and "Europe" groups:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelGroup.cs?highlight=5,6,14,20,26,32,38,44&range=6-56)]

<span data-ttu-id="24416-338">兩個群組如下所示：</span><span class="sxs-lookup"><span data-stu-id="24416-338">The two groups are shown below:</span></span>

![選項群組範例](working-with-forms/_static/grp.png)

<span data-ttu-id="24416-340">產生的 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-340">The generated HTML:</span></span>

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

### <a name="multiple-select"></a><span data-ttu-id="24416-341">多重選項</span><span class="sxs-lookup"><span data-stu-id="24416-341">Multiple select</span></span>

<span data-ttu-id="24416-342">選取標籤協助程式會自動產生 [multiple = "multiple"](http://w3c.github.io/html-reference/select.html) 屬性，如果 `asp-for` 屬性中指定的屬性是 `IEnumerable`。</span><span class="sxs-lookup"><span data-stu-id="24416-342">The Select Tag Helper  will automatically generate the [multiple = "multiple"](http://w3c.github.io/html-reference/select.html)  attribute if the property specified in the `asp-for` attribute is an `IEnumerable`.</span></span> <span data-ttu-id="24416-343">例如，假設有以下的模型：</span><span class="sxs-lookup"><span data-stu-id="24416-343">For example, given the following model:</span></span>

[!code-csharp[](../../mvc/views/working-with-forms/sample/final/ViewModels/CountryViewModelIEnumerable.cs?highlight=6)]

<span data-ttu-id="24416-344">具有下列檢視：</span><span class="sxs-lookup"><span data-stu-id="24416-344">With the following view:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexMultiSelect.cshtml?highlight=4)]

<span data-ttu-id="24416-345">產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-345">Generates the following HTML:</span></span>

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

### <a name="no-selection"></a><span data-ttu-id="24416-346">沒有選取項目</span><span class="sxs-lookup"><span data-stu-id="24416-346">No selection</span></span>

<span data-ttu-id="24416-347">如果您發現自己在多個頁面中使用「未指定」的選項，您可以建立範本，以避免重複 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-347">If you find yourself using the "not specified" option in multiple pages, you can create a template to eliminate repeating the HTML:</span></span>

[!code-HTML[](../../mvc/views/working-with-forms/sample/final/Views/Home/IndexEmptyTemplate.cshtml?highlight=5)]

<span data-ttu-id="24416-348">*Views/Shared/EditorTemplates/CountryViewModel.cshtml* 範本：</span><span class="sxs-lookup"><span data-stu-id="24416-348">The *Views/Shared/EditorTemplates/CountryViewModel.cshtml* template:</span></span>

[!code-HTML[](working-with-forms/sample/final/Views/Shared/EditorTemplates/CountryViewModel.cshtml)]

<span data-ttu-id="24416-349">新增 HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) 項目並不限於「沒有選取項目」的案例。</span><span class="sxs-lookup"><span data-stu-id="24416-349">Adding HTML [\<option>](https://www.w3.org/wiki/HTML/Elements/option) elements isn't limited to the *No selection* case.</span></span> <span data-ttu-id="24416-350">例如，下列檢視和動作方法會產生類似於上述程式碼的 HTML：</span><span class="sxs-lookup"><span data-stu-id="24416-350">For example, the following view and action method will generate HTML similar to the code above:</span></span>

[!code-csharp[](working-with-forms/sample/final/Controllers/HomeController.cs?range=114-119)]

[!code-HTML[](working-with-forms/sample/final/Views/Home/IndexOption.cshtml)]

<span data-ttu-id="24416-351">將會選取正確的 `<option>` 項目 (包含 `selected="selected"` 屬性)，視目前的 `Country` 值而定。</span><span class="sxs-lookup"><span data-stu-id="24416-351">The correct `<option>` element will be selected ( contain the `selected="selected"` attribute) depending on the current `Country` value.</span></span>

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

## <a name="additional-resources"></a><span data-ttu-id="24416-352">其他資源</span><span class="sxs-lookup"><span data-stu-id="24416-352">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* [<span data-ttu-id="24416-353">HTML 表單項目</span><span class="sxs-lookup"><span data-stu-id="24416-353">HTML Form element</span></span>](https://www.w3.org/TR/html401/interact/forms.html)
* [<span data-ttu-id="24416-354">要求驗證權杖</span><span class="sxs-lookup"><span data-stu-id="24416-354">Request Verification Token</span></span>](/aspnet/mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages)
* <xref:mvc/models/model-binding>
* <xref:mvc/models/validation>
* [<span data-ttu-id="24416-355">IAttributeAdapter 介面</span><span class="sxs-lookup"><span data-stu-id="24416-355">IAttributeAdapter Interface</span></span>](/dotnet/api/Microsoft.AspNetCore.Mvc.DataAnnotations.IAttributeAdapter)
* [<span data-ttu-id="24416-356">此文件的程式碼片段</span><span class="sxs-lookup"><span data-stu-id="24416-356">Code snippets for this document</span></span>](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/working-with-forms/sample/final)
