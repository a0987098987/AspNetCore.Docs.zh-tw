---
uid: whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
title: "新功能 ASP.NET 4.5 和 Visual Studio 2012 |Microsoft 文件"
author: rick-anderson
description: "本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。 它也會描述針對 web 開發所進行的增強功能..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/29/2012
ms.topic: article
ms.assetid: ba1fabb4-31a3-4ebf-8327-41a6bbba6eaf
ms.technology: 
ms.prod: .net-framework
msc.legacyurl: /whitepapers/whats-new-in-aspnet-45-and-visual-studio-2012
msc.type: content
ms.openlocfilehash: 93fdc7ca241198dc1d7c4c1f6be0a61b15790039
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="whats-new-in-aspnet-45-and-visual-studio-2012"></a><span data-ttu-id="d4534-104">什麼是 ASP.NET 4.5 和 Visual Studio 2012 的新功能</span><span class="sxs-lookup"><span data-stu-id="d4534-104">What's New in ASP.NET 4.5 and Visual Studio 2012</span></span>
====================
> <span data-ttu-id="d4534-105">本文件說明新功能和 ASP.NET 4.5 中引進的增強功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-105">This document describes new features and enhancements that are being introduced in ASP.NET 4.5.</span></span> <span data-ttu-id="d4534-106">它也會描述正在進行 Visual Studio 2012 中 web 程式開發的增強功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-106">It also describes improvements being made for web development in Visual Studio 2012.</span></span> <span data-ttu-id="d4534-107">這份文件最初發行於 2012 年 2 月 29 日。</span><span class="sxs-lookup"><span data-stu-id="d4534-107">This document was originally published on February 29, 2012.</span></span>


- [<span data-ttu-id="d4534-108">ASP.NET Core 執行階段和架構</span><span class="sxs-lookup"><span data-stu-id="d4534-108">ASP.NET Core Runtime and Framework</span></span>](#_Toc318097372)

    - [<span data-ttu-id="d4534-109">以非同步方式讀取和寫入 HTTP 要求和回應</span><span class="sxs-lookup"><span data-stu-id="d4534-109">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>](#_Toc318097373)
    - [<span data-ttu-id="d4534-110">HttpRequest 處理的增強功能</span><span class="sxs-lookup"><span data-stu-id="d4534-110">Improvements to HttpRequest handling</span></span>](#_Toc318097374)
    - [<span data-ttu-id="d4534-111">非同步清除回應</span><span class="sxs-lookup"><span data-stu-id="d4534-111">Asynchronously flushing a response</span></span>](#_Toc318097375)
    - [<span data-ttu-id="d4534-112">支援*await*和*工作*為基礎的非同步模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="d4534-112">Support for *await* and *Task*-Based Asynchronous Modules and Handlers</span></span>](#_Toc318097376)
    - [<span data-ttu-id="d4534-113">非同步 HTTP 模組</span><span class="sxs-lookup"><span data-stu-id="d4534-113">Asynchronous HTTP modules</span></span>](#_Toc318097377)
    - [<span data-ttu-id="d4534-114">非同步 HTTP 處理常式</span><span class="sxs-lookup"><span data-stu-id="d4534-114">Asynchronous HTTP handlers</span></span>](#_Toc318097378)
    - [<span data-ttu-id="d4534-115">ASP.NET 要求驗證的新功能</span><span class="sxs-lookup"><span data-stu-id="d4534-115">New ASP.NET Request Validation Features</span></span>](#_Toc318097379)
    - [<span data-ttu-id="d4534-116">延遲 （「 延遲 」） 的要求驗證</span><span class="sxs-lookup"><span data-stu-id="d4534-116">Deferred ("lazy") request validation</span></span>](#_Toc318097380)
    - [<span data-ttu-id="d4534-117">未驗證要求的支援</span><span class="sxs-lookup"><span data-stu-id="d4534-117">Support for unvalidated requests</span></span>](#_Toc318097381)
    - [<span data-ttu-id="d4534-118">AntiXSS 程式庫</span><span class="sxs-lookup"><span data-stu-id="d4534-118">AntiXSS Library</span></span>](#_Toc318097382)
    - [<span data-ttu-id="d4534-119">支援的 Websocket 通訊協定</span><span class="sxs-lookup"><span data-stu-id="d4534-119">Support for WebSockets Protocol</span></span>](#_Toc318097383)
    - [<span data-ttu-id="d4534-120">統合及縮製</span><span class="sxs-lookup"><span data-stu-id="d4534-120">Bundling and Minification</span></span>](#_Toc318097384)
    - [<span data-ttu-id="d4534-121">適用於虛擬主機的效能改進</span><span class="sxs-lookup"><span data-stu-id="d4534-121">Performance Improvements for Web Hosting</span></span>](#_Toc_perf)

        - [<span data-ttu-id="d4534-122">關鍵效能因素</span><span class="sxs-lookup"><span data-stu-id="d4534-122">Key Performance Factors</span></span>](#_Toc_perf_1)
        - [<span data-ttu-id="d4534-123">新的效能功能的需求</span><span class="sxs-lookup"><span data-stu-id="d4534-123">Requirements for New Performance Features</span></span>](#_Toc_perf_2)
        - [<span data-ttu-id="d4534-124">共用通用的組件</span><span class="sxs-lookup"><span data-stu-id="d4534-124">Sharing Common Assemblies</span></span>](#_Toc_perf_3)
        - [<span data-ttu-id="d4534-125">使用多核心 JIT 編譯的更快速啟動</span><span class="sxs-lookup"><span data-stu-id="d4534-125">Using multi-Core JIT compilation for faster startup</span></span>](#_Toc_perf_4)
        - [<span data-ttu-id="d4534-126">調整以最佳化記憶體回收</span><span class="sxs-lookup"><span data-stu-id="d4534-126">Tuning garbage collection to optimize for memory</span></span>](#_Toc_perf_5)
        - [<span data-ttu-id="d4534-127">預先擷取 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d4534-127">Prefetching for web applications</span></span>](#_Toc_perf_6)
- [<span data-ttu-id="d4534-128">ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="d4534-128">ASP.NET Web Forms</span></span>](#_Toc318097385)

    - [<span data-ttu-id="d4534-129">強型別的資料控制項</span><span class="sxs-lookup"><span data-stu-id="d4534-129">Strongly Typed Data Controls</span></span>](#_Toc318097386)
    - [<span data-ttu-id="d4534-130">模型繫結</span><span class="sxs-lookup"><span data-stu-id="d4534-130">Model Binding</span></span>](#_Toc318097387)

        - [<span data-ttu-id="d4534-131">選取資料</span><span class="sxs-lookup"><span data-stu-id="d4534-131">Selecting data</span></span>](#_Toc318097388)
        - [<span data-ttu-id="d4534-132">值提供者</span><span class="sxs-lookup"><span data-stu-id="d4534-132">Value providers</span></span>](#_Toc318097389)
        - [<span data-ttu-id="d4534-133">篩選控制項中的值</span><span class="sxs-lookup"><span data-stu-id="d4534-133">Filtering by values from a control</span></span>](#_Toc318097390)
    - [<span data-ttu-id="d4534-134">HTML 編碼的資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="d4534-134">HTML Encoded Data-Binding Expressions</span></span>](#_Toc318097391)
    - [<span data-ttu-id="d4534-135">不顯眼的驗證</span><span class="sxs-lookup"><span data-stu-id="d4534-135">Unobtrusive Validation</span></span>](#_Toc318097392)
    - [<span data-ttu-id="d4534-136">HTML5 更新</span><span class="sxs-lookup"><span data-stu-id="d4534-136">HTML5 Updates</span></span>](#_Toc318097393)
- [<span data-ttu-id="d4534-137">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d4534-137">ASP.NET MVC 4</span></span>](#_Toc318097394)
- [<span data-ttu-id="d4534-138">ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="d4534-138">ASP.NET Web Pages 2</span></span>](#_Toc318097395)
- [<span data-ttu-id="d4534-139">Visual Studio 2012 發行候選版本</span><span class="sxs-lookup"><span data-stu-id="d4534-139">Visual Studio 2012 Release Candidate</span></span>](#_Toc318097396)

    - [<span data-ttu-id="d4534-140">Visual Studio 2010 和 Visual Studio 2012 發行候選版本 （專案相容性） 之間共用的專案</span><span class="sxs-lookup"><span data-stu-id="d4534-140">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>](#project-compatibility)
    - [<span data-ttu-id="d4534-141">ASP.NET 4.5 的網站範本中的組態變更</span><span class="sxs-lookup"><span data-stu-id="d4534-141">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>](#Configuration_Changes_In_ASPNET45_Website_Templates)
    - [<span data-ttu-id="d4534-142">在 IIS 7 進行 ASP.NET 路由中的原生支援</span><span class="sxs-lookup"><span data-stu-id="d4534-142">Native Support in IIS 7 for ASP.NET Routing</span></span>](#Native_Support_In_IIS7_For_ASPNET_Routine)
    - [<span data-ttu-id="d4534-143">HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-143">HTML Editor</span></span>](#_Toc318097397)

        - [<span data-ttu-id="d4534-144">智慧工作提示</span><span class="sxs-lookup"><span data-stu-id="d4534-144">Smart Tasks</span></span>](#_Toc318097398)
        - [<span data-ttu-id="d4534-145">WAI ARIA 支援</span><span class="sxs-lookup"><span data-stu-id="d4534-145">WAI-ARIA support</span></span>](#_Toc318097399)
        - [<span data-ttu-id="d4534-146">新的 HTML5 程式碼片段</span><span class="sxs-lookup"><span data-stu-id="d4534-146">New HTML5 snippets</span></span>](#_Toc318097400)
        - [<span data-ttu-id="d4534-147">擷取至使用者控制項</span><span class="sxs-lookup"><span data-stu-id="d4534-147">Extract to user control</span></span>](#_Toc318097401)
        - [<span data-ttu-id="d4534-148">屬性中的程式碼區塊的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d4534-148">IntelliSense for code nuggets in attributes</span></span>](#_Toc318097402)
        - [<span data-ttu-id="d4534-149">當您重新命名的開頭或結尾標記時，比對標記的自動重新命名</span><span class="sxs-lookup"><span data-stu-id="d4534-149">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>](#_Toc318097403)
        - [<span data-ttu-id="d4534-150">事件處理常式產生</span><span class="sxs-lookup"><span data-stu-id="d4534-150">Event handler generation</span></span>](#_Toc318097404)
        - [<span data-ttu-id="d4534-151">智慧型縮排</span><span class="sxs-lookup"><span data-stu-id="d4534-151">Smart indent</span></span>](#_Toc318097405)
        - [<span data-ttu-id="d4534-152">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="d4534-152">Auto-reduce statement completion</span></span>](#_Toc318097406)
    - [<span data-ttu-id="d4534-153">JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-153">JavaScript Editor</span></span>](#_Toc318097407)

        - [<span data-ttu-id="d4534-154">程式碼大綱</span><span class="sxs-lookup"><span data-stu-id="d4534-154">Code outlining</span></span>](#_Toc318097408)
        - [<span data-ttu-id="d4534-155">括號對稱</span><span class="sxs-lookup"><span data-stu-id="d4534-155">Brace matching</span></span>](#_Toc318097409)
        - [<span data-ttu-id="d4534-156">移至定義</span><span class="sxs-lookup"><span data-stu-id="d4534-156">Go to Definition</span></span>](#_Toc318097410)
        - [<span data-ttu-id="d4534-157">ECMAScript5 支援</span><span class="sxs-lookup"><span data-stu-id="d4534-157">ECMAScript5 support</span></span>](#_Toc318097411)
        - [<span data-ttu-id="d4534-158">DOM IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d4534-158">DOM IntelliSense</span></span>](#_Toc318097412)
        - [<span data-ttu-id="d4534-159">VSDOC 簽章的多載</span><span class="sxs-lookup"><span data-stu-id="d4534-159">VSDOC signature overloads</span></span>](#_Toc318097413)
        - [<span data-ttu-id="d4534-160">隱含參考</span><span class="sxs-lookup"><span data-stu-id="d4534-160">Implicit references</span></span>](#_Toc318097414)
    - [<span data-ttu-id="d4534-161">CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-161">CSS Editor</span></span>](#_Toc318097415)

        - [<span data-ttu-id="d4534-162">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="d4534-162">Auto-reduce statement completion</span></span>](#_Toc318097416)
        - [<span data-ttu-id="d4534-163">階層式縮排。</span><span class="sxs-lookup"><span data-stu-id="d4534-163">Hierarchical indentation.</span></span>](#_Toc318097417)
        - [<span data-ttu-id="d4534-164">CSS 缺失支援</span><span class="sxs-lookup"><span data-stu-id="d4534-164">CSS hacks support</span></span>](#_Toc318097418)
        - [<span data-ttu-id="d4534-165">廠商特定的結構描述 (-moz--，webkit)</span><span class="sxs-lookup"><span data-stu-id="d4534-165">Vendor specific schemas (-moz-,-webkit)</span></span>](#_Toc318097419)
        - [<span data-ttu-id="d4534-166">註解和取消的支援</span><span class="sxs-lookup"><span data-stu-id="d4534-166">Commenting and uncommenting support</span></span>](#_Toc318097420)
        - [<span data-ttu-id="d4534-167">色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="d4534-167">Color picker</span></span>](#_Toc318097421)
        - [<span data-ttu-id="d4534-168">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="d4534-168">Snippets</span></span>](#_Toc318097422)
        - [<span data-ttu-id="d4534-169">自訂區域</span><span class="sxs-lookup"><span data-stu-id="d4534-169">Custom regions</span></span>](#_Toc318097423)
    - [<span data-ttu-id="d4534-170">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="d4534-170">Page Inspector</span></span>](#_Toc318097424)
    - [<span data-ttu-id="d4534-171">發行</span><span class="sxs-lookup"><span data-stu-id="d4534-171">Publishing</span></span>](#_Toc318097425)

        - [<span data-ttu-id="d4534-172">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="d4534-172">Publish profiles</span></span>](#_Toc318097426)
        - [<span data-ttu-id="d4534-173">ASP.NET 先行編譯及合併</span><span class="sxs-lookup"><span data-stu-id="d4534-173">ASP.NET precompilation and merge</span></span>](#_Toc318097427)
- [<span data-ttu-id="d4534-174">IIS Express</span><span class="sxs-lookup"><span data-stu-id="d4534-174">IIS Express</span></span>](#_Toc318097428)
- [<span data-ttu-id="d4534-175">免責聲明</span><span class="sxs-lookup"><span data-stu-id="d4534-175">Disclaimer</span></span>](#_Toc318097429)

<a id="_Toc318097372"></a>
## <a name="aspnet-core-runtime-and-framework"></a><span data-ttu-id="d4534-176">ASP.NET Core 執行階段和架構</span><span class="sxs-lookup"><span data-stu-id="d4534-176">ASP.NET Core Runtime and Framework</span></span>

<a id="_Toc318097373"></a>
### <a name="asynchronously-reading-and-writing-http-requests-and-responses"></a><span data-ttu-id="d4534-177">以非同步方式讀取和寫入 HTTP 要求和回應</span><span class="sxs-lookup"><span data-stu-id="d4534-177">Asynchronously Reading and Writing HTTP Requests and Responses</span></span>

<span data-ttu-id="d4534-178">ASP.NET 4 導入了能夠讀取資料流使用的 HTTP 要求實體*HttpRequest.GetBufferlessInputStream*方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-178">ASP.NET 4 introduced the ability to read an HTTP request entity as a stream using the *HttpRequest.GetBufferlessInputStream* method.</span></span> <span data-ttu-id="d4534-179">這個方法會提供資料流存取要求的實體。</span><span class="sxs-lookup"><span data-stu-id="d4534-179">This method provided streaming access to the request entity.</span></span> <span data-ttu-id="d4534-180">不過，它以同步方式執行，其中繫結起來，執行緒要求的持續時間。</span><span class="sxs-lookup"><span data-stu-id="d4534-180">However, it executed synchronously, which tied up a thread for the duration of a request.</span></span>

<span data-ttu-id="d4534-181">ASP.NET 4.5 支援 HTTP 要求實體，以非同步方式上的資料流中讀取和非同步排清的能力。</span><span class="sxs-lookup"><span data-stu-id="d4534-181">ASP.NET 4.5 supports the ability to read streams asynchronously on an HTTP request entity, and the ability to flush asynchronously.</span></span> <span data-ttu-id="d4534-182">ASP.NET 4.5 還提供雙重緩衝 HTTP 要求實體，可提供更容易整合具有下游的 HTTP 處理常式，例如.aspx 網頁處理常式和 ASP.NET MVC 控制站的能力。</span><span class="sxs-lookup"><span data-stu-id="d4534-182">ASP.NET 4.5 also gives you the ability to double-buffer an HTTP request entity, which provides easier integration with downstream HTTP handlers such as .aspx page handlers and ASP.NET MVC controllers.</span></span>

<a id="_Toc318097374"></a>
#### <a name="improvements-to-httprequest-handling"></a><span data-ttu-id="d4534-183">HttpRequest 處理的增強功能</span><span class="sxs-lookup"><span data-stu-id="d4534-183">Improvements to HttpRequest handling</span></span>

<span data-ttu-id="d4534-184">從 ASP.NET 4.5 所傳回的資料流參考*HttpRequest.GetBufferlessInputStream*支援同步和非同步讀取的方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-184">The Stream reference returned by ASP.NET 4.5 from *HttpRequest.GetBufferlessInputStream* supports both synchronous and asynchronous read methods.</span></span> <span data-ttu-id="d4534-185">*資料流*從傳回的物件*GetBufferlessInputStream*現在實作的 BeginRead 和 EndRead 方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-185">The *Stream* object returned from *GetBufferlessInputStream* now implements both the BeginRead and EndRead methods.</span></span> <span data-ttu-id="d4534-186">非同步*資料流*方法可讓您以非同步方式讀取要求實體中的區塊，而 ASP.NET 釋放目前執行緒的非同步讀取迴圈的每個反覆項目之間。</span><span class="sxs-lookup"><span data-stu-id="d4534-186">The asynchronous *Stream* methods let you asynchronously read the request entity in chunks, while ASP.NET releases the current thread between each iteration of an asynchronous read loop.</span></span>

<span data-ttu-id="d4534-187">ASP.NET 4.5 也加入附屬方法讀取要求實體的經緩衝處理的方式： *HttpRequest.GetBufferedInputStream*。</span><span class="sxs-lookup"><span data-stu-id="d4534-187">ASP.NET 4.5 has also added a companion method for reading the request entity in a buffered way: *HttpRequest.GetBufferedInputStream*.</span></span> <span data-ttu-id="d4534-188">這個新的多載的運作方式類似*GetBufferlessInputStream*，支援同步和非同步讀取。</span><span class="sxs-lookup"><span data-stu-id="d4534-188">This new overload works like *GetBufferlessInputStream*, supporting both synchronous and asynchronous reads.</span></span> <span data-ttu-id="d4534-189">不過，因為它所讀取*GetBufferedInputStream*也將實體位元組複製到 ASP.NET 內部緩衝區，如此下游的模組和處理常式仍然可以存取要求的實體。</span><span class="sxs-lookup"><span data-stu-id="d4534-189">However, as it reads, *GetBufferedInputStream* also copies the entity bytes into ASP.NET internal buffers so that downstream modules and handlers can still access the request entity.</span></span> <span data-ttu-id="d4534-190">例如，如果某些上游管線中的程式碼已讀取要求實體使用*GetBufferedInputStream*，您仍然可以使用*HttpRequest.Form*或*HttpRequest.Files*.</span><span class="sxs-lookup"><span data-stu-id="d4534-190">For example, if some upstream code in the pipeline has already read the request entity using *GetBufferedInputStream*, you can still use *HttpRequest.Form* or *HttpRequest.Files*.</span></span> <span data-ttu-id="d4534-191">這可讓您執行的非同步處理的要求 （例如，資料流處理大型檔案上的傳至資料庫），但仍執行的.aspx 網頁和 ASP.NET MVC 控制器之後。</span><span class="sxs-lookup"><span data-stu-id="d4534-191">This lets you perform asynchronous processing on a request (for example, streaming a large file upload to a database), but still run .aspx pages and MVC ASP.NET controllers afterward.</span></span>

<a id="_Toc318097375"></a>
#### <a name="asynchronously-flushing-a-response"></a><span data-ttu-id="d4534-192">非同步清除回應</span><span class="sxs-lookup"><span data-stu-id="d4534-192">Asynchronously flushing a response</span></span>

<span data-ttu-id="d4534-193">傳送回應至 HTTP 用戶端可能需要相當長的時間，當用戶端很遠，或具有低頻寬連線。</span><span class="sxs-lookup"><span data-stu-id="d4534-193">Sending responses to an HTTP client can take considerable time when the client is far away or has a low-bandwidth connection.</span></span> <span data-ttu-id="d4534-194">通常 ASP.NET 應用程式所建立時緩衝處理回應位元組。</span><span class="sxs-lookup"><span data-stu-id="d4534-194">Normally ASP.NET buffers the response bytes as they are created by an application.</span></span> <span data-ttu-id="d4534-195">ASP.NET 然後會執行單一傳送作業的要求處理結尾處累加的緩衝區。</span><span class="sxs-lookup"><span data-stu-id="d4534-195">ASP.NET then performs a single send operation of the accrued buffers at the very end of request processing.</span></span>

<span data-ttu-id="d4534-196">如果已緩衝的回應是大型 （例如，用戶端的大型檔案資料流處理），必須定期呼叫*HttpResponse.Flush*緩衝的輸出傳送至用戶端，並保留控制下的記憶體使用量。</span><span class="sxs-lookup"><span data-stu-id="d4534-196">If the buffered response is large (for example, streaming a large file to a client), you must periodically call *HttpResponse.Flush* to send buffered output to the client and keep memory usage under control.</span></span> <span data-ttu-id="d4534-197">不過，因為*排清*是同步的呼叫，並反覆地呼叫*排清*仍可能長時間執行要求期間使用執行緒。</span><span class="sxs-lookup"><span data-stu-id="d4534-197">However, because *Flush* is a synchronous call, iteratively calling *Flush* still consumes a thread for the duration of potentially long-running requests.</span></span>

<span data-ttu-id="d4534-198">ASP.NET 4.5 新增支援，如使用以非同步方式執行排清*BeginFlush*和*EndFlush*方法*HttpResponse*類別。</span><span class="sxs-lookup"><span data-stu-id="d4534-198">ASP.NET 4.5 adds support for performing flushes asynchronously using the *BeginFlush* and *EndFlush* methods of the *HttpResponse* class.</span></span> <span data-ttu-id="d4534-199">您可以使用這些方法，建立非同步的模組和累加地傳送資料到用戶端而不會佔用作業系統執行緒的非同步處理常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-199">Using these methods, you can create asynchronous modules and asynchronous handlers that incrementally send data to a client without tying up operating-system threads.</span></span> <span data-ttu-id="d4534-200">在中間*BeginFlush*和*EndFlush*呼叫，ASP.NET 釋放目前的執行緒。</span><span class="sxs-lookup"><span data-stu-id="d4534-200">In between *BeginFlush* and *EndFlush* calls, ASP.NET releases the current thread.</span></span> <span data-ttu-id="d4534-201">這會大幅地減少以支援長時間執行的 HTTP 下載所需的作用中執行緒的總數。</span><span class="sxs-lookup"><span data-stu-id="d4534-201">This substantially reduces the total number of active threads that are needed in order to support long-running HTTP downloads.</span></span>

<a id="_Toc318097376"></a>
### <a name="support-for-await-and-task---based-asynchronous-modules-and-handlers"></a><span data-ttu-id="d4534-202">支援*await*和*工作*為基礎的非同步模組和處理常式</span><span class="sxs-lookup"><span data-stu-id="d4534-202">Support for *await* and *Task* - Based Asynchronous Modules and Handlers</span></span>

<span data-ttu-id="d4534-203">.NET Framework 4 引進稱為 「 非同步程式設計概念*工作*。</span><span class="sxs-lookup"><span data-stu-id="d4534-203">The .NET Framework 4 introduced an asynchronous programming concept referred to as a *task*.</span></span> <span data-ttu-id="d4534-204">工作由*工作*類型和相關的類型中*System.Threading.Tasks*命名空間。</span><span class="sxs-lookup"><span data-stu-id="d4534-204">Tasks are represented by the *Task* type and related types in the *System.Threading.Tasks* namespace.</span></span> <span data-ttu-id="d4534-205">.NET Framework 4.5 與編譯器搭配使用的增強功能建置這*工作*簡單的物件。</span><span class="sxs-lookup"><span data-stu-id="d4534-205">The .NET Framework 4.5 builds on this with compiler enhancements that make working with *Task* objects simple.</span></span> <span data-ttu-id="d4534-206">在.NET Framework 4.5，編譯器支援兩個新的關鍵字： *await*和*非同步*。</span><span class="sxs-lookup"><span data-stu-id="d4534-206">In the .NET Framework 4.5, the compilers support two new keywords: *await* and *async*.</span></span> <span data-ttu-id="d4534-207">*Await*關鍵字是用以表示某段程式碼應該以非同步方式等候一段程式碼的語法縮寫。</span><span class="sxs-lookup"><span data-stu-id="d4534-207">The *await* keyword is syntactical shorthand for indicating that a piece of code should asynchronously wait on some other piece of code.</span></span> <span data-ttu-id="d4534-208">*非同步*關鍵字表示提示可讓您將方法標記為以工作為基礎的非同步方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-208">The *async* keyword represents a hint that you can use to mark methods as task-based asynchronous methods.</span></span>

<span data-ttu-id="d4534-209">組合*await*，*非同步*，而*工作*物件可讓您在.NET 4.5 中撰寫非同步程式碼更容易。</span><span class="sxs-lookup"><span data-stu-id="d4534-209">The combination of *await*, *async*, and the *Task* object makes it much easier for you to write asynchronous code in .NET 4.5.</span></span> <span data-ttu-id="d4534-210">ASP.NET 4.5 支援這些簡單化新應用程式開發介面可讓您撰寫非同步 HTTP 模組，並使用新編譯器增強功能的非同步 HTTP 處理常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-210">ASP.NET 4.5 supports these simplifications with new APIs that let you write asynchronous HTTP modules and asynchronous HTTP handlers using the new compiler enhancements.</span></span>

<a id="_Toc318097377"></a>
#### <a name="asynchronous-http-modules"></a><span data-ttu-id="d4534-211">非同步 HTTP 模組</span><span class="sxs-lookup"><span data-stu-id="d4534-211">Asynchronous HTTP modules</span></span>

<span data-ttu-id="d4534-212">假設您想要執行非同步工作傳回方法內*工作*物件。</span><span class="sxs-lookup"><span data-stu-id="d4534-212">Suppose that you want to perform asynchronous work within a method that returns a *Task* object.</span></span> <span data-ttu-id="d4534-213">下列程式碼範例會定義非同步方法，讓下載 Microsoft 首頁上的非同步呼叫。</span><span class="sxs-lookup"><span data-stu-id="d4534-213">The following code example defines an asynchronous method that makes an asynchronous call to download the Microsoft home page.</span></span> <span data-ttu-id="d4534-214">請注意，使用*非同步*方法簽章中的關鍵字和*await*呼叫*DownloadStringTaskAsync*。</span><span class="sxs-lookup"><span data-stu-id="d4534-214">Notice the use of the *async* keyword in the method signature and the *await* call to *DownloadStringTaskAsync*.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample1.cs)]

<span data-ttu-id="d4534-215">這就是您所撰寫，.NET Framework 將會自動處理在等候下載完成，以及下載完成之後，自動還原呼叫堆疊時回溯呼叫堆疊。</span><span class="sxs-lookup"><span data-stu-id="d4534-215">That's all you have to write — the .NET Framework will automatically handle unwinding the call stack while waiting for the download to complete, as well as automatically restoring the call stack after the download is done.</span></span>

<span data-ttu-id="d4534-216">現在，假設您想要使用非同步的 ASP.NET HTTP 模組中的這個非同步方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-216">Now suppose that you want to use this asynchronous method in an asynchronous ASP.NET HTTP module.</span></span> <span data-ttu-id="d4534-217">ASP.NET 4.5 包括 helper 方法 (*EventHandlerTaskAsyncHelper*) 和新的委派型別 (*TaskEventHandler*)，您可以使用與舊整合以工作為基礎的非同步方法將 ASP.NET HTTP 管線所公開的非同步程式設計模型。</span><span class="sxs-lookup"><span data-stu-id="d4534-217">ASP.NET 4.5 includes a helper method (*EventHandlerTaskAsyncHelper*) and a new delegate type (*TaskEventHandler*) that you can use to integrate task-based asynchronous methods with the older asynchronous programming model exposed by the ASP.NET HTTP pipeline.</span></span> <span data-ttu-id="d4534-218">這個範例會示範如何：</span><span class="sxs-lookup"><span data-stu-id="d4534-218">This example shows how:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample2.cs)]

<a id="_Toc318097378"></a>
#### <a name="asynchronous-http-handlers"></a><span data-ttu-id="d4534-219">非同步 HTTP 處理常式</span><span class="sxs-lookup"><span data-stu-id="d4534-219">Asynchronous HTTP handlers</span></span>

<span data-ttu-id="d4534-220">在 ASP.NET 中撰寫非同步處理常式的傳統方法為實作*IHttpAsyncHandler*介面。</span><span class="sxs-lookup"><span data-stu-id="d4534-220">The traditional approach to writing asynchronous handlers in ASP.NET is to implement the *IHttpAsyncHandler* interface.</span></span> <span data-ttu-id="d4534-221">ASP.NET 4.5 引進了*HttpTaskAsyncHandler*非同步基底類型可以衍生自，使其更容易撰寫非同步處理常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-221">ASP.NET 4.5 introduces the *HttpTaskAsyncHandler* asynchronous base type that you can derive from, which makes it much easier to write asynchronous handlers.</span></span>

<span data-ttu-id="d4534-222">*HttpTaskAsyncHandler*類型是抽象的會要求您覆寫*ProcessRequestAsync*方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-222">The *HttpTaskAsyncHandler* type is abstract and requires you to override the *ProcessRequestAsync* method.</span></span> <span data-ttu-id="d4534-223">ASP.NET 會在內部處理的整合傳回簽章 (*工作*物件) 的*ProcessRequestAsync*與較舊的非同步程式設計模型使用的 ASP.NET 管線。</span><span class="sxs-lookup"><span data-stu-id="d4534-223">Internally ASP.NET takes care of integrating the return signature (a *Task* object) of *ProcessRequestAsync* with the older asynchronous programming model used by the ASP.NET pipeline.</span></span>

<span data-ttu-id="d4534-224">下列範例示範如何使用*工作*和*await*為非同步的 HTTP 處理常式實作的一部分：</span><span class="sxs-lookup"><span data-stu-id="d4534-224">The following example shows how you can use *Task* and *await* as part of the implementation of an asynchronous HTTP handler:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample3.cs)]

<a id="_Toc318097379"></a>
### <a name="new-aspnet-request-validation-features"></a><span data-ttu-id="d4534-225">ASP.NET 要求驗證的新功能</span><span class="sxs-lookup"><span data-stu-id="d4534-225">New ASP.NET Request Validation Features</span></span>

<span data-ttu-id="d4534-226">根據預設，ASP.NET 會執行要求驗證，它會檢查標記或指令碼中的欄位、 標頭、 cookie 和等等的要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-226">By default, ASP.NET performs request validation — it examines requests to look for markup or script in fields, headers, cookies, and so on.</span></span> <span data-ttu-id="d4534-227">如果偵測到任何時，ASP.NET 會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d4534-227">If any is detected, ASP.NET throws an exception.</span></span> <span data-ttu-id="d4534-228">這可以做為第一行防禦機制以防止潛在的跨網站指令碼的攻擊。</span><span class="sxs-lookup"><span data-stu-id="d4534-228">This acts as a first line of defense against potential cross-site scripting attacks.</span></span>

<span data-ttu-id="d4534-229">ASP.NET 4.5 輕鬆地選擇性地讀取未經驗證的要求資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-229">ASP.NET 4.5 makes it easy to selectively read unvalidated request data.</span></span> <span data-ttu-id="d4534-230">ASP.NET 4.5 也整合熱門 AntiXSS 程式庫，原來外部程式庫。</span><span class="sxs-lookup"><span data-stu-id="d4534-230">ASP.NET 4.5 also integrates the popular AntiXSS library, which was formerly an external library.</span></span>

<span data-ttu-id="d4534-231">開發人員經常能夠選擇性地關閉要求驗證其應用程式的要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-231">Developers have frequently asked for the ability to selectively turn off request validation for their applications.</span></span> <span data-ttu-id="d4534-232">比方說，如果您的應用程式論壇軟體，您可能想要允許使用者提交的 HTML 格式論壇文章和註解，但是仍然確定要求驗證會檢查所有其他項目。</span><span class="sxs-lookup"><span data-stu-id="d4534-232">For example, if your application is forum software, you might want to allow users to submit HTML-formatted forum posts and comments, but still make sure that request validation is checking everything else.</span></span>

<span data-ttu-id="d4534-233">ASP.NET 4.5 引進了兩項功能可簡化您選擇性地使用未經驗證的輸入： 延遲 （「 延遲 」） 的要求驗證和未經驗證的要求資料的存取權。</span><span class="sxs-lookup"><span data-stu-id="d4534-233">ASP.NET 4.5 introduces two features that make it easy for you to selectively work with unvalidated input: deferred ("lazy") request validation and access to unvalidated request data.</span></span>

<a id="_Toc318097380"></a>
#### <a name="deferred-lazy-request-validation"></a><span data-ttu-id="d4534-234">延遲 （「 延遲 」） 的要求驗證</span><span class="sxs-lookup"><span data-stu-id="d4534-234">Deferred ("lazy") request validation</span></span>

<span data-ttu-id="d4534-235">在 ASP.NET 4.5，根據預設所有要求資料受都限於要求驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-235">In ASP.NET 4.5, by default all request data is subject to request validation.</span></span> <span data-ttu-id="d4534-236">不過，您可以設定應用程式，以延遲要求驗證，直到您實際存取要求的資料為止。</span><span class="sxs-lookup"><span data-stu-id="d4534-236">However, you can configure the application to defer request validation until you actually access request data.</span></span> <span data-ttu-id="d4534-237">（這有時候稱為延遲要求驗證，根據等消極式載入某些資料案例中的詞彙。）您可以設定應用程式的 Web.config 檔案中使用延後的驗證，藉由設定*requestValidationMode*屬性中的 4.5 *httpRUntime*項目，如下列範例所示：</span><span class="sxs-lookup"><span data-stu-id="d4534-237">(This is sometimes referred to as lazy request validation, based on terms like lazy loading for certain data scenarios.) You can configure the application to use deferred validation in the Web.config file by setting the *requestValidationMode* attribute to 4.5 in the *httpRUntime* element, as in the following example:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample4.xml)]

<span data-ttu-id="d4534-238">當要求的驗證模式設定為 4.5 時，僅針對特定要求值，且只有在程式碼會存取該值時，才觸發要求驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-238">When request validation mode is set to 4.5, request validation is triggered only for a specific request value and only when your code accesses that value.</span></span> <span data-ttu-id="d4534-239">比方說，如果您的程式碼取得 Request.Form["forum 值\_張貼"]，僅針對表單集合中的元素叫用要求驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-239">For example, if your code gets the value of Request.Form["forum\_post"], request validation is invoked only for that element in the form collection.</span></span> <span data-ttu-id="d4534-240">沒有任何其他元素中*表單*集合會進行驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-240">None of the other elements in the *Form* collection are validated.</span></span> <span data-ttu-id="d4534-241">在舊版 ASP.NET 中，是在整個要求集合觸發要求驗證已存取集合中的任何項目時。</span><span class="sxs-lookup"><span data-stu-id="d4534-241">In previous versions of ASP.NET, request validation was triggered for the entire request collection when any element in the collection was accessed.</span></span> <span data-ttu-id="d4534-242">新的行為可讓您更輕鬆地查看不同片段的要求資料，而不觸發要求驗證，其他部分的不同應用程式元件。</span><span class="sxs-lookup"><span data-stu-id="d4534-242">The new behavior makes it easier for different application components to look at different pieces of request data without triggering request validation on other pieces.</span></span>

<a id="_Toc318097381"></a>
#### <a name="support-for-unvalidated-requests"></a><span data-ttu-id="d4534-243">未驗證要求的支援</span><span class="sxs-lookup"><span data-stu-id="d4534-243">Support for unvalidated requests</span></span>

<span data-ttu-id="d4534-244">延後的要求驗證單獨做無法解決問題的選擇性地略過要求驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-244">Deferred request validation alone doesn't solve the problem of selectively bypassing request validation.</span></span> <span data-ttu-id="d4534-245">呼叫 Request.Form["forum\_張貼"] 觸發程序仍要求驗證，針對該特定要求的值。</span><span class="sxs-lookup"><span data-stu-id="d4534-245">The call to Request.Form["forum\_post"] still triggers request validation for that specific request value.</span></span> <span data-ttu-id="d4534-246">不過，您可能想要存取此欄位，而不觸發驗證，因為您想要讓該欄位中的標記。</span><span class="sxs-lookup"><span data-stu-id="d4534-246">However, you might want to access this field without triggering validation because you want to allow markup in that field.</span></span>

<span data-ttu-id="d4534-247">若要允許這樣做，ASP.NET 4.5 現在支援未經驗證之的資料的存取權要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-247">To allow this, ASP.NET 4.5 now supports unvalidated access to request data.</span></span> <span data-ttu-id="d4534-248">包含新的 ASP.NET 4.5 *Unvalidated*集合屬性中的*HttpRequest*類別。</span><span class="sxs-lookup"><span data-stu-id="d4534-248">ASP.NET 4.5 includes a new *Unvalidated* collection property in the *HttpRequest* class.</span></span> <span data-ttu-id="d4534-249">這個集合會提供類似的存取權的所有要求資料的一般值*表單*， *QueryString*， *Cookie*，和*Url*。</span><span class="sxs-lookup"><span data-stu-id="d4534-249">This collection provides access to all of the common values of request data, like *Form*, *QueryString*, *Cookies*, and *Url*.</span></span>

<span data-ttu-id="d4534-250">使用論壇的範例，若要能夠讀取未經驗證的要求資料，您必須先設定應用程式使用新的要求驗證模式：</span><span class="sxs-lookup"><span data-stu-id="d4534-250">Using the forum example, to be able to read unvalidated request data, you first need to configure the application to use the new request validation mode:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample5.xml)]

<span data-ttu-id="d4534-251">然後您可以使用*HttpRequest.Unvalidated*讀取未驗證的表單值的屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-251">You can then use the *HttpRequest.Unvalidated* property to read the unvalidated form value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample6.cs)]


> [!WARNING]
> <span data-ttu-id="d4534-252">安全性-*請小心使用未經驗證的要求資料 ！*</span><span class="sxs-lookup"><span data-stu-id="d4534-252">Security - *Use unvalidated request data with care!*</span></span> <span data-ttu-id="d4534-253">ASP.NET 4.5 中已加入的未經驗證的要求內容和以方便您存取非常特定未經驗證的要求資料的集合。</span><span class="sxs-lookup"><span data-stu-id="d4534-253">ASP.NET 4.5 added the unvalidated request properties and collections to make it easier for you to access very specific unvalidated request data.</span></span> <span data-ttu-id="d4534-254">不過，您仍必須執行未經處理的要求資料，以確保危險的文字不會呈現使用者自訂的驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-254">However, you must still perform custom validation on the raw request data to ensure that dangerous text is not rendered to users.</span></span>


<a id="_Toc318097382"></a>
### <a name="antixss-library"></a><span data-ttu-id="d4534-255">AntiXSS 程式庫</span><span class="sxs-lookup"><span data-stu-id="d4534-255">AntiXSS Library</span></span>

<span data-ttu-id="d4534-256">由於 Microsoft AntiXSS 文件庫的普及，ASP.NET 4.5 現已加入從 4.0 版，該程式庫的核心編碼常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-256">Due to the popularity of the Microsoft AntiXSS Library, ASP.NET 4.5 now incorporates core encoding routines from version 4.0 of that library.</span></span>

<span data-ttu-id="d4534-257">編碼常式由實作*AntiXssEncoder*在新的型別*System.Web.Security.AntiXss*命名空間。</span><span class="sxs-lookup"><span data-stu-id="d4534-257">The encoding routines are implemented by the *AntiXssEncoder* type in the new *System.Web.Security.AntiXss* namespace.</span></span> <span data-ttu-id="d4534-258">您可以使用*AntiXssEncoder*直接藉由呼叫的任何靜態類型中實作的編碼方法的型別。</span><span class="sxs-lookup"><span data-stu-id="d4534-258">You can use the *AntiXssEncoder* type directly by calling any of the static encoding methods that are implemented in the type.</span></span> <span data-ttu-id="d4534-259">不過，使用新的反 XSS 常式的最簡單方法是設定 ASP.NET 應用程式使用*AntiXssEncoder*預設類別。</span><span class="sxs-lookup"><span data-stu-id="d4534-259">However, the easiest approach for using the new anti-XSS routines is to configure an ASP.NET application to use the *AntiXssEncoder* class by default.</span></span> <span data-ttu-id="d4534-260">若要這樣做，請加入 Web.config 檔案中的下列屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-260">To do this, add the following attribute to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample7.xml)]

<span data-ttu-id="d4534-261">當*encoderType*屬性設定為使用*AntiXssEncoder*所有型別，輸出的編碼方式在 ASP.NET 自動使用新的編碼常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-261">When the *encoderType* attribute is set to use the *AntiXssEncoder* type, all output encoding in ASP.NET automatically uses the new encoding routines.</span></span>

<span data-ttu-id="d4534-262">這些是已納入 ASP.NET 4.5 外部 AntiXSS 文件庫的部分：</span><span class="sxs-lookup"><span data-stu-id="d4534-262">These are the portions of the external AntiXSS library that have been incorporated into ASP.NET 4.5:</span></span>

- <span data-ttu-id="d4534-263">*HtmlEncode*， *HtmlFormUrlEncode*，和*HtmlAttributeEncode*</span><span class="sxs-lookup"><span data-stu-id="d4534-263">*HtmlEncode*, *HtmlFormUrlEncode*, and *HtmlAttributeEncode*</span></span>
- <span data-ttu-id="d4534-264">*XmlAttributeEncode*和*XmlEncode*</span><span class="sxs-lookup"><span data-stu-id="d4534-264">*XmlAttributeEncode* and *XmlEncode*</span></span>
- <span data-ttu-id="d4534-265">*進行 urlencode 處理*和*UrlPathEncode* （新增）</span><span class="sxs-lookup"><span data-stu-id="d4534-265">*UrlEncode* and *UrlPathEncode* (new)</span></span>
- <span data-ttu-id="d4534-266">*CssEncode*</span><span class="sxs-lookup"><span data-stu-id="d4534-266">*CssEncode*</span></span>

<a id="_Toc318097383"></a>
### <a name="support-for-websockets-protocol"></a><span data-ttu-id="d4534-267">支援的 Websocket 通訊協定</span><span class="sxs-lookup"><span data-stu-id="d4534-267">Support for WebSockets Protocol</span></span>

<span data-ttu-id="d4534-268">Websocket 通訊協定會定義如何建立透過 HTTP 的用戶端與伺服器之間的安全、 即時雙向通訊的標準為基礎網路通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d4534-268">WebSockets protocol is a standards-based network protocol that defines how to establish secure, real-time bidirectional communications between a client and a server over HTTP.</span></span> <span data-ttu-id="d4534-269">Microsoft 努力與 IETF 和 W3C 標準內文，以協助定義的通訊協定。</span><span class="sxs-lookup"><span data-stu-id="d4534-269">Microsoft has worked with both the IETF and W3C standards bodies to help define the protocol.</span></span> <span data-ttu-id="d4534-270">Websocket 通訊協定與投資大量資源的用戶端及行動作業系統上支援 Websocket 通訊協定的 Microsoft 支援的任何用戶端 （不只是瀏覽器使用）。</span><span class="sxs-lookup"><span data-stu-id="d4534-270">The WebSockets protocol is supported by any client (not just browsers), with Microsoft investing substantial resources supporting WebSockets protocol on both client and mobile operating systems.</span></span>

<span data-ttu-id="d4534-271">Websocket 通訊協定可讓您更容易建立用戶端與伺服器之間的長時間執行資料傳輸。</span><span class="sxs-lookup"><span data-stu-id="d4534-271">WebSockets protocol makes it much easier to create long-running data transfers between a client and a server.</span></span> <span data-ttu-id="d4534-272">例如，寫入交談應用程式就能輕鬆因為您可以建立用戶端與伺服器之間，則為 true 的長時間執行連接。</span><span class="sxs-lookup"><span data-stu-id="d4534-272">For example, writing a chat application is much easier because you can establish a true long-running connection between a client and a server.</span></span> <span data-ttu-id="d4534-273">您不必訴諸於定期輪詢或 HTTP 長期輪詢模擬通訊端的行為類似的因應措施。</span><span class="sxs-lookup"><span data-stu-id="d4534-273">You do not have to resort to workarounds like periodic polling or HTTP long-polling to simulate the behavior of a socket.</span></span>

<span data-ttu-id="d4534-274">ASP.NET 4.5 和 IIS 8 包含低階 WebSockets 的支援，讓 ASP.NET 開發人員使用受管理的應用程式開發介面進行以非同步方式讀取和寫入 WebSockets 物件上的字串和二進位資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-274">ASP.NET 4.5 and IIS 8 include low-level WebSockets support, enabling ASP.NET developers to use managed APIs for asynchronously reading and writing both string and binary data on a WebSockets object.</span></span> <span data-ttu-id="d4534-275">針對 ASP.NET 4.5 沒有新*System.Web.WebSockets*包含型別，Websocket 通訊協定所使用的命名空間。</span><span class="sxs-lookup"><span data-stu-id="d4534-275">For ASP.NET 4.5, there is a new *System.Web.WebSockets* namespace that contains types for working with WebSockets protocol.</span></span>

<span data-ttu-id="d4534-276">瀏覽器用戶端藉由建立 DOM 建立 Websocket 連接*WebSocket* ASP.NET 應用程式中，如下列範例所示的 URL 所指向的物件：</span><span class="sxs-lookup"><span data-stu-id="d4534-276">A browser client establishes a WebSockets connection by creating a DOM *WebSocket* object that points to a URL in an ASP.NET application, as in the following example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample8.cs)]

<span data-ttu-id="d4534-277">您可以在 ASP.NET 中使用任何種類的模組或處理常式建立 WebSockets 端點。</span><span class="sxs-lookup"><span data-stu-id="d4534-277">You can create WebSockets endpoints in ASP.NET using any kind of module or handler.</span></span> <span data-ttu-id="d4534-278">在上述範例中，已使用.ashx 檔案，因為.ashx 檔案快速建立處理常式。</span><span class="sxs-lookup"><span data-stu-id="d4534-278">In the previous example, an .ashx file was used, because .ashx files are a quick way to create a handler.</span></span>

<span data-ttu-id="d4534-279">根據 Websocket 通訊協定，ASP.NET 應用程式會接受用戶端的 Websocket 要求，藉由指示要求應該升級從 HTTP GET 要求，為 Websocket 要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-279">According to the WebSockets protocol, an ASP.NET application accepts a client's WebSockets request by indicating that the request should be upgraded from an HTTP GET request to a WebSockets request.</span></span> <span data-ttu-id="d4534-280">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="d4534-280">Here's an example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample9.cs)]

<span data-ttu-id="d4534-281">*AcceptWebSocketRequest*方法可接受函式委派，因為 ASP.NET 回溯目前 HTTP 要求，然後將控制項傳送至函式委派。</span><span class="sxs-lookup"><span data-stu-id="d4534-281">The *AcceptWebSocketRequest* method accepts a function delegate because ASP.NET unwinds the current HTTP request and then transfers control to the function delegate.</span></span> <span data-ttu-id="d4534-282">在概念上類似於您如何使用這種方法是*System.Threading.Thread*，您用來定義在背景中執行工作的執行緒啟動委派。</span><span class="sxs-lookup"><span data-stu-id="d4534-282">Conceptually this approach is similar to how you use *System.Threading.Thread*, where you define a thread-start delegate in which background work is performed.</span></span>

<span data-ttu-id="d4534-283">ASP.NET 和用戶端已順利完成 WebSockets 交握之後，ASP.NET 會呼叫您的委派，而且 WebSockets 應用程式開始執行。</span><span class="sxs-lookup"><span data-stu-id="d4534-283">After ASP.NET and the client have successfully completed a WebSockets handshake, ASP.NET calls your delegate and the WebSockets application starts running.</span></span> <span data-ttu-id="d4534-284">下列程式碼範例示範使用內建的 Websocket 支援在 ASP.NET 中的簡單的回應應用程式：</span><span class="sxs-lookup"><span data-stu-id="d4534-284">The following code example shows a simple echo application that uses the built-in WebSockets support in ASP.NET:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample10.cs)]

<span data-ttu-id="d4534-285">為.NET 4.5 中的支援*await*關鍵字和以工作為基礎的非同步作業適合自然撰寫 WebSockets 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-285">The support in .NET 4.5 for the *await* keyword and asynchronous task-based operations is a natural fit for writing WebSockets applications.</span></span> <span data-ttu-id="d4534-286">程式碼範例示範 Websocket 要求內 ASP.NET 完全以非同步方式執行。</span><span class="sxs-lookup"><span data-stu-id="d4534-286">The code example shows that a WebSockets request runs completely asynchronously inside ASP.NET.</span></span> <span data-ttu-id="d4534-287">應用程式會以非同步方式等候藉由呼叫從用戶端傳送訊息*await 通訊端。ReceiveAsync*。</span><span class="sxs-lookup"><span data-stu-id="d4534-287">The application waits asynchronously for a message to be sent from a client by calling *await socket.ReceiveAsync*.</span></span> <span data-ttu-id="d4534-288">同樣地，將非同步訊息用戶端藉由呼叫*await 通訊端。SendAsync*。</span><span class="sxs-lookup"><span data-stu-id="d4534-288">Similarly, you can send an asynchronous message to a client by calling *await socket.SendAsync*.</span></span>

<span data-ttu-id="d4534-289">在瀏覽器中，應用程式接收透過 WebSockets 訊息*onmessage*函式。</span><span class="sxs-lookup"><span data-stu-id="d4534-289">In the browser, an application receives WebSockets messages through an *onmessage* function.</span></span> <span data-ttu-id="d4534-290">若要從瀏覽器傳送訊息，請呼叫*傳送*方法*WebSocket* DOM 型別，如本範例所示：</span><span class="sxs-lookup"><span data-stu-id="d4534-290">To send a message from a browser, you call the *send* method of the *WebSocket* DOM type, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample11.cs)]

<span data-ttu-id="d4534-291">未來，我們可能會發行更新至這項功能，抽象離開部分低階的編碼也就需要在此版本中 websockets 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-291">In the future, we might release updates to this functionality that abstract away some of the low-level coding that is required in this release for WebSockets applications.</span></span>

<a id="_Toc318097384"></a>
### <a name="bundling-and-minification"></a><span data-ttu-id="d4534-292">統合及縮製</span><span class="sxs-lookup"><span data-stu-id="d4534-292">Bundling and Minification</span></span>

<span data-ttu-id="d4534-293">結合在一起，可讓您可以視為單一檔案的配套中結合個別的 JavaScript 和 CSS 檔案。</span><span class="sxs-lookup"><span data-stu-id="d4534-293">Bundling lets you combine individual JavaScript and CSS files into a bundle that can be treated like a single file.</span></span> <span data-ttu-id="d4534-294">縮製總是 JavaScript 和 CSS 的檔案，藉由移除空白字元，則不需要其他字元。</span><span class="sxs-lookup"><span data-stu-id="d4534-294">Minification condenses JavaScript and CSS files by removing whitespace and other characters that are not required.</span></span> <span data-ttu-id="d4534-295">這些功能是由 Web Form、 ASP.NET MVC 中，以及網頁所使用。</span><span class="sxs-lookup"><span data-stu-id="d4534-295">These features work with Web Forms, ASP.NET MVC, and Web Pages.</span></span>

<span data-ttu-id="d4534-296">組合是使用組合類別或其中一個它的子類別、 組合和 StyleBundle 所建立的。</span><span class="sxs-lookup"><span data-stu-id="d4534-296">Bundles are created using the Bundle class or one of its child classes, ScriptBundle and StyleBundle.</span></span> <span data-ttu-id="d4534-297">設定後之組合的執行個體，組合所提供的內送要求只將它加入全域 BundleCollection 執行個體。</span><span class="sxs-lookup"><span data-stu-id="d4534-297">After configuring an instance of a bundle, the bundle is made available to incoming requests by simply adding it to a global BundleCollection instance.</span></span> <span data-ttu-id="d4534-298">在預設範本，套件組合的組態執行 BundleConfig 檔案中。</span><span class="sxs-lookup"><span data-stu-id="d4534-298">In the default templates, bundle configuration is performed in a BundleConfig file.</span></span> <span data-ttu-id="d4534-299">這個預設組態建立的所有核心指令碼和範本所使用的 css 檔案的組合。</span><span class="sxs-lookup"><span data-stu-id="d4534-299">This default configuration creates bundles for all of the core scripts and css files used by the templates.</span></span>

<span data-ttu-id="d4534-300">從檢視內組合會使用幾種可能的 helper 方法的其中一個參考。</span><span class="sxs-lookup"><span data-stu-id="d4534-300">Bundles are referenced from within views by using one of a couple possible helper methods.</span></span> <span data-ttu-id="d4534-301">為了支援呈現不同的標記在偵錯與發行模式中的組合，為組合和 StyleBundle 類別有 helper 方法，呈現。</span><span class="sxs-lookup"><span data-stu-id="d4534-301">In order to support rendering different markup for a bundle when in debug vs. release mode, the ScriptBundle and StyleBundle classes have the helper method, Render.</span></span> <span data-ttu-id="d4534-302">在偵錯模式中轉譯將會產生組合中的每個資源的標記。</span><span class="sxs-lookup"><span data-stu-id="d4534-302">When in debug mode, Render will generate markup for each resource in the bundle.</span></span> <span data-ttu-id="d4534-303">在發行模式中轉譯將會產生組合的單一標記項目。</span><span class="sxs-lookup"><span data-stu-id="d4534-303">When in release mode, Render will generate a single markup element for the entire bundle.</span></span> <span data-ttu-id="d4534-304">切換之間偵錯和發行模式可藉由修改 compilation 項目，在 web.config 中的偵錯屬性，如下所示：</span><span class="sxs-lookup"><span data-stu-id="d4534-304">Toggling between debug and release mode can be accomplished by modifying the debug attribute of the compilation element in web.config as shown below:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample12.xml)]

<span data-ttu-id="d4534-305">此外，啟用或停用最佳化可以直接透過 BundleTable.EnableOptimizations 屬性來設定。</span><span class="sxs-lookup"><span data-stu-id="d4534-305">Additionally, enabling or disabling optimization can be set directly via the BundleTable.EnableOptimizations property.</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample13.cs)]

<span data-ttu-id="d4534-306">當檔案會集結時，它們會先依字母順序排序 (中顯示的方式**方案總管 中**)。</span><span class="sxs-lookup"><span data-stu-id="d4534-306">When files are bundled, they are first sorted alphabetically (the way they are displayed in **Solution Explorer**).</span></span> <span data-ttu-id="d4534-307">然後組織方式，讓已知的文件庫，以及其自訂延伸模組 （例如 jQuery、 MooTools 和 Dojo） 載入第一次。</span><span class="sxs-lookup"><span data-stu-id="d4534-307">They are then organized so that known libraries and their custom extensions (such as jQuery, MooTools, and Dojo) are loaded first.</span></span> <span data-ttu-id="d4534-308">例如，如上所示的指令碼資料夾組合的最終次序會是：</span><span class="sxs-lookup"><span data-stu-id="d4534-308">For example, the final order for the bundling of the Scripts folder as shown above will be:</span></span>

1. <span data-ttu-id="d4534-309">jquery 1.6.2.js</span><span class="sxs-lookup"><span data-stu-id="d4534-309">jquery-1.6.2.js</span></span>
2. <span data-ttu-id="d4534-310">jquery ui.js</span><span class="sxs-lookup"><span data-stu-id="d4534-310">jquery-ui.js</span></span>
3. <span data-ttu-id="d4534-311">jquery.tools.js</span><span class="sxs-lookup"><span data-stu-id="d4534-311">jquery.tools.js</span></span>
4. <span data-ttu-id="d4534-312">a.js</span><span class="sxs-lookup"><span data-stu-id="d4534-312">a.js</span></span>

<span data-ttu-id="d4534-313">CSS 檔案是也依字母順序排序，然後重新組織，以便 reset.css 和 normalize.css 前面的任何其他檔案。</span><span class="sxs-lookup"><span data-stu-id="d4534-313">CSS files are also sorted alphabetically and then reorganized so that reset.css and normalize.css come before any other file.</span></span> <span data-ttu-id="d4534-314">最終排序結合在一起的如上所示的 [Styles] 資料夾會是這樣：</span><span class="sxs-lookup"><span data-stu-id="d4534-314">The final sorting of the bundling of the Styles folder shown above will be this:</span></span>

1. <span data-ttu-id="d4534-315">reset.css</span><span class="sxs-lookup"><span data-stu-id="d4534-315">reset.css</span></span>
2. <span data-ttu-id="d4534-316">content.css</span><span class="sxs-lookup"><span data-stu-id="d4534-316">content.css</span></span>
3. <span data-ttu-id="d4534-317">forms.css</span><span class="sxs-lookup"><span data-stu-id="d4534-317">forms.css</span></span>
4. <span data-ttu-id="d4534-318">globals.css</span><span class="sxs-lookup"><span data-stu-id="d4534-318">globals.css</span></span>
5. <span data-ttu-id="d4534-319">menu.css</span><span class="sxs-lookup"><span data-stu-id="d4534-319">menu.css</span></span>
6. <span data-ttu-id="d4534-320">styles.css</span><span class="sxs-lookup"><span data-stu-id="d4534-320">styles.css</span></span>

<a id="_Toc_perf"></a>
### <a name="performance-improvements-for-web-hosting"></a><span data-ttu-id="d4534-321">適用於虛擬主機的效能改進</span><span class="sxs-lookup"><span data-stu-id="d4534-321">Performance Improvements for Web Hosting</span></span>

<span data-ttu-id="d4534-322">.NET Framework 4.5 和 Windows 8 引進可協助您達成大幅提升效能的 web 伺服器工作負載的功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-322">The .NET Framework 4.5 and Windows 8 introduce features that can help you achieve a significant performance boost for web-server workloads.</span></span> <span data-ttu-id="d4534-323">這包括減少 （最多 35%) 在這兩個啟動時間和記憶體耗用量的 web 主控使用 ASP.NET 的網站。</span><span class="sxs-lookup"><span data-stu-id="d4534-323">This includes a reduction (up to 35%) in both startup time and in the memory footprint of web hosting sites that use ASP.NET.</span></span>

<a id="_Toc_perf_1"></a>
#### <a name="key-performance-factors"></a><span data-ttu-id="d4534-324">關鍵效能因素</span><span class="sxs-lookup"><span data-stu-id="d4534-324">Key performance factors</span></span>

<span data-ttu-id="d4534-325">在理想情況下，應使用的所有網站並在記憶體中以確保快速的下一個要求的回應，每當到達。</span><span class="sxs-lookup"><span data-stu-id="d4534-325">Ideally, all websites should be active and in memory to assure quick response to the next request, whenever it comes.</span></span> <span data-ttu-id="d4534-326">可能會影響網站的回應性的因素包括：</span><span class="sxs-lookup"><span data-stu-id="d4534-326">Factors that can affect site responsiveness include:</span></span>

- <span data-ttu-id="d4534-327">回收應用程式集區之後，重新啟動站台所需的時間。</span><span class="sxs-lookup"><span data-stu-id="d4534-327">The time it takes for a site to restart after an app pool recycles.</span></span> <span data-ttu-id="d4534-328">這是不再記憶體的站台的組件時啟動站台 web 伺服器處理序所花費的時間。</span><span class="sxs-lookup"><span data-stu-id="d4534-328">This is the time it takes to launch a web server process for the site when the site assemblies are no longer in memory.</span></span> <span data-ttu-id="d4534-329">（平台組件是仍在記憶體中，因為它們由其他站台）。這種情況稱為 「 冷網站，暖架構啟動 」 或只 「 冷網站啟動。 」</span><span class="sxs-lookup"><span data-stu-id="d4534-329">(The platform assemblies are still in memory, since they are used by other sites.) This situation is referred to as "cold site, warm framework startup" or just "cold site startup."</span></span>
- <span data-ttu-id="d4534-330">站台所佔據的記憶體數量。</span><span class="sxs-lookup"><span data-stu-id="d4534-330">How much memory the site occupies.</span></span> <span data-ttu-id="d4534-331">這個詞彙是 「 每個網站的記憶體耗用量 」 或 「 非共用的工作集 」。</span><span class="sxs-lookup"><span data-stu-id="d4534-331">Terms for this are "per-site memory consumption" or "unshared working set."</span></span>

<span data-ttu-id="d4534-332">新的效能改進專注於這兩個因素。</span><span class="sxs-lookup"><span data-stu-id="d4534-332">The new performance improvements focus on both of these factors.</span></span>

<a id="_Toc_perf_2"></a>
#### <a name="requirements-for-new-performance-features"></a><span data-ttu-id="d4534-333">新的效能功能的需求</span><span class="sxs-lookup"><span data-stu-id="d4534-333">Requirements for New Performance Features</span></span>

<span data-ttu-id="d4534-334">新功能的需求可以分成下列類別：</span><span class="sxs-lookup"><span data-stu-id="d4534-334">The requirements for the new features can be broken down into these categories:</span></span>

- <span data-ttu-id="d4534-335">執行.NET Framework 4 的增強功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-335">Improvements that run on the .NET Framework 4.</span></span>
- <span data-ttu-id="d4534-336">改良功能，需要.NET Framework 4.5，但是可以在任何版本的 Windows 上執行。</span><span class="sxs-lookup"><span data-stu-id="d4534-336">Improvements that require the .NET Framework 4.5 but can run on any version of Windows.</span></span>
- <span data-ttu-id="d4534-337">只能搭配 Windows 8 上執行的.NET Framework 4.5 可用的增強功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-337">Improvements that are available only with .NET Framework 4.5 running on Windows 8.</span></span>

<span data-ttu-id="d4534-338">效能會隨著的改進，您便能夠啟用每個層級。</span><span class="sxs-lookup"><span data-stu-id="d4534-338">Performance increases with each level of improvement that you are able to enable.</span></span>

<span data-ttu-id="d4534-339">某些.NET Framework 4.5 功能改進利用更廣泛適用於其他情況下的效能功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-339">Some of the .NET Framework 4.5 improvements take advantage of broader performance features that apply to other scenarios as well.</span></span>

<a id="_Toc_perf_3"></a>
#### <a name="sharing-common-assemblies"></a><span data-ttu-id="d4534-340">共用通用的組件</span><span class="sxs-lookup"><span data-stu-id="d4534-340">Sharing Common Assemblies</span></span>

<span data-ttu-id="d4534-341">**需求**:.NET Framework 4 和 Visual Studio 11 開發人員預覽 SDK</span><span class="sxs-lookup"><span data-stu-id="d4534-341">**Requirement**: .NET Framework 4 and Visual Studio 11 Developer Preview SDK</span></span>

<span data-ttu-id="d4534-342">不同的站台伺服器上通常會使用相同的協助程式組件 （例如，組件從入門套件或範例應用程式）。</span><span class="sxs-lookup"><span data-stu-id="d4534-342">Different sites on a server often use the same helper assemblies (for example, assemblies from a starter kit or sample application).</span></span> <span data-ttu-id="d4534-343">每個站台會有自己的這些組件的複本，它的 Bin 目錄中。</span><span class="sxs-lookup"><span data-stu-id="d4534-343">Each site has its own copy of these assemblies in its Bin directory.</span></span> <span data-ttu-id="d4534-344">即使組件的物件程式碼相同，它們是實體上不同的組件，因此每個組件必須個別讀取站台冷啟動期間，且分別保存在記憶體中。</span><span class="sxs-lookup"><span data-stu-id="d4534-344">Even though the object code for the assemblies is identical, they're physically separate assemblies, so each assembly has to be read separately during cold site startup and kept separately in memory.</span></span>

<span data-ttu-id="d4534-345">實習的新功能可解決此效率不彰，並減少 RAM 需求和載入時間。</span><span class="sxs-lookup"><span data-stu-id="d4534-345">The new interning functionality solves this inefficiency and reduces both RAM requirements and load time.</span></span> <span data-ttu-id="d4534-346">實習可讓 Windows 在檔案系統中，讓每個組件的單一複本和個別站台 Bin 資料夾中的組件會取代單一複本的符號連結。</span><span class="sxs-lookup"><span data-stu-id="d4534-346">Interning lets Windows keep a single copy of each assembly in the file system, and individual assemblies in the site Bin folders are replaced with symbolic links to the single copy.</span></span> <span data-ttu-id="d4534-347">如果個別站台需要不同版本的組件，符號連結取代為新版本的組件，並會影響這個站台。</span><span class="sxs-lookup"><span data-stu-id="d4534-347">If an individual site needs a distinct version of the assembly, the symbolic link is replaced by the new version of the assembly, and only that site is affected.</span></span>

<span data-ttu-id="d4534-348">共用組件使用的符號連結需要新的工具，名為 aspnet\_intern.exe，可讓您建立和管理組件已經保留的存放區。</span><span class="sxs-lookup"><span data-stu-id="d4534-348">Sharing assemblies using symbolic links requires a new tool named aspnet\_intern.exe, which lets you create and manage the store of interned assemblies.</span></span> <span data-ttu-id="d4534-349">它提供在 Visual Studio 11 開發人員預覽 SDK 的一部分。</span><span class="sxs-lookup"><span data-stu-id="d4534-349">It is provided as a part of the Visual Studio 11 Developer Preview SDK.</span></span> <span data-ttu-id="d4534-350">(不過，它適用於具有只有安裝.NET Framework 4，假設您已安裝最新的系統[更新](https://support.microsoft.com/kb/2468871)。)</span><span class="sxs-lookup"><span data-stu-id="d4534-350">(However, it will work on a system that has only the .NET Framework 4 installed, assuming you have installed the latest [update](https://support.microsoft.com/kb/2468871).)</span></span>

<span data-ttu-id="d4534-351">若要確保所有合格的組件有實習，您可以執行 aspnet\_intern.exe 定期 （例如每週一次當做排程工作）。</span><span class="sxs-lookup"><span data-stu-id="d4534-351">To make sure all eligible assemblies have been interned, you run aspnet\_intern.exe periodically (for example, once a week as a scheduled task).</span></span> <span data-ttu-id="d4534-352">一般用法如下所示：</span><span class="sxs-lookup"><span data-stu-id="d4534-352">A typical use is as follows:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample14.cmd)]

<span data-ttu-id="d4534-353">若要查看所有選項，執行不含引數的工具。</span><span class="sxs-lookup"><span data-stu-id="d4534-353">To see all options, run the tool with no arguments.</span></span>

<a id="_Toc_perf_4"></a>
#### <a name="using-multi-core-jit-compilation-for-faster-startup"></a><span data-ttu-id="d4534-354">使用多核心 JIT 編譯的更快速啟動</span><span class="sxs-lookup"><span data-stu-id="d4534-354">Using multi-Core JIT compilation for faster startup</span></span>

<span data-ttu-id="d4534-355">**需求**:.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="d4534-355">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="d4534-356">為冷網站啟動時，組件沒有要讀取的磁碟，不僅站台必須是 JIT 編譯。</span><span class="sxs-lookup"><span data-stu-id="d4534-356">For a cold site startup, not only do assemblies have to be read from disk, but the site must be JIT-compiled.</span></span> <span data-ttu-id="d4534-357">針對複雜的站台，這可以加入明顯的延遲。</span><span class="sxs-lookup"><span data-stu-id="d4534-357">For a complex site, this can add significant delays.</span></span> <span data-ttu-id="d4534-358">.NET Framework 4.5 中新的一般用途技術可減少這些延遲分散到可用的處理器核心分配 JIT 編譯。</span><span class="sxs-lookup"><span data-stu-id="d4534-358">A new general-purpose technique in the .NET Framework 4.5 reduces these delays by spreading JIT-compilation across available processor cores.</span></span> <span data-ttu-id="d4534-359">它會盡可能，並儘可能及早使用期間蒐集資訊先前啟動的站台。</span><span class="sxs-lookup"><span data-stu-id="d4534-359">It does this as much and as early as possible by using information gathered during previous launches of the site.</span></span> <span data-ttu-id="d4534-360">這項功能所實作[System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/en-us/library/system.runtime.profileoptimization.startprofile(VS.110).aspx)方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-360">This functionality implemented by the [System.Runtime.ProfileOptimization.StartProfile](https://msdn.microsoft.com/en-us/library/system.runtime.profileoptimization.startprofile(VS.110).aspx) method.</span></span>

<span data-ttu-id="d4534-361">JIT 編譯使用多個核心預設會啟用在 ASP.NET 中，因此您不需要執行任何動作來利用這項功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-361">JIT-compiling using multiple cores is enabled by default in ASP.NET, so you do not need to do anything to take advantage of this feature.</span></span> <span data-ttu-id="d4534-362">如果您想要停用此功能，請在 Web.config 檔案中進行下列設定：</span><span class="sxs-lookup"><span data-stu-id="d4534-362">If you want to disable this feature, make the following setting in the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample15.xml)]

<a id="_Toc_perf_5"></a>
#### <a name="tuning-garbage-collection-to-optimize-for-memory"></a><span data-ttu-id="d4534-363">調整以最佳化記憶體回收</span><span class="sxs-lookup"><span data-stu-id="d4534-363">Tuning garbage collection to optimize for memory</span></span>

<span data-ttu-id="d4534-364">**需求**:.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="d4534-364">**Requirement**: .NET Framework 4.5</span></span>

<span data-ttu-id="d4534-365">當站台執行時，其使用記憶體回收行程 (GC) 可以是堆積的其記憶體耗用量的重要因素。</span><span class="sxs-lookup"><span data-stu-id="d4534-365">Once a site is running, its use of the garbage-collector (GC) heap can be a significant factor in its memory consumption.</span></span> <span data-ttu-id="d4534-366">任何記憶體回收行程，像是.NET Framework GC 讓 （頻率和精確度的集合） 的 CPU 時間和記憶體耗用量 （新的、 已釋放，或可以釋出的物件都會使用的額外空間） 之間的權衡取捨。</span><span class="sxs-lookup"><span data-stu-id="d4534-366">Like any garbage collector, the .NET Framework GC makes tradeoffs between CPU time (frequency and significance of collections) and memory consumption (extra space that is used for new, freed, or free-able objects).</span></span> <span data-ttu-id="d4534-367">為了舊版本中，我們已提供指引，有關如何設定來達成的正確平衡 GC (例如，請參閱[ASP.NET 2.0/3.5 共用裝載組態](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration))。</span><span class="sxs-lookup"><span data-stu-id="d4534-367">For previous releases, we have provided guidance on how to configure the GC to achieve the right balance (for example, see [ASP.NET 2.0/3.5 Shared Hosting Configuration](https://www.iis.net/learn/web-hosting/web-server-for-shared-hosting/aspnet-20-35-shared-hosting-configuration)).</span></span>

<span data-ttu-id="d4534-368">.NET Framework 4.5，而不是多個獨立設定，工作負載定義的組態設定為可用，可讓所有的先前建議的 GC 設定，以及新的微調，以提供每個網站的其他效能工作集。</span><span class="sxs-lookup"><span data-stu-id="d4534-368">For the .NET Framework 4.5, instead of multiple standalone settings, a workload-defined configuration setting is available that enables all of the previously recommended GC settings as well as new tuning that delivers additional performance for the per-site working set.</span></span>

<span data-ttu-id="d4534-369">若要啟用 GC 記憶體微調，請先 Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config 檔案中加入下列設定：</span><span class="sxs-lookup"><span data-stu-id="d4534-369">To enable GC memory tuning, add the following setting to the Windows\Microsoft.NET\Framework\v4.0.30319\aspnet.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample16.xml)]

<span data-ttu-id="d4534-370">(如果您是熟悉 aspnet.config 來變更先前的指引，請注意這項設定會取代舊的設定 — 比方說，不是需要設定 gcServer gcConcurrent、 等等。您不必移除舊的設定。）</span><span class="sxs-lookup"><span data-stu-id="d4534-370">(If you're familiar with the previous guidance for changes to aspnet.config, note that this setting replaces the old settings — for example, there is no need to set gcServer, gcConcurrent, etc. You do not have to remove the old settings.)</span></span>

<a id="_Toc_perf_6"></a>
#### <a name="prefetching-for-web-applications"></a><span data-ttu-id="d4534-371">預先擷取 web 應用程式</span><span class="sxs-lookup"><span data-stu-id="d4534-371">Prefetching for web applications</span></span>

<span data-ttu-id="d4534-372">**需求**： 在 Windows 8 上執行的.NET Framework 4.5</span><span class="sxs-lookup"><span data-stu-id="d4534-372">**Requirement**: .NET Framework 4.5 running on Windows 8</span></span>

<span data-ttu-id="d4534-373">Windows 具有數個版本，包含技術，又稱為[預先提取器](http://en.wikipedia.org/wiki/Prefetcher)，降低磁碟讀取的應用程式啟動。</span><span class="sxs-lookup"><span data-stu-id="d4534-373">For several releases, Windows has included a technology known as the [prefetcher](http://en.wikipedia.org/wiki/Prefetcher) that reduces the disk-read cost of application startup.</span></span> <span data-ttu-id="d4534-374">因為冷啟動用戶端應用程式的主要問題，這項技術尚未包含在 Windows Server 中，包括 server 所必要的元件。</span><span class="sxs-lookup"><span data-stu-id="d4534-374">Because cold startup is a problem predominantly for client applications, this technology has not been included in Windows Server, which includes only components that are essential to a server.</span></span> <span data-ttu-id="d4534-375">預先提取是立即可用的最新版本的 Windows Server，它可以在此最佳化在個別網站中的啟動。</span><span class="sxs-lookup"><span data-stu-id="d4534-375">Prefetching is now available in the latest version of Windows Server, where it can optimize the launch of individual websites.</span></span>

<span data-ttu-id="d4534-376">適用於 Windows Server 預設不會啟用預先提取器。</span><span class="sxs-lookup"><span data-stu-id="d4534-376">For Windows Server, the prefetcher is not enabled by default.</span></span> <span data-ttu-id="d4534-377">若要啟用並設定預先提取器適用之高密度 web 主機，執行以下幾組命令在命令列：</span><span class="sxs-lookup"><span data-stu-id="d4534-377">To enable and configure the prefetcher for high-density web hosting, run the following set of commands at the command line:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample17.cmd)]

<span data-ttu-id="d4534-378">然後，若要預先提取器整合 ASP.NET 應用程式中，加入下列 Web.config 檔案：</span><span class="sxs-lookup"><span data-stu-id="d4534-378">Then, to integrate the prefetcher with ASP.NET applications, add the following to the Web.config file:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample18.xml)]

<a id="_Toc318097385"></a>
## <a name="aspnet-web-forms"></a><span data-ttu-id="d4534-379">ASP.NET Web Form</span><span class="sxs-lookup"><span data-stu-id="d4534-379">ASP.NET Web Forms</span></span>

<a id="_Toc318097386"></a>
### <a name="strongly-typed-data-controls"></a><span data-ttu-id="d4534-380">強型別的資料控制項</span><span class="sxs-lookup"><span data-stu-id="d4534-380">Strongly Typed Data Controls</span></span>

<span data-ttu-id="d4534-381">在 ASP.NET 4.5 Web Form 會包含一些改善使用資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-381">In ASP.NET 4.5, Web Forms includes some improvements for working with data.</span></span> <span data-ttu-id="d4534-382">改進的第一個是強型別的資料控制項。</span><span class="sxs-lookup"><span data-stu-id="d4534-382">The first improvement is strongly typed data controls.</span></span> <span data-ttu-id="d4534-383">在舊版的 ASP.NET Web Form 控制項，為您顯示資料繫結值，使用*Eval*和資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="d4534-383">For Web Forms controls in previous versions of ASP.NET, you display a data-bound value using *Eval* and a data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample19.aspx)]

<span data-ttu-id="d4534-384">在您使用雙向資料繫結，*繫結*:</span><span class="sxs-lookup"><span data-stu-id="d4534-384">For two-way data binding, you use *Bind*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample20.aspx)]

<span data-ttu-id="d4534-385">在執行階段，這些呼叫會使用反映以讀取的指定成員值，並在標記中顯示結果。</span><span class="sxs-lookup"><span data-stu-id="d4534-385">At run time, these calls use reflection to read the value of the specified member and then display the result in the markup.</span></span> <span data-ttu-id="d4534-386">這種方法讓您更容易針對任意、 unshaped 資料的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="d4534-386">This approach makes it easy to data bind against arbitrary, unshaped data.</span></span>

<span data-ttu-id="d4534-387">不過，像這樣的資料繫結運算式不支援 IntelliSense 等功能的成員名稱、 瀏覽 （例如移至定義） 或在編譯時間檢查這些名稱。</span><span class="sxs-lookup"><span data-stu-id="d4534-387">However, data-binding expressions like this don't support features like IntelliSense for member names, navigation (like Go To Definition), or compile-time checking for these names.</span></span>

<span data-ttu-id="d4534-388">若要解決此問題，ASP.NET 4.5，請加入的控制項繫結至資料的資料型別宣告的功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-388">To address this issue, ASP.NET 4.5 adds the ability to declare the data type of the data that a control is bound to.</span></span> <span data-ttu-id="d4534-389">您使用新*ItemType*屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-389">You do this using the new *ItemType* property.</span></span> <span data-ttu-id="d4534-390">當您設定這個屬性時，兩個新的型別的變數就可以使用資料繫結運算式的範圍：*項目*和*BindItem*。</span><span class="sxs-lookup"><span data-stu-id="d4534-390">When you set this property, two new typed variables are available in the scope of data-binding expressions: *Item* and *BindItem*.</span></span> <span data-ttu-id="d4534-391">因為變數強型別，您會取得 Visual Studio 程式開發體驗完整的優點。</span><span class="sxs-lookup"><span data-stu-id="d4534-391">Because the variables are strongly typed, you get the full benefits of the Visual Studio development experience.</span></span>


<span data-ttu-id="d4534-392">針對雙向資料繫結運算式，使用*BindItem*變數：</span><span class="sxs-lookup"><span data-stu-id="d4534-392">For two-way data-binding expressions, use the *BindItem* variable:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample21.aspx)]


<span data-ttu-id="d4534-393">ASP.NET Web Form framework 中大部分支援資料繫結的控制項已更新為支援*ItemType*屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-393">Most controls in the ASP.NET Web Forms framework that support data binding have been updated to support the *ItemType* property.</span></span>

<a id="_Toc318097387"></a>
### <a name="model-binding"></a><span data-ttu-id="d4534-394">模型繫結</span><span class="sxs-lookup"><span data-stu-id="d4534-394">Model Binding</span></span>

<span data-ttu-id="d4534-395">模型繫結延伸 ASP.NET Web Form 控制項中使用程式碼為重心資料存取的資料繫結。</span><span class="sxs-lookup"><span data-stu-id="d4534-395">Model binding extends data binding in ASP.NET Web Forms controls to work with code-focused data access.</span></span> <span data-ttu-id="d4534-396">其中包含從概念*ObjectDataSource*控制項與 ASP.NET MVC 中的模型繫結。</span><span class="sxs-lookup"><span data-stu-id="d4534-396">It incorporates concepts from the *ObjectDataSource* control and from model binding in ASP.NET MVC.</span></span>

<a id="_Toc318097388"></a>
#### <a name="selecting-data"></a><span data-ttu-id="d4534-397">選取資料</span><span class="sxs-lookup"><span data-stu-id="d4534-397">Selecting data</span></span>

<span data-ttu-id="d4534-398">若要設定要用來選取資料的模型繫結資料控制項，您將控制項的*SelectMethod*網頁的程式碼中的方法名稱的屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-398">To configure a data control to use model binding to select data, you set the control's *SelectMethod* property to the name of a method in the page's code.</span></span> <span data-ttu-id="d4534-399">資料控制項在頁面生命週期中的適當時間呼叫的方法，並自動繫結傳回的資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-399">The data control calls the method at the appropriate time in the page life cycle and automatically binds the returned data.</span></span> <span data-ttu-id="d4534-400">沒有不需要明確地呼叫*DataBind*方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-400">There's no need to explicitly call the *DataBind* method.</span></span>

<span data-ttu-id="d4534-401">在下列範例中， *GridView*控制項設定為使用名為的方法*GetCategories*:</span><span class="sxs-lookup"><span data-stu-id="d4534-401">In the following example, the *GridView* control is configured to use a method named *GetCategories*:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample22.aspx)]

<span data-ttu-id="d4534-402">您建立*GetCategories*網頁的程式碼中的方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-402">You create the *GetCategories* method in the page's code.</span></span> <span data-ttu-id="d4534-403">簡單的選取作業的方法不需要任何參數，而且應該傳回*IEnumerable*或*IQueryable*物件。</span><span class="sxs-lookup"><span data-stu-id="d4534-403">For a simple select operation, the method needs no parameters and should return an *IEnumerable* or *IQueryable* object.</span></span> <span data-ttu-id="d4534-404">如果新*ItemType*屬性設定 (這可讓強型別資料繫結運算式，如底下所說明[強型別資料控制項](#_Toc318097386)稍早)，這些介面的泛型版本應該傳回 — *IEnumerable&lt;T&gt;* 或*IQueryable&lt;T&gt;*，與*T*比對的型別參數*ItemType*屬性 (例如， *IQueryable&lt;類別&gt;*)。</span><span class="sxs-lookup"><span data-stu-id="d4534-404">If the new *ItemType* property is set (which enables strongly typed data-binding expressions, as explained under [Strongly Typed Data Controls](#_Toc318097386) earlier), the generic versions of these interfaces should be returned — *IEnumerable&lt;T&gt;* or *IQueryable&lt;T&gt;*, with the *T* parameter matching the type of the *ItemType* property (for example, *IQueryable&lt;Category&gt;*).</span></span>

<span data-ttu-id="d4534-405">下列範例顯示的程式碼*GetCategories*方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-405">The following example shows the code for a *GetCategories* method.</span></span> <span data-ttu-id="d4534-406">此範例使用 Northwind 範例資料庫使用 Entity Framework Code First 模型。</span><span class="sxs-lookup"><span data-stu-id="d4534-406">This example uses the Entity Framework Code First model with the Northwind sample database.</span></span> <span data-ttu-id="d4534-407">程式碼可確保查詢會傳回詳細資料，藉由每個類別目錄的相關產品*Include*方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-407">The code makes sure that the query returns details of the related products for each category by way of the *Include* method.</span></span> <span data-ttu-id="d4534-408">(這可確保*TemplateField*標記中的項目顯示每個類別目錄中的產品計數，而不需要[n + 1 選取](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem)。)</span><span class="sxs-lookup"><span data-stu-id="d4534-408">(This ensures that the *TemplateField* element in the markup displays the count of products in each category without requiring an [n+1 select](http://stackoverflow.com/questions/97197/what-is-the-n1-selects-problem).)</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample23.cs)]

<span data-ttu-id="d4534-409">當頁面執行，則*GridView*控制呼叫*GetCategories*方法自動呈現傳回的資料，使用已設定的欄位：</span><span class="sxs-lookup"><span data-stu-id="d4534-409">When the page runs, the *GridView* control calls the *GetCategories* method automatically and renders the returned data using the configured fields:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.png)

<span data-ttu-id="d4534-410">因為選取的方法會傳回*IQueryable*物件*GridView*控制項可以進一步操作查詢，然後再執行它。</span><span class="sxs-lookup"><span data-stu-id="d4534-410">Because the select method returns an *IQueryable* object, the *GridView* control can further manipulate the query before executing it.</span></span> <span data-ttu-id="d4534-411">例如， *GridView*控制可以加入查詢運算式的排序和分頁所傳回*IQueryable*物件之前執行，讓這些作業都是透過基礎LINQ 提供者。</span><span class="sxs-lookup"><span data-stu-id="d4534-411">For example, the *GridView* control can add query expressions for sorting and paging to the returned *IQueryable* object before it is executed, so that those operations are performed by the underlying LINQ provider.</span></span> <span data-ttu-id="d4534-412">在此情況下，Entity Framework 可確保這些作業將會在資料庫中。</span><span class="sxs-lookup"><span data-stu-id="d4534-412">In this case, Entity Framework will ensure those operations are performed in the database.</span></span>

<span data-ttu-id="d4534-413">下列範例所示*GridView*修改成允許排序和分頁的控制項：</span><span class="sxs-lookup"><span data-stu-id="d4534-413">The following example shows the *GridView* control modified to allow sorting and paging:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample24.aspx)]

<span data-ttu-id="d4534-414">現在當執行網頁時，控制項可以確定會顯示資料的目前頁面，依所選資料行：</span><span class="sxs-lookup"><span data-stu-id="d4534-414">Now when the page runs, the control can make sure that only the current page of data is displayed and that it's ordered by the selected column:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.png)

<span data-ttu-id="d4534-415">若要篩選傳回的資料，參數必須加入至 select 方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-415">To filter the returned data, parameters have to be added to the select method.</span></span> <span data-ttu-id="d4534-416">模型繫結，在執行階段，將會填入這些參數，您可以使用它們來傳回資料之前修改查詢。</span><span class="sxs-lookup"><span data-stu-id="d4534-416">These parameters will be populated by the model binding at run time, and you can use them to alter the query before returning the data.</span></span>

<span data-ttu-id="d4534-417">例如，假設您想要的查詢字串中輸入關鍵字來讓使用者篩選產品。</span><span class="sxs-lookup"><span data-stu-id="d4534-417">For example, assume that you want to let users filter products by entering a keyword in the query string.</span></span> <span data-ttu-id="d4534-418">您可以將參數加入至方法及更新程式碼以使用參數值：</span><span class="sxs-lookup"><span data-stu-id="d4534-418">You can add a parameter to the method and update the code to use the parameter value:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample25.cs)]

<span data-ttu-id="d4534-419">這個程式碼包含*其中*運算式，如果提供值給*關鍵字*，然後傳回查詢結果。</span><span class="sxs-lookup"><span data-stu-id="d4534-419">This code includes a *Where* expression if a value is provided for *keyword* and then returns the query results.</span></span>

<a id="_Toc318097389"></a>
#### <a name="value-providers"></a><span data-ttu-id="d4534-420">值提供者</span><span class="sxs-lookup"><span data-stu-id="d4534-420">Value providers</span></span>

<span data-ttu-id="d4534-421">前一個範例不是特定之位置的值*關鍵字*參數來自。</span><span class="sxs-lookup"><span data-stu-id="d4534-421">The previous example was not specific about where the value for the *keyword* parameter was coming from.</span></span> <span data-ttu-id="d4534-422">若要指出這項資訊，您可以使用 參數屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-422">To indicate this information, you can use a parameter attribute.</span></span> <span data-ttu-id="d4534-423">對於此範例中，您可以使用*QueryStringAttribute*中類別*System.Web.ModelBinding*命名空間：</span><span class="sxs-lookup"><span data-stu-id="d4534-423">For this example, you can use the *QueryStringAttribute* class that's in the *System.Web.ModelBinding* namespace:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample26.cs)]

<span data-ttu-id="d4534-424">這會指示模型繫結至繫結的查詢字串中的值再試一次*關鍵字*在執行階段參數。</span><span class="sxs-lookup"><span data-stu-id="d4534-424">This instructs model binding to try to bind a value from the query string to the *keyword* parameter at run time.</span></span> <span data-ttu-id="d4534-425">（這可能需要執行類型轉換，雖然它不在此情況下。）如果無法提供值的類型是不可為 null，則會擲回例外狀況。</span><span class="sxs-lookup"><span data-stu-id="d4534-425">(This might involve performing type conversion, although it doesn't in this case.) If a value cannot be provided and the type is non-nullable, an exception is thrown.</span></span>

<span data-ttu-id="d4534-426">這些方法的值的來源指值提供者，以及參數屬性，以指出要使用哪些值提供者指值提供者屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-426">The sources of values for these methods are referred to as value providers, and the parameter attributes that indicate which value provider to use are referred to as value provider attributes.</span></span> <span data-ttu-id="d4534-427">Web Form 會在 Web Form 應用程式，例如查詢字串、 cookies、 表單值、 控制項、 檢視狀態、 工作階段狀態，以及設定檔屬性包含值提供者及所有的一般使用者輸入的來源對應屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-427">Web Forms will include value providers and corresponding attributes for all of the typical sources of user input in a Web Forms application, such as the query string, cookies, form values, controls, view state, session state, and profile properties.</span></span> <span data-ttu-id="d4534-428">您也可以撰寫自訂值提供者。</span><span class="sxs-lookup"><span data-stu-id="d4534-428">You can also write custom value providers.</span></span>

<span data-ttu-id="d4534-429">根據預設，參數名稱用於做為索引鍵的值提供者集合中尋找的值。</span><span class="sxs-lookup"><span data-stu-id="d4534-429">By default, the parameter name is used as the key to find a value in the value provider collection.</span></span> <span data-ttu-id="d4534-430">在範例中，程式碼會尋找名為關鍵字的查詢字串值 (例如，~ / default.aspx?keyword=chef)。</span><span class="sxs-lookup"><span data-stu-id="d4534-430">In the example, the code will look for a query-string value named keyword (for example, ~/default.aspx?keyword=chef).</span></span> <span data-ttu-id="d4534-431">您可以將它做為引數傳遞給參數屬性來指定自訂的索引鍵。</span><span class="sxs-lookup"><span data-stu-id="d4534-431">You can specify a custom key by passing it as an argument to the parameter attribute.</span></span> <span data-ttu-id="d4534-432">例如，若要使用名為 q 的查詢字串變數的值，您無法這樣做：</span><span class="sxs-lookup"><span data-stu-id="d4534-432">For example, to use the value of the query-string variable named q, you could do this:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample27.cs)]

<span data-ttu-id="d4534-433">如果此方法是在頁面的程式碼中，使用者可以藉由傳遞使用的查詢字串關鍵字篩選的結果：</span><span class="sxs-lookup"><span data-stu-id="d4534-433">If this method is in the page's code, users can filter the results by passing a keyword using the query string:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.png)

<span data-ttu-id="d4534-434">模型繫結會完成您原本必須以手動方式編碼的許多工作： 讀取的值、 檢查 null 值、 嘗試將它轉換成適當型別、 檢查轉換是否成功，以及最後，使用中的值查詢。</span><span class="sxs-lookup"><span data-stu-id="d4534-434">Model binding accomplishes many tasks that you would otherwise have to code by hand: reading the value, checking for a null value, attempting to convert it to the appropriate type, checking whether the conversion was successful, and finally, using the value in the query.</span></span> <span data-ttu-id="d4534-435">模型繫結的結果，最少的程式碼和重複使用的功能整個應用程式的能力。</span><span class="sxs-lookup"><span data-stu-id="d4534-435">Model binding results in far less code and in the ability to reuse the functionality throughout your application.</span></span>

<a id="_Toc318097390"></a>
#### <a name="filtering-by-values-from-a-control"></a><span data-ttu-id="d4534-436">篩選控制項中的值</span><span class="sxs-lookup"><span data-stu-id="d4534-436">Filtering by values from a control</span></span>

<span data-ttu-id="d4534-437">假設您想要擴充範例以讓使用者從下拉式清單中選擇篩選值。</span><span class="sxs-lookup"><span data-stu-id="d4534-437">Suppose you want to extend the example to let the user choose a filter value from a drop-down list.</span></span> <span data-ttu-id="d4534-438">標記中加入下列下拉式清單，然後將它設定為從另一個使用的方法取得其資料*SelectMethod*屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-438">Add the following drop-down list to the markup and configure it to get its data from another method using the *SelectMethod* property:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample28.aspx)]

<span data-ttu-id="d4534-439">通常您也會將*EmptyDataTemplate*元素*GridView*控制，讓控制項將會顯示一則訊息，如果找不到任何相符的產品：</span><span class="sxs-lookup"><span data-stu-id="d4534-439">Typically you would also add an *EmptyDataTemplate* element to the *GridView* control so that the control will display a message if no matching products are found:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample29.aspx)]

<span data-ttu-id="d4534-440">在網頁程式碼中，加入新的選取下拉式清單的方法：</span><span class="sxs-lookup"><span data-stu-id="d4534-440">In the page code, add the new select method for the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample30.cs)]

<span data-ttu-id="d4534-441">最後，更新*GetProducts*選取包含從下拉式清單選取類別目錄識別碼的新參數的方法：</span><span class="sxs-lookup"><span data-stu-id="d4534-441">Finally, update the *GetProducts* select method to take a new parameter that contains the ID of the selected category from the drop-down list:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample31.cs)]

<span data-ttu-id="d4534-442">現在當執行網頁時，使用者可以從下拉式清單中，選取類別和*GridView*控制項是自動重新繫結以顯示篩選過的資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-442">Now when the page runs, users can select a category from the drop-down list, and the *GridView* control is automatically re-bound to show the filtered data.</span></span> <span data-ttu-id="d4534-443">這可能是因為模型繫結會追蹤選取的方法參數的值，並偵測回傳之後是否已變更任何參數值。</span><span class="sxs-lookup"><span data-stu-id="d4534-443">This is possible because model binding tracks the values of parameters for select methods and detects whether any parameter value has changed after a postback.</span></span> <span data-ttu-id="d4534-444">如果是的話，則模型繫結會強制相關聯的資料控制項重新繫結至資料。</span><span class="sxs-lookup"><span data-stu-id="d4534-444">If so, model binding forces the associated data control to re-bind to the data.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.png)

<a id="_Toc318097391"></a>
### <a name="html-encoded-data-binding-expressions"></a><span data-ttu-id="d4534-445">HTML 編碼的資料繫結運算式</span><span class="sxs-lookup"><span data-stu-id="d4534-445">HTML Encoded Data-Binding Expressions</span></span>

<span data-ttu-id="d4534-446">您可以立即進行 HTML 編碼的資料繫結運算式的結果。</span><span class="sxs-lookup"><span data-stu-id="d4534-446">You can now HTML-encode the result of data-binding expressions.</span></span> <span data-ttu-id="d4534-447">加入冒號 （:） 的結尾&lt;%# 前置詞標示的資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="d4534-447">Add a colon (:) to the end of the &lt;%# prefix that marks the data-binding expression:</span></span>

[!code-aspx[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample32.aspx)]

<a id="_Toc318097392"></a>
### <a name="unobtrusive-validation"></a><span data-ttu-id="d4534-448">不顯眼的驗證</span><span class="sxs-lookup"><span data-stu-id="d4534-448">Unobtrusive Validation</span></span>

<span data-ttu-id="d4534-449">您現在可以設定要使用不顯眼的 JavaScript 用戶端驗證邏輯的內建的驗證程式控制項。</span><span class="sxs-lookup"><span data-stu-id="d4534-449">You can now configure the built-in validator controls to use unobtrusive JavaScript for client-side validation logic.</span></span> <span data-ttu-id="d4534-450">這會大幅減少呈現內嵌在網頁標記中的 JavaScript，並減少整體的頁面大小。</span><span class="sxs-lookup"><span data-stu-id="d4534-450">This significantly reduces the amount of JavaScript rendered inline in the page markup and reduces the overall page size.</span></span> <span data-ttu-id="d4534-451">您可以設定不顯眼的 JavaScript 驗證程式控制項的任何一種：</span><span class="sxs-lookup"><span data-stu-id="d4534-451">You can configure unobtrusive JavaScript for validator controls in any of these ways:</span></span>

- <span data-ttu-id="d4534-452">全域藉由新增下列設定加入 *&lt;appSettings&gt;*  Web.config 檔案中的項目：</span><span class="sxs-lookup"><span data-stu-id="d4534-452">Globally by adding the following setting to the *&lt;appSettings&gt;* element in the Web.config file:</span></span> 

    [!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample33.xml)]
- <span data-ttu-id="d4534-453">全域藉由設定靜態*System.Web.UI.ValidationSettings.UnobtrusiveValidationMode*屬性*UnobtrusiveValidationMode.WebForms* (通常在*應用程式\_啟動*Global.asax 檔中的方法)。</span><span class="sxs-lookup"><span data-stu-id="d4534-453">Globally by setting the static *System.Web.UI.ValidationSettings.UnobtrusiveValidationMode* property to *UnobtrusiveValidationMode.WebForms* (typically in the *Application\_Start* method in the Global.asax file).</span></span>
- <span data-ttu-id="d4534-454">個別的頁面，藉由設定新*UnobtrusiveValidationMode*屬性*頁面*類別*UnobtrusiveValidationMode.WebForms*。</span><span class="sxs-lookup"><span data-stu-id="d4534-454">Individually for a page by setting the new *UnobtrusiveValidationMode* property of the *Page* class to *UnobtrusiveValidationMode.WebForms*.</span></span>

<a id="_Toc318097393"></a>
### <a name="html5-updates"></a><span data-ttu-id="d4534-455">HTML5 更新</span><span class="sxs-lookup"><span data-stu-id="d4534-455">HTML5 Updates</span></span>

<span data-ttu-id="d4534-456">某些已改良 Web form 伺服器控制項，以充分利用新的 HTML5 功能：</span><span class="sxs-lookup"><span data-stu-id="d4534-456">Some improvements have been made to Web Forms server controls to take advantage of new features of HTML5:</span></span>

- <span data-ttu-id="d4534-457">*TextMode*屬性*文字方塊*控制項已更新以支援新 HTML5 輸入的類型，例如*電子郵件*， *datetime*，和等等。</span><span class="sxs-lookup"><span data-stu-id="d4534-457">The *TextMode* property of the *TextBox* control has been updated to support the new HTML5 input types like *email*, *datetime*, and so on.</span></span>
- <span data-ttu-id="d4534-458">*檔案上傳*控制現在支援多個檔案上傳支援這項功能 HTML5 的瀏覽器中。</span><span class="sxs-lookup"><span data-stu-id="d4534-458">The *FileUpload* control now supports multiple file uploads from browsers that support this HTML5 feature.</span></span>
- <span data-ttu-id="d4534-459">驗證程式控制現在支援驗證 HTML5 的輸入項目。</span><span class="sxs-lookup"><span data-stu-id="d4534-459">Validator controls now support validating HTML5 input elements.</span></span>
- <span data-ttu-id="d4534-460">新的 HTML5 項目具有屬性代表 URL 現在支援 runat ="server"。</span><span class="sxs-lookup"><span data-stu-id="d4534-460">New HTML5 elements that have attributes that represent a URL now support runat="server".</span></span> <span data-ttu-id="d4534-461">如此一來，您可以使用 ASP.NET 慣例在 URL 路徑，例如 ~ 來表示應用程式根目錄運算子 (例如，&lt;視訊 runat ="server"src="~/myVideo.wmv"/&gt;)。</span><span class="sxs-lookup"><span data-stu-id="d4534-461">As a result, you can use ASP.NET conventions in URL paths, like the ~ operator to represent the application root (for example, &lt;video runat="server" src="~/myVideo.wmv" /&gt;).</span></span>
- <span data-ttu-id="d4534-462">*UpdatePanel*控制項問題已獲得修正，以支援張貼 HTML5 輸入的欄位。</span><span class="sxs-lookup"><span data-stu-id="d4534-462">The *UpdatePanel* control has been fixed to support posting HTML5 input fields.</span></span>

<a id="_Toc318097394"></a>
## <a name="aspnet-mvc-4"></a><span data-ttu-id="d4534-463">ASP.NET MVC 4</span><span class="sxs-lookup"><span data-stu-id="d4534-463">ASP.NET MVC 4</span></span>

<span data-ttu-id="d4534-464">ASP.NET MVC 4 Beta 已隨附於 Visual Studio 11 Beta。</span><span class="sxs-lookup"><span data-stu-id="d4534-464">ASP.NET MVC 4 Beta is now included with Visual Studio 11 Beta.</span></span> <span data-ttu-id="d4534-465">ASP.NET MVC 是運用 「 模型檢視控制器 (MVC) 模式開發高度可測試且可維護的 Web 應用程式的架構。</span><span class="sxs-lookup"><span data-stu-id="d4534-465">ASP.NET MVC is a framework for developing highly testable and maintainable Web applications by leveraging the Model-View-Controller (MVC) pattern.</span></span> <span data-ttu-id="d4534-466">ASP.NET MVC 4 可讓您更輕鬆地建立行動裝置的 Web 應用程式，並包含 ASP.NET Web API，可協助您建立可以連線到任何裝置的 HTTP 服務。</span><span class="sxs-lookup"><span data-stu-id="d4534-466">ASP.NET MVC 4 makes it easy to build applications for the mobile Web and includes ASP.NET Web API, which helps you build HTTP services that can reach any device.</span></span> <span data-ttu-id="d4534-467">如需詳細資訊，請參閱[ASP.NET MVC 4 版本資訊](mvc4-release-notes.md)。</span><span class="sxs-lookup"><span data-stu-id="d4534-467">For more information, see the [ASP.NET MVC 4 Release Notes](mvc4-release-notes.md).</span></span>

<a id="_Toc318097395"></a>
## <a name="aspnet-web-pages-2"></a><span data-ttu-id="d4534-468">ASP.NET Web Pages 2</span><span class="sxs-lookup"><span data-stu-id="d4534-468">ASP.NET Web Pages 2</span></span>

<span data-ttu-id="d4534-469">新功能包括下列各項：</span><span class="sxs-lookup"><span data-stu-id="d4534-469">New features include the following:</span></span>

- <span data-ttu-id="d4534-470">新增和更新的網站範本。</span><span class="sxs-lookup"><span data-stu-id="d4534-470">New and updated site templates.</span></span>
- <span data-ttu-id="d4534-471">加入伺服器端和用戶端驗證使用*驗證*協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-471">Adding server-side and client-side validation using the *Validation* helper.</span></span>
- <span data-ttu-id="d4534-472">若要註冊指令碼使用的資產管理員功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-472">The ability to register scripts using an assets manager.</span></span>
- <span data-ttu-id="d4534-473">啟用從 Facebook 和其他站台使用 OAuth 和 OpenID 登入。</span><span class="sxs-lookup"><span data-stu-id="d4534-473">Enabling logins from Facebook and other sites using OAuth and OpenID.</span></span>
- <span data-ttu-id="d4534-474">新增對應使用*對應*協助程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-474">Adding maps using the *Maps* helper.</span></span>
- <span data-ttu-id="d4534-475">執行網頁應用程式的並存。</span><span class="sxs-lookup"><span data-stu-id="d4534-475">Running Web Pages applications side-by-side.</span></span>
- <span data-ttu-id="d4534-476">行動裝置的轉譯頁面。</span><span class="sxs-lookup"><span data-stu-id="d4534-476">Rendering pages for mobile devices.</span></span>

<span data-ttu-id="d4534-477">如需有關這些功能和整頁的程式碼範例的詳細資訊，請參閱[Web Pages 2 beta 頂端功能](https://go.microsoft.com/fwlink/?LinkID=227824)。</span><span class="sxs-lookup"><span data-stu-id="d4534-477">For more information about these features and full-page code examples, see [The Top Features in Web Pages 2 Beta](https://go.microsoft.com/fwlink/?LinkID=227824).</span></span>

<a id="_Toc318097396"></a>
## <a name="visual-web-developer-11-beta"></a><span data-ttu-id="d4534-478">Visual Web Developer 11 Beta</span><span class="sxs-lookup"><span data-stu-id="d4534-478">Visual Web Developer 11 Beta</span></span>

<span data-ttu-id="d4534-479">本節提供針對 web 程式開發，Visual Web Developer 11 beta 版，並且 Visual Studio 2012 發行候選版本的改進功能的相關資訊。</span><span class="sxs-lookup"><span data-stu-id="d4534-479">This section provides information about improvements for web development in Visual Web Developer 11 Beta and Visual Studio 2012 Release Candidate.</span></span>

<a id="project-compatibility"></a>
### <a name="project-sharing-between-visual-studio-2010-and-visual-studio-2012-release-candidate-project-compatibility"></a><span data-ttu-id="d4534-480">Visual Studio 2010 和 Visual Studio 2012 發行候選版本 （專案相容性） 之間共用的專案</span><span class="sxs-lookup"><span data-stu-id="d4534-480">Project Sharing Between Visual Studio 2010 and Visual Studio 2012 Release Candidate (Project Compatibility)</span></span>

<span data-ttu-id="d4534-481">Visual Studio 2012 發行候選版本，直到在較新版的 Visual Studio 中開啟現有專案啟動轉換精靈。</span><span class="sxs-lookup"><span data-stu-id="d4534-481">Until Visual Studio 2012 Release Candidate, opening an existing project in a newer version of Visual Studio launched the Conversion Wizard.</span></span> <span data-ttu-id="d4534-482">這強制新的格式不相容的內容 （資產） 的專案及方案的升級。</span><span class="sxs-lookup"><span data-stu-id="d4534-482">This forced an upgrade of the content (assets) of a project and solution to new formats that were not backward compatible.</span></span> <span data-ttu-id="d4534-483">因此，在轉換後您無法開啟專案在舊版的 Visual Studio 中。</span><span class="sxs-lookup"><span data-stu-id="d4534-483">Therefore, after the conversion you could not open the project in the older version of Visual Studio.</span></span>

<span data-ttu-id="d4534-484">許多客戶有告訴我們這不是正確的方法。</span><span class="sxs-lookup"><span data-stu-id="d4534-484">Many customers have told us that this was not the right approach.</span></span> <span data-ttu-id="d4534-485">在 Visual Studio 11 Beta，我們現在支援共用專案和方案具有 Visual Studio 2010 SP1。</span><span class="sxs-lookup"><span data-stu-id="d4534-485">In Visual Studio 11 Beta, we now support sharing projects and solutions with Visual Studio 2010 SP1.</span></span> <span data-ttu-id="d4534-486">這表示，如果您在 Visual Studio 2012 發行候選版本中開啟 2010年專案，您將仍然能夠在 Visual Studio 2010 SP1 中開啟專案。</span><span class="sxs-lookup"><span data-stu-id="d4534-486">This means that if you open a 2010 project in Visual Studio 2012 Release Candidate, you will still be able to open the project in Visual Studio 2010 SP1.</span></span>

> [!NOTE]
> <span data-ttu-id="d4534-487">幾個類型的專案無法在 Visual Studio 2010 SP1 和 Visual Studio 2012 發行候選版本之間共用。</span><span class="sxs-lookup"><span data-stu-id="d4534-487">A few types of projects cannot be shared between Visual Studio 2010 SP1 and Visual Studio 2012 Release Candidate.</span></span> <span data-ttu-id="d4534-488">包括一些較舊的專案 （例如 ASP.NET MVC 2 專案） 或專案 （例如安裝專案） 的特殊用途。</span><span class="sxs-lookup"><span data-stu-id="d4534-488">These include some older projects (such as ASP.NET MVC 2 projects) or projects for special purposes (such as Setup projects).</span></span>

<span data-ttu-id="d4534-489">當您在 Visual Studio 11 Beta 中的第一次開啟 Visual Studio 2010 SP1 Web 專案時，專案檔會加入下列屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-489">When you open a Visual Studio 2010 SP1 Web project for the first time in Visual Studio 11 Beta, the following properties are added to the project file:</span></span>

- <span data-ttu-id="d4534-490">FileUpgradeFlags</span><span class="sxs-lookup"><span data-stu-id="d4534-490">FileUpgradeFlags</span></span>
- <span data-ttu-id="d4534-491">UpgradeBackupLocation</span><span class="sxs-lookup"><span data-stu-id="d4534-491">UpgradeBackupLocation</span></span>
- <span data-ttu-id="d4534-492">OldToolsVersion</span><span class="sxs-lookup"><span data-stu-id="d4534-492">OldToolsVersion</span></span>
- <span data-ttu-id="d4534-493">VisualStudioVersion</span><span class="sxs-lookup"><span data-stu-id="d4534-493">VisualStudioVersion</span></span>
- <span data-ttu-id="d4534-494">VSToolsPath</span><span class="sxs-lookup"><span data-stu-id="d4534-494">VSToolsPath</span></span>

<span data-ttu-id="d4534-495">升級專案檔的程序會使用 FileUpgradeFlags、 UpgradeBackupLocation 和 OldToolsVersion。</span><span class="sxs-lookup"><span data-stu-id="d4534-495">FileUpgradeFlags, UpgradeBackupLocation, and OldToolsVersion are used by the process that upgrades the project file.</span></span> <span data-ttu-id="d4534-496">它們不會有影響使用 Visual Studio 2010 中的專案。</span><span class="sxs-lookup"><span data-stu-id="d4534-496">They have no impact on working with the project in Visual Studio 2010.</span></span>

<span data-ttu-id="d4534-497">VisualStudioVersion 是 MSBuild 4.5，指出目前專案的 Visual Studio 版本所使用的新屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-497">VisualStudioVersion is a new property used by MSBuild 4.5 that indicates the version of Visual Studio for the current project.</span></span> <span data-ttu-id="d4534-498">因為這個屬性不存在於在 MSBuild 4.0 中 （Visual Studio 2010 SP1 使用的 MSBuild 版本），我們會將插入專案檔的預設值。</span><span class="sxs-lookup"><span data-stu-id="d4534-498">Because this property didn't exist in MSBuild 4.0 (the version of MSBuild that Visual Studio 2010 SP1 uses), we inject a default value into the project file.</span></span>

<span data-ttu-id="d4534-499">VSToolsPath 屬性用來決定要從表示 MSBuildExtensionsPath32 設定的路徑匯入正確的.targets 檔案中。</span><span class="sxs-lookup"><span data-stu-id="d4534-499">The VSToolsPath property is used to determine the correct .targets file to import from the path represented by the MSBuildExtensionsPath32 setting.</span></span>

<span data-ttu-id="d4534-500">也有一些與匯入項目相關的變更。</span><span class="sxs-lookup"><span data-stu-id="d4534-500">There are also some changes related to Import elements.</span></span> <span data-ttu-id="d4534-501">為了支援這兩個版本的 Visual Studio 之間的相容性需要進行這些變更。</span><span class="sxs-lookup"><span data-stu-id="d4534-501">These changes are required in order to support compatibility between both versions of Visual Studio.</span></span>

> [!NOTE]
> <span data-ttu-id="d4534-502">如果專案 Visual Studio 2010 SP1 和 Visual Studio 11 Beta 之間兩個不同的電腦上共用，而且專案的應用程式中包含本機資料庫\_資料資料夾中，您必須確定 SQL server 資料庫所用的版本安裝在兩部電腦上。</span><span class="sxs-lookup"><span data-stu-id="d4534-502">If a project is being shared between Visual Studio 2010 SP1 and Visual Studio 11 Beta on two different computers, and if the project includes a local database in the App\_Data folder, you must make sure that the version of SQL Server used by the database is installed on both computers.</span></span>

<a id="Configuration_Changes_In_ASPNET45_Website_Templates"></a>
### <a name="configuration-changes-in-aspnet-45-website-templates"></a><span data-ttu-id="d4534-503">ASP.NET 4.5 的網站範本中的組態變更</span><span class="sxs-lookup"><span data-stu-id="d4534-503">Configuration Changes in ASP.NET 4.5 Website Templates</span></span>

<span data-ttu-id="d4534-504">預設值來進行下列變更，就有*Web.config*網站使用網站範本在 Visual Studio 2012 發行候選版本中建立的檔案：</span><span class="sxs-lookup"><span data-stu-id="d4534-504">The following changes have been made to the default *Web.config* file for site that are created using website templates in Visual Studio 2012 Release Candidate:</span></span>

- <span data-ttu-id="d4534-505">在`<httpRuntime>`項目，`encoderType`屬性現在會依預設設定為使用已加入至 ASP.NET AntiXSS 類型。</span><span class="sxs-lookup"><span data-stu-id="d4534-505">In the `<httpRuntime>` element, the `encoderType` attribute is now set by default to use the AntiXSS types that were added to ASP.NET.</span></span> <span data-ttu-id="d4534-506">如需詳細資訊，請參閱[AntiXSS 文件庫](#_Toc318097382)。</span><span class="sxs-lookup"><span data-stu-id="d4534-506">For details, see [AntiXSS Library](#_Toc318097382).</span></span>
- <span data-ttu-id="d4534-507">此外，在`<httpRuntime>`項目，`requestValidationMode`屬性設為"4.5"。</span><span class="sxs-lookup"><span data-stu-id="d4534-507">Also in the `<httpRuntime>` element, the `requestValidationMode` attribute is set to "4.5".</span></span> <span data-ttu-id="d4534-508">這表示根據預設，設定要求驗證會使用延後 （「 延遲 」） 的驗證。</span><span class="sxs-lookup"><span data-stu-id="d4534-508">This means that by default, request validation is configured to use deferred ("lazy") validation.</span></span> <span data-ttu-id="d4534-509">如需詳細資訊，請參閱[新 ASP.NET 要求驗證功能](#_Toc318097379)。</span><span class="sxs-lookup"><span data-stu-id="d4534-509">For details, see [New ASP.NET Request Validation Features](#_Toc318097379).</span></span>
- <span data-ttu-id="d4534-510">`<modules>`元素`<system.webServer>`區段不包含`runAllManagedModulesForAllRequests`屬性。</span><span class="sxs-lookup"><span data-stu-id="d4534-510">The `<modules>` element of the `<system.webServer>` section does not contain a `runAllManagedModulesForAllRequests` attribute.</span></span> <span data-ttu-id="d4534-511">（預設值為 false）。這表示如果您使用 IIS 7，尚未更新為 SP1 版本，您可能會有問題中新的站台的路由。</span><span class="sxs-lookup"><span data-stu-id="d4534-511">(Its default value is false.) This means that if you are using a version of IIS 7 that has not been updated to SP1, you might have issues with routing in a new site.</span></span> <span data-ttu-id="d4534-512">如需詳細資訊，請參閱[在 IIS 7 進行 ASP.NET 路由中的原生支援](#Native_Support_In_IIS7_For_ASPNET_Routine)。</span><span class="sxs-lookup"><span data-stu-id="d4534-512">For more information, see [Native Support in IIS 7 for ASP.NET Routing](#Native_Support_In_IIS7_For_ASPNET_Routine).</span></span>

<span data-ttu-id="d4534-513">這些變更不會影響現有的應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-513">These changes do not affect existing applications.</span></span> <span data-ttu-id="d4534-514">不過，它們可能代表現有的網站和您建立使用新範本的 ASP.NET 4.5 的新網站之間的行為差異。</span><span class="sxs-lookup"><span data-stu-id="d4534-514">However, they might represent a difference in behavior between existing websites and new websites that you create for ASP.NET 4.5 using the new templates.</span></span>

<a id="Native_Support_In_IIS7_For_ASPNET_Routine"></a>
### <a name="native-support-in-iis-7-for-aspnet-routing"></a><span data-ttu-id="d4534-515">在 IIS 7 進行 ASP.NET 路由中的原生支援</span><span class="sxs-lookup"><span data-stu-id="d4534-515">Native Support in IIS 7 for ASP.NET Routing</span></span>

<span data-ttu-id="d4534-516">這不是 ASP.NET 的變更，但如果會影響您使用 IIS 7 未套用 SP1 更新版本的新網站專案範本中的變更。</span><span class="sxs-lookup"><span data-stu-id="d4534-516">This is not a change to ASP.NET as such, but a change in templates for new website projects that can affect you if you are working a version of IIS 7 that has not had the SP1 update applied.</span></span>

<span data-ttu-id="d4534-517">在 ASP.NET 中，您可以將下列組態設定加入應用程式，才能支援路由：</span><span class="sxs-lookup"><span data-stu-id="d4534-517">In ASP.NET, you can add the following configuration setting to applications in order to support routing:</span></span>

[!code-xml[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample34.xml?highlight=3)]

<span data-ttu-id="d4534-518">當**runAllManagedModulesForAllRequests**是 true，URL，例如`http://mysite/myapp/home`前往 ASP.NET，即使有沒有*.aspx*， *.mvc*，或類似的副檔名URL。</span><span class="sxs-lookup"><span data-stu-id="d4534-518">When **runAllManagedModulesForAllRequests** is true, a URL like `http://mysite/myapp/home` goes to ASP.NET, even though there is no *.aspx*, *.mvc*, or similar extension on the URL.</span></span>

<span data-ttu-id="d4534-519">已對 IIS 7 的更新可讓**runAllManagedModulesForAllRequests**不必要的設定，並支援 ASP.NET 路由原生。</span><span class="sxs-lookup"><span data-stu-id="d4534-519">An update that was made to IIS 7 makes the **runAllManagedModulesForAllRequests** setting unnecessary and supports ASP.NET routing natively.</span></span> <span data-ttu-id="d4534-520">(如需更新的資訊，請參閱 Microsoft 支援文章[可更新功能可讓某些 IIS 7.0 或 IIS 7.5 處理常式來處理要求的 Url 不以句號結束](https://support.microsoft.com/kb/980368)。)</span><span class="sxs-lookup"><span data-stu-id="d4534-520">(For information about the update, see the Microsoft Support article [An update is available that enables certain IIS 7.0 or IIS 7.5 handlers to handle requests whose URLs do not end with a period](https://support.microsoft.com/kb/980368).)</span></span>

<span data-ttu-id="d4534-521">如果您的網站在 IIS 7 上執行，而且如果 IIS 已經更新，您不需要設定**runAllManagedModulesForAllRequests**為 true。</span><span class="sxs-lookup"><span data-stu-id="d4534-521">If your website is running on IIS 7 and if IIS has been updated, you do not need to set **runAllManagedModulesForAllRequests** to true.</span></span> <span data-ttu-id="d4534-522">事實上，將它設定為 true 不建議，因為它會加入不必要處理負荷的要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-522">In fact, setting it to true is not recommended, because it adds unnecessary processing overhead to request.</span></span> <span data-ttu-id="d4534-523">這項設定為 true 時，所有要求，包括用於*.htm*， *.jpg*，和其他靜態檔案，也會經過 ASP.NET 要求管線。</span><span class="sxs-lookup"><span data-stu-id="d4534-523">When this setting is true, all requests, including those for *.htm*, *.jpg*, and other static files, also go through the ASP.NET request pipeline.</span></span>

<span data-ttu-id="d4534-524">如果您建立新的 ASP.NET 4.5 網站，使用 Visual Studio 2012 RC 中所提供的範本，為網站組態不包含**runAllManagedModulesForAllRequests**設定。</span><span class="sxs-lookup"><span data-stu-id="d4534-524">If you create a new ASP.NET 4.5 website using the templates that are provided in Visual Studio 2012 RC, the configuration for the website does not include the **runAllManagedModulesForAllRequests** setting.</span></span> <span data-ttu-id="d4534-525">這表示根據預設設定為 false。</span><span class="sxs-lookup"><span data-stu-id="d4534-525">This means that by default the setting is false.</span></span>

<span data-ttu-id="d4534-526">如果您之後執行的網站在 Windows 7 上未安裝 SP1，IIS 7 將不會包含所需的更新。</span><span class="sxs-lookup"><span data-stu-id="d4534-526">If you then run the website on Windows 7 without SP1 installed, IIS 7 will not include the required update.</span></span> <span data-ttu-id="d4534-527">因此，路由將無法運作，您會看到錯誤。</span><span class="sxs-lookup"><span data-stu-id="d4534-527">As a consequence, routing will not work and you will see errors.</span></span> <span data-ttu-id="d4534-528">如果您有問題，路由無法運作，您可以執行下列其中一個：</span><span class="sxs-lookup"><span data-stu-id="d4534-528">If you have a problem where routing does not work, you can do either the following:</span></span>

- <span data-ttu-id="d4534-529">更新 Windows 7 SP1，會將更新新增至 IIS 7。</span><span class="sxs-lookup"><span data-stu-id="d4534-529">Update Windows 7 to SP1, which will add the update to IIS 7.</span></span>
- <span data-ttu-id="d4534-530">安裝先前所列的 Microsoft 支援文章所述的更新。</span><span class="sxs-lookup"><span data-stu-id="d4534-530">Install the update that's described in the Microsoft Support article listed previously.</span></span>
- <span data-ttu-id="d4534-531">設定**runAllManagedModulesForAllRequests**設為 true，該網站的 Web.config 檔案中。</span><span class="sxs-lookup"><span data-stu-id="d4534-531">Set **runAllManagedModulesForAllRequests** to true in that website's Web.config file.</span></span> <span data-ttu-id="d4534-532">請注意，這會造成額外負擔的要求。</span><span class="sxs-lookup"><span data-stu-id="d4534-532">Note that this will add some overhead to requests.</span></span>

<a id="_Toc318097397"></a>
### <a name="html-editor"></a><span data-ttu-id="d4534-533">HTML 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-533">HTML Editor</span></span>

<a id="_Toc318097398"></a>
#### <a name="smart-tasks"></a><span data-ttu-id="d4534-534">智慧工作提示</span><span class="sxs-lookup"><span data-stu-id="d4534-534">Smart Tasks</span></span>

<span data-ttu-id="d4534-535">在設計檢視中，通常為伺服器控制項的複雜屬性有關聯的對話方塊和精靈，可讓您輕鬆設定它們。</span><span class="sxs-lookup"><span data-stu-id="d4534-535">In Design view, complex properties of server controls often have associated dialog boxes and wizards to make it easy to set them.</span></span> <span data-ttu-id="d4534-536">例如，您可以使用特殊的對話方塊加入至資料來源*中繼器*控制或將資料行加入*GridView*控制項。</span><span class="sxs-lookup"><span data-stu-id="d4534-536">For example, you can use a special dialog box to add a data source to a *Repeater* control or add columns to a *GridView* control.</span></span>

<span data-ttu-id="d4534-537">不過，這種類型的複雜屬性 UI 說明尚未在來源檢視中使用。</span><span class="sxs-lookup"><span data-stu-id="d4534-537">However, this type of UI help for complex properties has not been available in Source view.</span></span> <span data-ttu-id="d4534-538">因此，Visual Studio 11 導入智慧工作提示來源檢視。</span><span class="sxs-lookup"><span data-stu-id="d4534-538">Therefore, Visual Studio 11 introduces Smart Tasks for Source view.</span></span> <span data-ttu-id="d4534-539">智慧工作都在 C# 和 Visual Basic 編輯器中的常用功能的內容感知快速鍵。</span><span class="sxs-lookup"><span data-stu-id="d4534-539">Smart Tasks are context-aware shortcuts for commonly used features in the C# and Visual Basic editors.</span></span>

<span data-ttu-id="d4534-540">ASP.NET Web Form 控制項的智慧工作提示時顯示在上伺服器標記為小型字符插入點位於項目內：</span><span class="sxs-lookup"><span data-stu-id="d4534-540">For ASP.NET Web Forms controls, Smart Tasks appear on server tags as a small glyph when the insertion point is inside the element:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.png)

<span data-ttu-id="d4534-541">當您按一下圖像，或按 CTRL +，則會展開智慧工作。</span><span class="sxs-lookup"><span data-stu-id="d4534-541">The Smart Task expands when you click the glyph or press CTRL+.</span></span> <span data-ttu-id="d4534-542">（點），如同在程式碼編輯器。</span><span class="sxs-lookup"><span data-stu-id="d4534-542">(dot), just as in the code editors.</span></span> <span data-ttu-id="d4534-543">然後，它會顯示類似於在設計檢視中的智慧工作的捷徑。</span><span class="sxs-lookup"><span data-stu-id="d4534-543">It then displays shortcuts that are similar to the Smart Tasks in Design view.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image7.png)

<span data-ttu-id="d4534-544">例如，在上圖中的智慧工作會示範 GridView 工作選項。</span><span class="sxs-lookup"><span data-stu-id="d4534-544">For example, the Smart Task in the previous illustration shows the GridView Tasks options.</span></span> <span data-ttu-id="d4534-545">如果您選擇編輯資料行時，會顯示下列對話方塊：</span><span class="sxs-lookup"><span data-stu-id="d4534-545">If you choose Edit Columns, the following dialog box is displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image8.png)

<span data-ttu-id="d4534-546">對話方塊會設定相同的屬性填入您可以設定在設計檢視中。</span><span class="sxs-lookup"><span data-stu-id="d4534-546">Filling in the dialog box sets the same properties you can set in Design view.</span></span> <span data-ttu-id="d4534-547">當您按一下 [確定] 時，控制項的標記會更新以新的設定：</span><span class="sxs-lookup"><span data-stu-id="d4534-547">When you click OK, the markup for the control is updated with the new settings:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image9.png)

<a id="_Toc318097399"></a>
#### <a name="wai-aria-support"></a><span data-ttu-id="d4534-548">WAI ARIA 支援</span><span class="sxs-lookup"><span data-stu-id="d4534-548">WAI-ARIA support</span></span>

<span data-ttu-id="d4534-549">撰寫可存取的網站變得越來越重要。</span><span class="sxs-lookup"><span data-stu-id="d4534-549">Writing accessible websites is becoming increasingly important.</span></span> <span data-ttu-id="d4534-550">[WAI ARIA 協助工具標準](http://www.w3.org/WAI/intro/aria)定義開發人員撰寫可存取的網站的方式。</span><span class="sxs-lookup"><span data-stu-id="d4534-550">The [WAI-ARIA accessibility standard](http://www.w3.org/WAI/intro/aria) defines how developers should write accessible websites.</span></span> <span data-ttu-id="d4534-551">在 Visual Studio 現在完全支援這項標準。</span><span class="sxs-lookup"><span data-stu-id="d4534-551">This standard is now fully supported in Visual Studio.</span></span>

<span data-ttu-id="d4534-552">例如，*角色*屬性現在會有完整的 IntelliSense:</span><span class="sxs-lookup"><span data-stu-id="d4534-552">For example, the *role* attribute now has full IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image10.png)

<span data-ttu-id="d4534-553">WAI ARIA 標準也導入了前面會加上的屬性*aria-* ，可讓您加入的 HTML5 文件的語意。</span><span class="sxs-lookup"><span data-stu-id="d4534-553">The WAI-ARIA standard also introduces attributes that are prefixed with *aria-* that let you add semantics to an HTML5 document.</span></span> <span data-ttu-id="d4534-554">Visual Studio 也完全支援這些*aria-*屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-554">Visual Studio also fully supports these *aria-* attributes:</span></span>

<span data-ttu-id="d4534-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span><span class="sxs-lookup"><span data-stu-id="d4534-555">![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image11.png) ![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image12.png)</span></span>

<a id="_Toc318097400"></a>
#### <a name="new-html5-snippets"></a><span data-ttu-id="d4534-556">新的 HTML5 程式碼片段</span><span class="sxs-lookup"><span data-stu-id="d4534-556">New HTML5 snippets</span></span>

<span data-ttu-id="d4534-557">為了讓您更快速且更容易撰寫常用的 HTML5 標記，Visual Studio 會包含程式碼片段的數目。</span><span class="sxs-lookup"><span data-stu-id="d4534-557">To make it faster and easier to write commonly used HTML5 markup, Visual Studio includes a number of snippets.</span></span> <span data-ttu-id="d4534-558">範例是視訊的程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="d4534-558">An example is the video snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image13.png)

<span data-ttu-id="d4534-559">要叫用程式碼片段，按下 Tab 鍵會在 IntelliSense 中選取的項目時，兩次：</span><span class="sxs-lookup"><span data-stu-id="d4534-559">To invoke the snippet, press Tab twice when the element is selected in IntelliSense:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image14.png)

<span data-ttu-id="d4534-560">這會產生程式碼片段，您可以自訂。</span><span class="sxs-lookup"><span data-stu-id="d4534-560">This produces a snippet that you can customize.</span></span>

<a id="_Toc318097401"></a>
#### <a name="extract-to-user-control"></a><span data-ttu-id="d4534-561">擷取至使用者控制項</span><span class="sxs-lookup"><span data-stu-id="d4534-561">Extract to user control</span></span>

<span data-ttu-id="d4534-562">在大型網頁，它可以是不錯的想法，即可將個別項目移至使用者控制項。</span><span class="sxs-lookup"><span data-stu-id="d4534-562">In large web pages, it can be a good idea to move individual pieces into user controls.</span></span> <span data-ttu-id="d4534-563">這種形式的重構協助提升網頁的可讀性，並可簡化頁面結構。</span><span class="sxs-lookup"><span data-stu-id="d4534-563">This form of refactoring can help increase the readability of the page and can simplify the page structure.</span></span>

<span data-ttu-id="d4534-564">若要進行簡化，當您編輯原始碼檢視中的 Web Form 網頁時，您可以立即在頁面中選取文字、 以滑鼠右鍵按一下，，然後選擇 擷取至使用者控制項：</span><span class="sxs-lookup"><span data-stu-id="d4534-564">To make this easier, when you edit Web Forms pages in Source view, you can now select text in a page, right-click it, and then choose Extract to User Control:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image2.jpg)

<a id="_Toc318097402"></a>
#### <a name="intellisense-for-code-nuggets-in-attributes"></a><span data-ttu-id="d4534-565">屬性中的程式碼區塊的 IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d4534-565">IntelliSense for code nuggets in attributes</span></span>

<span data-ttu-id="d4534-566">Visual Studio 的伺服器端程式碼區塊中的任何頁面或控制項一律提供 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="d4534-566">Visual Studio has always provided IntelliSense for server-side code nuggets in any page or control.</span></span> <span data-ttu-id="d4534-567">現在 Visual Studio 會包含程式碼區塊，以及 HTML 屬性中的 IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="d4534-567">Now Visual Studio includes IntelliSense for code nuggets in HTML attributes as well.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image15.png)

<span data-ttu-id="d4534-568">這可讓您更輕鬆地建立資料繫結運算式：</span><span class="sxs-lookup"><span data-stu-id="d4534-568">This makes it easier to create data-binding expressions:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image16.png)

<a id="_Toc318097403"></a>
#### <a name="automatic-renaming-of-matching-tag-when-you-rename-an-opening-or-closing-tag"></a><span data-ttu-id="d4534-569">當您重新命名的開頭或結尾標記時，比對標記的自動重新命名</span><span class="sxs-lookup"><span data-stu-id="d4534-569">Automatic renaming of matching tag when you rename an opening or closing tag</span></span>

<span data-ttu-id="d4534-570">如果您重新命名的 HTML 項目 (例如，您將變更*div*標記為*標頭*標記)，則對應的開啟或關閉標記也會變更即時。</span><span class="sxs-lookup"><span data-stu-id="d4534-570">If you rename an HTML element (for example, you change a *div* tag to be a *header* tag), the corresponding opening or closing tag also changes in real time.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image17.png)

<span data-ttu-id="d4534-571">這有助於避免您忘了用來變更結尾標記，或變更錯誤的錯誤。</span><span class="sxs-lookup"><span data-stu-id="d4534-571">This helps avoid the error where you forget to change a closing tag or change the wrong one.</span></span>

<a id="_Toc318097404"></a>
#### <a name="event-handler-generation"></a><span data-ttu-id="d4534-572">事件處理常式產生</span><span class="sxs-lookup"><span data-stu-id="d4534-572">Event handler generation</span></span>

<span data-ttu-id="d4534-573">Visual Studio 現在會包含可協助您撰寫事件處理常式，並手動將其繫結的來源檢視中的功能。</span><span class="sxs-lookup"><span data-stu-id="d4534-573">Visual Studio now includes features in Source view to help you write event handlers and bind them manually.</span></span> <span data-ttu-id="d4534-574">如果您編輯原始碼檢視中的事件名稱，IntelliSense 會顯示&lt;建立新的事件&gt;，這將會建立事件處理常式中網頁的程式碼，具有正確的簽章：</span><span class="sxs-lookup"><span data-stu-id="d4534-574">If you are editing an event name in Source view, IntelliSense displays &lt;Create New Event&gt;, which will create an event handler in the page's code that has the right signature:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image3.jpg)

<span data-ttu-id="d4534-575">根據預設，此事件處理常式將事件處理方法的名稱使用控制項的 ID:</span><span class="sxs-lookup"><span data-stu-id="d4534-575">By default, the event handler will use the control's ID for the name of the event-handling method:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image4.jpg)

<span data-ttu-id="d4534-576">（在此情況下，在 C# 中)，產生的事件處理常式看起來像這樣：</span><span class="sxs-lookup"><span data-stu-id="d4534-576">The resulting event handler will look like this (in this case, in C#):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image18.png)

<a id="_Toc318097405"></a>
#### <a name="smart-indent"></a><span data-ttu-id="d4534-577">智慧型縮排</span><span class="sxs-lookup"><span data-stu-id="d4534-577">Smart indent</span></span>

<span data-ttu-id="d4534-578">當您按下 Enter 空白 HTML 項目內時，編輯器會將插入點放在正確的位置：</span><span class="sxs-lookup"><span data-stu-id="d4534-578">When you press Enter while inside an empty HTML element, the editor will put the insertion point in the right place:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image19.png)

<span data-ttu-id="d4534-579">如果您按下 Enter，在這個位置，結尾標記會向下移動，並縮排到符合的開頭標記。</span><span class="sxs-lookup"><span data-stu-id="d4534-579">If you press Enter in this location, the closing tag is moved down and indented to match the opening tag.</span></span> <span data-ttu-id="d4534-580">插入點也會縮排：</span><span class="sxs-lookup"><span data-stu-id="d4534-580">The insertion point is also indented:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image20.png)

<a id="_Toc318097406"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="d4534-581">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="d4534-581">Auto-reduce statement completion</span></span>

<span data-ttu-id="d4534-582">在 Visual Studio 現在篩選，根據您輸入的內容，使其顯示只有相關的選項中的 IntelliSense 清單：</span><span class="sxs-lookup"><span data-stu-id="d4534-582">The IntelliSense list in Visual Studio now filters based on what you type so that it displays only relevant options:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image21.png)

<span data-ttu-id="d4534-583">IntelliSense 也篩選依據的 IntelliSense 清單中的個別字首大寫。</span><span class="sxs-lookup"><span data-stu-id="d4534-583">IntelliSense also filters based on the title case of the individual words in the IntelliSense list.</span></span> <span data-ttu-id="d4534-584">例如，如果您輸入 「 dl"，則 dl 和 asp: DataList 將會顯示：</span><span class="sxs-lookup"><span data-stu-id="d4534-584">For example, if you type "dl", both dl and asp:DataList are displayed:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image22.png)

<span data-ttu-id="d4534-585">此功能讓您更快取得的已知的項目陳述式完成。</span><span class="sxs-lookup"><span data-stu-id="d4534-585">This feature makes it faster to get statement completion for known elements.</span></span>

<a id="_Toc318097407"></a>
### <a name="javascript-editor"></a><span data-ttu-id="d4534-586">JavaScript 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-586">JavaScript Editor</span></span>

<span data-ttu-id="d4534-587">Visual Studio 2012 發行候選版本中的 JavaScript 編輯器是全新的它可大幅提升使用 Visual Studio 中的 JavaScript 的體驗。</span><span class="sxs-lookup"><span data-stu-id="d4534-587">The JavaScript editor in Visual Studio 2012 Release Candidate is completely new and it greatly improves the experience of working with JavaScript in Visual Studio.</span></span>

<a id="_Toc318097408"></a>
#### <a name="code-outlining"></a><span data-ttu-id="d4534-588">程式碼大綱</span><span class="sxs-lookup"><span data-stu-id="d4534-588">Code outlining</span></span>

<span data-ttu-id="d4534-589">大綱區域現在會自動建立的所有函式，讓您可以摺疊的檔案不是與您目前的焦點相關的組件。</span><span class="sxs-lookup"><span data-stu-id="d4534-589">Outlining regions are now automatically created for all functions, allowing you to collapse parts of the file that aren't pertinent to your current focus.</span></span>

<a id="_Toc318097409"></a>
#### <a name="brace-matching"></a><span data-ttu-id="d4534-590">括號對稱</span><span class="sxs-lookup"><span data-stu-id="d4534-590">Brace matching</span></span>

<span data-ttu-id="d4534-591">當您將插入點放在左或右大括號上時，編輯器會反白顯示比對其中一個。</span><span class="sxs-lookup"><span data-stu-id="d4534-591">When you put the insertion point on an opening or closing brace, the editor highlights the matching one.</span></span>

<a id="_Toc318097410"></a>
#### <a name="go-to-definition"></a><span data-ttu-id="d4534-592">移至定義</span><span class="sxs-lookup"><span data-stu-id="d4534-592">Go to Definition</span></span>

<span data-ttu-id="d4534-593">移至定義命令可讓您跳到函式或變數的來源。</span><span class="sxs-lookup"><span data-stu-id="d4534-593">The Go to Definition command lets you jump to the source for a function or variable.</span></span>

<a id="_Toc318097411"></a>
#### <a name="ecmascript5-support"></a><span data-ttu-id="d4534-594">ECMAScript5 支援</span><span class="sxs-lookup"><span data-stu-id="d4534-594">ECMAScript5 support</span></span>

<span data-ttu-id="d4534-595">編輯器支援 ECMAScript5，說明 JavaScript 語言的標準的最新版本的新語法和應用程式開發介面。</span><span class="sxs-lookup"><span data-stu-id="d4534-595">The editor supports the new syntax and APIs in ECMAScript5, the latest version of the standard that describes the JavaScript language.</span></span>

<a id="_Toc318097412"></a>
#### <a name="dom-intellisense"></a><span data-ttu-id="d4534-596">DOM IntelliSense</span><span class="sxs-lookup"><span data-stu-id="d4534-596">DOM IntelliSense</span></span>

<span data-ttu-id="d4534-597">已改進 IntelliSense 的 DOM 應用程式開發介面，支援許多新 HTML5 應用程式開發介面包括*querySelector*，DOM 儲存體，跨文件訊息，和*畫布*。</span><span class="sxs-lookup"><span data-stu-id="d4534-597">IntelliSense for DOM APIs has been improved, with support for many new HTML5 APIs including *querySelector*, DOM Storage, cross-document messaging, and *canvas*.</span></span> <span data-ttu-id="d4534-598">單一的簡單 JavaScript 檔案，而非原生型別程式庫定義，現在是驅動 DOM IntelliSense。</span><span class="sxs-lookup"><span data-stu-id="d4534-598">DOM IntelliSense is now driven by a single simple JavaScript file, rather than by a native type library definition.</span></span> <span data-ttu-id="d4534-599">這可讓您輕鬆地擴充或取代。</span><span class="sxs-lookup"><span data-stu-id="d4534-599">This makes it easy to extend or replace.</span></span>

<a id="_Toc318097413"></a>
#### <a name="vsdoc-signature-overloads"></a><span data-ttu-id="d4534-600">VSDOC 簽章的多載</span><span class="sxs-lookup"><span data-stu-id="d4534-600">VSDOC signature overloads</span></span>

<span data-ttu-id="d4534-601">詳細的 IntelliSense 註解可以現在為宣告不同的 JavaScript 函式多載使用新*&lt;簽章&gt;*項目，如這個範例所示：</span><span class="sxs-lookup"><span data-stu-id="d4534-601">Detailed IntelliSense comments can now be declared for separate overloads of JavaScript functions by using the new *&lt;signature&gt;* element, as shown in this example:</span></span>

[!code-csharp[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample35.cs)]

<a id="_Toc318097414"></a>
#### <a name="implicit-references"></a><span data-ttu-id="d4534-602">隱含參考</span><span class="sxs-lookup"><span data-stu-id="d4534-602">Implicit references</span></span>

<span data-ttu-id="d4534-603">現在您可以將 JavaScript 檔案加入中央的清單會以隱含方式包含的檔案清單中任何特定的 JavaScript 檔案或區塊的參考，表示您可以使用 IntelliSense，為其內容。</span><span class="sxs-lookup"><span data-stu-id="d4534-603">You can now add JavaScript files to a central list that will be implicitly included in the list of files that any given JavaScript file or block references, meaning you'll get IntelliSense for its contents.</span></span> <span data-ttu-id="d4534-604">比方說，您可以將 jQuery 檔案新增至中央檔案的清單，且您可以 IntelliSense jQuery 函式中的檔案，任何 JavaScript 區塊是否已明確參考 (使用 / /&lt;參考 /&gt;) 與否。</span><span class="sxs-lookup"><span data-stu-id="d4534-604">For example, you can add jQuery files to the central list of files, and you'll get IntelliSense for jQuery functions in any JavaScript block of file, whether you've referenced it explicitly (using /// &lt;reference /&gt;) or not.</span></span>

<a id="_Toc318097415"></a>
### <a name="css-editor"></a><span data-ttu-id="d4534-605">CSS 編輯器</span><span class="sxs-lookup"><span data-stu-id="d4534-605">CSS Editor</span></span>

<a id="_Toc318097416"></a>
#### <a name="auto-reduce-statement-completion"></a><span data-ttu-id="d4534-606">自動減少陳述式完成</span><span class="sxs-lookup"><span data-stu-id="d4534-606">Auto-reduce statement completion</span></span>

<span data-ttu-id="d4534-607">用於根據 CSS 屬性的 CSS 現在篩選與所選取的結構描述支援的值的 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="d4534-607">The IntelliSense list for CSS now filters based on the CSS properties and values supported by the selected schema.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image23.png)

<span data-ttu-id="d4534-608">IntelliSense 也支援標題大小寫搜尋：</span><span class="sxs-lookup"><span data-stu-id="d4534-608">IntelliSense also supports title case searches:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image24.png)

<a id="_Toc318097417"></a>
#### <a name="hierarchical-indentation"></a><span data-ttu-id="d4534-609">階層式縮排</span><span class="sxs-lookup"><span data-stu-id="d4534-609">Hierarchical indentation</span></span>

<span data-ttu-id="d4534-610">CSS 編輯器使用縮排顯示階層式的規則，讓您有階層式的規則邏輯的組織方式的概觀。</span><span class="sxs-lookup"><span data-stu-id="d4534-610">The CSS editor uses indentation to display hierarchical rules, which gives you an overview of how the cascading rules are logically organized.</span></span> <span data-ttu-id="d4534-611">在下列範例中，#list 選取器是階層式清單的子系，因此縮排。</span><span class="sxs-lookup"><span data-stu-id="d4534-611">In the following example, the #list a selector is a cascading child of list and is therefore indented.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image25.png)

<span data-ttu-id="d4534-612">下列範例會示範更複雜的繼承：</span><span class="sxs-lookup"><span data-stu-id="d4534-612">The following example shows more complex inheritance:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image26.png)

<span data-ttu-id="d4534-613">規則的縮排是由其父系規則所決定。</span><span class="sxs-lookup"><span data-stu-id="d4534-613">The indentation of a rule is determined by its parent rules.</span></span> <span data-ttu-id="d4534-614">依預設，會啟用階層式縮排，但您可以停用 [選項] 對話方塊 （工具，從功能表列的選項）：</span><span class="sxs-lookup"><span data-stu-id="d4534-614">Hierarchical indentation is enabled by default, but you can disable it the Options dialog box (Tools, Options from the menu bar):</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image27.png)

<a id="_Toc318097418"></a>
#### <a name="css-hacks-support"></a><span data-ttu-id="d4534-615">CSS 缺失支援</span><span class="sxs-lookup"><span data-stu-id="d4534-615">CSS hacks support</span></span>

<span data-ttu-id="d4534-616">數百個真實世界的 CSS 檔案的分析顯示 CSS hack 是很常見，Visual Studio 現在支援最廣泛使用的項目。</span><span class="sxs-lookup"><span data-stu-id="d4534-616">Analysis of hundreds of real-world CSS files shows that CSS hacks are very common, and now Visual Studio supports the most widely used ones.</span></span> <span data-ttu-id="d4534-617">這項支援包括 IntelliSense 和驗證的星號 (\*) 和底線 (\_) 駭客屬性：</span><span class="sxs-lookup"><span data-stu-id="d4534-617">This support includes IntelliSense and validation of the star (\*) and underscore (\_) property hacks:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image28.png)

<span data-ttu-id="d4534-618">一般的選取器 hack 也支援階層式縮排維持即使它們會套用。</span><span class="sxs-lookup"><span data-stu-id="d4534-618">Typical selector hacks are also supported so that hierarchical indentation is maintained even when they are applied.</span></span> <span data-ttu-id="d4534-619">前面加上與選取器是用於目標 Internet Explorer 7 一般的選取器 hack  *\*： 第一個子系 + html*。</span><span class="sxs-lookup"><span data-stu-id="d4534-619">A typical selector hack used to target Internet Explorer 7 is to prepend a selector with *\*:first-child + html*.</span></span> <span data-ttu-id="d4534-620">使用該規則將會維護階層式縮排：</span><span class="sxs-lookup"><span data-stu-id="d4534-620">Using that rule will maintain the hierarchical indentation:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image29.png)

<a id="_Toc318097419"></a>
#### <a name="vendor-specific-schemas--moz---webkit"></a><span data-ttu-id="d4534-621">廠商特定的結構描述 (-moz-、-webkit)</span><span class="sxs-lookup"><span data-stu-id="d4534-621">Vendor specific schemas (-moz-, -webkit)</span></span>

<span data-ttu-id="d4534-622">CSS3 導入了許多屬性已實作不同的瀏覽器在不同的時間。</span><span class="sxs-lookup"><span data-stu-id="d4534-622">CSS3 introduces many properties that have been implemented by different browsers at different times.</span></span> <span data-ttu-id="d4534-623">先前，這會強制特定瀏覽器的程式碼開發人員使用廠商特有的語法。</span><span class="sxs-lookup"><span data-stu-id="d4534-623">This previously forced developers to code for specific browsers by using vendor-specific syntax.</span></span> <span data-ttu-id="d4534-624">這些瀏覽器特有的屬性現在會包含在 IntelliSense 中。</span><span class="sxs-lookup"><span data-stu-id="d4534-624">These browser-specific properties are now included in IntelliSense.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image30.png)

<a id="_Toc318097420"></a>
#### <a name="commenting-and-uncommenting-support"></a><span data-ttu-id="d4534-625">註解和取消的支援</span><span class="sxs-lookup"><span data-stu-id="d4534-625">Commenting and uncommenting support</span></span>

<span data-ttu-id="d4534-626">您現在可以註解，並取消註解使用相同的快速鍵 （Ctrl + K、 註解的 C 和 Ctrl + K、 您取消註解） 程式碼編輯器中使用的 CSS 規則。</span><span class="sxs-lookup"><span data-stu-id="d4534-626">You can now comment and uncomment CSS rules using the same shortcuts that you use in the code editor (Ctrl+K,C to comment and Ctrl+K,U to uncomment).</span></span>

<a id="_Toc318097421"></a>
#### <a name="color-picker"></a><span data-ttu-id="d4534-627">色彩選擇器</span><span class="sxs-lookup"><span data-stu-id="d4534-627">Color picker</span></span>

<span data-ttu-id="d4534-628">在舊版的 Visual Studio 中，IntelliSense 的色彩相關屬性所組成的命名的色彩值的下拉式清單。</span><span class="sxs-lookup"><span data-stu-id="d4534-628">In previous versions of Visual Studio, IntelliSense for color-related attributes consisted of a drop-down list of named color values.</span></span> <span data-ttu-id="d4534-629">該清單已取代的功能完整的色彩選擇器。</span><span class="sxs-lookup"><span data-stu-id="d4534-629">That list has been replaced by a full-featured color picker.</span></span>

<span data-ttu-id="d4534-630">當您輸入色彩值時，色彩選擇器會自動顯示，並顯示先前使用後面接著預設色彩調色盤的色彩清單。</span><span class="sxs-lookup"><span data-stu-id="d4534-630">When you enter a color value, the color picker is displayed automatically and presents a list of previously used colors followed by a default color palette.</span></span> <span data-ttu-id="d4534-631">您可以選取色彩，使用滑鼠或鍵盤。</span><span class="sxs-lookup"><span data-stu-id="d4534-631">You can select a color using the mouse or the keyboard.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image31.png)

<span data-ttu-id="d4534-632">清單可以展開到完整的色彩選擇器。</span><span class="sxs-lookup"><span data-stu-id="d4534-632">The list can be expanded into a complete color picker.</span></span> <span data-ttu-id="d4534-633">選擇器可讓您控制所移動的不透明度滑桿時，自動將任何色彩轉換成 RGBA alpha 色板：</span><span class="sxs-lookup"><span data-stu-id="d4534-633">The picker lets you control the alpha channel by automatically converting any color into RGBA when you move the opacity slider:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image32.png)

<a id="_Toc318097422"></a>
#### <a name="snippets"></a><span data-ttu-id="d4534-634">程式碼片段</span><span class="sxs-lookup"><span data-stu-id="d4534-634">Snippets</span></span>

<span data-ttu-id="d4534-635">CSS 編輯器中的程式碼片段讓您更輕鬆且快速地建立跨瀏覽器樣式。</span><span class="sxs-lookup"><span data-stu-id="d4534-635">Snippets in the CSS editor make it easier and faster to create cross-browser styles.</span></span> <span data-ttu-id="d4534-636">需要特定的瀏覽器設定的許多 CSS3 屬性現在已回復到程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d4534-636">Many CSS3 properties that require browser-specific settings have now been rolled into snippets.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image33.png)

<span data-ttu-id="d4534-637">CSS 程式碼片段支援進階的案例 （例如 CSS3 媒體查詢），輸入在符號 (@)，其中顯示 IntelliSense 清單。</span><span class="sxs-lookup"><span data-stu-id="d4534-637">CSS snippets support advanced scenarios (like CSS3 media queries) by typing the at-symbol (@), which shows the IntelliSense list.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image34.png)

<span data-ttu-id="d4534-638">當您選取@media值並按下 Tab 鍵，CSS 編輯器會插入下列程式碼片段：</span><span class="sxs-lookup"><span data-stu-id="d4534-638">When you select @media value and press Tab, the CSS editor inserts the following snippet:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image5.jpg)

<span data-ttu-id="d4534-639">如同程式碼的程式碼片段，您可以建立您自己的 CSS 程式碼片段。</span><span class="sxs-lookup"><span data-stu-id="d4534-639">As with snippets for code, you can create your own CSS snippets.</span></span>

<a id="_Toc318097423"></a>
#### <a name="custom-regions"></a><span data-ttu-id="d4534-640">自訂區域</span><span class="sxs-lookup"><span data-stu-id="d4534-640">Custom regions</span></span>

<span data-ttu-id="d4534-641">名為程式碼區域，已有程式碼編輯器中，現在均可供編輯 CSS。</span><span class="sxs-lookup"><span data-stu-id="d4534-641">Named code regions, which are already available in the code editor, are now available for CSS editing.</span></span> <span data-ttu-id="d4534-642">這可讓您輕鬆地群組相關的樣式區塊。</span><span class="sxs-lookup"><span data-stu-id="d4534-642">This lets you easily group related style blocks.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image35.png)

<span data-ttu-id="d4534-643">區域摺疊時顯示區域的名稱：</span><span class="sxs-lookup"><span data-stu-id="d4534-643">When a region is collapsed it displays the name of the region:</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image36.png)

<a id="_Toc318097424"></a>
### <a name="page-inspector"></a><span data-ttu-id="d4534-644">Page Inspector</span><span class="sxs-lookup"><span data-stu-id="d4534-644">Page Inspector</span></span>

<span data-ttu-id="d4534-645">Page Inspector 是一種工具，呈現網頁 （HTML、 Web Form、 ASP.NET MVC 或 Web 網頁） 在 Visual Studio IDE，並可讓您檢查原始程式碼與產生的輸出。</span><span class="sxs-lookup"><span data-stu-id="d4534-645">Page Inspector is a tool that renders a web page (HTML, Web Forms, ASP.NET MVC, or Web Pages) in the Visual Studio IDE and lets you examine both the source code and the resulting output.</span></span> <span data-ttu-id="d4534-646">適用於 ASP.NET 網頁，Page Inspector 可讓您判斷哪些伺服器端程式碼所產生的瀏覽器中呈現的 HTML 標記。</span><span class="sxs-lookup"><span data-stu-id="d4534-646">For ASP.NET pages, Page Inspector lets you determine which server-side code has produced the HTML markup that is rendered to the browser.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image37.png)

<span data-ttu-id="d4534-647">如需有關 Page Inspector 的詳細資訊，請參閱下列教學課程：</span><span class="sxs-lookup"><span data-stu-id="d4534-647">For more information about Page Inspector, please see the following tutorials:</span></span>

- <span data-ttu-id="d4534-648">使用 Page Inspector 中[ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span><span class="sxs-lookup"><span data-stu-id="d4534-648">Using Page Inspector in [ASP.NET MVC](../mvc/overview/views/using-page-inspector-in-aspnet-mvc.md)</span></span>
- <span data-ttu-id="d4534-649">使用 Page Inspector 中[ASP.NET Web Form](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span><span class="sxs-lookup"><span data-stu-id="d4534-649">Using Page Inspector in [ASP.NET Web Forms](../web-forms/overview/getting-started/using-page-inspector-in-a-visual-studio-11-beta-web-forms-project.md)</span></span>

<a id="_Toc318097425"></a>
### <a name="publishing"></a><span data-ttu-id="d4534-650">發佈</span><span class="sxs-lookup"><span data-stu-id="d4534-650">Publishing</span></span>

<a id="_Toc318097426"></a>
#### <a name="publish-profiles"></a><span data-ttu-id="d4534-651">發行設定檔</span><span class="sxs-lookup"><span data-stu-id="d4534-651">Publish profiles</span></span>

<span data-ttu-id="d4534-652">在 Visual Studio 2010、 發行 Web 應用程式專案的資訊不會儲存在版本控制，而且不是與他人共用。</span><span class="sxs-lookup"><span data-stu-id="d4534-652">In Visual Studio 2010, publishing information for Web application projects is not stored in version control and is not designed for sharing with others.</span></span> <span data-ttu-id="d4534-653">在 Visual Studio 2012 發行候選版本，將發行設定檔的格式已經變更。</span><span class="sxs-lookup"><span data-stu-id="d4534-653">In Visual Studio 2012 Release Candidate, the format of the publish profile has been changed.</span></span> <span data-ttu-id="d4534-654">已在進行團隊的成品，所以現在可以輕鬆地利用從 MSBuild 的組建。</span><span class="sxs-lookup"><span data-stu-id="d4534-654">It has been made a team artifact, and it is now easy to leverage from builds based on MSBuild.</span></span> <span data-ttu-id="d4534-655">組建組態資訊是在 [發行] 對話方塊中，讓您可以輕鬆切換發行前的組建組態。</span><span class="sxs-lookup"><span data-stu-id="d4534-655">Build configuration information is in the Publish dialog box so that you can easily switch build configurations before publishing.</span></span>

<span data-ttu-id="d4534-656">發行設定檔會儲存在 PublishProfiles 資料夾。</span><span class="sxs-lookup"><span data-stu-id="d4534-656">Publish profiles are stored in the PublishProfiles folder.</span></span> <span data-ttu-id="d4534-657">資料夾的位置取決於您使用何種程式語言：</span><span class="sxs-lookup"><span data-stu-id="d4534-657">The location of the folder depends on what programming language you are using:</span></span>

- <span data-ttu-id="d4534-658">C#: Properties\PublishProfiles</span><span class="sxs-lookup"><span data-stu-id="d4534-658">C#: Properties\PublishProfiles</span></span>
- <span data-ttu-id="d4534-659">我 Project\PublishProfiles Visual Basic:</span><span class="sxs-lookup"><span data-stu-id="d4534-659">Visual Basic: My Project\PublishProfiles</span></span>

<span data-ttu-id="d4534-660">每個設定檔是 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="d4534-660">Each profile is an MSBuild file.</span></span> <span data-ttu-id="d4534-661">在發佈期間，此檔案匯入專案的 MSBuild 檔案。</span><span class="sxs-lookup"><span data-stu-id="d4534-661">During publishing, this file is imported into the project's MSBuild file.</span></span> <span data-ttu-id="d4534-662">在 Visual Studio 2010 中，如果您想要變更的發行或封裝的程序，您必須在名為的檔案中放入您的自訂**ProjectName**。 wpp.targets。</span><span class="sxs-lookup"><span data-stu-id="d4534-662">In Visual Studio 2010, if you want to make changes to the publish or package process, you have to put your customizations in a file named **ProjectName**.wpp.targets.</span></span> <span data-ttu-id="d4534-663">仍然支援，但現在，您可以將您的自訂放在本身的發行設定檔中。</span><span class="sxs-lookup"><span data-stu-id="d4534-663">This is still supported, but you can now put your customizations in the publish profile itself.</span></span> <span data-ttu-id="d4534-664">這樣一來，將只針對該設定檔使用自訂項目。</span><span class="sxs-lookup"><span data-stu-id="d4534-664">That way, the customizations will be used only for that profile.</span></span>

<span data-ttu-id="d4534-665">您可以現在也利用會發行從 MSBuild 的設定檔。</span><span class="sxs-lookup"><span data-stu-id="d4534-665">You can now also leverage publish profiles from MSBuild.</span></span> <span data-ttu-id="d4534-666">當您建置專案時，若要這樣做，請使用下列命令：</span><span class="sxs-lookup"><span data-stu-id="d4534-666">To do so, use the following command when you build the project:</span></span>

[!code-console[Main](whats-new-in-aspnet-45-and-visual-studio-2012/samples/sample36.cmd)]

<span data-ttu-id="d4534-667">Project.csproj 值是專案的路徑和設定檔名稱是要發行設定檔的名稱。</span><span class="sxs-lookup"><span data-stu-id="d4534-667">The project.csproj value is the path of the project, and ProfileName is the name of the profile to publish.</span></span> <span data-ttu-id="d4534-668">或者，而不是傳遞的設定檔名稱*PublishProfile*屬性，您可以傳入的完整路徑的發行設定檔。</span><span class="sxs-lookup"><span data-stu-id="d4534-668">Alternatively, instead of passing the profile name for the *PublishProfile* property, you can pass in the full path to the publish profile.</span></span>

<a id="_Toc318097427"></a>
#### <a name="aspnet-precompilation-and-merge"></a><span data-ttu-id="d4534-669">ASP.NET 先行編譯及合併</span><span class="sxs-lookup"><span data-stu-id="d4534-669">ASP.NET precompilation and merge</span></span>

<span data-ttu-id="d4534-670">針對 Web 應用程式專案，Visual Studio 2012 發行候選版本會加入可讓您先行編譯，以及合併站台的內容，當您發行或封裝專案的 封裝/發行 Web 屬性頁面上的選項。</span><span class="sxs-lookup"><span data-stu-id="d4534-670">For Web application projects, Visual Studio 2012 Release Candidate adds an option on the Package/Publish Web properties page that lets you precompile and merge your site's content when you publish or package the project.</span></span> <span data-ttu-id="d4534-671">若要查看這些選項，以滑鼠右鍵按一下方案總管 中的專案，選擇 內容，然後選擇 封裝/發行 Web 屬性頁。</span><span class="sxs-lookup"><span data-stu-id="d4534-671">To see these options, right-click the project in Solution Explorer, choose Properties, and then choose the Package/Publish Web property page.</span></span> <span data-ttu-id="d4534-672">下圖顯示 Precompile 此應用程式，才能發行選項。</span><span class="sxs-lookup"><span data-stu-id="d4534-672">The following illustration shows the Precompile this application before publishing option.</span></span>

![](whats-new-in-aspnet-45-and-visual-studio-2012/_static/image6.jpg)

<span data-ttu-id="d4534-673">選取此選項時，Visual Studio 會先行編譯的應用程式，每當您發行或封裝的 web 應用程式。</span><span class="sxs-lookup"><span data-stu-id="d4534-673">When this option is selected, Visual Studio precompiles the application whenever you publish or package the web application.</span></span> <span data-ttu-id="d4534-674">如果您想要控制如何先行編譯網站時，或如何合併組件，請按一下 [進階] 按鈕來設定這些選項。</span><span class="sxs-lookup"><span data-stu-id="d4534-674">If you want to control how the site is precompiled or how assemblies are merged, click the Advanced button to configure those options.</span></span>

<a id="_Toc318097428"></a>
### <a name="iis-express"></a><span data-ttu-id="d4534-675">IIS Express</span><span class="sxs-lookup"><span data-stu-id="d4534-675">IIS Express</span></span>

<span data-ttu-id="d4534-676">在 Visual Studio 中的測試 web 專案的預設 web 伺服器現在為 IIS Express。</span><span class="sxs-lookup"><span data-stu-id="d4534-676">The default web server for testing web projects in Visual Studio is now IIS Express.</span></span> <span data-ttu-id="d4534-677">Visual Studio 程式開發伺服器仍在開發期間，本機 web 伺服器的選項，但 IIS Express 現在是建議的伺服器。</span><span class="sxs-lookup"><span data-stu-id="d4534-677">The Visual Studio Development Server is still an option for local web server during development, but IIS Express is now the recommended server.</span></span> <span data-ttu-id="d4534-678">在 Visual Studio 11 Beta 中使用 IIS Express 的經驗是非常類似於在 Visual Studio 2010 SP1 中使用它。</span><span class="sxs-lookup"><span data-stu-id="d4534-678">The experience of using IIS Express in Visual Studio 11 Beta is very similar to using it in Visual Studio 2010 SP1.</span></span>

<a id="_Toc318097429"></a>
## <a name="disclaimer"></a><span data-ttu-id="d4534-679">免責聲明</span><span class="sxs-lookup"><span data-stu-id="d4534-679">Disclaimer</span></span>

<span data-ttu-id="d4534-680">這是一份初稿，內容在本文所述的軟體於正式商業發行前都可能有所更動。</span><span class="sxs-lookup"><span data-stu-id="d4534-680">This is a preliminary document and may be changed substantially prior to final commercial release of the software described herein.</span></span>

<span data-ttu-id="d4534-681">本文件中的資訊表示直到文件發行日前 Microsoft Corporation 針對問題的看法。</span><span class="sxs-lookup"><span data-stu-id="d4534-681">The information contained in this document represents the current view of Microsoft Corporation on the issues discussed as of the date of publication.</span></span> <span data-ttu-id="d4534-682">Microsoft 必須因應不斷變化的市場狀況，因此本文件不代表 Microsoft 的保證，且 Microsoft 不保證這些資訊在文件發行後的正確性。</span><span class="sxs-lookup"><span data-stu-id="d4534-682">Because Microsoft must respond to changing market conditions, it should not be interpreted to be a commitment on the part of Microsoft, and Microsoft cannot guarantee the accuracy of any information presented after the date of publication.</span></span>

<span data-ttu-id="d4534-683">本技術白皮書僅供參考。</span><span class="sxs-lookup"><span data-stu-id="d4534-683">This White Paper is for informational purposes only.</span></span> <span data-ttu-id="d4534-684">MICROSOFT 對本文件中的資訊不提供任何明示、暗示或法定擔保。</span><span class="sxs-lookup"><span data-stu-id="d4534-684">MICROSOFT MAKES NO WARRANTIES, EXPRESS, IMPLIED OR STATUTORY, AS TO THE INFORMATION IN THIS DOCUMENT.</span></span>

<span data-ttu-id="d4534-685">承諾遵守所有適用的著作權法是使用者的責任。</span><span class="sxs-lookup"><span data-stu-id="d4534-685">Complying with all applicable copyright laws is the responsibility of the user.</span></span> <span data-ttu-id="d4534-686">著作權法沒有針對某種權利加以限制，但在未獲得 Microsoft Corporation 書面同意的情況下，本文件的任何部分不得複製、以檢索系統存放或擷取、以任何形式或方法傳送 (電子、機械、影像複製、錄音或其他任何方法)、或基於任何其他不良意圖。</span><span class="sxs-lookup"><span data-stu-id="d4534-686">Without limiting the rights under copyright, no part of this document may be reproduced, stored in or introduced into a retrieval system, or transmitted in any form or by any means (electronic, mechanical, photocopying, recording, or otherwise), or for any purpose, without the express written permission of Microsoft Corporation.</span></span>

<span data-ttu-id="d4534-687">本文件所提及的主要事務，Microsoft 得擁有專利、專利應用程式、商標、著作權或其他智慧財產權。</span><span class="sxs-lookup"><span data-stu-id="d4534-687">Microsoft may have patents, patent applications, trademarks, copyrights, or other intellectual property rights covering subject matter in this document.</span></span> <span data-ttu-id="d4534-688">除了 Microsoft 於授權合約書中書面提供的之外，本文件所述內容並未賦予您這些專利、商標、著作權、或其他智慧財產的任何授權或使用權利。</span><span class="sxs-lookup"><span data-stu-id="d4534-688">Except as expressly provided in any written license agreement from Microsoft, the furnishing of this document does not give you any license to these patents, trademarks, copyrights, or other intellectual property.</span></span>

<span data-ttu-id="d4534-689">除非特別註明，否則本文件中所述，用來舉例之公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所和事件皆為虛構，沒有意圖或不應該推斷為與任何真實存在的公司、組織、產品、網域名稱、電子郵件地址、標誌、人物、場所或事件有所關聯。</span><span class="sxs-lookup"><span data-stu-id="d4534-689">Unless otherwise noted, the example companies, organizations, products, domain names, e-mail addresses, logos, people, places and events depicted herein are fictitious, and no association with any real company, organization, product, domain name, email address, logo, person, place or event is intended or should be inferred.</span></span>

<span data-ttu-id="d4534-690">(C) 2012 Microsoft Corporation.</span><span class="sxs-lookup"><span data-stu-id="d4534-690">© 2012 Microsoft Corporation.</span></span> <span data-ttu-id="d4534-691">著作權所有，並保留一切權利。</span><span class="sxs-lookup"><span data-stu-id="d4534-691">All rights reserved.</span></span>

<span data-ttu-id="d4534-692">Microsoft 和 Windows 是 Microsoft Corporation 在美國及/或其他國家/地區的註冊商標或商標。</span><span class="sxs-lookup"><span data-stu-id="d4534-692">Microsoft and Windows are either registered trademarks or trademarks of Microsoft Corporation in the United States and/or other countries.</span></span>

<span data-ttu-id="d4534-693">本文件中所提實際公司和產品，可能為各所有人所有之商標。</span><span class="sxs-lookup"><span data-stu-id="d4534-693">The names of actual companies and products mentioned herein may be the trademarks of their respective owners.</span></span>
