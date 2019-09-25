---
title: ASP.NET Core 中的腳本標記協助程式
author: rick-anderson
ms.author: riande
description: 探索 ASP.NET Core 腳本標記協助程式屬性，以及每個屬性在 HTML 腳本標記的擴充行為中所扮演的角色。
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256495"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="a7c9c-103">ASP.NET Core 中的腳本標記協助程式</span><span class="sxs-lookup"><span data-stu-id="a7c9c-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="a7c9c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="a7c9c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="a7c9c-105">[腳本標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper)協助程式會產生主要或切換回腳本檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="a7c9c-106">主要腳本檔案通常位於[內容傳遞網路](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn)（CDN）上。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="a7c9c-107">當 CDN 無法使用時，腳本標籤協助程式可讓您指定腳本檔案的 CDN 和回退。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="a7c9c-108">腳本標記協助程式可提供 CDN 的效能優勢，以及本機裝載的穩定性。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="a7c9c-109">下列 Razor 標記會顯示使用`script` ASP.NET Core web 應用程式範本所建立之配置檔案的元素：</span><span class="sxs-lookup"><span data-stu-id="a7c9c-109">The following Razor markup shows the `script` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

<span data-ttu-id="a7c9c-110">下列類似于上述程式碼所呈現的 HTML （在非開發環境中）：</span><span class="sxs-lookup"><span data-stu-id="a7c9c-110">The following is similar to the rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

<span data-ttu-id="a7c9c-111">在上述程式碼中，腳本標記協助程式`<script>  (window.jQuery || document.write(` `window.jQuery`產生了第二個 script （）專案，它會測試。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-111">In the preceding code, the Script Tag Helper generated the second script ( `<script>  (window.jQuery || document.write(`) element, which tests for `window.jQuery`.</span></span> <span data-ttu-id="a7c9c-112">如果`window.jQuery`找不到， `document.write(`則會執行並建立腳本</span><span class="sxs-lookup"><span data-stu-id="a7c9c-112">If `window.jQuery` is not found, `document.write(` runs and creates a script</span></span> 

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="a7c9c-113">常用的腳本標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="a7c9c-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="a7c9c-114">如需所有腳本標記協助程式屬性、屬性和方法，請參閱[腳本標記 helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) 。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="a7c9c-115">href</span><span class="sxs-lookup"><span data-stu-id="a7c9c-115">href</span></span>

<span data-ttu-id="a7c9c-116">連結資源的慣用位址。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="a7c9c-117">在所有情況下，會將位址視為產生的 HTML。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="a7c9c-118">asp-fallback-href</span><span class="sxs-lookup"><span data-stu-id="a7c9c-118">asp-fallback-href</span></span>

<span data-ttu-id="a7c9c-119">當主要 URL 失敗時，要回復的 CSS 樣式表單 URL。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="a7c9c-120">asp-fallback-測試類別</span><span class="sxs-lookup"><span data-stu-id="a7c9c-120">asp-fallback-test-class</span></span>

<span data-ttu-id="a7c9c-121">在樣式表單中定義用於回溯測試的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="a7c9c-122">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="a7c9c-123">asp-fallback-測試-屬性</span><span class="sxs-lookup"><span data-stu-id="a7c9c-123">asp-fallback-test-property</span></span>

<span data-ttu-id="a7c9c-124">用於回退測試的 CSS 屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="a7c9c-125">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="a7c9c-126">asp-fallback-測試-值</span><span class="sxs-lookup"><span data-stu-id="a7c9c-126">asp-fallback-test-value</span></span>

<span data-ttu-id="a7c9c-127">要用於 fallback 測試的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="a7c9c-128">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="a7c9c-129">asp-fallback-測試-值</span><span class="sxs-lookup"><span data-stu-id="a7c9c-129">asp-fallback-test-value</span></span>

<span data-ttu-id="a7c9c-130">要用於 fallback 測試的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-130">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="a7c9c-131">如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>。</span><span class="sxs-lookup"><span data-stu-id="a7c9c-131">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue></span></span>

## <a name="additional-resources"></a><span data-ttu-id="a7c9c-132">其他資源</span><span class="sxs-lookup"><span data-stu-id="a7c9c-132">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
