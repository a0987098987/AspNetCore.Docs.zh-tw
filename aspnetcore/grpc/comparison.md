---
<span data-ttu-id="4e4e1-101">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-101">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-102">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-103">'Identity'</span></span>
- <span data-ttu-id="4e4e1-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-105">'Razor'</span></span>
- <span data-ttu-id="4e4e1-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-106">'SignalR' uid:</span></span> 

---
# <a name="compare-grpc-services-with-http-apis"></a><span data-ttu-id="4e4e1-107">比較 gRPC 服務與 HTTP API</span><span class="sxs-lookup"><span data-stu-id="4e4e1-107">Compare gRPC services with HTTP APIs</span></span>

<span data-ttu-id="4e4e1-108">依[James 牛頓-王](https://twitter.com/jamesnk)</span><span class="sxs-lookup"><span data-stu-id="4e4e1-108">By [James Newton-King](https://twitter.com/jamesnk)</span></span>

<span data-ttu-id="4e4e1-109">本文說明[gRPC 服務](https://grpc.io/docs/guides/)如何與 JSON （包括 ASP.NET Core [web api](xref:web-api/index)）中的 HTTP api 進行比較。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-109">This article explains how [gRPC services](https://grpc.io/docs/guides/) compare to HTTP APIs with JSON (including ASP.NET Core [web APIs](xref:web-api/index)).</span></span> <span data-ttu-id="4e4e1-110">用來為您的應用程式提供 API 的技術是一項重要的選擇，gRPC 提供與 HTTP Api 相較之下的獨特優點。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-110">The technology used to provide an API for your app is an important choice, and gRPC offers unique benefits compared to HTTP APIs.</span></span> <span data-ttu-id="4e4e1-111">本文討論 gRPC 的優點和缺點，並建議在其他技術上使用 gRPC 的案例。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-111">This article discusses the strengths and weaknesses of gRPC and recommends scenarios for using gRPC over other technologies.</span></span>

## <a name="high-level-comparison"></a><span data-ttu-id="4e4e1-112">高階比較</span><span class="sxs-lookup"><span data-stu-id="4e4e1-112">High-level comparison</span></span>

<span data-ttu-id="4e4e1-113">下表提供 gRPC 和 HTTP Api 與 JSON 之間功能的高階比較。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-113">The following table offers a high-level comparison of features between gRPC and HTTP APIs with JSON.</span></span>

| <span data-ttu-id="4e4e1-114">功能</span><span class="sxs-lookup"><span data-stu-id="4e4e1-114">Feature</span></span>          | <span data-ttu-id="4e4e1-115">gRPC</span><span class="sxs-lookup"><span data-stu-id="4e4e1-115">gRPC</span></span>                                               | <span data-ttu-id="4e4e1-116">HTTP Api 與 JSON</span><span class="sxs-lookup"><span data-stu-id="4e4e1-116">HTTP APIs with JSON</span></span>           |
| ---
<span data-ttu-id="4e4e1-117">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-117">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-118">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-118">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-119">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-119">'Identity'</span></span>
- <span data-ttu-id="4e4e1-120">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-120">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-121">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-121">'Razor'</span></span>
- <span data-ttu-id="4e4e1-122">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-122">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-123">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-123">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-124">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-124">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-125">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-125">'Identity'</span></span>
- <span data-ttu-id="4e4e1-126">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-126">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-127">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-127">'Razor'</span></span>
- <span data-ttu-id="4e4e1-128">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-128">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-129">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-129">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-130">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-130">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-131">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-131">'Identity'</span></span>
- <span data-ttu-id="4e4e1-132">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-132">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-133">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-133">'Razor'</span></span>
- <span data-ttu-id="4e4e1-134">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-134">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-135">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-135">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-136">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-136">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-137">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-137">'Identity'</span></span>
- <span data-ttu-id="4e4e1-138">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-138">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-139">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-139">'Razor'</span></span>
- <span data-ttu-id="4e4e1-140">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-140">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-141">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-141">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-142">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-142">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-143">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-143">'Identity'</span></span>
- <span data-ttu-id="4e4e1-144">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-144">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-145">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-145">'Razor'</span></span>
- <span data-ttu-id="4e4e1-146">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-146">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-147">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-147">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-148">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-148">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-149">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-149">'Identity'</span></span>
- <span data-ttu-id="4e4e1-150">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-150">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-151">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-151">'Razor'</span></span>
- <span data-ttu-id="4e4e1-152">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-152">'SignalR' uid:</span></span> 

<span data-ttu-id="4e4e1-153">-------- |---標題： author： description： monikerRange： ms-chap： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-153">-------- | --- title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-154">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-154">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-155">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-155">'Identity'</span></span>
- <span data-ttu-id="4e4e1-156">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-156">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-157">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-157">'Razor'</span></span>
- <span data-ttu-id="4e4e1-158">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-158">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-159">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-159">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-160">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-160">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-161">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-161">'Identity'</span></span>
- <span data-ttu-id="4e4e1-162">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-162">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-163">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-163">'Razor'</span></span>
- <span data-ttu-id="4e4e1-164">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-164">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-165">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-165">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-166">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-166">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-167">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-167">'Identity'</span></span>
- <span data-ttu-id="4e4e1-168">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-168">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-169">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-169">'Razor'</span></span>
- <span data-ttu-id="4e4e1-170">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-170">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-171">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-171">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-172">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-172">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-173">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-173">'Identity'</span></span>
- <span data-ttu-id="4e4e1-174">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-174">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-175">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-175">'Razor'</span></span>
- <span data-ttu-id="4e4e1-176">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-176">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-177">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-177">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-178">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-178">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-179">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-179">'Identity'</span></span>
- <span data-ttu-id="4e4e1-180">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-180">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-181">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-181">'Razor'</span></span>
- <span data-ttu-id="4e4e1-182">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-182">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-183">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-183">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-184">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-184">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-185">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-185">'Identity'</span></span>
- <span data-ttu-id="4e4e1-186">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-186">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-187">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-187">'Razor'</span></span>
- <span data-ttu-id="4e4e1-188">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-188">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-189">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-189">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-190">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-190">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-191">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-191">'Identity'</span></span>
- <span data-ttu-id="4e4e1-192">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-192">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-193">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-193">'Razor'</span></span>
- <span data-ttu-id="4e4e1-194">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-194">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-195">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-195">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-196">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-196">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-197">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-197">'Identity'</span></span>
- <span data-ttu-id="4e4e1-198">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-198">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-199">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-199">'Razor'</span></span>
- <span data-ttu-id="4e4e1-200">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-200">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-201">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-201">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-202">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-202">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-203">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-203">'Identity'</span></span>
- <span data-ttu-id="4e4e1-204">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-204">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-205">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-205">'Razor'</span></span>
- <span data-ttu-id="4e4e1-206">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-206">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-207">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-207">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-208">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-208">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-209">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-209">'Identity'</span></span>
- <span data-ttu-id="4e4e1-210">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-210">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-211">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-211">'Razor'</span></span>
- <span data-ttu-id="4e4e1-212">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-212">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-213">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-213">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-214">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-214">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-215">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-215">'Identity'</span></span>
- <span data-ttu-id="4e4e1-216">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-216">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-217">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-217">'Razor'</span></span>
- <span data-ttu-id="4e4e1-218">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-218">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-219">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-219">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-220">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-220">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-221">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-221">'Identity'</span></span>
- <span data-ttu-id="4e4e1-222">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-222">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-223">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-223">'Razor'</span></span>
- <span data-ttu-id="4e4e1-224">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-224">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-225">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-225">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-226">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-226">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-227">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-227">'Identity'</span></span>
- <span data-ttu-id="4e4e1-228">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-228">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-229">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-229">'Razor'</span></span>
- <span data-ttu-id="4e4e1-230">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-230">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-231">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-231">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-232">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-232">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-233">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-233">'Identity'</span></span>
- <span data-ttu-id="4e4e1-234">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-234">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-235">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-235">'Razor'</span></span>
- <span data-ttu-id="4e4e1-236">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-236">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-237">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-237">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-238">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-238">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-239">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-239">'Identity'</span></span>
- <span data-ttu-id="4e4e1-240">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-240">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-241">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-241">'Razor'</span></span>
- <span data-ttu-id="4e4e1-242">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-242">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-243">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-243">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-244">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-244">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-245">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-245">'Identity'</span></span>
- <span data-ttu-id="4e4e1-246">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-246">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-247">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-247">'Razor'</span></span>
- <span data-ttu-id="4e4e1-248">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-248">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-249">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-249">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-250">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-250">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-251">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-251">'Identity'</span></span>
- <span data-ttu-id="4e4e1-252">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-252">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-253">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-253">'Razor'</span></span>
- <span data-ttu-id="4e4e1-254">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-254">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-255">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-255">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-256">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-256">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-257">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-257">'Identity'</span></span>
- <span data-ttu-id="4e4e1-258">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-258">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-259">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-259">'Razor'</span></span>
- <span data-ttu-id="4e4e1-260">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-260">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-261">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-261">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-262">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-262">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-263">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-263">'Identity'</span></span>
- <span data-ttu-id="4e4e1-264">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-264">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-265">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-265">'Razor'</span></span>
- <span data-ttu-id="4e4e1-266">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-266">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-267">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-267">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-268">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-268">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-269">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-269">'Identity'</span></span>
- <span data-ttu-id="4e4e1-270">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-270">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-271">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-271">'Razor'</span></span>
- <span data-ttu-id="4e4e1-272">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-272">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-273">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-273">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-274">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-274">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-275">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-275">'Identity'</span></span>
- <span data-ttu-id="4e4e1-276">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-276">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-277">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-277">'Razor'</span></span>
- <span data-ttu-id="4e4e1-278">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-278">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-279">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-279">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-280">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-280">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-281">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-281">'Identity'</span></span>
- <span data-ttu-id="4e4e1-282">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-282">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-283">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-283">'Razor'</span></span>
- <span data-ttu-id="4e4e1-284">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-284">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-285">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-285">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-286">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-286">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-287">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-287">'Identity'</span></span>
- <span data-ttu-id="4e4e1-288">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-288">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-289">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-289">'Razor'</span></span>
- <span data-ttu-id="4e4e1-290">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-290">'SignalR' uid:</span></span> 

<span data-ttu-id="4e4e1-291">------------------------- |---標題： author： description： monikerRange： ms-chap： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-291">------------------------- | --- title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-292">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-292">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-293">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-293">'Identity'</span></span>
- <span data-ttu-id="4e4e1-294">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-294">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-295">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-295">'Razor'</span></span>
- <span data-ttu-id="4e4e1-296">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-296">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-297">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-297">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-298">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-298">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-299">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-299">'Identity'</span></span>
- <span data-ttu-id="4e4e1-300">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-300">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-301">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-301">'Razor'</span></span>
- <span data-ttu-id="4e4e1-302">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-302">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-303">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-303">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-304">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-304">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-305">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-305">'Identity'</span></span>
- <span data-ttu-id="4e4e1-306">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-306">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-307">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-307">'Razor'</span></span>
- <span data-ttu-id="4e4e1-308">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-308">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-309">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-309">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-310">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-310">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-311">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-311">'Identity'</span></span>
- <span data-ttu-id="4e4e1-312">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-312">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-313">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-313">'Razor'</span></span>
- <span data-ttu-id="4e4e1-314">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-314">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-315">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-315">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-316">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-316">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-317">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-317">'Identity'</span></span>
- <span data-ttu-id="4e4e1-318">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-318">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-319">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-319">'Razor'</span></span>
- <span data-ttu-id="4e4e1-320">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-320">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-321">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-321">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-322">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-322">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-323">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-323">'Identity'</span></span>
- <span data-ttu-id="4e4e1-324">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-324">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-325">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-325">'Razor'</span></span>
- <span data-ttu-id="4e4e1-326">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-326">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-327">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-327">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-328">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-328">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-329">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-329">'Identity'</span></span>
- <span data-ttu-id="4e4e1-330">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-330">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-331">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-331">'Razor'</span></span>
- <span data-ttu-id="4e4e1-332">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-332">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-333">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-333">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-334">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-334">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-335">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-335">'Identity'</span></span>
- <span data-ttu-id="4e4e1-336">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-336">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-337">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-337">'Razor'</span></span>
- <span data-ttu-id="4e4e1-338">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-338">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-339">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-339">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-340">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-340">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-341">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-341">'Identity'</span></span>
- <span data-ttu-id="4e4e1-342">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-342">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-343">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-343">'Razor'</span></span>
- <span data-ttu-id="4e4e1-344">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-344">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-345">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-345">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-346">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-346">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-347">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-347">'Identity'</span></span>
- <span data-ttu-id="4e4e1-348">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-348">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-349">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-349">'Razor'</span></span>
- <span data-ttu-id="4e4e1-350">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-350">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-351">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-351">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-352">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-352">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-353">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-353">'Identity'</span></span>
- <span data-ttu-id="4e4e1-354">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-354">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-355">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-355">'Razor'</span></span>
- <span data-ttu-id="4e4e1-356">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-356">'SignalR' uid:</span></span> 

-
<span data-ttu-id="4e4e1-357">標題： author： description： monikerRange： ms. author： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-357">title: author: description: monikerRange: ms.author: ms.date: no-loc:</span></span>
- <span data-ttu-id="4e4e1-358">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-358">'Blazor'</span></span>
- <span data-ttu-id="4e4e1-359">'Identity'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-359">'Identity'</span></span>
- <span data-ttu-id="4e4e1-360">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-360">'Let's Encrypt'</span></span>
- <span data-ttu-id="4e4e1-361">'Razor'</span><span class="sxs-lookup"><span data-stu-id="4e4e1-361">'Razor'</span></span>
- <span data-ttu-id="4e4e1-362">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-362">'SignalR' uid:</span></span> 

<span data-ttu-id="4e4e1-363">--------------- | |合約 |必要（*proto*） |選擇性（OpenAPI） | |通訊協定 |HTTP/2 |HTTP | |裝載 |[Protobuf （小型，二進位）](#performance) |JSON （大型、人類可讀取） | |Prescriptiveness |[嚴格規格](#strict-specification)|鬆動.</span><span class="sxs-lookup"><span data-stu-id="4e4e1-363">--------------- | | Contract         | Required (*.proto*)                                | Optional (OpenAPI)            | | Protocol         | HTTP/2                                             | HTTP                          | | Payload          | [Protobuf (small, binary)](#performance)           | JSON (large, human readable)  | | Prescriptiveness | [Strict specification](#strict-specification)      | Loose.</span></span> <span data-ttu-id="4e4e1-364">任何 HTTP 都是有效的。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-364">Any HTTP is valid.</span></span>     <span data-ttu-id="4e4e1-365">| |串流 |[用戶端，伺服器，雙向](#streaming)|用戶端、伺服器 | |瀏覽器支援 |[否（需要 grpc-web）](#limited-browser-support) |是 | |安全性 |傳輸（TLS） |傳輸（TLS） | |用戶端程式代碼產生 |[是](#code-generation)|OpenAPI + 協力廠商工具 |</span><span class="sxs-lookup"><span data-stu-id="4e4e1-365">| | Streaming        | [Client, server, bi-directional](#streaming)       | Client, server                | | Browser support  | [No (requires grpc-web)](#limited-browser-support) | Yes                           | | Security         | Transport (TLS)                                    | Transport (TLS)               | | Client code-generation | [Yes](#code-generation)                      | OpenAPI + third-party tooling |</span></span>

## <a name="grpc-strengths"></a><span data-ttu-id="4e4e1-366">gRPC 的優點</span><span class="sxs-lookup"><span data-stu-id="4e4e1-366">gRPC strengths</span></span>

### <a name="performance"></a><span data-ttu-id="4e4e1-367">效能</span><span class="sxs-lookup"><span data-stu-id="4e4e1-367">Performance</span></span>

<span data-ttu-id="4e4e1-368">gRPC 訊息會使用[Protobuf](https://developers.google.com/protocol-buffers/docs/overview)序列化，這是一種有效率的二進位訊息格式。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-368">gRPC messages are serialized using [Protobuf](https://developers.google.com/protocol-buffers/docs/overview), an efficient binary message format.</span></span> <span data-ttu-id="4e4e1-369">Protobuf 會非常快速地在伺服器和用戶端上序列化。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-369">Protobuf serializes very quickly on the server and client.</span></span> <span data-ttu-id="4e4e1-370">在小型訊息承載中 Protobuf 序列化結果，這在有限的頻寬案例（例如行動應用程式）中很重要。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-370">Protobuf serialization results in small message payloads, important in limited bandwidth scenarios like mobile apps.</span></span>

<span data-ttu-id="4e4e1-371">gRPC 是針對 HTTP/2 所設計，這是一種可透過 HTTP 1.x 提供顯著效能優勢的 HTTP 主要修訂版：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-371">gRPC is designed for HTTP/2, a major revision of HTTP that provides significant performance benefits over HTTP 1.x:</span></span>

* <span data-ttu-id="4e4e1-372">二進位框架和壓縮。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-372">Binary framing and compression.</span></span> <span data-ttu-id="4e4e1-373">HTTP/2 通訊協定在傳送和接收時都是精簡且有效率的。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-373">HTTP/2 protocol is compact and efficient both in sending and receiving.</span></span>
* <span data-ttu-id="4e4e1-374">透過單一 TCP 連線進行多個 HTTP/2 呼叫的多工處理。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-374">Multiplexing of multiple HTTP/2 calls over a single TCP connection.</span></span> <span data-ttu-id="4e4e1-375">多工化可避免[行標頭封鎖](https://en.wikipedia.org/wiki/Head-of-line_blocking)。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-375">Multiplexing eliminates [head-of-line blocking](https://en.wikipedia.org/wiki/Head-of-line_blocking).</span></span>

<span data-ttu-id="4e4e1-376">HTTP/2 不是 gRPC 專屬的。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-376">HTTP/2 is not exclusive to gRPC.</span></span> <span data-ttu-id="4e4e1-377">許多要求類型，包括具有 JSON 的 HTTP Api，都可以使用 HTTP/2，並受益于其效能改進。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-377">Many request types, including HTTP APIs with JSON, can use HTTP/2 and benefit from its performance improvements.</span></span>

### <a name="code-generation"></a><span data-ttu-id="4e4e1-378">程式碼產生</span><span class="sxs-lookup"><span data-stu-id="4e4e1-378">Code generation</span></span>

<span data-ttu-id="4e4e1-379">所有的 gRPC 架構都提供第一級的程式碼產生支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-379">All gRPC frameworks provide first-class support for code generation.</span></span> <span data-ttu-id="4e4e1-380">GRPC 開發的核心檔案是定義 gRPC 服務和訊息合約的[proto](https://developers.google.com/protocol-buffers/docs/proto3)檔案。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-380">A core file to gRPC development is the [.proto file](https://developers.google.com/protocol-buffers/docs/proto3), which defines the contract of gRPC services and messages.</span></span> <span data-ttu-id="4e4e1-381">在此檔案中，gRPC 架構會產生服務基類、訊息和完整用戶端的程式碼。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-381">From this file gRPC frameworks will code generate a service base class, messages, and a complete client.</span></span>

<span data-ttu-id="4e4e1-382">藉由共用伺服器和用戶端之間的*proto*檔案，可以從端對端產生訊息和用戶端程式代碼。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-382">By sharing the *.proto* file between the server and client, messages and client code can be generated from end to end.</span></span> <span data-ttu-id="4e4e1-383">用戶端的程式碼產生不會在用戶端和伺服器上排除重複的訊息，並為您建立強型別用戶端。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-383">Code generation of the client eliminates duplication of messages on the client and server, and creates a strongly-typed client for you.</span></span> <span data-ttu-id="4e4e1-384">不需要撰寫用戶端，就能在具有許多服務的應用程式中節省大量的開發時間。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-384">Not having to write a client saves significant development time in applications with many services.</span></span>

### <a name="strict-specification"></a><span data-ttu-id="4e4e1-385">嚴格規格</span><span class="sxs-lookup"><span data-stu-id="4e4e1-385">Strict specification</span></span>

<span data-ttu-id="4e4e1-386">具有 JSON 的 HTTP API 的正式規格不存在。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-386">A formal specification for HTTP API with JSON doesn't exist.</span></span> <span data-ttu-id="4e4e1-387">開發人員會爭論 Url、HTTP 動詞命令和回應碼的最佳格式。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-387">Developers debate the best format of URLs, HTTP verbs, and response codes.</span></span>

<span data-ttu-id="4e4e1-388">[GRPC 規格](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md)規定了 gRPC 服務必須遵循的格式。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-388">The [gRPC specification](https://github.com/grpc/grpc/blob/master/doc/PROTOCOL-HTTP2.md) is prescriptive about the format a gRPC service must follow.</span></span> <span data-ttu-id="4e4e1-389">gRPC 可排除爭論並節省開發人員的時間，因為 gRPC 在平臺和實現之間是一致的。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-389">gRPC eliminates debate and saves developer time because gRPC is consistent across platforms and implementations.</span></span>

### <a name="streaming"></a><span data-ttu-id="4e4e1-390">串流</span><span class="sxs-lookup"><span data-stu-id="4e4e1-390">Streaming</span></span>

<span data-ttu-id="4e4e1-391">HTTP/2 提供長時間即時通訊資料流程的基礎。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-391">HTTP/2 provides a foundation for long-lived, real-time communication streams.</span></span> <span data-ttu-id="4e4e1-392">gRPC 提供透過 HTTP/2 進行串流的第一級支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-392">gRPC provides first-class support for streaming through HTTP/2.</span></span>

<span data-ttu-id="4e4e1-393">GRPC 服務支援所有串流組合：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-393">A gRPC service supports all streaming combinations:</span></span>

* <span data-ttu-id="4e4e1-394">一元（無串流）</span><span class="sxs-lookup"><span data-stu-id="4e4e1-394">Unary (no streaming)</span></span>
* <span data-ttu-id="4e4e1-395">伺服器對用戶端串流</span><span class="sxs-lookup"><span data-stu-id="4e4e1-395">Server to client streaming</span></span>
* <span data-ttu-id="4e4e1-396">用戶端對伺服器的串流處理</span><span class="sxs-lookup"><span data-stu-id="4e4e1-396">Client to server streaming</span></span>
* <span data-ttu-id="4e4e1-397">雙向串流</span><span class="sxs-lookup"><span data-stu-id="4e4e1-397">Bi-directional streaming</span></span>

### <a name="deadlinetimeouts-and-cancellation"></a><span data-ttu-id="4e4e1-398">期限/超時和取消</span><span class="sxs-lookup"><span data-stu-id="4e4e1-398">Deadline/timeouts and cancellation</span></span>

<span data-ttu-id="4e4e1-399">gRPC 可讓用戶端指定他們願意等待 RPC 完成的時間長度。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-399">gRPC allows clients to specify how long they are willing to wait for an RPC to complete.</span></span> <span data-ttu-id="4e4e1-400">[期限](https://grpc.io/blog/deadlines)會傳送至伺服器，而伺服器可以決定在超過期限時要採取的動作。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-400">The [deadline](https://grpc.io/blog/deadlines) is sent to the server, and the server can decide what action to take if it exceeds the deadline.</span></span> <span data-ttu-id="4e4e1-401">例如，伺服器可能會在超時時取消進行中的 gRPC/HTTP/資料庫要求。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-401">For example, the server might cancel in-progress gRPC/HTTP/database requests on timeout.</span></span>

<span data-ttu-id="4e4e1-402">透過子 gRPC 呼叫來傳播期限和取消，有助於強制執行資源使用限制。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-402">Propagating the deadline and cancellation through child gRPC calls helps enforce resource usage limits.</span></span>

## <a name="grpc-recommended-scenarios"></a><span data-ttu-id="4e4e1-403">gRPC 建議的案例</span><span class="sxs-lookup"><span data-stu-id="4e4e1-403">gRPC recommended scenarios</span></span>

<span data-ttu-id="4e4e1-404">gRPC 適用于下列案例：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-404">gRPC is well suited to the following scenarios:</span></span>

* <span data-ttu-id="4e4e1-405">**微服務**： gRPC 是針對低延遲和高輸送量通訊所設計。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-405">**Microservices**: gRPC is designed for low latency and high throughput communication.</span></span> <span data-ttu-id="4e4e1-406">gRPC 非常適合用於效率非常重要的輕量微服務。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-406">gRPC is great for lightweight microservices where efficiency is critical.</span></span>
* <span data-ttu-id="4e4e1-407">**點對點即時通訊**： gRPC 有絕佳的雙向串流支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-407">**Point-to-point real-time communication**: gRPC has excellent support for bi-directional streaming.</span></span> <span data-ttu-id="4e4e1-408">gRPC 服務可以即時推送訊息，而不需要輪詢。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-408">gRPC services can push messages in real-time without polling.</span></span>
* <span data-ttu-id="4e4e1-409">**多語言環境**： gRPC 工具支援所有熱門的開發語言，讓 gRPC 成為多語言環境的理想選擇。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-409">**Polyglot environments**: gRPC tooling supports all popular development languages, making gRPC a good choice for multi-language environments.</span></span>
* <span data-ttu-id="4e4e1-410">**網路限制環境**： gRPC 訊息會使用 Protobuf （輕量訊息格式）進行序列化。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-410">**Network constrained environments**: gRPC messages are serialized with Protobuf, a lightweight message format.</span></span> <span data-ttu-id="4e4e1-411">GRPC 訊息一律會小於對等的 JSON 訊息。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-411">A gRPC message is always smaller than an equivalent JSON message.</span></span>

## <a name="grpc-weaknesses"></a><span data-ttu-id="4e4e1-412">gRPC 弱點</span><span class="sxs-lookup"><span data-stu-id="4e4e1-412">gRPC weaknesses</span></span>

### <a name="limited-browser-support"></a><span data-ttu-id="4e4e1-413">有限的瀏覽器支援</span><span class="sxs-lookup"><span data-stu-id="4e4e1-413">Limited browser support</span></span>

<span data-ttu-id="4e4e1-414">目前無法從瀏覽器直接呼叫 gRPC 服務。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-414">It's impossible to directly call a gRPC service from a browser today.</span></span> <span data-ttu-id="4e4e1-415">gRPC 大量使用 HTTP/2 功能，而且沒有瀏覽器提供 web 要求所需的控制層級來支援 gRPC 用戶端。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-415">gRPC heavily uses HTTP/2 features and no browser provides the level of control required over web requests to support a gRPC client.</span></span> <span data-ttu-id="4e4e1-416">例如，瀏覽器不允許呼叫端要求使用 HTTP/2，或提供基礎 HTTP/2 框架的存取權。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-416">For example, browsers do not allow a caller to require that HTTP/2 be used, or provide access to underlying HTTP/2 frames.</span></span>

<span data-ttu-id="4e4e1-417">[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html)是 gRPC 團隊提供的額外技術，可在瀏覽器中提供有限的 gRPC 支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-417">[gRPC-Web](https://grpc.io/docs/tutorials/basic/web.html) is an additional technology from the gRPC team that provides limited gRPC support in the browser.</span></span> <span data-ttu-id="4e4e1-418">gRPC 是由兩個部分組成：支援所有新式瀏覽器的 JavaScript 用戶端，以及伺服器上的 gRPC Web proxy。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-418">gRPC-Web consists of two parts: a JavaScript client that supports all modern browsers, and a gRPC-Web proxy on the server.</span></span> <span data-ttu-id="4e4e1-419">GRPC-Web 用戶端會呼叫 proxy，而 proxy 會在 gRPC 要求上轉送至 gRPC 伺服器。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-419">The gRPC-Web client calls the proxy and the proxy will forward on the gRPC requests to the gRPC server.</span></span>

<span data-ttu-id="4e4e1-420">GRPC-Web 不支援所有 gRPC 的功能。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-420">Not all of gRPC's features are supported by gRPC-Web.</span></span> <span data-ttu-id="4e4e1-421">用戶端和雙向串流不受支援，而且對伺服器串流的支援有限。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-421">Client and bi-directional streaming isn't supported, and there is limited support for server streaming.</span></span>

> [!TIP]
> <span data-ttu-id="4e4e1-422">.NET Core 具有 gRPC Web 的實驗性支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-422">.NET Core has experimental support for gRPC-Web.</span></span> <span data-ttu-id="4e4e1-423">如需詳細資訊，請造訪 <xref:grpc/browser> 。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-423">Visit <xref:grpc/browser> for more information.</span></span>

### <a name="not-human-readable"></a><span data-ttu-id="4e4e1-424">不是人類看得懂</span><span class="sxs-lookup"><span data-stu-id="4e4e1-424">Not human readable</span></span>

<span data-ttu-id="4e4e1-425">HTTP API 要求會以文字傳送，並可供人類讀取和建立。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-425">HTTP API requests are sent as text and can be read and created by humans.</span></span>

<span data-ttu-id="4e4e1-426">根據預設，gRPC 訊息會以 Protobuf 編碼。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-426">gRPC messages are encoded with Protobuf by default.</span></span> <span data-ttu-id="4e4e1-427">雖然 Protobuf 的傳送和接收效率較高，但其二進位格式卻不容易閱讀。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-427">While Protobuf is efficient to send and receive, its binary format isn't human readable.</span></span> <span data-ttu-id="4e4e1-428">Protobuf 需要指定于*proto*檔案中的訊息介面描述，才能正確還原序列化。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-428">Protobuf requires the message's interface description specified in the *.proto* file to properly deserialize.</span></span> <span data-ttu-id="4e4e1-429">需要額外的工具，才能在網路上分析 Protobuf 承載，並以手動方式撰寫要求。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-429">Additional tooling is required to analyze Protobuf payloads on the wire and to compose requests by hand.</span></span>

<span data-ttu-id="4e4e1-430">[伺服器反映](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md)和[gRPC 命令列工具](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md)等功能都存在，以協助進行二進位 Protobuf 訊息。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-430">Features such as [server reflection](https://github.com/grpc/grpc/blob/master/doc/server-reflection.md) and the [gRPC command line tool](https://github.com/grpc/grpc/blob/master/doc/command_line_tool.md) exist to assist with binary Protobuf messages.</span></span> <span data-ttu-id="4e4e1-431">此外，Protobuf 訊息支援[JSON 的轉換](https://developers.google.com/protocol-buffers/docs/proto3#json)。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-431">Also, Protobuf messages support [conversion to and from JSON](https://developers.google.com/protocol-buffers/docs/proto3#json).</span></span> <span data-ttu-id="4e4e1-432">內建的 JSON 轉換提供了一種有效率的方式，可在進行偵錯工具時，將 Protobuf 訊息轉換成人類看得懂的格式。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-432">The built-in JSON conversion provides an efficient way to convert Protobuf messages to and from human readable form when debugging.</span></span>

## <a name="alternative-framework-scenarios"></a><span data-ttu-id="4e4e1-433">替代架構案例</span><span class="sxs-lookup"><span data-stu-id="4e4e1-433">Alternative framework scenarios</span></span>

<span data-ttu-id="4e4e1-434">在下列案例中，建議您透過 gRPC 使用其他架構：</span><span class="sxs-lookup"><span data-stu-id="4e4e1-434">Other frameworks are recommended over gRPC in the following scenarios:</span></span>

* <span data-ttu-id="4e4e1-435">**瀏覽器可存取的 api**：瀏覽器中未完全支援 gRPC。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-435">**Browser accessible APIs**: gRPC isn't fully supported in the browser.</span></span> <span data-ttu-id="4e4e1-436">gRPC-Web 可以提供瀏覽器支援，但它有一些限制，而且引進了伺服器 proxy。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-436">gRPC-Web can offer browser support, but it has limitations and introduces a server proxy.</span></span>
* <span data-ttu-id="4e4e1-437">**廣播即時通訊**： gRPC 支援透過串流進行即時通訊，但將訊息廣播到已註冊連線的概念並不存在。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-437">**Broadcast real-time communication**: gRPC supports real-time communication via streaming, but the concept of broadcasting a message out to registered connections doesn't exist.</span></span> <span data-ttu-id="4e4e1-438">例如，在聊天室案例中，新的聊天訊息應傳送至聊天室中的所有用戶端時，每個 gRPC 呼叫都需要個別將新的聊天訊息串流至用戶端。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-438">For example in a chat room scenario where new chat messages should be sent to all clients in the chat room, each gRPC call is required to individually stream new chat messages to the client.</span></span> <span data-ttu-id="4e4e1-439">[SignalR](xref:signalr/introduction)在此案例中是有用的架構。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-439">[SignalR](xref:signalr/introduction) is a useful framework for this scenario.</span></span> SignalR<span data-ttu-id="4e4e1-440">具有持續性連接的概念，以及廣播訊息的內建支援。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-440"> has the concept of persistent connections and built-in support for broadcasting messages.</span></span>
* <span data-ttu-id="4e4e1-441">**處理序間通訊**：處理常式必須裝載 HTTP/2 伺服器，以接受傳入的 gRPC 呼叫。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-441">**Inter-process communication**: A process must host an HTTP/2 server to accept incoming gRPC calls.</span></span> <span data-ttu-id="4e4e1-442">對於 Windows 而言，處理序間通訊[管道](/dotnet/standard/io/pipe-operations)是快速、輕量的通訊方法。</span><span class="sxs-lookup"><span data-stu-id="4e4e1-442">For Windows, inter-process communication [pipes](/dotnet/standard/io/pipe-operations) is a fast, lightweight method of communication.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4e4e1-443">其他資源</span><span class="sxs-lookup"><span data-stu-id="4e4e1-443">Additional resources</span></span>

* <xref:tutorials/grpc/grpc-start>
* <xref:grpc/index>
* <xref:grpc/basics>
* <xref:grpc/migration>
