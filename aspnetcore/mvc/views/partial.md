---
title: ASP.NET Core 中的部分檢視
author: ardalis
description: 了解如何使用部分檢視來分割大型的標記檔案，並減少 ASP.NET Core 應用程式中跨 Web 網頁一般標記的重複。
ms.author: riande
ms.custom: mvc
ms.date: 09/11/2018
uid: mvc/views/partial
ms.openlocfilehash: a836ed073dfe769fc3cc0cd0622b17937747928b
ms.sourcegitcommit: 70fb7c9d5f2ddfcf4747382a9f7159feca7a6aa7
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/14/2018
ms.locfileid: "45601752"
---
# <a name="partial-views-in-aspnet-core"></a><span data-ttu-id="9d53f-103">ASP.NET Core 中的部分檢視</span><span class="sxs-lookup"><span data-stu-id="9d53f-103">Partial views in ASP.NET Core</span></span>

<span data-ttu-id="9d53f-104">作者：[Steve Smith](https://ardalis.com/)、[Luke Latham](https://github.com/guardrex)、[Maher JENDOUBI](https://twitter.com/maherjend)、[Rick Anderson](https://twitter.com/RickAndMSFT) 和 [Scott Sauber](https://twitter.com/scottsauber)</span><span class="sxs-lookup"><span data-stu-id="9d53f-104">By [Steve Smith](https://ardalis.com/), [Luke Latham](https://github.com/guardrex), [Maher JENDOUBI](https://twitter.com/maherjend), [Rick Anderson](https://twitter.com/RickAndMSFT), and [Scott Sauber](https://twitter.com/scottsauber)</span></span>

<span data-ttu-id="9d53f-105">部分檢視是 [Razor](xref:mvc/views/razor) 標記檔案 (*.cshtml*)，可在另一個標記檔案的轉譯輸出*內*轉譯 HTML 輸出。</span><span class="sxs-lookup"><span data-stu-id="9d53f-105">A partial view is a [Razor](xref:mvc/views/razor) markup file (*.cshtml*) that renders HTML output *within* another markup file's rendered output.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-106">字詞「部分檢視」在開發 MVC 應用程式時使用，其中標記檔案稱為「檢視」；或是 Razor Pages 應用程式，其中標記檔案稱為「頁面」。</span><span class="sxs-lookup"><span data-stu-id="9d53f-106">The term *partial view* is used when developing either an MVC app, where markup files are called *views*, or a Razor Pages app, where markup files are called *pages*.</span></span> <span data-ttu-id="9d53f-107">本主題一般將 MVC 檢視和 Razor Pages 頁面稱為「標記檔案」。</span><span class="sxs-lookup"><span data-stu-id="9d53f-107">This topic generically refers to MVC views and Razor Pages pages as *markup files*.</span></span>

::: moniker-end

<span data-ttu-id="9d53f-108">[檢視或下載範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) \(英文\) ([如何下載](xref:tutorials/index#how-to-download-a-sample))</span><span class="sxs-lookup"><span data-stu-id="9d53f-108">[View or download sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/views/partial/sample) ([how to download](xref:tutorials/index#how-to-download-a-sample))</span></span>

## <a name="when-to-use-partial-views"></a><span data-ttu-id="9d53f-109">使用部分檢視的時機</span><span class="sxs-lookup"><span data-stu-id="9d53f-109">When to use partial views</span></span>

<span data-ttu-id="9d53f-110">針對下列項目，部分檢視是有效的方法：</span><span class="sxs-lookup"><span data-stu-id="9d53f-110">Partial views are an effective way to:</span></span>

* <span data-ttu-id="9d53f-111">將大型標記檔案分解為較小的元件。</span><span class="sxs-lookup"><span data-stu-id="9d53f-111">Break up large markup files into smaller components.</span></span>

  <span data-ttu-id="9d53f-112">在由多個邏輯部分組成的大型複雜標記檔案中，使用隔離至部分檢視中的每個部分具有優勢。</span><span class="sxs-lookup"><span data-stu-id="9d53f-112">In a large, complex markup file composed of several logical pieces, there's an advantage to working with each piece isolated into a partial view.</span></span> <span data-ttu-id="9d53f-113">標記檔案中的程式碼為可管理，因為標記僅包含整體頁面結構和對部分檢視的參考。</span><span class="sxs-lookup"><span data-stu-id="9d53f-113">The code in the markup file is manageable because the markup only contains the overall page structure and references to partial views.</span></span>
* <span data-ttu-id="9d53f-114">減少標記檔案中一般標記內容的重複。</span><span class="sxs-lookup"><span data-stu-id="9d53f-114">Reduce the duplication of common markup content across markup files.</span></span>

  <span data-ttu-id="9d53f-115">當在標記檔案中使用相同的標記項目時，部分檢視會將重複的標記內容移除至某個部分檢視檔案中。</span><span class="sxs-lookup"><span data-stu-id="9d53f-115">When the same markup elements are used across markup files, a partial view removes the duplication of markup content into one partial view file.</span></span> <span data-ttu-id="9d53f-116">在部分檢視中變更標記時，會更新使用部分檢視之標記檔案的轉譯輸出。</span><span class="sxs-lookup"><span data-stu-id="9d53f-116">When the markup is changed in the partial view, it updates the rendered output of the markup files that use the partial view.</span></span>

<span data-ttu-id="9d53f-117">部分檢視不應該用於維護一般版面配置項目。</span><span class="sxs-lookup"><span data-stu-id="9d53f-117">Partial views shouldn't be used to maintain common layout elements.</span></span> <span data-ttu-id="9d53f-118">您應該在 [_Layout.cshtml](xref:mvc/views/layout) 檔案中指定一般版面配置項目。</span><span class="sxs-lookup"><span data-stu-id="9d53f-118">Common layout elements should be specified in [_Layout.cshtml](xref:mvc/views/layout) files.</span></span>

<span data-ttu-id="9d53f-119">請勿使用需要複雜轉譯邏輯或程式碼執行來轉譯標記的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-119">Don't use a partial view where complex rendering logic or code execution is required to render the markup.</span></span> <span data-ttu-id="9d53f-120">請使用[檢視元件](xref:mvc/views/view-components)，而非部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-120">Instead of a partial view, use a [view component](xref:mvc/views/view-components).</span></span>

## <a name="declare-partial-views"></a><span data-ttu-id="9d53f-121">宣告部分檢視</span><span class="sxs-lookup"><span data-stu-id="9d53f-121">Declare partial views</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-122">部分檢視是在「檢視」資料夾 (MVC) 或「頁面」資料夾 (Razor Pages) 中維護的 *.cshtml* 標記檔案。</span><span class="sxs-lookup"><span data-stu-id="9d53f-122">A partial view is a *.cshtml* markup file maintained within the *Views* folder (MVC) or *Pages* folder (Razor Pages).</span></span>

<span data-ttu-id="9d53f-123">在 ASP.NET Core MVC 中，控制器的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 能夠傳回檢視或部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-123">In ASP.NET Core MVC, a controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span> <span data-ttu-id="9d53f-124">已為 ASP.NET Core 2.2 中的 Razor Pages 規劃類似的功能。</span><span class="sxs-lookup"><span data-stu-id="9d53f-124">An analogous capability is planned for Razor Pages in ASP.NET Core 2.2.</span></span> <span data-ttu-id="9d53f-125">在 Razor Pages 中，<xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> 可以傳回 <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-125">In Razor Pages, a <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel> can return a <xref:Microsoft.AspNetCore.Mvc.PartialViewResult>.</span></span> <span data-ttu-id="9d53f-126">[參考部分檢視](#reference-a-partial-view)一節介紹參考和轉譯部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-126">Referencing and rendering partial views is described in the [Reference a partial view](#reference-a-partial-view) section.</span></span>

<span data-ttu-id="9d53f-127">不同於 MVC 檢視或網頁轉譯，部分檢視不會執行 *_ViewStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9d53f-127">Unlike MVC view or page rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="9d53f-128">如需 *_ViewStart.cshtml* 的詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-128">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="9d53f-129">部分檢視檔案名稱通常以底線 (`_`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="9d53f-129">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="9d53f-130">您不一定要依照這項慣例命名，但最好先在視覺呈現上將部分檢視與檢視和頁面區別開來。</span><span class="sxs-lookup"><span data-stu-id="9d53f-130">This naming convention isn't required, but it helps to visually differentiate partial views from views and pages.</span></span> <span data-ttu-id="9d53f-131">當檔案名稱以底線開頭時，即使檔案的標記包含 `@page` 指示詞，Razor Pages 也不會將標記檔案當作 Razor Pages 頁面處理。</span><span class="sxs-lookup"><span data-stu-id="9d53f-131">When the file name starts with an underscore, Razor Pages doesn't process the markup file as a Razor Pages page, even when the file's markup includes the `@page` directive.</span></span>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9d53f-132">部分檢視是在「檢視」資料夾中維護的 *.cshtml* 標記檔案。</span><span class="sxs-lookup"><span data-stu-id="9d53f-132">A partial view is a *.cshtml* markup file maintained within the *Views* folder.</span></span>

<span data-ttu-id="9d53f-133">控制器的 <xref:Microsoft.AspNetCore.Mvc.ViewResult> 能夠傳回檢視或部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-133">A controller's <xref:Microsoft.AspNetCore.Mvc.ViewResult> is capable of returning either a view or a partial view.</span></span>

<span data-ttu-id="9d53f-134">不同於 MVC 檢視轉譯，部分檢視不會執行 *_ViewStart.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="9d53f-134">Unlike MVC view rendering, a partial view doesn't run *_ViewStart.cshtml*.</span></span> <span data-ttu-id="9d53f-135">如需 *_ViewStart.cshtml* 的詳細資訊，請參閱 <xref:mvc/views/layout>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-135">For more information on *_ViewStart.cshtml*, see <xref:mvc/views/layout>.</span></span>

<span data-ttu-id="9d53f-136">部分檢視檔案名稱通常以底線 (`_`) 開頭。</span><span class="sxs-lookup"><span data-stu-id="9d53f-136">Partial view file names often begin with an underscore (`_`).</span></span> <span data-ttu-id="9d53f-137">您不一定要依照這項慣例命名，但最好先在視覺呈現上將部分檢視與檢視區別開來。</span><span class="sxs-lookup"><span data-stu-id="9d53f-137">This naming convention isn't required, but it helps to visually differentiate partial views from views.</span></span>

::: moniker-end

## <a name="reference-a-partial-view"></a><span data-ttu-id="9d53f-138">參考部分檢視</span><span class="sxs-lookup"><span data-stu-id="9d53f-138">Reference a partial view</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-139">在標記檔案中，您可以使用數種方法參考部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-139">Within a markup file, there are several ways to reference a partial view.</span></span> <span data-ttu-id="9d53f-140">我們建議應用程式使用下列其中一個非同步轉譯方法：</span><span class="sxs-lookup"><span data-stu-id="9d53f-140">We recommend that apps use one of the following asynchronous rendering approaches:</span></span>

* [<span data-ttu-id="9d53f-141">部分標記協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-141">Partial Tag Helper</span></span>](#partial-tag-helper)
* [<span data-ttu-id="9d53f-142">非同步 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-142">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)

::: moniker-end

::: moniker range="< aspnetcore-2.1"

<span data-ttu-id="9d53f-143">在標記檔案中，您可以使用兩種方法參考部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-143">Within a markup file, there are two ways to reference a partial view:</span></span>

* [<span data-ttu-id="9d53f-144">非同步 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-144">Asynchronous HTML Helper</span></span>](#asynchronous-html-helper)
* [<span data-ttu-id="9d53f-145">同步 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-145">Synchronous HTML Helper</span></span>](#synchronous-html-helper)

<span data-ttu-id="9d53f-146">我們建議應用程式使用[非同步 HTML 協助程式](#asynchronous-html-helper)。</span><span class="sxs-lookup"><span data-stu-id="9d53f-146">We recommend that apps use the [Asynchronous HTML Helper](#asynchronous-html-helper).</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.1"

### <a name="partial-tag-helper"></a><span data-ttu-id="9d53f-147">Partial 標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-147">Partial Tag Helper</span></span>

<span data-ttu-id="9d53f-148">[部分標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)需要使用 ASP.NET Core 2.1 或更新版本。</span><span class="sxs-lookup"><span data-stu-id="9d53f-148">The [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper) requires ASP.NET Core 2.1 or later.</span></span>

<span data-ttu-id="9d53f-149">部分標籤協助程式會以非同步方式轉譯內容，並使用類似於 HTML 的語法：</span><span class="sxs-lookup"><span data-stu-id="9d53f-149">The Partial Tag Helper renders content asynchronously and uses an HTML-like syntax:</span></span>

```cshtml
<partial name="_PartialName" />
```

<span data-ttu-id="9d53f-150">當存在檔案副檔名時，部分標籤協助程式會參考部分檢視，該檢視必須與呼叫部分檢視的標記檔案位於相同資料夾中：</span><span class="sxs-lookup"><span data-stu-id="9d53f-150">When a file extension is present, the Tag Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
<partial name="_PartialName.cshtml" />
```

<span data-ttu-id="9d53f-151">下列範例會從應用程式根目錄參考部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-151">The following example references a partial view from the app root.</span></span> <span data-ttu-id="9d53f-152">開頭為波狀符號斜線 (`~/`) 或斜線 (`/`) 的路徑表示應用程式根目錄：</span><span class="sxs-lookup"><span data-stu-id="9d53f-152">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

<span data-ttu-id="9d53f-153">**Razor 頁面**</span><span class="sxs-lookup"><span data-stu-id="9d53f-153">**Razor Pages**</span></span>

```cshtml
<partial name="~/Pages/Folder/_PartialName.cshtml" />
<partial name="/Pages/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="9d53f-154">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9d53f-154">**MVC**</span></span>

```cshtml
<partial name="~/Views/Folder/_PartialName.cshtml" />
<partial name="/Views/Folder/_PartialName.cshtml" />
```

<span data-ttu-id="9d53f-155">下列範例會以相對路徑參考部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-155">The following example references a partial view with a relative path:</span></span>

```cshtml
<partial name="../Account/_PartialName.cshtml" />
```

<span data-ttu-id="9d53f-156">如需詳細資訊，請參閱<xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-156">For more information, see <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>.</span></span>

::: moniker-end

### <a name="asynchronous-html-helper"></a><span data-ttu-id="9d53f-157">非同步 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-157">Asynchronous HTML Helper</span></span>

<span data-ttu-id="9d53f-158">使用 HTML 協助程式時，最佳做法是使用 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-158">When using an HTML Helper, the best practice is to use <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.PartialAsync*>.</span></span> <span data-ttu-id="9d53f-159">`PartialAsync` 會傳回包裝在 <xref:System.Threading.Tasks.Task`1> 的 <xref:Microsoft.AspNetCore.Html.IHtmlContent> 類型。</span><span class="sxs-lookup"><span data-stu-id="9d53f-159">`PartialAsync` returns an <xref:Microsoft.AspNetCore.Html.IHtmlContent> type wrapped in a <xref:System.Threading.Tasks.Task`1>.</span></span> <span data-ttu-id="9d53f-160">方法的參考方式，是在等候的呼叫前面加上 `@` 字元：</span><span class="sxs-lookup"><span data-stu-id="9d53f-160">The method is referenced by prefixing the awaited call with an `@` character:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName")
```

<span data-ttu-id="9d53f-161">當存在檔案副檔名時，HTML 協助程式會參考部分檢視，該檢視必須與呼叫部分檢視的標記檔案位於相同資料夾中：</span><span class="sxs-lookup"><span data-stu-id="9d53f-161">When the file extension is present, the HTML Helper references a partial view that must be in the same folder as the markup file calling the partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName.cshtml")
```

<span data-ttu-id="9d53f-162">下列範例會從應用程式根目錄參考部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-162">The following example references a partial view from the app root.</span></span> <span data-ttu-id="9d53f-163">開頭為波狀符號斜線 (`~/`) 或斜線 (`/`) 的路徑表示應用程式根目錄：</span><span class="sxs-lookup"><span data-stu-id="9d53f-163">Paths that start with a tilde-slash (`~/`) or a slash (`/`) refer to the app root:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-164">**Razor 頁面**</span><span class="sxs-lookup"><span data-stu-id="9d53f-164">**Razor Pages**</span></span>

```cshtml
@await Html.PartialAsync("~/Pages/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Pages/Folder/_PartialName.cshtml")
```

<span data-ttu-id="9d53f-165">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9d53f-165">**MVC**</span></span>

::: moniker-end

```cshtml
@await Html.PartialAsync("~/Views/Folder/_PartialName.cshtml")
@await Html.PartialAsync("/Views/Folder/_PartialName.cshtml")
```

<span data-ttu-id="9d53f-166">下列範例會以相對路徑參考部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-166">The following example references a partial view with a relative path:</span></span>

```cshtml
@await Html.PartialAsync("../Account/_LoginPartial.cshtml")
```

<span data-ttu-id="9d53f-167">或者，您可以使用 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*> 轉譯部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-167">Alternatively, you can render a partial view with <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartialAsync*>.</span></span> <span data-ttu-id="9d53f-168">此方法不會傳回 <xref:Microsoft.AspNetCore.Html.IHtmlContent>。</span><span class="sxs-lookup"><span data-stu-id="9d53f-168">This method doesn't return an <xref:Microsoft.AspNetCore.Html.IHtmlContent>.</span></span> <span data-ttu-id="9d53f-169">，而是將轉譯輸出直接串流給回應。</span><span class="sxs-lookup"><span data-stu-id="9d53f-169">It streams the rendered output directly to the response.</span></span> <span data-ttu-id="9d53f-170">因為該方法不會傳回結果，所以您必須在 Razor 程式碼區塊內呼叫它：</span><span class="sxs-lookup"><span data-stu-id="9d53f-170">Because the method doesn't return a result, it must be called within a Razor code block:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Home/Discovery.cshtml?name=snippet_RenderPartialAsync)]

<span data-ttu-id="9d53f-171">由於 `RenderPartialAsync` 會串流轉譯的內容，因此在某些情況下可提供更好的效能。</span><span class="sxs-lookup"><span data-stu-id="9d53f-171">Since `RenderPartialAsync` streams rendered content, it provides better performance in some scenarios.</span></span> <span data-ttu-id="9d53f-172">在效能十分重要的情況下，請使用這兩種方法對頁面進行效能評定，並使用可產生更快速回應的方法。</span><span class="sxs-lookup"><span data-stu-id="9d53f-172">In performance-critical situations, benchmark the page using both approaches and use the approach that generates a faster response.</span></span>

### <a name="synchronous-html-helper"></a><span data-ttu-id="9d53f-173">同步 HTML 協助程式</span><span class="sxs-lookup"><span data-stu-id="9d53f-173">Synchronous HTML Helper</span></span>

<span data-ttu-id="9d53f-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> 和 <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> 分別是 `PartialAsync` 和 `RenderPartialAsync` 的同步對等方法。</span><span class="sxs-lookup"><span data-stu-id="9d53f-174"><xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.Partial*> and <xref:Microsoft.AspNetCore.Mvc.Rendering.HtmlHelperPartialExtensions.RenderPartial*> are the synchronous equivalents of `PartialAsync` and `RenderPartialAsync`, respectively.</span></span> <span data-ttu-id="9d53f-175">不建議使用同步對等，原因是會出現這些方法發生死結的情況。</span><span class="sxs-lookup"><span data-stu-id="9d53f-175">The synchronous equivalents aren't recommended because there are scenarios in which they deadlock.</span></span> <span data-ttu-id="9d53f-176">同步方法將於未來版本中移除。</span><span class="sxs-lookup"><span data-stu-id="9d53f-176">The synchronous methods are targeted for removal in a future release.</span></span>

> [!IMPORTANT]
> <span data-ttu-id="9d53f-177">如果您需要執行程式碼，請使用[檢視元件](xref:mvc/views/view-components)，而非部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-177">If you need to execute code, use a [view component](xref:mvc/views/view-components) instead of a partial view.</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-178">呼叫 `Partial` 或 `RenderPartial` 會產生 Visual Studio Analyzer 警告。</span><span class="sxs-lookup"><span data-stu-id="9d53f-178">Calling `Partial` or `RenderPartial` results in a Visual Studio analyzer warning.</span></span> <span data-ttu-id="9d53f-179">例如，`Partial` 的存在會產生下列警告訊息：</span><span class="sxs-lookup"><span data-stu-id="9d53f-179">For example, the presence of `Partial` yields the following warning message:</span></span>

> <span data-ttu-id="9d53f-180">使用 IHtmlHelper.Partial 可能會導致應用程式死結。</span><span class="sxs-lookup"><span data-stu-id="9d53f-180">Use of IHtmlHelper.Partial may result in application deadlocks.</span></span> <span data-ttu-id="9d53f-181">請考慮使用&lt;部分&gt;標籤協助程式或 IHtmlHelper.PartialAsync。</span><span class="sxs-lookup"><span data-stu-id="9d53f-181">Consider using &lt;partial&gt; Tag Helper or IHtmlHelper.PartialAsync.</span></span>

<span data-ttu-id="9d53f-182">將對 `@Html.Partial` 的呼叫取代為對 `@await Html.PartialAsync` 的呼叫或[部分標籤協助程式](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper)。</span><span class="sxs-lookup"><span data-stu-id="9d53f-182">Replace calls to `@Html.Partial` with `@await Html.PartialAsync` or the [Partial Tag Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper).</span></span> <span data-ttu-id="9d53f-183">如需 Partial 標籤協助程式移轉的詳細資訊，請參閱[從 HTML 協助程式移轉](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper)。</span><span class="sxs-lookup"><span data-stu-id="9d53f-183">For more information on Partial Tag Helper migration, see [Migrate from an HTML Helper](xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper#migrate-from-an-html-helper).</span></span>

::: moniker-end

## <a name="partial-view-discovery"></a><span data-ttu-id="9d53f-184">部分檢視探索</span><span class="sxs-lookup"><span data-stu-id="9d53f-184">Partial view discovery</span></span>

<span data-ttu-id="9d53f-185">如果以不含檔案副檔名的名稱參考部分檢視，則會依所述順序搜尋下列位置：</span><span class="sxs-lookup"><span data-stu-id="9d53f-185">When a partial view is referenced by name without a file extension, the following locations are searched in the stated order:</span></span>

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-186">**Razor 頁面**</span><span class="sxs-lookup"><span data-stu-id="9d53f-186">**Razor Pages**</span></span>

1. <span data-ttu-id="9d53f-187">目前正在執行頁面的資料夾</span><span class="sxs-lookup"><span data-stu-id="9d53f-187">Currently executing page's folder</span></span>
1. <span data-ttu-id="9d53f-188">頁面資料夾上方的目錄圖表</span><span class="sxs-lookup"><span data-stu-id="9d53f-188">Directory graph above the page's folder</span></span>
1. `/Shared`
1. `/Pages/Shared`
1. `/Views/Shared`

<span data-ttu-id="9d53f-189">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9d53f-189">**MVC**</span></span>

::: moniker-end

::: moniker range=">= aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`
1. `/Pages/Shared`

::: moniker-end

::: moniker range="< aspnetcore-2.0"

1. `/Areas/<Area-Name>/Views/<Controller-Name>`
1. `/Areas/<Area-Name>/Views/Shared`
1. `/Views/Shared`

::: moniker-end

<span data-ttu-id="9d53f-190">部分檢視探索適用於下列慣例：</span><span class="sxs-lookup"><span data-stu-id="9d53f-190">The following conventions apply to partial view discovery:</span></span>

* <span data-ttu-id="9d53f-191">當部分檢視位於不同的資料夾中時，可允許使用具有相同檔案名稱的不同部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-191">Different partial views with the same file name are allowed when the partial views are in different folders.</span></span>
* <span data-ttu-id="9d53f-192">當以不含檔案副檔名的名稱參考部分檢視，且部分檢視出現在呼叫者的資料夾和*共用*資料夾中時，呼叫者資料夾中的部分檢視會提供部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-192">When referencing a partial view by name without a file extension and the partial view is present in both the caller's folder and the *Shared* folder, the partial view in the caller's folder supplies the partial view.</span></span> <span data-ttu-id="9d53f-193">如果呼叫者資料夾中不存在部分檢視，則會從「共用」資料夾中提供部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-193">If the partial view isn't present in the caller's folder, the partial view is provided from the *Shared* folder.</span></span> <span data-ttu-id="9d53f-194">「共用」資料夾中的部分檢視稱為「共用部分檢視」或「預設部分檢視」。</span><span class="sxs-lookup"><span data-stu-id="9d53f-194">Partial views in the *Shared* folder are called *shared partial views* or *default partial views*.</span></span>
* <span data-ttu-id="9d53f-195">可鏈結部分檢視&mdash;如果呼叫未形成循環參考，則部分檢視可以呼叫另一個部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-195">Partial views can be *chained*&mdash;a partial view can call another partial view if a circular reference isn't formed by the calls.</span></span> <span data-ttu-id="9d53f-196">相對路徑一律相對於目前的檔案，而不是相對於檔案的根目錄或父檔案。</span><span class="sxs-lookup"><span data-stu-id="9d53f-196">Relative paths are always relative to the current file, not to the root or parent of the file.</span></span>

> [!NOTE]
> <span data-ttu-id="9d53f-197">父標記檔案不會顯示在部分視圖中定義的 [Razor](xref:mvc/views/razor) `section`。</span><span class="sxs-lookup"><span data-stu-id="9d53f-197">A [Razor](xref:mvc/views/razor) `section` defined in a partial view is invisible to parent markup files.</span></span> <span data-ttu-id="9d53f-198">`section` 只會顯示在具有其定義的部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-198">The `section` is only visible to the partial view in which it's defined.</span></span>

## <a name="access-data-from-partial-views"></a><span data-ttu-id="9d53f-199">從部分檢視存取資料</span><span class="sxs-lookup"><span data-stu-id="9d53f-199">Access data from partial views</span></span>

<span data-ttu-id="9d53f-200">將部分檢視具現化時，會收到父檢視 `ViewData` 字典的*複本*。</span><span class="sxs-lookup"><span data-stu-id="9d53f-200">When a partial view is instantiated, it receives a *copy* of the parent's `ViewData` dictionary.</span></span> <span data-ttu-id="9d53f-201">父檢視不會保存部分檢視內的資料更新。</span><span class="sxs-lookup"><span data-stu-id="9d53f-201">Updates made to the data within the partial view aren't persisted to the parent view.</span></span> <span data-ttu-id="9d53f-202">傳回部分檢視時，部分檢視內的 `ViewData` 變更會遺失。</span><span class="sxs-lookup"><span data-stu-id="9d53f-202">`ViewData` changes in a partial view are lost when the partial view returns.</span></span>

<span data-ttu-id="9d53f-203">下列範例示範如何將 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 的執行個體傳遞給部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-203">The following example demonstrates how to pass an instance of [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) to a partial view:</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", customViewData)
```

<span data-ttu-id="9d53f-204">您可以將模型傳入部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-204">You can pass a model into a partial view.</span></span> <span data-ttu-id="9d53f-205">模型可以是自訂物件。</span><span class="sxs-lookup"><span data-stu-id="9d53f-205">The model can be a custom object.</span></span> <span data-ttu-id="9d53f-206">您可以使用 `PartialAsync` (對呼叫者呈現內容區塊) 或 `RenderPartialAsync` (將內容串流至輸出) 來傳遞模型：</span><span class="sxs-lookup"><span data-stu-id="9d53f-206">You can pass a model with `PartialAsync` (renders a block of content to the caller) or `RenderPartialAsync` (streams the content to the output):</span></span>

```cshtml
@await Html.PartialAsync("_PartialName", model)
```

::: moniker range=">= aspnetcore-2.1"

<span data-ttu-id="9d53f-207">**Razor 頁面**</span><span class="sxs-lookup"><span data-stu-id="9d53f-207">**Razor Pages**</span></span>

<span data-ttu-id="9d53f-208">範例應用程式中的下列標記來自 *Pages/ArticlesRP/ReadRP.cshtml* 頁面。</span><span class="sxs-lookup"><span data-stu-id="9d53f-208">The following markup in the sample app is from the *Pages/ArticlesRP/ReadRP.cshtml* page.</span></span> <span data-ttu-id="9d53f-209">此頁面包含兩個部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-209">The page contains two partial views.</span></span> <span data-ttu-id="9d53f-210">第二個部分檢視將模型和 `ViewData` 傳入部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-210">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="9d53f-211">`ViewDataDictionary` 建構函式多載用於傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="9d53f-211">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/ReadRP.cshtml?name=snippet_ReadPartialViewRP&highlight=5,15-19)]

<span data-ttu-id="9d53f-212">*Pages/Shared/_AuthorPartialRP.cshtml* 是由 *ReadRP.cshtml* 標記檔案參考的第一個部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-212">*Pages/Shared/_AuthorPartialRP.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/Shared/_AuthorPartialRP.cshtml)]

<span data-ttu-id="9d53f-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* 是由 *ReadRP.cshtml* 標記檔案參考的第二個部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-213">*Pages/ArticlesRP/_ArticleSectionRP.cshtml* is the second partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Pages/ArticlesRP/_ArticleSectionRP.cshtml)]

<span data-ttu-id="9d53f-214">**MVC**</span><span class="sxs-lookup"><span data-stu-id="9d53f-214">**MVC**</span></span>

::: moniker-end

<span data-ttu-id="9d53f-215">範例應用程式中的下列標記顯示 *Views/Articles/Read.cshtml* 檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-215">The following markup in the sample app shows the *Views/Articles/Read.cshtml* view.</span></span> <span data-ttu-id="9d53f-216">此檢視包含兩個部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-216">The view contains two partial views.</span></span> <span data-ttu-id="9d53f-217">第二個部分檢視將模型和 `ViewData` 傳入部分檢視。</span><span class="sxs-lookup"><span data-stu-id="9d53f-217">The second partial view passes in a model and `ViewData` to the partial view.</span></span> <span data-ttu-id="9d53f-218">`ViewDataDictionary` 建構函式多載用於傳遞新的 `ViewData` 字典，同時保留現有的 `ViewData` 字典。</span><span class="sxs-lookup"><span data-stu-id="9d53f-218">The `ViewDataDictionary` constructor overload is used to pass a new `ViewData` dictionary while retaining the existing `ViewData` dictionary.</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/Read.cshtml?name=snippet_ReadPartialView&highlight=5,15-19)]

<span data-ttu-id="9d53f-219">*Views/Shared/_AuthorPartial.cshtml* 是由 *ReadRP.cshtml* 標記檔案參考的第一個部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-219">*Views/Shared/_AuthorPartial.cshtml* is the first partial view referenced by the *ReadRP.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Shared/_AuthorPartial.cshtml)]

<span data-ttu-id="9d53f-220">*Views/Articles/_ArticleSection.cshtml* 是由 *Read.cshtml* 標記檔案參考的第二個部分檢視：</span><span class="sxs-lookup"><span data-stu-id="9d53f-220">*Views/Articles/_ArticleSection.cshtml* is the second partial view referenced by the *Read.cshtml* markup file:</span></span>

[!code-cshtml[](partial/sample/PartialViewsSample/Views/Articles/_ArticleSection.cshtml)]

<span data-ttu-id="9d53f-221">在執行階段期間，會將部分檢視轉譯為父標記檔案的轉譯輸出，而這個父檢視本身則在共用的 *_Layout.cshtml* 中轉譯。</span><span class="sxs-lookup"><span data-stu-id="9d53f-221">At runtime, the partials are rendered into the parent markup file's rendered output, which itself is rendered within the shared *_Layout.cshtml*.</span></span> <span data-ttu-id="9d53f-222">第一個部分檢視會轉譯文章作者的名稱和發行日期：</span><span class="sxs-lookup"><span data-stu-id="9d53f-222">The first partial view renders the article author's name and publication date:</span></span>

> <span data-ttu-id="9d53f-223">Abraham Lincoln</span><span class="sxs-lookup"><span data-stu-id="9d53f-223">Abraham Lincoln</span></span>
>
> <span data-ttu-id="9d53f-224">此部分檢視來自&lt;共用的部分檢視檔案路徑&gt;。</span><span class="sxs-lookup"><span data-stu-id="9d53f-224">This partial view from &lt;shared partial view file path&gt;.</span></span>
> <span data-ttu-id="9d53f-225">1863 年 11 月 19 日上午 12:00:00</span><span class="sxs-lookup"><span data-stu-id="9d53f-225">11/19/1863 12:00:00 AM</span></span>

<span data-ttu-id="9d53f-226">第二個部分檢視會轉譯文章的章節：</span><span class="sxs-lookup"><span data-stu-id="9d53f-226">The second partial view renders the article's sections:</span></span>

> <span data-ttu-id="9d53f-227">第一節索引：0</span><span class="sxs-lookup"><span data-stu-id="9d53f-227">Section One Index: 0</span></span>
>
> <span data-ttu-id="9d53f-228">八十七年前...</span><span class="sxs-lookup"><span data-stu-id="9d53f-228">Four score and seven years ago ...</span></span>
>
> <span data-ttu-id="9d53f-229">第二節索引：1</span><span class="sxs-lookup"><span data-stu-id="9d53f-229">Section Two Index: 1</span></span>
>
> <span data-ttu-id="9d53f-230">如今，我們正在進行一場偉大的內戰，考驗著...</span><span class="sxs-lookup"><span data-stu-id="9d53f-230">Now we are engaged in a great civil war, testing ...</span></span>
>
> <span data-ttu-id="9d53f-231">第三節索引：2</span><span class="sxs-lookup"><span data-stu-id="9d53f-231">Section Three Index: 2</span></span>
>
> <span data-ttu-id="9d53f-232">然而，從更廣泛的意義上說，我們無法在此奉獻...</span><span class="sxs-lookup"><span data-stu-id="9d53f-232">But, in a larger sense, we can not dedicate ...</span></span>

## <a name="additional-resources"></a><span data-ttu-id="9d53f-233">其他資源</span><span class="sxs-lookup"><span data-stu-id="9d53f-233">Additional resources</span></span>

::: moniker range=">= aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/views/tag-helpers/builtin-th/partial-tag-helper>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end

::: moniker range="< aspnetcore-2.1"

* <xref:mvc/views/razor>
* <xref:mvc/views/view-components>
* <xref:mvc/controllers/areas>

::: moniker-end
