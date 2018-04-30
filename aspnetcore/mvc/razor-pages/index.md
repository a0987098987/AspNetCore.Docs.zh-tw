---
title: ASP.NET Core 中的 Razor Pages 簡介
author: Rick-Anderson
description: 了解 ASP.NET Core 中的 Razor Pages 如何使注重頁面的案例編碼變得更輕鬆，並增加生產力，達到比使用 MVC 更好的成效。
manager: wpickett
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 09/12/2017
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: get-started-article
uid: mvc/razor-pages/index
ms.openlocfilehash: 5e2b53a4771a97b0a4091f593720b9c0e4e345bf
ms.sourcegitcommit: c4a31aaf902f2e84aaf4a9d882ca980fdf6488c0
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/23/2018
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="78673-103">ASP.NET Core 中的 Razor Pages 簡介</span><span class="sxs-lookup"><span data-stu-id="78673-103">Introduction to Razor Pages in ASP.NET Core</span></span>

<span data-ttu-id="78673-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="78673-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

<span data-ttu-id="78673-105">Razor Pages 是 ASP.NET Core MVC 的新功能，更容易編寫以頁面為焦點的案例程式碼，也更具生產力。</span><span class="sxs-lookup"><span data-stu-id="78673-105">Razor Pages is a new feature of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="78673-106">如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="78673-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="78673-107">本文件提供 Razor Pages 簡介。</span><span class="sxs-lookup"><span data-stu-id="78673-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="78673-108">它不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="78673-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="78673-109">如果您發現某些章節很難遵循，請參閱[9開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="78673-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="78673-110">如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="78673-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="78673-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="78673-111">Prerequisites</span></span>

[!INCLUDE [](~/includes/net-core-prereqs.md)]

<a name="rpvs17"></a>

## <a name="creating-a-razor-pages-project"></a><span data-ttu-id="78673-112">建立 Razor Pages 專案</span><span class="sxs-lookup"><span data-stu-id="78673-112">Creating a Razor Pages project</span></span>

# <a name="visual-studiotabvisual-studio"></a>[<span data-ttu-id="78673-113">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="78673-113">Visual Studio</span></span>](#tab/visual-studio) 

<span data-ttu-id="78673-114">如需如何使用 Visual Studio 建立 Razor Pages 專案的詳細說明，請參閱[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="78673-114">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project using Visual Studio.</span></span>

# <a name="visual-studio-for-mactabvisual-studio-mac"></a>[<span data-ttu-id="78673-115">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="78673-115">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="78673-116">從命令列執行 `dotnet new razor`。</span><span class="sxs-lookup"><span data-stu-id="78673-116">Run `dotnet new razor` from the command line.</span></span>

<span data-ttu-id="78673-117">從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="78673-117">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-codetabvisual-studio-code"></a>[<span data-ttu-id="78673-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="78673-118">Visual Studio Code</span></span>](#tab/visual-studio-code) 

<span data-ttu-id="78673-119">從命令列執行 `dotnet new razor`。</span><span class="sxs-lookup"><span data-stu-id="78673-119">Run `dotnet new razor` from the command line.</span></span>

# <a name="net-core-clitabnetcore-cli"></a>[<span data-ttu-id="78673-120">.NET Core CLI</span><span class="sxs-lookup"><span data-stu-id="78673-120">.NET Core CLI</span></span>](#tab/netcore-cli) 

<span data-ttu-id="78673-121">從命令列執行 `dotnet new razor`。</span><span class="sxs-lookup"><span data-stu-id="78673-121">Run `dotnet new razor` from the command line.</span></span>

---

## <a name="razor-pages"></a><span data-ttu-id="78673-122">Razor 頁面</span><span class="sxs-lookup"><span data-stu-id="78673-122">Razor Pages</span></span>

<span data-ttu-id="78673-123">Razor Pages 是在 *Startup.cs* 中啟用：</span><span class="sxs-lookup"><span data-stu-id="78673-123">Razor Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="78673-124">請考慮使用基本頁面：<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="78673-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="78673-125">上述程式碼看起來很像 Razor 檢視檔案。</span><span class="sxs-lookup"><span data-stu-id="78673-125">The preceding code looks a lot like a Razor view file.</span></span> <span data-ttu-id="78673-126">讓它不同的是 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="78673-126">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="78673-127">`@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。</span><span class="sxs-lookup"><span data-stu-id="78673-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="78673-128">`@page` 必須是頁面上的第一個 Razor 指示詞。</span><span class="sxs-lookup"><span data-stu-id="78673-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="78673-129">`@page` 會影響其他的 Razor 建構行為。</span><span class="sxs-lookup"><span data-stu-id="78673-129">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="78673-130">使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。</span><span class="sxs-lookup"><span data-stu-id="78673-130">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="78673-131">*Pages/Index2.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-131">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="78673-132">*Pages/Index2.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="78673-132">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="78673-133">依照慣例，`PageModel` 類別檔和附加 *.cs* 檔名的 Razor Page 檔案名稱相同。</span><span class="sxs-lookup"><span data-stu-id="78673-133">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="78673-134">例如，前一個 Razor Page 是 *Pages/Index2.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="78673-134">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="78673-135">包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="78673-135">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="78673-136">頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。</span><span class="sxs-lookup"><span data-stu-id="78673-136">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="78673-137">下表顯示 Razor 頁面路徑和相符的 URL：</span><span class="sxs-lookup"><span data-stu-id="78673-137">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="78673-138">檔案名稱和路徑</span><span class="sxs-lookup"><span data-stu-id="78673-138">File name and path</span></span>               | <span data-ttu-id="78673-139">比對 URL</span><span class="sxs-lookup"><span data-stu-id="78673-139">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="78673-140">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-140">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="78673-141">`/` 或 `/Index`</span><span class="sxs-lookup"><span data-stu-id="78673-141">`/` or `/Index`</span></span> |
| <span data-ttu-id="78673-142">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-142">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="78673-143">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-143">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="78673-144">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-144">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="78673-145">`/Store` 或 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="78673-145">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="78673-146">附註：</span><span class="sxs-lookup"><span data-stu-id="78673-146">Notes:</span></span>

* <span data-ttu-id="78673-147">執行階段預設會在 *Pages* 資料夾中尋找 Razor Pages 的檔案。</span><span class="sxs-lookup"><span data-stu-id="78673-147">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="78673-148">`Index` 是 URL 未包含頁面時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-148">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="writing-a-basic-form"></a><span data-ttu-id="78673-149">撰寫基本表單</span><span class="sxs-lookup"><span data-stu-id="78673-149">Writing a basic form</span></span>

<span data-ttu-id="78673-150">Razor 頁面功能旨在讓常見模式容易搭配網頁瀏覽器使用。</span><span class="sxs-lookup"><span data-stu-id="78673-150">Razor Pages features are designed to make common patterns used with web browsers easy.</span></span> <span data-ttu-id="78673-151">[模型繫結](xref:mvc/models/model-binding)、[標記協助程式](xref:mvc/views/tag-helpers/intro)和 HTML 協助程式搭配 Razor Page 類別中定義的屬性「就這麼簡單」。</span><span class="sxs-lookup"><span data-stu-id="78673-151">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="78673-152">`Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：</span><span class="sxs-lookup"><span data-stu-id="78673-152">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="78673-153">本文件中的範例，會在 [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) 檔案中初始化 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="78673-153">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/aspnet/Docs/blob/master/aspnetcore/mvc/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="78673-154">資料模型：</span><span class="sxs-lookup"><span data-stu-id="78673-154">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="78673-155">DB 內容：</span><span class="sxs-lookup"><span data-stu-id="78673-155">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="78673-156">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-156">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="78673-157">*Pages/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="78673-157">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="78673-158">依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="78673-158">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="78673-159">`PageModel` 類別可以分離頁面邏輯與頁面展示。</span><span class="sxs-lookup"><span data-stu-id="78673-159">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="78673-160">此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。</span><span class="sxs-lookup"><span data-stu-id="78673-160">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="78673-161">分離頁面邏輯與頁面展示可讓您透過[相依性插入](xref:fundamentals/dependency-injection)來管理頁面相依性，以及針對頁面進行[單元測試](xref:testing/razor-pages-testing)。</span><span class="sxs-lookup"><span data-stu-id="78673-161">This separation allows you to manage page dependencies through [dependency injection](xref:fundamentals/dependency-injection) and to [unit test](xref:testing/razor-pages-testing) the pages.</span></span>

<span data-ttu-id="78673-162">在 `POST` 要求上執行的頁面具有 `OnPostAsync`「處理常式方法」 (當使用者張貼表單時)。</span><span class="sxs-lookup"><span data-stu-id="78673-162">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="78673-163">您可以新增任何 HTTP 指令動詞的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="78673-163">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="78673-164">最常見的處理常式包括：</span><span class="sxs-lookup"><span data-stu-id="78673-164">The most common handlers are:</span></span>

* <span data-ttu-id="78673-165">`OnGet`，初始化頁所需要的狀態。</span><span class="sxs-lookup"><span data-stu-id="78673-165">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="78673-166">[OnGet](#OnGet) 範例。</span><span class="sxs-lookup"><span data-stu-id="78673-166">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="78673-167">`OnPost`，處理表單提交作業。</span><span class="sxs-lookup"><span data-stu-id="78673-167">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="78673-168">`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。</span><span class="sxs-lookup"><span data-stu-id="78673-168">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="78673-169">上例中的 `OnPostAsync` 程式碼看起來類似您一般在控制器中撰寫的內容。</span><span class="sxs-lookup"><span data-stu-id="78673-169">The `OnPostAsync` code in the preceding example looks similar to what you would normally write in a controller.</span></span> <span data-ttu-id="78673-170">上述程式碼一般用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-170">The preceding code is typical for Razor Pages.</span></span> <span data-ttu-id="78673-171">大部分的基本 MVC，像是[模型繫結](xref:mvc/models/model-binding)、[驗證](xref:mvc/models/validation)和動作結果都是共用的。</span><span class="sxs-lookup"><span data-stu-id="78673-171">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results are shared.</span></span>  <!-- Review: Ryan, can we get a list of what is shared and what isn't? -->

<span data-ttu-id="78673-172">前一個 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="78673-172">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="78673-173">`OnPostAsync` 的基本流程：</span><span class="sxs-lookup"><span data-stu-id="78673-173">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="78673-174">檢查驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="78673-174">Check for validation errors.</span></span>

*  <span data-ttu-id="78673-175">如果沒有任何錯誤，會儲存資料並重新導向。</span><span class="sxs-lookup"><span data-stu-id="78673-175">If there are no errors, save the data and redirect.</span></span>
*  <span data-ttu-id="78673-176">如果有錯誤，會再次顯示有驗證訊息的頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-176">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="78673-177">用戶端驗證和傳統的 ASP.NET Core MVC 應用程式完全相同。</span><span class="sxs-lookup"><span data-stu-id="78673-177">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="78673-178">在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。</span><span class="sxs-lookup"><span data-stu-id="78673-178">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="78673-179">成功輸入資料後，`OnPostAsync` 處理常式方法會呼叫 `RedirectToPage` 協助程式方法，傳回 `RedirectToPageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="78673-179">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="78673-180">`RedirectToPage` 是新的動作結果，類似於 `RedirectToAction` 或 `RedirectToRoute`，但會針對頁面自訂。</span><span class="sxs-lookup"><span data-stu-id="78673-180">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="78673-181">在上述範例中，它會重新導向至根索引頁面 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="78673-181">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="78673-182">[產生頁面 URL](#url_gen)一節會詳細說明 `RedirectToPage`。</span><span class="sxs-lookup"><span data-stu-id="78673-182">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="78673-183">當提交的表單有驗證錯誤時 (傳遞至伺服器)，`OnPostAsync` 處理常式方法會呼叫 `Page` 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="78673-183">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="78673-184">`Page` 傳回 `PageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="78673-184">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="78673-185">傳回 `Page` 類似於控制站中的動作傳回 `View`。</span><span class="sxs-lookup"><span data-stu-id="78673-185">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="78673-186">`PageResult` 是處理常式方法的預設 <!-- Review  --> 傳回型別。</span><span class="sxs-lookup"><span data-stu-id="78673-186">`PageResult` is the default <!-- Review  --> return type for a handler method.</span></span> <span data-ttu-id="78673-187">傳回 `void` 的處理常式方法會呈現頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-187">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="78673-188">`Customer` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 加入模型繫結。</span><span class="sxs-lookup"><span data-stu-id="78673-188">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

<span data-ttu-id="78673-189">Razor Pages 預設只繫結屬性和非 GET 指令動詞。</span><span class="sxs-lookup"><span data-stu-id="78673-189">Razor Pages, by default, bind properties only with non-GET verbs.</span></span> <span data-ttu-id="78673-190">繫結至屬性可以減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="78673-190">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="78673-191">透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name" />`) 並接受輸入，繫結可以減少程式碼。</span><span class="sxs-lookup"><span data-stu-id="78673-191">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name" />`) and accept the input.</span></span>

> [!NOTE]
> <span data-ttu-id="78673-192">基於安全性考量，您必須加入才能將 GET 要求資料繫結至頁面模型屬性。</span><span class="sxs-lookup"><span data-stu-id="78673-192">For security reasons, you must opt in to binding GET request data to page model properties.</span></span> <span data-ttu-id="78673-193">請先驗證使用者輸入再將其對應至屬性。</span><span class="sxs-lookup"><span data-stu-id="78673-193">Verify user input before mapping it to properties.</span></span> <span data-ttu-id="78673-194">在建置依靠查詢字串或路由值的功能時，加入此行為相當實用。</span><span class="sxs-lookup"><span data-stu-id="78673-194">Opting in to this behavior is useful when building features which rely on query string or route values.</span></span>
>
> <span data-ttu-id="78673-195">若要在 GET 要求上繫結屬性，請將 `[BindProperty]` 屬性的 `SupportsGet` 屬性設定為 `true`：`[BindProperty(SupportsGet = true)]`</span><span class="sxs-lookup"><span data-stu-id="78673-195">To bind a property on GET requests, set the `[BindProperty]` attribute's `SupportsGet` property to `true`: `[BindProperty(SupportsGet = true)]`</span></span>

<span data-ttu-id="78673-196">首頁 (*Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="78673-196">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="78673-197">程式碼後置 *Index.cshtml.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-197">The code behind *Index.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="78673-198">*Index.cshtml* 檔案包含下列標記可為每個連絡人建立編輯連結：</span><span class="sxs-lookup"><span data-stu-id="78673-198">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="78673-199">[錨定標記協助程式](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)過去使用 `asp-route-{value}` 屬性產生 [編輯] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="78673-199">The [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="78673-200">該連結包含路由資料和連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="78673-200">The link contains route data with the contact ID.</span></span> <span data-ttu-id="78673-201">例如，`http://localhost:5000/Edit/1`。</span><span class="sxs-lookup"><span data-stu-id="78673-201">For example, `http://localhost:5000/Edit/1`.</span></span>

<span data-ttu-id="78673-202">*Pages/Edit.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-202">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="78673-203">第一行包含 `@page "{id:int}"` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="78673-203">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="78673-204">路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="78673-204">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="78673-205">如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="78673-205">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="78673-206">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="78673-206">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="78673-207">*Pages/Edit.cshtml.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-207">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="78673-208">*Index.cshtml* 檔案也包含能夠為每個客戶連絡人建立刪除按鈕的標記：</span><span class="sxs-lookup"><span data-stu-id="78673-208">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="78673-209">使用 HTML 轉譯刪除按鈕時，其 `formaction` 會包含下列項目的參數：</span><span class="sxs-lookup"><span data-stu-id="78673-209">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="78673-210">`asp-route-id` 屬性指定的客戶連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="78673-210">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="78673-211">`asp-page-handler` 屬性指定的 `handler`。</span><span class="sxs-lookup"><span data-stu-id="78673-211">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="78673-212">以下是轉譯的刪除按鈕範例，內含客戶連絡人識別碼 `1`：</span><span class="sxs-lookup"><span data-stu-id="78673-212">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="78673-213">選取按鈕時，表單 `POST` 要求會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="78673-213">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="78673-214">依照慣例，會依據配置 `OnPost[handler]Async` 按 `handler` 參數的值來選取處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="78673-214">By convention, the name of the handler method is selected based the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="78673-215">在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。</span><span class="sxs-lookup"><span data-stu-id="78673-215">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="78673-216">若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的頁面處理常式。</span><span class="sxs-lookup"><span data-stu-id="78673-216">If the `asp-page-handler` is set to a different value, such as `remove`, a page handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="78673-217">`OnPostDeleteAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="78673-217">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="78673-218">接受查詢字串的 `id`。</span><span class="sxs-lookup"><span data-stu-id="78673-218">Accepts the `id` from the query string.</span></span>
* <span data-ttu-id="78673-219">使用 `FindAsync` 在資料庫中查詢客戶連絡人。</span><span class="sxs-lookup"><span data-stu-id="78673-219">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="78673-220">若找到客戶連絡人，會從客戶連絡人清單中予以移除。</span><span class="sxs-lookup"><span data-stu-id="78673-220">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="78673-221">資料庫隨即更新。</span><span class="sxs-lookup"><span data-stu-id="78673-221">The database is updated.</span></span>
* <span data-ttu-id="78673-222">呼叫 `RedirectToPage` 以重新導向至根索引頁 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="78673-222">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="78673-223">XSRF/CSRF 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="78673-223">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="78673-224">您不必撰寫任何[防偽驗證](xref:security/anti-request-forgery)程式碼。</span><span class="sxs-lookup"><span data-stu-id="78673-224">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="78673-225">防偽權杖的產生和驗證會自動包含在 Razor 頁面中。</span><span class="sxs-lookup"><span data-stu-id="78673-225">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>
## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="78673-226">搭配 Razor 頁面使用版面配置、部分、範本和標記協助程式。</span><span class="sxs-lookup"><span data-stu-id="78673-226">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="78673-227">頁面使用 Razor 檢視引擎的所有功能。</span><span class="sxs-lookup"><span data-stu-id="78673-227">Pages work with all the features of the Razor view engine.</span></span> <span data-ttu-id="78673-228">版面配置、部分、範本、標記協助程式、*_ViewStart.cshtml*、*_ViewImports.cshtml* 運作方式一如它們在傳統 Razor 檢視中的方式。</span><span class="sxs-lookup"><span data-stu-id="78673-228">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="78673-229">利用這些功能的一部分來清理此頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-229">Let's declutter this page by taking advantage of some of those features.</span></span>

<span data-ttu-id="78673-230">將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="78673-230">Add a [layout page](xref:mvc/views/layout) to *Pages/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="78673-231">[配置](xref:mvc/views/layout)：</span><span class="sxs-lookup"><span data-stu-id="78673-231">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="78673-232">控制每個頁面的版面配置 (除非頁面退出版面配置)。</span><span class="sxs-lookup"><span data-stu-id="78673-232">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="78673-233">匯入 HTML 結構，例如 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="78673-233">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="78673-234">如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="78673-234">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="78673-235">[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：</span><span class="sxs-lookup"><span data-stu-id="78673-235">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="78673-236">**注意：**版面配置是在 *Pages* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="78673-236">**Note:** The layout is in the *Pages* folder.</span></span> <span data-ttu-id="78673-237">頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。</span><span class="sxs-lookup"><span data-stu-id="78673-237">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="78673-238">您可以從任何 Razor 頁面下的 *Pages* 資料夾中，使用 *Pages* 資料夾中的版面配置。</span><span class="sxs-lookup"><span data-stu-id="78673-238">A layout in the *Pages* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="78673-239">我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="78673-239">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="78673-240">*Views/Shared* 是 MVC 檢視模式。</span><span class="sxs-lookup"><span data-stu-id="78673-240">*Views/Shared* is an MVC views pattern.</span></span> <span data-ttu-id="78673-241">Razor 頁面應該要依賴資料夾階層，不是路徑慣例。</span><span class="sxs-lookup"><span data-stu-id="78673-241">Razor Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="78673-242">Razor 頁面的檢視搜尋包括 *Pages* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="78673-242">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="78673-243">搭配 MVC 控制器使用的版面配置、範本和部分以及傳統的 Razor 檢視「就這麼簡單」。</span><span class="sxs-lookup"><span data-stu-id="78673-243">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="78673-244">新增 *Pages/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-244">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="78673-245">本教學課程稍後會說明 `@namespace`。</span><span class="sxs-lookup"><span data-stu-id="78673-245">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="78673-246">`@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-246">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="78673-247">在頁面上明確使用 `@namespace` 指示詞時：</span><span class="sxs-lookup"><span data-stu-id="78673-247">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="78673-248">指示詞會設定頁面的命名空間。</span><span class="sxs-lookup"><span data-stu-id="78673-248">The directive sets the namespace for the page.</span></span> <span data-ttu-id="78673-249">`@model` 指示詞不需要包含命名空間。</span><span class="sxs-lookup"><span data-stu-id="78673-249">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="78673-250">當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。</span><span class="sxs-lookup"><span data-stu-id="78673-250">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="78673-251">所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="78673-251">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="78673-252">例如，程式碼後置檔案*Pages/Customers/Edit.cshtml.cs* 會以明確方式設定命名空間：</span><span class="sxs-lookup"><span data-stu-id="78673-252">For example, the code behind file *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="78673-253">*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="78673-253">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="78673-254">針對 *Pages/Customers/Edit.cshtml* Razor 頁面產生的命名空間和程式碼後置檔案相同。</span><span class="sxs-lookup"><span data-stu-id="78673-254">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the code behind file.</span></span> <span data-ttu-id="78673-255">`@namespace` 指示詞的設計是為了將 C# 類別新增至專案，頁面產生的程式碼「就這麼簡單」，不必為程式碼後置檔案新增 `@using` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="78673-255">The `@namespace` directive was designed so the C# classes added to a project and pages-generated code *just work* without having to add an `@using` directive for the code behind file.</span></span>

<span data-ttu-id="78673-256">**注意：**`@namespace`也適用於傳統的 Razor 檢視。</span><span class="sxs-lookup"><span data-stu-id="78673-256">**Note:** `@namespace` also works with conventional Razor views.</span></span>

<span data-ttu-id="78673-257">原始的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-257">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="78673-258">更新的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="78673-258">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="78673-259">[Razor Pages 入門專案](#rpvs17)包含 *Pages/_ValidationScriptsPartial.cshtml*，連結用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="78673-259">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="78673-260">產生頁面 URL</span><span class="sxs-lookup"><span data-stu-id="78673-260">URL generation for Pages</span></span>

<span data-ttu-id="78673-261">前面出現過的 `Create` 頁面使用 `RedirectToPage`：</span><span class="sxs-lookup"><span data-stu-id="78673-261">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="78673-262">應用程式有下列檔案/資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="78673-262">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="78673-263">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="78673-263">*/Pages*</span></span>

  * <span data-ttu-id="78673-264">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-264">*Index.cshtml*</span></span>
  * <span data-ttu-id="78673-265">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="78673-265">*/Customers*</span></span>

    * <span data-ttu-id="78673-266">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-266">*Create.cshtml*</span></span>
    * <span data-ttu-id="78673-267">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-267">*Edit.cshtml*</span></span>
    * <span data-ttu-id="78673-268">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="78673-268">*Index.cshtml*</span></span>

<span data-ttu-id="78673-269">*Pages/Customers/Create.cshtml* 和 *Pages/Customers/Edit.cshtml* 頁面在成功後會重新導向至 *Pages/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="78673-269">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="78673-270">字串 `/Index` 為 URI 的一部分，可存取前一個頁面。</span><span class="sxs-lookup"><span data-stu-id="78673-270">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="78673-271">字串 `/Index` 可以用來產生 *Pages/Index.cshtml* 頁面的 URI。</span><span class="sxs-lookup"><span data-stu-id="78673-271">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="78673-272">例如: </span><span class="sxs-lookup"><span data-stu-id="78673-272">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="78673-273">頁面名稱是來自根 */Pages* 資料夾的頁面路徑 (包括前置的 `/`，例如 `/Index`)。</span><span class="sxs-lookup"><span data-stu-id="78673-273">The page name is the path to the page from the root */Pages* folder (including a leading `/`, for example `/Index`).</span></span> <span data-ttu-id="78673-274">上述的 URL 產生範例比僅有硬式編碼的 URL 更有特色。</span><span class="sxs-lookup"><span data-stu-id="78673-274">The preceding URL generation samples are much more feature rich than just hardcoding a URL.</span></span> <span data-ttu-id="78673-275">URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。</span><span class="sxs-lookup"><span data-stu-id="78673-275">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="78673-276">產生頁面 URL 支援相關的名稱。</span><span class="sxs-lookup"><span data-stu-id="78673-276">URL generation for pages supports relative names.</span></span> <span data-ttu-id="78673-277">下表顯示從 *Pages/Customers/Create.cshtml* 以不同的 `RedirectToPage` 參數選取的索引頁：</span><span class="sxs-lookup"><span data-stu-id="78673-277">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="78673-278">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="78673-278">RedirectToPage(x)</span></span>| <span data-ttu-id="78673-279">頁面</span><span class="sxs-lookup"><span data-stu-id="78673-279">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="78673-280">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="78673-280">RedirectToPage("/Index")</span></span> | <span data-ttu-id="78673-281">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="78673-281">*Pages/Index*</span></span> |
| <span data-ttu-id="78673-282">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="78673-282">RedirectToPage("./Index");</span></span> | <span data-ttu-id="78673-283">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="78673-283">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="78673-284">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="78673-284">RedirectToPage("../Index")</span></span> | <span data-ttu-id="78673-285">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="78673-285">*Pages/Index*</span></span> |
| <span data-ttu-id="78673-286">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="78673-286">RedirectToPage("Index")</span></span>  | <span data-ttu-id="78673-287">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="78673-287">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="78673-288">`RedirectToPage("Index")`、`RedirectToPage("./Index")` 和 `RedirectToPage("../Index")` 是「相對名稱」。</span><span class="sxs-lookup"><span data-stu-id="78673-288">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are <em>relative names</em>.</span></span> <span data-ttu-id="78673-289">`RedirectToPage` 參數「結合」了目前頁面的路徑，以計算目的地頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="78673-289">The `RedirectToPage` parameter is <em>combined</em> with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="78673-290">相對名稱連結在以複雜結構建置網站時很有用。</span><span class="sxs-lookup"><span data-stu-id="78673-290">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="78673-291">如果您使用相對名稱連結資料夾中的頁面，您可以重新命名該資料夾。</span><span class="sxs-lookup"><span data-stu-id="78673-291">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="78673-292">所有連結仍可運作 (因為它們不包含資料夾名稱)。</span><span class="sxs-lookup"><span data-stu-id="78673-292">All the links still work (because they didn't include the folder name).</span></span>

## <a name="tempdata"></a><span data-ttu-id="78673-293">TempData</span><span class="sxs-lookup"><span data-stu-id="78673-293">TempData</span></span>

<span data-ttu-id="78673-294">ASP.NET Core 公開[控制器](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller)上的 [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 屬性。</span><span class="sxs-lookup"><span data-stu-id="78673-294">ASP.NET Core exposes the [TempData](https://docs.microsoft.com/en-us/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="78673-295">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="78673-295">This property stores data until it's read.</span></span> <span data-ttu-id="78673-296">`Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="78673-296">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="78673-297">當有多個要求需要資料時，`TempData` 對重新導向很有幫助。</span><span class="sxs-lookup"><span data-stu-id="78673-297">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="78673-298">`[TempData]` 是 ASP.NET Core 2.0 的新屬性，在控制站和頁面都受支援。</span><span class="sxs-lookup"><span data-stu-id="78673-298">The `[TempData]` attribute is new in ASP.NET Core 2.0 and is supported on controllers and pages.</span></span>

<span data-ttu-id="78673-299">下列程式碼會設定使用 `TempData` 的 `Message` 值：</span><span class="sxs-lookup"><span data-stu-id="78673-299">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="78673-300">*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `TempData` 的 `Message` 值。</span><span class="sxs-lookup"><span data-stu-id="78673-300">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="78673-301">*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="78673-301">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```cs
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="78673-302">如需詳細資訊，請參閱 [TempData](xref:fundamentals/app-state#temp)。</span><span class="sxs-lookup"><span data-stu-id="78673-302">See [TempData](xref:fundamentals/app-state#temp) for more information.</span></span>

<a name="mhpp"></a>
## <a name="multiple-handlers-per-page"></a><span data-ttu-id="78673-303">每頁面有多個處理常式</span><span class="sxs-lookup"><span data-stu-id="78673-303">Multiple handlers per page</span></span>

<span data-ttu-id="78673-304">下列頁面會使用 `asp-page-handler` 標記協助程式為兩個頁面處理常式產生標記：</span><span class="sxs-lookup"><span data-stu-id="78673-304">The following page generates markup for two page handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="78673-305">上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="78673-305">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="78673-306">`asp-page-handler` 屬性附隨於 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="78673-306">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="78673-307">`asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="78673-307">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="78673-308">因為範例連結至目前的頁面，所以未指定 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="78673-308">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="78673-309">頁面模型：</span><span class="sxs-lookup"><span data-stu-id="78673-309">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="78673-310">上述程式碼使用「具名的處理常式方法」。</span><span class="sxs-lookup"><span data-stu-id="78673-310">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="78673-311">具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。</span><span class="sxs-lookup"><span data-stu-id="78673-311">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="78673-312">在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。</span><span class="sxs-lookup"><span data-stu-id="78673-312">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="78673-313">移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="78673-313">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="78673-314">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `http://localhost:5000/Customers/CreateFATH?handler=JoinList`。</span><span class="sxs-lookup"><span data-stu-id="78673-314">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="78673-315">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="78673-315">The URL path that submits to `OnPostJoinListUCAsync` is `http://localhost:5000/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="customizing-routing"></a><span data-ttu-id="78673-316">自訂路由</span><span class="sxs-lookup"><span data-stu-id="78673-316">Customizing Routing</span></span>

<span data-ttu-id="78673-317">如果您不喜歡 URL 有查詢字串 `?handler=JoinList`，您可以變更路由，將處理常式名稱置於 URL 的路徑部分。</span><span class="sxs-lookup"><span data-stu-id="78673-317">If you don't like the query string `?handler=JoinList` in the URL, you can change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="78673-318">您可以新增路由範本，在 `@page` 指示詞後面用雙引號括住，以自訂路由。</span><span class="sxs-lookup"><span data-stu-id="78673-318">You can customize the route by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="78673-319">上述的路由會將處理常式名稱放入 URL 路徑，而不是放入查詢字串。</span><span class="sxs-lookup"><span data-stu-id="78673-319">The preceding route puts the handler name in the URL path instead of the query string.</span></span> <span data-ttu-id="78673-320">跟在 `handler` 後面的 `?` 表示路由參數為選擇性。</span><span class="sxs-lookup"><span data-stu-id="78673-320">The `?` following `handler` means the route parameter is optional.</span></span>

<span data-ttu-id="78673-321">您可以使用 `@page` 將額外的區段和參數新增至頁面的路由。</span><span class="sxs-lookup"><span data-stu-id="78673-321">You can use `@page` to add additional segments and parameters to a page's route.</span></span> <span data-ttu-id="78673-322">其中包含的所有項目都將**附加**至頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="78673-322">Whatever's there's **appended** to the default route of the page.</span></span> <span data-ttu-id="78673-323">不支援使用絕對或虛擬路徑變更頁面的路由 (例如 `"~/Some/Other/Path"`)。</span><span class="sxs-lookup"><span data-stu-id="78673-323">Using an absolute or virtual path to change the page's route (like `"~/Some/Other/Path"`) isn't supported.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="78673-324">組態與設定</span><span class="sxs-lookup"><span data-stu-id="78673-324">Configuration and settings</span></span>

<span data-ttu-id="78673-325">若要設定進階選項，請在 MVC 產生器上使用擴充方法 `AddRazorPagesOptions`：</span><span class="sxs-lookup"><span data-stu-id="78673-325">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="78673-326">目前可以使用 `RazorPagesOptions` 設定頁面的根目錄，或新增頁面的應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="78673-326">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="78673-327">我們將在未來以這種方式獲得更多的擴充性。</span><span class="sxs-lookup"><span data-stu-id="78673-327">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="78673-328">若要先行編譯檢視，請參閱 [Razor 檢視編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="78673-328">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="78673-329">[下載或檢視範例程式碼](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="78673-329">[Download or view sample code](https://github.com/aspnet/Docs/tree/master/aspnetcore/mvc/razor-pages/index/sample).</span></span>

<span data-ttu-id="78673-330">請參閱根據本簡介編纂的[開始使用 Razor Pages](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="78673-330">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="78673-331">指定 Razor Pages 位於內容根目錄</span><span class="sxs-lookup"><span data-stu-id="78673-331">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="78673-332">根據預設，Razor Pages 位於 */Pages* 根目錄。</span><span class="sxs-lookup"><span data-stu-id="78673-332">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="78673-333">將 [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) 新增至 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 可指定 Razor Pages 位於應用程式的內容根目錄 ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath))：</span><span class="sxs-lookup"><span data-stu-id="78673-333">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the content root ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="78673-334">指定 Razor Pages 位於自訂根目錄</span><span class="sxs-lookup"><span data-stu-id="78673-334">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="78673-335">將 [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) 新增至 [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) 可指定 Razor Pages 位於應用程式的自訂根目錄 (提供相對路徑)：</span><span class="sxs-lookup"><span data-stu-id="78673-335">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="see-also"></a><span data-ttu-id="78673-336">另請參閱</span><span class="sxs-lookup"><span data-stu-id="78673-336">See also</span></span>

* [<span data-ttu-id="78673-337">ASP.NET Core 簡介</span><span class="sxs-lookup"><span data-stu-id="78673-337">Introduction to ASP.NET Core</span></span>](xref:index)
* [<span data-ttu-id="78673-338">Razor 語法</span><span class="sxs-lookup"><span data-stu-id="78673-338">Razor syntax</span></span>](xref:mvc/views/razor)
* [<span data-ttu-id="78673-339">開始使用 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="78673-339">Get started with Razor Pages</span></span>](xref:tutorials/razor-pages/razor-pages-start)
* [<span data-ttu-id="78673-340">Razor Pages 授權慣例</span><span class="sxs-lookup"><span data-stu-id="78673-340">Razor Pages authorization conventions</span></span>](xref:security/authorization/razor-pages-authorization)
* [<span data-ttu-id="78673-341">Razor Pages 自訂路由和頁面模型提供者</span><span class="sxs-lookup"><span data-stu-id="78673-341">Razor Pages custom route and page model providers</span></span>](xref:mvc/razor-pages/razor-pages-convention-features)
* [<span data-ttu-id="78673-342">Razor 頁面單元與整合測試</span><span class="sxs-lookup"><span data-stu-id="78673-342">Razor Pages unit and integration tests</span></span>](xref:testing/razor-pages-testing)
