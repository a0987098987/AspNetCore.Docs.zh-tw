---
title: RazorASP.NET Core 中的頁面簡介
author: Rick-Anderson
description: 瞭解 Razor ASP.NET Core 中的頁面如何讓撰寫以頁面為焦點的案例更輕鬆且更具生產力，而不是使用 MVC。
monikerRange: '>= aspnetcore-2.0'
ms.author: riande
ms.date: 02/12/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: razor-pages/index
ms.openlocfilehash: 70f5da1dad9b4c0b9526a7688862637291be9a68
ms.sourcegitcommit: fa67462abdf0cc4051977d40605183c629db7c64
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/10/2020
ms.locfileid: "84652581"
---
# <a name="introduction-to-razor-pages-in-aspnet-core"></a><span data-ttu-id="e273b-103">RazorASP.NET Core 中的頁面簡介</span><span class="sxs-lookup"><span data-stu-id="e273b-103">Introduction to Razor Pages in ASP.NET Core</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="e273b-104">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="e273b-104">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

Razor<span data-ttu-id="e273b-105">頁面可讓撰寫以頁面為焦點的案例更輕鬆且更具生產力，而不是使用控制器和視圖。</span><span class="sxs-lookup"><span data-stu-id="e273b-105"> Pages can make coding page-focused scenarios easier and more productive than using controllers and views.</span></span>

<span data-ttu-id="e273b-106">如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="e273b-106">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="e273b-107">本檔提供 Razor 頁面簡介。</span><span class="sxs-lookup"><span data-stu-id="e273b-107">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="e273b-108">它不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="e273b-108">It's not a step by step tutorial.</span></span> <span data-ttu-id="e273b-109">如果您發現某些區段太先進，請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="e273b-109">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="e273b-110">如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="e273b-110">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e273b-111">必要條件</span><span class="sxs-lookup"><span data-stu-id="e273b-111">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e273b-112">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e273b-112">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs-3.0.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e273b-113">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e273b-113">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-3.0.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e273b-114">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e273b-114">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-3.0.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="e273b-115">建立 Razor 頁面專案</span><span class="sxs-lookup"><span data-stu-id="e273b-115">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e273b-116">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e273b-116">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e273b-117">如需如何建立頁面專案的詳細指示，請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start) Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-117">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e273b-118">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e273b-118">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e273b-119">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="e273b-119">Run `dotnet new webapp` from the command line.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e273b-120">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e273b-120">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e273b-121">如需如何建立頁面專案的詳細指示，請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start) Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-121">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

---

## <a name="razor-pages"></a>Razor<span data-ttu-id="e273b-122">頁面</span><span class="sxs-lookup"><span data-stu-id="e273b-122"> Pages</span></span>

Razor<span data-ttu-id="e273b-123">頁面已在*Startup.cs*中啟用：</span><span class="sxs-lookup"><span data-stu-id="e273b-123"> Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Startup.cs?name=snippet_Startup&highlight=12,36)]

<span data-ttu-id="e273b-124">請考慮使用基本頁面：<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="e273b-124">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index.cshtml?highlight=1)]

<span data-ttu-id="e273b-125">上述程式碼看起來很像是在具有控制器和 views 的 ASP.NET Core 應用程式中使用的[ Razor 視圖](xref:tutorials/first-mvc-app/adding-view)檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-125">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="e273b-126">這會讓指示詞不同 [`@page`](xref:mvc/views/razor#page) 。</span><span class="sxs-lookup"><span data-stu-id="e273b-126">What makes it different is the [`@page`](xref:mvc/views/razor#page) directive.</span></span> <span data-ttu-id="e273b-127">`@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。</span><span class="sxs-lookup"><span data-stu-id="e273b-127">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="e273b-128">`@page`必須是頁面上的第一個指示詞 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-128">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="e273b-129">`@page`會影響其他結構的行為 [Razor](xref:mvc/views/razor) 。</span><span class="sxs-lookup"><span data-stu-id="e273b-129">`@page` affects the behavior of other [Razor](xref:mvc/views/razor) constructs.</span></span> Razor<span data-ttu-id="e273b-130">分頁檔名的開頭為 *..* \* 尾碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-130"> Pages file names have a *.cshtml* suffix.</span></span>

<span data-ttu-id="e273b-131">使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。</span><span class="sxs-lookup"><span data-stu-id="e273b-131">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="e273b-132">*Pages/Index2.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-132">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="e273b-133">*Pages/Index2.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-133">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="e273b-134">依照慣例， `PageModel` 類別檔案的名稱與分頁檔相同， Razor 附加 *.cs* 。</span><span class="sxs-lookup"><span data-stu-id="e273b-134">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="e273b-135">例如，前一 Razor 頁是*Pages/Index2*。</span><span class="sxs-lookup"><span data-stu-id="e273b-135">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="e273b-136">包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="e273b-136">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="e273b-137">頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。</span><span class="sxs-lookup"><span data-stu-id="e273b-137">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="e273b-138">下表顯示 Razor 頁面路徑和相符的 URL：</span><span class="sxs-lookup"><span data-stu-id="e273b-138">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="e273b-139">檔案名稱和路徑</span><span class="sxs-lookup"><span data-stu-id="e273b-139">File name and path</span></span>               | <span data-ttu-id="e273b-140">比對 URL</span><span class="sxs-lookup"><span data-stu-id="e273b-140">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e273b-141">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-141">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="e273b-142">`/` 或 `/Index`</span><span class="sxs-lookup"><span data-stu-id="e273b-142">`/` or `/Index`</span></span> |
| <span data-ttu-id="e273b-143">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-143">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="e273b-144">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-144">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="e273b-145">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-145">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="e273b-146">`/Store` 或 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="e273b-146">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="e273b-147">注意：</span><span class="sxs-lookup"><span data-stu-id="e273b-147">Notes:</span></span>

* <span data-ttu-id="e273b-148">執行時間預設會 Razor 在*pages*資料夾中尋找分頁檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-148">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="e273b-149">`Index` 是 URL 未包含頁面時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-149">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="e273b-150">撰寫基本表單</span><span class="sxs-lookup"><span data-stu-id="e273b-150">Write a basic form</span></span>

Razor<span data-ttu-id="e273b-151">頁面的設計目的是要在建立應用程式時，讓與網頁瀏覽器搭配使用的常見模式變得容易執行。</span><span class="sxs-lookup"><span data-stu-id="e273b-151"> Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="e273b-152">[模型](xref:mvc/models/model-binding)系結、[標記](xref:mvc/views/tag-helpers/intro)協助程式和 HTML 協助程式都*只*會使用頁面類別中定義的屬性 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-152">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="e273b-153">`Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：</span><span class="sxs-lookup"><span data-stu-id="e273b-153">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="e273b-154">本文件中的範例，會在 [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) 檔案中初始化 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="e273b-154">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/3.0sample/RazorPagesContacts/Startup.cs#L23-L24) file.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Startup.cs?name=snippet)]

<span data-ttu-id="e273b-155">資料模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-155">The data model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="e273b-156">DB 內容：</span><span class="sxs-lookup"><span data-stu-id="e273b-156">The db context:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Data/CustomerDbContext.cs)]

<span data-ttu-id="e273b-157">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-157">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="e273b-158">*Pages/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-158">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="e273b-159">依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-159">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="e273b-160">`PageModel` 類別可以分離頁面邏輯與頁面展示。</span><span class="sxs-lookup"><span data-stu-id="e273b-160">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="e273b-161">此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。</span><span class="sxs-lookup"><span data-stu-id="e273b-161">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="e273b-162">這種分隔允許：</span><span class="sxs-lookup"><span data-stu-id="e273b-162">This separation allows:</span></span>

* <span data-ttu-id="e273b-163">透過相依性[插入](xref:fundamentals/dependency-injection)來管理頁面相依性。</span><span class="sxs-lookup"><span data-stu-id="e273b-163">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* [<span data-ttu-id="e273b-164">單元測試</span><span class="sxs-lookup"><span data-stu-id="e273b-164">Unit testing</span></span>](xref:test/razor-pages-tests)

<span data-ttu-id="e273b-165">在 `POST` 要求上執行的頁面具有 「處理常式方法」`OnPostAsync` \*\* (當使用者張貼表單時)。</span><span class="sxs-lookup"><span data-stu-id="e273b-165">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="e273b-166">可以新增任何 HTTP 指令動詞的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-166">Handler methods for any HTTP verb can be added.</span></span> <span data-ttu-id="e273b-167">最常見的處理常式包括：</span><span class="sxs-lookup"><span data-stu-id="e273b-167">The most common handlers are:</span></span>

* <span data-ttu-id="e273b-168">`OnGet`，初始化頁所需要的狀態。</span><span class="sxs-lookup"><span data-stu-id="e273b-168">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="e273b-169">在上述程式碼中， `OnGet` 方法會顯示*CreateModel* Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-169">In the preceding code, the `OnGet` method displays the *CreateModel.cshtml* Razor Page.</span></span>
* <span data-ttu-id="e273b-170">`OnPost`，處理表單提交作業。</span><span class="sxs-lookup"><span data-stu-id="e273b-170">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="e273b-171">`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。</span><span class="sxs-lookup"><span data-stu-id="e273b-171">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="e273b-172">上述程式碼通常用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-172">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="e273b-173">如果您熟悉如何使用控制器和 views 來 ASP.NET 應用程式：</span><span class="sxs-lookup"><span data-stu-id="e273b-173">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="e273b-174">`OnPostAsync`上述範例中的程式碼看起來類似一般的控制器程式碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-174">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="e273b-175">大部分的 MVC 基本類型（例如[模型](xref:mvc/models/model-binding)系結、[驗證](xref:mvc/models/validation)和動作結果）在控制器和頁面上的運作方式都相同 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-175">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), and action results work the same with Controllers and Razor Pages.</span></span> 

<span data-ttu-id="e273b-176">前一個 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="e273b-176">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e273b-177">`OnPostAsync` 的基本流程：</span><span class="sxs-lookup"><span data-stu-id="e273b-177">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="e273b-178">檢查驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="e273b-178">Check for validation errors.</span></span>

* <span data-ttu-id="e273b-179">如果沒有任何錯誤，會儲存資料並重新導向。</span><span class="sxs-lookup"><span data-stu-id="e273b-179">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="e273b-180">如果有錯誤，會再次顯示有驗證訊息的頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-180">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="e273b-181">在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-181">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="e273b-182">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-182">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml)]

<span data-ttu-id="e273b-183">從*Pages/Create. cshtml*轉譯的 HTML：</span><span class="sxs-lookup"><span data-stu-id="e273b-183">The rendered HTML from *Pages/Create.cshtml*:</span></span>

[!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.html)]

<span data-ttu-id="e273b-184">在先前的程式碼中，張貼表單：</span><span class="sxs-lookup"><span data-stu-id="e273b-184">In the previous code, posting the form:</span></span>

* <span data-ttu-id="e273b-185">具有有效的資料：</span><span class="sxs-lookup"><span data-stu-id="e273b-185">With valid data:</span></span>

  * <span data-ttu-id="e273b-186">`OnPostAsync`處理常式方法會呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-186">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> helper method.</span></span> <span data-ttu-id="e273b-187">`RedirectToPage` 傳回 <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult> 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e273b-187">`RedirectToPage` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RedirectToPageResult>.</span></span> <span data-ttu-id="e273b-188">`RedirectToPage`:</span><span class="sxs-lookup"><span data-stu-id="e273b-188">`RedirectToPage`:</span></span>

    * <span data-ttu-id="e273b-189">是動作結果。</span><span class="sxs-lookup"><span data-stu-id="e273b-189">Is an action result.</span></span>
    * <span data-ttu-id="e273b-190">類似于 `RedirectToAction` 或 `RedirectToRoute` （用於控制器和 views）。</span><span class="sxs-lookup"><span data-stu-id="e273b-190">Is similar to `RedirectToAction` or `RedirectToRoute` (used in controllers and views).</span></span>
    * <span data-ttu-id="e273b-191">已針對頁面自訂。</span><span class="sxs-lookup"><span data-stu-id="e273b-191">Is customized for pages.</span></span> <span data-ttu-id="e273b-192">在上述範例中，它會重新導向至根索引頁面 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-192">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="e273b-193">[產生頁面 URL](#url_gen)一節會詳細說明 `RedirectToPage`。</span><span class="sxs-lookup"><span data-stu-id="e273b-193">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

* <span data-ttu-id="e273b-194">傳遞至伺服器的驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="e273b-194">With validation errors that are passed to the server:</span></span>

  * <span data-ttu-id="e273b-195">`OnPostAsync`處理常式方法會呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper 方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-195">The `OnPostAsync` handler method calls the <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageBase.Page*> helper method.</span></span> <span data-ttu-id="e273b-196">`Page` 傳回 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult> 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e273b-196">`Page` returns an instance of <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageResult>.</span></span> <span data-ttu-id="e273b-197">傳回 `Page` 類似於控制站中的動作傳回 `View`。</span><span class="sxs-lookup"><span data-stu-id="e273b-197">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="e273b-198">`PageResult`這是處理常式方法的預設傳回型別。</span><span class="sxs-lookup"><span data-stu-id="e273b-198">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="e273b-199">傳回 `void` 的處理常式方法會呈現頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-199">A handler method that returns `void` renders the page.</span></span>
  * <span data-ttu-id="e273b-200">在上述範例中，張貼不含值的表單會導致[ModelState](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid)傳回 false。</span><span class="sxs-lookup"><span data-stu-id="e273b-200">In the preceding example, posting the form with no value results in [ModelState.IsValid](xref:Microsoft.AspNetCore.Mvc.ModelBinding.ModelStateDictionary.IsValid) returning false.</span></span> <span data-ttu-id="e273b-201">在此範例中，用戶端上不會顯示任何驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="e273b-201">In this sample, no validation errors are displayed on the client.</span></span> <span data-ttu-id="e273b-202">本檔稍後會涵蓋驗證錯誤處理。</span><span class="sxs-lookup"><span data-stu-id="e273b-202">Validation error handing is covered later in this document.</span></span>

  [!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=3-6)]

* <span data-ttu-id="e273b-203">用戶端驗證偵測到驗證錯誤：</span><span class="sxs-lookup"><span data-stu-id="e273b-203">With validation errors detected by client side validation:</span></span>

  * <span data-ttu-id="e273b-204">資料**未**張貼至伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-204">Data is **not** posted to the server.</span></span>
  * <span data-ttu-id="e273b-205">本檔稍後會說明用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-205">Client-side validation is explained later in this document.</span></span>

<span data-ttu-id="e273b-206">`Customer`屬性會使用 [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) 屬性來加入宣告模型系結：</span><span class="sxs-lookup"><span data-stu-id="e273b-206">The `Customer` property uses [`[BindProperty]`](xref:Microsoft.AspNetCore.Mvc.BindPropertyAttribute) attribute to opt in to model binding:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=15-16)]

<span data-ttu-id="e273b-207">`[BindProperty]`**不**應該用於包含不應由用戶端變更之屬性的模型。</span><span class="sxs-lookup"><span data-stu-id="e273b-207">`[BindProperty]` should **not** be used on models containing properties that should not be changed by the client.</span></span> <span data-ttu-id="e273b-208">如需詳細資訊，請參閱[防止大量指派](xref:data/ef-rp/crud#overposting)。</span><span class="sxs-lookup"><span data-stu-id="e273b-208">For more information, see [Overposting](xref:data/ef-rp/crud#overposting).</span></span>

Razor<span data-ttu-id="e273b-209">根據預設，頁面只會系結屬性與非 `GET` 動詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-209"> Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="e273b-210">系結至屬性不需要撰寫程式碼來將 HTTP 資料轉換成模型型別。</span><span class="sxs-lookup"><span data-stu-id="e273b-210">Binding to properties removes the need to writing code to convert HTTP data to the model type.</span></span> <span data-ttu-id="e273b-211">透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-211">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="e273b-212">查看*Pages/Create. cshtml* view file：</span><span class="sxs-lookup"><span data-stu-id="e273b-212">Reviewing the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml?highlight=3,9)]

* <span data-ttu-id="e273b-213">在上述程式碼中，[輸入標記](xref:mvc/views/working-with-forms#the-input-tag-helper)協助程式會將 HTML 專案系結 `<input asp-for="Customer.Name" />` `<input>` 至 `Customer.Name` 模型運算式。</span><span class="sxs-lookup"><span data-stu-id="e273b-213">In the preceding code, the [input tag helper](xref:mvc/views/working-with-forms#the-input-tag-helper) `<input asp-for="Customer.Name" />` binds the HTML `<input>` element to the `Customer.Name` model expression.</span></span>
* <span data-ttu-id="e273b-214">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available)讓標籤協助程式可供使用。</span><span class="sxs-lookup"><span data-stu-id="e273b-214">[`@addTagHelper`](xref:mvc/views/tag-helpers/intro#addtaghelper-makes-tag-helpers-available) makes Tag Helpers available.</span></span>

### <a name="the-home-page"></a><span data-ttu-id="e273b-215">首頁</span><span class="sxs-lookup"><span data-stu-id="e273b-215">The home page</span></span>

<span data-ttu-id="e273b-216">*Index. cshtml*是首頁：</span><span class="sxs-lookup"><span data-stu-id="e273b-216">*Index.cshtml* is the home page:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml)]

<span data-ttu-id="e273b-217">已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="e273b-217">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet)]

<span data-ttu-id="e273b-218">*Index. cshtml*檔案包含下列標記：</span><span class="sxs-lookup"><span data-stu-id="e273b-218">The *Index.cshtml* file contains the following markup:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=21)]

<span data-ttu-id="e273b-219">`<a /a>`[錨點](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)標籤協助程式使用 `asp-route-{value}` 屬性來產生 [編輯] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="e273b-219">The `<a /a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="e273b-220">該連結包含路由資料和連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-220">The link contains route data with the contact ID.</span></span> <span data-ttu-id="e273b-221">例如： `https://localhost:5001/Edit/1` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-221">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="e273b-222">[標記](xref:mvc/views/tag-helpers/intro)協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-222">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span>

<span data-ttu-id="e273b-223">*Index. cshtml*檔案包含標記，以建立每個客戶連絡人的刪除按鈕：</span><span class="sxs-lookup"><span data-stu-id="e273b-223">The *Index.cshtml* file contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml?range=22-23)]

<span data-ttu-id="e273b-224">呈現的 HTML：</span><span class="sxs-lookup"><span data-stu-id="e273b-224">The rendered HTML:</span></span>

```html
<button type="submit" formaction="/Customers?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="e273b-225">當 [刪除] 按鈕以 HTML 轉譯時，其[formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction)會包含的參數：</span><span class="sxs-lookup"><span data-stu-id="e273b-225">When the delete button is rendered in HTML, its [formaction](https://developer.mozilla.org/docs/Web/HTML/Element/button#attr-formaction) includes parameters for:</span></span>

* <span data-ttu-id="e273b-226">由屬性指定的客戶連絡人識別碼 `asp-route-id` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-226">The customer contact ID, specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="e273b-227">`handler`屬性所指定的 `asp-page-handler` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-227">The `handler`, specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="e273b-228">選取按鈕時，表單 `POST` 要求會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-228">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="e273b-229">依照慣例，會依據配置 `OnPost[handler]Async`，按 `handler` 參數的值來選取處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-229">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="e273b-230">在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。</span><span class="sxs-lookup"><span data-stu-id="e273b-230">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="e273b-231">若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-231">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Index.cshtml.cs?name=snippet2)]

<span data-ttu-id="e273b-232">`OnPostDeleteAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="e273b-232">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="e273b-233">`id`從查詢字串取得。</span><span class="sxs-lookup"><span data-stu-id="e273b-233">Gets the `id` from the query string.</span></span>
* <span data-ttu-id="e273b-234">使用 `FindAsync` 在資料庫中查詢客戶連絡人。</span><span class="sxs-lookup"><span data-stu-id="e273b-234">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="e273b-235">如果找到客戶連絡人，就會將其移除並更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="e273b-235">If the customer contact is found, it's removed and the database is updated.</span></span>
* <span data-ttu-id="e273b-236">呼叫 <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> 以重新導向至根索引頁 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-236">Calls <xref:Microsoft.AspNetCore.Mvc.RazorPages.PageModel.RedirectToPage*> to redirect to the root Index page (`/Index`).</span></span>

### <a name="the-editcshtml-file"></a><span data-ttu-id="e273b-237">編輯 cshtml 檔案</span><span class="sxs-lookup"><span data-stu-id="e273b-237">The Edit.cshtml file</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml?highlight=1)]

<span data-ttu-id="e273b-238">第一行包含 `@page "{id:int}"` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-238">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="e273b-239">路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="e273b-239">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="e273b-240">如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e273b-240">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="e273b-241">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="e273b-241">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="e273b-242">*Edit.cshtml.cs*檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-242">The *Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Edit.cshtml.cs?name=snippet)]

## <a name="validation"></a><span data-ttu-id="e273b-243">驗證</span><span class="sxs-lookup"><span data-stu-id="e273b-243">Validation</span></span>

<span data-ttu-id="e273b-244">驗證規則：</span><span class="sxs-lookup"><span data-stu-id="e273b-244">Validation rules:</span></span>

* <span data-ttu-id="e273b-245">在模型類別中是以宣告方式指定。</span><span class="sxs-lookup"><span data-stu-id="e273b-245">Are declaratively specified in the model class.</span></span>
* <span data-ttu-id="e273b-246">會在應用程式的任何位置強制執行。</span><span class="sxs-lookup"><span data-stu-id="e273b-246">Are enforced everywhere in the app.</span></span>

<span data-ttu-id="e273b-247"><xref:System.ComponentModel.DataAnnotations>命名空間提供一組內建的驗證屬性，會以宣告方式套用至類別或屬性。</span><span class="sxs-lookup"><span data-stu-id="e273b-247">The <xref:System.ComponentModel.DataAnnotations> namespace provides a set of built-in validation attributes that are applied declaratively to a class or property.</span></span> <span data-ttu-id="e273b-248">DataAnnotations 也包含格式化屬性，像 [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) 是格式化的說明，而且不提供任何驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-248">DataAnnotations also contains formatting attributes like [`[DataType]`](xref:System.ComponentModel.DataAnnotations.DataTypeAttribute) that help with formatting and don't provide any validation.</span></span>

<span data-ttu-id="e273b-249">請考慮 `Customer` 下列模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-249">Consider the `Customer` model:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Models/Customer.cs)]

<span data-ttu-id="e273b-250">使用下列*建立. cshtml*視圖檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-250">Using the following *Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=3,8-9,15-99)]

<span data-ttu-id="e273b-251">上述程式碼：</span><span class="sxs-lookup"><span data-stu-id="e273b-251">The preceding code:</span></span>

* <span data-ttu-id="e273b-252">包含 jQuery 和 jQuery 驗證腳本。</span><span class="sxs-lookup"><span data-stu-id="e273b-252">Includes jQuery and jQuery validation scripts.</span></span>
* <span data-ttu-id="e273b-253">會使用 `<div />` 和 `<span />` [標記](xref:mvc/views/tag-helpers/intro)協助程式來啟用：</span><span class="sxs-lookup"><span data-stu-id="e273b-253">Uses the `<div />` and `<span />` [Tag Helpers](xref:mvc/views/tag-helpers/intro) to enable:</span></span>

  * <span data-ttu-id="e273b-254">用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-254">Client-side validation.</span></span>
  * <span data-ttu-id="e273b-255">驗證錯誤呈現。</span><span class="sxs-lookup"><span data-stu-id="e273b-255">Validation error rendering.</span></span>

* <span data-ttu-id="e273b-256">產生下列 HTML：</span><span class="sxs-lookup"><span data-stu-id="e273b-256">Generates the following HTML:</span></span>

  [!code-html[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create5.html)]

<span data-ttu-id="e273b-257">張貼不含名稱值的建立表單時，會顯示錯誤訊息「需要名稱欄位」。</span><span class="sxs-lookup"><span data-stu-id="e273b-257">Posting the Create form without a name value displays the error message "The Name field is required."</span></span> <span data-ttu-id="e273b-258">在表單上。</span><span class="sxs-lookup"><span data-stu-id="e273b-258">on the form.</span></span> <span data-ttu-id="e273b-259">如果在用戶端上啟用 JavaScript，瀏覽器會顯示錯誤，而不會張貼至伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-259">If JavaScript is enabled on the client, the browser displays the error without posting to the server.</span></span>

<span data-ttu-id="e273b-260">`[StringLength(10)]`屬性會 `data-val-length-max="10"` 在呈現的 HTML 上產生。</span><span class="sxs-lookup"><span data-stu-id="e273b-260">The `[StringLength(10)]` attribute generates `data-val-length-max="10"` on the rendered HTML.</span></span> <span data-ttu-id="e273b-261">`data-val-length-max`防止瀏覽器輸入超過指定的最大長度。</span><span class="sxs-lookup"><span data-stu-id="e273b-261">`data-val-length-max` prevents browsers from entering more than the maximum length specified.</span></span> <span data-ttu-id="e273b-262">如果使用[Fiddler](https://www.telerik.com/fiddler)之類的工具來編輯和重新播放文章：</span><span class="sxs-lookup"><span data-stu-id="e273b-262">If a tool such as [Fiddler](https://www.telerik.com/fiddler) is used to edit and replay the post:</span></span>

* <span data-ttu-id="e273b-263">，其名稱超過10。</span><span class="sxs-lookup"><span data-stu-id="e273b-263">With the name longer than 10.</span></span>
* <span data-ttu-id="e273b-264">錯誤訊息「功能變數名稱必須是最大長度為10的字串」。</span><span class="sxs-lookup"><span data-stu-id="e273b-264">The error message "The field Name must be a string with a maximum length of 10."</span></span> <span data-ttu-id="e273b-265">」錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="e273b-265">is returned.</span></span>

<span data-ttu-id="e273b-266">請考慮下列 `Movie` 模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-266">Consider the following `Movie` model:</span></span>

[!code-csharp[](~/tutorials/razor-pages/razor-pages-start/sample/RazorPagesMovie30/Models/MovieDateRatingDA.cs?name=snippet1)]

<span data-ttu-id="e273b-267">驗證屬性會指定要在其套用的模型屬性上強制執行的行為：</span><span class="sxs-lookup"><span data-stu-id="e273b-267">The validation attributes specify behavior to enforce on the model properties they're applied to:</span></span>

* <span data-ttu-id="e273b-268">`Required`和 `MinimumLength` 屬性（attribute）表示屬性（property）必須有值，但不會防止使用者輸入空白字元來滿足這種驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-268">The `Required` and `MinimumLength` attributes indicate that a property must have a value, but nothing prevents a user from entering white space to satisfy this validation.</span></span>
* <span data-ttu-id="e273b-269">`RegularExpression` 屬性則用來限制可輸入的字元。</span><span class="sxs-lookup"><span data-stu-id="e273b-269">The `RegularExpression` attribute is used to limit what characters can be input.</span></span> <span data-ttu-id="e273b-270">在上述程式碼中，"Genre"：</span><span class="sxs-lookup"><span data-stu-id="e273b-270">In the preceding code, "Genre":</span></span>

  * <span data-ttu-id="e273b-271">必須指使用字母。</span><span class="sxs-lookup"><span data-stu-id="e273b-271">Must only use letters.</span></span>
  * <span data-ttu-id="e273b-272">第一個字母必須是大寫。</span><span class="sxs-lookup"><span data-stu-id="e273b-272">The first letter is required to be uppercase.</span></span> <span data-ttu-id="e273b-273">不允許使用空格、數字和特殊字元。</span><span class="sxs-lookup"><span data-stu-id="e273b-273">White space, numbers, and special characters are not allowed.</span></span>

* <span data-ttu-id="e273b-274">`RegularExpression` "Rating"：</span><span class="sxs-lookup"><span data-stu-id="e273b-274">The `RegularExpression` "Rating":</span></span>

  * <span data-ttu-id="e273b-275">第一個字元必須為大寫字母。</span><span class="sxs-lookup"><span data-stu-id="e273b-275">Requires that the first character be an uppercase letter.</span></span>
  * <span data-ttu-id="e273b-276">允許後續空格中的特殊字元和數位。</span><span class="sxs-lookup"><span data-stu-id="e273b-276">Allows special characters and numbers in subsequent spaces.</span></span> <span data-ttu-id="e273b-277">"PG-13" 對分級而言有效，但不適用於 "Genre"。</span><span class="sxs-lookup"><span data-stu-id="e273b-277">"PG-13" is valid for a rating, but fails for a "Genre".</span></span>

* <span data-ttu-id="e273b-278">`Range` 屬性會將值限制在指定的範圍內。</span><span class="sxs-lookup"><span data-stu-id="e273b-278">The `Range` attribute constrains a value to within a specified range.</span></span>
* <span data-ttu-id="e273b-279">`StringLength`屬性會設定字串屬性的最大長度，並選擇性地設定其最小長度。</span><span class="sxs-lookup"><span data-stu-id="e273b-279">The `StringLength` attribute sets the maximum length of a string property, and optionally its minimum length.</span></span>
* <span data-ttu-id="e273b-280">實值型別 (如`decimal`、`int`、`float`、`DateTime`) 原本就是必要項目，而且不需要 `[Required]` 屬性。</span><span class="sxs-lookup"><span data-stu-id="e273b-280">Value types (such as `decimal`, `int`, `float`, `DateTime`) are inherently required and don't need the `[Required]` attribute.</span></span>

<span data-ttu-id="e273b-281">模型的 [建立] 頁面會 `Movie` 顯示具有無效值的錯誤：</span><span class="sxs-lookup"><span data-stu-id="e273b-281">The Create page for the `Movie` model shows displays errors with invalid values:</span></span>

![有多個 jQuery 用戶端驗證錯誤的電影檢視表單](~/tutorials/razor-pages/validation/_static/val.png)

<span data-ttu-id="e273b-283">如需詳細資訊，請參閱：</span><span class="sxs-lookup"><span data-stu-id="e273b-283">For more information, see:</span></span>

* [<span data-ttu-id="e273b-284">將驗證新增至電影應用程式</span><span class="sxs-lookup"><span data-stu-id="e273b-284">Add validation to the Movie app</span></span>](xref:tutorials/razor-pages/validation)
* <span data-ttu-id="e273b-285">[ASP.NET Core 中的模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="e273b-285">[Model validation in ASP.NET Core](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="e273b-286">使用 OnGet 處理常式後援來處理 HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="e273b-286">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="e273b-287">`HEAD`要求可讓您抓取特定資源的標頭。</span><span class="sxs-lookup"><span data-stu-id="e273b-287">`HEAD` requests allow retrieving the headers for a specific resource.</span></span> <span data-ttu-id="e273b-288">不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。</span><span class="sxs-lookup"><span data-stu-id="e273b-288">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="e273b-289">一般來說，會為 `HEAD` 要求建立及呼叫 `OnHead` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="e273b-289">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Privacy.cshtml.cs?name=snippet)]

Razor<span data-ttu-id="e273b-290">`OnGet`如果未 `OnHead` 定義任何處理程式，則頁面會回復為呼叫處理常式。</span><span class="sxs-lookup"><span data-stu-id="e273b-290"> Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span>

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="e273b-291">XSRF/CSRF 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e273b-291">XSRF/CSRF and Razor Pages</span></span>

Razor<span data-ttu-id="e273b-292">頁面會受到[Antiforgery 驗證](xref:security/anti-request-forgery)的保護。</span><span class="sxs-lookup"><span data-stu-id="e273b-292"> Pages are protected by [Antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="e273b-293">[FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper)會將 antiforgery TOKEN 插入 HTML 表單元素中。</span><span class="sxs-lookup"><span data-stu-id="e273b-293">The [FormTagHelper](xref:mvc/views/working-with-forms#the-form-tag-helper) injects antiforgery tokens into HTML form elements.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="e273b-294">搭配頁面使用版面配置、部分、範本和標籤協助程式 Razor</span><span class="sxs-lookup"><span data-stu-id="e273b-294">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="e273b-295">頁面會與視圖引擎的所有功能搭配使用 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-295">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="e273b-296">版面配置、部分、範本、標記協助程式、 *_ViewStart*和 *_ViewImports。 cshtml*的工作方式與傳統 Razor 視圖相同。</span><span class="sxs-lookup"><span data-stu-id="e273b-296">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, and *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="e273b-297">可利用這些功能的一部分來整理這個頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-297">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="e273b-298">將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="e273b-298">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Shared/_Layout2.cshtml?hightlight=12)]

<span data-ttu-id="e273b-299">[版面](xref:mvc/views/layout)配置：</span><span class="sxs-lookup"><span data-stu-id="e273b-299">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="e273b-300">控制每個頁面的版面配置 (除非頁面退出版面配置)。</span><span class="sxs-lookup"><span data-stu-id="e273b-300">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="e273b-301">匯入 HTML 結構，例如 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="e273b-301">Imports HTML structures such as JavaScript and stylesheets.</span></span>
* <span data-ttu-id="e273b-302">頁面的內容 Razor 會在呼叫的位置呈現 `@RenderBody()` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-302">The contents of the Razor page are rendered where `@RenderBody()` is called.</span></span>

<span data-ttu-id="e273b-303">如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="e273b-303">For more information, see [layout page](xref:mvc/views/layout).</span></span>

<span data-ttu-id="e273b-304">[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：</span><span class="sxs-lookup"><span data-stu-id="e273b-304">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="e273b-305">版面配置位於 *Pages/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-305">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="e273b-306">頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。</span><span class="sxs-lookup"><span data-stu-id="e273b-306">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="e273b-307">*Pages/Shared*資料夾中的版面配置可以從 Razor [ *pages* ] 資料夾下的任何頁面使用。</span><span class="sxs-lookup"><span data-stu-id="e273b-307">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="e273b-308">版面配置頁面應位於 *Pages/Shared* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e273b-308">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="e273b-309">我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-309">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="e273b-310">*Views/Shared* 是 MVC 檢視模式。</span><span class="sxs-lookup"><span data-stu-id="e273b-310">*Views/Shared* is an MVC views pattern.</span></span> Razor<span data-ttu-id="e273b-311">頁面的目的是要依賴資料夾階層，而不是路徑慣例。</span><span class="sxs-lookup"><span data-stu-id="e273b-311"> Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="e273b-312">從頁面中查看搜尋 Razor 包含*Pages*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-312">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="e273b-313">與 MVC 控制器和傳統視圖搭配使用的版面配置、範本和部分都 Razor *是可行*的。</span><span class="sxs-lookup"><span data-stu-id="e273b-313">The layouts, templates, and partials used with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="e273b-314">新增 *Pages/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-314">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="e273b-315">本教學課程稍後會說明 `@namespace`。</span><span class="sxs-lookup"><span data-stu-id="e273b-315">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="e273b-316">`@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-316">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="e273b-317">在 `@namespace` 頁面上設定的指示詞：</span><span class="sxs-lookup"><span data-stu-id="e273b-317">The `@namespace` directive set on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="e273b-318">指示詞會 `@namespace` 設定頁面的命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-318">The `@namespace` directive sets the namespace for the page.</span></span> <span data-ttu-id="e273b-319">`@model` 指示詞不需要包含命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-319">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="e273b-320">當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-320">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="e273b-321">所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e273b-321">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="e273b-322">例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：</span><span class="sxs-lookup"><span data-stu-id="e273b-322">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="e273b-323">*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="e273b-323">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="e273b-324">針對 [ *Pages/Customers/編輯. cshtml* ] 頁面產生的命名空間與 Razor 類別相同 `PageModel` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-324">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="e273b-325">`@namespace`*也適用于傳統的 Razor 視圖。*</span><span class="sxs-lookup"><span data-stu-id="e273b-325">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="e273b-326">請考慮*Pages/Create. cshtml* view file：</span><span class="sxs-lookup"><span data-stu-id="e273b-326">Consider the *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create3.cshtml?highlight=2-3)]

<span data-ttu-id="e273b-327">已更新的*Pages/Create. cshtml*視圖檔案，包含 *_ViewImports. cshtml*和先前的配置檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-327">The updated *Pages/Create.cshtml* view file with *_ViewImports.cshtml* and the preceding layout file:</span></span>

[!code-cshtml[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create4.cshtml?highlight=2)]

<span data-ttu-id="e273b-328">在上述程式碼中， *_ViewImports. cshtml*已匯入命名空間和標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="e273b-328">In the preceding code, the *_ViewImports.cshtml* imported the namespace and Tag Helpers.</span></span> <span data-ttu-id="e273b-329">設定檔案已匯入 JavaScript 檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-329">The layout file imported the JavaScript files.</span></span>

<span data-ttu-id="e273b-330">[ Razor Pages 入門專案](#rpvs17)包含*pages/_ValidationScriptsPartial. cshtml*，這會連結用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-330">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="e273b-331">如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。</span><span class="sxs-lookup"><span data-stu-id="e273b-331">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="e273b-332">產生頁面 URL</span><span class="sxs-lookup"><span data-stu-id="e273b-332">URL generation for Pages</span></span>

<span data-ttu-id="e273b-333">前面出現過的 `Create` 頁面使用 `RedirectToPage`：</span><span class="sxs-lookup"><span data-stu-id="e273b-333">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/Pages/Customers/Create.cshtml.cs?name=snippet_PageModel&highlight=28)]

<span data-ttu-id="e273b-334">應用程式有下列檔案/資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="e273b-334">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="e273b-335">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="e273b-335">*/Pages*</span></span>

  * <span data-ttu-id="e273b-336">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-336">*Index.cshtml*</span></span>
  * <span data-ttu-id="e273b-337">*隱私權. cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-337">*Privacy.cshtml*</span></span>
  * <span data-ttu-id="e273b-338">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="e273b-338">*/Customers*</span></span>

    * <span data-ttu-id="e273b-339">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-339">*Create.cshtml*</span></span>
    * <span data-ttu-id="e273b-340">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-340">*Edit.cshtml*</span></span>
    * <span data-ttu-id="e273b-341">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-341">*Index.cshtml*</span></span>

<span data-ttu-id="e273b-342">*Pages/customers/Create. cshtml*和*pages/customers/Edit. cshtml*頁面會在成功之後重新導向至*Pages/customers/Index. cshtml* 。</span><span class="sxs-lookup"><span data-stu-id="e273b-342">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Customers/Index.cshtml* after success.</span></span> <span data-ttu-id="e273b-343">此字串 `./Index` 是用來存取前一頁的相對頁面名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-343">The string `./Index` is a relative page name used to access the preceding page.</span></span> <span data-ttu-id="e273b-344">它可用來產生*Pages/Customers/Index. cshtml*頁面的 url。</span><span class="sxs-lookup"><span data-stu-id="e273b-344">It is used to generate URLs to the *Pages/Customers/Index.cshtml* page.</span></span> <span data-ttu-id="e273b-345">例如：</span><span class="sxs-lookup"><span data-stu-id="e273b-345">For example:</span></span>

* `Url.Page("./Index", ...)`
* `<a asp-page="./Index">Customers Index Page</a>`
* `RedirectToPage("./Index")`

<span data-ttu-id="e273b-346">絕對頁面名稱 `/Index` 是用來產生*Pages/Index. cshtml*頁面的 url。</span><span class="sxs-lookup"><span data-stu-id="e273b-346">The absolute page name `/Index` is used to generate URLs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="e273b-347">例如：</span><span class="sxs-lookup"><span data-stu-id="e273b-347">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">Home Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="e273b-348">頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-348">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="e273b-349">先前的 URL 產生範例提供了增強的選項和功能，透過硬式編碼 URL。</span><span class="sxs-lookup"><span data-stu-id="e273b-349">The preceding URL generation samples offer enhanced options and functional capabilities over hard-coding a URL.</span></span> <span data-ttu-id="e273b-350">URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。</span><span class="sxs-lookup"><span data-stu-id="e273b-350">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="e273b-351">產生頁面 URL 支援相關的名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-351">URL generation for pages supports relative names.</span></span> <span data-ttu-id="e273b-352">下表顯示 `RedirectToPage` 在*Pages/Customers/Create. cshtml*中使用不同參數選取的索引頁。</span><span class="sxs-lookup"><span data-stu-id="e273b-352">The following table shows which Index page is selected using different `RedirectToPage` parameters in *Pages/Customers/Create.cshtml*.</span></span>

| <span data-ttu-id="e273b-353">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="e273b-353">RedirectToPage(x)</span></span>| <span data-ttu-id="e273b-354">頁面</span><span class="sxs-lookup"><span data-stu-id="e273b-354">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e273b-355">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-355">RedirectToPage("/Index")</span></span> | <span data-ttu-id="e273b-356">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-356">*Pages/Index*</span></span> |
| <span data-ttu-id="e273b-357">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="e273b-357">RedirectToPage("./Index");</span></span> | <span data-ttu-id="e273b-358">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-358">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="e273b-359">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-359">RedirectToPage("../Index")</span></span> | <span data-ttu-id="e273b-360">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-360">*Pages/Index*</span></span> |
| <span data-ttu-id="e273b-361">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-361">RedirectToPage("Index")</span></span>  | <span data-ttu-id="e273b-362">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-362">*Pages/Customers/Index*</span></span> |

<!-- Test via ~/razor-pages/index/3.0sample/RazorPagesContacts/Pages/Customers/Details.cshtml.cs -->

<span data-ttu-id="e273b-363">`RedirectToPage("Index")`、 `RedirectToPage("./Index")` 和 `RedirectToPage("../Index")` 是*相對名稱*。</span><span class="sxs-lookup"><span data-stu-id="e273b-363">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")` are *relative names*.</span></span> <span data-ttu-id="e273b-364">`RedirectToPage` 參數「結合」\*\* 了目前頁面的路徑，以計算目的地頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-364">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>

<span data-ttu-id="e273b-365">相對名稱連結在以複雜結構建置網站時很有用。</span><span class="sxs-lookup"><span data-stu-id="e273b-365">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="e273b-366">當使用相對名稱來連結資料夾中的頁面時：</span><span class="sxs-lookup"><span data-stu-id="e273b-366">When relative names are used to link between pages in a folder:</span></span>

* <span data-ttu-id="e273b-367">重新命名資料夾並不會中斷相對連結。</span><span class="sxs-lookup"><span data-stu-id="e273b-367">Renaming a folder doesn't break the relative links.</span></span>
* <span data-ttu-id="e273b-368">連結不會中斷，因為它們不包含資料夾名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-368">Links are not broken because they don't include the folder name.</span></span>

<span data-ttu-id="e273b-369">若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：</span><span class="sxs-lookup"><span data-stu-id="e273b-369">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="e273b-370">如需詳細資訊，請參閱 <xref:mvc/controllers/areas> 和 <xref:razor-pages/razor-pages-conventions>。</span><span class="sxs-lookup"><span data-stu-id="e273b-370">For more information, see <xref:mvc/controllers/areas> and <xref:razor-pages/razor-pages-conventions>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="e273b-371">ViewData 屬性</span><span class="sxs-lookup"><span data-stu-id="e273b-371">ViewData attribute</span></span>

<span data-ttu-id="e273b-372">您可以使用將資料傳遞至頁面 <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute> 。</span><span class="sxs-lookup"><span data-stu-id="e273b-372">Data can be passed to a page with <xref:Microsoft.AspNetCore.Mvc.ViewDataAttribute>.</span></span> <span data-ttu-id="e273b-373">具有屬性的屬性 `[ViewData]` 會將其值儲存並從載入 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary> 。</span><span class="sxs-lookup"><span data-stu-id="e273b-373">Properties with the `[ViewData]` attribute have their values stored and loaded from the <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.ViewDataDictionary>.</span></span>

<span data-ttu-id="e273b-374">在下列範例中，會將 `AboutModel` `[ViewData]` 屬性套用至 `Title` 屬性：</span><span class="sxs-lookup"><span data-stu-id="e273b-374">In the following example, the `AboutModel` applies the `[ViewData]` attribute to the `Title` property:</span></span>

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

<span data-ttu-id="e273b-375">在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：</span><span class="sxs-lookup"><span data-stu-id="e273b-375">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="e273b-376">在此配置中，標題會從 ViewData 字典中讀取：</span><span class="sxs-lookup"><span data-stu-id="e273b-376">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="e273b-377">TempData</span><span class="sxs-lookup"><span data-stu-id="e273b-377">TempData</span></span>

<span data-ttu-id="e273b-378">ASP.NET Core 會公開 <xref:Microsoft.AspNetCore.Mvc.Controller.TempData> 。</span><span class="sxs-lookup"><span data-stu-id="e273b-378">ASP.NET Core exposes the <xref:Microsoft.AspNetCore.Mvc.Controller.TempData>.</span></span> <span data-ttu-id="e273b-379">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="e273b-379">This property stores data until it's read.</span></span> <span data-ttu-id="e273b-380"><xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> 和 <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="e273b-380">The <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Keep*> and <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.TempDataDictionary.Peek*> methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="e273b-381">`TempData`當需要多個單一要求的資料時，對重新導向很有用。</span><span class="sxs-lookup"><span data-stu-id="e273b-381">`TempData` is useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="e273b-382">下列程式碼會設定使用 `TempData` 的 `Message` 值：</span><span class="sxs-lookup"><span data-stu-id="e273b-382">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="e273b-383">*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `TempData` 的 `Message` 值。</span><span class="sxs-lookup"><span data-stu-id="e273b-383">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="e273b-384">*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="e273b-384">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="e273b-385">如需詳細資訊，請參閱[TempData](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="e273b-385">For more information, see [TempData](xref:fundamentals/app-state#tempdata).</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="e273b-386">每頁面有多個處理常式</span><span class="sxs-lookup"><span data-stu-id="e273b-386">Multiple handlers per page</span></span>

<span data-ttu-id="e273b-387">下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：</span><span class="sxs-lookup"><span data-stu-id="e273b-387">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<span data-ttu-id="e273b-388">上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="e273b-388">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="e273b-389">`asp-page-handler` 屬性附隨於 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="e273b-389">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="e273b-390">`asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-390">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="e273b-391">因為範例連結至目前的頁面，所以未指定 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="e273b-391">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="e273b-392">頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-392">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="e273b-393">上述程式碼使用「具名的處理常式方法」\*\*。</span><span class="sxs-lookup"><span data-stu-id="e273b-393">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="e273b-394">具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。</span><span class="sxs-lookup"><span data-stu-id="e273b-394">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="e273b-395">在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。</span><span class="sxs-lookup"><span data-stu-id="e273b-395">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="e273b-396">移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-396">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="e273b-397">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。</span><span class="sxs-lookup"><span data-stu-id="e273b-397">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="e273b-398">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-398">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="e273b-399">自訂路由</span><span class="sxs-lookup"><span data-stu-id="e273b-399">Custom routes</span></span>

<span data-ttu-id="e273b-400">使用 `@page` 指示詞，可以：</span><span class="sxs-lookup"><span data-stu-id="e273b-400">Use the `@page` directive to:</span></span>

* <span data-ttu-id="e273b-401">指定頁面的自訂路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-401">Specify a custom route to a page.</span></span> <span data-ttu-id="e273b-402">例如，[關於] 頁面的路由可使用 `@page "/Some/Other/Path"` 設為 `/Some/Other/Path`。</span><span class="sxs-lookup"><span data-stu-id="e273b-402">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="e273b-403">將區段附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-403">Append segments to a page's default route.</span></span> <span data-ttu-id="e273b-404">例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-404">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="e273b-405">將參數附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-405">Append parameters to a page's default route.</span></span> <span data-ttu-id="e273b-406">例如，具有 `@page "{id}"` 的頁面可要求識別碼參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="e273b-406">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="e273b-407">支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e273b-407">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="e273b-408">例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。</span><span class="sxs-lookup"><span data-stu-id="e273b-408">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="e273b-409">如果您不喜歡 URL 中的查詢字串 `?handler=JoinList` ，請變更路由，將處理常式名稱放在 url 的路徑部分。</span><span class="sxs-lookup"><span data-stu-id="e273b-409">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="e273b-410">藉由在指示詞後面加上雙引號括住的路由範本，可以自訂路由 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-410">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="e273b-411">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="e273b-411">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="e273b-412">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-412">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="e273b-413">跟在 `handler` 後面的 `?` 表示路由參數為選擇性。</span><span class="sxs-lookup"><span data-stu-id="e273b-413">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="advanced-configuration-and-settings"></a><span data-ttu-id="e273b-414">先進的設定和設定</span><span class="sxs-lookup"><span data-stu-id="e273b-414">Advanced configuration and settings</span></span>

<span data-ttu-id="e273b-415">大部分的應用程式都不需要下列章節中的設定和設定。</span><span class="sxs-lookup"><span data-stu-id="e273b-415">The configuration and settings in following sections is not required by most apps.</span></span>

<span data-ttu-id="e273b-416">若要設定 advanced 選項，請使用擴充方法 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*> ：</span><span class="sxs-lookup"><span data-stu-id="e273b-416">To configure advanced options, use the extension method <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.AddRazorPagesOptions*>:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupRPoptions.cs?name=snippet)]

<span data-ttu-id="e273b-417">使用 <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> 來設定頁面的根目錄，或加入頁面的應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="e273b-417">Use the <xref:Microsoft.AspNetCore.Mvc.RazorPages.RazorPagesOptions> to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="e273b-418">如需慣例的詳細資訊，請參閱[ Razor 頁面授權慣例](xref:security/authorization/razor-pages-authorization)。</span><span class="sxs-lookup"><span data-stu-id="e273b-418">For more information on conventions, see [Razor Pages authorization conventions](xref:security/authorization/razor-pages-authorization).</span></span>

<span data-ttu-id="e273b-419">若要先行編譯視圖，請參閱[ Razor view 編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="e273b-419">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation).</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="e273b-420">指定 Razor 頁面位於內容根目錄</span><span class="sxs-lookup"><span data-stu-id="e273b-420">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="e273b-421">根據預設， Razor 頁面會以根方式在 */Pages*目錄中。</span><span class="sxs-lookup"><span data-stu-id="e273b-421">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="e273b-422">新增 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> 以指定您的 Razor 頁面位於應用程式的[內容根目錄](xref:fundamentals/index#content-root)（ <xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath> ）：</span><span class="sxs-lookup"><span data-stu-id="e273b-422">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcBuilderExtensions.WithRazorPagesAtContentRoot*> to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) (<xref:Microsoft.AspNetCore.Hosting.IHostingEnvironment.ContentRootPath>) of the app:</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesAtContentRoot.cs?name=snippet)]

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="e273b-423">指定 Razor 頁面位於自訂根目錄</span><span class="sxs-lookup"><span data-stu-id="e273b-423">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="e273b-424">新增 <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> 以指定 Razor 頁面位於應用程式中的自訂根目錄（提供相對路徑）：</span><span class="sxs-lookup"><span data-stu-id="e273b-424">Add <xref:Microsoft.Extensions.DependencyInjection.MvcRazorPagesMvcCoreBuilderExtensions.WithRazorPagesRoot*> to specify that Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

[!code-cs[](index/3.0sample/RazorPagesContacts/StartupWithRazorPagesRoot.cs?name=snippet)]

## <a name="additional-resources"></a><span data-ttu-id="e273b-425">其他資源</span><span class="sxs-lookup"><span data-stu-id="e273b-425">Additional resources</span></span>

* <span data-ttu-id="e273b-426">請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，這會在此簡介中建立。</span><span class="sxs-lookup"><span data-stu-id="e273b-426">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>
* <span data-ttu-id="e273b-427">[授權屬性和 Razor 頁面](xref:security/authorization/simple#aarp)</span><span class="sxs-lookup"><span data-stu-id="e273b-427">[Authorize attribute and Razor Pages](xref:security/authorization/simple#aarp)</span></span>
* [<span data-ttu-id="e273b-428">下載或查看範例程式碼</span><span class="sxs-lookup"><span data-stu-id="e273b-428">Download or view sample code</span></span>](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/3.0sample)
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>
* <xref:blazor/integrate-components>

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="e273b-429">作者：[Rick Anderson](https://twitter.com/RickAndMSFT) 與 [Ryan Nowak](https://github.com/rynowak)</span><span class="sxs-lookup"><span data-stu-id="e273b-429">By [Rick Anderson](https://twitter.com/RickAndMSFT) and [Ryan Nowak](https://github.com/rynowak)</span></span>

Razor<span data-ttu-id="e273b-430">頁面是 ASP.NET Core MVC 的新層面，可讓撰寫以頁面為焦點的案例更輕鬆且更具生產力。</span><span class="sxs-lookup"><span data-stu-id="e273b-430"> Pages is a new aspect of ASP.NET Core MVC that makes coding page-focused scenarios easier and more productive.</span></span>

<span data-ttu-id="e273b-431">如果您在尋找使用模型檢視控制器方法的教學課程，請參閱[開始使用 ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc)。</span><span class="sxs-lookup"><span data-stu-id="e273b-431">If you're looking for a tutorial that uses the Model-View-Controller approach, see [Get started with ASP.NET Core MVC](xref:tutorials/first-mvc-app/start-mvc).</span></span>

<span data-ttu-id="e273b-432">本檔提供 Razor 頁面簡介。</span><span class="sxs-lookup"><span data-stu-id="e273b-432">This document provides an introduction to Razor Pages.</span></span> <span data-ttu-id="e273b-433">它不是逐步教學課程。</span><span class="sxs-lookup"><span data-stu-id="e273b-433">It's not a step by step tutorial.</span></span> <span data-ttu-id="e273b-434">如果您發現某些區段太先進，請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)。</span><span class="sxs-lookup"><span data-stu-id="e273b-434">If you find some of the sections too advanced, see [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start).</span></span> <span data-ttu-id="e273b-435">如需 ASP.NET Core 的概觀，請參閱[ASP.NET Core 簡介](xref:index)。</span><span class="sxs-lookup"><span data-stu-id="e273b-435">For an overview of ASP.NET Core, see the [Introduction to ASP.NET Core](xref:index).</span></span>

## <a name="prerequisites"></a><span data-ttu-id="e273b-436">必要條件</span><span class="sxs-lookup"><span data-stu-id="e273b-436">Prerequisites</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e273b-437">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e273b-437">Visual Studio</span></span>](#tab/visual-studio)

[!INCLUDE[](~/includes/net-core-prereqs-vs2019-2.2.md)]

# <a name="visual-studio-code"></a>[<span data-ttu-id="e273b-438">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e273b-438">Visual Studio Code</span></span>](#tab/visual-studio-code)

[!INCLUDE[](~/includes/net-core-prereqs-vsc-2.2.md)]

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e273b-439">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e273b-439">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

[!INCLUDE[](~/includes/net-core-prereqs-mac-2.2.md)]

---

<a name="rpvs17"></a>

## <a name="create-a-razor-pages-project"></a><span data-ttu-id="e273b-440">建立 Razor 頁面專案</span><span class="sxs-lookup"><span data-stu-id="e273b-440">Create a Razor Pages project</span></span>

# <a name="visual-studio"></a>[<span data-ttu-id="e273b-441">Visual Studio</span><span class="sxs-lookup"><span data-stu-id="e273b-441">Visual Studio</span></span>](#tab/visual-studio)

<span data-ttu-id="e273b-442">如需如何建立頁面專案的詳細指示，請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start) Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-442">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start) for detailed instructions on how to create a Razor Pages project.</span></span>

# <a name="visual-studio-for-mac"></a>[<span data-ttu-id="e273b-443">Visual Studio for Mac</span><span class="sxs-lookup"><span data-stu-id="e273b-443">Visual Studio for Mac</span></span>](#tab/visual-studio-mac)

<span data-ttu-id="e273b-444">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="e273b-444">Run `dotnet new webapp` from the command line.</span></span>

<span data-ttu-id="e273b-445">從 Visual Studio for Mac 開啟已產生的 *.csproj* 檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-445">Open the generated *.csproj* file from Visual Studio for Mac.</span></span>

# <a name="visual-studio-code"></a>[<span data-ttu-id="e273b-446">Visual Studio Code</span><span class="sxs-lookup"><span data-stu-id="e273b-446">Visual Studio Code</span></span>](#tab/visual-studio-code)

<span data-ttu-id="e273b-447">從命令列執行 `dotnet new webapp`。</span><span class="sxs-lookup"><span data-stu-id="e273b-447">Run `dotnet new webapp` from the command line.</span></span>

---

## <a name="razor-pages"></a>Razor<span data-ttu-id="e273b-448">頁面</span><span class="sxs-lookup"><span data-stu-id="e273b-448"> Pages</span></span>

Razor<span data-ttu-id="e273b-449">頁面已在*Startup.cs*中啟用：</span><span class="sxs-lookup"><span data-stu-id="e273b-449"> Pages is enabled in *Startup.cs*:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Startup.cs?name=snippet_Startup)]

<span data-ttu-id="e273b-450">請考慮使用基本頁面：<a name="OnGet"></a></span><span class="sxs-lookup"><span data-stu-id="e273b-450">Consider a basic page: <a name="OnGet"></a></span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index.cshtml)]

<span data-ttu-id="e273b-451">上述程式碼看起來很像是在具有控制器和 views 的 ASP.NET Core 應用程式中使用的[ Razor 視圖](xref:tutorials/first-mvc-app/adding-view)檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-451">The preceding code looks a lot like a [Razor view file](xref:tutorials/first-mvc-app/adding-view) used in an ASP.NET Core app with controllers and views.</span></span> <span data-ttu-id="e273b-452">讓它不同的是 `@page` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-452">What makes it different is the `@page` directive.</span></span> <span data-ttu-id="e273b-453">`@page` 會將檔案轉換成 MVC 動作，這表示它會直接處理要求，不用透過控制器。</span><span class="sxs-lookup"><span data-stu-id="e273b-453">`@page` makes the file into an MVC action - which means that it handles requests directly, without going through a controller.</span></span> <span data-ttu-id="e273b-454">`@page`必須是頁面上的第一個指示詞 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-454">`@page` must be the first Razor directive on a page.</span></span> <span data-ttu-id="e273b-455">`@page`會影響其他結構的行為 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-455">`@page` affects the behavior of other Razor constructs.</span></span>

<span data-ttu-id="e273b-456">使用`PageModel`類別的類似頁面，顯示於下列兩個檔案中。</span><span class="sxs-lookup"><span data-stu-id="e273b-456">A similar page, using a `PageModel` class, is shown in the following two files.</span></span> <span data-ttu-id="e273b-457">*Pages/Index2.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-457">The *Pages/Index2.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Index2.cshtml)]

<span data-ttu-id="e273b-458">*Pages/Index2.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-458">The *Pages/Index2.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesIntro/Pages/Index2.cshtml.cs)]

<span data-ttu-id="e273b-459">依照慣例， `PageModel` 類別檔案的名稱與分頁檔相同， Razor 附加 *.cs* 。</span><span class="sxs-lookup"><span data-stu-id="e273b-459">By convention, the `PageModel` class file has the same name as the Razor Page file with *.cs* appended.</span></span> <span data-ttu-id="e273b-460">例如，前一 Razor 頁是*Pages/Index2*。</span><span class="sxs-lookup"><span data-stu-id="e273b-460">For example, the previous Razor Page is *Pages/Index2.cshtml*.</span></span> <span data-ttu-id="e273b-461">包含 `PageModel` 類別的檔案名為 *Pages/Index2.cshtml.cs*。</span><span class="sxs-lookup"><span data-stu-id="e273b-461">The file containing the `PageModel` class is named *Pages/Index2.cshtml.cs*.</span></span>

<span data-ttu-id="e273b-462">頁面的 URL 路徑關聯是由頁面在檔案系統中的位置決定。</span><span class="sxs-lookup"><span data-stu-id="e273b-462">The associations of URL paths to pages are determined by the page's location in the file system.</span></span> <span data-ttu-id="e273b-463">下表顯示 Razor 頁面路徑和相符的 URL：</span><span class="sxs-lookup"><span data-stu-id="e273b-463">The following table shows a Razor Page path and the matching URL:</span></span>

| <span data-ttu-id="e273b-464">檔案名稱和路徑</span><span class="sxs-lookup"><span data-stu-id="e273b-464">File name and path</span></span>               | <span data-ttu-id="e273b-465">比對 URL</span><span class="sxs-lookup"><span data-stu-id="e273b-465">matching URL</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e273b-466">*/Pages/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-466">*/Pages/Index.cshtml*</span></span> | <span data-ttu-id="e273b-467">`/` 或 `/Index`</span><span class="sxs-lookup"><span data-stu-id="e273b-467">`/` or `/Index`</span></span> |
| <span data-ttu-id="e273b-468">*/Pages/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-468">*/Pages/Contact.cshtml*</span></span> | `/Contact` |
| <span data-ttu-id="e273b-469">*/Pages/Store/Contact.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-469">*/Pages/Store/Contact.cshtml*</span></span> | `/Store/Contact` |
| <span data-ttu-id="e273b-470">*/Pages/Store/Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-470">*/Pages/Store/Index.cshtml*</span></span> | <span data-ttu-id="e273b-471">`/Store` 或 `/Store/Index`</span><span class="sxs-lookup"><span data-stu-id="e273b-471">`/Store` or `/Store/Index`</span></span> |

<span data-ttu-id="e273b-472">注意：</span><span class="sxs-lookup"><span data-stu-id="e273b-472">Notes:</span></span>

* <span data-ttu-id="e273b-473">執行時間預設會 Razor 在*pages*資料夾中尋找分頁檔案。</span><span class="sxs-lookup"><span data-stu-id="e273b-473">The runtime looks for Razor Pages files in the *Pages* folder by default.</span></span>
* <span data-ttu-id="e273b-474">`Index` 是 URL 未包含頁面時的預設頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-474">`Index` is the default page when a URL doesn't include a page.</span></span>

## <a name="write-a-basic-form"></a><span data-ttu-id="e273b-475">撰寫基本表單</span><span class="sxs-lookup"><span data-stu-id="e273b-475">Write a basic form</span></span>

Razor<span data-ttu-id="e273b-476">頁面的設計目的是要在建立應用程式時，讓與網頁瀏覽器搭配使用的常見模式變得容易執行。</span><span class="sxs-lookup"><span data-stu-id="e273b-476"> Pages is designed to make common patterns used with web browsers easy to implement when building an app.</span></span> <span data-ttu-id="e273b-477">[模型](xref:mvc/models/model-binding)系結、[標記](xref:mvc/views/tag-helpers/intro)協助程式和 HTML 協助程式都*只*會使用頁面類別中定義的屬性 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-477">[Model binding](xref:mvc/models/model-binding), [Tag Helpers](xref:mvc/views/tag-helpers/intro), and HTML helpers all *just work* with the properties defined in a Razor Page class.</span></span> <span data-ttu-id="e273b-478">`Contact` 模型請考慮實作基本的「與我們連絡」格式頁面：</span><span class="sxs-lookup"><span data-stu-id="e273b-478">Consider a page that implements a basic "contact us" form for the `Contact` model:</span></span>

<span data-ttu-id="e273b-479">本文件中的範例，會在 [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) 檔案中初始化 `DbContext`。</span><span class="sxs-lookup"><span data-stu-id="e273b-479">For the samples in this document, the `DbContext` is initialized in the [Startup.cs](https://github.com/dotnet/AspNetCore.Docs/blob/master/aspnetcore/razor-pages/index/sample/RazorPagesContacts/Startup.cs#L15-L16) file.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Startup.cs?highlight=15-16)]

<span data-ttu-id="e273b-480">資料模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-480">The data model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/Customer.cs)]

<span data-ttu-id="e273b-481">DB 內容：</span><span class="sxs-lookup"><span data-stu-id="e273b-481">The db context:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Data/AppDbContext.cs)]

<span data-ttu-id="e273b-482">*Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-482">The *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml)]

<span data-ttu-id="e273b-483">*Pages/Create.cshtml.cs* 頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-483">The *Pages/Create.cshtml.cs* page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_ALL)]

<span data-ttu-id="e273b-484">依照慣例，`PageModel` 類別稱之為 `<PageName>Model`，與頁面位於相同的命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-484">By convention, the `PageModel` class is called `<PageName>Model` and is in the same namespace as the page.</span></span>

<span data-ttu-id="e273b-485">`PageModel` 類別可以分離頁面邏輯與頁面展示。</span><span class="sxs-lookup"><span data-stu-id="e273b-485">The `PageModel` class allows separation of the logic of a page from its presentation.</span></span> <span data-ttu-id="e273b-486">此類別會定義頁面的處理常式，以處理傳送至頁面的要求與用於轉譯頁面的資料。</span><span class="sxs-lookup"><span data-stu-id="e273b-486">It defines page handlers for requests sent to the page and the data used to render the page.</span></span> <span data-ttu-id="e273b-487">這種分隔允許：</span><span class="sxs-lookup"><span data-stu-id="e273b-487">This separation allows:</span></span>

* <span data-ttu-id="e273b-488">透過相依性[插入](xref:fundamentals/dependency-injection)來管理頁面相依性。</span><span class="sxs-lookup"><span data-stu-id="e273b-488">Managing of page dependencies through [dependency injection](xref:fundamentals/dependency-injection).</span></span>
* <span data-ttu-id="e273b-489">對頁面進行[單元測試](xref:test/razor-pages-tests)。</span><span class="sxs-lookup"><span data-stu-id="e273b-489">[Unit testing](xref:test/razor-pages-tests) the pages.</span></span>

<span data-ttu-id="e273b-490">在 `POST` 要求上執行的頁面具有 「處理常式方法」`OnPostAsync` \*\* (當使用者張貼表單時)。</span><span class="sxs-lookup"><span data-stu-id="e273b-490">The page has an `OnPostAsync` *handler method*, which runs on `POST` requests (when a user posts the form).</span></span> <span data-ttu-id="e273b-491">您可以新增任何 HTTP 指令動詞的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-491">You can add handler methods for any HTTP verb.</span></span> <span data-ttu-id="e273b-492">最常見的處理常式包括：</span><span class="sxs-lookup"><span data-stu-id="e273b-492">The most common handlers are:</span></span>

* <span data-ttu-id="e273b-493">`OnGet`，初始化頁所需要的狀態。</span><span class="sxs-lookup"><span data-stu-id="e273b-493">`OnGet` to initialize state needed for the page.</span></span> <span data-ttu-id="e273b-494">[OnGet](#OnGet) 範例。</span><span class="sxs-lookup"><span data-stu-id="e273b-494">[OnGet](#OnGet) sample.</span></span>
* <span data-ttu-id="e273b-495">`OnPost`，處理表單提交作業。</span><span class="sxs-lookup"><span data-stu-id="e273b-495">`OnPost` to handle form submissions.</span></span>

<span data-ttu-id="e273b-496">`Async` 命名尾碼是選擇性的，但依照慣例通常用於非同步函式。</span><span class="sxs-lookup"><span data-stu-id="e273b-496">The `Async` naming suffix is optional but is often used by convention for asynchronous functions.</span></span> <span data-ttu-id="e273b-497">上述程式碼通常用於 Razor 頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-497">The preceding code is typical for Razor Pages.</span></span>

<span data-ttu-id="e273b-498">如果您熟悉如何使用控制器和 views 來 ASP.NET 應用程式：</span><span class="sxs-lookup"><span data-stu-id="e273b-498">If you're familiar with ASP.NET apps using controllers and views:</span></span>

* <span data-ttu-id="e273b-499">`OnPostAsync`上述範例中的程式碼看起來類似一般的控制器程式碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-499">The `OnPostAsync` code in the preceding example looks similar to typical controller code.</span></span>
* <span data-ttu-id="e273b-500">大部分的 MVC 基本類型（例如[模型](xref:mvc/models/model-binding)系結、[驗證](xref:mvc/models/validation)、[驗證](xref:mvc/models/validation)和動作結果）都是共用的。</span><span class="sxs-lookup"><span data-stu-id="e273b-500">Most of the MVC primitives like [model binding](xref:mvc/models/model-binding), [validation](xref:mvc/models/validation), [Validation](xref:mvc/models/validation),  and action results are shared.</span></span>

<span data-ttu-id="e273b-501">前一個 `OnPostAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="e273b-501">The previous `OnPostAsync` method:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync)]

<span data-ttu-id="e273b-502">`OnPostAsync` 的基本流程：</span><span class="sxs-lookup"><span data-stu-id="e273b-502">The basic flow of `OnPostAsync`:</span></span>

<span data-ttu-id="e273b-503">檢查驗證錯誤。</span><span class="sxs-lookup"><span data-stu-id="e273b-503">Check for validation errors.</span></span>

* <span data-ttu-id="e273b-504">如果沒有任何錯誤，會儲存資料並重新導向。</span><span class="sxs-lookup"><span data-stu-id="e273b-504">If there are no errors, save the data and redirect.</span></span>
* <span data-ttu-id="e273b-505">如果有錯誤，會再次顯示有驗證訊息的頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-505">If there are errors, show the page again with validation messages.</span></span> <span data-ttu-id="e273b-506">用戶端驗證和傳統的 ASP.NET Core MVC 應用程式完全相同。</span><span class="sxs-lookup"><span data-stu-id="e273b-506">Client-side validation is identical to traditional ASP.NET Core MVC applications.</span></span> <span data-ttu-id="e273b-507">在許多情況下，用戶端會偵測到驗證錯誤，但從不提交給伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-507">In many cases, validation errors would be detected on the client, and never submitted to the server.</span></span>

<span data-ttu-id="e273b-508">成功輸入資料後，`OnPostAsync` 處理常式方法會呼叫 `RedirectToPage` 協助程式方法，傳回 `RedirectToPageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e273b-508">When the data is entered successfully, the `OnPostAsync` handler method calls the `RedirectToPage` helper method to return an instance of `RedirectToPageResult`.</span></span> <span data-ttu-id="e273b-509">`RedirectToPage` 是新的動作結果，類似於 `RedirectToAction` 或 `RedirectToRoute`，但會針對頁面自訂。</span><span class="sxs-lookup"><span data-stu-id="e273b-509">`RedirectToPage` is a new action result, similar to `RedirectToAction` or `RedirectToRoute`, but customized for pages.</span></span> <span data-ttu-id="e273b-510">在上述範例中，它會重新導向至根索引頁面 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-510">In the preceding sample, it redirects to the root Index page (`/Index`).</span></span> <span data-ttu-id="e273b-511">[產生頁面 URL](#url_gen)一節會詳細說明 `RedirectToPage`。</span><span class="sxs-lookup"><span data-stu-id="e273b-511">`RedirectToPage` is detailed in the [URL generation for Pages](#url_gen) section.</span></span>

<span data-ttu-id="e273b-512">當提交的表單有驗證錯誤時 (傳遞至伺服器)，`OnPostAsync` 處理常式方法會呼叫 `Page` 協助程式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-512">When the submitted form has validation errors (that are passed to the server), the`OnPostAsync` handler method calls the `Page` helper method.</span></span> <span data-ttu-id="e273b-513">`Page` 傳回 `PageResult` 的執行個體。</span><span class="sxs-lookup"><span data-stu-id="e273b-513">`Page` returns an instance of `PageResult`.</span></span> <span data-ttu-id="e273b-514">傳回 `Page` 類似於控制站中的動作傳回 `View`。</span><span class="sxs-lookup"><span data-stu-id="e273b-514">Returning `Page` is similar to how actions in controllers return `View`.</span></span> <span data-ttu-id="e273b-515">`PageResult`這是處理常式方法的預設傳回型別。</span><span class="sxs-lookup"><span data-stu-id="e273b-515">`PageResult` is the default return type for a handler method.</span></span> <span data-ttu-id="e273b-516">傳回 `void` 的處理常式方法會呈現頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-516">A handler method that returns `void` renders the page.</span></span>

<span data-ttu-id="e273b-517">`Customer` 屬性 (property) 使用 `[BindProperty]` 屬性 (attribute) 加入模型繫結。</span><span class="sxs-lookup"><span data-stu-id="e273b-517">The `Customer` property uses `[BindProperty]` attribute to opt in to model binding.</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_PageModel&highlight=10-11)]

Razor<span data-ttu-id="e273b-518">根據預設，頁面只會系結屬性與非 `GET` 動詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-518"> Pages, by default, bind properties only with non-`GET` verbs.</span></span> <span data-ttu-id="e273b-519">繫結至屬性可以減少您必須撰寫的程式碼數量。</span><span class="sxs-lookup"><span data-stu-id="e273b-519">Binding to properties can reduce the amount of code you have to write.</span></span> <span data-ttu-id="e273b-520">透過使用相同的屬性呈現表單欄位 (`<input asp-for="Customer.Name">`) 並接受輸入，繫結可以減少程式碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-520">Binding reduces code by using the same property to render form fields (`<input asp-for="Customer.Name">`) and accept the input.</span></span>

[!INCLUDE[](~/includes/bind-get.md)]

<span data-ttu-id="e273b-521">首頁 (*Index.cshtml*)：</span><span class="sxs-lookup"><span data-stu-id="e273b-521">The home page (*Index.cshtml*):</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml)]

<span data-ttu-id="e273b-522">已建立關聯的 `PageModel` 類別 (*Index.cshtml.cs*)：</span><span class="sxs-lookup"><span data-stu-id="e273b-522">The associated `PageModel` class (*Index.cshtml.cs*):</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs)]

<span data-ttu-id="e273b-523">*Index.cshtml* 檔案包含下列標記可為每個連絡人建立編輯連結：</span><span class="sxs-lookup"><span data-stu-id="e273b-523">The *Index.cshtml* file contains the following markup to create an edit link for each contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=21)]

<span data-ttu-id="e273b-524">`<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>`[錨點](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper)標籤協助程式使用 `asp-route-{value}` 屬性來產生 [編輯] 頁面的連結。</span><span class="sxs-lookup"><span data-stu-id="e273b-524">The `<a asp-page="./Edit" asp-route-id="@contact.Id">Edit</a>` [Anchor Tag Helper](xref:mvc/views/tag-helpers/builtin-th/anchor-tag-helper) used the `asp-route-{value}` attribute to generate a link to the Edit page.</span></span> <span data-ttu-id="e273b-525">該連結包含路由資料和連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-525">The link contains route data with the contact ID.</span></span> <span data-ttu-id="e273b-526">例如： `https://localhost:5001/Edit/1` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-526">For example, `https://localhost:5001/Edit/1`.</span></span> <span data-ttu-id="e273b-527">[標記](xref:mvc/views/tag-helpers/intro)協助程式可讓伺服器端程式碼參與建立和轉譯檔案中的 HTML 元素 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-527">[Tag Helpers](xref:mvc/views/tag-helpers/intro) enable server-side code to participate in creating and rendering HTML elements in Razor files.</span></span> <span data-ttu-id="e273b-528">標記協助程式由 `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers` 啟用</span><span class="sxs-lookup"><span data-stu-id="e273b-528">Tag Helpers are enabled by `@addTagHelper *, Microsoft.AspNetCore.Mvc.TagHelpers`</span></span>

<span data-ttu-id="e273b-529">*Pages/Edit.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-529">The *Pages/Edit.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Edit.cshtml?highlight=1)]

<span data-ttu-id="e273b-530">第一行包含 `@page "{id:int}"` 指示詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-530">The first line contains the `@page "{id:int}"` directive.</span></span> <span data-ttu-id="e273b-531">路由條件約束 `"{id:int}"` 通知頁面接受包含 `int` 路由資料的頁面要求。</span><span class="sxs-lookup"><span data-stu-id="e273b-531">The routing constraint`"{id:int}"` tells the page to accept requests to the page that contain `int` route data.</span></span> <span data-ttu-id="e273b-532">如果頁面要求不包含可以轉換成 `int` 的路由資料，執行階段會傳回 HTTP 404 (找不到) 錯誤。</span><span class="sxs-lookup"><span data-stu-id="e273b-532">If a request to the page doesn't contain route data that can be converted to an `int`, the runtime returns an HTTP 404 (not found) error.</span></span> <span data-ttu-id="e273b-533">若要使識別碼成為選擇性，請將 `?` 附加至路由條件約束：</span><span class="sxs-lookup"><span data-stu-id="e273b-533">To make the ID optional, append `?` to the route constraint:</span></span>

 ```cshtml
@page "{id:int?}"
```

<span data-ttu-id="e273b-534">*Pages/Edit.cshtml.cs* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-534">The *Pages/Edit.cshtml.cs* file:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Edit.cshtml.cs)]

<span data-ttu-id="e273b-535">*Index.cshtml* 檔案也包含能夠為每個客戶連絡人建立刪除按鈕的標記：</span><span class="sxs-lookup"><span data-stu-id="e273b-535">The *Index.cshtml* file also contains markup to create a delete button for each customer contact:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Index.cshtml?range=22-23)]

<span data-ttu-id="e273b-536">使用 HTML 轉譯刪除按鈕時，其 `formaction` 會包含下列項目的參數：</span><span class="sxs-lookup"><span data-stu-id="e273b-536">When the delete button is rendered in HTML, its `formaction` includes parameters for:</span></span>

* <span data-ttu-id="e273b-537">`asp-route-id` 屬性指定的客戶連絡人識別碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-537">The customer contact ID specified by the `asp-route-id` attribute.</span></span>
* <span data-ttu-id="e273b-538">`asp-page-handler` 屬性指定的 `handler`。</span><span class="sxs-lookup"><span data-stu-id="e273b-538">The `handler` specified by the `asp-page-handler` attribute.</span></span>

<span data-ttu-id="e273b-539">以下是轉譯的刪除按鈕範例，內含客戶連絡人識別碼 `1`：</span><span class="sxs-lookup"><span data-stu-id="e273b-539">Here is an example of a rendered delete button with a customer contact ID of `1`:</span></span>

```html
<button type="submit" formaction="/?id=1&amp;handler=delete">delete</button>
```

<span data-ttu-id="e273b-540">選取按鈕時，表單 `POST` 要求會傳送至伺服器。</span><span class="sxs-lookup"><span data-stu-id="e273b-540">When the button is selected, a form `POST` request is sent to the server.</span></span> <span data-ttu-id="e273b-541">依照慣例，會依據配置 `OnPost[handler]Async`，按 `handler` 參數的值來選取處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-541">By convention, the name of the handler method is selected based on the value of the `handler` parameter according to the scheme `OnPost[handler]Async`.</span></span>

<span data-ttu-id="e273b-542">在此範例中，因為 `handler` 為 `delete`，所以會使用 `OnPostDeleteAsync` 處理常式方法來處理 `POST` 要求。</span><span class="sxs-lookup"><span data-stu-id="e273b-542">Because the `handler` is `delete` in this example, the `OnPostDeleteAsync` handler method is used to process the `POST` request.</span></span> <span data-ttu-id="e273b-543">若 `asp-page-handler` 設為其他值 (例如 `remove`)，則會選取名為 `OnPostRemoveAsync` 的處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-543">If the `asp-page-handler` is set to a different value, such as `remove`, a handler method with the name `OnPostRemoveAsync` is selected.</span></span> <span data-ttu-id="e273b-544">下列程式碼會顯示 `OnPostDeleteAsync` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="e273b-544">The following code shows the `OnPostDeleteAsync` handler:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Index.cshtml.cs?range=26-37)]

<span data-ttu-id="e273b-545">`OnPostDeleteAsync` 方法：</span><span class="sxs-lookup"><span data-stu-id="e273b-545">The `OnPostDeleteAsync` method:</span></span>

* <span data-ttu-id="e273b-546">接受查詢字串的 `id`。</span><span class="sxs-lookup"><span data-stu-id="e273b-546">Accepts the `id` from the query string.</span></span> <span data-ttu-id="e273b-547">如果包含路由條件約束的*Index. cshtml*頁面指示詞 `"{id:int?}"` ，則 `id` 會來自路由資料。</span><span class="sxs-lookup"><span data-stu-id="e273b-547">If the *Index.cshtml* page directive contained routing constraint `"{id:int?}"`, `id` would come from route data.</span></span> <span data-ttu-id="e273b-548">的路由資料 `id` 是在 URI 中指定，例如 `https://localhost:5001/Customers/2` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-548">The route data for `id` is specified in the URI such as `https://localhost:5001/Customers/2`.</span></span>
* <span data-ttu-id="e273b-549">使用 `FindAsync` 在資料庫中查詢客戶連絡人。</span><span class="sxs-lookup"><span data-stu-id="e273b-549">Queries the database for the customer contact with `FindAsync`.</span></span>
* <span data-ttu-id="e273b-550">若找到客戶連絡人，會從客戶連絡人清單中予以移除。</span><span class="sxs-lookup"><span data-stu-id="e273b-550">If the customer contact is found, they're removed from the list of customer contacts.</span></span> <span data-ttu-id="e273b-551">資料庫隨即更新。</span><span class="sxs-lookup"><span data-stu-id="e273b-551">The database is updated.</span></span>
* <span data-ttu-id="e273b-552">呼叫 `RedirectToPage` 以重新導向至根索引頁 (`/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-552">Calls `RedirectToPage` to redirect to the root Index page (`/Index`).</span></span>

## <a name="mark-page-properties-as-required"></a><span data-ttu-id="e273b-553">將頁面屬性標示為必要</span><span class="sxs-lookup"><span data-stu-id="e273b-553">Mark page properties as required</span></span>

<span data-ttu-id="e273b-554">上的屬性 `PageModel` 可以標記為[必要](/dotnet/api/system.componentmodel.dataannotations.requiredattribute)的屬性：</span><span class="sxs-lookup"><span data-stu-id="e273b-554">Properties on a `PageModel` can be marked with the [Required](/dotnet/api/system.componentmodel.dataannotations.requiredattribute) attribute:</span></span>

[!code-cs[](index/sample/Create.cshtml.cs?highlight=3,15-16)]

<span data-ttu-id="e273b-555">如需詳細資訊，請參閱[模型驗證](xref:mvc/models/validation)。</span><span class="sxs-lookup"><span data-stu-id="e273b-555">For more information, see [Model validation](xref:mvc/models/validation).</span></span>

## <a name="handle-head-requests-with-an-onget-handler-fallback"></a><span data-ttu-id="e273b-556">使用 OnGet 處理常式後援來處理 HEAD 要求</span><span class="sxs-lookup"><span data-stu-id="e273b-556">Handle HEAD requests with an OnGet handler fallback</span></span>

<span data-ttu-id="e273b-557">`HEAD` 要求可讓您擷取特定資源的標頭。</span><span class="sxs-lookup"><span data-stu-id="e273b-557">`HEAD` requests allow you to retrieve the headers for a specific resource.</span></span> <span data-ttu-id="e273b-558">不同於 `GET` 要求，`HEAD` 要求不會傳回回應主體。</span><span class="sxs-lookup"><span data-stu-id="e273b-558">Unlike `GET` requests, `HEAD` requests don't return a response body.</span></span>

<span data-ttu-id="e273b-559">一般來說，會為 `HEAD` 要求建立及呼叫 `OnHead` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="e273b-559">Ordinarily, an `OnHead` handler is created and called for `HEAD` requests:</span></span> 

```csharp
public void OnHead()
{
    HttpContext.Response.Headers.Add("HandledBy", "Handled by OnHead!");
}
```

<span data-ttu-id="e273b-560">在 ASP.NET Core 2.1 或更新版本中， Razor `OnGet` 如果未定義任何處理程式，頁面就會回復為呼叫處理常式 `OnHead` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-560">In ASP.NET Core 2.1 or later, Razor Pages falls back to calling the `OnGet` handler if no `OnHead` handler is defined.</span></span> <span data-ttu-id="e273b-561">這個行為藉由在 `Startup.ConfigureServices` 中呼叫 [SetCompatibilityVersion](xref:mvc/compatibility-version) 來啟用：</span><span class="sxs-lookup"><span data-stu-id="e273b-561">This behavior is enabled by the call to [SetCompatibilityVersion](xref:mvc/compatibility-version) in `Startup.ConfigureServices`:</span></span>

```csharp
services.AddMvc()
    .SetCompatibilityVersion(CompatibilityVersion.Version_2_1);
```

<span data-ttu-id="e273b-562">預設範本會在 ASP.NET Core 2.1 和 2.2 中產生 `SetCompatibilityVersion` 呼叫。</span><span class="sxs-lookup"><span data-stu-id="e273b-562">The default templates generate the `SetCompatibilityVersion` call in ASP.NET Core 2.1 and 2.2.</span></span> <span data-ttu-id="e273b-563">`SetCompatibilityVersion`將 Pages 選項有效地設定 Razor `AllowMappingHeadRequestsToGetHandler` 為 `true` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-563">`SetCompatibilityVersion` effectively sets the Razor Pages option `AllowMappingHeadRequestsToGetHandler` to `true`.</span></span>

<span data-ttu-id="e273b-564">您可以明確選擇「特定」\*\* 行為，而不必透過 `SetCompatibilityVersion` 選擇所有行為。</span><span class="sxs-lookup"><span data-stu-id="e273b-564">Rather than opting in to all behaviors with `SetCompatibilityVersion`, you can explicitly opt in to *specific* behaviors.</span></span> <span data-ttu-id="e273b-565">下列程式碼會選擇讓 `HEAD` 要求對應到 `OnGet` 處理常式：</span><span class="sxs-lookup"><span data-stu-id="e273b-565">The following code opts in to allowing `HEAD` requests to be mapped to the `OnGet` handler:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        options.AllowMappingHeadRequestsToGetHandler = true;
    });
```

<a name="xsrf"></a>

## <a name="xsrfcsrf-and-razor-pages"></a><span data-ttu-id="e273b-566">XSRF/CSRF 和 Razor Pages</span><span class="sxs-lookup"><span data-stu-id="e273b-566">XSRF/CSRF and Razor Pages</span></span>

<span data-ttu-id="e273b-567">您不必撰寫任何[防偽驗證](xref:security/anti-request-forgery)程式碼。</span><span class="sxs-lookup"><span data-stu-id="e273b-567">You don't have to write any code for [antiforgery validation](xref:security/anti-request-forgery).</span></span> <span data-ttu-id="e273b-568">Antiforgery 權杖的產生和驗證會自動包含在 Razor 頁面中。</span><span class="sxs-lookup"><span data-stu-id="e273b-568">Antiforgery token generation and validation are automatically included in Razor Pages.</span></span>

<a name="layout"></a>

## <a name="using-layouts-partials-templates-and-tag-helpers-with-razor-pages"></a><span data-ttu-id="e273b-569">搭配頁面使用版面配置、部分、範本和標籤協助程式 Razor</span><span class="sxs-lookup"><span data-stu-id="e273b-569">Using Layouts, partials, templates, and Tag Helpers with Razor Pages</span></span>

<span data-ttu-id="e273b-570">頁面會與視圖引擎的所有功能搭配使用 Razor 。</span><span class="sxs-lookup"><span data-stu-id="e273b-570">Pages work with all the capabilities of the Razor view engine.</span></span> <span data-ttu-id="e273b-571">版面配置、部分、範本、標記協助程式、 *_ViewStart* *_ViewImports cshtml*的工作方式，與傳統 Razor 視圖相同。</span><span class="sxs-lookup"><span data-stu-id="e273b-571">Layouts, partials, templates, Tag Helpers, *_ViewStart.cshtml*, *_ViewImports.cshtml* work in the same way they do for conventional Razor views.</span></span>

<span data-ttu-id="e273b-572">可利用這些功能的一部分來整理這個頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-572">Let's declutter this page by taking advantage of some of those capabilities.</span></span>

<span data-ttu-id="e273b-573">將[版面配置頁面](xref:mvc/views/layout)新增至 *Pages/Shared/_Layout.cshtml*：</span><span class="sxs-lookup"><span data-stu-id="e273b-573">Add a [layout page](xref:mvc/views/layout) to *Pages/Shared/_Layout.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_LayoutSimple.cshtml)]

<span data-ttu-id="e273b-574">[版面](xref:mvc/views/layout)配置：</span><span class="sxs-lookup"><span data-stu-id="e273b-574">The [Layout](xref:mvc/views/layout):</span></span>

* <span data-ttu-id="e273b-575">控制每個頁面的版面配置 (除非頁面退出版面配置)。</span><span class="sxs-lookup"><span data-stu-id="e273b-575">Controls the layout of each page (unless the page opts out of layout).</span></span>
* <span data-ttu-id="e273b-576">匯入 HTML 結構，例如 JavaScript 和樣式表。</span><span class="sxs-lookup"><span data-stu-id="e273b-576">Imports HTML structures such as JavaScript and stylesheets.</span></span>

<span data-ttu-id="e273b-577">如需詳細資訊，請參閱[版面配置頁面](xref:mvc/views/layout)。</span><span class="sxs-lookup"><span data-stu-id="e273b-577">See [layout page](xref:mvc/views/layout) for more information.</span></span>

<span data-ttu-id="e273b-578">[版面配置](xref:mvc/views/layout#specifying-a-layout)屬性是在 *Pages/_ViewStart.cshtml* 中設定：</span><span class="sxs-lookup"><span data-stu-id="e273b-578">The [Layout](xref:mvc/views/layout#specifying-a-layout) property is set in *Pages/_ViewStart.cshtml*:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewStart.cshtml)]

<span data-ttu-id="e273b-579">版面配置位於 *Pages/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-579">The layout is in the *Pages/Shared* folder.</span></span> <span data-ttu-id="e273b-580">頁面會以階層方式尋找其他檢視 (版面配置、範本、部分)，從目前頁面的相同資料夾開始。</span><span class="sxs-lookup"><span data-stu-id="e273b-580">Pages look for other views (layouts, templates, partials) hierarchically, starting in the same folder as the current page.</span></span> <span data-ttu-id="e273b-581">*Pages/Shared*資料夾中的版面配置可以從 Razor [ *pages* ] 資料夾下的任何頁面使用。</span><span class="sxs-lookup"><span data-stu-id="e273b-581">A layout in the *Pages/Shared* folder can be used from any Razor page under the *Pages* folder.</span></span>

<span data-ttu-id="e273b-582">版面配置頁面應位於 *Pages/Shared* 資料夾中。</span><span class="sxs-lookup"><span data-stu-id="e273b-582">The layout file should go in the *Pages/Shared* folder.</span></span>

<span data-ttu-id="e273b-583">我們**不**建議您將配置檔案放入 *Views/Shared* 資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-583">We recommend you **not** put the layout file in the *Views/Shared* folder.</span></span> <span data-ttu-id="e273b-584">*Views/Shared* 是 MVC 檢視模式。</span><span class="sxs-lookup"><span data-stu-id="e273b-584">*Views/Shared* is an MVC views pattern.</span></span> Razor<span data-ttu-id="e273b-585">頁面的目的是要依賴資料夾階層，而不是路徑慣例。</span><span class="sxs-lookup"><span data-stu-id="e273b-585"> Pages are meant to rely on folder hierarchy, not path conventions.</span></span>

<span data-ttu-id="e273b-586">從頁面中查看搜尋 Razor 包含*Pages*資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-586">View search from a Razor Page includes the *Pages* folder.</span></span> <span data-ttu-id="e273b-587">您在 MVC 控制器和傳統視圖中使用的配置、範本和部分 Razor 都*是可行*的。</span><span class="sxs-lookup"><span data-stu-id="e273b-587">The layouts, templates, and partials you're using with MVC controllers and conventional Razor views *just work*.</span></span>

<span data-ttu-id="e273b-588">新增 *Pages/_ViewImports.cshtml* 檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-588">Add a *Pages/_ViewImports.cshtml* file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml)]

<span data-ttu-id="e273b-589">本教學課程稍後會說明 `@namespace`。</span><span class="sxs-lookup"><span data-stu-id="e273b-589">`@namespace` is explained later in the tutorial.</span></span> <span data-ttu-id="e273b-590">`@addTagHelper` 指示詞會將[內建標記協助程式](xref:mvc/views/tag-helpers/builtin-th/Index)帶入 *Pages* 資料夾中的所有頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-590">The `@addTagHelper` directive brings in the [built-in Tag Helpers](xref:mvc/views/tag-helpers/builtin-th/Index) to all the pages in the *Pages* folder.</span></span>

<a name="namespace"></a>

<span data-ttu-id="e273b-591">在頁面上明確使用 `@namespace` 指示詞時：</span><span class="sxs-lookup"><span data-stu-id="e273b-591">When the `@namespace` directive is used explicitly on a page:</span></span>

[!code-cshtml[](index/sample/RazorPagesIntro/Pages/Customers/Namespace2.cshtml?highlight=2)]

<span data-ttu-id="e273b-592">指示詞會設定頁面的命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-592">The directive sets the namespace for the page.</span></span> <span data-ttu-id="e273b-593">`@model` 指示詞不需要包含命名空間。</span><span class="sxs-lookup"><span data-stu-id="e273b-593">The `@model` directive doesn't need to include the namespace.</span></span>

<span data-ttu-id="e273b-594">當 `@namespace` 指示詞包含在 *_ViewImports.cshtml* 中時，指定的命名空間會在匯入 `@namespace` 指示詞的頁面中提供所產生之命名空間的前置詞。</span><span class="sxs-lookup"><span data-stu-id="e273b-594">When the `@namespace` directive is contained in *_ViewImports.cshtml*, the specified namespace supplies the prefix for the generated namespace in the Page that imports the `@namespace` directive.</span></span> <span data-ttu-id="e273b-595">所產生命名空間的其餘部分 (後置字元部分) 是包含 *_ViewImports.cshtml* 的資料夾和包含頁面的資料夾之間，以句點分隔的相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e273b-595">The rest of the generated namespace (the suffix portion) is the dot-separated relative path between the folder containing *_ViewImports.cshtml* and the folder containing the page.</span></span>

<span data-ttu-id="e273b-596">例如，`PageModel` 類別 *Pages/Customers/Edit.cshtml.cs* 會明確地設定命名空間：</span><span class="sxs-lookup"><span data-stu-id="e273b-596">For example, the `PageModel` class *Pages/Customers/Edit.cshtml.cs* explicitly sets the namespace:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/Edit.cshtml.cs?name=snippet_namespace)]

<span data-ttu-id="e273b-597">*Pages/_ViewImports.cshtml* 檔案會設定下列命名空間：</span><span class="sxs-lookup"><span data-stu-id="e273b-597">The *Pages/_ViewImports.cshtml* file sets the following namespace:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/_ViewImports.cshtml?highlight=1)]

<span data-ttu-id="e273b-598">針對 [ *Pages/Customers/編輯. cshtml* ] 頁面產生的命名空間與 Razor 類別相同 `PageModel` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-598">The generated namespace for the *Pages/Customers/Edit.cshtml* Razor Page is the same as the `PageModel` class.</span></span>

<span data-ttu-id="e273b-599">`@namespace`*也適用于傳統的 Razor 視圖。*</span><span class="sxs-lookup"><span data-stu-id="e273b-599">`@namespace` *also works with conventional Razor views.*</span></span>

<span data-ttu-id="e273b-600">原始的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-600">The original *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts/Pages/Create.cshtml?highlight=2)]

<span data-ttu-id="e273b-601">更新的 *Pages/Create.cshtml* 檢視檔案：</span><span class="sxs-lookup"><span data-stu-id="e273b-601">The updated *Pages/Create.cshtml* view file:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/Create.cshtml?highlight=2)]

<span data-ttu-id="e273b-602">[ Razor Pages 入門專案](#rpvs17)包含*pages/_ValidationScriptsPartial. cshtml*，這會連結用戶端驗證。</span><span class="sxs-lookup"><span data-stu-id="e273b-602">The [Razor Pages starter project](#rpvs17) contains the *Pages/_ValidationScriptsPartial.cshtml*, which hooks up client-side validation.</span></span>

<span data-ttu-id="e273b-603">如需部分檢視的詳細資訊，請參閱 <xref:mvc/views/partial>。</span><span class="sxs-lookup"><span data-stu-id="e273b-603">For more information on partial views, see <xref:mvc/views/partial>.</span></span>

<a name="url_gen"></a>

## <a name="url-generation-for-pages"></a><span data-ttu-id="e273b-604">產生頁面 URL</span><span class="sxs-lookup"><span data-stu-id="e273b-604">URL generation for Pages</span></span>

<span data-ttu-id="e273b-605">前面出現過的 `Create` 頁面使用 `RedirectToPage`：</span><span class="sxs-lookup"><span data-stu-id="e273b-605">The `Create` page, shown previously, uses `RedirectToPage`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/Pages/Create.cshtml.cs?name=snippet_OnPostAsync&highlight=10)]

<span data-ttu-id="e273b-606">應用程式有下列檔案/資料夾結構：</span><span class="sxs-lookup"><span data-stu-id="e273b-606">The app has the following file/folder structure:</span></span>

* <span data-ttu-id="e273b-607">*/Pages*</span><span class="sxs-lookup"><span data-stu-id="e273b-607">*/Pages*</span></span>

  * <span data-ttu-id="e273b-608">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-608">*Index.cshtml*</span></span>
  * <span data-ttu-id="e273b-609">*/Customers*</span><span class="sxs-lookup"><span data-stu-id="e273b-609">*/Customers*</span></span>

    * <span data-ttu-id="e273b-610">*Create.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-610">*Create.cshtml*</span></span>
    * <span data-ttu-id="e273b-611">*Edit.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-611">*Edit.cshtml*</span></span>
    * <span data-ttu-id="e273b-612">*Index.cshtml*</span><span class="sxs-lookup"><span data-stu-id="e273b-612">*Index.cshtml*</span></span>

<span data-ttu-id="e273b-613">*Pages/Customers/Create.cshtml* 和 *Pages/Customers/Edit.cshtml* 頁面在成功後會重新導向至 *Pages/Index.cshtml*。</span><span class="sxs-lookup"><span data-stu-id="e273b-613">The *Pages/Customers/Create.cshtml* and *Pages/Customers/Edit.cshtml* pages redirect to *Pages/Index.cshtml* after success.</span></span> <span data-ttu-id="e273b-614">字串 `/Index` 為 URI 的一部分，可存取前一個頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-614">The string `/Index` is part of the URI to access the preceding page.</span></span> <span data-ttu-id="e273b-615">字串 `/Index` 可以用來產生 *Pages/Index.cshtml* 頁面的 URI。</span><span class="sxs-lookup"><span data-stu-id="e273b-615">The string `/Index` can be used to generate URIs to the *Pages/Index.cshtml* page.</span></span> <span data-ttu-id="e273b-616">例如：</span><span class="sxs-lookup"><span data-stu-id="e273b-616">For example:</span></span>

* `Url.Page("/Index", ...)`
* `<a asp-page="/Index">My Index Page</a>`
* `RedirectToPage("/Index")`

<span data-ttu-id="e273b-617">頁面名稱是從根 */Pages* 資料夾到該頁面的路徑 (包括前置的 `/`，例如 `/Index`)。</span><span class="sxs-lookup"><span data-stu-id="e273b-617">The page name is the path to the page from the root */Pages* folder including a leading `/` (for example, `/Index`).</span></span> <span data-ttu-id="e273b-618">上述 URL 產生範例，透過硬式編碼的 URL 提供更加優異的選項與功能。</span><span class="sxs-lookup"><span data-stu-id="e273b-618">The preceding URL generation samples offer enhanced options and functional capabilities over hardcoding a URL.</span></span> <span data-ttu-id="e273b-619">URL 產生使用[路由](xref:mvc/controllers/routing)，可以根據路由在目的地路徑中定義的方式，產生並且編碼參數。</span><span class="sxs-lookup"><span data-stu-id="e273b-619">URL generation uses [routing](xref:mvc/controllers/routing) and can generate and encode parameters according to how the route is defined in the destination path.</span></span>

<span data-ttu-id="e273b-620">產生頁面 URL 支援相關的名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-620">URL generation for pages supports relative names.</span></span> <span data-ttu-id="e273b-621">下表顯示從 *Pages/Customers/Create.cshtml* 以不同的 `RedirectToPage` 參數選取的索引頁：</span><span class="sxs-lookup"><span data-stu-id="e273b-621">The following table shows which Index page is selected with different `RedirectToPage` parameters from *Pages/Customers/Create.cshtml*:</span></span>

| <span data-ttu-id="e273b-622">RedirectToPage(x)</span><span class="sxs-lookup"><span data-stu-id="e273b-622">RedirectToPage(x)</span></span>| <span data-ttu-id="e273b-623">頁面</span><span class="sxs-lookup"><span data-stu-id="e273b-623">Page</span></span> |
| ----------------- | ------------ |
| <span data-ttu-id="e273b-624">RedirectToPage("/Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-624">RedirectToPage("/Index")</span></span> | <span data-ttu-id="e273b-625">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-625">*Pages/Index*</span></span> |
| <span data-ttu-id="e273b-626">RedirectToPage("./Index");</span><span class="sxs-lookup"><span data-stu-id="e273b-626">RedirectToPage("./Index");</span></span> | <span data-ttu-id="e273b-627">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-627">*Pages/Customers/Index*</span></span> |
| <span data-ttu-id="e273b-628">RedirectToPage("../Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-628">RedirectToPage("../Index")</span></span> | <span data-ttu-id="e273b-629">*Pages/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-629">*Pages/Index*</span></span> |
| <span data-ttu-id="e273b-630">RedirectToPage("Index")</span><span class="sxs-lookup"><span data-stu-id="e273b-630">RedirectToPage("Index")</span></span>  | <span data-ttu-id="e273b-631">*Pages/Customers/Index*</span><span class="sxs-lookup"><span data-stu-id="e273b-631">*Pages/Customers/Index*</span></span> |

<span data-ttu-id="e273b-632">`RedirectToPage("Index")`、`RedirectToPage("./Index")` 和 `RedirectToPage("../Index")` 是「相對名稱」\*\*。</span><span class="sxs-lookup"><span data-stu-id="e273b-632">`RedirectToPage("Index")`, `RedirectToPage("./Index")`, and `RedirectToPage("../Index")`  are *relative names*.</span></span> <span data-ttu-id="e273b-633">`RedirectToPage` 參數「結合」\*\* 了目前頁面的路徑，以計算目的地頁面的名稱。</span><span class="sxs-lookup"><span data-stu-id="e273b-633">The `RedirectToPage` parameter is *combined* with the path of the current page to compute the name of the destination page.</span></span>  <!-- Review: Original had The provided string is combined with the page name of the current page to compute the name of the destination page.  page name, not page path -->

<span data-ttu-id="e273b-634">相對名稱連結在以複雜結構建置網站時很有用。</span><span class="sxs-lookup"><span data-stu-id="e273b-634">Relative name linking is useful when building sites with a complex structure.</span></span> <span data-ttu-id="e273b-635">如果您使用相對名稱連結資料夾中的頁面，您可以重新命名該資料夾。</span><span class="sxs-lookup"><span data-stu-id="e273b-635">If you use relative names to link between pages in a folder, you can rename that folder.</span></span> <span data-ttu-id="e273b-636">所有連結仍可運作 (因為它們不包含資料夾名稱)。</span><span class="sxs-lookup"><span data-stu-id="e273b-636">All the links still work (because they didn't include the folder name).</span></span>

<span data-ttu-id="e273b-637">若要重新導向到不同[區域](xref:mvc/controllers/areas)中的頁面，請指定區域：</span><span class="sxs-lookup"><span data-stu-id="e273b-637">To redirect to a page in a different [Area](xref:mvc/controllers/areas), specify the area:</span></span>

```csharp
RedirectToPage("/Index", new { area = "Services" });
```

<span data-ttu-id="e273b-638">如需詳細資訊，請參閱 <xref:mvc/controllers/areas> 。</span><span class="sxs-lookup"><span data-stu-id="e273b-638">For more information, see <xref:mvc/controllers/areas>.</span></span>

## <a name="viewdata-attribute"></a><span data-ttu-id="e273b-639">ViewData 屬性</span><span class="sxs-lookup"><span data-stu-id="e273b-639">ViewData attribute</span></span>

<span data-ttu-id="e273b-640">資料可以傳遞至具有 [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute) 的頁面。</span><span class="sxs-lookup"><span data-stu-id="e273b-640">Data can be passed to a page with [ViewDataAttribute](/dotnet/api/microsoft.aspnetcore.mvc.viewdataattribute).</span></span> <span data-ttu-id="e273b-641">具有屬性的控制器或頁面模型上的屬性， Razor `[ViewData]` 其值會從[ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary)儲存並載入。</span><span class="sxs-lookup"><span data-stu-id="e273b-641">Properties on controllers or Razor Page models with the `[ViewData]` attribute have their values stored and loaded from the [ViewDataDictionary](/dotnet/api/microsoft.aspnetcore.mvc.viewfeatures.viewdatadictionary).</span></span>

<span data-ttu-id="e273b-642">在下列範例中， `AboutModel` 包含以 `Title` 標記的屬性 `[ViewData]` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-642">In the following example, the `AboutModel` contains a `Title` property marked with `[ViewData]`.</span></span> <span data-ttu-id="e273b-643">`Title` 屬性會設定為 [關於] 頁面的標題：</span><span class="sxs-lookup"><span data-stu-id="e273b-643">The `Title` property is set to the title of the About page:</span></span>

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

<span data-ttu-id="e273b-644">在 [關於] 頁面上，存取 `Title` 屬性作為模型屬性：</span><span class="sxs-lookup"><span data-stu-id="e273b-644">In the About page, access the `Title` property as a model property:</span></span>

```cshtml
<h1>@Model.Title</h1>
```

<span data-ttu-id="e273b-645">在此配置中，標題會從 ViewData 字典中讀取：</span><span class="sxs-lookup"><span data-stu-id="e273b-645">In the layout, the title is read from the ViewData dictionary:</span></span>

```cshtml
<!DOCTYPE html>
<html lang="en">
<head>
    <title>@ViewData["Title"] - WebApplication</title>
    ...
```

## <a name="tempdata"></a><span data-ttu-id="e273b-646">TempData</span><span class="sxs-lookup"><span data-stu-id="e273b-646">TempData</span></span>

<span data-ttu-id="e273b-647">ASP.NET Core 公開[控制器](/dotnet/api/microsoft.aspnetcore.mvc.controller)上的 [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) 屬性。</span><span class="sxs-lookup"><span data-stu-id="e273b-647">ASP.NET Core exposes the [TempData](/dotnet/api/microsoft.aspnetcore.mvc.controller.tempdata?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_Controller_TempData) property on a [controller](/dotnet/api/microsoft.aspnetcore.mvc.controller).</span></span> <span data-ttu-id="e273b-648">這個屬性會儲存資料，直到讀取為止。</span><span class="sxs-lookup"><span data-stu-id="e273b-648">This property stores data until it's read.</span></span> <span data-ttu-id="e273b-649">`Keep` 和 `Peek` 方法可以用來檢查資料，不用刪除。</span><span class="sxs-lookup"><span data-stu-id="e273b-649">The `Keep` and `Peek` methods can be used to examine the data without deletion.</span></span> <span data-ttu-id="e273b-650">當有多個要求需要資料時，`TempData` 對重新導向很有幫助。</span><span class="sxs-lookup"><span data-stu-id="e273b-650">`TempData` is  useful for redirection, when data is needed for more than a single request.</span></span>

<span data-ttu-id="e273b-651">下列程式碼會設定使用 `TempData` 的 `Message` 值：</span><span class="sxs-lookup"><span data-stu-id="e273b-651">The following code sets the value of `Message` using `TempData`:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateDot.cshtml.cs?highlight=10-11,25&name=snippet_Temp)]

<span data-ttu-id="e273b-652">*Pages/Customers/Index.cshtml* 檔案中的下列標記會顯示使用 `TempData` 的 `Message` 值。</span><span class="sxs-lookup"><span data-stu-id="e273b-652">The following markup in the *Pages/Customers/Index.cshtml* file displays the value of `Message` using `TempData`.</span></span>

```cshtml
<h3>Msg: @Model.Message</h3>
```

<span data-ttu-id="e273b-653">*Pages/Customers/Index.cshtml.cs* 頁面模型會將 `[TempData]` 屬性 (attribute) 套用到 `Message` 屬性 (property)。</span><span class="sxs-lookup"><span data-stu-id="e273b-653">The *Pages/Customers/Index.cshtml.cs* page model applies the `[TempData]` attribute to the `Message` property.</span></span>

```csharp
[TempData]
public string Message { get; set; }
```

<span data-ttu-id="e273b-654">如需詳細資訊，請參閱 [TempData](xref:fundamentals/app-state#tempdata)。</span><span class="sxs-lookup"><span data-stu-id="e273b-654">For more information, see [TempData](xref:fundamentals/app-state#tempdata) .</span></span>

<a name="mhpp"></a>

## <a name="multiple-handlers-per-page"></a><span data-ttu-id="e273b-655">每頁面有多個處理常式</span><span class="sxs-lookup"><span data-stu-id="e273b-655">Multiple handlers per page</span></span>

<span data-ttu-id="e273b-656">下列頁面會使用 `asp-page-handler` 標記協助程式為兩個處理常式產生標記：</span><span class="sxs-lookup"><span data-stu-id="e273b-656">The following page generates markup for two handlers using the `asp-page-handler` Tag Helper:</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?highlight=12-13)]

<!-- Review: the FormActionTagHelper applies to all <form /> elements on a Razor page, even when there's no `asp-` attribute   -->

<span data-ttu-id="e273b-657">上例中的表單有兩個提交按鈕，每一個都使用 `FormActionTagHelper` 提交至不同的 URL。</span><span class="sxs-lookup"><span data-stu-id="e273b-657">The form in the preceding example has two submit buttons, each using the `FormActionTagHelper` to submit to a different URL.</span></span> <span data-ttu-id="e273b-658">`asp-page-handler` 屬性附隨於 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="e273b-658">The `asp-page-handler` attribute is a companion to `asp-page`.</span></span> <span data-ttu-id="e273b-659">`asp-page-handler` 產生的 URL 會提交至頁面所定義的每一個處理常式方法。</span><span class="sxs-lookup"><span data-stu-id="e273b-659">`asp-page-handler` generates URLs that submit to each of the handler methods defined by a page.</span></span> <span data-ttu-id="e273b-660">因為範例連結至目前的頁面，所以未指定 `asp-page`。</span><span class="sxs-lookup"><span data-stu-id="e273b-660">`asp-page` isn't specified because the sample is linking to the current page.</span></span>

<span data-ttu-id="e273b-661">頁面模型：</span><span class="sxs-lookup"><span data-stu-id="e273b-661">The page model:</span></span>

[!code-cs[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml.cs?highlight=20,32)]

<span data-ttu-id="e273b-662">上述程式碼使用「具名的處理常式方法」\*\*。</span><span class="sxs-lookup"><span data-stu-id="e273b-662">The preceding code uses *named handler methods*.</span></span> <span data-ttu-id="e273b-663">具名的處理常式方法的建立方式是採用名稱中在 `On<HTTP Verb>` 後面、`Async` 之前 (如有) 的文字。</span><span class="sxs-lookup"><span data-stu-id="e273b-663">Named handler methods are created by taking the text in the name after `On<HTTP Verb>` and before `Async` (if present).</span></span> <span data-ttu-id="e273b-664">在上例中，頁面方法是 OnPost**JoinList**Async 和 OnPost**JoinListUC**Async。</span><span class="sxs-lookup"><span data-stu-id="e273b-664">In the preceding example, the page methods are OnPost**JoinList**Async and OnPost**JoinListUC**Async.</span></span> <span data-ttu-id="e273b-665">移除 *OnPost* 和 *Async*，處理常式名稱就是 `JoinList` 和 `JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-665">With *OnPost* and *Async* removed, the handler names are `JoinList` and `JoinListUC`.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateFATH.cshtml?range=12-13)]

<span data-ttu-id="e273b-666">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinList`。</span><span class="sxs-lookup"><span data-stu-id="e273b-666">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinList`.</span></span> <span data-ttu-id="e273b-667">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-667">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH?handler=JoinListUC`.</span></span>

## <a name="custom-routes"></a><span data-ttu-id="e273b-668">自訂路由</span><span class="sxs-lookup"><span data-stu-id="e273b-668">Custom routes</span></span>

<span data-ttu-id="e273b-669">使用 `@page` 指示詞，可以：</span><span class="sxs-lookup"><span data-stu-id="e273b-669">Use the `@page` directive to:</span></span>

* <span data-ttu-id="e273b-670">指定頁面的自訂路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-670">Specify a custom route to a page.</span></span> <span data-ttu-id="e273b-671">例如，[關於] 頁面的路由可使用 `@page "/Some/Other/Path"` 設為 `/Some/Other/Path`。</span><span class="sxs-lookup"><span data-stu-id="e273b-671">For example, the route to the About page can be set to `/Some/Other/Path` with `@page "/Some/Other/Path"`.</span></span>
* <span data-ttu-id="e273b-672">將區段附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-672">Append segments to a page's default route.</span></span> <span data-ttu-id="e273b-673">例如，使用 `@page "item"` 可將 "item" 區段新增到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-673">For example, an "item" segment can be added to a page's default route with `@page "item"`.</span></span>
* <span data-ttu-id="e273b-674">將參數附加到頁面的預設路由。</span><span class="sxs-lookup"><span data-stu-id="e273b-674">Append parameters to a page's default route.</span></span> <span data-ttu-id="e273b-675">例如，具有 `@page "{id}"` 的頁面可要求識別碼參數 `id`。</span><span class="sxs-lookup"><span data-stu-id="e273b-675">For example, an ID parameter, `id`, can be required for a page with `@page "{id}"`.</span></span>

<span data-ttu-id="e273b-676">支援在路徑開頭以波狀符號 (`~`) 指定根相對路徑。</span><span class="sxs-lookup"><span data-stu-id="e273b-676">A root-relative path designated by a tilde (`~`) at the beginning of the path is supported.</span></span> <span data-ttu-id="e273b-677">例如，`@page "~/Some/Other/Path"` 與 `@page "/Some/Other/Path"` 相同。</span><span class="sxs-lookup"><span data-stu-id="e273b-677">For example, `@page "~/Some/Other/Path"` is the same as `@page "/Some/Other/Path"`.</span></span>

<span data-ttu-id="e273b-678">如果您不喜歡 URL 中的查詢字串 `?handler=JoinList` ，請變更路由，將處理常式名稱放在 url 的路徑部分。</span><span class="sxs-lookup"><span data-stu-id="e273b-678">If you don't like the query string `?handler=JoinList` in the URL, change the route to put the handler name in the path portion of the URL.</span></span> <span data-ttu-id="e273b-679">藉由在指示詞後面加上雙引號括住的路由範本，可以自訂路由 `@page` 。</span><span class="sxs-lookup"><span data-stu-id="e273b-679">The route can be customized by adding a route template enclosed in double quotes after the `@page` directive.</span></span>

[!code-cshtml[](index/sample/RazorPagesContacts2/Pages/Customers/CreateRoute.cshtml?highlight=1)]

<span data-ttu-id="e273b-680">使用上述程式碼，提交至 `OnPostJoinListAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinList`。</span><span class="sxs-lookup"><span data-stu-id="e273b-680">Using the preceding code, the URL path that submits to `OnPostJoinListAsync` is `https://localhost:5001/Customers/CreateFATH/JoinList`.</span></span> <span data-ttu-id="e273b-681">提交至 `OnPostJoinListUCAsync` 的 URL 路徑是 `https://localhost:5001/Customers/CreateFATH/JoinListUC`。</span><span class="sxs-lookup"><span data-stu-id="e273b-681">The URL path that submits to `OnPostJoinListUCAsync` is `https://localhost:5001/Customers/CreateFATH/JoinListUC`.</span></span>

<span data-ttu-id="e273b-682">跟在 `handler` 後面的 `?` 表示路由參數為選擇性。</span><span class="sxs-lookup"><span data-stu-id="e273b-682">The `?` following `handler` means the route parameter is optional.</span></span>

## <a name="configuration-and-settings"></a><span data-ttu-id="e273b-683">組態與設定</span><span class="sxs-lookup"><span data-stu-id="e273b-683">Configuration and settings</span></span>

<span data-ttu-id="e273b-684">若要設定進階選項，請在 MVC 產生器上使用擴充方法 `AddRazorPagesOptions`：</span><span class="sxs-lookup"><span data-stu-id="e273b-684">To configure advanced options, use the extension method `AddRazorPagesOptions` on the MVC builder:</span></span>

[!code-cs[](index/sample/RazorPagesContacts/StartupAdvanced.cs?name=snippet_1)]

<span data-ttu-id="e273b-685">目前可以使用 `RazorPagesOptions` 設定頁面的根目錄，或新增頁面的應用程式模型慣例。</span><span class="sxs-lookup"><span data-stu-id="e273b-685">Currently you can use the `RazorPagesOptions` to set the root directory for pages, or add application model conventions for pages.</span></span> <span data-ttu-id="e273b-686">我們將在未來以這種方式獲得更多的擴充性。</span><span class="sxs-lookup"><span data-stu-id="e273b-686">We'll enable more extensibility this way in the future.</span></span>

<span data-ttu-id="e273b-687">若要先行編譯視圖，請參閱[ Razor view 編譯](xref:mvc/views/view-compilation)。</span><span class="sxs-lookup"><span data-stu-id="e273b-687">To precompile views, see [Razor view compilation](xref:mvc/views/view-compilation) .</span></span>

<span data-ttu-id="e273b-688">[下載或檢視範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample)。</span><span class="sxs-lookup"><span data-stu-id="e273b-688">[Download or view sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/razor-pages/index/sample).</span></span>

<span data-ttu-id="e273b-689">請參閱[開始使用 Razor 頁面](xref:tutorials/razor-pages/razor-pages-start)，這會在此簡介中建立。</span><span class="sxs-lookup"><span data-stu-id="e273b-689">See [Get started with Razor Pages](xref:tutorials/razor-pages/razor-pages-start), which builds on this introduction.</span></span>

### <a name="specify-that-razor-pages-are-at-the-content-root"></a><span data-ttu-id="e273b-690">指定 Razor 頁面位於內容根目錄</span><span class="sxs-lookup"><span data-stu-id="e273b-690">Specify that Razor Pages are at the content root</span></span>

<span data-ttu-id="e273b-691">根據預設， Razor 頁面會以根方式在 */Pages*目錄中。</span><span class="sxs-lookup"><span data-stu-id="e273b-691">By default, Razor Pages are rooted in the */Pages* directory.</span></span> <span data-ttu-id="e273b-692">將[WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot)新增至[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) ，以指定您的 Razor 頁面位於應用程式的[內容根目錄](xref:fundamentals/index#content-root)（[ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)）：</span><span class="sxs-lookup"><span data-stu-id="e273b-692">Add [WithRazorPagesAtContentRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvcbuilderextensions.withrazorpagesatcontentroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at the [content root](xref:fundamentals/index#content-root) ([ContentRootPath](/dotnet/api/microsoft.aspnetcore.hosting.ihostingenvironment.contentrootpath)) of the app:</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesAtContentRoot();
```

### <a name="specify-that-razor-pages-are-at-a-custom-root-directory"></a><span data-ttu-id="e273b-693">指定 Razor 頁面位於自訂根目錄</span><span class="sxs-lookup"><span data-stu-id="e273b-693">Specify that Razor Pages are at a custom root directory</span></span>

<span data-ttu-id="e273b-694">將[withrazorpagesroot 新增](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot)新增至[AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) ，以指定您的 Razor 頁面位於應用程式中的自訂根目錄（提供相對路徑）：</span><span class="sxs-lookup"><span data-stu-id="e273b-694">Add [WithRazorPagesRoot](/dotnet/api/microsoft.extensions.dependencyinjection.mvcrazorpagesmvccorebuilderextensions.withrazorpagesroot) to [AddMvc](/dotnet/api/microsoft.extensions.dependencyinjection.mvcservicecollectionextensions.addmvc#Microsoft_Extensions_DependencyInjection_MvcServiceCollectionExtensions_AddMvc_Microsoft_Extensions_DependencyInjection_IServiceCollection_) to specify that your Razor Pages are at a custom root directory in the app (provide a relative path):</span></span>

```csharp
services.AddMvc()
    .AddRazorPagesOptions(options =>
    {
        ...
    })
    .WithRazorPagesRoot("/path/to/razor/pages");
```

## <a name="additional-resources"></a><span data-ttu-id="e273b-695">其他資源</span><span class="sxs-lookup"><span data-stu-id="e273b-695">Additional resources</span></span>

* <span data-ttu-id="e273b-696">[授權屬性和 Razor 頁面](xref:security/authorization/simple#aarp)</span><span class="sxs-lookup"><span data-stu-id="e273b-696">[Authorize attribute and Razor Pages](xref:security/authorization/simple#aarp)</span></span>
* <xref:index>
* <xref:mvc/views/razor>
* <xref:mvc/controllers/areas>
* <xref:tutorials/razor-pages/razor-pages-start>
* <xref:security/authorization/razor-pages-authorization>
* <xref:razor-pages/razor-pages-conventions>
* <xref:test/razor-pages-tests>
* <xref:mvc/views/partial>

::: moniker-end
