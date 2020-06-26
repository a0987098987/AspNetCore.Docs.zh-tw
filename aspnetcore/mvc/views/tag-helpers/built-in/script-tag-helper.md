---
title: ASP.NET Core 中的腳本標記協助程式
author: rick-anderson
ms.author: riande
description: 探索 ASP.NET Core 腳本標記協助程式屬性，以及每個屬性在 HTML 腳本標記的擴充行為中所扮演的角色。
ms.custom: mvc
ms.date: 12/02/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: b9b90c1c40fccbc7bb6b6c9050bd525b5fa8cd92
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85407404"
---
# <a name="script-tag-helper-in-aspnet-core"></a><span data-ttu-id="f97e1-103">ASP.NET Core 中的腳本標記協助程式</span><span class="sxs-lookup"><span data-stu-id="f97e1-103">Script Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="f97e1-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="f97e1-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

<span data-ttu-id="f97e1-105">[腳本標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper)協助程式會產生主要或切換回腳本檔案的連結。</span><span class="sxs-lookup"><span data-stu-id="f97e1-105">The [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) generates a link to a primary or fall back script file.</span></span> <span data-ttu-id="f97e1-106">主要腳本檔案通常位於[內容傳遞網路](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn)（CDN）上。</span><span class="sxs-lookup"><span data-stu-id="f97e1-106">Typically the primary script file is on a [Content Delivery Network](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn) (CDN).</span></span>

[!INCLUDE[](~/includes/cdn.md)]

<span data-ttu-id="f97e1-107">當 CDN 無法使用時，腳本標籤協助程式可讓您指定腳本檔案的 CDN 和回退。</span><span class="sxs-lookup"><span data-stu-id="f97e1-107">The Script Tag Helper allows you to specify a CDN for the script file and a fallback when the CDN is not available.</span></span> <span data-ttu-id="f97e1-108">腳本標記協助程式可提供 CDN 的效能優勢，以及本機裝載的穩定性。</span><span class="sxs-lookup"><span data-stu-id="f97e1-108">The Script Tag Helper provides the performance advantage of a CDN with the robustness of local hosting.</span></span>

<span data-ttu-id="f97e1-109">下列 Razor 標記顯示 `script` 具有 fallback 的元素：</span><span class="sxs-lookup"><span data-stu-id="f97e1-109">The following Razor markup shows a `script` element with a fallback:</span></span>

```html
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

<span data-ttu-id="f97e1-110">請勿使用 `<script>` 元素的[defer](https://developer.mozilla.org/docs/Web/HTML/Element/script)屬性來延遲載入 CDN 腳本。</span><span class="sxs-lookup"><span data-stu-id="f97e1-110">Don't use the `<script>` element's [defer](https://developer.mozilla.org/docs/Web/HTML/Element/script) attribute to defer loading the CDN script.</span></span> <span data-ttu-id="f97e1-111">腳本標記協助程式會轉譯 JavaScript，以立即執行[asp-fallback-測試](#asp-fallback-test)運算式。</span><span class="sxs-lookup"><span data-stu-id="f97e1-111">The Script Tag Helper renders JavaScript that immediately executes the [asp-fallback-test](#asp-fallback-test) expression.</span></span> <span data-ttu-id="f97e1-112">如果載入 CDN 腳本已延遲，運算式就會失敗。</span><span class="sxs-lookup"><span data-stu-id="f97e1-112">The expression fails if loading the CDN script is deferred.</span></span>

## <a name="commonly-used-script-tag-helper-attributes"></a><span data-ttu-id="f97e1-113">常用的腳本標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="f97e1-113">Commonly used Script Tag Helper attributes</span></span>

<span data-ttu-id="f97e1-114">如需所有腳本標記協助程式屬性、屬性和方法，請參閱[腳本標記 helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) 。</span><span class="sxs-lookup"><span data-stu-id="f97e1-114">See [Script Tag Helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) for all the Script Tag Helper attributes, properties, and methods.</span></span>

### <a name="asp-fallback-test"></a><span data-ttu-id="f97e1-115">asp-回溯-測試</span><span class="sxs-lookup"><span data-stu-id="f97e1-115">asp-fallback-test</span></span>

<span data-ttu-id="f97e1-116">要用於回溯測試之主要腳本中定義的腳本方法。</span><span class="sxs-lookup"><span data-stu-id="f97e1-116">The script method defined in the primary script to use for the fallback test.</span></span> <span data-ttu-id="f97e1-117">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression> 。</span><span class="sxs-lookup"><span data-stu-id="f97e1-117">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>.</span></span>

### <a name="asp-fallback-src"></a><span data-ttu-id="f97e1-118">asp-fallback-src</span><span class="sxs-lookup"><span data-stu-id="f97e1-118">asp-fallback-src</span></span>

<span data-ttu-id="f97e1-119">當主要複本失敗時，要回復之腳本標記的 URL。</span><span class="sxs-lookup"><span data-stu-id="f97e1-119">The URL of a Script tag to fallback to in the case the primary one fails.</span></span> <span data-ttu-id="f97e1-120">如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc> 。</span><span class="sxs-lookup"><span data-stu-id="f97e1-120">For more information, see <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="f97e1-121">其他資源</span><span class="sxs-lookup"><span data-stu-id="f97e1-121">Additional resources</span></span>

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
