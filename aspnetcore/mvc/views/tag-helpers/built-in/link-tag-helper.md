---
title: ASP.NET Core 中的連結標記協助程式
author: rick-anderson
ms.author: riande
description: 探索 ASP.NET Core 連結標記協助程式屬性，以及每個屬性在 HTML 連結標記的擴充行為中所扮演的角色。
ms.custom: mvc
ms.date: 09/24/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/link-tag-helper
ms.openlocfilehash: ac9f6449e2b7b135318ecf116e1dba7b33ddff83
ms.sourcegitcommit: 50e7c970f327dbe92d45eaf4c21caa001c9106d0
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/10/2020
ms.locfileid: "86212398"
---
# <a name="link-tag-helper-in-aspnet-core"></a><span data-ttu-id="c4f90-103">ASP.NET Core 中的連結標記協助程式</span><span class="sxs-lookup"><span data-stu-id="c4f90-103">Link Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="c4f90-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="c4f90-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="c4f90-105">[連結](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper)標籤協助程式會產生主要或切換回 CSS 檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="c4f90-105">The [Link Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper) generates a link to a primary or fall back CSS file.</span></span> <span data-ttu-id="c4f90-106">主要 CSS 檔案通常是在[內容傳遞網路](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN) 上。</span><span class="sxs-lookup"><span data-stu-id="c4f90-106">Typically the primary CSS file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="c4f90-107">連結標籤協助程式可讓您指定 CSS 檔案的 CDN，以及 CDN 無法使用時的回復。</span><span class="sxs-lookup"><span data-stu-id="c4f90-107">The Link Tag Helper allows you to specify a CDN for the CSS file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="c4f90-108">連結標籤協助程式可提供 CDN 的效能優勢，以及本機裝載的穩定性。</span><span class="sxs-lookup"><span data-stu-id="c4f90-108">The Link Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="c4f90-109">下列 Razor 標記顯示 `head` 使用 ASP.NET Core web 應用程式範本所建立之配置檔案的元素：</span><span class="sxs-lookup"><span data-stu-id="c4f90-109">The following Razor markup shows the `head` element of a layout file created with the ASP.NET Core web app template:</span></span>

[!code-cshtml[](link-tag-helper/sample/_Layout.cshtml?name=snippet)]

<span data-ttu-id="c4f90-110">以下是先前程式碼 (在非開發環境中轉譯的 HTML) ：</span><span class="sxs-lookup"><span data-stu-id="c4f90-110">The following is rendered HTML from the preceding code (in a non-Development environment):</span></span>

[!code-html[](link-tag-helper/sample/HtmlPage1.html)]

<span data-ttu-id="c4f90-111">在上述程式碼中，連結標記協助 `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` 程式會產生元素和下列 JavaScript，用來驗證要求的*啟動程式。最小的 .css*檔案可在 CDN 上使用。</span><span class="sxs-lookup"><span data-stu-id="c4f90-111">In the preceding code, the Link Tag Helper generated the `<meta name="x-stylesheet-fallback-test" content="" class="sr-only" />` element and the following JavaScript which is used to verify the requested *bootstrap.min.css* file is available on the CDN.</span></span> <span data-ttu-id="c4f90-112">在此情況下，CSS 檔案可供使用，標記協助程式會 `<link />` 使用 CDN CSS 檔案產生元素。</span><span class="sxs-lookup"><span data-stu-id="c4f90-112">In this case, the CSS file was available so the Tag Helper generated the `<link />` element with the CDN CSS file.</span></span>

## <a name="commonly-used-link-tag-helper-attributes"></a><span data-ttu-id="c4f90-113">常用的連結標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="c4f90-113">Commonly used Link Tag Helper attributes</span></span>

<span data-ttu-id="c4f90-114">請參閱[連結](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper)標記協助程式，以取得所有的連結標籤 helper 屬性、屬性和方法。</span><span class="sxs-lookup"><span data-stu-id="c4f90-114">See [Link Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper)  for all the Link Tag Helper attributes, properties, and methods.</span></span>

### <a name="href"></a><span data-ttu-id="c4f90-115">href</span><span class="sxs-lookup"><span data-stu-id="c4f90-115">href</span></span>

<span data-ttu-id="c4f90-116">連結資源的慣用位址。</span><span class="sxs-lookup"><span data-stu-id="c4f90-116">Preferred address of the linked resource.</span></span> <span data-ttu-id="c4f90-117">在所有情況下，會將位址視為產生的 HTML。</span><span class="sxs-lookup"><span data-stu-id="c4f90-117">The address is passed thought to the generated HTML in all cases.</span></span>

### <a name="asp-fallback-href"></a><span data-ttu-id="c4f90-118">asp-fallback-href</span><span class="sxs-lookup"><span data-stu-id="c4f90-118">asp-fallback-href</span></span>

<span data-ttu-id="c4f90-119">當主要 URL 失敗時，要回復的 CSS 樣式表單 URL。</span><span class="sxs-lookup"><span data-stu-id="c4f90-119">The URL of a CSS stylesheet to fallback to in the case the primary URL fails.</span></span>

### <a name="asp-fallback-test-class"></a><span data-ttu-id="c4f90-120">asp-fallback-測試類別</span><span class="sxs-lookup"><span data-stu-id="c4f90-120">asp-fallback-test-class</span></span>

<span data-ttu-id="c4f90-121">在樣式表單中定義用於回溯測試的類別名稱。</span><span class="sxs-lookup"><span data-stu-id="c4f90-121">The class name defined in the stylesheet to use for the fallback test.</span></span> <span data-ttu-id="c4f90-122">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass> 。</span><span class="sxs-lookup"><span data-stu-id="c4f90-122">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>.</span></span>

### <a name="asp-fallback-test-property"></a><span data-ttu-id="c4f90-123">asp-fallback-測試-屬性</span><span class="sxs-lookup"><span data-stu-id="c4f90-123">asp-fallback-test-property</span></span>

<span data-ttu-id="c4f90-124">用於回退測試的 CSS 屬性名稱。</span><span class="sxs-lookup"><span data-stu-id="c4f90-124">The CSS property name to use for the fallback test.</span></span> <span data-ttu-id="c4f90-125">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty> 。</span><span class="sxs-lookup"><span data-stu-id="c4f90-125">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>.</span></span>

### <a name="asp-fallback-test-value"></a><span data-ttu-id="c4f90-126">asp-fallback-測試-值</span><span class="sxs-lookup"><span data-stu-id="c4f90-126">asp-fallback-test-value</span></span>

<span data-ttu-id="c4f90-127">要用於 fallback 測試的 CSS 屬性值。</span><span class="sxs-lookup"><span data-stu-id="c4f90-127">The CSS property value to use for the fallback test.</span></span> <span data-ttu-id="c4f90-128">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue> 。</span><span class="sxs-lookup"><span data-stu-id="c4f90-128">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="c4f90-129">其他資源</span><span class="sxs-lookup"><span data-stu-id="c4f90-129">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
