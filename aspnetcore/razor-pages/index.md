---
title: ASP.NET Core 中的 Razor Pages 簡介
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 10/07/2019
uid: razor-pages/index
ms.openlocfilehash: 67cc4f9b261372996d612f922c9f491f53948ece
ms.sourcegitcommit: ddc813f0f1fb293861a01597532919945b0e7fe5
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/23/2019
ms.locfileid: "74412066"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="3859c-103">ASP.NET Core 中的 Razor Pages 簡介</span><span class="sxs-lookup"><span data-stu-id="3859c-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="3859c-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="3859c-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="3859c-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span><span class="sxs-lookup"><span data-stu-id="3859c-105">Razor Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="3859c-106">如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="3859c-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="3859c-107">本文件提供 Razor Pages 簡介。</span><span class="sxs-lookup"><span data-stu-id="3859c-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="3859c-108">它不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="3859c-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="3859c-109">如果您發現某些章節很難遵循，請參閱[9開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="3859c-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="3859c-110">如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="3859c-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3859c-111">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3859c-111">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3859c-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3859c-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3859c-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3859c-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3859c-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3859c-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="3859c-115">建立 Razor Pages 專案</span><span class="sxs-lookup"><span data-stu-id="3859c-115">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3859c-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3859c-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3859c-117">如需如何建立 Razor Pages 專案的詳細說明，請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="3859c-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3859c-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3859c-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3859c-119">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="3859c-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3859c-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3859c-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3859c-121">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="3859c-121">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="3859c-122">從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3859c-122">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="3859c-123">Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="3859c-123">Razor Pages</span></span>

<span data-ttu-id="3859c-124">Razor Pages 是在 *Startup.cs* 中啟用：</span><span class="sxs-lookup"><span data-stu-id="3859c-124">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="3859c-125">請考慮使用基本頁面：<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="3859c-125">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="3859c-126">上述程式碼看起來很像用於 ASP.NET Core 應用程式的 [Razor 檢視檔案](xref:tutorials/first-mvc-app/adding-view)，含有控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="3859c-126">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="3859c-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span><span class="sxs-lookup"><span data-stu-id="3859c-127">What makes it different is the [@page](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="3859c-128">`@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。</span><span class="sxs-lookup"><span data-stu-id="3859c-128">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="3859c-129">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-129">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3859c-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span><span class="sxs-lookup"><span data-stu-id="3859c-130">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> <span data-ttu-id="3859c-131">Razor Pages file names have a *.cshtml* suffix.</span><span class="sxs-lookup"><span data-stu-id="3859c-131">Razor Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="3859c-132">使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。</span><span class="sxs-lookup"><span data-stu-id="3859c-132">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="3859c-133">*Pages/Index2.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-133">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="3859c-134">*Pages/Index2.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-134">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="3859c-135">依照慣例，`PageModel` 類別檔和附加 *.cs* 檔名的 Razor Page 檔案名稱相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-135">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="3859c-136">例如，前一個 Razor Page 是 *Pages/Index2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="3859c-136">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="3859c-137">包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="3859c-137">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="3859c-138">頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。</span><span class="sxs-lookup"><span data-stu-id="3859c-138">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="3859c-139">下表顯示 Razor 頁面路徑和相符的 URL：</span><span class="sxs-lookup"><span data-stu-id="3859c-139">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="3859c-140">檔案名稱和路徑</span><span class="sxs-lookup"><span data-stu-id="3859c-140">File name and path</span></span>               | <span data-ttu-id="3859c-141">比對 URL</span><span class="sxs-lookup"><span data-stu-id="3859c-141">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3859c-142">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-142">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="3859c-143">`/` 或 `/Index`</span><span class="sxs-lookup"><span data-stu-id="3859c-143">`/` or `/Index`</span></span> |
| <span data-ttu-id="3859c-144">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-144">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="3859c-145">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-145">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="3859c-146">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-146">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="3859c-147">`/Store` 或 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="3859c-147">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="3859c-148">附註：</span><span class="sxs-lookup"><span data-stu-id="3859c-148">Notes:</span></span>

* <span data-ttu-id="3859c-149">執行階段預設會在 *Pages* 資料夾中尋找 Razor Pages 的檔案。</span><span class="sxs-lookup"><span data-stu-id="3859c-149">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="3859c-150">`Index` 是 URL 未包含頁面時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-150">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="3859c-151">撰寫基本表單</span><span class="sxs-lookup"><span data-stu-id="3859c-151">Write a basic form</span></span>

<span data-ttu-id="3859c-152">Razor Pages 設計用於製作一般的模式，可搭配網頁瀏覽器一起使用，在建置應用程式時能易於實作。</span><span class="sxs-lookup"><span data-stu-id="3859c-152">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="3859c-153">[模型繫結](xref:mvc/models/model-binding)、[標記協助程式](xref:mvc/views/tag-helpers/intro)和 HTML 協助程式搭配 Razor Page 類別中定義的屬性「就這麼簡單」。</span><span class="sxs-lookup"><span data-stu-id="3859c-153">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="3859c-154">`Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：</span><span class="sxs-lookup"><span data-stu-id="3859c-154">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="3859c-155">本文件中的範例，會在 [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) 檔案中初始化 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="3859c-155">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="3859c-156">資料模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-156">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="3859c-157">DB 內容：</span><span class="sxs-lookup"><span data-stu-id="3859c-157">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="3859c-158">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-158">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="3859c-159">*Pages/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-159">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="3859c-160">依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="3859c-160">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="3859c-161">`PageModel` 類別可以分離頁面邏輯與頁面展示。</span><span class="sxs-lookup"><span data-stu-id="3859c-161">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="3859c-162">此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。</span><span class="sxs-lookup"><span data-stu-id="3859c-162">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="3859c-163">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="3859c-163">This separation allows:</span></span>

* <span data-ttu-id="3859c-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3859c-164">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="3859c-165">單元測試</span><span class="sxs-lookup"><span data-stu-id="3859c-165">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="3859c-166">在 `POST` 要求上執行的頁面具有 `OnPostAsync`「處理常式方法」 (當使用者張貼表單時)。</span><span class="sxs-lookup"><span data-stu-id="3859c-166">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="3859c-167">Handler methods for any HTTP verb can be added.</span><span class="sxs-lookup"><span data-stu-id="3859c-167">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="3859c-168">最常見的處理常式包括：</span><span class="sxs-lookup"><span data-stu-id="3859c-168">The most common handlers are:</span></span>

* <span data-ttu-id="3859c-169">`OnGet`，初始化頁所需要的狀態。</span><span class="sxs-lookup"><span data-stu-id="3859c-169">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="3859c-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span><span class="sxs-lookup"><span data-stu-id="3859c-170">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="3859c-171">`OnPost`，處理表單提交作業。</span><span class="sxs-lookup"><span data-stu-id="3859c-171">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="3859c-172">`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。</span><span class="sxs-lookup"><span data-stu-id="3859c-172">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="3859c-173">上述程式碼一般用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-173">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="3859c-174">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="3859c-174">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="3859c-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="3859c-175">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="3859c-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span><span class="sxs-lookup"><span data-stu-id="3859c-176">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="3859c-177">前一個 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="3859c-177">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3859c-178">`OnPostAsync` 的基本流程：</span><span class="sxs-lookup"><span data-stu-id="3859c-178">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="3859c-179">檢查驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="3859c-179">Check for validation errors.</span></span>

* <span data-ttu-id="3859c-180">如果沒有任何錯誤，會儲存資料並重新導向。</span><span class="sxs-lookup"><span data-stu-id="3859c-180">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="3859c-181">如果有錯誤，會再次顯示有驗證訊息的頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-181">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="3859c-182">在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。</span><span class="sxs-lookup"><span data-stu-id="3859c-182">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="3859c-183">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-183">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="3859c-184">The rendered HTML from *Pages/Create.cshtml*:</span><span class="sxs-lookup"><span data-stu-id="3859c-184">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="3859c-185">In the previous code, posting the form:</span><span class="sxs-lookup"><span data-stu-id="3859c-185">In the previous code, posting the form:</span></span>

* <span data-ttu-id="3859c-186">With valid data:</span><span class="sxs-lookup"><span data-stu-id="3859c-186">With valid data:</span></span>

  * <span data-ttu-id="3859c-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span><span class="sxs-lookup"><span data-stu-id="3859c-187">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="3859c-188">`RedirectToPage` 傳回 <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult> 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="3859c-188">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="3859c-189">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="3859c-189">`RedirectToPage`:</span></span>

    * <span data-ttu-id="3859c-190">Is an action result.</span><span class="sxs-lookup"><span data-stu-id="3859c-190">Is an action result.</span></span>
    * <span data-ttu-id="3859c-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span><span class="sxs-lookup"><span data-stu-id="3859c-191">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="3859c-192">Is customized for pages.</span><span class="sxs-lookup"><span data-stu-id="3859c-192">Is customized for pages.</span></span> <span data-ttu-id="3859c-193">在上述範例中，它會重新導向至根索引頁面 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-193">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="3859c-194">[產生頁面 URL](#url_gen)一節會詳細說明 `RedirectToPage`。</span><span class="sxs-lookup"><span data-stu-id="3859c-194">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="3859c-195">With validation errors that are passed to the server:</span><span class="sxs-lookup"><span data-stu-id="3859c-195">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="3859c-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span><span class="sxs-lookup"><span data-stu-id="3859c-196">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="3859c-197">`Page` 傳回 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="3859c-197">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="3859c-198">傳回 `Page` 類似於控制站中的動作傳回 `View`。</span><span class="sxs-lookup"><span data-stu-id="3859c-198">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="3859c-199">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="3859c-199">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="3859c-200">傳回 `void` 的處理常式方法會呈現頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-200">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="3859c-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span><span class="sxs-lookup"><span data-stu-id="3859c-201">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="3859c-202">In this sample, no validation errors are displayed on the client.</span><span class="sxs-lookup"><span data-stu-id="3859c-202">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="3859c-203">Validation error handing is covered later in this document.</span><span class="sxs-lookup"><span data-stu-id="3859c-203">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="3859c-204">With validation errors detected by client side validation:</span><span class="sxs-lookup"><span data-stu-id="3859c-204">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="3859c-205">Data is **not** posted to the server.</span><span class="sxs-lookup"><span data-stu-id="3859c-205">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="3859c-206">Client-side validation is explained later in this document.</span><span class="sxs-lookup"><span data-stu-id="3859c-206">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="3859c-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span><span class="sxs-lookup"><span data-stu-id="3859c-207">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="3859c-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span><span class="sxs-lookup"><span data-stu-id="3859c-208">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="3859c-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span><span class="sxs-lookup"><span data-stu-id="3859c-209">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

<span data-ttu-id="3859c-210">根據預設，Razor Pages 只會建立屬性與非 `GET` 動詞之間的繫結。</span><span class="sxs-lookup"><span data-stu-id="3859c-210">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="3859c-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span><span class="sxs-lookup"><span data-stu-id="3859c-211">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="3859c-212">透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-212">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="3859c-213">Reviewing the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="3859c-213">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="3859c-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span><span class="sxs-lookup"><span data-stu-id="3859c-214">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="3859c-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span><span class="sxs-lookup"><span data-stu-id="3859c-215">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="3859c-216">The home page</span><span class="sxs-lookup"><span data-stu-id="3859c-216">The home page</span></span>

<span data-ttu-id="3859c-217">*Index.cshtml* is the home page:</span><span class="sxs-lookup"><span data-stu-id="3859c-217">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="3859c-218">已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="3859c-218">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="3859c-219">The *Index.cshtml* file contains the following markup:</span><span class="sxs-lookup"><span data-stu-id="3859c-219">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="3859c-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="3859c-220">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="3859c-221">該連結包含路由資料和連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-221">The link contains route data with the contact ID.</span></span> <span data-ttu-id="3859c-222">例如，`https://localhost:5001/Edit/1`。</span><span class="sxs-lookup"><span data-stu-id="3859c-222">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="3859c-223">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="3859c-223">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="3859c-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span><span class="sxs-lookup"><span data-stu-id="3859c-224">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="3859c-225">The rendered HTML:</span><span class="sxs-lookup"><span data-stu-id="3859c-225">The rendered HTML:</span></span>

```HTML
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="3859c-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span><span class="sxs-lookup"><span data-stu-id="3859c-226">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="3859c-227">The customer contact ID, specified by the `asp-route-id` attribute.</span><span class="sxs-lookup"><span data-stu-id="3859c-227">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="3859c-228">The `handler`, specified by the `asp-page-handler` attribute.</span><span class="sxs-lookup"><span data-stu-id="3859c-228">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="3859c-229">選取按鈕時，表單 `POST` 要求會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="3859c-229">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="3859c-230">依照慣例，會依據配置 `OnPost[handler]Async`，按 `handler` 參數的值來選取處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-230">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="3859c-231">在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。</span><span class="sxs-lookup"><span data-stu-id="3859c-231">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="3859c-232">若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-232">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="3859c-233">`OnPostDeleteAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="3859c-233">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="3859c-234">Gets the `id` from the query string.</span><span class="sxs-lookup"><span data-stu-id="3859c-234">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="3859c-235">使用 `FindAsync` 在資料庫中查詢客戶連絡人。</span><span class="sxs-lookup"><span data-stu-id="3859c-235">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="3859c-236">If the customer contact is found, it's removed and the database is updated.</span><span class="sxs-lookup"><span data-stu-id="3859c-236">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="3859c-237">呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> 以重新導向至根索引頁 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-237">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="3859c-238">The Edit.cshtml file</span><span class="sxs-lookup"><span data-stu-id="3859c-238">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="3859c-239">第一行包含 `@page "{id:int}"` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-239">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="3859c-240">路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="3859c-240">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="3859c-241">如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="3859c-241">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3859c-242">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="3859c-242">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3859c-243">The *Edit.cshtml.cs* file:</span><span class="sxs-lookup"><span data-stu-id="3859c-243">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="3859c-244">驗證</span><span class="sxs-lookup"><span data-stu-id="3859c-244">Validation</span></span>

<span data-ttu-id="3859c-245">Validation rules:</span><span class="sxs-lookup"><span data-stu-id="3859c-245">Validation rules:</span></span>

* <span data-ttu-id="3859c-246">Are declaratively specified in the model class.</span><span class="sxs-lookup"><span data-stu-id="3859c-246">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="3859c-247">Are enforced everywhere in the app.</span><span class="sxs-lookup"><span data-stu-id="3859c-247">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="3859c-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span><span class="sxs-lookup"><span data-stu-id="3859c-248">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="3859c-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span><span class="sxs-lookup"><span data-stu-id="3859c-249">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="3859c-250">Consider the `Customer` model:</span><span class="sxs-lookup"><span data-stu-id="3859c-250">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="3859c-251">Using the following *Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="3859c-251">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="3859c-252">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="3859c-252">The preceding code:</span></span>

* <span data-ttu-id="3859c-253">Includes jQuery and jQuery validation scripts.</span><span class="sxs-lookup"><span data-stu-id="3859c-253">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="3859c-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span><span class="sxs-lookup"><span data-stu-id="3859c-254">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="3859c-255">Client-side validation.</span><span class="sxs-lookup"><span data-stu-id="3859c-255">Client-side validation.</span></span>
  * <span data-ttu-id="3859c-256">Validation error rendering.</span><span class="sxs-lookup"><span data-stu-id="3859c-256">Validation error rendering.</span></span>

* <span data-ttu-id="3859c-257">產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="3859c-257">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="3859c-258">Posting the Create form without a name value displays the error message "The Name field is required."</span><span class="sxs-lookup"><span data-stu-id="3859c-258">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="3859c-259">on the form.</span><span class="sxs-lookup"><span data-stu-id="3859c-259">on the form.</span></span> <span data-ttu-id="3859c-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span><span class="sxs-lookup"><span data-stu-id="3859c-260">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="3859c-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span><span class="sxs-lookup"><span data-stu-id="3859c-261">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="3859c-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span><span class="sxs-lookup"><span data-stu-id="3859c-262">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="3859c-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span><span class="sxs-lookup"><span data-stu-id="3859c-263">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="3859c-264">With the name longer than 10.</span><span class="sxs-lookup"><span data-stu-id="3859c-264">With the name longer than 10.</span></span>
* <span data-ttu-id="3859c-265">The error message "The field Name must be a string with a maximum length of 10."</span><span class="sxs-lookup"><span data-stu-id="3859c-265">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="3859c-266">is returned.</span><span class="sxs-lookup"><span data-stu-id="3859c-266">is returned.</span></span>

<span data-ttu-id="3859c-267">Consider the following `Movie` model:</span><span class="sxs-lookup"><span data-stu-id="3859c-267">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="3859c-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span><span class="sxs-lookup"><span data-stu-id="3859c-268">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="3859c-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span><span class="sxs-lookup"><span data-stu-id="3859c-269">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="3859c-270">`RegularExpression` 屬性則用來限制可輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="3859c-270">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="3859c-271">在上述程式碼中，"Genre"：</span><span class="sxs-lookup"><span data-stu-id="3859c-271">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="3859c-272">必須指使用字母。</span><span class="sxs-lookup"><span data-stu-id="3859c-272">Must only use letters.</span></span>
  * <span data-ttu-id="3859c-273">第一個字母必須是大寫。</span><span class="sxs-lookup"><span data-stu-id="3859c-273">The first letter is required to be uppercase.</span></span> <span data-ttu-id="3859c-274">不允許使用空格、數字和特殊字元。</span><span class="sxs-lookup"><span data-stu-id="3859c-274">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="3859c-275">`RegularExpression` "Rating"：</span><span class="sxs-lookup"><span data-stu-id="3859c-275">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="3859c-276">第一個字元必須為大寫字母。</span><span class="sxs-lookup"><span data-stu-id="3859c-276">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="3859c-277">Allows special characters and numbers in subsequent spaces.</span><span class="sxs-lookup"><span data-stu-id="3859c-277">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="3859c-278">"PG-13" 對分級而言有效，但不適用於 "Genre"。</span><span class="sxs-lookup"><span data-stu-id="3859c-278">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="3859c-279">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="3859c-279">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="3859c-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span><span class="sxs-lookup"><span data-stu-id="3859c-280">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="3859c-281">實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="3859c-281">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="3859c-282">The Create page for the `Movie` model shows displays errors with invalid values:</span><span class="sxs-lookup"><span data-stu-id="3859c-282">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="3859c-284">如需詳細資訊，請參閱:</span><span class="sxs-lookup"><span data-stu-id="3859c-284">For more information, see:</span></span>

* [<span data-ttu-id="3859c-285">Add validation to the Movie app</span><span class="sxs-lookup"><span data-stu-id="3859c-285">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="3859c-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span><span class="sxs-lookup"><span data-stu-id="3859c-286">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="3859c-287">使用 OnGet 處理常式後援來處理 HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="3859c-287">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="3859c-288">`HEAD` requests allow retrieving the headers for a specific resource.</span><span class="sxs-lookup"><span data-stu-id="3859c-288">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="3859c-289">不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。</span><span class="sxs-lookup"><span data-stu-id="3859c-289">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="3859c-290">一般來說，會為 `HEAD` 要求建立及呼叫 `OnHead` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="3859c-290">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

<span data-ttu-id="3859c-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span><span class="sxs-lookup"><span data-stu-id="3859c-291">Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="3859c-292">XSRF/CSRF 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3859c-292">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="3859c-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span><span class="sxs-lookup"><span data-stu-id="3859c-293">Razor Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="3859c-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span><span class="sxs-lookup"><span data-stu-id="3859c-294">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="3859c-295">搭配 Razor 頁面使用版面配置、部分、範本和標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="3859c-295">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="3859c-296">Pages 可搭配 Razor 檢視引擎的所有功能一起使用。</span><span class="sxs-lookup"><span data-stu-id="3859c-296">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="3859c-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span><span class="sxs-lookup"><span data-stu-id="3859c-297">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="3859c-298">可利用這些功能的一部分來整理這個頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-298">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="3859c-299">將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="3859c-299">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="3859c-300">[配置](xref:mvc/views/layout)：</span><span class="sxs-lookup"><span data-stu-id="3859c-300">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="3859c-301">控制每個頁面的版面配置 (除非頁面退出版面配置)。</span><span class="sxs-lookup"><span data-stu-id="3859c-301">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="3859c-302">匯入 HTML 結構，例如 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="3859c-302">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="3859c-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span><span class="sxs-lookup"><span data-stu-id="3859c-303">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="3859c-304">For more information, see [layout page](xref:mvc/views/layout).</span><span class="sxs-lookup"><span data-stu-id="3859c-304">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="3859c-305">[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：</span><span class="sxs-lookup"><span data-stu-id="3859c-305">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3859c-306">版面配置位於 *Pages/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-306">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="3859c-307">頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。</span><span class="sxs-lookup"><span data-stu-id="3859c-307">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3859c-308">您可以從任何 Razor 頁面中的 *Pages* 資料夾下，使用 *Pages/Shared* 資料夾中的版面配置。</span><span class="sxs-lookup"><span data-stu-id="3859c-308">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="3859c-309">版面配置頁面應位於 *Pages/Shared* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3859c-309">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="3859c-310">我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-310">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="3859c-311">*Views/Shared* 是 MVC 檢視模式。</span><span class="sxs-lookup"><span data-stu-id="3859c-311">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="3859c-312">Razor 頁面應該要依賴資料夾階層，不是路徑慣例。</span><span class="sxs-lookup"><span data-stu-id="3859c-312">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="3859c-313">Razor 頁面的檢視搜尋包括 *Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-313">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="3859c-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span><span class="sxs-lookup"><span data-stu-id="3859c-314">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="3859c-315">新增 *Pages/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-315">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="3859c-316">本教學課程稍後會說明 `@namespace`。</span><span class="sxs-lookup"><span data-stu-id="3859c-316">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="3859c-317">`@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-317">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="3859c-318">The `@namespace` directive set on a page:</span><span class="sxs-lookup"><span data-stu-id="3859c-318">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="3859c-319">The `@namespace` directive sets the namespace for the page.</span><span class="sxs-lookup"><span data-stu-id="3859c-319">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="3859c-320">`@model` 指示詞不需要包含命名空間。</span><span class="sxs-lookup"><span data-stu-id="3859c-320">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="3859c-321">當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-321">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="3859c-322">所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3859c-322">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="3859c-323">例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：</span><span class="sxs-lookup"><span data-stu-id="3859c-323">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="3859c-324">*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="3859c-324">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="3859c-325">為 *Pages/Customers/Edit.cshtml* Razor 頁面產生的命名空間和 `PageModel` 類別相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-325">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="3859c-326">`@namespace`  *也適用於傳統的 Razor 檢視。*</span><span class="sxs-lookup"><span data-stu-id="3859c-326">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="3859c-327">Consider the *Pages/Create.cshtml* view file:</span><span class="sxs-lookup"><span data-stu-id="3859c-327">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="3859c-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span><span class="sxs-lookup"><span data-stu-id="3859c-328">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="3859c-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span><span class="sxs-lookup"><span data-stu-id="3859c-329">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="3859c-330">The layout file imported the JavaScript files.</span><span class="sxs-lookup"><span data-stu-id="3859c-330">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="3859c-331">[Razor Pages 入門專案](#rpvs17)包含 *Pages/_ValidationScriptsPartial.cshtml*，連結用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="3859c-331">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="3859c-332">如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。</span><span class="sxs-lookup"><span data-stu-id="3859c-332">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="3859c-333">產生頁面 URL</span><span class="sxs-lookup"><span data-stu-id="3859c-333">URL generation for Pages</span></span>

<span data-ttu-id="3859c-334">前面出現過的 `Create` 頁面使用 `RedirectToPage`：</span><span class="sxs-lookup"><span data-stu-id="3859c-334">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="3859c-335">應用程式有下列檔案/資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="3859c-335">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="3859c-336">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="3859c-336">*/Pages*</span></span>

  * <span data-ttu-id="3859c-337">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-337">*Index.cshtml*</span></span>
  * <span data-ttu-id="3859c-338">*Privacy.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-338">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="3859c-339">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="3859c-339">*/Customers*</span></span>

    * <span data-ttu-id="3859c-340">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-340">*Create.cshtml*</span></span>
    * <span data-ttu-id="3859c-341">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-341">*Edit.cshtml*</span></span>
    * <span data-ttu-id="3859c-342">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-342">*Index.cshtml*</span></span>

<span data-ttu-id="3859c-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span><span class="sxs-lookup"><span data-stu-id="3859c-343">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="3859c-344">The string `./Index` is a relative page name used to access the preceding page.</span><span class="sxs-lookup"><span data-stu-id="3859c-344">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="3859c-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="3859c-345">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="3859c-346">例如:</span><span class="sxs-lookup"><span data-stu-id="3859c-346">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="3859c-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span><span class="sxs-lookup"><span data-stu-id="3859c-347">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="3859c-348">例如:</span><span class="sxs-lookup"><span data-stu-id="3859c-348">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="3859c-349">頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-349">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="3859c-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span><span class="sxs-lookup"><span data-stu-id="3859c-350">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="3859c-351">URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。</span><span class="sxs-lookup"><span data-stu-id="3859c-351">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="3859c-352">產生頁面 URL 支援相關的名稱。</span><span class="sxs-lookup"><span data-stu-id="3859c-352">URL generation for pages supports relative names.</span></span> <span data-ttu-id="3859c-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span><span class="sxs-lookup"><span data-stu-id="3859c-353">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="3859c-354">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="3859c-354">RedirectToPage(x)</span></span>| <span data-ttu-id="3859c-355">頁面</span><span class="sxs-lookup"><span data-stu-id="3859c-355">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3859c-356">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-356">RedirectToPage("/Index")</span></span> | <span data-ttu-id="3859c-357">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-357">*Pages/Index*</span></span> |
| <span data-ttu-id="3859c-358">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="3859c-358">RedirectToPage("./Index");</span></span> | <span data-ttu-id="3859c-359">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-359">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="3859c-360">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-360">RedirectToPage("../Index")</span></span> | <span data-ttu-id="3859c-361">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-361">*Pages/Index*</span></span> |
| <span data-ttu-id="3859c-362">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-362">RedirectToPage("Index")</span></span>  | <span data-ttu-id="3859c-363">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-363">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="3859c-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span><span class="sxs-lookup"><span data-stu-id="3859c-364">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="3859c-365">`RedirectToPage` 參數「結合」了目前頁面的路徑，以計算目的地頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="3859c-365">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="3859c-366">相對名稱連結在以複雜結構建置網站時很有用。</span><span class="sxs-lookup"><span data-stu-id="3859c-366">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="3859c-367">When relative names are used to link between pages in a folder:</span><span class="sxs-lookup"><span data-stu-id="3859c-367">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="3859c-368">Renaming a folder doesn't break the relative links.</span><span class="sxs-lookup"><span data-stu-id="3859c-368">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="3859c-369">Links are not broken because they don't include the folder name.</span><span class="sxs-lookup"><span data-stu-id="3859c-369">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="3859c-370">若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：</span><span class="sxs-lookup"><span data-stu-id="3859c-370">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="3859c-371">如需詳細資訊，請參閱 <xref:mvc/controllers/areas> 與 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="3859c-371">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="3859c-372">ViewData 屬性</span><span class="sxs-lookup"><span data-stu-id="3859c-372">ViewData attribute</span></span>

<span data-ttu-id="3859c-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span><span class="sxs-lookup"><span data-stu-id="3859c-373">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="3859c-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span><span class="sxs-lookup"><span data-stu-id="3859c-374">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="3859c-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span><span class="sxs-lookup"><span data-stu-id="3859c-375">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="3859c-376">在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：</span><span class="sxs-lookup"><span data-stu-id="3859c-376">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="3859c-377">在此配置中，標題會從 ViewData 字典中讀取：</span><span class="sxs-lookup"><span data-stu-id="3859c-377">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="3859c-378">TempData</span><span class="sxs-lookup"><span data-stu-id="3859c-378">TempData</span></span>

<span data-ttu-id="3859c-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span><span class="sxs-lookup"><span data-stu-id="3859c-379">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="3859c-380">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="3859c-380">This property stores data until it's read.</span></span> <span data-ttu-id="3859c-381"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> 和 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="3859c-381">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3859c-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span><span class="sxs-lookup"><span data-stu-id="3859c-382">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="3859c-383">下列程式碼會設定使用 `TempData` 的 `Message` 值：</span><span class="sxs-lookup"><span data-stu-id="3859c-383">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="3859c-384">*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `TempData` 的 `Message` 值。</span><span class="sxs-lookup"><span data-stu-id="3859c-384">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="3859c-385">*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="3859c-385">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="3859c-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span><span class="sxs-lookup"><span data-stu-id="3859c-386">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="3859c-387">每頁面有多個處理常式</span><span class="sxs-lookup"><span data-stu-id="3859c-387">Multiple handlers per page</span></span>

<span data-ttu-id="3859c-388">下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：</span><span class="sxs-lookup"><span data-stu-id="3859c-388">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="3859c-389">上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="3859c-389">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="3859c-390">`asp-page-handler` 屬性附隨於 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="3859c-390">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="3859c-391">`asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-391">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="3859c-392">因為範例連結至目前的頁面，所以未指定 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="3859c-392">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="3859c-393">頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-393">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="3859c-394">上述程式碼使用「具名的處理常式方法」。</span><span class="sxs-lookup"><span data-stu-id="3859c-394">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="3859c-395">具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。</span><span class="sxs-lookup"><span data-stu-id="3859c-395">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="3859c-396">在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。</span><span class="sxs-lookup"><span data-stu-id="3859c-396">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="3859c-397">移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-397">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="3859c-398">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-398">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="3859c-399">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-399">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="3859c-400">自訂路由</span><span class="sxs-lookup"><span data-stu-id="3859c-400">Custom routes</span></span>

<span data-ttu-id="3859c-401">使用 `@page` 指示詞，可以：</span><span class="sxs-lookup"><span data-stu-id="3859c-401">Use the `@page` directive to:</span></span>

* <span data-ttu-id="3859c-402">指定頁面的自訂路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-402">Specify a custom route to a page.</span></span> <span data-ttu-id="3859c-403">例如，[關於] 頁面的路由可使用 `@page "/Some/Other/Path"` 設為 `/Some/Other/Path`。</span><span class="sxs-lookup"><span data-stu-id="3859c-403">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="3859c-404">將區段附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-404">Append segments to a page's default route.</span></span> <span data-ttu-id="3859c-405">例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-405">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="3859c-406">將參數附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-406">Append parameters to a page's default route.</span></span> <span data-ttu-id="3859c-407">例如，具有 `@page "{id}"` 的頁面可要求識別碼參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="3859c-407">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="3859c-408">支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3859c-408">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="3859c-409">例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-409">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="3859c-410">您可以藉由指定路由範本 `@page "{handler?}"`，將 URL 中的查詢字串 `?handler=JoinList` 變更為路由區段 `/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-410">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="3859c-411">如果您不喜歡 URL 有查詢字串 `?handler=JoinList`，您可以變更路由，將處理常式名稱置於 URL 的路徑部分。</span><span class="sxs-lookup"><span data-stu-id="3859c-411">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="3859c-412">您可以新增路由範本，在 `@page` 指示詞後面用雙引號括住，以自訂路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-412">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="3859c-413">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-413">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="3859c-414">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-414">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="3859c-415">跟在 `handler` 後面的 `?` 表示路由參數為選擇性。</span><span class="sxs-lookup"><span data-stu-id="3859c-415">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="3859c-416">Advanced configuration and settings</span><span class="sxs-lookup"><span data-stu-id="3859c-416">Advanced configuration and settings</span></span>

<span data-ttu-id="3859c-417">The configuration and settings in following sections is not required by most apps.</span><span class="sxs-lookup"><span data-stu-id="3859c-417">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="3859c-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span><span class="sxs-lookup"><span data-stu-id="3859c-418">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="3859c-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span><span class="sxs-lookup"><span data-stu-id="3859c-419">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="3859c-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span><span class="sxs-lookup"><span data-stu-id="3859c-420">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="3859c-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span><span class="sxs-lookup"><span data-stu-id="3859c-421">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="3859c-422">指定 Razor Pages 位於內容根目錄</span><span class="sxs-lookup"><span data-stu-id="3859c-422">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="3859c-423">根據預設，Razor Pages 位於 */Pages* 根目錄。</span><span class="sxs-lookup"><span data-stu-id="3859c-423">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="3859c-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span><span class="sxs-lookup"><span data-stu-id="3859c-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="3859c-425">指定 Razor Pages 位於自訂根目錄</span><span class="sxs-lookup"><span data-stu-id="3859c-425">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="3859c-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span><span class="sxs-lookup"><span data-stu-id="3859c-426">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="3859c-427">其他資源</span><span class="sxs-lookup"><span data-stu-id="3859c-427">Additional resources</span></span>

* <span data-ttu-id="3859c-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span><span class="sxs-lookup"><span data-stu-id="3859c-428">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction</span></span>
* [<span data-ttu-id="3859c-429">Download or view sample code</span><span class="sxs-lookup"><span data-stu-id="3859c-429">Download or view sample code</span></span>](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="3859c-430">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="3859c-430">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="3859c-431">Razor Pages 是 ASP.NET Core MVC 新的部分，更容易編寫以頁面為焦點的案例程式碼，也更具生產力。</span><span class="sxs-lookup"><span data-stu-id="3859c-431">Razor Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="3859c-432">如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="3859c-432">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="3859c-433">本文件提供 Razor Pages 簡介。</span><span class="sxs-lookup"><span data-stu-id="3859c-433">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="3859c-434">它不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="3859c-434">It's not a step by step tutorial.</span></span> <span data-ttu-id="3859c-435">如果您發現某些章節很難遵循，請參閱[9開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="3859c-435">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="3859c-436">如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="3859c-436">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="3859c-437">Prerequisites</span><span class="sxs-lookup"><span data-stu-id="3859c-437">Prerequisites</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3859c-438">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3859c-438">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3859c-439">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3859c-439">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3859c-440">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3859c-440">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="3859c-441">建立 Razor Pages 專案</span><span class="sxs-lookup"><span data-stu-id="3859c-441">Create a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="3859c-442">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="3859c-442">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="3859c-443">如需如何建立 Razor Pages 專案的詳細說明，請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="3859c-443">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="3859c-444">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="3859c-444">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="3859c-445">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="3859c-445">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="3859c-446">從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="3859c-446">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="3859c-447">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="3859c-447">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="3859c-448">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="3859c-448">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="3859c-449">Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="3859c-449">Razor Pages</span></span>

<span data-ttu-id="3859c-450">Razor Pages 是在 *Startup.cs* 中啟用：</span><span class="sxs-lookup"><span data-stu-id="3859c-450">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="3859c-451">請考慮使用基本頁面：<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="3859c-451">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="3859c-452">上述程式碼看起來很像用於 ASP.NET Core 應用程式的 [Razor 檢視檔案](xref:tutorials/first-mvc-app/adding-view)，含有控制器和檢視。</span><span class="sxs-lookup"><span data-stu-id="3859c-452">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="3859c-453">讓它不同的是 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-453">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="3859c-454">`@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。</span><span class="sxs-lookup"><span data-stu-id="3859c-454">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="3859c-455">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-455">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="3859c-456">`@page` 會影響其他的 Razor 建構行為。</span><span class="sxs-lookup"><span data-stu-id="3859c-456">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="3859c-457">使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。</span><span class="sxs-lookup"><span data-stu-id="3859c-457">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="3859c-458">*Pages/Index2.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-458">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="3859c-459">*Pages/Index2.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-459">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="3859c-460">依照慣例，`PageModel` 類別檔和附加 *.cs* 檔名的 Razor Page 檔案名稱相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-460">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="3859c-461">例如，前一個 Razor Page 是 *Pages/Index2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="3859c-461">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="3859c-462">包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="3859c-462">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="3859c-463">頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。</span><span class="sxs-lookup"><span data-stu-id="3859c-463">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="3859c-464">下表顯示 Razor 頁面路徑和相符的 URL：</span><span class="sxs-lookup"><span data-stu-id="3859c-464">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="3859c-465">檔案名稱和路徑</span><span class="sxs-lookup"><span data-stu-id="3859c-465">File name and path</span></span>               | <span data-ttu-id="3859c-466">比對 URL</span><span class="sxs-lookup"><span data-stu-id="3859c-466">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3859c-467">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-467">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="3859c-468">`/` 或 `/Index`</span><span class="sxs-lookup"><span data-stu-id="3859c-468">`/` or `/Index`</span></span> |
| <span data-ttu-id="3859c-469">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-469">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="3859c-470">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-470">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="3859c-471">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-471">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="3859c-472">`/Store` 或 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="3859c-472">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="3859c-473">附註：</span><span class="sxs-lookup"><span data-stu-id="3859c-473">Notes:</span></span>

* <span data-ttu-id="3859c-474">執行階段預設會在 *Pages* 資料夾中尋找 Razor Pages 的檔案。</span><span class="sxs-lookup"><span data-stu-id="3859c-474">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="3859c-475">`Index` 是 URL 未包含頁面時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-475">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="3859c-476">撰寫基本表單</span><span class="sxs-lookup"><span data-stu-id="3859c-476">Write a basic form</span></span>

<span data-ttu-id="3859c-477">Razor Pages 設計用於製作一般的模式，可搭配網頁瀏覽器一起使用，在建置應用程式時能易於實作。</span><span class="sxs-lookup"><span data-stu-id="3859c-477">Razor Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="3859c-478">[模型繫結](xref:mvc/models/model-binding)、[標記協助程式](xref:mvc/views/tag-helpers/intro)和 HTML 協助程式搭配 Razor Page 類別中定義的屬性「就這麼簡單」。</span><span class="sxs-lookup"><span data-stu-id="3859c-478">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="3859c-479">`Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：</span><span class="sxs-lookup"><span data-stu-id="3859c-479">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="3859c-480">本文件中的範例，會在 [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) 檔案中初始化 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="3859c-480">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="3859c-481">資料模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-481">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="3859c-482">DB 內容：</span><span class="sxs-lookup"><span data-stu-id="3859c-482">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="3859c-483">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-483">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="3859c-484">*Pages/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-484">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="3859c-485">依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="3859c-485">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="3859c-486">`PageModel` 類別可以分離頁面邏輯與頁面展示。</span><span class="sxs-lookup"><span data-stu-id="3859c-486">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="3859c-487">此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。</span><span class="sxs-lookup"><span data-stu-id="3859c-487">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="3859c-488">This separation allows:</span><span class="sxs-lookup"><span data-stu-id="3859c-488">This separation allows:</span></span>

* <span data-ttu-id="3859c-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span><span class="sxs-lookup"><span data-stu-id="3859c-489">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="3859c-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span><span class="sxs-lookup"><span data-stu-id="3859c-490">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="3859c-491">在 `POST` 要求上執行的頁面具有 `OnPostAsync`「處理常式方法」 (當使用者張貼表單時)。</span><span class="sxs-lookup"><span data-stu-id="3859c-491">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="3859c-492">您可以新增任何 HTTP 指令動詞的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-492">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="3859c-493">最常見的處理常式包括：</span><span class="sxs-lookup"><span data-stu-id="3859c-493">The most common handlers are:</span></span>

* <span data-ttu-id="3859c-494">`OnGet`，初始化頁所需要的狀態。</span><span class="sxs-lookup"><span data-stu-id="3859c-494">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="3859c-495">[OnGet](#OnGet) 範例。</span><span class="sxs-lookup"><span data-stu-id="3859c-495">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="3859c-496">`OnPost`，處理表單提交作業。</span><span class="sxs-lookup"><span data-stu-id="3859c-496">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="3859c-497">`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。</span><span class="sxs-lookup"><span data-stu-id="3859c-497">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="3859c-498">上述程式碼一般用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-498">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="3859c-499">If you're familiar with ASP.NET apps using controllers and views:</span><span class="sxs-lookup"><span data-stu-id="3859c-499">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="3859c-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span><span class="sxs-lookup"><span data-stu-id="3859c-500">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="3859c-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span><span class="sxs-lookup"><span data-stu-id="3859c-501">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="3859c-502">前一個 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="3859c-502">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="3859c-503">`OnPostAsync` 的基本流程：</span><span class="sxs-lookup"><span data-stu-id="3859c-503">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="3859c-504">檢查驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="3859c-504">Check for validation errors.</span></span>

* <span data-ttu-id="3859c-505">如果沒有任何錯誤，會儲存資料並重新導向。</span><span class="sxs-lookup"><span data-stu-id="3859c-505">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="3859c-506">如果有錯誤，會再次顯示有驗證訊息的頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-506">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="3859c-507">用戶端驗證和傳統的 ASP.NET Core MVC 應用程式完全相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-507">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="3859c-508">在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。</span><span class="sxs-lookup"><span data-stu-id="3859c-508">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="3859c-509">成功輸入資料後，`OnPostAsync` 處理常式方法會呼叫 `RedirectToPage` 協助程式方法，傳回 `RedirectToPageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="3859c-509">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="3859c-510">`RedirectToPage` 是新的動作結果，類似於 `RedirectToAction` 或 `RedirectToRoute`，但會針對頁面自訂。</span><span class="sxs-lookup"><span data-stu-id="3859c-510">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="3859c-511">在上述範例中，它會重新導向至根索引頁面 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-511">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="3859c-512">[產生頁面 URL](#url_gen)一節會詳細說明 `RedirectToPage`。</span><span class="sxs-lookup"><span data-stu-id="3859c-512">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="3859c-513">當提交的表單有驗證錯誤時 (傳遞至伺服器)，`OnPostAsync` 處理常式方法會呼叫 `Page` 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-513">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="3859c-514">`Page` 傳回 `PageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="3859c-514">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="3859c-515">傳回 `Page` 類似於控制站中的動作傳回 `View`。</span><span class="sxs-lookup"><span data-stu-id="3859c-515">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="3859c-516">`PageResult` is the default return type for a handler method.</span><span class="sxs-lookup"><span data-stu-id="3859c-516">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="3859c-517">傳回 `void` 的處理常式方法會呈現頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-517">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="3859c-518">`Customer` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 加入模型繫結。</span><span class="sxs-lookup"><span data-stu-id="3859c-518">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="3859c-519">根據預設，Razor Pages 只會建立屬性與非 `GET` 動詞之間的繫結。</span><span class="sxs-lookup"><span data-stu-id="3859c-519">Razor Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="3859c-520">繫結至屬性可以減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="3859c-520">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="3859c-521">透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-521">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="3859c-522">首頁 (*Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="3859c-522">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="3859c-523">已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="3859c-523">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="3859c-524">*Index.cshtml* 檔案包含下列標記可為每個連絡人建立編輯連結：</span><span class="sxs-lookup"><span data-stu-id="3859c-524">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="3859c-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span><span class="sxs-lookup"><span data-stu-id="3859c-525">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="3859c-526">該連結包含路由資料和連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-526">The link contains route data with the contact ID.</span></span> <span data-ttu-id="3859c-527">例如，`https://localhost:5001/Edit/1`。</span><span class="sxs-lookup"><span data-stu-id="3859c-527">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="3859c-528">[標記協助程式](xref:mvc/views/tag-helpers/intro)可啟用伺服器端程式碼，以參與建立和轉譯 Razor 檔案中的 HTML 元素。</span><span class="sxs-lookup"><span data-stu-id="3859c-528">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="3859c-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span><span class="sxs-lookup"><span data-stu-id="3859c-529">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="3859c-530">*Pages/Edit.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-530">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="3859c-531">第一行包含 `@page "{id:int}"` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-531">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="3859c-532">路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="3859c-532">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="3859c-533">如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="3859c-533">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="3859c-534">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="3859c-534">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="3859c-535">*Pages/Edit.cshtml.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-535">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="3859c-536">*Index.cshtml* 檔案也包含能夠為每個客戶連絡人建立刪除按鈕的標記：</span><span class="sxs-lookup"><span data-stu-id="3859c-536">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="3859c-537">使用 HTML 轉譯刪除按鈕時，其 `formaction` 會包含下列項目的參數：</span><span class="sxs-lookup"><span data-stu-id="3859c-537">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="3859c-538">`asp-route-id` 屬性指定的客戶連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-538">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="3859c-539">`asp-page-handler` 屬性指定的 `handler`。</span><span class="sxs-lookup"><span data-stu-id="3859c-539">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="3859c-540">以下是轉譯的刪除按鈕範例，內含客戶連絡人識別碼 `1`：</span><span class="sxs-lookup"><span data-stu-id="3859c-540">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="3859c-541">選取按鈕時，表單 `POST` 要求會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="3859c-541">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="3859c-542">依照慣例，會依據配置 `OnPost[handler]Async`，按 `handler` 參數的值來選取處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-542">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="3859c-543">在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。</span><span class="sxs-lookup"><span data-stu-id="3859c-543">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="3859c-544">若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-544">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="3859c-545">The following code shows the `OnPostDeleteAsync` handler:</span><span class="sxs-lookup"><span data-stu-id="3859c-545">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="3859c-546">`OnPostDeleteAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="3859c-546">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="3859c-547">接受查詢字串的 `id`。</span><span class="sxs-lookup"><span data-stu-id="3859c-547">Accepts the `id` from the query string.</span></span> <span data-ttu-id="3859c-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span><span class="sxs-lookup"><span data-stu-id="3859c-548">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="3859c-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span><span class="sxs-lookup"><span data-stu-id="3859c-549">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="3859c-550">使用 `FindAsync` 在資料庫中查詢客戶連絡人。</span><span class="sxs-lookup"><span data-stu-id="3859c-550">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="3859c-551">若找到客戶連絡人，會從客戶連絡人清單中予以移除。</span><span class="sxs-lookup"><span data-stu-id="3859c-551">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="3859c-552">資料庫隨即更新。</span><span class="sxs-lookup"><span data-stu-id="3859c-552">The database is updated.</span></span>
* <span data-ttu-id="3859c-553">呼叫 `RedirectToPage` 以重新導向至根索引頁 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-553">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="3859c-554">將頁面屬性標示為必要</span><span class="sxs-lookup"><span data-stu-id="3859c-554">Mark page properties as required</span></span>

<span data-ttu-id="3859c-555">`PageModel` 上的屬性可以裝飾以 [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) 屬性：</span><span class="sxs-lookup"><span data-stu-id="3859c-555">Properties on a `PageModel` can be decorated with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="3859c-556">如需詳細資訊，請參閱[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="3859c-556">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="3859c-557">使用 OnGet 處理常式後援來處理 HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="3859c-557">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="3859c-558">`HEAD` 要求可讓您擷取特定資源的標頭。</span><span class="sxs-lookup"><span data-stu-id="3859c-558">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="3859c-559">不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。</span><span class="sxs-lookup"><span data-stu-id="3859c-559">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="3859c-560">一般來說，會為 `HEAD` 要求建立及呼叫 `OnHead` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="3859c-560">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="3859c-561">在 ASP.NET Core 2.1 或更新版本中，若未定義任何 `OnGet` 處理常式，Razor Pages 會轉而呼叫 `OnHead` 處理常式。</span><span class="sxs-lookup"><span data-stu-id="3859c-561">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="3859c-562">這個行為藉由在 `Startup.ConfigureServices` 中呼叫 [SetCompatibilityVersion](xref:mvc/compatibility-version) 來啟用：</span><span class="sxs-lookup"><span data-stu-id="3859c-562">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="3859c-563">預設範本會在 ASP.NET Core 2.1 和 2.2 中產生 `SetCompatibilityVersion` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="3859c-563">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="3859c-564">`SetCompatibilityVersion` 實際上是將 Razor 頁面選項 `AllowMappingHeadRequestsToGetHandler` 設為 `true`。</span><span class="sxs-lookup"><span data-stu-id="3859c-564">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="3859c-565">您可以明確選擇「特定」行為，而不必透過 `SetCompatibilityVersion` 選擇所有行為。</span><span class="sxs-lookup"><span data-stu-id="3859c-565">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="3859c-566">下列程式碼會選擇讓 `HEAD` 要求對應到 `OnGet` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="3859c-566">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="3859c-567">XSRF/CSRF 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="3859c-567">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="3859c-568">您不必撰寫任何[防偽驗證](xref:security/anti-request-forgery)程式碼。</span><span class="sxs-lookup"><span data-stu-id="3859c-568">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="3859c-569">防偽權杖的產生和驗證會自動包含在 Razor 頁面中。</span><span class="sxs-lookup"><span data-stu-id="3859c-569">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="3859c-570">搭配 Razor 頁面使用版面配置、部分、範本和標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="3859c-570">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="3859c-571">Pages 可搭配 Razor 檢視引擎的所有功能一起使用。</span><span class="sxs-lookup"><span data-stu-id="3859c-571">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="3859c-572">版面配置、部分、範本、標記協助程式、 *_ViewStart.cshtml*、 *_ViewImports.cshtml* 運作方式一如它們在傳統 Razor 檢視中的方式。</span><span class="sxs-lookup"><span data-stu-id="3859c-572">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="3859c-573">可利用這些功能的一部分來整理這個頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-573">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="3859c-574">將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="3859c-574">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="3859c-575">[配置](xref:mvc/views/layout)：</span><span class="sxs-lookup"><span data-stu-id="3859c-575">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="3859c-576">控制每個頁面的版面配置 (除非頁面退出版面配置)。</span><span class="sxs-lookup"><span data-stu-id="3859c-576">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="3859c-577">匯入 HTML 結構，例如 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="3859c-577">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="3859c-578">如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="3859c-578">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="3859c-579">[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：</span><span class="sxs-lookup"><span data-stu-id="3859c-579">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="3859c-580">版面配置位於 *Pages/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-580">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="3859c-581">頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。</span><span class="sxs-lookup"><span data-stu-id="3859c-581">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="3859c-582">您可以從任何 Razor 頁面中的 *Pages* 資料夾下，使用 *Pages/Shared* 資料夾中的版面配置。</span><span class="sxs-lookup"><span data-stu-id="3859c-582">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="3859c-583">版面配置頁面應位於 *Pages/Shared* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="3859c-583">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="3859c-584">我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-584">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="3859c-585">*Views/Shared* 是 MVC 檢視模式。</span><span class="sxs-lookup"><span data-stu-id="3859c-585">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="3859c-586">Razor 頁面應該要依賴資料夾階層，不是路徑慣例。</span><span class="sxs-lookup"><span data-stu-id="3859c-586">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="3859c-587">Razor 頁面的檢視搜尋包括 *Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-587">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="3859c-588">搭配 MVC 控制器使用的版面配置、範本和部分以及傳統的 Razor 檢視「就這麼簡單」。</span><span class="sxs-lookup"><span data-stu-id="3859c-588">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="3859c-589">新增 *Pages/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-589">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="3859c-590">本教學課程稍後會說明 `@namespace`。</span><span class="sxs-lookup"><span data-stu-id="3859c-590">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="3859c-591">`@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-591">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="3859c-592">在頁面上明確使用 `@namespace` 指示詞時：</span><span class="sxs-lookup"><span data-stu-id="3859c-592">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="3859c-593">指示詞會設定頁面的命名空間。</span><span class="sxs-lookup"><span data-stu-id="3859c-593">The directive sets the namespace for the page.</span></span> <span data-ttu-id="3859c-594">`@model` 指示詞不需要包含命名空間。</span><span class="sxs-lookup"><span data-stu-id="3859c-594">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="3859c-595">當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。</span><span class="sxs-lookup"><span data-stu-id="3859c-595">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="3859c-596">所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3859c-596">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="3859c-597">例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：</span><span class="sxs-lookup"><span data-stu-id="3859c-597">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="3859c-598">*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="3859c-598">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="3859c-599">為 *Pages/Customers/Edit.cshtml* Razor 頁面產生的命名空間和 `PageModel` 類別相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-599">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="3859c-600">`@namespace`  *也適用於傳統的 Razor 檢視。*</span><span class="sxs-lookup"><span data-stu-id="3859c-600">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="3859c-601">原始的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-601">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="3859c-602">更新的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="3859c-602">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="3859c-603">[Razor Pages 入門專案](#rpvs17)包含 *Pages/_ValidationScriptsPartial.cshtml*，連結用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="3859c-603">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="3859c-604">如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。</span><span class="sxs-lookup"><span data-stu-id="3859c-604">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="3859c-605">產生頁面 URL</span><span class="sxs-lookup"><span data-stu-id="3859c-605">URL generation for Pages</span></span>

<span data-ttu-id="3859c-606">前面出現過的 `Create` 頁面使用 `RedirectToPage`：</span><span class="sxs-lookup"><span data-stu-id="3859c-606">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="3859c-607">應用程式有下列檔案/資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="3859c-607">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="3859c-608">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="3859c-608">*/Pages*</span></span>

  * <span data-ttu-id="3859c-609">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-609">*Index.cshtml*</span></span>
  * <span data-ttu-id="3859c-610">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="3859c-610">*/Customers*</span></span>

    * <span data-ttu-id="3859c-611">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-611">*Create.cshtml*</span></span>
    * <span data-ttu-id="3859c-612">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-612">*Edit.cshtml*</span></span>
    * <span data-ttu-id="3859c-613">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="3859c-613">*Index.cshtml*</span></span>

<span data-ttu-id="3859c-614">*Pages/Customers/Create.cshtml* 和 *Pages/Customers/Edit.cshtml* 頁面在成功後會重新導向至 *Pages/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="3859c-614">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="3859c-615">字串 `/Index` 為 URI 的一部分，可存取前一個頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-615">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="3859c-616">字串 `/Index` 可以用來產生 *Pages/Index.cshtml* 頁面的 URI。</span><span class="sxs-lookup"><span data-stu-id="3859c-616">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="3859c-617">例如:</span><span class="sxs-lookup"><span data-stu-id="3859c-617">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="3859c-618">頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。</span><span class="sxs-lookup"><span data-stu-id="3859c-618">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="3859c-619">上述 URL 產生範例，透過硬式編碼的 URL 提供更加優異的選項與功能。</span><span class="sxs-lookup"><span data-stu-id="3859c-619">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="3859c-620">URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。</span><span class="sxs-lookup"><span data-stu-id="3859c-620">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="3859c-621">產生頁面 URL 支援相關的名稱。</span><span class="sxs-lookup"><span data-stu-id="3859c-621">URL generation for pages supports relative names.</span></span> <span data-ttu-id="3859c-622">下表顯示從 *Pages/Customers/Create.cshtml* 以不同的 `RedirectToPage` 參數選取的索引頁：</span><span class="sxs-lookup"><span data-stu-id="3859c-622">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="3859c-623">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="3859c-623">RedirectToPage(x)</span></span>| <span data-ttu-id="3859c-624">頁面</span><span class="sxs-lookup"><span data-stu-id="3859c-624">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="3859c-625">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-625">RedirectToPage("/Index")</span></span> | <span data-ttu-id="3859c-626">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-626">*Pages/Index*</span></span> |
| <span data-ttu-id="3859c-627">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="3859c-627">RedirectToPage("./Index");</span></span> | <span data-ttu-id="3859c-628">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-628">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="3859c-629">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-629">RedirectToPage("../Index")</span></span> | <span data-ttu-id="3859c-630">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-630">*Pages/Index*</span></span> |
| <span data-ttu-id="3859c-631">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="3859c-631">RedirectToPage("Index")</span></span>  | <span data-ttu-id="3859c-632">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="3859c-632">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="3859c-633">`RedirectToPage("Index")`、`RedirectToPage("./Index")` 和 `RedirectToPage("../Index")` 是「相對名稱」。</span><span class="sxs-lookup"><span data-stu-id="3859c-633">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="3859c-634">`RedirectToPage` 參數「結合」了目前頁面的路徑，以計算目的地頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="3859c-634">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="3859c-635">相對名稱連結在以複雜結構建置網站時很有用。</span><span class="sxs-lookup"><span data-stu-id="3859c-635">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="3859c-636">如果您使用相對名稱連結資料夾中的頁面，您可以重新命名該資料夾。</span><span class="sxs-lookup"><span data-stu-id="3859c-636">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="3859c-637">所有連結仍可運作 (因為它們不包含資料夾名稱)。</span><span class="sxs-lookup"><span data-stu-id="3859c-637">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="3859c-638">若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：</span><span class="sxs-lookup"><span data-stu-id="3859c-638">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="3859c-639">如需詳細資訊，請參閱<xref:mvc/controllers/areas>。</span><span class="sxs-lookup"><span data-stu-id="3859c-639">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="3859c-640">ViewData 屬性</span><span class="sxs-lookup"><span data-stu-id="3859c-640">ViewData attribute</span></span>

<span data-ttu-id="3859c-641">資料可以傳遞至具有 [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute) 的頁面。</span><span class="sxs-lookup"><span data-stu-id="3859c-641">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="3859c-642">控制器或 Razor 頁面模型上裝飾以 `[ViewData]` 的屬性會將其值儲存在 [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary) 並從中載入。</span><span class="sxs-lookup"><span data-stu-id="3859c-642">Properties on controllers or Razor Page models decorated with `[ViewData]` have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="3859c-643">在下列範例中，`AboutModel` 包含裝飾以 `[ViewData]` 的 `Title`屬性。</span><span class="sxs-lookup"><span data-stu-id="3859c-643">In the following example, the `AboutModel` contains a `Title` property decorated with `[ViewData]`.</span></span> <span data-ttu-id="3859c-644">`Title` 屬性會設定為 [關於] 頁面的標題：</span><span class="sxs-lookup"><span data-stu-id="3859c-644">The `Title` property is set to the title of the About page:</span></span>

```csharp
public class AboutModel : PageModel
{
    [ViewData]
    public string Title { get; } = "About";

    public void OnGet()
    {
    }
}
```

<span data-ttu-id="3859c-645">在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：</span><span class="sxs-lookup"><span data-stu-id="3859c-645">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="3859c-646">在此配置中，標題會從 ViewData 字典中讀取：</span><span class="sxs-lookup"><span data-stu-id="3859c-646">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="3859c-647">TempData</span><span class="sxs-lookup"><span data-stu-id="3859c-647">TempData</span></span>

<span data-ttu-id="3859c-648">ASP.NET Core 公開[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)上的 [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 屬性。</span><span class="sxs-lookup"><span data-stu-id="3859c-648">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="3859c-649">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="3859c-649">This property stores data until it's read.</span></span> <span data-ttu-id="3859c-650">`Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="3859c-650">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="3859c-651">當有多個要求需要資料時，`TempData` 對重新導向很有幫助。</span><span class="sxs-lookup"><span data-stu-id="3859c-651">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="3859c-652">下列程式碼會設定使用 `TempData` 的 `Message` 值：</span><span class="sxs-lookup"><span data-stu-id="3859c-652">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="3859c-653">*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `TempData` 的 `Message` 值。</span><span class="sxs-lookup"><span data-stu-id="3859c-653">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="3859c-654">*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="3859c-654">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="3859c-655">如需詳細資訊，請參閱 [TempData](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="3859c-655">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="3859c-656">每頁面有多個處理常式</span><span class="sxs-lookup"><span data-stu-id="3859c-656">Multiple handlers per page</span></span>

<span data-ttu-id="3859c-657">下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：</span><span class="sxs-lookup"><span data-stu-id="3859c-657">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="3859c-658">上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="3859c-658">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="3859c-659">`asp-page-handler` 屬性附隨於 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="3859c-659">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="3859c-660">`asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="3859c-660">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="3859c-661">因為範例連結至目前的頁面，所以未指定 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="3859c-661">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="3859c-662">頁面模型：</span><span class="sxs-lookup"><span data-stu-id="3859c-662">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="3859c-663">上述程式碼使用「具名的處理常式方法」。</span><span class="sxs-lookup"><span data-stu-id="3859c-663">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="3859c-664">具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。</span><span class="sxs-lookup"><span data-stu-id="3859c-664">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="3859c-665">在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。</span><span class="sxs-lookup"><span data-stu-id="3859c-665">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="3859c-666">移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-666">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="3859c-667">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-667">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="3859c-668">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-668">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="3859c-669">自訂路由</span><span class="sxs-lookup"><span data-stu-id="3859c-669">Custom routes</span></span>

<span data-ttu-id="3859c-670">使用 `@page` 指示詞，可以：</span><span class="sxs-lookup"><span data-stu-id="3859c-670">Use the `@page` directive to:</span></span>

* <span data-ttu-id="3859c-671">指定頁面的自訂路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-671">Specify a custom route to a page.</span></span> <span data-ttu-id="3859c-672">例如，[關於] 頁面的路由可使用 `@page "/Some/Other/Path"` 設為 `/Some/Other/Path`。</span><span class="sxs-lookup"><span data-stu-id="3859c-672">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="3859c-673">將區段附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-673">Append segments to a page's default route.</span></span> <span data-ttu-id="3859c-674">例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-674">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="3859c-675">將參數附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-675">Append parameters to a page's default route.</span></span> <span data-ttu-id="3859c-676">例如，具有 `@page "{id}"` 的頁面可要求識別碼參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="3859c-676">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="3859c-677">支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="3859c-677">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="3859c-678">例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。</span><span class="sxs-lookup"><span data-stu-id="3859c-678">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="3859c-679">您可以藉由指定路由範本 `@page "{handler?}"`，將 URL 中的查詢字串 `?handler=JoinList` 變更為路由區段 `/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-679">You can change the query string `?handler=JoinList` in the URL to a route segment `/JoinList` by specifying the route template `@page "{handler?}"`.</span></span>

<span data-ttu-id="3859c-680">如果您不喜歡 URL 有查詢字串 `?handler=JoinList`，您可以變更路由，將處理常式名稱置於 URL 的路徑部分。</span><span class="sxs-lookup"><span data-stu-id="3859c-680">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="3859c-681">您可以新增路由範本，在 `@page` 指示詞後面用雙引號括住，以自訂路由。</span><span class="sxs-lookup"><span data-stu-id="3859c-681">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="3859c-682">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="3859c-682">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="3859c-683">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="3859c-683">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="3859c-684">跟在 `handler` 後面的 `?` 表示路由參數為選擇性。</span><span class="sxs-lookup"><span data-stu-id="3859c-684">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="3859c-685">組態與設定</span><span class="sxs-lookup"><span data-stu-id="3859c-685">Configuration and settings</span></span>

<span data-ttu-id="3859c-686">若要設定進階選項，請在 MVC 產生器上使用擴充方法 `AddRazorPagesOptions`：</span><span class="sxs-lookup"><span data-stu-id="3859c-686">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="3859c-687">目前可以使用 `RazorPagesOptions` 設定頁面的根目錄，或新增頁面的應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="3859c-687">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="3859c-688">我們將在未來以這種方式獲得更多的擴充性。</span><span class="sxs-lookup"><span data-stu-id="3859c-688">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="3859c-689">若要先行編譯檢視，請參閱 [Razor 檢視編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="3859c-689">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="3859c-690">[下載或檢視範例程式碼](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="3859c-690">[Download or view sample code](https://github.com/aspnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="3859c-691">請參閱根據本簡介編纂的[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="3859c-691">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="3859c-692">指定 Razor Pages 位於內容根目錄</span><span class="sxs-lookup"><span data-stu-id="3859c-692">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="3859c-693">根據預設，Razor Pages 位於 */Pages* 根目錄。</span><span class="sxs-lookup"><span data-stu-id="3859c-693">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="3859c-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span><span class="sxs-lookup"><span data-stu-id="3859c-694">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="3859c-695">指定 Razor Pages 位於自訂根目錄</span><span class="sxs-lookup"><span data-stu-id="3859c-695">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="3859c-696">將 [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) 新增至 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 可指定 Razor 頁面位於應用程式的自訂根目錄 (提供相對路徑)：</span><span class="sxs-lookup"><span data-stu-id="3859c-696">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="3859c-697">其他資源</span><span class="sxs-lookup"><span data-stu-id="3859c-697">Additional resources</span></span>

* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
