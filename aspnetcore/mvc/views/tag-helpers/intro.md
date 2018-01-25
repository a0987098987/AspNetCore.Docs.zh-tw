---
title: "在 ASP.NET Core 標記協助程式"
author: rick-anderson
description: "瞭解什麼標記協助程式，以及如何在 ASP.NET Core 中使用它們。"
ms.author: riande
manager: wpickett
ms.date: 7/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: asp.net-core
uid: mvc/views/tag-helpers/intro
ms.custom: H1Hack27Feb2017
ms.openlocfilehash: 3c198ccc3e3e2c11f3e2b9379bc63bd6428dbf69
ms.sourcegitcommit: 060879fcf3f73d2366b5c811986f8695fff65db8
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/24/2018
---
# <a name="introduction-to-tag-helpers-in-aspnet-core"></a><span data-ttu-id="fb10d-103">在 ASP.NET Core 標記協助程式簡介</span><span class="sxs-lookup"><span data-stu-id="fb10d-103">Introduction to Tag Helpers in ASP.NET Core</span></span> 

<span data-ttu-id="fb10d-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT)</span><span class="sxs-lookup"><span data-stu-id="fb10d-104">By [Rick Anderson](https://twitter.com/RickAndMSFT)</span></span>

## <a name="what-are-tag-helpers"></a><span data-ttu-id="fb10d-105">標記協助程式有哪些？</span><span class="sxs-lookup"><span data-stu-id="fb10d-105">What are Tag Helpers?</span></span>

<span data-ttu-id="fb10d-106">標記協助程式可讓伺服器端程式碼參與建立和呈現 HTML Razor 檔案中的項目。</span><span class="sxs-lookup"><span data-stu-id="fb10d-106">Tag Helpers enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="fb10d-107">例如，內建`ImageTagHelper`可以將版本號碼附加至映像名稱。</span><span class="sxs-lookup"><span data-stu-id="fb10d-107">For example, the built-in `ImageTagHelper` can append a version number to the image name.</span></span> <span data-ttu-id="fb10d-108">每當映像變更時，伺服器會產生新的唯一版本映像，讓用戶端保證以取得目前的映像 （而不是過期的快取映像）。</span><span class="sxs-lookup"><span data-stu-id="fb10d-108">Whenever the image changes, the server generates a new unique version for the image, so clients are guaranteed to get the current image (instead of a stale cached image).</span></span> <span data-ttu-id="fb10d-109">有許多內建的標記協助程式的一般工作-例如建立表單、 連結、 載入資產和更多的和更多可用公用 GitHub 儲存機制中以及 NuGet 封裝。</span><span class="sxs-lookup"><span data-stu-id="fb10d-109">There are many built-in Tag Helpers for common tasks - such as creating forms, links, loading assets and more - and even more available in public GitHub repositories and as NuGet packages.</span></span> <span data-ttu-id="fb10d-110">標記協助程式以 C# 中，撰寫，與它們目標項目名稱、 屬性名稱，或父標記的 HTML 項目。</span><span class="sxs-lookup"><span data-stu-id="fb10d-110">Tag Helpers are authored in C#, and they target HTML elements based on element name, attribute name, or parent tag.</span></span> <span data-ttu-id="fb10d-111">例如，內建`LabelTagHelper`可鎖定目標 HTML`<label>`項目時`LabelTagHelper`屬性會套用。</span><span class="sxs-lookup"><span data-stu-id="fb10d-111">For example, the built-in `LabelTagHelper` can target the HTML `<label>` element when the `LabelTagHelper` attributes are applied.</span></span> <span data-ttu-id="fb10d-112">如果您已熟悉[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)，標記協助減少 Razor 檢視的 HTML 和 C# 之間的明確轉換。</span><span class="sxs-lookup"><span data-stu-id="fb10d-112">If you're familiar with [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers), Tag Helpers reduce the explicit transitions between HTML and C# in Razor views.</span></span> <span data-ttu-id="fb10d-113">在許多情況下，HTML Helper 提供的替代方式給特定的標記協助程式，但請務必識別標記協助程式不會取代 HTML Helper，並不是標記協助程式的每個 HTML Helper。</span><span class="sxs-lookup"><span data-stu-id="fb10d-113">In many cases, HTML Helpers provide an alternative approach to a specific Tag Helper, but it's important to recognize that Tag Helpers don't replace HTML Helpers and there's not a Tag Helper for each HTML Helper.</span></span> <span data-ttu-id="fb10d-114">[標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。</span><span class="sxs-lookup"><span data-stu-id="fb10d-114">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span>

## <a name="what-tag-helpers-provide"></a><span data-ttu-id="fb10d-115">標記協助程式所提供的內容</span><span class="sxs-lookup"><span data-stu-id="fb10d-115">What Tag Helpers provide</span></span>

<span data-ttu-id="fb10d-116">**HTML 易用的開發經驗**在大部分的情況，Razor 標記使用標記協助程式看起來像標準 HTML。</span><span class="sxs-lookup"><span data-stu-id="fb10d-116">**An HTML-friendly development experience** For the most part, Razor markup using Tag Helpers looks like standard HTML.</span></span> <span data-ttu-id="fb10d-117">熟悉 HTML/CSS/JavaScript 前端設計工具可以編輯 Razor，無需了解 C# Razor 語法。</span><span class="sxs-lookup"><span data-stu-id="fb10d-117">Front-end designers conversant with HTML/CSS/JavaScript can edit Razor without learning C# Razor syntax.</span></span>

<span data-ttu-id="fb10d-118">**豐富的 IntelliSense 環境，以建立 HTML 和 Razor 標記**這種清晰對比，以 HTML Helper，伺服器端建立 Razor 檢視中的標記之前的方法。</span><span class="sxs-lookup"><span data-stu-id="fb10d-118">**A rich IntelliSense environment for creating HTML and Razor markup** This is in sharp contrast to HTML Helpers, the previous approach to server-side creation of markup in Razor views.</span></span> <span data-ttu-id="fb10d-119">[標記協助程式相較於 HTML Helper](#tag-helpers-compared-to-html-helpers)說明詳細的差異。</span><span class="sxs-lookup"><span data-stu-id="fb10d-119">[Tag Helpers compared to HTML Helpers](#tag-helpers-compared-to-html-helpers) explains the differences in more detail.</span></span> <span data-ttu-id="fb10d-120">[標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)說明 IntelliSense 環境。</span><span class="sxs-lookup"><span data-stu-id="fb10d-120">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) explains the IntelliSense environment.</span></span> <span data-ttu-id="fb10d-121">甚至 Razor C# 語法有經驗的開發人員會更有效率使用比撰寫 C# Razor 標記的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb10d-121">Even developers experienced with Razor C# syntax are more productive using Tag Helpers than writing C# Razor markup.</span></span>

<span data-ttu-id="fb10d-122">**讓您更具生產力且能夠產生更強固、 可靠的方式和維護的程式碼使用伺服器上的資訊僅適用於**比方說，在過去在更新映像宗旨已變更的映像名稱，當您變更映像。</span><span class="sxs-lookup"><span data-stu-id="fb10d-122">**A way to make you more productive and able to produce more robust, reliable, and maintainable code using information only available on the server** For example, historically the mantra on updating images was to change the name of the image when you change the image.</span></span> <span data-ttu-id="fb10d-123">影像應該主動快取基於效能考量，除非您變更影像的名稱，就可能會取得過時複本的用戶端。</span><span class="sxs-lookup"><span data-stu-id="fb10d-123">Images should be aggressively cached for performance reasons, and unless you change the name of an image, you risk clients getting a stale copy.</span></span> <span data-ttu-id="fb10d-124">在過去，編輯映像之後，名稱已變更，並需要更新的 web 應用程式中的映像的每個參考。</span><span class="sxs-lookup"><span data-stu-id="fb10d-124">Historically, after an image was edited, the name had to be changed and each reference to the image in the web app needed to be updated.</span></span> <span data-ttu-id="fb10d-125">不只是這非常人工大量，也很容易發生 （您可能遺漏了參考、 不小心輸入錯誤的字串，依此類推） 錯誤內建`ImageTagHelper`可以這樣做為您自動。</span><span class="sxs-lookup"><span data-stu-id="fb10d-125">Not only is this very labor intensive, it's also error prone (you could miss a reference, accidentally enter the wrong string, etc.) The built-in `ImageTagHelper` can do this for you automatically.</span></span> <span data-ttu-id="fb10d-126">`ImageTagHelper`可以將版本附加數字的映像名稱，所以每當映像變更時，伺服器會自動產生新的唯一版本映像。</span><span class="sxs-lookup"><span data-stu-id="fb10d-126">The `ImageTagHelper` can append a version number to the image name, so whenever the image changes, the server automatically generates a new unique version for the image.</span></span> <span data-ttu-id="fb10d-127">用戶端保證取得目前的映像。</span><span class="sxs-lookup"><span data-stu-id="fb10d-127">Clients are guaranteed to get the current image.</span></span> <span data-ttu-id="fb10d-128">使用此健全性和人力節省來自基本上可用`ImageTagHelper`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-128">This robustness and labor savings comes essentially free by using the `ImageTagHelper`.</span></span>

<span data-ttu-id="fb10d-129">大部分的內建的標記協助程式和目標現有的 HTML 元素的元素提供伺服器端的屬性。</span><span class="sxs-lookup"><span data-stu-id="fb10d-129">Most of the built-in Tag Helpers target existing HTML elements and provide server-side attributes for the element.</span></span> <span data-ttu-id="fb10d-130">例如，`<input>`所使用的檢視中的許多項目的*檢視/帳戶*資料夾包含`asp-for`屬性，它會擷取到轉譯的 HTML 與指定的模型屬性的名稱。</span><span class="sxs-lookup"><span data-stu-id="fb10d-130">For example, the `<input>` element used in many of the views in the *Views/Account* folder contains the `asp-for` attribute, which extracts the name of the specified model property into the rendered HTML.</span></span> <span data-ttu-id="fb10d-131">下列的 Razor 標記：</span><span class="sxs-lookup"><span data-stu-id="fb10d-131">The following Razor markup:</span></span>

```cshtml
<label asp-for="Email"></label>
```

<span data-ttu-id="fb10d-132">會產生下列 HTML:</span><span class="sxs-lookup"><span data-stu-id="fb10d-132">Generates the following HTML:</span></span>

```cshtml
<label for="Email">Email</label>
```

<span data-ttu-id="fb10d-133">`asp-for`屬性可由`For`屬性`LabelTagHelper`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-133">The `asp-for` attribute is made available by the `For` property in the `LabelTagHelper`.</span></span> <span data-ttu-id="fb10d-134">請參閱[撰寫標記協助程式](authoring.md)如需詳細資訊。</span><span class="sxs-lookup"><span data-stu-id="fb10d-134">See [Authoring Tag Helpers](authoring.md) for more information.</span></span>

## <a name="managing-tag-helper-scope"></a><span data-ttu-id="fb10d-135">管理標記協助程式範圍</span><span class="sxs-lookup"><span data-stu-id="fb10d-135">Managing Tag Helper scope</span></span>

<span data-ttu-id="fb10d-136">標記協助程式範圍由多種`@addTagHelper`， `@removeTagHelper`，而"！"退出字元。</span><span class="sxs-lookup"><span data-stu-id="fb10d-136">Tag Helpers scope is controlled by a combination of `@addTagHelper`, `@removeTagHelper`, and the "!" opt-out character.</span></span>

<a name="add-helper-label"></a>

### <a name="addtaghelper-makes-tag-helpers-available"></a><span data-ttu-id="fb10d-137">`@addTagHelper`提供標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fb10d-137">`@addTagHelper` makes Tag Helpers available</span></span>

<span data-ttu-id="fb10d-138">如果您建立新 ASP.NET Core web 應用程式名為*AuthoringTagHelpers* （使用無驗證），下列*Views/_ViewImports.cshtml*檔案會加入至您的專案：</span><span class="sxs-lookup"><span data-stu-id="fb10d-138">If you create a new ASP.NET Core web app named *AuthoringTagHelpers* (with no authentication), the following *Views/_ViewImports.cshtml* file will be added to your project:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=2&range=2-3)]

<span data-ttu-id="fb10d-139">`@addTagHelper`指示詞使標記協助程式可供檢視。</span><span class="sxs-lookup"><span data-stu-id="fb10d-139">The `@addTagHelper` directive makes Tag Helpers available to the view.</span></span> <span data-ttu-id="fb10d-140">在此案例中，檢視檔案是*Views/_ViewImports.cshtml*，預設會繼承中的所有檢視檔案*檢視*資料夾和子的目錄; 可用標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb10d-140">In this case, the view file is *Views/_ViewImports.cshtml*, which by default is inherited by all view files in the *Views* folder and sub-directories; making Tag Helpers available.</span></span> <span data-ttu-id="fb10d-141">上述程式碼使用的萬用字元語法 (「\*") 來指定在指定的組件中的所有標記協助程式 (*Microsoft.AspNetCore.Mvc.TagHelpers*) 將可在每個檢視檔案*檢視*目錄或子目錄。</span><span class="sxs-lookup"><span data-stu-id="fb10d-141">The code above uses the wildcard syntax ("\*") to specify that all Tag Helpers in the specified assembly (*Microsoft.AspNetCore.Mvc.TagHelpers*) will be available to every view file in the *Views* directory or sub-directory.</span></span> <span data-ttu-id="fb10d-142">之後的第一個參數`@addTagHelper`指定載入標記協助程式 (我們使用 「\*"的所有標記協助程式)，而第二個參數會指定包含標記協助程式的組件 」 Microsoft.AspNetCore.Mvc.TagHelpers"。</span><span class="sxs-lookup"><span data-stu-id="fb10d-142">The first parameter after `@addTagHelper` specifies the Tag Helpers to load (we are using "\*" for all Tag Helpers), and the second parameter "Microsoft.AspNetCore.Mvc.TagHelpers" specifies the assembly containing the Tag Helpers.</span></span> <span data-ttu-id="fb10d-143">*Microsoft.AspNetCore.Mvc.TagHelpers*是內建 ASP.NET Core 標記協助程式組件。</span><span class="sxs-lookup"><span data-stu-id="fb10d-143">*Microsoft.AspNetCore.Mvc.TagHelpers* is the assembly for the built-in ASP.NET Core Tag Helpers.</span></span>

<span data-ttu-id="fb10d-144">若要公開所有將在此專案中的標記協助程式 (這會建立名為組件*AuthoringTagHelpers*)，您會使用下列：</span><span class="sxs-lookup"><span data-stu-id="fb10d-144">To expose all of the Tag Helpers in this project (which creates an assembly named *AuthoringTagHelpers*), you would use the following:</span></span>

[!code-cshtml[Main](../../../mvc/views/tag-helpers/authoring/sample/AuthoringTagHelpers/src/AuthoringTagHelpers/Views/_ViewImportsCopy.cshtml?highlight=3)]

<span data-ttu-id="fb10d-145">如果您的專案包含`EmailTagHelper`與預設的命名空間 (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，您可以提供標記協助程式的完整限定的名稱 (FQN):</span><span class="sxs-lookup"><span data-stu-id="fb10d-145">If your project contains an `EmailTagHelper` with the default namespace (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), you can provide the fully qualified name (FQN) of the Tag Helper:</span></span>

```cshtml
@using AuthoringTagHelpers
@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.EmailTagHelper, AuthoringTagHelpers
```

<span data-ttu-id="fb10d-146">若要新增的檢視使用 FQN 標記協助程式，您先新增 FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`)，並接著組件名稱 (*AuthoringTagHelpers*)。</span><span class="sxs-lookup"><span data-stu-id="fb10d-146">To add a Tag Helper to a view using an FQN, you first add the FQN (`AuthoringTagHelpers.TagHelpers.EmailTagHelper`), and then the assembly name (*AuthoringTagHelpers*).</span></span> <span data-ttu-id="fb10d-147">大部分的開發人員想要使用 「\*"的萬用字元語法。</span><span class="sxs-lookup"><span data-stu-id="fb10d-147">Most developers prefer to use the  "\*" wildcard syntax.</span></span> <span data-ttu-id="fb10d-148">萬用字元語法可讓您插入萬用字元"\*"FQN 中的尾碼。</span><span class="sxs-lookup"><span data-stu-id="fb10d-148">The wildcard syntax allows you to insert the wildcard character "\*" as the suffix in an FQN.</span></span> <span data-ttu-id="fb10d-149">例如，任何下列指示詞會顯示`EmailTagHelper`:</span><span class="sxs-lookup"><span data-stu-id="fb10d-149">For example, any of the following directives will bring in the `EmailTagHelper`:</span></span>

```cshtml
@addTagHelper AuthoringTagHelpers.TagHelpers.E*, AuthoringTagHelpers
@addTagHelper AuthoringTagHelpers.TagHelpers.Email*, AuthoringTagHelpers
```

<span data-ttu-id="fb10d-150">如先前所述，新增`@addTagHelper`指示詞加入*Views/_ViewImports.cshtml*檔案讓標記協助程式中的所有檢視檔案*檢視*目錄和子目錄。</span><span class="sxs-lookup"><span data-stu-id="fb10d-150">As mentioned previously, adding the `@addTagHelper` directive to the *Views/_ViewImports.cshtml* file makes the Tag Helper available to all view files in the *Views* directory and sub-directories.</span></span> <span data-ttu-id="fb10d-151">您可以使用`@addTagHelper`指示詞中特定的檢視檔案，如果您想要選擇加入以公開這些檢視表標記協助專家。</span><span class="sxs-lookup"><span data-stu-id="fb10d-151">You can use the `@addTagHelper` directive in specific view files if you want to opt-in to exposing the Tag Helper to only those views.</span></span>

<a name="remove-razor-directives-label"></a>

### <a name="removetaghelper-removes-tag-helpers"></a><span data-ttu-id="fb10d-152">`@removeTagHelper`移除標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fb10d-152">`@removeTagHelper` removes Tag Helpers</span></span>

<span data-ttu-id="fb10d-153">`@removeTagHelper`具有相同的兩個參數，做為`@addTagHelper`，並且會移除先前新增標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb10d-153">The `@removeTagHelper` has the same two parameters as `@addTagHelper`, and it removes a Tag Helper that was previously added.</span></span> <span data-ttu-id="fb10d-154">例如，`@removeTagHelper`從檢視套用至特定檢視移除指定的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb10d-154">For example, `@removeTagHelper` applied to a specific view removes the specified Tag Helper from the view.</span></span> <span data-ttu-id="fb10d-155">使用`@removeTagHelper`中*Views/Folder/_ViewImports.cshtml*檔案從所有檢視中都移除指定的標記協助程式*資料夾*。</span><span class="sxs-lookup"><span data-stu-id="fb10d-155">Using `@removeTagHelper` in a *Views/Folder/_ViewImports.cshtml* file removes the specified Tag Helper from all of the views in *Folder*.</span></span>

### <a name="controlling-tag-helper-scope-with-the-viewimportscshtml-file"></a><span data-ttu-id="fb10d-156">控制標記協助程式範圍*_ViewImports.cshtml*檔案</span><span class="sxs-lookup"><span data-stu-id="fb10d-156">Controlling Tag Helper scope with the *_ViewImports.cshtml* file</span></span>

<span data-ttu-id="fb10d-157">您可以加入*_ViewImports.cshtml*任何檢視資料夾中，檢視引擎要套用至和指示詞從這兩個檔案和*Views/_ViewImports.cshtml*檔案。</span><span class="sxs-lookup"><span data-stu-id="fb10d-157">You can add a *_ViewImports.cshtml* to any view folder, and the view engine applies the directives from both that file and the *Views/_ViewImports.cshtml* file.</span></span> <span data-ttu-id="fb10d-158">如果您加入空白*Views/Home/_ViewImports.cshtml*檔，以取得*首頁*檢視，會有任何變更因為*_ViewImports.cshtml*檔案是加總。</span><span class="sxs-lookup"><span data-stu-id="fb10d-158">If you added an empty *Views/Home/_ViewImports.cshtml* file for the *Home* views, there would be no change because the *_ViewImports.cshtml* file is additive.</span></span> <span data-ttu-id="fb10d-159">任何`@addTagHelper`指示詞加入*Views/Home/_ViewImports.cshtml*檔案 (不在預設*Views/_ViewImports.cshtml*檔案) 會公開至檢視這些標記協助程式只能在*首頁*資料夾。</span><span class="sxs-lookup"><span data-stu-id="fb10d-159">Any `@addTagHelper` directives you add to the *Views/Home/_ViewImports.cshtml* file (that are not in the default *Views/_ViewImports.cshtml* file) would expose those Tag Helpers to views only in the *Home* folder.</span></span>

<a name="opt-out"></a>

### <a name="opting-out-of-individual-elements"></a><span data-ttu-id="fb10d-160">選擇退出個別項目</span><span class="sxs-lookup"><span data-stu-id="fb10d-160">Opting out of individual elements</span></span>

<span data-ttu-id="fb10d-161">您可以停用在項目層級，以標記協助程式退出字元標記協助程式 ("！")。</span><span class="sxs-lookup"><span data-stu-id="fb10d-161">You can disable a Tag Helper at the element level with the Tag Helper opt-out character ("!").</span></span> <span data-ttu-id="fb10d-162">例如，`Email`中已停用驗證`<span>`標記協助程式退出字元：</span><span class="sxs-lookup"><span data-stu-id="fb10d-162">For example, `Email` validation is disabled in the `<span>` with the Tag Helper opt-out character:</span></span>

```cshtml
<!span asp-validation-for="Email" class="text-danger"></!span>
```

<span data-ttu-id="fb10d-163">您必須套用標記協助程式退出字元的開頭和結尾標記。</span><span class="sxs-lookup"><span data-stu-id="fb10d-163">You must apply the Tag Helper opt-out character to the opening and closing tag.</span></span> <span data-ttu-id="fb10d-164">（在 Visual Studio 編輯器中自動將退出字元加入結尾標記，當您加入一個開頭標記）。</span><span class="sxs-lookup"><span data-stu-id="fb10d-164">(The Visual Studio editor automatically adds the opt-out character to the closing tag when you add one to the opening tag).</span></span> <span data-ttu-id="fb10d-165">加入選擇退出字元之後，此項目和標記協助程式屬性不再顯示在特殊的字型。</span><span class="sxs-lookup"><span data-stu-id="fb10d-165">After you add the opt-out character, the element and Tag Helper attributes are no longer displayed in a distinctive font.</span></span>

<a name="prefix-razor-directives-label"></a>

### <a name="using-taghelperprefix-to-make-tag-helper-usage-explicit"></a><span data-ttu-id="fb10d-166">使用`@tagHelperPrefix`進行明確標記協助程式使用方式</span><span class="sxs-lookup"><span data-stu-id="fb10d-166">Using `@tagHelperPrefix` to make Tag Helper usage explicit</span></span>

<span data-ttu-id="fb10d-167">`@tagHelperPrefix`指示詞可讓您指定要啟用標記協助程式支援，並讓標記協助程式使用明確標記前置詞字串。</span><span class="sxs-lookup"><span data-stu-id="fb10d-167">The `@tagHelperPrefix` directive allows you to specify a tag prefix string to enable Tag Helper support and to make Tag Helper usage explicit.</span></span> <span data-ttu-id="fb10d-168">例如，您可以加入下列標記以*Views/_ViewImports.cshtml*檔案：</span><span class="sxs-lookup"><span data-stu-id="fb10d-168">For example, you could add the following markup to the *Views/_ViewImports.cshtml* file:</span></span>

```cshtml
@tagHelperPrefix th:
```
<span data-ttu-id="fb10d-169">在下列程式碼影像，標記協助程式前置詞設定為`th:`，因此只有在使用前置詞的元素`th:`支援標記協助程式 （啟用標記協助程式的項目會有特殊的字型）。</span><span class="sxs-lookup"><span data-stu-id="fb10d-169">In the code image below, the Tag Helper prefix is set to `th:`, so only those elements using the prefix `th:` support Tag Helpers (Tag Helper-enabled elements have a distinctive font).</span></span> <span data-ttu-id="fb10d-170">`<label>`和`<input>`元素有標記協助程式前置詞且標記協助程式功能，同時`<span>`項目不會。</span><span class="sxs-lookup"><span data-stu-id="fb10d-170">The `<label>` and `<input>` elements have the Tag Helper prefix and are Tag Helper-enabled, while the `<span>` element doesn't.</span></span>

![影像](intro/_static/thp.png)

<span data-ttu-id="fb10d-172">相同的階層規則套用至`@addTagHelper`也適用於`@tagHelperPrefix`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-172">The same hierarchy rules that apply to `@addTagHelper` also apply to `@tagHelperPrefix`.</span></span>

## <a name="intellisense-support-for-tag-helpers"></a><span data-ttu-id="fb10d-173">標記協助程式的 IntelliSense 支援</span><span class="sxs-lookup"><span data-stu-id="fb10d-173">IntelliSense support for Tag Helpers</span></span>

<span data-ttu-id="fb10d-174">當您在 Visual Studio 中建立新的 ASP.NET web 應用程式時，它會加入 NuGet 套件 」 Microsoft.AspNetCore.Razor.Tools"。</span><span class="sxs-lookup"><span data-stu-id="fb10d-174">When you create a new ASP.NET web app in Visual Studio, it adds the NuGet package "Microsoft.AspNetCore.Razor.Tools".</span></span> <span data-ttu-id="fb10d-175">這是封裝會新增標記協助程式工具。</span><span class="sxs-lookup"><span data-stu-id="fb10d-175">This is the package that adds Tag Helper tooling.</span></span>

<span data-ttu-id="fb10d-176">請考慮將寫入 HTML`<label>`項目。</span><span class="sxs-lookup"><span data-stu-id="fb10d-176">Consider writing an HTML `<label>` element.</span></span> <span data-ttu-id="fb10d-177">當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：</span><span class="sxs-lookup"><span data-stu-id="fb10d-177">As soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![影像](intro/_static/label.png)

<span data-ttu-id="fb10d-179">不只取得 HTML 說明，但圖示 (「@" symbol with "<>"下)。</span><span class="sxs-lookup"><span data-stu-id="fb10d-179">Not only do you get HTML help, but the icon (the "@" symbol with "<>" under it).</span></span>

![影像](intro/_static/tagSym.png)

<span data-ttu-id="fb10d-181">識別項目，為標記協助程式的目標。</span><span class="sxs-lookup"><span data-stu-id="fb10d-181">identifies the element as targeted by Tag Helpers.</span></span> <span data-ttu-id="fb10d-182">純的 HTML 項目 (例如`fieldset`) 顯示 「 <> 」 圖示。</span><span class="sxs-lookup"><span data-stu-id="fb10d-182">Pure HTML elements (such as the `fieldset`) display the "<>" icon.</span></span>

<span data-ttu-id="fb10d-183">純 HTML`<label>`標記棕色字型的屬性，以紅色顯示 （使用預設 Visual Studio 色彩佈景主題） 的 HTML 標記和屬性值以藍色。</span><span class="sxs-lookup"><span data-stu-id="fb10d-183">A pure HTML `<label>` tag displays the HTML tag (with the default Visual Studio color theme) in a brown font, the attributes in red, and the attribute values in blue.</span></span>

![影像](intro/_static/LableHtmlTag.png)

<span data-ttu-id="fb10d-185">您輸入之後`<label`，IntelliSense 就會列出可用的 HTML/CSS 屬性和標記協助程式為目標屬性：</span><span class="sxs-lookup"><span data-stu-id="fb10d-185">After you enter `<label`, IntelliSense lists the available HTML/CSS attributes and the Tag Helper-targeted attributes:</span></span>

![影像](intro/_static/labelattr.png)

<span data-ttu-id="fb10d-187">IntelliSense 陳述式完成可讓您輸入 tab 鍵來完成選取的值的陳述式：</span><span class="sxs-lookup"><span data-stu-id="fb10d-187">IntelliSense statement completion allows you to enter the tab key to complete the statement with the selected value:</span></span>

![影像](intro/_static/stmtcomplete.png)

<span data-ttu-id="fb10d-189">輸入標記協助程式屬性，因為變更標記和屬性的字型。</span><span class="sxs-lookup"><span data-stu-id="fb10d-189">As soon as a Tag Helper attribute is entered, the tag and attribute fonts change.</span></span> <span data-ttu-id="fb10d-190">使用預設 Visual Studio"Blue"或"Light"色彩佈景主題，字型為粗體紫色。</span><span class="sxs-lookup"><span data-stu-id="fb10d-190">Using the default Visual Studio "Blue" or "Light" color theme, the font is bold purple.</span></span> <span data-ttu-id="fb10d-191">如果您使用 「 暗色調 」 佈景主題的字型是粗體藍綠色。</span><span class="sxs-lookup"><span data-stu-id="fb10d-191">If you're using the "Dark" theme the font is bold teal.</span></span> <span data-ttu-id="fb10d-192">使用預設佈景主題取得這份文件中的影像。</span><span class="sxs-lookup"><span data-stu-id="fb10d-192">The images in this document were taken using the default theme.</span></span>

![影像](intro/_static/labelaspfor2.png)

<span data-ttu-id="fb10d-194">您可以輸入 Visual Studio *CompleteWord*快顯 (Ctrl + 空格鍵是[預設](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio)置於雙引號內 ("")，而您現在在 C# 中，就像您會在 C# 類別。</span><span class="sxs-lookup"><span data-stu-id="fb10d-194">You can enter the Visual Studio *CompleteWord* shortcut (Ctrl +spacebar is the [default](https://docs.microsoft.com/visualstudio/ide/default-keyboard-shortcuts-in-visual-studio) inside the double quotes (""), and you are now in C#, just like you would be in a C# class.</span></span> <span data-ttu-id="fb10d-195">IntelliSense 會顯示頁面模型上所有的方法和屬性。</span><span class="sxs-lookup"><span data-stu-id="fb10d-195">IntelliSense displays all the methods and properties on the page model.</span></span> <span data-ttu-id="fb10d-196">方法和屬性可用的屬性類型，所以`ModelExpression`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-196">The methods and properties are available because the property type is `ModelExpression`.</span></span> <span data-ttu-id="fb10d-197">在下列影像中，我編輯`Register` 檢視中，所以`RegisterViewModel`可用。</span><span class="sxs-lookup"><span data-stu-id="fb10d-197">In the image below, I'm editing the `Register` view, so the `RegisterViewModel` is available.</span></span>

![影像](intro/_static/intellemail.png)

<span data-ttu-id="fb10d-199">IntelliSense 會列出的屬性和方法在頁面上的模型。</span><span class="sxs-lookup"><span data-stu-id="fb10d-199">IntelliSense lists the properties and methods available to the model on the page.</span></span> <span data-ttu-id="fb10d-200">豐富的 IntelliSense 環境，可協助您選取的 CSS 類別：</span><span class="sxs-lookup"><span data-stu-id="fb10d-200">The rich IntelliSense environment helps you select the CSS class:</span></span>

![影像](intro/_static/iclass.png)

![影像](intro/_static/intel3.png)

## <a name="tag-helpers-compared-to-html-helpers"></a><span data-ttu-id="fb10d-203">相較於 HTML Helper 的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fb10d-203">Tag Helpers compared to HTML Helpers</span></span>

<span data-ttu-id="fb10d-204">標記協助程式附加至 HTML 元素在 Razor 檢視中，雖然[HTML Helper](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers)會叫用的方法與 HTML 顛倒 Razor 檢視中。</span><span class="sxs-lookup"><span data-stu-id="fb10d-204">Tag Helpers attach to HTML elements in Razor views, while [HTML Helpers](http://stephenwalther.com/archive/2009/03/03/chapter-6-understanding-html-helpers) are invoked as methods interspersed with HTML in Razor views.</span></span> <span data-ttu-id="fb10d-205">請考慮下列的 Razor 標記 CSS 類別 「 標題 」 建立的 HTML 標籤：</span><span class="sxs-lookup"><span data-stu-id="fb10d-205">Consider the following Razor markup, which creates an HTML label with the CSS class "caption":</span></span>

```cshtml
@Html.Label("FirstName", "First Name:", new {@class="caption"})
```

<span data-ttu-id="fb10d-206">在 (`@`) 符號會告知 Razor 這是程式碼啟動。</span><span class="sxs-lookup"><span data-stu-id="fb10d-206">The at (`@`) symbol tells Razor this is the start of code.</span></span> <span data-ttu-id="fb10d-207">下面兩個參數 ("FirstName"和"名字:") 是字串，因此[IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense)無法幫助。</span><span class="sxs-lookup"><span data-stu-id="fb10d-207">The next two parameters ("FirstName" and "First Name:") are strings, so [IntelliSense](https://docs.microsoft.com/visualstudio/ide/using-intellisense) can't help.</span></span> <span data-ttu-id="fb10d-208">最後的引數：</span><span class="sxs-lookup"><span data-stu-id="fb10d-208">The last argument:</span></span>

```cshtml
new {@class="caption"}
```

<span data-ttu-id="fb10d-209">用來代表屬性的匿名物件。</span><span class="sxs-lookup"><span data-stu-id="fb10d-209">Is an anonymous object used to represent attributes.</span></span> <span data-ttu-id="fb10d-210">因為**類別**是保留的關鍵字在 C# 中，使用`@`符號，以強制執行 C# 來解譯"@class="做為符號 （屬性名稱）。</span><span class="sxs-lookup"><span data-stu-id="fb10d-210">Because **class** is a reserved keyword in C#, you use the `@` symbol to force C# to interpret "@class=" as a symbol (property name).</span></span> <span data-ttu-id="fb10d-211">前端的設計工具 (人熟悉 HTML/CSS/JavaScript 和其他用戶端技術，但不是熟悉 C# 和 Razor)，大部分的行是外部角色。</span><span class="sxs-lookup"><span data-stu-id="fb10d-211">To a front-end designer (someone familiar with HTML/CSS/JavaScript and other client technologies but not familiar with C# and Razor), most of the line is foreign.</span></span> <span data-ttu-id="fb10d-212">有沒有來自 IntelliSense 的說明，必須撰寫的整行。</span><span class="sxs-lookup"><span data-stu-id="fb10d-212">The entire line must be authored with no help from IntelliSense.</span></span>

<span data-ttu-id="fb10d-213">使用`LabelTagHelper`，相同的標記可以寫成：</span><span class="sxs-lookup"><span data-stu-id="fb10d-213">Using the `LabelTagHelper`, the same markup can be written as:</span></span>

![影像](intro/_static/label2.png)

<span data-ttu-id="fb10d-215">標記協助程式版本，當您輸入`<l`在 Visual Studio 編輯器中，IntelliSense 會顯示相符項目：</span><span class="sxs-lookup"><span data-stu-id="fb10d-215">With the Tag Helper version, as soon as you enter `<l` in the Visual Studio editor, IntelliSense displays matching elements:</span></span>

![影像](intro/_static/label.png)

<span data-ttu-id="fb10d-217">IntelliSense 可協助您撰寫的整行。</span><span class="sxs-lookup"><span data-stu-id="fb10d-217">IntelliSense helps you write the entire line.</span></span> <span data-ttu-id="fb10d-218">`LabelTagHelper`也預設為設定的內容`asp-for`到 「 名字 」; 屬性值 ("FirstName")依照 camel 命名法的大小寫慣例的內容轉換為句子空間，其中每個新的大小寫字母，就會發生在屬性名稱所組成。</span><span class="sxs-lookup"><span data-stu-id="fb10d-218">The `LabelTagHelper` also defaults to setting the content of the `asp-for` attribute value ("FirstName") to "First Name"; It converts camel-cased properties to a sentence composed of the property name with a space where each new upper-case letter occurs.</span></span> <span data-ttu-id="fb10d-219">在下列標記：</span><span class="sxs-lookup"><span data-stu-id="fb10d-219">In the following markup:</span></span>

![影像](intro/_static/label2.png)

<span data-ttu-id="fb10d-221">會產生：</span><span class="sxs-lookup"><span data-stu-id="fb10d-221">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">First Name</label>
```

<span data-ttu-id="fb10d-222">如果您將內容加入到依照 camel 命名法的大小寫慣例使用句子大小寫的內容不使用`<label>`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-222">The camel-cased to sentence-cased content isn't used if you add content to the `<label>`.</span></span> <span data-ttu-id="fb10d-223">例如: </span><span class="sxs-lookup"><span data-stu-id="fb10d-223">For example:</span></span>

![影像](intro/_static/1stName.png)

<span data-ttu-id="fb10d-225">會產生：</span><span class="sxs-lookup"><span data-stu-id="fb10d-225">generates:</span></span>

```cshtml
<label class="caption" for="FirstName">Name First</label>
```

<span data-ttu-id="fb10d-226">下圖的程式碼顯示的表單一部分*Views/Account/Register.cshtml* Razor 檢視舊版 ASP.NET 4.5.x MVC 範本產生隨附於 Visual Studio 2015。</span><span class="sxs-lookup"><span data-stu-id="fb10d-226">The following code image shows the Form portion of the *Views/Account/Register.cshtml* Razor view generated from the legacy ASP.NET 4.5.x MVC template included with Visual Studio 2015.</span></span>

![影像](intro/_static/regCS.png)

<span data-ttu-id="fb10d-228">Visual Studio 編輯器顯示 C# 程式碼以灰色背景。</span><span class="sxs-lookup"><span data-stu-id="fb10d-228">The Visual Studio editor displays C# code with a grey background.</span></span> <span data-ttu-id="fb10d-229">例如， `AntiForgeryToken` HTML Helper:</span><span class="sxs-lookup"><span data-stu-id="fb10d-229">For example, the `AntiForgeryToken` HTML Helper:</span></span>

```cshtml
@Html.AntiForgeryToken()
```

<span data-ttu-id="fb10d-230">即會顯示灰色背景。</span><span class="sxs-lookup"><span data-stu-id="fb10d-230">is displayed with a grey background.</span></span> <span data-ttu-id="fb10d-231">大部分的登錄檢視中標記為 C#。</span><span class="sxs-lookup"><span data-stu-id="fb10d-231">Most of the markup in the Register view is C#.</span></span> <span data-ttu-id="fb10d-232">使用標記協助程式的對等方法來比較：</span><span class="sxs-lookup"><span data-stu-id="fb10d-232">Compare that to the equivalent approach using Tag Helpers:</span></span>

![影像](intro/_static/regTH.png)

<span data-ttu-id="fb10d-234">標記是更簡潔且更容易讀取、 編輯和維護的 HTML Helper 方法。</span><span class="sxs-lookup"><span data-stu-id="fb10d-234">The markup is much cleaner and easier to read, edit, and maintain than the HTML Helpers approach.</span></span> <span data-ttu-id="fb10d-235">C# 程式碼會降低該伺服器需要了解最小值。</span><span class="sxs-lookup"><span data-stu-id="fb10d-235">The C# code is reduced to the minimum that the server needs to know about.</span></span> <span data-ttu-id="fb10d-236">Visual Studio 編輯器顯示標記以特殊的字型標記協助程式的目標。</span><span class="sxs-lookup"><span data-stu-id="fb10d-236">The Visual Studio editor displays markup targeted by a Tag Helper in a distinctive font.</span></span>

<span data-ttu-id="fb10d-237">請考慮*電子郵件*群組：</span><span class="sxs-lookup"><span data-stu-id="fb10d-237">Consider the *Email* group:</span></span>

[!code-csharp[Main](intro/sample/Register.cshtml?range=12-18)]

<span data-ttu-id="fb10d-238">每個 「 asp-」 屬性的值為"Email"，但是"Email"不是字串。</span><span class="sxs-lookup"><span data-stu-id="fb10d-238">Each of the "asp-" attributes has a value of "Email", but "Email" isn't a string.</span></span> <span data-ttu-id="fb10d-239">在此內容中，"Email"是 C# 模型運算式屬性`RegisterViewModel`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-239">In this context, "Email" is the C# model expression property for the `RegisterViewModel`.</span></span>

<span data-ttu-id="fb10d-240">在 Visual Studio 編輯器可協助您撰寫**所有**中的標記的註冊表單中，而 Visual Studio 不提供任何說明中的 HTML Helper 方法的程式碼的大部分標記協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="fb10d-240">The Visual Studio editor helps you write **all** of the markup in the Tag Helper approach of the register form, while Visual Studio provides no help for most of the code in the HTML Helpers approach.</span></span> <span data-ttu-id="fb10d-241">[標記協助程式的 IntelliSense 支援](#intellisense-support-for-tag-helpers)會深入探討在使用 Visual Studio 編輯器中的標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="fb10d-241">[IntelliSense support for Tag Helpers](#intellisense-support-for-tag-helpers) goes into detail on working with Tag Helpers in the Visual Studio editor.</span></span>

## <a name="tag-helpers-compared-to-web-server-controls"></a><span data-ttu-id="fb10d-242">相較於 Web 伺服器控制項的標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fb10d-242">Tag Helpers compared to Web Server Controls</span></span>

* <span data-ttu-id="fb10d-243">標記協助程式沒有; 與其相關的項目它們只參與呈現的項目和內容。</span><span class="sxs-lookup"><span data-stu-id="fb10d-243">Tag Helpers don't own the element they're associated with; they simply participate in the rendering of the element and content.</span></span> <span data-ttu-id="fb10d-244">ASP.NET [Web 伺服器控制項](https://msdn.microsoft.com/library/7698y1f0.aspx)宣告和頁面叫用。</span><span class="sxs-lookup"><span data-stu-id="fb10d-244">ASP.NET [Web Server controls](https://msdn.microsoft.com/library/7698y1f0.aspx) are declared and invoked on a page.</span></span>

* <span data-ttu-id="fb10d-245">[Web 伺服器控制項](https://msdn.microsoft.com/library/zsyt68f1.aspx)具有非一般的生命週期可以讓開發及偵錯更加困難。</span><span class="sxs-lookup"><span data-stu-id="fb10d-245">[Web Server controls](https://msdn.microsoft.com/library/zsyt68f1.aspx) have a non-trivial lifecycle that can make developing and debugging difficult.</span></span>

* <span data-ttu-id="fb10d-246">Web 伺服器控制項可讓您使用的用戶端控制項，將功能加入至用戶端文件物件模型 (DOM) 項目。</span><span class="sxs-lookup"><span data-stu-id="fb10d-246">Web Server controls allow you to add functionality to the client Document Object Model (DOM) elements by using a client control.</span></span> <span data-ttu-id="fb10d-247">標記協助程式有沒有 DOM</span><span class="sxs-lookup"><span data-stu-id="fb10d-247">Tag Helpers have no DOM.</span></span>

* <span data-ttu-id="fb10d-248">Web 伺服器控制項包括瀏覽器自動偵測。</span><span class="sxs-lookup"><span data-stu-id="fb10d-248">Web Server controls include automatic browser detection.</span></span> <span data-ttu-id="fb10d-249">標記協助程式不知道的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="fb10d-249">Tag Helpers have no knowledge of the browser.</span></span>

* <span data-ttu-id="fb10d-250">多個標記協助程式就可以處理相同的項目 (請參閱[避免標記協助程式衝突](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts)) 時通常無法撰寫的 Web 伺服器控制項。</span><span class="sxs-lookup"><span data-stu-id="fb10d-250">Multiple Tag Helpers can act on the same element (see [Avoiding Tag Helper conflicts](https://docs.microsoft.com/aspnet/core/mvc/views/tag-helpers/authoring#avoiding-tag-helper-conflicts) ) while you typically can't compose Web Server controls.</span></span>

* <span data-ttu-id="fb10d-251">標記協助程式的標籤和 HTML 項目，它們範圍，內容可以修改，但不直接修改頁面上的其他任何項目。</span><span class="sxs-lookup"><span data-stu-id="fb10d-251">Tag Helpers can modify the tag and content of HTML elements that they're scoped to, but don't directly modify anything else on a page.</span></span> <span data-ttu-id="fb10d-252">Web 伺服器控制項具有更廣泛的範圍，而且可以執行的動作會影響您的網頁; 的其他部分啟用非預期的副作用。</span><span class="sxs-lookup"><span data-stu-id="fb10d-252">Web Server controls have a less specific scope and can perform actions that affect other parts of your page; enabling unintended side effects.</span></span>

* <span data-ttu-id="fb10d-253">Web 伺服器控制項使用類型轉換器，將字串轉換成物件。</span><span class="sxs-lookup"><span data-stu-id="fb10d-253">Web Server controls use type converters to convert strings into objects.</span></span> <span data-ttu-id="fb10d-254">標記協助程式，您可以使用原生方式在 C# 中，因此您不需要進行類型轉換。</span><span class="sxs-lookup"><span data-stu-id="fb10d-254">With Tag Helpers, you work natively in C#, so you don't need to do type conversion.</span></span>

* <span data-ttu-id="fb10d-255">Web 伺服器控制項使用[System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel)來實作元件和控制項的 run-time 和設計階段行為。</span><span class="sxs-lookup"><span data-stu-id="fb10d-255">Web Server controls use [System.ComponentModel](https://docs.microsoft.com/dotnet/api/system.componentmodel) to implement the run-time and design-time behavior of components and controls.</span></span> <span data-ttu-id="fb10d-256">`System.ComponentModel`包含基底類別和介面的實作屬性和型別轉換子、 繫結至資料來源，以及授權元件。</span><span class="sxs-lookup"><span data-stu-id="fb10d-256">`System.ComponentModel` includes the base classes and interfaces for implementing attributes and type converters, binding to data sources, and licensing components.</span></span> <span data-ttu-id="fb10d-257">對比，以標記協助程式，通常衍生自`TagHelper`，而`TagHelper`基底類別會公開只有兩個方法，`Process`和`ProcessAsync`。</span><span class="sxs-lookup"><span data-stu-id="fb10d-257">Contrast that to Tag Helpers, which typically derive from `TagHelper`, and the `TagHelper` base class exposes only two methods, `Process` and `ProcessAsync`.</span></span>

## <a name="customizing-the-tag-helper-element-font"></a><span data-ttu-id="fb10d-258">自訂標記協助程式項目字型</span><span class="sxs-lookup"><span data-stu-id="fb10d-258">Customizing the Tag Helper element font</span></span>

<span data-ttu-id="fb10d-259">您可以自訂字型和顏色標示從**工具** > **選項** > **環境** > **字型和色彩**:</span><span class="sxs-lookup"><span data-stu-id="fb10d-259">You can customize the font and colorization from **Tools** > **Options** > **Environment** > **Fonts and Colors**:</span></span>

![影像](intro/_static/fontoptions2.png)

## <a name="additional-resources"></a><span data-ttu-id="fb10d-261">其他資源</span><span class="sxs-lookup"><span data-stu-id="fb10d-261">Additional Resources</span></span>

* [<span data-ttu-id="fb10d-262">撰寫標記協助程式</span><span class="sxs-lookup"><span data-stu-id="fb10d-262">Authoring Tag Helpers</span></span>](authoring.md)
* [<span data-ttu-id="fb10d-263">使用表單</span><span class="sxs-lookup"><span data-stu-id="fb10d-263">Working with Forms </span></span>](../working-with-forms.md)
* <span data-ttu-id="fb10d-264">[GitHub 上的 TagHelperSamples](https://github.com/dpaquette/TagHelperSamples)含有標記協助程式的範例使用[Bootstrap](http://getbootstrap.com/)。</span><span class="sxs-lookup"><span data-stu-id="fb10d-264">[TagHelperSamples on GitHub](https://github.com/dpaquette/TagHelperSamples) contains Tag Helper samples for working with [Bootstrap](http://getbootstrap.com/).</span></span>

<!--
* [Working with Forms ](xref:mvc/views/working-with-forms)
-->
