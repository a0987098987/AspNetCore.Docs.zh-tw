---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: 什麼是 ASP.NET 4.5 和 Visual Studio 2012 的新功能 |Microsoft Docs
author: rick-anderson
description: 本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 此外，本文也將說明進行網頁程式開發的增強功能...
ms.author: riande
ms.date: 02/29/2012
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 6bbfb4aa7f29e4c189da4dfdca6f2113c7550b68
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41831147"
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a><span data-ttu-id="ea72c-104">在 ASP.NET 4.5 和 Visual Studio 2012 中最新消息</span><span class="sxs-lookup"><span data-stu-id="ea72c-104">What's New in ASP.NET 4.5 and Visual Studio 2012</span></span>
====================
> <span data-ttu-id="ea72c-105">本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-105">This document describes new features and enhancements that are being introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="ea72c-106">它也會說明適用於 Visual Studio 2012 中的 web 開發所做的改進。</span><span class="sxs-lookup"><span data-stu-id="ea72c-106">It also describes improvements being made for web development in Visual Studio 2012.</span></span> <span data-ttu-id="ea72c-107">這份文件最初發行於 2012 年 2 月 29 日。</span><span class="sxs-lookup"><span data-stu-id="ea72c-107">This document was originally published on February 29, 2012.</span></span>


- [<span data-ttu-id="ea72c-108">ASP.NET Core 執行階段和架構</span><span class="sxs-lookup"><span data-stu-id="ea72c-108">ASP.NET Core Runtime and Framework</span></span>](#_Toc318097372)

    - [<span data-ttu-id="ea72c-109">以非同步方式讀取和寫入 HTTP 要求和回應</span><span class="sxs-lookup"><span data-stu-id="ea72c-109">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>](#_Toc318097373)
    - [<span data-ttu-id="ea72c-110">HttpRequest 處理的增強功能</span><span class="sxs-lookup"><span data-stu-id="ea72c-110">Improvements to HttpRequest handling</span></span>](#_Toc318097374)
    - [<span data-ttu-id="ea72c-111">以非同步方式排清回應</span><span class="sxs-lookup"><span data-stu-id="ea72c-111">Asynchronously flushing a response</span></span>](#_Toc318097375)
    - [<span data-ttu-id="ea72c-112">支援*await*並*工作*-架構非同步模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-112">Support for *await* and *Task*-Based Asynchronous Modules and Handlers</span></span>](#_Toc318097376)
    - [<span data-ttu-id="ea72c-113">非同步 HTTP 模組</span><span class="sxs-lookup"><span data-stu-id="ea72c-113">Asynchronous HTTP modules</span></span>](#_Toc318097377)
    - [<span data-ttu-id="ea72c-114">非同步 HTTP 處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-114">Asynchronous HTTP handlers</span></span>](#_Toc318097378)
    - [<span data-ttu-id="ea72c-115">新的 ASP.NET 要求驗證功能</span><span class="sxs-lookup"><span data-stu-id="ea72c-115">New ASP.NET Request Validation Features</span></span>](#_Toc318097379)
    - [<span data-ttu-id="ea72c-116">延後 （「 延遲 」） 的要求驗證</span><span class="sxs-lookup"><span data-stu-id="ea72c-116">Deferred ("lazy") request validation</span></span>](#_Toc318097380)
    - [<span data-ttu-id="ea72c-117">支援未經驗證的要求</span><span class="sxs-lookup"><span data-stu-id="ea72c-117">Support for unvalidated requests</span></span>](#_Toc318097381)
    - [<span data-ttu-id="ea72c-118">AntiXSS 程式庫</span><span class="sxs-lookup"><span data-stu-id="ea72c-118">AntiXSS Library</span></span>](#_Toc318097382)
    - [<span data-ttu-id="ea72c-119">支援 WebSockets 通訊協定</span><span class="sxs-lookup"><span data-stu-id="ea72c-119">Support for WebSockets Protocol</span></span>](#_Toc318097383)
    - [<span data-ttu-id="ea72c-120">統合和縮製</span><span class="sxs-lookup"><span data-stu-id="ea72c-120">Bundling and Minification</span></span>](#_Toc318097384)
    - [<span data-ttu-id="ea72c-121">適用於虛擬主機的效能改進</span><span class="sxs-lookup"><span data-stu-id="ea72c-121">Performance Improvements for Web Hosting</span></span>](#_Toc_perf)

        - [<span data-ttu-id="ea72c-122">關鍵效能因素</span><span class="sxs-lookup"><span data-stu-id="ea72c-122">Key Performance Factors</span></span>](#_Toc_perf_1)
        - [<span data-ttu-id="ea72c-123">如需新效能功能的需求</span><span class="sxs-lookup"><span data-stu-id="ea72c-123">Requirements for New Performance Features</span></span>](#_Toc_perf_2)
        - [<span data-ttu-id="ea72c-124">共用通用的組件</span><span class="sxs-lookup"><span data-stu-id="ea72c-124">Sharing Common Assemblies</span></span>](#_Toc_perf_3)
        - [<span data-ttu-id="ea72c-125">使用多核心 JIT 編譯的啟動速度更快</span><span class="sxs-lookup"><span data-stu-id="ea72c-125">Using multi-Core JIT compilation for faster startup</span></span>](#_Toc_perf_4)
        - [<span data-ttu-id="ea72c-126">調整記憶體回收的記憶體最佳化</span><span class="sxs-lookup"><span data-stu-id="ea72c-126">Tuning garbage collection to optimize for memory</span></span>](#_Toc_perf_5)
        - [<span data-ttu-id="ea72c-127">預先擷取 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ea72c-127">Prefetching for web applications</span></span>](#_Toc_perf_6)
- [<span data-ttu-id="ea72c-128">ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="ea72c-128">ASP.NET Web Forms</span></span>](#_Toc318097385)

    - [<span data-ttu-id="ea72c-129">強型別資料控制項</span><span class="sxs-lookup"><span data-stu-id="ea72c-129">Strongly Typed Data Controls</span></span>](#_Toc318097386)
    - [<span data-ttu-id="ea72c-130">模型繫結</span><span class="sxs-lookup"><span data-stu-id="ea72c-130">Model Binding</span></span>](#_Toc318097387)

        - [<span data-ttu-id="ea72c-131">選取資料</span><span class="sxs-lookup"><span data-stu-id="ea72c-131">Selecting data</span></span>](#_Toc318097388)
        - [<span data-ttu-id="ea72c-132">值提供者</span><span class="sxs-lookup"><span data-stu-id="ea72c-132">Value providers</span></span>](#_Toc318097389)
        - [<span data-ttu-id="ea72c-133">依控制項的值進行篩選</span><span class="sxs-lookup"><span data-stu-id="ea72c-133">Filtering by values from a control</span></span>](#_Toc318097390)
    - [<span data-ttu-id="ea72c-134">HTML 編碼的資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="ea72c-134">HTML Encoded Data-Binding Expressions</span></span>](#_Toc318097391)
    - [<span data-ttu-id="ea72c-135">低調驗證</span><span class="sxs-lookup"><span data-stu-id="ea72c-135">Unobtrusive Validation</span></span>](#_Toc318097392)
    - [<span data-ttu-id="ea72c-136">HTML5 更新</span><span class="sxs-lookup"><span data-stu-id="ea72c-136">HTML5 Updates</span></span>](#_Toc318097393)
- [<span data-ttu-id="ea72c-137">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea72c-137">ASP.NET MVC 4</span></span>](#_Toc318097394)
- [<span data-ttu-id="ea72c-138">ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="ea72c-138">ASP.NET Web Pages 2</span></span>](#_Toc318097395)
- [<span data-ttu-id="ea72c-139">Visual Studio 2012 Release Candidate</span><span class="sxs-lookup"><span data-stu-id="ea72c-139">Visual Studio 2012 Release Candidate</span></span>](#_Toc318097396)

    - [<span data-ttu-id="ea72c-140">Visual Studio 2010 和 Visual Studio 2012 Release Candidate （專案相容性） 之間共用的專案</span><span class="sxs-lookup"><span data-stu-id="ea72c-140">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>](#project-compatibility)
    - [<span data-ttu-id="ea72c-141">ASP.NET 4.5 網站範本中的組態變更</span><span class="sxs-lookup"><span data-stu-id="ea72c-141">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [<span data-ttu-id="ea72c-142">在 IIS 7 ASP.NET 路由中的原生支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-142">Native Support in IIS 7 for ASP.NET Routing</span></span>](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [<span data-ttu-id="ea72c-143">HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-143">HTML Editor</span></span>](#_Toc318097397)

        - [<span data-ttu-id="ea72c-144">智慧工作</span><span class="sxs-lookup"><span data-stu-id="ea72c-144">Smart Tasks</span></span>](#_Toc318097398)
        - [<span data-ttu-id="ea72c-145">等待 ARIA 支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-145">WAI-ARIA support</span></span>](#_Toc318097399)
        - [<span data-ttu-id="ea72c-146">新的 HTML5 片段</span><span class="sxs-lookup"><span data-stu-id="ea72c-146">New HTML5 snippets</span></span>](#_Toc318097400)
        - [<span data-ttu-id="ea72c-147">擷取至使用者控制項</span><span class="sxs-lookup"><span data-stu-id="ea72c-147">Extract to user control</span></span>](#_Toc318097401)
        - [<span data-ttu-id="ea72c-148">在屬性中的程式碼區塊的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ea72c-148">IntelliSense for code nuggets in attributes</span></span>](#_Toc318097402)
        - [<span data-ttu-id="ea72c-149">當您重新命名開頭或結尾標記相符的標記的自動重新命名</span><span class="sxs-lookup"><span data-stu-id="ea72c-149">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>](#_Toc318097403)
        - [<span data-ttu-id="ea72c-150">產生事件處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-150">Event handler generation</span></span>](#_Toc318097404)
        - [<span data-ttu-id="ea72c-151">智慧縮排</span><span class="sxs-lookup"><span data-stu-id="ea72c-151">Smart indent</span></span>](#_Toc318097405)
        - [<span data-ttu-id="ea72c-152">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="ea72c-152">Auto-reduce statement completion</span></span>](#_Toc318097406)
    - [<span data-ttu-id="ea72c-153">JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-153">JavaScript Editor</span></span>](#_Toc318097407)

        - [<span data-ttu-id="ea72c-154">程式碼大綱</span><span class="sxs-lookup"><span data-stu-id="ea72c-154">Code outlining</span></span>](#_Toc318097408)
        - [<span data-ttu-id="ea72c-155">括號對稱</span><span class="sxs-lookup"><span data-stu-id="ea72c-155">Brace matching</span></span>](#_Toc318097409)
        - [<span data-ttu-id="ea72c-156">移至定義</span><span class="sxs-lookup"><span data-stu-id="ea72c-156">Go to Definition</span></span>](#_Toc318097410)
        - [<span data-ttu-id="ea72c-157">支援 ECMAScript5</span><span class="sxs-lookup"><span data-stu-id="ea72c-157">ECMAScript5 support</span></span>](#_Toc318097411)
        - [<span data-ttu-id="ea72c-158">DOM 的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ea72c-158">DOM IntelliSense</span></span>](#_Toc318097412)
        - [<span data-ttu-id="ea72c-159">VSDOC 簽章的多載</span><span class="sxs-lookup"><span data-stu-id="ea72c-159">VSDOC signature overloads</span></span>](#_Toc318097413)
        - [<span data-ttu-id="ea72c-160">隱含的參考</span><span class="sxs-lookup"><span data-stu-id="ea72c-160">Implicit references</span></span>](#_Toc318097414)
    - [<span data-ttu-id="ea72c-161">CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-161">CSS Editor</span></span>](#_Toc318097415)

        - [<span data-ttu-id="ea72c-162">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="ea72c-162">Auto-reduce statement completion</span></span>](#_Toc318097416)
        - [<span data-ttu-id="ea72c-163">階層式縮排。</span><span class="sxs-lookup"><span data-stu-id="ea72c-163">Hierarchical indentation.</span></span>](#_Toc318097417)
        - [<span data-ttu-id="ea72c-164">CSS 區隔設計支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-164">CSS hacks support</span></span>](#_Toc318097418)
        - [<span data-ttu-id="ea72c-165">廠商特定的結構描述 (-moz--，webkit)</span><span class="sxs-lookup"><span data-stu-id="ea72c-165">Vendor specific schemas (-moz-,-webkit)</span></span>](#_Toc318097419)
        - [<span data-ttu-id="ea72c-166">註解和取消註解的支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-166">Commenting and uncommenting support</span></span>](#_Toc318097420)
        - [<span data-ttu-id="ea72c-167">色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="ea72c-167">Color picker</span></span>](#_Toc318097421)
        - [<span data-ttu-id="ea72c-168">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="ea72c-168">Snippets</span></span>](#_Toc318097422)
        - [<span data-ttu-id="ea72c-169">自訂區域</span><span class="sxs-lookup"><span data-stu-id="ea72c-169">Custom regions</span></span>](#_Toc318097423)
    - [<span data-ttu-id="ea72c-170">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="ea72c-170">Page Inspector</span></span>](#_Toc318097424)
    - [<span data-ttu-id="ea72c-171">發行</span><span class="sxs-lookup"><span data-stu-id="ea72c-171">Publishing</span></span>](#_Toc318097425)

        - [<span data-ttu-id="ea72c-172">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="ea72c-172">Publish profiles</span></span>](#_Toc318097426)
        - [<span data-ttu-id="ea72c-173">ASP.NET 先行編譯和合併</span><span class="sxs-lookup"><span data-stu-id="ea72c-173">ASP.NET precompilation and merge</span></span>](#_Toc318097427)
- [<span data-ttu-id="ea72c-174">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ea72c-174">IIS Express</span></span>](#_Toc318097428)
- [<span data-ttu-id="ea72c-175">免責聲明</span><span class="sxs-lookup"><span data-stu-id="ea72c-175">Disclaimer</span></span>](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a><span data-ttu-id="ea72c-176">ASP.NET Core 執行階段和架構</span><span class="sxs-lookup"><span data-stu-id="ea72c-176">ASP.NET Core Runtime and Framework</span></span>

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a><span data-ttu-id="ea72c-177">以非同步方式讀取和寫入 HTTP 要求和回應</span><span class="sxs-lookup"><span data-stu-id="ea72c-177">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>

<span data-ttu-id="ea72c-178">ASP.NET 4 引進了能夠讀取 HTTP 要求實體做為資料流，使用*HttpRequest.GetBufferlessInputStream*方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-178">ASP.NET 4 introduced the ability to read an HTTP request entity as a stream using the *HttpRequest.GetBufferlessInputStream* method.</span></span> <span data-ttu-id="ea72c-179">這個方法會提供資料流存取要求的實體。</span><span class="sxs-lookup"><span data-stu-id="ea72c-179">This method provided streaming access to the request entity.</span></span> <span data-ttu-id="ea72c-180">不過，它以同步方式執行，其中繫結要求的持續期間的執行緒。</span><span class="sxs-lookup"><span data-stu-id="ea72c-180">However, it executed synchronously, which tied up a thread for the duration of a request.</span></span>

<span data-ttu-id="ea72c-181">ASP.NET 4.5 支援能夠讀取資料流上 HTTP 要求實體中，以非同步方式，以及能夠以非同步方式排清。</span><span class="sxs-lookup"><span data-stu-id="ea72c-181">ASP.NET 4.5 supports the ability to read streams asynchronously on an HTTP request entity, and the ability to flush asynchronously.</span></span> <span data-ttu-id="ea72c-182">ASP.NET 4.5 也提供雙重緩衝 HTTP 要求實體，可提供您更輕鬆整合使用下游的 HTTP 處理常式，例如.aspx 頁面處理常式和 ASP.NET MVC 控制站的能力。</span><span class="sxs-lookup"><span data-stu-id="ea72c-182">ASP.NET 4.5 also gives you the ability to double-buffer an HTTP request entity, which provides easier integration with downstream HTTP handlers such as .aspx page handlers and ASP.NET MVC controllers.</span></span>

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a><span data-ttu-id="ea72c-183">HttpRequest 處理的增強功能</span><span class="sxs-lookup"><span data-stu-id="ea72c-183">Improvements to HttpRequest handling</span></span>

<span data-ttu-id="ea72c-184">從 ASP.NET 4.5 所傳回的 Stream 參考*HttpRequest.GetBufferlessInputStream*支援同步和非同步讀取的方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-184">The Stream reference returned by ASP.NET 4.5 from *HttpRequest.GetBufferlessInputStream* supports both synchronous and asynchronous read methods.</span></span> <span data-ttu-id="ea72c-185">*Stream*從傳回的物件*GetBufferlessInputStream*現在實作將 BeginRead 與 EndRead 方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-185">The *Stream* object returned from *GetBufferlessInputStream* now implements both the BeginRead and EndRead methods.</span></span> <span data-ttu-id="ea72c-186">非同步*Stream*方法可讓您以非同步方式讀取要求實體中的區塊，而 ASP.NET 會釋放目前執行緒的非同步讀取迴圈的每個反覆項目之間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-186">The asynchronous *Stream* methods let you asynchronously read the request entity in chunks, while ASP.NET releases the current thread between each iteration of an asynchronous read loop.</span></span>

<span data-ttu-id="ea72c-187">ASP.NET 4.5 也已新增方法以經過緩衝處理的方式讀取要求的實體： *HttpRequest.GetBufferedInputStream*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-187">ASP.NET 4.5 has also added a companion method for reading the request entity in a buffered way: *HttpRequest.GetBufferedInputStream*.</span></span> <span data-ttu-id="ea72c-188">這個新的多載的運作方式類似*GetBufferlessInputStream*，支援同步和非同步讀取。</span><span class="sxs-lookup"><span data-stu-id="ea72c-188">This new overload works like *GetBufferlessInputStream*, supporting both synchronous and asynchronous reads.</span></span> <span data-ttu-id="ea72c-189">不過，它會讀取，如*GetBufferedInputStream*也將實體位元組複製到 ASP.NET 內部緩衝區，以便下游的模組和處理常式仍然可以存取要求的實體。</span><span class="sxs-lookup"><span data-stu-id="ea72c-189">However, as it reads, *GetBufferedInputStream* also copies the entity bytes into ASP.NET internal buffers so that downstream modules and handlers can still access the request entity.</span></span> <span data-ttu-id="ea72c-190">例如，如果某些上游管線中的程式碼已讀取要求實體使用*GetBufferedInputStream*，您仍然可以使用*HttpRequest.Form*或*HttpRequest.Files*.</span><span class="sxs-lookup"><span data-stu-id="ea72c-190">For example, if some upstream code in the pipeline has already read the request entity using *GetBufferedInputStream*, you can still use *HttpRequest.Form* or *HttpRequest.Files*.</span></span> <span data-ttu-id="ea72c-191">這可讓您執行的非同步處理的要求 （例如，資料流處理大型檔案上的傳至資料庫），但仍執行的.aspx 頁面和 ASP.NET MVC 控制站之後。</span><span class="sxs-lookup"><span data-stu-id="ea72c-191">This lets you perform asynchronous processing on a request (for example, streaming a large file upload to a database), but still run .aspx pages and MVC ASP.NET controllers afterward.</span></span>

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a><span data-ttu-id="ea72c-192">以非同步方式排清回應</span><span class="sxs-lookup"><span data-stu-id="ea72c-192">Asynchronously flushing a response</span></span>

<span data-ttu-id="ea72c-193">傳送至 HTTP 用戶端的回應，可能需要相當長的時間，當用戶端位於遠處或低頻寬連線較。</span><span class="sxs-lookup"><span data-stu-id="ea72c-193">Sending responses to an HTTP client can take considerable time when the client is far away or has a low-bandwidth connection.</span></span> <span data-ttu-id="ea72c-194">在建立應用程式中，通常 ASP.NET 會緩衝處理回應位元組。</span><span class="sxs-lookup"><span data-stu-id="ea72c-194">Normally ASP.NET buffers the response bytes as they are created by an application.</span></span> <span data-ttu-id="ea72c-195">然後，ASP.NET 會執行單一傳送作業的每一端的要求處理應計的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="ea72c-195">ASP.NET then performs a single send operation of the accrued buffers at the very end of request processing.</span></span>

<span data-ttu-id="ea72c-196">如果已緩衝的回應很大 （例如，串流處理到用戶端的大型檔案） 的必須定期呼叫*HttpResponse.Flush*緩衝的輸出傳送至用戶端，而且將保留在控制下的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="ea72c-196">If the buffered response is large (for example, streaming a large file to a client), you must periodically call *HttpResponse.Flush* to send buffered output to the client and keep memory usage under control.</span></span> <span data-ttu-id="ea72c-197">不過，因為*排清*是同步的呼叫，並反覆地呼叫*排清*仍會在執行緒耗用可能長時間執行要求的持續時間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-197">However, because *Flush* is a synchronous call, iteratively calling *Flush* still consumes a thread for the duration of potentially long-running requests.</span></span>

<span data-ttu-id="ea72c-198">ASP.NET 4.5 新增了對支援使用以非同步方式執行排清*BeginFlush*並*EndFlush*方法*HttpResponse*類別。</span><span class="sxs-lookup"><span data-stu-id="ea72c-198">ASP.NET 4.5 adds support for performing flushes asynchronously using the *BeginFlush* and *EndFlush* methods of the *HttpResponse* class.</span></span> <span data-ttu-id="ea72c-199">您可以使用這些方法，建立非同步模組和非同步處理常式，但不會佔用作業系統執行緒，以累加方式將資料傳送至用戶端。</span><span class="sxs-lookup"><span data-stu-id="ea72c-199">Using these methods, you can create asynchronous modules and asynchronous handlers that incrementally send data to a client without tying up operating-system threads.</span></span> <span data-ttu-id="ea72c-200">在中間*BeginFlush*並*EndFlush*呼叫，ASP.NET 會釋放目前的執行緒。</span><span class="sxs-lookup"><span data-stu-id="ea72c-200">In between *BeginFlush* and *EndFlush* calls, ASP.NET releases the current thread.</span></span> <span data-ttu-id="ea72c-201">這可以大幅減少以支援長時間執行 HTTP 下載所需的作用中執行緒的總數。</span><span class="sxs-lookup"><span data-stu-id="ea72c-201">This substantially reduces the total number of active threads that are needed in order to support long-running HTTP downloads.</span></span>

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a><span data-ttu-id="ea72c-202">支援*await*並*工作*-架構非同步模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-202">Support for *await* and *Task* - Based Asynchronous Modules and Handlers</span></span>

<span data-ttu-id="ea72c-203">.NET Framework 4 引進稱為 「 非同步程式設計概念*任務*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-203">The .NET Framework 4 introduced an asynchronous programming concept referred to as a *task*.</span></span> <span data-ttu-id="ea72c-204">工作由*任務*型別和中的相關型別*System.Threading.Tasks*命名空間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-204">Tasks are represented by the *Task* type and related types in the *System.Threading.Tasks* namespace.</span></span> <span data-ttu-id="ea72c-205">.NET Framework 4.5 中建立這項以便於使用的編譯器強化*任務*簡單的物件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-205">The .NET Framework 4.5 builds on this with compiler enhancements that make working with *Task* objects simple.</span></span> <span data-ttu-id="ea72c-206">在.NET Framework 4.5 中，編譯器都支援兩個新的關鍵字： *await*並*非同步*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-206">In the .NET Framework 4.5, the compilers support two new keywords: *await* and *async*.</span></span> <span data-ttu-id="ea72c-207">*Await*關鍵字是速記語法來表示，某段程式碼應該以非同步方式等候一段程式碼。</span><span class="sxs-lookup"><span data-stu-id="ea72c-207">The *await* keyword is syntactical shorthand for indicating that a piece of code should asynchronously wait on some other piece of code.</span></span> <span data-ttu-id="ea72c-208">*非同步*關鍵字各表示可用來將方法標記為以工作為基礎的非同步方法的提示。</span><span class="sxs-lookup"><span data-stu-id="ea72c-208">The *async* keyword represents a hint that you can use to mark methods as task-based asynchronous methods.</span></span>

<span data-ttu-id="ea72c-209">組合*await*，*非同步*，而*工作*物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。</span><span class="sxs-lookup"><span data-stu-id="ea72c-209">The combination of *await*, *async*, and the *Task* object makes it much easier for you to write asynchronous code in .NET 4.5.</span></span> <span data-ttu-id="ea72c-210">ASP.NET 4.5 支援這些新的 Api，可讓您撰寫非同步 HTTP 模組，並使用新的編譯器強化的非同步 HTTP 處理常式與簡單化。</span><span class="sxs-lookup"><span data-stu-id="ea72c-210">ASP.NET 4.5 supports these simplifications with new APIs that let you write asynchronous HTTP modules and asynchronous HTTP handlers using the new compiler enhancements.</span></span>

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a><span data-ttu-id="ea72c-211">非同步 HTTP 模組</span><span class="sxs-lookup"><span data-stu-id="ea72c-211">Asynchronous HTTP modules</span></span>

<span data-ttu-id="ea72c-212">假設您想要執行非同步工作傳回方法內*任務*物件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-212">Suppose that you want to perform asynchronous work within a method that returns a *Task* object.</span></span> <span data-ttu-id="ea72c-213">下列程式碼範例會定義非同步方法，可讓下載 Microsoft 首頁上的非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="ea72c-213">The following code example defines an asynchronous method that makes an asynchronous call to download the Microsoft home page.</span></span> <span data-ttu-id="ea72c-214">請注意，使用*非同步*方法簽章中的關鍵字而*await*呼叫*DownloadStringTaskAsync*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-214">Notice the use of the *async* keyword in the method signature and the *await* call to *DownloadStringTaskAsync*.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

<span data-ttu-id="ea72c-215">這就是您必須撰寫 —.NET Framework 會自動處理時等待，才能完成下載，以及在下載完成後，自動還原在呼叫堆疊回溯呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="ea72c-215">That's all you have to write — the .NET Framework will automatically handle unwinding the call stack while waiting for the download to complete, as well as automatically restoring the call stack after the download is done.</span></span>

<span data-ttu-id="ea72c-216">現在，假設您想要使用非同步 ASP.NET HTTP 模組中的這個非同步方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-216">Now suppose that you want to use this asynchronous method in an asynchronous ASP.NET HTTP module.</span></span> <span data-ttu-id="ea72c-217">ASP.NET 4.5 包含協助程式方法 (*EventHandlerTaskAsyncHelper*) 和新的委派型別 (*TaskEventHandler*)，您可以使用與舊版整合以工作為基礎的非同步方法ASP.NET HTTP 管線所公開的非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="ea72c-217">ASP.NET 4.5 includes a helper method (*EventHandlerTaskAsyncHelper*) and a new delegate type (*TaskEventHandler*) that you can use to integrate task-based asynchronous methods with the older asynchronous programming model exposed by the ASP.NET HTTP pipeline.</span></span> <span data-ttu-id="ea72c-218">此範例示範如何：</span><span class="sxs-lookup"><span data-stu-id="ea72c-218">This example shows how:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a><span data-ttu-id="ea72c-219">非同步 HTTP 處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-219">Asynchronous HTTP handlers</span></span>

<span data-ttu-id="ea72c-220">在 ASP.NET 中撰寫非同步處理常式的傳統方法是實作*IHttpAsyncHandler*介面。</span><span class="sxs-lookup"><span data-stu-id="ea72c-220">The traditional approach to writing asynchronous handlers in ASP.NET is to implement the *IHttpAsyncHandler* interface.</span></span> <span data-ttu-id="ea72c-221">ASP.NET 4.5 引入*HttpTaskAsyncHandler*非同步基底型別，您可以從中衍生且可更輕鬆地撰寫非同步處理常式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-221">ASP.NET 4.5 introduces the *HttpTaskAsyncHandler* asynchronous base type that you can derive from, which makes it much easier to write asynchronous handlers.</span></span>

<span data-ttu-id="ea72c-222">*HttpTaskAsyncHandler*型別是抽象的而且會要求您覆寫*ProcessRequestAsync*方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-222">The *HttpTaskAsyncHandler* type is abstract and requires you to override the *ProcessRequestAsync* method.</span></span> <span data-ttu-id="ea72c-223">ASP.NET 在內部會負責將傳回的簽章的整合 (*任務*物件) 的*ProcessRequestAsync*與較舊的非同步程式設計模型使用 ASP.NET 管線。</span><span class="sxs-lookup"><span data-stu-id="ea72c-223">Internally ASP.NET takes care of integrating the return signature (a *Task* object) of *ProcessRequestAsync* with the older asynchronous programming model used by the ASP.NET pipeline.</span></span>

<span data-ttu-id="ea72c-224">下列範例示範如何使用*任務*並*await*非同步 HTTP 處理常式實作的一部分：</span><span class="sxs-lookup"><span data-stu-id="ea72c-224">The following example shows how you can use *Task* and *await* as part of the implementation of an asynchronous HTTP handler:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a><span data-ttu-id="ea72c-225">新的 ASP.NET 要求驗證功能</span><span class="sxs-lookup"><span data-stu-id="ea72c-225">New ASP.NET Request Validation Features</span></span>

<span data-ttu-id="ea72c-226">根據預設，ASP.NET 會執行要求驗證，它會檢查要求，若要尋找的標記或指令碼中的欄位、 標頭、 cookie 和等等。</span><span class="sxs-lookup"><span data-stu-id="ea72c-226">By default, ASP.NET performs request validation — it examines requests to look for markup or script in fields, headers, cookies, and so on.</span></span> <span data-ttu-id="ea72c-227">如果偵測到任何時，ASP.NET 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ea72c-227">If any is detected, ASP.NET throws an exception.</span></span> <span data-ttu-id="ea72c-228">這可以做為第一道防禦潛在的跨網站指令碼的攻擊。</span><span class="sxs-lookup"><span data-stu-id="ea72c-228">This acts as a first line of defense against potential cross-site scripting attacks.</span></span>

<span data-ttu-id="ea72c-229">ASP.NET 4.5 可讓您更容易選擇性讀取未經驗證的要求資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-229">ASP.NET 4.5 makes it easy to selectively read unvalidated request data.</span></span> <span data-ttu-id="ea72c-230">ASP.NET 4.5 也整合了熱門 AntiXSS 程式庫，原來的外部程式庫。</span><span class="sxs-lookup"><span data-stu-id="ea72c-230">ASP.NET 4.5 also integrates the popular AntiXSS library, which was formerly an external library.</span></span>

<span data-ttu-id="ea72c-231">開發人員必須能夠選擇性地關閉他們的應用程式的要求驗證的常見問題集。</span><span class="sxs-lookup"><span data-stu-id="ea72c-231">Developers have frequently asked for the ability to selectively turn off request validation for their applications.</span></span> <span data-ttu-id="ea72c-232">比方說，如果您的應用程式論壇軟體，您可能想要允許使用者提交 HTML 格式的論壇文章和註解，但是仍然確定要求驗證會檢查所有其他項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-232">For example, if your application is forum software, you might want to allow users to submit HTML-formatted forum posts and comments, but still make sure that request validation is checking everything else.</span></span>

<span data-ttu-id="ea72c-233">ASP.NET 4.5 引進了兩項功能可簡化您選擇性地使用未經驗證的輸入： 延後 （「 延遲 」） 的要求驗證和未經驗證的要求資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="ea72c-233">ASP.NET 4.5 introduces two features that make it easy for you to selectively work with unvalidated input: deferred ("lazy") request validation and access to unvalidated request data.</span></span>

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a><span data-ttu-id="ea72c-234">延後 （「 延遲 」） 的要求驗證</span><span class="sxs-lookup"><span data-stu-id="ea72c-234">Deferred ("lazy") request validation</span></span>

<span data-ttu-id="ea72c-235">在 ASP.NET 4.5 中，依預設所有要求資料受都限於要求驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-235">In ASP.NET 4.5, by default all request data is subject to request validation.</span></span> <span data-ttu-id="ea72c-236">不過，您可以設定要延後要求驗證，直到您實際存取要求資料的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-236">However, you can configure the application to defer request validation until you actually access request data.</span></span> <span data-ttu-id="ea72c-237">（這有時候稱為延遲要求驗證，根據等詞彙消極式載入特定的資料案例。）您可以設定應用程式的 Web.config 檔案中使用延後的驗證，藉由設定*requestValidationMode*屬性中的 4.5 *httpRUntime*項目，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-237">(This is sometimes referred to as lazy request validation, based on terms like lazy loading for certain data scenarios.) You can configure the application to use deferred validation in the Web.config file by setting the *requestValidationMode* attribute to 4.5 in the *httpRUntime* element, as in the following example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

<span data-ttu-id="ea72c-238">當要求的驗證模式設定為 4.5 時，只會針對特定要求的值，且只有在您的程式碼會存取該值時，才將會觸發要求驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-238">When request validation mode is set to 4.5, request validation is triggered only for a specific request value and only when your code accesses that value.</span></span> <span data-ttu-id="ea72c-239">例如，如果您的程式碼取得的值 Request.Form["forum\_張貼"]，只會針對該表單集合中的項目叫用要求驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-239">For example, if your code gets the value of Request.Form["forum\_post"], request validation is invoked only for that element in the form collection.</span></span> <span data-ttu-id="ea72c-240">沒有任何其他元素在*表單*集合會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-240">None of the other elements in the *Form* collection are validated.</span></span> <span data-ttu-id="ea72c-241">在舊版 ASP.NET 中，已在整個要求集合觸發要求驗證時存取集合中的任何項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-241">In previous versions of ASP.NET, request validation was triggered for the entire request collection when any element in the collection was accessed.</span></span> <span data-ttu-id="ea72c-242">新的行為更容易查看要求資料的不同片段，而不觸發要求驗證，在其他片段上的不同應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-242">The new behavior makes it easier for different application components to look at different pieces of request data without triggering request validation on other pieces.</span></span>

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a><span data-ttu-id="ea72c-243">支援未經驗證的要求</span><span class="sxs-lookup"><span data-stu-id="ea72c-243">Support for unvalidated requests</span></span>

<span data-ttu-id="ea72c-244">延後的要求驗證單獨做無法解決問題，選擇性地略過要求驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-244">Deferred request validation alone doesn't solve the problem of selectively bypassing request validation.</span></span> <span data-ttu-id="ea72c-245">呼叫 Request.Form["forum\_張貼 「] 觸發程序仍要求驗證，針對該特定的要求值。</span><span class="sxs-lookup"><span data-stu-id="ea72c-245">The call to Request.Form["forum\_post"] still triggers request validation for that specific request value.</span></span> <span data-ttu-id="ea72c-246">不過，您可能要存取這個欄位，而不觸發驗證，因為您想要允許在該欄位中的標記。</span><span class="sxs-lookup"><span data-stu-id="ea72c-246">However, you might want to access this field without triggering validation because you want to allow markup in that field.</span></span>

<span data-ttu-id="ea72c-247">若要允許這種情況，ASP.NET 4.5 現在支援未經驗證的存取要求資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-247">To allow this, ASP.NET 4.5 now supports unvalidated access to request data.</span></span> <span data-ttu-id="ea72c-248">ASP.NET 4.5 包含新*Unvalidated*中的集合屬性*HttpRequest*類別。</span><span class="sxs-lookup"><span data-stu-id="ea72c-248">ASP.NET 4.5 includes a new *Unvalidated* collection property in the *HttpRequest* class.</span></span> <span data-ttu-id="ea72c-249">這個集合提供存取所有常見的值，要求資料，例如*表單*， *QueryString*， *Cookie*，以及*Url*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-249">This collection provides access to all of the common values of request data, like *Form*, *QueryString*, *Cookies*, and *Url*.</span></span>

<span data-ttu-id="ea72c-250">使用論壇範例中，若要能夠讀取未經驗證的要求資料，您必須先設定應用程式，以使用新的要求驗證模式：</span><span class="sxs-lookup"><span data-stu-id="ea72c-250">Using the forum example, to be able to read unvalidated request data, you first need to configure the application to use the new request validation mode:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

<span data-ttu-id="ea72c-251">然後您可以使用*HttpRequest.Unvalidated*讀取未驗證的表單值的屬性：</span><span class="sxs-lookup"><span data-stu-id="ea72c-251">You can then use the *HttpRequest.Unvalidated* property to read the unvalidated form value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> <span data-ttu-id="ea72c-252">安全性-*請小心使用未經驗證的要求資料 ！*</span><span class="sxs-lookup"><span data-stu-id="ea72c-252">Security - *Use unvalidated request data with care!*</span></span> <span data-ttu-id="ea72c-253">ASP.NET 4.5 中已加入的未經驗證的要求屬性和集合，以讓您更輕鬆地存取非常特定的未驗證的要求資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-253">ASP.NET 4.5 added the unvalidated request properties and collections to make it easier for you to access very specific unvalidated request data.</span></span> <span data-ttu-id="ea72c-254">不過，您仍必須執行未經處理的要求資料，以確保危險的文字不會呈現使用者自訂的驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-254">However, you must still perform custom validation on the raw request data to ensure that dangerous text is not rendered to users.</span></span>


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a><span data-ttu-id="ea72c-255">AntiXSS 程式庫</span><span class="sxs-lookup"><span data-stu-id="ea72c-255">AntiXSS Library</span></span>

<span data-ttu-id="ea72c-256">由於 Microsoft AntiXSS 程式庫的熱門程度，ASP.NET 4.5 現在會包含從 4.0 版，該程式庫的核心編碼常式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-256">Due to the popularity of the Microsoft AntiXSS Library, ASP.NET 4.5 now incorporates core encoding routines from version 4.0 of that library.</span></span>

<span data-ttu-id="ea72c-257">編碼常式由實作*AntiXssEncoder*在新的型別*System.Web.Security.AntiXss*命名空間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-257">The encoding routines are implemented by the *AntiXssEncoder* type in the new *System.Web.Security.AntiXss* namespace.</span></span> <span data-ttu-id="ea72c-258">您可以使用*AntiXssEncoder*直接藉由呼叫的任何靜態型別中實作的編碼方法的型別。</span><span class="sxs-lookup"><span data-stu-id="ea72c-258">You can use the *AntiXssEncoder* type directly by calling any of the static encoding methods that are implemented in the type.</span></span> <span data-ttu-id="ea72c-259">不過，使用新的 ANTI-XSS 常式最簡單的方法是設定 ASP.NET 應用程式使用*AntiXssEncoder*預設類別。</span><span class="sxs-lookup"><span data-stu-id="ea72c-259">However, the easiest approach for using the new anti-XSS routines is to configure an ASP.NET application to use the *AntiXssEncoder* class by default.</span></span> <span data-ttu-id="ea72c-260">若要這樣做，請在 Web.config 檔案中加入下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ea72c-260">To do this, add the following attribute to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

<span data-ttu-id="ea72c-261">當*encoderType*屬性設定為使用*AntiXssEncoder*所有型別，輸出的編碼方式在 ASP.NET 中自動使用新的編碼常式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-261">When the *encoderType* attribute is set to use the *AntiXssEncoder* type, all output encoding in ASP.NET automatically uses the new encoding routines.</span></span>

<span data-ttu-id="ea72c-262">以下是已併入 ASP.NET 4.5 的外部 AntiXSS 程式庫部分：</span><span class="sxs-lookup"><span data-stu-id="ea72c-262">These are the portions of the external AntiXSS library that have been incorporated into ASP.NET 4.5:</span></span>

- <span data-ttu-id="ea72c-263">*HtmlEncode*， *HtmlFormUrlEncode*，和*HtmlAttributeEncode*</span><span class="sxs-lookup"><span data-stu-id="ea72c-263">*HtmlEncode*, *HtmlFormUrlEncode*, and *HtmlAttributeEncode*</span></span>
- <span data-ttu-id="ea72c-264">*XmlAttributeEncode*和*XmlEncode*</span><span class="sxs-lookup"><span data-stu-id="ea72c-264">*XmlAttributeEncode* and *XmlEncode*</span></span>
- <span data-ttu-id="ea72c-265">*UrlEncode*並*UrlPathEncode* （新）</span><span class="sxs-lookup"><span data-stu-id="ea72c-265">*UrlEncode* and *UrlPathEncode* (new)</span></span>
- <span data-ttu-id="ea72c-266">*CssEncode*</span><span class="sxs-lookup"><span data-stu-id="ea72c-266">*CssEncode*</span></span>

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a><span data-ttu-id="ea72c-267">支援 WebSockets 通訊協定</span><span class="sxs-lookup"><span data-stu-id="ea72c-267">Support for WebSockets Protocol</span></span>

<span data-ttu-id="ea72c-268">WebSockets 通訊協定會定義如何建立透過 HTTP 的用戶端與伺服器之間的安全、 即時的雙向通訊的標準網路通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-268">WebSockets protocol is a standards-based network protocol that defines how to establish secure, real-time bidirectional communications between a client and a server over HTTP.</span></span> <span data-ttu-id="ea72c-269">Microsoft 已具有 IETF 和 W3C 標準內文，以協助定義通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-269">Microsoft has worked with both the IETF and W3C standards bodies to help define the protocol.</span></span> <span data-ttu-id="ea72c-270">任何用戶端 （不只是瀏覽器），支援 WebSockets 通訊協定與 Microsoft 投入大量資源的用戶端和行動作業系統上支援 WebSockets 通訊協定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-270">The WebSockets protocol is supported by any client (not just browsers), with Microsoft investing substantial resources supporting WebSockets protocol on both client and mobile operating systems.</span></span>

<span data-ttu-id="ea72c-271">WebSockets 通訊協定可讓您更容易建立用戶端與伺服器之間的長時間執行資料傳輸。</span><span class="sxs-lookup"><span data-stu-id="ea72c-271">WebSockets protocol makes it much easier to create long-running data transfers between a client and a server.</span></span> <span data-ttu-id="ea72c-272">比方說，撰寫聊天應用程式就能輕鬆因為您可以建立用戶端與伺服器之間，則為 true 的長時間執行連線。</span><span class="sxs-lookup"><span data-stu-id="ea72c-272">For example, writing a chat application is much easier because you can establish a true long-running connection between a client and a server.</span></span> <span data-ttu-id="ea72c-273">您不必訴諸於因應措施，例如定期輪詢或 HTTP 長時間輪詢來模擬通訊端的行為。</span><span class="sxs-lookup"><span data-stu-id="ea72c-273">You do not have to resort to workarounds like periodic polling or HTTP long-polling to simulate the behavior of a socket.</span></span>

<span data-ttu-id="ea72c-274">ASP.NET 4.5 和 IIS 8 包含低層級的 Websocket 支援，讓 ASP.NET 開發人員使用受管理的 Api 來以非同步方式讀取和寫入 WebSockets 物件上的 字串和二進位資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-274">ASP.NET 4.5 and IIS 8 include low-level WebSockets support, enabling ASP.NET developers to use managed APIs for asynchronously reading and writing both string and binary data on a WebSockets object.</span></span> <span data-ttu-id="ea72c-275">針對 ASP.NET 4.5，沒有新*System.Web.WebSockets*使用 WebSockets 通訊協定包含類型的命名空間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-275">For ASP.NET 4.5, there is a new *System.Web.WebSockets* namespace that contains types for working with WebSockets protocol.</span></span>

<span data-ttu-id="ea72c-276">瀏覽器用戶端建立 Websocket 連線，藉由建立 DOM *WebSocket*中 ASP.NET 應用程式，如下列範例所示的 URL 所指向的物件：</span><span class="sxs-lookup"><span data-stu-id="ea72c-276">A browser client establishes a WebSockets connection by creating a DOM *WebSocket* object that points to a URL in an ASP.NET application, as in the following example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

<span data-ttu-id="ea72c-277">您可以在 ASP.NET 中使用任何類型的模組或處理常式來建立 Websocket 端點。</span><span class="sxs-lookup"><span data-stu-id="ea72c-277">You can create WebSockets endpoints in ASP.NET using any kind of module or handler.</span></span> <span data-ttu-id="ea72c-278">在上述範例中，已使用.ashx 檔案，因為.ashx 檔案快速建立處理常式所示。</span><span class="sxs-lookup"><span data-stu-id="ea72c-278">In the previous example, an .ashx file was used, because .ashx files are a quick way to create a handler.</span></span>

<span data-ttu-id="ea72c-279">根據 Websocket 通訊協定，ASP.NET 應用程式會藉由指示要求應該升級從 HTTP GET 要求，為 Websocket 要求中接受用戶端的 Websocket 要求。</span><span class="sxs-lookup"><span data-stu-id="ea72c-279">According to the WebSockets protocol, an ASP.NET application accepts a client's WebSockets request by indicating that the request should be upgraded from an HTTP GET request to a WebSockets request.</span></span> <span data-ttu-id="ea72c-280">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="ea72c-280">Here's an example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

<span data-ttu-id="ea72c-281">*AcceptWebSocketRequest*方法接受函數委派，因為 ASP.NET 會回溯目前 HTTP 要求，然後將控制權傳輸至函式委派。</span><span class="sxs-lookup"><span data-stu-id="ea72c-281">The *AcceptWebSocketRequest* method accepts a function delegate because ASP.NET unwinds the current HTTP request and then transfers control to the function delegate.</span></span> <span data-ttu-id="ea72c-282">在概念上類似於您如何使用這種方法是*System.Threading.Thread*，您可以在此定義的工作在背景中執行的執行緒開始委派。</span><span class="sxs-lookup"><span data-stu-id="ea72c-282">Conceptually this approach is similar to how you use *System.Threading.Thread*, where you define a thread-start delegate in which background work is performed.</span></span>

<span data-ttu-id="ea72c-283">ASP.NET 和用戶端已順利完成 Websocket 信號交換之後，ASP.NET 會呼叫您的委派和 WebSockets 的應用程式開始執行。</span><span class="sxs-lookup"><span data-stu-id="ea72c-283">After ASP.NET and the client have successfully completed a WebSockets handshake, ASP.NET calls your delegate and the WebSockets application starts running.</span></span> <span data-ttu-id="ea72c-284">下列程式碼範例顯示一個簡單的回應應用程式，在 ASP.NET 中使用的內建的 Websocket 支援：</span><span class="sxs-lookup"><span data-stu-id="ea72c-284">The following code example shows a simple echo application that uses the built-in WebSockets support in ASP.NET:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

<span data-ttu-id="ea72c-285">在.NET 4.5 中的支援*await*關鍵字和以工作為基礎的非同步作業相當適合用來撰寫 WebSockets 的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-285">The support in .NET 4.5 for the *await* keyword and asynchronous task-based operations is a natural fit for writing WebSockets applications.</span></span> <span data-ttu-id="ea72c-286">在程式碼範例示範的 Websocket 要求 ASP.NET 內完全以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="ea72c-286">The code example shows that a WebSockets request runs completely asynchronously inside ASP.NET.</span></span> <span data-ttu-id="ea72c-287">在應用程式以非同步方式等候呼叫，從用戶端傳送的訊息*await 通訊端。ReceiveAsync*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-287">The application waits asynchronously for a message to be sent from a client by calling *await socket.ReceiveAsync*.</span></span> <span data-ttu-id="ea72c-288">同樣地，傳送非同步訊息給用戶端藉由呼叫*await 通訊端。SendAsync*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-288">Similarly, you can send an asynchronous message to a client by calling *await socket.SendAsync*.</span></span>

<span data-ttu-id="ea72c-289">在瀏覽器中，應用程式會收到 Websocket 訊息*onmessage*函式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-289">In the browser, an application receives WebSockets messages through an *onmessage* function.</span></span> <span data-ttu-id="ea72c-290">若要從瀏覽器傳送訊息，請呼叫*傳送*方法*WebSocket* DOM 型別，在此範例中所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-290">To send a message from a browser, you call the *send* method of the *WebSocket* DOM type, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

<span data-ttu-id="ea72c-291">在未來，我們可能會發行這項功能，抽象消失的低層級撰寫的程式碼也就一些所需在此版本中的 Websocket 應用程式的更新。</span><span class="sxs-lookup"><span data-stu-id="ea72c-291">In the future, we might release updates to this functionality that abstract away some of the low-level coding that is required in this release for WebSockets applications.</span></span>

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a><span data-ttu-id="ea72c-292">統合和縮製</span><span class="sxs-lookup"><span data-stu-id="ea72c-292">Bundling and Minification</span></span>

<span data-ttu-id="ea72c-293">統合可讓您將個別的 JavaScript 和 CSS 檔案合併成一個可以被視為單一檔案的配套。</span><span class="sxs-lookup"><span data-stu-id="ea72c-293">Bundling lets you combine individual JavaScript and CSS files into a bundle that can be treated like a single file.</span></span> <span data-ttu-id="ea72c-294">縮製總是 JavaScript 和 CSS 檔案，藉由移除空白字元和其他不必要的字元。</span><span class="sxs-lookup"><span data-stu-id="ea72c-294">Minification condenses JavaScript and CSS files by removing whitespace and other characters that are not required.</span></span> <span data-ttu-id="ea72c-295">這些功能是由 Web Form、 ASP.NET MVC 和 Web 網頁所使用。</span><span class="sxs-lookup"><span data-stu-id="ea72c-295">These features work with Web Forms, ASP.NET MVC, and Web Pages.</span></span>

<span data-ttu-id="ea72c-296">套件組合會建立使用 Bundle 類別或其中一個子類別、 組合和 StyleBundle。</span><span class="sxs-lookup"><span data-stu-id="ea72c-296">Bundles are created using the Bundle class or one of its child classes, ScriptBundle and StyleBundle.</span></span> <span data-ttu-id="ea72c-297">設定套件組合的執行個體之後, 組合是所提供的內送要求只將它新增至全域 BundleCollection 執行個體。</span><span class="sxs-lookup"><span data-stu-id="ea72c-297">After configuring an instance of a bundle, the bundle is made available to incoming requests by simply adding it to a global BundleCollection instance.</span></span> <span data-ttu-id="ea72c-298">在預設範本中，套件組合設定被執行 BundleConfig 檔案中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-298">In the default templates, bundle configuration is performed in a BundleConfig file.</span></span> <span data-ttu-id="ea72c-299">此預設設定建立的所有核心指令碼和範本所使用的 css 檔案的搭售方案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-299">This default configuration creates bundles for all of the core scripts and css files used by the templates.</span></span>

<span data-ttu-id="ea72c-300">從檢視內套組會使用幾個可能的 helper 方法的其中一個參考。</span><span class="sxs-lookup"><span data-stu-id="ea72c-300">Bundles are referenced from within views by using one of a couple possible helper methods.</span></span> <span data-ttu-id="ea72c-301">為了支援呈現不同的標記在偵錯與發行模式中的套件組合，組合和 StyleBundle 類別有 helper 方法會呈現。</span><span class="sxs-lookup"><span data-stu-id="ea72c-301">In order to support rendering different markup for a bundle when in debug vs. release mode, the ScriptBundle and StyleBundle classes have the helper method, Render.</span></span> <span data-ttu-id="ea72c-302">在 偵錯模式中，轉譯會產生組合中的每個資源的標記。</span><span class="sxs-lookup"><span data-stu-id="ea72c-302">When in debug mode, Render will generate markup for each resource in the bundle.</span></span> <span data-ttu-id="ea72c-303">在發行模式中轉譯會產生整個套件組合的單一標記項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-303">When in release mode, Render will generate a single markup element for the entire bundle.</span></span> <span data-ttu-id="ea72c-304">切換之間偵錯和發行模式，可透過修改 compilation 項目，在 web.config 中的偵錯屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-304">Toggling between debug and release mode can be accomplished by modifying the debug attribute of the compilation element in web.config as shown below:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

<span data-ttu-id="ea72c-305">此外，啟用或停用最佳化可以直接透過 BundleTable.EnableOptimizations 屬性來設定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-305">Additionally, enabling or disabling optimization can be set directly via the BundleTable.EnableOptimizations property.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

<span data-ttu-id="ea72c-306">當檔案會集結時，它們會先依字母順序排序 (中所顯示的方式**方案總管 中**)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-306">When files are bundled, they are first sorted alphabetically (the way they are displayed in **Solution Explorer**).</span></span> <span data-ttu-id="ea72c-307">然後組織，讓已知的程式庫和其自訂的擴充功能 （例如 jQuery、 MooTools 和 Dojo） 會載入第一次。</span><span class="sxs-lookup"><span data-stu-id="ea72c-307">They are then organized so that known libraries and their custom extensions (such as jQuery, MooTools, and Dojo) are loaded first.</span></span> <span data-ttu-id="ea72c-308">比方說，會是最終的順序，統合的指令碼資料夾，如上所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-308">For example, the final order for the bundling of the Scripts folder as shown above will be:</span></span>

1. <span data-ttu-id="ea72c-309">jquery-1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="ea72c-309">jquery-1.6.2.js</span></span>
2. <span data-ttu-id="ea72c-310">jquery-ui.js</span><span class="sxs-lookup"><span data-stu-id="ea72c-310">jquery-ui.js</span></span>
3. <span data-ttu-id="ea72c-311">jquery.tools.js</span><span class="sxs-lookup"><span data-stu-id="ea72c-311">jquery.tools.js</span></span>
4. <span data-ttu-id="ea72c-312">a.js</span><span class="sxs-lookup"><span data-stu-id="ea72c-312">a.js</span></span>

<span data-ttu-id="ea72c-313">CSS 檔案也會依字母順序排序，並再重新組織，以便 reset.css 和 normalize.css 前面的任何其他檔案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-313">CSS files are also sorted alphabetically and then reorganized so that reset.css and normalize.css come before any other file.</span></span> <span data-ttu-id="ea72c-314">最終排序的統合如上所示的 [Styles] 資料夾，將會是這樣：</span><span class="sxs-lookup"><span data-stu-id="ea72c-314">The final sorting of the bundling of the Styles folder shown above will be this:</span></span>

1. <span data-ttu-id="ea72c-315">reset.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-315">reset.css</span></span>
2. <span data-ttu-id="ea72c-316">content.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-316">content.css</span></span>
3. <span data-ttu-id="ea72c-317">forms.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-317">forms.css</span></span>
4. <span data-ttu-id="ea72c-318">globals.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-318">globals.css</span></span>
5. <span data-ttu-id="ea72c-319">menu.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-319">menu.css</span></span>
6. <span data-ttu-id="ea72c-320">styles.css</span><span class="sxs-lookup"><span data-stu-id="ea72c-320">styles.css</span></span>

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a><span data-ttu-id="ea72c-321">適用於虛擬主機的效能改進</span><span class="sxs-lookup"><span data-stu-id="ea72c-321">Performance Improvements for Web Hosting</span></span>

<span data-ttu-id="ea72c-322">.NET Framework 4.5 和 Windows 8 導入功能，可協助您達成大幅提升效能的 web 伺服器工作負載。</span><span class="sxs-lookup"><span data-stu-id="ea72c-322">The .NET Framework 4.5 and Windows 8 introduce features that can help you achieve a significant performance boost for web-server workloads.</span></span> <span data-ttu-id="ea72c-323">這包括減少 （最多 35%) 在這兩個啟動時間和 web 裝載使用 ASP.NET 的站台的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="ea72c-323">This includes a reduction (up to 35%) in both startup time and in the memory footprint of web hosting sites that use ASP.NET.</span></span>

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a><span data-ttu-id="ea72c-324">關鍵效能因素</span><span class="sxs-lookup"><span data-stu-id="ea72c-324">Key performance factors</span></span>

<span data-ttu-id="ea72c-325">在理想情況下，應使用的所有網站，而且在記憶體中以確保快速回應的下一個要求，每當其中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-325">Ideally, all websites should be active and in memory to assure quick response to the next request, whenever it comes.</span></span> <span data-ttu-id="ea72c-326">可能會影響網站回應能力的因素包括：</span><span class="sxs-lookup"><span data-stu-id="ea72c-326">Factors that can affect site responsiveness include:</span></span>

- <span data-ttu-id="ea72c-327">應用程式集區回收之後重新啟動站台所需的時間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-327">The time it takes for a site to restart after an app pool recycles.</span></span> <span data-ttu-id="ea72c-328">這是啟動 web 伺服器處理序站台的站台的組件已不在記憶體中時所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-328">This is the time it takes to launch a web server process for the site when the site assemblies are no longer in memory.</span></span> <span data-ttu-id="ea72c-329">（平台組件是仍在記憶體中，因為它們由其他站台）。這種情況稱為 「 冷的網站，framework 暖啟動 」 或只是 「 冷網站啟動。 」</span><span class="sxs-lookup"><span data-stu-id="ea72c-329">(The platform assemblies are still in memory, since they are used by other sites.) This situation is referred to as "cold site, warm framework startup" or just "cold site startup."</span></span>
- <span data-ttu-id="ea72c-330">站台會佔用的記憶體數量。</span><span class="sxs-lookup"><span data-stu-id="ea72c-330">How much memory the site occupies.</span></span> <span data-ttu-id="ea72c-331">這個詞彙是 「 每個網站的記憶體耗用量 」 或 「 取消共用工作集 」。</span><span class="sxs-lookup"><span data-stu-id="ea72c-331">Terms for this are "per-site memory consumption" or "unshared working set."</span></span>

<span data-ttu-id="ea72c-332">新的效能改進專注於這兩個這些因素。</span><span class="sxs-lookup"><span data-stu-id="ea72c-332">The new performance improvements focus on both of these factors.</span></span>

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a><span data-ttu-id="ea72c-333">如需新效能功能的需求</span><span class="sxs-lookup"><span data-stu-id="ea72c-333">Requirements for New Performance Features</span></span>

<span data-ttu-id="ea72c-334">新功能的需求可以分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="ea72c-334">The requirements for the new features can be broken down into these categories:</span></span>

- <span data-ttu-id="ea72c-335">在.NET Framework 4 執行的增強功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-335">Improvements that run on the .NET Framework 4.</span></span>
- <span data-ttu-id="ea72c-336">需要.NET Framework 4.5，但是可以在任何版本的 Windows 上執行的改進。</span><span class="sxs-lookup"><span data-stu-id="ea72c-336">Improvements that require the .NET Framework 4.5 but can run on any version of Windows.</span></span>
- <span data-ttu-id="ea72c-337">有只與 Windows 8 上執行的.NET Framework 4.5 的增強功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-337">Improvements that are available only with .NET Framework 4.5 running on Windows 8.</span></span>

<span data-ttu-id="ea72c-338">效能會隨著每個層級的改進措施，您就可以啟用。</span><span class="sxs-lookup"><span data-stu-id="ea72c-338">Performance increases with each level of improvement that you are able to enable.</span></span>

<span data-ttu-id="ea72c-339">某些.NET Framework 4.5 改善利用更廣泛套用至其他案例的效能功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-339">Some of the .NET Framework 4.5 improvements take advantage of broader performance features that apply to other scenarios as well.</span></span>

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a><span data-ttu-id="ea72c-340">共用通用的組件</span><span class="sxs-lookup"><span data-stu-id="ea72c-340">Sharing Common Assemblies</span></span>

<span data-ttu-id="ea72c-341">**需求**:.NET Framework 4 和 Visual Studio 11 開發人員預覽 SDK</span><span class="sxs-lookup"><span data-stu-id="ea72c-341">**Requirement**: .NET Framework 4 and Visual Studio 11 Developer Preview SDK</span></span>

<span data-ttu-id="ea72c-342">不同的站台伺服器上通常會使用相同的協助程式組件 （例如，組件從入門套件 」 或 「 範例應用程式）。</span><span class="sxs-lookup"><span data-stu-id="ea72c-342">Different sites on a server often use the same helper assemblies (for example, assemblies from a starter kit or sample application).</span></span> <span data-ttu-id="ea72c-343">每個站台會有自己的這些組件的複本，它的 Bin 目錄中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-343">Each site has its own copy of these assemblies in its Bin directory.</span></span> <span data-ttu-id="ea72c-344">即使組件的物件程式碼完全相同，它們是實際分開的組件，因此每個組件具有個別讀取冷的站台啟動期間，並分開保留在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-344">Even though the object code for the assemblies is identical, they're physically separate assemblies, so each assembly has to be read separately during cold site startup and kept separately in memory.</span></span>

<span data-ttu-id="ea72c-345">新的 interning 功能解決了此效率不彰，以及減少 RAM 需求和負載的時間。</span><span class="sxs-lookup"><span data-stu-id="ea72c-345">The new interning functionality solves this inefficiency and reduces both RAM requirements and load time.</span></span> <span data-ttu-id="ea72c-346">暫留 」 可讓 Windows 檔案系統中保留一份每個組件，並在站台 Bin 資料夾中的個別組件會取代成單一複本的符號連結。</span><span class="sxs-lookup"><span data-stu-id="ea72c-346">Interning lets Windows keep a single copy of each assembly in the file system, and individual assemblies in the site Bin folders are replaced with symbolic links to the single copy.</span></span> <span data-ttu-id="ea72c-347">如果個別的網站需要不同版本的組件，符號連結取代了新版的組件，所以只有該站台會受到影響。</span><span class="sxs-lookup"><span data-stu-id="ea72c-347">If an individual site needs a distinct version of the assembly, the symbolic link is replaced by the new version of the assembly, and only that site is affected.</span></span>

<span data-ttu-id="ea72c-348">共用使用符號連結的組件需要新的工具，名為 aspnet\_intern.exe，可讓您建立及管理保留的組件的存放區。</span><span class="sxs-lookup"><span data-stu-id="ea72c-348">Sharing assemblies using symbolic links requires a new tool named aspnet\_intern.exe, which lets you create and manage the store of interned assemblies.</span></span> <span data-ttu-id="ea72c-349">它可在 Visual Studio 11 開發人員預覽 SDK 的一部分。</span><span class="sxs-lookup"><span data-stu-id="ea72c-349">It is provided as a part of the Visual Studio 11 Developer Preview SDK.</span></span> <span data-ttu-id="ea72c-350">(不過，它會在具有只安裝.NET Framework 4，假設您已安裝最新的系統上運作[更新](https://support.microsoft.com/kb/2468871)。)</span><span class="sxs-lookup"><span data-stu-id="ea72c-350">(However, it will work on a system that has only the .NET Framework 4 installed, assuming you have installed the latest [update](https://support.microsoft.com/kb/2468871).)</span></span>

<span data-ttu-id="ea72c-351">若要確定所有合格的組件已暫留，您可以執行 aspnet\_intern.exe 定期 （例如每週一次當做排程工作）。</span><span class="sxs-lookup"><span data-stu-id="ea72c-351">To make sure all eligible assemblies have been interned, you run aspnet\_intern.exe periodically (for example, once a week as a scheduled task).</span></span> <span data-ttu-id="ea72c-352">典型的用法就是，如下所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-352">A typical use is as follows:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

<span data-ttu-id="ea72c-353">若要查看所有選項，請使用任何引數執行工具。</span><span class="sxs-lookup"><span data-stu-id="ea72c-353">To see all options, run the tool with no arguments.</span></span>

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a><span data-ttu-id="ea72c-354">使用多核心 JIT 編譯的啟動速度更快</span><span class="sxs-lookup"><span data-stu-id="ea72c-354">Using multi-Core JIT compilation for faster startup</span></span>

<span data-ttu-id="ea72c-355">**需求**:.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="ea72c-355">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="ea72c-356">冷的站台啟動，不只執行組件必須從磁碟讀取，但站台必須是 JIT 編譯。</span><span class="sxs-lookup"><span data-stu-id="ea72c-356">For a cold site startup, not only do assemblies have to be read from disk, but the site must be JIT-compiled.</span></span> <span data-ttu-id="ea72c-357">針對複雜的站台，這可以新增明顯的延遲。</span><span class="sxs-lookup"><span data-stu-id="ea72c-357">For a complex site, this can add significant delays.</span></span> <span data-ttu-id="ea72c-358">在.NET Framework 4.5 的新通用技術可減少這些延遲將分散在可用的處理器核心的 JIT 編譯。</span><span class="sxs-lookup"><span data-stu-id="ea72c-358">A new general-purpose technique in the .NET Framework 4.5 reduces these delays by spreading JIT-compilation across available processor cores.</span></span> <span data-ttu-id="ea72c-359">它會盡可能並儘早使用期間所收集的資訊之前啟動站台。</span><span class="sxs-lookup"><span data-stu-id="ea72c-359">It does this as much and as early as possible by using information gathered during previous launches of the site.</span></span> <span data-ttu-id="ea72c-360">藉由將此功能[System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-360">This functionality implemented by the [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) method.</span></span>

<span data-ttu-id="ea72c-361">JIT 編譯使用多個核心預設會啟用在 ASP.NET 中，因此您不需要採取任何動作來利用這項功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-361">JIT-compiling using multiple cores is enabled by default in ASP.NET, so you do not need to do anything to take advantage of this feature.</span></span> <span data-ttu-id="ea72c-362">如果您想要停用此功能，請在 Web.config 檔案中進行下列設定：</span><span class="sxs-lookup"><span data-stu-id="ea72c-362">If you want to disable this feature, make the following setting in the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a><span data-ttu-id="ea72c-363">調整記憶體回收的記憶體最佳化</span><span class="sxs-lookup"><span data-stu-id="ea72c-363">Tuning garbage collection to optimize for memory</span></span>

<span data-ttu-id="ea72c-364">**需求**:.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="ea72c-364">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="ea72c-365">當站台執行時，其使用的記憶體回收行程 (GC) 堆積可以是它的記憶體耗用量的重要因素。</span><span class="sxs-lookup"><span data-stu-id="ea72c-365">Once a site is running, its use of the garbage-collector (GC) heap can be a significant factor in its memory consumption.</span></span> <span data-ttu-id="ea72c-366">任何記憶體回收行程，例如.NET Framework GC 會讓 CPU 時間 （頻率和重要性的集合） 和記憶體耗用量 （用於新的、 已釋放，或可以釋放的物件的額外空格） 之間的權衡取捨。</span><span class="sxs-lookup"><span data-stu-id="ea72c-366">Like any garbage collector, the .NET Framework GC makes tradeoffs between CPU time (frequency and significance of collections) and memory consumption (extra space that is used for new, freed, or free-able objects).</span></span> <span data-ttu-id="ea72c-367">舊版本中，我們提供指引有關如何設定以達到適當的平衡 GC (例如，請參閱[ASP.NET 2.0/3.5 共用裝載設定](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。</span><span class="sxs-lookup"><span data-stu-id="ea72c-367">For previous releases, we have provided guidance on how to configure the GC to achieve the right balance (for example, see [ASP.NET 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).</span></span>

<span data-ttu-id="ea72c-368">針對.NET Framework 4.5 中，而不是多個獨立設定，工作負載定義的組態設定，可讓所有先前建議的 GC 設定，以及新的微調，可提供額外的效能，每個網站工作集。</span><span class="sxs-lookup"><span data-stu-id="ea72c-368">For the .NET Framework 4.5, instead of multiple standalone settings, a workload-defined configuration setting is available that enables all of the previously recommended GC settings as well as new tuning that delivers additional performance for the per-site working set.</span></span>

<span data-ttu-id="ea72c-369">若要啟用調整的 GC 記憶體，請先 Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config 檔案中加入下列設定：</span><span class="sxs-lookup"><span data-stu-id="ea72c-369">To enable GC memory tuning, add the following setting to the Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

<span data-ttu-id="ea72c-370">(如果您已熟悉 aspnet.config 的變更之前的指引，請注意，此設定會取代舊的設定 — 比方說，則不需要設定 Gcserver>、 gcConcurrent 等。您不必移除舊的設定。）</span><span class="sxs-lookup"><span data-stu-id="ea72c-370">(If you're familiar with the previous guidance for changes to aspnet.config, note that this setting replaces the old settings — for example, there is no need to set gcServer, gcConcurrent, etc. You do not have to remove the old settings.)</span></span>

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a><span data-ttu-id="ea72c-371">預先擷取 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="ea72c-371">Prefetching for web applications</span></span>

<span data-ttu-id="ea72c-372">**需求**: Windows 8 上執行的.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="ea72c-372">**Requirement**: .NET Framework 4.5 running on Windows 8</span></span>

<span data-ttu-id="ea72c-373">Windows 具有數個版本，包含技術，稱為[預先擷取程式](http://en.wikipedia.org/wiki/Prefetcher)，降低磁碟讀取的應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="ea72c-373">For several releases, Windows has included a technology known as the [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) that reduces the disk-read cost of application startup.</span></span> <span data-ttu-id="ea72c-374">由於冷啟動是主要的用戶端應用程式的問題，這項技術尚未包含在 Windows Server 中，其中包含 server 所必要的元件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-374">Because cold startup is a problem predominantly for client applications, this technology has not been included in Windows Server, which includes only components that are essential to a server.</span></span> <span data-ttu-id="ea72c-375">預先提取是現在用於 Windows Server，它可以在此最佳化的個別網站推出的最新版本。</span><span class="sxs-lookup"><span data-stu-id="ea72c-375">Prefetching is now available in the latest version of Windows Server, where it can optimize the launch of individual websites.</span></span>

<span data-ttu-id="ea72c-376">適用於 Windows Server 預設不會啟用預先擷取程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-376">For Windows Server, the prefetcher is not enabled by default.</span></span> <span data-ttu-id="ea72c-377">若要啟用並設定高密度 web 裝載的預先擷取程式，執行下列命令組在命令列：</span><span class="sxs-lookup"><span data-stu-id="ea72c-377">To enable and configure the prefetcher for high-density web hosting, run the following set of commands at the command line:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

<span data-ttu-id="ea72c-378">然後，若要整合 ASP.NET 應用程式的預先擷取程式，加入下列 Web.config 檔案：</span><span class="sxs-lookup"><span data-stu-id="ea72c-378">Then, to integrate the prefetcher with ASP.NET applications, add the following to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="ea72c-379">ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="ea72c-379">ASP.NET Web Forms</span></span>

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a><span data-ttu-id="ea72c-380">強型別的資料控制項</span><span class="sxs-lookup"><span data-stu-id="ea72c-380">Strongly Typed Data Controls</span></span>

<span data-ttu-id="ea72c-381">在 ASP.NET 4.5 Web Form 會包含一些改良，用於處理資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-381">In ASP.NET 4.5, Web Forms includes some improvements for working with data.</span></span> <span data-ttu-id="ea72c-382">第一次的改進是強型別的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-382">The first improvement is strongly typed data controls.</span></span> <span data-ttu-id="ea72c-383">在舊版的 ASP.NET Web Form 控制項，為您顯示資料繫結值，使用*Eval*和資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="ea72c-383">For Web Forms controls in previous versions of ASP.NET, you display a data-bound value using *Eval* and a data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

<span data-ttu-id="ea72c-384">在您使用雙向資料繫結，如*繫結*:</span><span class="sxs-lookup"><span data-stu-id="ea72c-384">For two-way data binding, you use *Bind*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

<span data-ttu-id="ea72c-385">在執行階段，這些呼叫會使用反映來讀取指定成員的值，然後在標記中顯示結果。</span><span class="sxs-lookup"><span data-stu-id="ea72c-385">At run time, these calls use reflection to read the value of the specified member and then display the result in the markup.</span></span> <span data-ttu-id="ea72c-386">這種方法可讓您更輕鬆地針對任意、 unshaped 資料的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="ea72c-386">This approach makes it easy to data bind against arbitrary, unshaped data.</span></span>

<span data-ttu-id="ea72c-387">不過，這類的資料繫結運算式不支援成員名稱、 瀏覽 （例如移至定義），或在編譯時間檢查這些名稱的功能，例如 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="ea72c-387">However, data-binding expressions like this don't support features like IntelliSense for member names, navigation (like Go To Definition), or compile-time checking for these names.</span></span>

<span data-ttu-id="ea72c-388">若要解決此問題，ASP.NET 4.5 會新增的控制項繫結至資料的資料型別宣告的功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-388">To address this issue, ASP.NET 4.5 adds the ability to declare the data type of the data that a control is bound to.</span></span> <span data-ttu-id="ea72c-389">做法是使用新*ItemType*屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-389">You do this using the new *ItemType* property.</span></span> <span data-ttu-id="ea72c-390">當您設定這個屬性時，兩個新的具型別的的變數可在資料繫結運算式的範圍內：*項目*並*BindItem*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-390">When you set this property, two new typed variables are available in the scope of data-binding expressions: *Item* and *BindItem*.</span></span> <span data-ttu-id="ea72c-391">因為變數強型別，您會取得之完整優勢的 Visual Studio 開發經驗。</span><span class="sxs-lookup"><span data-stu-id="ea72c-391">Because the variables are strongly typed, you get the full benefits of the Visual Studio development experience.</span></span>


<span data-ttu-id="ea72c-392">如需雙向資料繫結運算式，使用*BindItem*變數：</span><span class="sxs-lookup"><span data-stu-id="ea72c-392">For two-way data-binding expressions, use the *BindItem* variable:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


<span data-ttu-id="ea72c-393">ASP.NET Web Forms framework 中大部分支援資料繫結的控制項已更新為支援*ItemType*屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-393">Most controls in the ASP.NET Web Forms framework that support data binding have been updated to support the *ItemType* property.</span></span>

<a id="_Toc318097387"></a>
### <a name="model-binding"></a><span data-ttu-id="ea72c-394">模型繫結</span><span class="sxs-lookup"><span data-stu-id="ea72c-394">Model Binding</span></span>

<span data-ttu-id="ea72c-395">模型繫結延伸 ASP.NET Web Form 控制項中使用程式碼為主的資料存取的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="ea72c-395">Model binding extends data binding in ASP.NET Web Forms controls to work with code-focused data access.</span></span> <span data-ttu-id="ea72c-396">其中包含從概念*ObjectDataSource*控制項和 ASP.NET MVC 中的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="ea72c-396">It incorporates concepts from the *ObjectDataSource* control and from model binding in ASP.NET MVC.</span></span>

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a><span data-ttu-id="ea72c-397">選取資料</span><span class="sxs-lookup"><span data-stu-id="ea72c-397">Selecting data</span></span>

<span data-ttu-id="ea72c-398">若要設定資料控制項使用模型繫結來選取資料，您將控制項的*SelectMethod*網頁的程式碼中的方法名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-398">To configure a data control to use model binding to select data, you set the control's *SelectMethod* property to the name of a method in the page's code.</span></span> <span data-ttu-id="ea72c-399">資料控制項在網頁生命週期中的適當時間呼叫的方法，並自動將傳回的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="ea72c-399">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="ea72c-400">不需要明確呼叫*DataBind*方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-400">There's no need to explicitly call the *DataBind* method.</span></span>

<span data-ttu-id="ea72c-401">在下列範例中， *GridView*控制項設定為使用名為方法*GetCategories*:</span><span class="sxs-lookup"><span data-stu-id="ea72c-401">In the following example, the *GridView* control is configured to use a method named *GetCategories*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

<span data-ttu-id="ea72c-402">您建立*GetCategories*網頁的程式碼中的方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-402">You create the *GetCategories* method in the page's code.</span></span> <span data-ttu-id="ea72c-403">簡單的選取作業中，此方法不需要任何參數，並應該會傳回*IEnumerable*或是*IQueryable*物件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-403">For a simple select operation, the method needs no parameters and should return an *IEnumerable* or *IQueryable* object.</span></span> <span data-ttu-id="ea72c-404">如果新*ItemType*屬性設定 (可讓強型別資料繫結運算式，如底下所述[強型別資料控制項](#_Toc318097386)稍早)，這些介面的泛型版本應該傳回 — *IEnumerable&lt;T&gt;* 或是*IQueryable&lt;T&gt;*，使用*T*比對的型別參數*ItemType*屬性 (例如*IQueryable&lt;類別&gt;*)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-404">If the new *ItemType* property is set (which enables strongly typed data-binding expressions, as explained under [Strongly Typed Data Controls](#_Toc318097386) earlier), the generic versions of these interfaces should be returned — *IEnumerable&lt;T&gt;* or *IQueryable&lt;T&gt;*, with the *T* parameter matching the type of the *ItemType* property (for example, *IQueryable&lt;Category&gt;*).</span></span>

<span data-ttu-id="ea72c-405">下列範例顯示的程式碼*GetCategories*方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-405">The following example shows the code for a *GetCategories* method.</span></span> <span data-ttu-id="ea72c-406">此範例會使用 Northwind 範例資料庫中的 Entity Framework Code First 模型。</span><span class="sxs-lookup"><span data-stu-id="ea72c-406">This example uses the Entity Framework Code First model with the Northwind sample database.</span></span> <span data-ttu-id="ea72c-407">程式碼可確保查詢會傳回的每個類別藉由相關的產品詳細資料*Include*方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-407">The code makes sure that the query returns details of the related products for each category by way of the *Include* method.</span></span> <span data-ttu-id="ea72c-408">(這可確保*TemplateField*標記中的項目顯示每個類別中的產品計數，而不需要[n + 1 選取](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)。)</span><span class="sxs-lookup"><span data-stu-id="ea72c-408">(This ensures that the *TemplateField* element in the markup displays the count of products in each category without requiring an [n+1 select](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

<span data-ttu-id="ea72c-409">當頁面執行時，則*GridView*控制呼叫*GetCategories*方法自動並呈現傳回的資料，使用已設定的欄位：</span><span class="sxs-lookup"><span data-stu-id="ea72c-409">When the page runs, the *GridView* control calls the *GetCategories* method automatically and renders the returned data using the configured fields:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

<span data-ttu-id="ea72c-410">因為選取的方法會傳回*IQueryable*物件， *GridView*控制項可以進一步操作查詢，然後再執行它。</span><span class="sxs-lookup"><span data-stu-id="ea72c-410">Because the select method returns an *IQueryable* object, the *GridView* control can further manipulate the query before executing it.</span></span> <span data-ttu-id="ea72c-411">例如， *GridView*控制可以加入排序和分頁所傳回的查詢運算式*IQueryable*物件之前執行，使這些作業由基礎LINQ 提供者。</span><span class="sxs-lookup"><span data-stu-id="ea72c-411">For example, the *GridView* control can add query expressions for sorting and paging to the returned *IQueryable* object before it is executed, so that those operations are performed by the underlying LINQ provider.</span></span> <span data-ttu-id="ea72c-412">在此情況下，Entity Framework 可確保這些作業將會在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-412">In this case, Entity Framework will ensure those operations are performed in the database.</span></span>

<span data-ttu-id="ea72c-413">下列範例所示*GridView*修改成允許排序和分頁的控制項：</span><span class="sxs-lookup"><span data-stu-id="ea72c-413">The following example shows the *GridView* control modified to allow sorting and paging:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

<span data-ttu-id="ea72c-414">現在頁面執行時，控制項可以確定資料的目前頁面會顯示並依所選資料行：</span><span class="sxs-lookup"><span data-stu-id="ea72c-414">Now when the page runs, the control can make sure that only the current page of data is displayed and that it's ordered by the selected column:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

<span data-ttu-id="ea72c-415">若要篩選傳回的資料，參數必須加入至 select 方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-415">To filter the returned data, parameters have to be added to the select method.</span></span> <span data-ttu-id="ea72c-416">模型繫結，在執行階段，系統會填入這些參數，您可以使用它們來改變查詢之前傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-416">These parameters will be populated by the model binding at run time, and you can use them to alter the query before returning the data.</span></span>

<span data-ttu-id="ea72c-417">例如，假設您想要的查詢字串中輸入關鍵字來讓使用者篩選產品。</span><span class="sxs-lookup"><span data-stu-id="ea72c-417">For example, assume that you want to let users filter products by entering a keyword in the query string.</span></span> <span data-ttu-id="ea72c-418">您可以將參數加入至方法，並更新程式碼以使用參數值：</span><span class="sxs-lookup"><span data-stu-id="ea72c-418">You can add a parameter to the method and update the code to use the parameter value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

<span data-ttu-id="ea72c-419">這個程式碼包含*何處*運算式，如果提供的值是*關鍵字*，然後傳回查詢結果。</span><span class="sxs-lookup"><span data-stu-id="ea72c-419">This code includes a *Where* expression if a value is provided for *keyword* and then returns the query results.</span></span>

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a><span data-ttu-id="ea72c-420">值提供者</span><span class="sxs-lookup"><span data-stu-id="ea72c-420">Value providers</span></span>

<span data-ttu-id="ea72c-421">前一個範例不是特定位置的值*關鍵字*參數來自。</span><span class="sxs-lookup"><span data-stu-id="ea72c-421">The previous example was not specific about where the value for the *keyword* parameter was coming from.</span></span> <span data-ttu-id="ea72c-422">若要指出這項資訊，您可以使用參數屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-422">To indicate this information, you can use a parameter attribute.</span></span> <span data-ttu-id="ea72c-423">針對此範例中，您可以使用*QueryStringAttribute*中的類別*System.Web.ModelBinding*命名空間：</span><span class="sxs-lookup"><span data-stu-id="ea72c-423">For this example, you can use the *QueryStringAttribute* class that's in the *System.Web.ModelBinding* namespace:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

<span data-ttu-id="ea72c-424">這會指示嘗試將值繫結至查詢字串中的模型繫結*關鍵字*在執行階段參數。</span><span class="sxs-lookup"><span data-stu-id="ea72c-424">This instructs model binding to try to bind a value from the query string to the *keyword* parameter at run time.</span></span> <span data-ttu-id="ea72c-425">（這可能需要執行類型轉換，雖然它不會在此情況下。）如果無法提供的值，而且不可為 null 的型別，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="ea72c-425">(This might involve performing type conversion, although it doesn't in this case.) If a value cannot be provided and the type is non-nullable, an exception is thrown.</span></span>

<span data-ttu-id="ea72c-426">這些方法的值的來源指值提供者，並指出要使用哪些值提供者的參數屬性指值提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-426">The sources of values for these methods are referred to as value providers, and the parameter attributes that indicate which value provider to use are referred to as value provider attributes.</span></span> <span data-ttu-id="ea72c-427">Web Form 會包含值提供者和所有一般來源，使用者輸入的對應屬性中的 Web Form 應用程式，例如查詢字串、 cookie、 表單值、 控制項、 檢視狀態、 工作階段狀態和設定檔屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-427">Web Forms will include value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="ea72c-428">您也可以撰寫自訂值提供者。</span><span class="sxs-lookup"><span data-stu-id="ea72c-428">You can also write custom value providers.</span></span>

<span data-ttu-id="ea72c-429">根據預設，參數名稱用做為索引鍵來尋找值提供者集合中的值。</span><span class="sxs-lookup"><span data-stu-id="ea72c-429">By default, the parameter name is used as the key to find a value in the value provider collection.</span></span> <span data-ttu-id="ea72c-430">在範例中，程式碼會尋找名為關鍵字的查詢字串值 (例如，~ / default.aspx?keyword=chef)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-430">In the example, the code will look for a query-string value named keyword (for example, ~/default.aspx?keyword=chef).</span></span> <span data-ttu-id="ea72c-431">您可以指定自訂的索引鍵做為引數傳遞至參數屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-431">You can specify a custom key by passing it as an argument to the parameter attribute.</span></span> <span data-ttu-id="ea72c-432">例如，若要使用名為 q 的查詢字串變數的值，您無法這樣做：</span><span class="sxs-lookup"><span data-stu-id="ea72c-432">For example, to use the value of the query-string variable named q, you could do this:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

<span data-ttu-id="ea72c-433">如果這個方法是在頁面的程式碼中，使用者可以藉由傳遞關鍵字，使用查詢字串來篩選結果：</span><span class="sxs-lookup"><span data-stu-id="ea72c-433">If this method is in the page's code, users can filter the results by passing a keyword using the query string:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

<span data-ttu-id="ea72c-434">模型繫結會完成許多工作，您原本必須以手動方式撰寫程式碼： 讀取的值、 檢查 null 值、 嘗試將它轉換成適當的型別、 檢查轉換是否成功，以及最後，使用中的值查詢。</span><span class="sxs-lookup"><span data-stu-id="ea72c-434">Model binding accomplishes many tasks that you would otherwise have to code by hand: reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="ea72c-435">模型繫結結果，最少的程式碼和重複使用的功能，在您的應用程式的能力。</span><span class="sxs-lookup"><span data-stu-id="ea72c-435">Model binding results in far less code and in the ability to reuse the functionality throughout your application.</span></span>

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a><span data-ttu-id="ea72c-436">依控制項的值進行篩選</span><span class="sxs-lookup"><span data-stu-id="ea72c-436">Filtering by values from a control</span></span>

<span data-ttu-id="ea72c-437">假設您想要擴充範例，以讓使用者從下拉式清單中選擇篩選值。</span><span class="sxs-lookup"><span data-stu-id="ea72c-437">Suppose you want to extend the example to let the user choose a filter value from a drop-down list.</span></span> <span data-ttu-id="ea72c-438">將下列下拉式清單加入至標記，並設定它以取得其資料從另一個方法使用*SelectMethod*屬性：</span><span class="sxs-lookup"><span data-stu-id="ea72c-438">Add the following drop-down list to the markup and configure it to get its data from another method using the *SelectMethod* property:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

<span data-ttu-id="ea72c-439">通常您也可以加入*EmptyDataTemplate*項目*GridView*控制項，讓控制項將會顯示一則訊息，如果找不到任何相符的產品：</span><span class="sxs-lookup"><span data-stu-id="ea72c-439">Typically you would also add an *EmptyDataTemplate* element to the *GridView* control so that the control will display a message if no matching products are found:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

<span data-ttu-id="ea72c-440">在頁面程式碼中，加入新的選取下拉式清單的方法：</span><span class="sxs-lookup"><span data-stu-id="ea72c-440">In the page code, add the new select method for the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

<span data-ttu-id="ea72c-441">最後，更新*GetProducts*選取採用新的參數，其中包含從下拉式清單中選取的類別識別碼的方法：</span><span class="sxs-lookup"><span data-stu-id="ea72c-441">Finally, update the *GetProducts* select method to take a new parameter that contains the ID of the selected category from the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

<span data-ttu-id="ea72c-442">現在當頁面執行時，使用者可以從下拉式清單中，選取類別和*GridView*控制項是自動重新繫結以顯示篩選過的資料。</span><span class="sxs-lookup"><span data-stu-id="ea72c-442">Now when the page runs, users can select a category from the drop-down list, and the *GridView* control is automatically re-bound to show the filtered data.</span></span> <span data-ttu-id="ea72c-443">這可能是因為模型繫結會追蹤選取的方法參數的值，並偵測是否在回傳後，已變更任何參數值。</span><span class="sxs-lookup"><span data-stu-id="ea72c-443">This is possible because model binding tracks the values of parameters for select methods and detects whether any parameter value has changed after a postback.</span></span> <span data-ttu-id="ea72c-444">如果是這樣，模型繫結會強制重新繫結至資料相關聯的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-444">If so, model binding forces the associated data control to re-bind to the data.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a><span data-ttu-id="ea72c-445">HTML 編碼的資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="ea72c-445">HTML Encoded Data-Binding Expressions</span></span>

<span data-ttu-id="ea72c-446">您可以立即進行 HTML 編碼資料繫結運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="ea72c-446">You can now HTML-encode the result of data-binding expressions.</span></span> <span data-ttu-id="ea72c-447">結尾加上冒號 （:） &lt;%# 前置詞標示資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="ea72c-447">Add a colon (:) to the end of the &lt;%# prefix that marks the data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a><span data-ttu-id="ea72c-448">低調驗證</span><span class="sxs-lookup"><span data-stu-id="ea72c-448">Unobtrusive Validation</span></span>

<span data-ttu-id="ea72c-449">您現在可以設定要使用不顯眼的 JavaScript 用戶端驗證邏輯的內建的驗證程式控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-449">You can now configure the built-in validator controls to use unobtrusive JavaScript for client-side validation logic.</span></span> <span data-ttu-id="ea72c-450">這會大幅減少的 JavaScript 內嵌在網頁標記中的轉譯，並減少整體的頁面大小。</span><span class="sxs-lookup"><span data-stu-id="ea72c-450">This significantly reduces the amount of JavaScript rendered inline in the page markup and reduces the overall page size.</span></span> <span data-ttu-id="ea72c-451">您可以使用任何一種方式來設定不顯眼的 JavaScript 驗證程式控制項：</span><span class="sxs-lookup"><span data-stu-id="ea72c-451">You can configure unobtrusive JavaScript for validator controls in any of these ways:</span></span>

- <span data-ttu-id="ea72c-452">全域藉由新增下列設定加入*&lt;appSettings&gt;* Web.config 檔案中的項目：</span><span class="sxs-lookup"><span data-stu-id="ea72c-452">Globally by adding the following setting to the *&lt;appSettings&gt;* element in the Web.config file:</span></span> 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- <span data-ttu-id="ea72c-453">全域藉由設定靜態*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*屬性設*UnobtrusiveValidationMode.WebForms* (通常在*應用程式\_啟動*Global.asax 檔案中的方法)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-453">Globally by setting the static *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* property to *UnobtrusiveValidationMode.WebForms* (typically in the *Application\_Start* method in the Global.asax file).</span></span>
- <span data-ttu-id="ea72c-454">藉由設定新的頁面針對個別*UnobtrusiveValidationMode*屬性*頁*類別*UnobtrusiveValidationMode.WebForms*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-454">Individually for a page by setting the new *UnobtrusiveValidationMode* property of the *Page* class to *UnobtrusiveValidationMode.WebForms*.</span></span>

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a><span data-ttu-id="ea72c-455">HTML5 更新</span><span class="sxs-lookup"><span data-stu-id="ea72c-455">HTML5 Updates</span></span>

<span data-ttu-id="ea72c-456">一些改善已對 Web Form 伺服器控制項，以充分利用的 HTML5 的新功能：</span><span class="sxs-lookup"><span data-stu-id="ea72c-456">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>

- <span data-ttu-id="ea72c-457">*TextMode*屬性*TextBox*控制項已更新為支援新 HTML5 輸入的類型，例如*電子郵件*， *datetime*，及等等。</span><span class="sxs-lookup"><span data-stu-id="ea72c-457">The *TextMode* property of the *TextBox* control has been updated to support the new HTML5 input types like *email*, *datetime*, and so on.</span></span>
- <span data-ttu-id="ea72c-458">*FileUpload*控制項現在支援多重檔案上傳的支援這個 HTML5 功能的瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ea72c-458">The *FileUpload* control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
- <span data-ttu-id="ea72c-459">驗證程式控制項現在支援驗證 HTML5 輸入項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-459">Validator controls now support validating HTML5 input elements.</span></span>
- <span data-ttu-id="ea72c-460">新的 HTML5 項目具有表示屬性的 URL 現在支援 runat ="server"。</span><span class="sxs-lookup"><span data-stu-id="ea72c-460">New HTML5 elements that have attributes that represent a URL now support runat="server".</span></span> <span data-ttu-id="ea72c-461">如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來代表應用程式根目錄運算子 (例如&lt;視訊 runat ="server"src="~/myVideo.wmv"/&gt;)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-461">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat="server" src="~/myVideo.wmv" /&gt;).</span></span>
- <span data-ttu-id="ea72c-462">*UpdatePanel*已修正以支援張貼的 HTML5 輸入的欄位的控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-462">The *UpdatePanel* control has been fixed to support posting HTML5 input fields.</span></span>

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a><span data-ttu-id="ea72c-463">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="ea72c-463">ASP.NET MVC 4</span></span>

<span data-ttu-id="ea72c-464">ASP.NET MVC 4 beta 版現已隨附於 Visual Studio 11 beta 版。</span><span class="sxs-lookup"><span data-stu-id="ea72c-464">ASP.NET MVC 4 Beta is now included with Visual Studio 11 Beta.</span></span> <span data-ttu-id="ea72c-465">ASP.NET MVC 是運用 「 模型-檢視-控制器 (MVC) 模式開發高度可測試且可維護的 Web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="ea72c-465">ASP.NET MVC is a framework for developing highly testable and maintainable Web applications by leveraging the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="ea72c-466">ASP.NET MVC 4 可讓您更輕鬆建置行動 Web 應用程式，並包含 ASP.NET Web API，可協助您建置 HTTP 服務可以連線到任何裝置。</span><span class="sxs-lookup"><span data-stu-id="ea72c-466">ASP.NET MVC 4 makes it easy to build applications for the mobile Web and includes ASP.NET Web API, which helps you build HTTP services that can reach any device.</span></span> <span data-ttu-id="ea72c-467">如需詳細資訊，請參閱 < [ASP.NET MVC 4 版本資訊](mvc4-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-467">For more information, see the [ASP.NET MVC 4 Release Notes](mvc4-release-notes.md).</span></span>

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a><span data-ttu-id="ea72c-468">ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="ea72c-468">ASP.NET Web Pages 2</span></span>

<span data-ttu-id="ea72c-469">新功能包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="ea72c-469">New features include the following:</span></span>

- <span data-ttu-id="ea72c-470">新增和更新的網站範本。</span><span class="sxs-lookup"><span data-stu-id="ea72c-470">New and updated site templates.</span></span>
- <span data-ttu-id="ea72c-471">新增伺服器端和用戶端驗證，使用*驗證*協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-471">Adding server-side and client-side validation using the *Validation* helper.</span></span>
- <span data-ttu-id="ea72c-472">若要註冊指令碼使用資產管理員能力。</span><span class="sxs-lookup"><span data-stu-id="ea72c-472">The ability to register scripts using an assets manager.</span></span>
- <span data-ttu-id="ea72c-473">啟用登入和來自 Facebook 與使用 OAuth 和 OpenID 其他站台。</span><span class="sxs-lookup"><span data-stu-id="ea72c-473">Enabling logins from Facebook and other sites using OAuth and OpenID.</span></span>
- <span data-ttu-id="ea72c-474">使用新增 mape*對應*協助程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-474">Adding maps using the *Maps* helper.</span></span>
- <span data-ttu-id="ea72c-475">執行網頁應用程式的並存。</span><span class="sxs-lookup"><span data-stu-id="ea72c-475">Running Web Pages applications side-by-side.</span></span>
- <span data-ttu-id="ea72c-476">行動裝置的轉譯頁面。</span><span class="sxs-lookup"><span data-stu-id="ea72c-476">Rendering pages for mobile devices.</span></span>

<span data-ttu-id="ea72c-477">如需有關這些功能和整頁的程式碼範例的詳細資訊，請參閱 < [Web Pages 2 的 beta 版的最佳功能](https://go.microsoft.com/fwlink/?LinkID=227824)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-477">For more information about these features and full-page code examples, see [The Top Features in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a><span data-ttu-id="ea72c-478">Visual Web Developer 11 beta 版</span><span class="sxs-lookup"><span data-stu-id="ea72c-478">Visual Web Developer 11 Beta</span></span>

<span data-ttu-id="ea72c-479">本節提供在 Visual Web Developer 11 beta 版和 Visual Studio 2012 Release Candidate 中的 web 程式開發的增強功能相關資訊。</span><span class="sxs-lookup"><span data-stu-id="ea72c-479">This section provides information about improvements for web development in Visual Web Developer 11 Beta and Visual Studio 2012 Release Candidate.</span></span>

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a><span data-ttu-id="ea72c-480">Visual Studio 2010 和 Visual Studio 2012 Release Candidate （專案相容性） 之間共用的專案</span><span class="sxs-lookup"><span data-stu-id="ea72c-480">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>

<span data-ttu-id="ea72c-481">Visual Studio 2012 Release Candidate，直到新版的 Visual Studio 中開啟現有的專案啟動轉換精靈。</span><span class="sxs-lookup"><span data-stu-id="ea72c-481">Until Visual Studio 2012 Release Candidate, opening an existing project in a newer version of Visual Studio launched the Conversion Wizard.</span></span> <span data-ttu-id="ea72c-482">這並不具有回溯相容性的新格式強制升級的專案和方案的內容 （資產）。</span><span class="sxs-lookup"><span data-stu-id="ea72c-482">This forced an upgrade of the content (assets) of a project and solution to new formats that were not backward compatible.</span></span> <span data-ttu-id="ea72c-483">因此，轉換之後，您無法開啟專案在舊版的 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-483">Therefore, after the conversion you could not open the project in the older version of Visual Studio.</span></span>

<span data-ttu-id="ea72c-484">許多客戶告訴我們這不是正確的方法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-484">Many customers have told us that this was not the right approach.</span></span> <span data-ttu-id="ea72c-485">在 Visual Studio 11 beta 版，我們現在支援共用的專案和解決方案，利用 Visual Studio 2010 SP1。</span><span class="sxs-lookup"><span data-stu-id="ea72c-485">In Visual Studio 11 Beta, we now support sharing projects and solutions with Visual Studio 2010 SP1.</span></span> <span data-ttu-id="ea72c-486">這表示，如果您在 Visual Studio 2012 發行候選版本中開啟 2010年專案，您仍然能夠在 Visual Studio 2010 SP1 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-486">This means that if you open a 2010 project in Visual Studio 2012 Release Candidate, you will still be able to open the project in Visual Studio 2010 SP1.</span></span>

> [!NOTE]
> <span data-ttu-id="ea72c-487">Visual Studio 2010 SP1 和 Visual Studio 2012 Release Candidate 之間無法共用許多類型的專案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-487">A few types of projects cannot be shared between Visual Studio 2010 SP1 and Visual Studio 2012 Release Candidate.</span></span> <span data-ttu-id="ea72c-488">這些包含一些較舊的專案 （例如 ASP.NET MVC 2 專案） 或專案 （例如安裝程式專案） 的特殊用途。</span><span class="sxs-lookup"><span data-stu-id="ea72c-488">These include some older projects (such as ASP.NET MVC 2 projects) or projects for special purposes (such as Setup projects).</span></span>

<span data-ttu-id="ea72c-489">當您在 Visual Studio 11 Beta 中的第一次開啟 Visual Studio 2010 SP1 Web 專案時，專案檔會新增下列屬性：</span><span class="sxs-lookup"><span data-stu-id="ea72c-489">When you open a Visual Studio 2010 SP1 Web project for the first time in Visual Studio 11 Beta, the following properties are added to the project file:</span></span>

- <span data-ttu-id="ea72c-490">FileUpgradeFlags</span><span class="sxs-lookup"><span data-stu-id="ea72c-490">FileUpgradeFlags</span></span>
- <span data-ttu-id="ea72c-491">UpgradeBackupLocation</span><span class="sxs-lookup"><span data-stu-id="ea72c-491">UpgradeBackupLocation</span></span>
- <span data-ttu-id="ea72c-492">OldToolsVersion</span><span class="sxs-lookup"><span data-stu-id="ea72c-492">OldToolsVersion</span></span>
- <span data-ttu-id="ea72c-493">VisualStudioVersion</span><span class="sxs-lookup"><span data-stu-id="ea72c-493">VisualStudioVersion</span></span>
- <span data-ttu-id="ea72c-494">VSToolsPath</span><span class="sxs-lookup"><span data-stu-id="ea72c-494">VSToolsPath</span></span>

<span data-ttu-id="ea72c-495">升級專案檔的程序會使用 FileUpgradeFlags、 UpgradeBackupLocation 和 OldToolsVersion。</span><span class="sxs-lookup"><span data-stu-id="ea72c-495">FileUpgradeFlags, UpgradeBackupLocation, and OldToolsVersion are used by the process that upgrades the project file.</span></span> <span data-ttu-id="ea72c-496">還不會影響到使用 Visual Studio 2010 中的專案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-496">They have no impact on working with the project in Visual Studio 2010.</span></span>

<span data-ttu-id="ea72c-497">VisualStudioVersion 是 MSBuild 4.5，指出目前專案的 Visual Studio 版本所使用的新屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-497">VisualStudioVersion is a new property used by MSBuild 4.5 that indicates the version of Visual Studio for the current project.</span></span> <span data-ttu-id="ea72c-498">因為此屬性不存在於 MSBuild 4.0 （Visual Studio 2010 SP1 使用的 MSBuild 版本） 中，我們會將插入專案檔的預設值。</span><span class="sxs-lookup"><span data-stu-id="ea72c-498">Because this property didn't exist in MSBuild 4.0 (the version of MSBuild that Visual Studio 2010 SP1 uses), we inject a default value into the project file.</span></span>

<span data-ttu-id="ea72c-499">VSToolsPath 屬性用來判斷正確的.targets 檔案，才能從 MSBuildExtensionsPath32 設定所代表的路徑匯入。</span><span class="sxs-lookup"><span data-stu-id="ea72c-499">The VSToolsPath property is used to determine the correct .targets file to import from the path represented by the MSBuildExtensionsPath32 setting.</span></span>

<span data-ttu-id="ea72c-500">另外還有一些匯入項目相關的變更。</span><span class="sxs-lookup"><span data-stu-id="ea72c-500">There are also some changes related to Import elements.</span></span> <span data-ttu-id="ea72c-501">為了支援這兩個版本的 Visual Studio 之間的相容性需要這些變更。</span><span class="sxs-lookup"><span data-stu-id="ea72c-501">These changes are required in order to support compatibility between both versions of Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="ea72c-502">如果專案 Visual Studio 2010 SP1 和 Visual Studio 11 beta 版之間兩個不同的電腦上共用，而且專案的應用程式中包含本機資料庫\_資料 資料夾中，您必須確定 SQL server 資料庫所使用的版本安裝在兩部電腦上。</span><span class="sxs-lookup"><span data-stu-id="ea72c-502">If a project is being shared between Visual Studio 2010 SP1 and Visual Studio 11 Beta on two different computers, and if the project includes a local database in the App\_Data folder, you must make sure that the version of SQL Server used by the database is installed on both computers.</span></span>

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a><span data-ttu-id="ea72c-503">ASP.NET 4.5 網站範本中的組態變更</span><span class="sxs-lookup"><span data-stu-id="ea72c-503">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>

<span data-ttu-id="ea72c-504">已進行下列變更為預設值*Web.config*網站使用 Visual Studio 2012 Release Candidate 中的網站範本所建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="ea72c-504">The following changes have been made to the default *Web.config* file for site that are created using website templates in Visual Studio 2012 Release Candidate:</span></span>

- <span data-ttu-id="ea72c-505">在 `<httpRuntime>`項目，`encoderType`屬性現在已設定預設為使用 AntiXSS 類型已新增至 ASP.NET。</span><span class="sxs-lookup"><span data-stu-id="ea72c-505">In the `<httpRuntime>` element, the `encoderType` attribute is now set by default to use the AntiXSS types that were added to ASP.NET.</span></span> <span data-ttu-id="ea72c-506">如需詳細資訊，請參閱 < [AntiXSS 程式庫](#_Toc318097382)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-506">For details, see [AntiXSS Library](#_Toc318097382).</span></span>
- <span data-ttu-id="ea72c-507">此外，在`<httpRuntime>`項目，`requestValidationMode`屬性設為"4.5"。</span><span class="sxs-lookup"><span data-stu-id="ea72c-507">Also in the `<httpRuntime>` element, the `requestValidationMode` attribute is set to "4.5".</span></span> <span data-ttu-id="ea72c-508">這表示根據預設，設定要求驗證會使用延後 （「 延遲 」） 驗證。</span><span class="sxs-lookup"><span data-stu-id="ea72c-508">This means that by default, request validation is configured to use deferred ("lazy") validation.</span></span> <span data-ttu-id="ea72c-509">如需詳細資訊，請參閱 <<c0> [ 新 ASP.NET 要求驗證功能](#_Toc318097379)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-509">For details, see [New ASP.NET Request Validation Features](#_Toc318097379).</span></span>
- <span data-ttu-id="ea72c-510">`<modules>`項目`<system.webServer>`一節不包含`runAllManagedModulesForAllRequests`屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-510">The `<modules>` element of the `<system.webServer>` section does not contain a `runAllManagedModulesForAllRequests` attribute.</span></span> <span data-ttu-id="ea72c-511">（預設值為 false）。這表示，如果您使用尚未更新為 SP1 的 IIS 7 的版本，您可能必須路由至新站台中的問題。</span><span class="sxs-lookup"><span data-stu-id="ea72c-511">(Its default value is false.) This means that if you are using a version of IIS 7 that has not been updated to SP1, you might have issues with routing in a new site.</span></span> <span data-ttu-id="ea72c-512">如需詳細資訊，請參閱 < [IIS 7 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)。</span><span class="sxs-lookup"><span data-stu-id="ea72c-512">For more information, see [Native Support in IIS 7 for ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).</span></span>

<span data-ttu-id="ea72c-513">這些變更不會影響現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-513">These changes do not affect existing applications.</span></span> <span data-ttu-id="ea72c-514">不過，它們可能代表現有的網站和您建立使用新的範本的 ASP.NET 4.5 的新網站之間的行為差異。</span><span class="sxs-lookup"><span data-stu-id="ea72c-514">However, they might represent a difference in behavior between existing websites and new websites that you create for ASP.NET 4.5 using the new templates.</span></span>

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a><span data-ttu-id="ea72c-515">在 IIS 7 ASP.NET 路由中的原生支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-515">Native Support in IIS 7 for ASP.NET Routing</span></span>

<span data-ttu-id="ea72c-516">這不是 ASP.NET 的變更，而可能影響您如果您正在 IIS 7 未套用 SP1 更新的版本的新網站專案範本中的變更。</span><span class="sxs-lookup"><span data-stu-id="ea72c-516">This is not a change to ASP.NET as such, but a change in templates for new website projects that can affect you if you are working a version of IIS 7 that has not had the SP1 update applied.</span></span>

<span data-ttu-id="ea72c-517">在 ASP.NET 中，您可以將下列組態設定新增至應用程式，才能支援路由：</span><span class="sxs-lookup"><span data-stu-id="ea72c-517">In ASP.NET, you can add the following configuration setting to applications in order to support routing:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

<span data-ttu-id="ea72c-518">時**runAllManagedModulesForAllRequests**是 true，之類的 URL`http://mysite/myapp/home`前往 ASP.NET 中，即使沒有任何 *.aspx*， *.mvc*，或在類似的延伸模組URL。</span><span class="sxs-lookup"><span data-stu-id="ea72c-518">When **runAllManagedModulesForAllRequests** is true, a URL like `http://mysite/myapp/home` goes to ASP.NET, even though there is no *.aspx*, *.mvc*, or similar extension on the URL.</span></span>

<span data-ttu-id="ea72c-519">已對 IIS 7 的更新可讓**runAllManagedModulesForAllRequests**不必要的設定，並支援 ASP.NET 路由原生。</span><span class="sxs-lookup"><span data-stu-id="ea72c-519">An update that was made to IIS 7 makes the **runAllManagedModulesForAllRequests** setting unnecessary and supports ASP.NET routing natively.</span></span> <span data-ttu-id="ea72c-520">(如需更新的資訊，請參閱 Microsoft 支援服務文章[的更新功能，可讓特定 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。)</span><span class="sxs-lookup"><span data-stu-id="ea72c-520">(For information about the update, see the Microsoft Support article [An update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).)</span></span>

<span data-ttu-id="ea72c-521">如果您的網站在 IIS 7 上執行，而且如果 IIS 已經更新，您不需要設定**runAllManagedModulesForAllRequests**設為 true。</span><span class="sxs-lookup"><span data-stu-id="ea72c-521">If your website is running on IIS 7 and if IIS has been updated, you do not need to set **runAllManagedModulesForAllRequests** to true.</span></span> <span data-ttu-id="ea72c-522">事實上，將它設定為 true 時不建議，因為它會增加不必要的處理負擔要求。</span><span class="sxs-lookup"><span data-stu-id="ea72c-522">In fact, setting it to true is not recommended, because it adds unnecessary processing overhead to request.</span></span> <span data-ttu-id="ea72c-523">此設定為 true 時，所有的要求，包括 *.htm*， *.jpg*，和其他靜態檔案，也會經過 ASP.NET 要求管線。</span><span class="sxs-lookup"><span data-stu-id="ea72c-523">When this setting is true, all requests, including those for *.htm*, *.jpg*, and other static files, also go through the ASP.NET request pipeline.</span></span>

<span data-ttu-id="ea72c-524">如果您建立新的 ASP.NET 4.5 網站，使用 Visual Studio 2012 RC 中提供的範本時，網站組態不包含**runAllManagedModulesForAllRequests**設定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-524">If you create a new ASP.NET 4.5 website using the templates that are provided in Visual Studio 2012 RC, the configuration for the website does not include the **runAllManagedModulesForAllRequests** setting.</span></span> <span data-ttu-id="ea72c-525">這表示根據預設設定為 false。</span><span class="sxs-lookup"><span data-stu-id="ea72c-525">This means that by default the setting is false.</span></span>

<span data-ttu-id="ea72c-526">如果您之後執行的網站在 Windows 7 上未安裝 SP1，IIS 7 不會包含所需的更新。</span><span class="sxs-lookup"><span data-stu-id="ea72c-526">If you then run the website on Windows 7 without SP1 installed, IIS 7 will not include the required update.</span></span> <span data-ttu-id="ea72c-527">如此一來，路由將無法運作，且您會看到錯誤。</span><span class="sxs-lookup"><span data-stu-id="ea72c-527">As a consequence, routing will not work and you will see errors.</span></span> <span data-ttu-id="ea72c-528">如果您有任何問題，路由不會無法運作，您可以執行下列其中一個：</span><span class="sxs-lookup"><span data-stu-id="ea72c-528">If you have a problem where routing does not work, you can do either the following:</span></span>

- <span data-ttu-id="ea72c-529">更新 Windows 7 SP1，會將更新新增至 IIS 7。</span><span class="sxs-lookup"><span data-stu-id="ea72c-529">Update Windows 7 to SP1, which will add the update to IIS 7.</span></span>
- <span data-ttu-id="ea72c-530">在先前所列的 Microsoft 支援文章中，安裝所述的更新。</span><span class="sxs-lookup"><span data-stu-id="ea72c-530">Install the update that's described in the Microsoft Support article listed previously.</span></span>
- <span data-ttu-id="ea72c-531">設定**runAllManagedModulesForAllRequests**設為 true，該網站的 Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-531">Set **runAllManagedModulesForAllRequests** to true in that website's Web.config file.</span></span> <span data-ttu-id="ea72c-532">請注意，這會將一些額外負荷新增至要求。</span><span class="sxs-lookup"><span data-stu-id="ea72c-532">Note that this will add some overhead to requests.</span></span>

<a id="_Toc318097397"></a>
### <a name="html-editor"></a><span data-ttu-id="ea72c-533">HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-533">HTML Editor</span></span>

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a><span data-ttu-id="ea72c-534">智慧工作</span><span class="sxs-lookup"><span data-stu-id="ea72c-534">Smart Tasks</span></span>

<span data-ttu-id="ea72c-535">在 [設計] 檢視中，通常伺服器控制項的複雜屬性有相關的對話方塊和精靈，可讓您輕鬆地設定它們。</span><span class="sxs-lookup"><span data-stu-id="ea72c-535">In Design view, complex properties of server controls often have associated dialog boxes and wizards to make it easy to set them.</span></span> <span data-ttu-id="ea72c-536">比方說，您可以使用特殊的對話方塊中，加入至資料來源*Repeater*控制或將資料行加入*GridView*控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-536">For example, you can use a special dialog box to add a data source to a *Repeater* control or add columns to a *GridView* control.</span></span>

<span data-ttu-id="ea72c-537">不過，這種類型的複雜屬性 UI 說明已在來源檢視中可用。</span><span class="sxs-lookup"><span data-stu-id="ea72c-537">However, this type of UI help for complex properties has not been available in Source view.</span></span> <span data-ttu-id="ea72c-538">因此，Visual Studio 11 介紹智慧工作的來源檢視。</span><span class="sxs-lookup"><span data-stu-id="ea72c-538">Therefore, Visual Studio 11 introduces Smart Tasks for Source view.</span></span> <span data-ttu-id="ea72c-539">智慧工作是內容感知的捷徑，在 C# 和 Visual Basic 編輯器中的常用功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-539">Smart Tasks are context-aware shortcuts for commonly used features in the C# and Visual Basic editors.</span></span>

<span data-ttu-id="ea72c-540">ASP.NET Web Form 控制項的智慧工作時，會顯示伺服器標記為小型的圖像 （glyph） 的插入點項目內：</span><span class="sxs-lookup"><span data-stu-id="ea72c-540">For ASP.NET Web Forms controls, Smart Tasks appear on server tags as a small glyph when the insertion point is inside the element:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

<span data-ttu-id="ea72c-541">當您按一下圖像或按 CTRL +，則會展開 「 Smart Task 」。</span><span class="sxs-lookup"><span data-stu-id="ea72c-541">The Smart Task expands when you click the glyph or press CTRL+.</span></span> <span data-ttu-id="ea72c-542">（點），在程式碼編輯器中一樣。</span><span class="sxs-lookup"><span data-stu-id="ea72c-542">(dot), just as in the code editors.</span></span> <span data-ttu-id="ea72c-543">然後，它會顯示類似於智慧工作，在 [設計] 檢視中的快速鍵。</span><span class="sxs-lookup"><span data-stu-id="ea72c-543">It then displays shortcuts that are similar to the Smart Tasks in Design view.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

<span data-ttu-id="ea72c-544">比方說，「 Smart Task 」 在上圖中顯示 [GridView 工作] 選項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-544">For example, the Smart Task in the previous illustration shows the GridView Tasks options.</span></span> <span data-ttu-id="ea72c-545">如果您選擇編輯資料行時，會顯示下列對話方塊：</span><span class="sxs-lookup"><span data-stu-id="ea72c-545">If you choose Edit Columns, the following dialog box is displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

<span data-ttu-id="ea72c-546">填寫對話方塊會設定相同的屬性。 您可以設定在設計檢視中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-546">Filling in the dialog box sets the same properties you can set in Design view.</span></span> <span data-ttu-id="ea72c-547">當您按一下 [確定] 時，控制項的標記會更新以新的設定：</span><span class="sxs-lookup"><span data-stu-id="ea72c-547">When you click OK, the markup for the control is updated with the new settings:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a><span data-ttu-id="ea72c-548">等待 ARIA 支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-548">WAI-ARIA support</span></span>

<span data-ttu-id="ea72c-549">撰寫可存取的網站會變得越來越重要的作業。</span><span class="sxs-lookup"><span data-stu-id="ea72c-549">Writing accessible websites is becoming increasingly important.</span></span> <span data-ttu-id="ea72c-550">[WAI ARIA 協助工具標準](http://www.w3.org/WAI/intro/aria)定義開發人員應該如何撰寫可存取的網站。</span><span class="sxs-lookup"><span data-stu-id="ea72c-550">The [WAI-ARIA accessibility standard](http://www.w3.org/WAI/intro/aria) defines how developers should write accessible websites.</span></span> <span data-ttu-id="ea72c-551">在 Visual Studio 現在完全支援這項標準。</span><span class="sxs-lookup"><span data-stu-id="ea72c-551">This standard is now fully supported in Visual Studio.</span></span>

<span data-ttu-id="ea72c-552">例如，*角色*屬性現在會有完整的 IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="ea72c-552">For example, the *role* attribute now has full IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

<span data-ttu-id="ea72c-553">等待 ARIA 標準也導入了前面會加上的屬性*aria-* ，可讓您加入的 HTML5 文件中的語意。</span><span class="sxs-lookup"><span data-stu-id="ea72c-553">The WAI-ARIA standard also introduces attributes that are prefixed with *aria-* that let you add semantics to an HTML5 document.</span></span> <span data-ttu-id="ea72c-554">Visual Studio 也完全支援這些*aria-* 屬性：</span><span class="sxs-lookup"><span data-stu-id="ea72c-554">Visual Studio also fully supports these *aria-* attributes:</span></span>

<span data-ttu-id="ea72c-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="ea72c-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span></span>

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a><span data-ttu-id="ea72c-556">新的 HTML5 片段</span><span class="sxs-lookup"><span data-stu-id="ea72c-556">New HTML5 snippets</span></span>

<span data-ttu-id="ea72c-557">若要更快速且輕鬆地撰寫常用的 HTML5 標記，Visual Studio 會包含程式碼片段的數目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-557">To make it faster and easier to write commonly used HTML5 markup, Visual Studio includes a number of snippets.</span></span> <span data-ttu-id="ea72c-558">以下是影片的程式碼片段範例：</span><span class="sxs-lookup"><span data-stu-id="ea72c-558">An example is the video snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

<span data-ttu-id="ea72c-559">若要叫用程式碼片段，請按下 Tab 鍵會在 IntelliSense 中選取的項目時，兩次：</span><span class="sxs-lookup"><span data-stu-id="ea72c-559">To invoke the snippet, press Tab twice when the element is selected in IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

<span data-ttu-id="ea72c-560">這會產生您可以自訂的程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="ea72c-560">This produces a snippet that you can customize.</span></span>

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a><span data-ttu-id="ea72c-561">擷取至使用者控制項</span><span class="sxs-lookup"><span data-stu-id="ea72c-561">Extract to user control</span></span>

<span data-ttu-id="ea72c-562">在大型網頁中，它可以是個不錯的主意，將個別的項目移至使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-562">In large web pages, it can be a good idea to move individual pieces into user controls.</span></span> <span data-ttu-id="ea72c-563">這種形式的重構可協助提高網頁的可讀性，並可簡化頁面結構。</span><span class="sxs-lookup"><span data-stu-id="ea72c-563">This form of refactoring can help increase the readability of the page and can simplify the page structure.</span></span>

<span data-ttu-id="ea72c-564">為了簡化起見，當您編輯原始碼檢視中的 Web Form 網頁時，您可以現在在頁面中選取文字，以滑鼠右鍵按一下它，，然後選擇 擷取至使用者控制項：</span><span class="sxs-lookup"><span data-stu-id="ea72c-564">To make this easier, when you edit Web Forms pages in Source view, you can now select text in a page, right-click it, and then choose Extract to User Control:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a><span data-ttu-id="ea72c-565">在屬性中的程式碼區塊的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ea72c-565">IntelliSense for code nuggets in attributes</span></span>

<span data-ttu-id="ea72c-566">Visual Studio 的伺服器端程式碼區塊中的任何頁面或控制項一律提供 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="ea72c-566">Visual Studio has always provided IntelliSense for server-side code nuggets in any page or control.</span></span> <span data-ttu-id="ea72c-567">現在 Visual Studio 包含程式碼區塊，以及 HTML 屬性中的 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="ea72c-567">Now Visual Studio includes IntelliSense for code nuggets in HTML attributes as well.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

<span data-ttu-id="ea72c-568">這可讓您更輕鬆地建立資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="ea72c-568">This makes it easier to create data-binding expressions:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a><span data-ttu-id="ea72c-569">當您重新命名開頭或結尾標記相符的標記的自動重新命名</span><span class="sxs-lookup"><span data-stu-id="ea72c-569">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>

<span data-ttu-id="ea72c-570">如果您重新命名的 HTML 項目 (比方說，您的變更*div*標記要*標頭*標記)，則相對應的開啟或關閉標記也會即時變更。</span><span class="sxs-lookup"><span data-stu-id="ea72c-570">If you rename an HTML element (for example, you change a *div* tag to be a *header* tag), the corresponding opening or closing tag also changes in real time.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

<span data-ttu-id="ea72c-571">這有助於避免您忘了用來變更結尾標記，或變更了錯誤的錯誤。</span><span class="sxs-lookup"><span data-stu-id="ea72c-571">This helps avoid the error where you forget to change a closing tag or change the wrong one.</span></span>

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a><span data-ttu-id="ea72c-572">產生事件處理常式</span><span class="sxs-lookup"><span data-stu-id="ea72c-572">Event handler generation</span></span>

<span data-ttu-id="ea72c-573">Visual Studio 現在會包含可協助您撰寫事件處理常式，並以手動方式將它們繫結的來源檢視中的功能。</span><span class="sxs-lookup"><span data-stu-id="ea72c-573">Visual Studio now includes features in Source view to help you write event handlers and bind them manually.</span></span> <span data-ttu-id="ea72c-574">如果您要編輯原始碼檢視中的事件名稱，IntelliSense 會顯示&lt;建立新的事件&gt;，這會建立事件處理常式在頁面的程式碼中，具有正確的簽章：</span><span class="sxs-lookup"><span data-stu-id="ea72c-574">If you are editing an event name in Source view, IntelliSense displays &lt;Create New Event&gt;, which will create an event handler in the page's code that has the right signature:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

<span data-ttu-id="ea72c-575">根據預設，事件處理常式會使用控制項的 ID，事件處理方法的名稱：</span><span class="sxs-lookup"><span data-stu-id="ea72c-575">By default, the event handler will use the control's ID for the name of the event-handling method:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

<span data-ttu-id="ea72c-576">（在此情況下，在 C# 中)，產生的事件處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="ea72c-576">The resulting event handler will look like this (in this case, in C#):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a><span data-ttu-id="ea72c-577">智慧縮排</span><span class="sxs-lookup"><span data-stu-id="ea72c-577">Smart indent</span></span>

<span data-ttu-id="ea72c-578">當您按 Enter，空的 HTML 項目內時，編輯器會將插入點放在正確的位置：</span><span class="sxs-lookup"><span data-stu-id="ea72c-578">When you press Enter while inside an empty HTML element, the editor will put the insertion point in the right place:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

<span data-ttu-id="ea72c-579">如果您按下 Enter，在這個位置，結尾標記會向下移動，並縮排，以符合項目的開頭標記。</span><span class="sxs-lookup"><span data-stu-id="ea72c-579">If you press Enter in this location, the closing tag is moved down and indented to match the opening tag.</span></span> <span data-ttu-id="ea72c-580">插入點也會縮排：</span><span class="sxs-lookup"><span data-stu-id="ea72c-580">The insertion point is also indented:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="ea72c-581">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="ea72c-581">Auto-reduce statement completion</span></span>

<span data-ttu-id="ea72c-582">在 Visual Studio 目前篩選條件，根據您輸入的內容，以顯示只有相關的選項 [IntelliSense] 清單中：</span><span class="sxs-lookup"><span data-stu-id="ea72c-582">The IntelliSense list in Visual Studio now filters based on what you type so that it displays only relevant options:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

<span data-ttu-id="ea72c-583">IntelliSense 篩選也根據標題大小寫的 IntelliSense 清單中的個別文字。</span><span class="sxs-lookup"><span data-stu-id="ea72c-583">IntelliSense also filters based on the title case of the individual words in the IntelliSense list.</span></span> <span data-ttu-id="ea72c-584">例如，如果您輸入"dl"，則 dl 和 asp: DataList 將會顯示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-584">For example, if you type "dl", both dl and asp:DataList are displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

<span data-ttu-id="ea72c-585">這項功能可讓更快取得的已知的項目陳述式完成。</span><span class="sxs-lookup"><span data-stu-id="ea72c-585">This feature makes it faster to get statement completion for known elements.</span></span>

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a><span data-ttu-id="ea72c-586">JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-586">JavaScript Editor</span></span>

<span data-ttu-id="ea72c-587">Visual Studio 2012 Release Candidate 中的 JavaScript 編輯器是第一次，它可大幅提升使用 Visual Studio 中的 JavaScript 的經驗。</span><span class="sxs-lookup"><span data-stu-id="ea72c-587">The JavaScript editor in Visual Studio 2012 Release Candidate is completely new and it greatly improves the experience of working with JavaScript in Visual Studio.</span></span>

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a><span data-ttu-id="ea72c-588">程式碼大綱</span><span class="sxs-lookup"><span data-stu-id="ea72c-588">Code outlining</span></span>

<span data-ttu-id="ea72c-589">大綱區域現在會自動建立的所有函式，可讓您摺疊的檔案不是與您目前的焦點相關的組件。</span><span class="sxs-lookup"><span data-stu-id="ea72c-589">Outlining regions are now automatically created for all functions, allowing you to collapse parts of the file that aren't pertinent to your current focus.</span></span>

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a><span data-ttu-id="ea72c-590">括號對稱</span><span class="sxs-lookup"><span data-stu-id="ea72c-590">Brace matching</span></span>

<span data-ttu-id="ea72c-591">當您將插入點放在開頭或結尾大括號上時，編輯器會反白顯示相符的一個。</span><span class="sxs-lookup"><span data-stu-id="ea72c-591">When you put the insertion point on an opening or closing brace, the editor highlights the matching one.</span></span>

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a><span data-ttu-id="ea72c-592">移至定義</span><span class="sxs-lookup"><span data-stu-id="ea72c-592">Go to Definition</span></span>

<span data-ttu-id="ea72c-593">[移至定義] 命令可讓您直接跳至函式或變數的來源。</span><span class="sxs-lookup"><span data-stu-id="ea72c-593">The Go to Definition command lets you jump to the source for a function or variable.</span></span>

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a><span data-ttu-id="ea72c-594">支援 ECMAScript5</span><span class="sxs-lookup"><span data-stu-id="ea72c-594">ECMAScript5 support</span></span>

<span data-ttu-id="ea72c-595">編輯器支援 ECMAScript5，說明 JavaScript 語言的標準的最新版本的新語法和 Api。</span><span class="sxs-lookup"><span data-stu-id="ea72c-595">The editor supports the new syntax and APIs in ECMAScript5, the latest version of the standard that describes the JavaScript language.</span></span>

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a><span data-ttu-id="ea72c-596">DOM 的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="ea72c-596">DOM IntelliSense</span></span>

<span data-ttu-id="ea72c-597">已改善 IntelliSense DOM api，並支援許多新 HTML5 Api 包括*querySelector*，DOM 儲存、 跨文件訊息，並*畫布*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-597">IntelliSense for DOM APIs has been improved, with support for many new HTML5 APIs including *querySelector*, DOM Storage, cross-document messaging, and *canvas*.</span></span> <span data-ttu-id="ea72c-598">單一的簡單 JavaScript 檔案，而不是由原生型別程式庫定義，現在驅動 DOM IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="ea72c-598">DOM IntelliSense is now driven by a single simple JavaScript file, rather than by a native type library definition.</span></span> <span data-ttu-id="ea72c-599">這可讓您輕鬆地擴充或取代。</span><span class="sxs-lookup"><span data-stu-id="ea72c-599">This makes it easy to extend or replace.</span></span>

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a><span data-ttu-id="ea72c-600">VSDOC 簽章的多載</span><span class="sxs-lookup"><span data-stu-id="ea72c-600">VSDOC signature overloads</span></span>

<span data-ttu-id="ea72c-601">詳細的 IntelliSense 註解可以現在為宣告不同的 JavaScript 函式多載使用新*&lt;簽章&gt;* 項目，在此範例中所示：</span><span class="sxs-lookup"><span data-stu-id="ea72c-601">Detailed IntelliSense comments can now be declared for separate overloads of JavaScript functions by using the new *&lt;signature&gt;* element, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a><span data-ttu-id="ea72c-602">隱含的參考</span><span class="sxs-lookup"><span data-stu-id="ea72c-602">Implicit references</span></span>

<span data-ttu-id="ea72c-603">您現在可以將 JavaScript 檔案，加入將會以隱含方式包含在檔案清單中任何特定的 JavaScript 檔案] 或 [封鎖的參考，這表示您會收到 IntelliSense，為其內容的集中清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-603">You can now add JavaScript files to a central list that will be implicitly included in the list of files that any given JavaScript file or block references, meaning you'll get IntelliSense for its contents.</span></span> <span data-ttu-id="ea72c-604">比方說，您可以將 jQuery 檔案新增至中央檔案的清單，而得到 IntelliSense jQuery 函式中的檔案，任何 JavaScript 區塊是否已明確參考 (使用 / /&lt;參考 /&gt;) 與否。</span><span class="sxs-lookup"><span data-stu-id="ea72c-604">For example, you can add jQuery files to the central list of files, and you'll get IntelliSense for jQuery functions in any JavaScript block of file, whether you've referenced it explicitly (using /// &lt;reference /&gt;) or not.</span></span>

<a id="_Toc318097415"></a>
### <a name="css-editor"></a><span data-ttu-id="ea72c-605">CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="ea72c-605">CSS Editor</span></span>

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="ea72c-606">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="ea72c-606">Auto-reduce statement completion</span></span>

<span data-ttu-id="ea72c-607">IntelliSense 對 CSS 屬性為基礎的 CSS 現在篩選和選取的結構描述所支援的值清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-607">The IntelliSense list for CSS now filters based on the CSS properties and values supported by the selected schema.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

<span data-ttu-id="ea72c-608">IntelliSense 也支援標題大小寫的搜尋：</span><span class="sxs-lookup"><span data-stu-id="ea72c-608">IntelliSense also supports title case searches:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a><span data-ttu-id="ea72c-609">階層式縮排</span><span class="sxs-lookup"><span data-stu-id="ea72c-609">Hierarchical indentation</span></span>

<span data-ttu-id="ea72c-610">CSS 編輯器會使用縮排顯示階層式的規則，讓您的階層式的規則以邏輯方式的組織方式概觀。</span><span class="sxs-lookup"><span data-stu-id="ea72c-610">The CSS editor uses indentation to display hierarchical rules, which gives you an overview of how the cascading rules are logically organized.</span></span> <span data-ttu-id="ea72c-611">在下列範例中，#list 選取器是階層式清單的子系，因此縮排。</span><span class="sxs-lookup"><span data-stu-id="ea72c-611">In the following example, the #list a selector is a cascading child of list and is therefore indented.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

<span data-ttu-id="ea72c-612">下列範例會示範更複雜的繼承：</span><span class="sxs-lookup"><span data-stu-id="ea72c-612">The following example shows more complex inheritance:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

<span data-ttu-id="ea72c-613">規則的縮排是由其父代規則決定。</span><span class="sxs-lookup"><span data-stu-id="ea72c-613">The indentation of a rule is determined by its parent rules.</span></span> <span data-ttu-id="ea72c-614">根據預設，啟用階層式縮排，但您可以停用 [選項] 對話方塊中 （工具，從功能表列的選項）：</span><span class="sxs-lookup"><span data-stu-id="ea72c-614">Hierarchical indentation is enabled by default, but you can disable it the Options dialog box (Tools, Options from the menu bar):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a><span data-ttu-id="ea72c-615">CSS 區隔設計支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-615">CSS hacks support</span></span>

<span data-ttu-id="ea72c-616">數百個真實世界的 CSS 檔案的分析顯示 CSS 區隔設計是很常見，，和 Visual Studio 現在支援最廣泛使用的項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-616">Analysis of hundreds of real-world CSS files shows that CSS hacks are very common, and now Visual Studio supports the most widely used ones.</span></span> <span data-ttu-id="ea72c-617">這項支援包括 IntelliSense 和驗證的星號 (\*) 和底線 (\_) 屬性區隔設計：</span><span class="sxs-lookup"><span data-stu-id="ea72c-617">This support includes IntelliSense and validation of the star (\*) and underscore (\_) property hacks:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

<span data-ttu-id="ea72c-618">也支援一般的選取器駭客攻擊，如此即使套用維護階層式縮排。</span><span class="sxs-lookup"><span data-stu-id="ea72c-618">Typical selector hacks are also supported so that hierarchical indentation is maintained even when they are applied.</span></span> <span data-ttu-id="ea72c-619">目標 Internet Explorer 7 使用一般的選取器駭客是前面加上使用的選取器 *\*: nth-child(1 + html*。</span><span class="sxs-lookup"><span data-stu-id="ea72c-619">A typical selector hack used to target Internet Explorer 7 is to prepend a selector with *\*:first-child + html*.</span></span> <span data-ttu-id="ea72c-620">使用該規則會維護階層式縮排：</span><span class="sxs-lookup"><span data-stu-id="ea72c-620">Using that rule will maintain the hierarchical indentation:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a><span data-ttu-id="ea72c-621">廠商特定的結構描述 (-moz-，-webkit)</span><span class="sxs-lookup"><span data-stu-id="ea72c-621">Vendor specific schemas (-moz-, -webkit)</span></span>

<span data-ttu-id="ea72c-622">CSS3 導入了許多由實作過不同瀏覽器在不同時間的屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-622">CSS3 introduces many properties that have been implemented by different browsers at different times.</span></span> <span data-ttu-id="ea72c-623">先前，這會強制開發人員可以針對特定瀏覽器的程式碼使用廠商特有的語法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-623">This previously forced developers to code for specific browsers by using vendor-specific syntax.</span></span> <span data-ttu-id="ea72c-624">在 IntelliSense 中現在包含這些瀏覽器特定的屬性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-624">These browser-specific properties are now included in IntelliSense.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a><span data-ttu-id="ea72c-625">註解和取消註解的支援</span><span class="sxs-lookup"><span data-stu-id="ea72c-625">Commenting and uncommenting support</span></span>

<span data-ttu-id="ea72c-626">您現在可以註解，並使用相同的快速鍵 (Ctrl + K、 註解的 C 和 Ctrl + K、 您取消註解） 程式碼編輯器中使用的 CSS 規則取消註解。</span><span class="sxs-lookup"><span data-stu-id="ea72c-626">You can now comment and uncomment CSS rules using the same shortcuts that you use in the code editor (Ctrl+K,C to comment and Ctrl+K,U to uncomment).</span></span>

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a><span data-ttu-id="ea72c-627">色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="ea72c-627">Color picker</span></span>

<span data-ttu-id="ea72c-628">在舊版的 Visual Studio 中，IntelliSense 的色彩相關的屬性包含具名的色彩值的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-628">In previous versions of Visual Studio, IntelliSense for color-related attributes consisted of a drop-down list of named color values.</span></span> <span data-ttu-id="ea72c-629">該清單已取代的功能完整的色彩選擇器。</span><span class="sxs-lookup"><span data-stu-id="ea72c-629">That list has been replaced by a full-featured color picker.</span></span>

<span data-ttu-id="ea72c-630">當您輸入的色彩值時，色彩選擇器會自動顯示，並提供先前使用後面接著預設色彩調色盤的色彩的清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-630">When you enter a color value, the color picker is displayed automatically and presents a list of previously used colors followed by a default color palette.</span></span> <span data-ttu-id="ea72c-631">您可以選取色彩，使用滑鼠或鍵盤。</span><span class="sxs-lookup"><span data-stu-id="ea72c-631">You can select a color using the mouse or the keyboard.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

<span data-ttu-id="ea72c-632">到完整的色彩選擇器，即可展開清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-632">The list can be expanded into a complete color picker.</span></span> <span data-ttu-id="ea72c-633">選擇器可讓您控制 alpha 色頻所移動的不透明度滑桿時，會自動將任何色彩轉換成 RGBA:</span><span class="sxs-lookup"><span data-stu-id="ea72c-633">The picker lets you control the alpha channel by automatically converting any color into RGBA when you move the opacity slider:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a><span data-ttu-id="ea72c-634">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="ea72c-634">Snippets</span></span>

<span data-ttu-id="ea72c-635">CSS 編輯器中的程式碼片段可讓您更輕鬆且快速地建立跨瀏覽器樣式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-635">Snippets in the CSS editor make it easier and faster to create cross-browser styles.</span></span> <span data-ttu-id="ea72c-636">需要特定的瀏覽器設定的許多 CSS3 屬性現在已回復到程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="ea72c-636">Many CSS3 properties that require browser-specific settings have now been rolled into snippets.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

<span data-ttu-id="ea72c-637">CSS 程式碼片段支援進階的案例中 （例如 CSS3 媒體查詢） 輸入 at 符號 (@)，這會顯示 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="ea72c-637">CSS snippets support advanced scenarios (like CSS3 media queries) by typing the at-symbol (@), which shows the IntelliSense list.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

<span data-ttu-id="ea72c-638">當您選取@media值並按下 Tab 鍵，CSS 編輯器會插入下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="ea72c-638">When you select @media value and press Tab, the CSS editor inserts the following snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

<span data-ttu-id="ea72c-639">如同程式碼的程式碼片段，您可以建立您自己的 CSS 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="ea72c-639">As with snippets for code, you can create your own CSS snippets.</span></span>

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a><span data-ttu-id="ea72c-640">自訂區域</span><span class="sxs-lookup"><span data-stu-id="ea72c-640">Custom regions</span></span>

<span data-ttu-id="ea72c-641">名為程式碼區域，已在程式碼編輯器中，已可供編輯 CSS。</span><span class="sxs-lookup"><span data-stu-id="ea72c-641">Named code regions, which are already available in the code editor, are now available for CSS editing.</span></span> <span data-ttu-id="ea72c-642">這可讓您輕鬆地為群組相關的樣式區塊。</span><span class="sxs-lookup"><span data-stu-id="ea72c-642">This lets you easily group related style blocks.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

<span data-ttu-id="ea72c-643">摺疊的區域時它會顯示區域的名稱：</span><span class="sxs-lookup"><span data-stu-id="ea72c-643">When a region is collapsed it displays the name of the region:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a><span data-ttu-id="ea72c-644">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="ea72c-644">Page Inspector</span></span>

<span data-ttu-id="ea72c-645">Page Inspector 是一種工具可呈現在 Visual Studio IDE 中的 web 頁面 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁），並可讓您檢查原始程式碼和產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="ea72c-645">Page Inspector is a tool that renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) in the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="ea72c-646">適用於 ASP.NET 網頁，Page Inspector 可讓您判斷哪些伺服器端程式碼所產生的 HTML 標記呈現至瀏覽器。</span><span class="sxs-lookup"><span data-stu-id="ea72c-646">For ASP.NET pages, Page Inspector lets you determine which server-side code has produced the HTML markup that is rendered to the browser.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

<span data-ttu-id="ea72c-647">如需有關 Page Inspector 的詳細資訊，請參閱下列教學課程：</span><span class="sxs-lookup"><span data-stu-id="ea72c-647">For more information about Page Inspector, please see the following tutorials:</span></span>

- <span data-ttu-id="ea72c-648">使用中的 Page Inspector [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span><span class="sxs-lookup"><span data-stu-id="ea72c-648">Using Page Inspector in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span></span>
- <span data-ttu-id="ea72c-649">使用中的 Page Inspector [ASP.NET Web Form](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span><span class="sxs-lookup"><span data-stu-id="ea72c-649">Using Page Inspector in [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span></span>

<a id="_Toc318097425"></a>
### <a name="publishing"></a><span data-ttu-id="ea72c-650">發佈</span><span class="sxs-lookup"><span data-stu-id="ea72c-650">Publishing</span></span>

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a><span data-ttu-id="ea72c-651">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="ea72c-651">Publish profiles</span></span>

<span data-ttu-id="ea72c-652">在 Visual Studio 2010 中，針對 Web 應用程式專案的發佈資訊不會儲存在版本控制，而且不是針對與其他人共用。</span><span class="sxs-lookup"><span data-stu-id="ea72c-652">In Visual Studio 2010, publishing information for Web application projects is not stored in version control and is not designed for sharing with others.</span></span> <span data-ttu-id="ea72c-653">Visual Studio 2012 發行候選版本中，發行設定檔的格式已經變更。</span><span class="sxs-lookup"><span data-stu-id="ea72c-653">In Visual Studio 2012 Release Candidate, the format of the publish profile has been changed.</span></span> <span data-ttu-id="ea72c-654">已在進行小組的成品，並就現在可以輕鬆地利用的 MSBuild 為基礎的組建。</span><span class="sxs-lookup"><span data-stu-id="ea72c-654">It has been made a team artifact, and it is now easy to leverage from builds based on MSBuild.</span></span> <span data-ttu-id="ea72c-655">組建組態資訊都位於 [發行] 對話方塊中，以便您可以輕鬆地切換之前發行的組建組態。</span><span class="sxs-lookup"><span data-stu-id="ea72c-655">Build configuration information is in the Publish dialog box so that you can easily switch build configurations before publishing.</span></span>

<span data-ttu-id="ea72c-656">發行設定檔會儲存在 [PublishProfiles] 資料夾。</span><span class="sxs-lookup"><span data-stu-id="ea72c-656">Publish profiles are stored in the PublishProfiles folder.</span></span> <span data-ttu-id="ea72c-657">資料夾的位置取決於您使用何種程式設計語言：</span><span class="sxs-lookup"><span data-stu-id="ea72c-657">The location of the folder depends on what programming language you are using:</span></span>

- <span data-ttu-id="ea72c-658">C#: Properties\PublishProfiles</span><span class="sxs-lookup"><span data-stu-id="ea72c-658">C#: Properties\PublishProfiles</span></span>
- <span data-ttu-id="ea72c-659">我 Project\PublishProfiles Visual Basic:</span><span class="sxs-lookup"><span data-stu-id="ea72c-659">Visual Basic: My Project\PublishProfiles</span></span>

<span data-ttu-id="ea72c-660">每個設定檔是 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-660">Each profile is an MSBuild file.</span></span> <span data-ttu-id="ea72c-661">期間發行，此檔案會匯入專案的 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="ea72c-661">During publishing, this file is imported into the project's MSBuild file.</span></span> <span data-ttu-id="ea72c-662">在 Visual Studio 2010 中，如果您想要變更的發行或封裝的程序，您必須將您的自訂項目放在名為**ProjectName**.wpp.targets。</span><span class="sxs-lookup"><span data-stu-id="ea72c-662">In Visual Studio 2010, if you want to make changes to the publish or package process, you have to put your customizations in a file named **ProjectName**.wpp.targets.</span></span> <span data-ttu-id="ea72c-663">仍支援這樣設定，但現在，您可以將您的自訂項目放在本身的發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="ea72c-663">This is still supported, but you can now put your customizations in the publish profile itself.</span></span> <span data-ttu-id="ea72c-664">如此一來，您將只為該設定檔使用自訂項目。</span><span class="sxs-lookup"><span data-stu-id="ea72c-664">That way, the customizations will be used only for that profile.</span></span>

<span data-ttu-id="ea72c-665">您可以現在也利用會發行從 MSBuild 的設定檔。</span><span class="sxs-lookup"><span data-stu-id="ea72c-665">You can now also leverage publish profiles from MSBuild.</span></span> <span data-ttu-id="ea72c-666">若要這樣做，請使用下列命令，當您建置專案：</span><span class="sxs-lookup"><span data-stu-id="ea72c-666">To do so, use the following command when you build the project:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

<span data-ttu-id="ea72c-667">Project.csproj 值是路徑的專案，而且 ProfileName 是發行設定檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="ea72c-667">The project.csproj value is the path of the project, and ProfileName is the name of the profile to publish.</span></span> <span data-ttu-id="ea72c-668">或者，而不是傳遞的設定檔名稱*PublishProfile*屬性，您可以傳入的完整路徑的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="ea72c-668">Alternatively, instead of passing the profile name for the *PublishProfile* property, you can pass in the full path to the publish profile.</span></span>

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a><span data-ttu-id="ea72c-669">ASP.NET 先行編譯和合併</span><span class="sxs-lookup"><span data-stu-id="ea72c-669">ASP.NET precompilation and merge</span></span>

<span data-ttu-id="ea72c-670">Web 應用程式專案的 Visual Studio 2012 Release Candidate 將加入 封裝/發行 Web 屬性頁可讓您先行編譯，以及合併站台的內容，當您發行或封裝的專案上的選項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-670">For Web application projects, Visual Studio 2012 Release Candidate adds an option on the Package/Publish Web properties page that lets you precompile and merge your site's content when you publish or package the project.</span></span> <span data-ttu-id="ea72c-671">若要查看這些選項，以滑鼠右鍵按一下方案總管 中的專案，選擇 內容，然後選擇 封裝/發行 Web 屬性頁。</span><span class="sxs-lookup"><span data-stu-id="ea72c-671">To see these options, right-click the project in Solution Explorer, choose Properties, and then choose the Package/Publish Web property page.</span></span> <span data-ttu-id="ea72c-672">下圖顯示預先編譯此應用程式，再發行選項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-672">The following illustration shows the Precompile this application before publishing option.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

<span data-ttu-id="ea72c-673">選取此選項時，Visual Studio 會預先編譯的應用程式，每當您發行或封裝的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="ea72c-673">When this option is selected, Visual Studio precompiles the application whenever you publish or package the web application.</span></span> <span data-ttu-id="ea72c-674">如果您想要控制如何先行編譯網站時，或如何合併組件，請按一下 [進階] 按鈕來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="ea72c-674">If you want to control how the site is precompiled or how assemblies are merged, click the Advanced button to configure those options.</span></span>

<a id="_Toc318097428"></a>
### <a name="iis-express"></a><span data-ttu-id="ea72c-675">IIS Express</span><span class="sxs-lookup"><span data-stu-id="ea72c-675">IIS Express</span></span>

<span data-ttu-id="ea72c-676">Visual Studio 中測試的 web 專案的預設 web 伺服器現在是 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="ea72c-676">The default web server for testing web projects in Visual Studio is now IIS Express.</span></span> <span data-ttu-id="ea72c-677">Visual Studio 程式開發伺服器仍在開發期間，本機 web 伺服器的選項，但 IIS Express 現在是建議的伺服器。</span><span class="sxs-lookup"><span data-stu-id="ea72c-677">The Visual Studio Development Server is still an option for local web server during development, but IIS Express is now the recommended server.</span></span> <span data-ttu-id="ea72c-678">使用 Visual Studio 11 Beta 中的 IIS Express 的經驗是非常類似於在 Visual Studio 2010 SP1 中使用它。</span><span class="sxs-lookup"><span data-stu-id="ea72c-678">The experience of using IIS Express in Visual Studio 11 Beta is very similar to using it in Visual Studio 2010 SP1.</span></span>

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a><span data-ttu-id="ea72c-679">免責聲明</span><span class="sxs-lookup"><span data-stu-id="ea72c-679">Disclaimer</span></span>

<span data-ttu-id="ea72c-680">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="ea72c-680">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="ea72c-681">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="ea72c-681">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="ea72c-682">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="ea72c-682">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="ea72c-683">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="ea72c-683">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="ea72c-684">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="ea72c-684">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="ea72c-685">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="ea72c-685">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="ea72c-686">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="ea72c-686">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="ea72c-687">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="ea72c-687">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="ea72c-688">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="ea72c-688">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="ea72c-689">除非另有說明，範例公司、 組織、 產品、 網域名稱、 電子郵件地址、 標誌、 人物、 地點及事件本文所述之屬虛構，以及與任何真實公司、 組織、 產品、 網域名稱、 電子郵件沒有關聯地址、 標誌、 人員、 位置或事件是純屬巧合。</span><span class="sxs-lookup"><span data-stu-id="ea72c-689">Unless otherwise noted, the example companies, organizations, products, domain names, email addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="ea72c-690">(C) 2012 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="ea72c-690">© 2012 Microsoft Corporation.</span></span> <span data-ttu-id="ea72c-691">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="ea72c-691">All rights reserved.</span></span>

<span data-ttu-id="ea72c-692">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="ea72c-692">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="ea72c-693">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="ea72c-693">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
