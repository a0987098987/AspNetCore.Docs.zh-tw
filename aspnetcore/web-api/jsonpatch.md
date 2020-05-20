---
<span data-ttu-id="01c1f-101">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-101">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-102">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-102">'Blazor'</span></span>
- <span data-ttu-id="01c1f-103">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-103">'Identity'</span></span>
- <span data-ttu-id="01c1f-104">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-104">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-105">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-105">'Razor'</span></span>
- <span data-ttu-id="01c1f-106">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-106">'SignalR' uid:</span></span> 

---

# <a name="jsonpatch-in-aspnet-core-web-api"></a><span data-ttu-id="01c1f-107">ASP.NET Core Web API 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="01c1f-107">JsonPatch in ASP.NET Core web API</span></span>

<span data-ttu-id="01c1f-108">由[Tom 作者: dykstra](https://github.com/tdykstra)和[Kirk Larkin](https://github.com/serpent5)</span><span class="sxs-lookup"><span data-stu-id="01c1f-108">By [Tom Dykstra](https://github.com/tdykstra) and [Kirk Larkin](https://github.com/serpent5)</span></span>

::: moniker range=">= aspnetcore-3.0"

<span data-ttu-id="01c1f-109">本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。</span><span class="sxs-lookup"><span data-stu-id="01c1f-109">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="package-installation"></a><span data-ttu-id="01c1f-110">套件安裝</span><span class="sxs-lookup"><span data-stu-id="01c1f-110">Package installation</span></span>

<span data-ttu-id="01c1f-111">若要在您的應用程式中啟用 JSON 修補程式支援，請完成下列步驟：</span><span class="sxs-lookup"><span data-stu-id="01c1f-111">To enable JSON Patch support in your app, complete the following steps:</span></span>

1. <span data-ttu-id="01c1f-112">安裝 [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet 套件。</span><span class="sxs-lookup"><span data-stu-id="01c1f-112">Install the [`Microsoft.AspNetCore.Mvc.NewtonsoftJson`](https://www.nuget.org/packages/Microsoft.AspNetCore.Mvc.NewtonsoftJson/) NuGet package.</span></span>
1. <span data-ttu-id="01c1f-113">更新專案的 `Startup.ConfigureServices` 方法以呼叫 <xref:Microsoft.Extensions.DependencyInjection.NewtonsoftJsonMvcBuilderExtensions.AddNewtonsoftJson*> 。</span><span class="sxs-lookup"><span data-stu-id="01c1f-113">Update the project's `Startup.ConfigureServices` method to call <xref:Microsoft.Extensions.DependencyInjection.NewtonsoftJsonMvcBuilderExtensions.AddNewtonsoftJson*>.</span></span> <span data-ttu-id="01c1f-114">例如：</span><span class="sxs-lookup"><span data-stu-id="01c1f-114">For example:</span></span>

    ```csharp
    services
        .AddControllersWithViews()
        .AddNewtonsoftJson();
    ```

<span data-ttu-id="01c1f-115">`AddNewtonsoftJson`與 MVC 服務註冊方法相容：</span><span class="sxs-lookup"><span data-stu-id="01c1f-115">`AddNewtonsoftJson` is compatible with the MVC service registration methods:</span></span>

* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddRazorPages*>
* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddControllersWithViews*>
* <xref:Microsoft.Extensions.DependencyInjection.MvcServiceCollectionExtensions.AddControllers*>

## <a name="json-patch-addnewtonsoftjson-and-systemtextjson"></a><span data-ttu-id="01c1f-116">JSON Patch、AddNewtonsoftJson 和 System.web</span><span class="sxs-lookup"><span data-stu-id="01c1f-116">JSON Patch, AddNewtonsoftJson, and System.Text.Json</span></span>

<span data-ttu-id="01c1f-117">`AddNewtonsoftJson`取代 `System.Text.Json` 以為基礎的輸入和輸出格式器，用於格式化**所有**JSON 內容。</span><span class="sxs-lookup"><span data-stu-id="01c1f-117">`AddNewtonsoftJson` replaces the `System.Text.Json`-based input and output formatters used for formatting **all** JSON content.</span></span> <span data-ttu-id="01c1f-118">若要使用新增對 JSON 修補程式的支援 `Newtonsoft.Json` ，同時讓其他格式器保持不變，請更新專案的 `Startup.ConfigureServices` 方法，如下所示：</span><span class="sxs-lookup"><span data-stu-id="01c1f-118">To add support for JSON Patch using `Newtonsoft.Json`, while leaving the other formatters unchanged, update the project's `Startup.ConfigureServices` method as follows:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet)]

<span data-ttu-id="01c1f-119">上述程式碼需要 `Microsoft.AspNetCore.Mvc.NewtonsoftJson` 封裝和下列 `using` 語句：</span><span class="sxs-lookup"><span data-stu-id="01c1f-119">The preceding code requires the `Microsoft.AspNetCore.Mvc.NewtonsoftJson` package and the following `using` statements:</span></span>

[!code-csharp[](jsonpatch/samples/3.0/WebApp1/Startup.cs?name=snippet1)]

## <a name="patch-http-request-method"></a><span data-ttu-id="01c1f-120">PATCH HTTP 要求方法</span><span class="sxs-lookup"><span data-stu-id="01c1f-120">PATCH HTTP request method</span></span>

<span data-ttu-id="01c1f-121">PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。</span><span class="sxs-lookup"><span data-stu-id="01c1f-121">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="01c1f-122">它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。</span><span class="sxs-lookup"><span data-stu-id="01c1f-122">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="01c1f-123">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="01c1f-123">JSON Patch</span></span>

<span data-ttu-id="01c1f-124">[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。</span><span class="sxs-lookup"><span data-stu-id="01c1f-124">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="01c1f-125">JSON Patch 文件具有一個「作業」\*\* 陣列。</span><span class="sxs-lookup"><span data-stu-id="01c1f-125">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="01c1f-126">每個作業都會識別特定類型的變更。</span><span class="sxs-lookup"><span data-stu-id="01c1f-126">Each operation identifies a particular type of change.</span></span> <span data-ttu-id="01c1f-127">這類變更的範例包括新增陣列元素或取代屬性值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-127">Examples of such changes include adding an array element or replacing a property value.</span></span>

<span data-ttu-id="01c1f-128">例如，下列 JSON 檔代表資源、資源的 JSON 修補程式檔，以及套用修補程式作業的結果。</span><span class="sxs-lookup"><span data-stu-id="01c1f-128">For example, the following JSON documents represent a resource, a JSON Patch document for the resource, and the result of applying the Patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="01c1f-129">資源範例</span><span class="sxs-lookup"><span data-stu-id="01c1f-129">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="01c1f-130">JSON 修補範例</span><span class="sxs-lookup"><span data-stu-id="01c1f-130">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="01c1f-131">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="01c1f-131">In the preceding JSON:</span></span>

* <span data-ttu-id="01c1f-132">`op` 屬性會指出作業的類型。</span><span class="sxs-lookup"><span data-stu-id="01c1f-132">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="01c1f-133">`path` 屬性會指出要更新的元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-133">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="01c1f-134">`value` 屬性會提供新值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-134">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="01c1f-135">修補之後的資源</span><span class="sxs-lookup"><span data-stu-id="01c1f-135">Resource after patch</span></span>

<span data-ttu-id="01c1f-136">以下是套用上述 JSON Patch 文件之後的資源：</span><span class="sxs-lookup"><span data-stu-id="01c1f-136">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="01c1f-137">將 JSON 修補檔套用至資源所做的變更是不可部分完成的。</span><span class="sxs-lookup"><span data-stu-id="01c1f-137">The changes made by applying a JSON Patch document to a resource are atomic.</span></span> <span data-ttu-id="01c1f-138">如果清單中的任何作業失敗，則不會套用清單中的任何作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-138">If any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="01c1f-139">路徑語法</span><span class="sxs-lookup"><span data-stu-id="01c1f-139">Path syntax</span></span>

<span data-ttu-id="01c1f-140">作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。</span><span class="sxs-lookup"><span data-stu-id="01c1f-140">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="01c1f-141">例如：`"/address/zipCode"`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-141">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="01c1f-142">以零為起始的索引可用來指定陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-142">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="01c1f-143">`addresses` 陣列的第一個元素會在 `/addresses/0` 上。</span><span class="sxs-lookup"><span data-stu-id="01c1f-143">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="01c1f-144">到 `add` 陣列結尾，請使用連字號（ `-` ），而不是索引編號： `/addresses/-` 。</span><span class="sxs-lookup"><span data-stu-id="01c1f-144">To `add` to the end of an array, use a hyphen (`-`) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="01c1f-145">作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-145">Operations</span></span>

<span data-ttu-id="01c1f-146">下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：</span><span class="sxs-lookup"><span data-stu-id="01c1f-146">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="01c1f-147">作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-147">Operation</span></span>  | <span data-ttu-id="01c1f-148">備忘稿</span><span class="sxs-lookup"><span data-stu-id="01c1f-148">Notes</span></span> |
|---
<span data-ttu-id="01c1f-149">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-149">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-150">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-150">'Blazor'</span></span>
- <span data-ttu-id="01c1f-151">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-151">'Identity'</span></span>
- <span data-ttu-id="01c1f-152">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-152">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-153">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-153">'Razor'</span></span>
- <span data-ttu-id="01c1f-154">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-154">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-155">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-155">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-156">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-156">'Blazor'</span></span>
- <span data-ttu-id="01c1f-157">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-157">'Identity'</span></span>
- <span data-ttu-id="01c1f-158">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-158">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-159">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-159">'Razor'</span></span>
- <span data-ttu-id="01c1f-160">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-160">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-161">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-161">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-162">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-162">'Blazor'</span></span>
- <span data-ttu-id="01c1f-163">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-163">'Identity'</span></span>
- <span data-ttu-id="01c1f-164">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-164">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-165">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-165">'Razor'</span></span>
- <span data-ttu-id="01c1f-166">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-166">'SignalR' uid:</span></span> 

<span data-ttu-id="01c1f-167">------|---
標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-167">------|---
title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-168">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-168">'Blazor'</span></span>
- <span data-ttu-id="01c1f-169">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-169">'Identity'</span></span>
- <span data-ttu-id="01c1f-170">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-170">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-171">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-171">'Razor'</span></span>
- <span data-ttu-id="01c1f-172">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-172">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-173">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-173">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-174">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-174">'Blazor'</span></span>
- <span data-ttu-id="01c1f-175">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-175">'Identity'</span></span>
- <span data-ttu-id="01c1f-176">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-176">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-177">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-177">'Razor'</span></span>
- <span data-ttu-id="01c1f-178">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-178">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-179">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-179">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-180">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-180">'Blazor'</span></span>
- <span data-ttu-id="01c1f-181">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-181">'Identity'</span></span>
- <span data-ttu-id="01c1f-182">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-182">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-183">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-183">'Razor'</span></span>
- <span data-ttu-id="01c1f-184">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-184">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-185">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-185">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-186">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-186">'Blazor'</span></span>
- <span data-ttu-id="01c1f-187">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-187">'Identity'</span></span>
- <span data-ttu-id="01c1f-188">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-188">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-189">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-189">'Razor'</span></span>
- <span data-ttu-id="01c1f-190">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-190">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-191">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-191">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-192">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-192">'Blazor'</span></span>
- <span data-ttu-id="01c1f-193">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-193">'Identity'</span></span>
- <span data-ttu-id="01c1f-194">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-194">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-195">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-195">'Razor'</span></span>
- <span data-ttu-id="01c1f-196">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-196">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-197">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-197">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-198">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-198">'Blazor'</span></span>
- <span data-ttu-id="01c1f-199">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-199">'Identity'</span></span>
- <span data-ttu-id="01c1f-200">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-200">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-201">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-201">'Razor'</span></span>
- <span data-ttu-id="01c1f-202">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-202">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-203">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-203">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-204">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-204">'Blazor'</span></span>
- <span data-ttu-id="01c1f-205">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-205">'Identity'</span></span>
- <span data-ttu-id="01c1f-206">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-206">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-207">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-207">'Razor'</span></span>
- <span data-ttu-id="01c1f-208">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-208">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-209">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-209">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-210">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-210">'Blazor'</span></span>
- <span data-ttu-id="01c1f-211">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-211">'Identity'</span></span>
- <span data-ttu-id="01c1f-212">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-212">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-213">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-213">'Razor'</span></span>
- <span data-ttu-id="01c1f-214">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-214">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-215">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-215">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-216">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-216">'Blazor'</span></span>
- <span data-ttu-id="01c1f-217">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-217">'Identity'</span></span>
- <span data-ttu-id="01c1f-218">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-218">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-219">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-219">'Razor'</span></span>
- <span data-ttu-id="01c1f-220">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-220">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-221">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-221">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-222">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-222">'Blazor'</span></span>
- <span data-ttu-id="01c1f-223">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-223">'Identity'</span></span>
- <span data-ttu-id="01c1f-224">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-224">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-225">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-225">'Razor'</span></span>
- <span data-ttu-id="01c1f-226">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-226">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-227">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-227">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-228">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-228">'Blazor'</span></span>
- <span data-ttu-id="01c1f-229">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-229">'Identity'</span></span>
- <span data-ttu-id="01c1f-230">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-230">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-231">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-231">'Razor'</span></span>
- <span data-ttu-id="01c1f-232">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-232">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-233">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-233">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-234">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-234">'Blazor'</span></span>
- <span data-ttu-id="01c1f-235">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-235">'Identity'</span></span>
- <span data-ttu-id="01c1f-236">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-236">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-237">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-237">'Razor'</span></span>
- <span data-ttu-id="01c1f-238">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-238">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-239">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-239">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-240">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-240">'Blazor'</span></span>
- <span data-ttu-id="01c1f-241">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-241">'Identity'</span></span>
- <span data-ttu-id="01c1f-242">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-242">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-243">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-243">'Razor'</span></span>
- <span data-ttu-id="01c1f-244">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-244">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-245">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-245">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-246">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-246">'Blazor'</span></span>
- <span data-ttu-id="01c1f-247">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-247">'Identity'</span></span>
- <span data-ttu-id="01c1f-248">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-248">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-249">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-249">'Razor'</span></span>
- <span data-ttu-id="01c1f-250">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-250">'SignalR' uid:</span></span> 

<span data-ttu-id="01c1f-251">----------------| |`add`     |新增屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-251">----------------| | `add`     | Add a property or array element.</span></span> <span data-ttu-id="01c1f-252">針對現有的屬性： set 值。 ||`remove`  |移除屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-252">For existing property: set value.| | `remove`  | Remove a property or array element.</span></span> <span data-ttu-id="01c1f-253">| |`replace` |相同于， `remove` 後面接著 `add` 相同的位置。</span><span class="sxs-lookup"><span data-stu-id="01c1f-253">| | `replace` | Same as `remove` followed by `add` at same location.</span></span> <span data-ttu-id="01c1f-254">| |`move`    |與 `remove` 從來源開始，然後 `add` 使用來源的值，後面接著至目的地。</span><span class="sxs-lookup"><span data-stu-id="01c1f-254">| | `move`    | Same as `remove` from source followed by `add` to destination using value from source.</span></span> <span data-ttu-id="01c1f-255">| |`copy`    |`add`使用來源的值，與目的地相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-255">| | `copy`    | Same as `add` to destination using value from source.</span></span> <span data-ttu-id="01c1f-256">| |`test`    |如果提供的值為，則傳回成功狀態碼 `path` `value` 。 |</span><span class="sxs-lookup"><span data-stu-id="01c1f-256">| | `test`    | Return success status code if value at `path` = provided `value`.|</span></span>

## <a name="json-patch-in-aspnet-core"></a><span data-ttu-id="01c1f-257">ASP.NET Core 中的 JSON 修補程式</span><span class="sxs-lookup"><span data-stu-id="01c1f-257">JSON Patch in ASP.NET Core</span></span>

<span data-ttu-id="01c1f-258">[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。</span><span class="sxs-lookup"><span data-stu-id="01c1f-258">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="01c1f-259">動作方法程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-259">Action method code</span></span>

<span data-ttu-id="01c1f-260">在 API 控制器中，JSON Patch 的動作方法：</span><span class="sxs-lookup"><span data-stu-id="01c1f-260">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="01c1f-261">使用 `HttpPatch` 屬性來標註。</span><span class="sxs-lookup"><span data-stu-id="01c1f-261">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="01c1f-262">接受 `JsonPatchDocument<T>` ，通常使用 `[FromBody]` 。</span><span class="sxs-lookup"><span data-stu-id="01c1f-262">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="01c1f-263">呼叫修補文件上的 `ApplyTo` 以套用變更。</span><span class="sxs-lookup"><span data-stu-id="01c1f-263">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="01c1f-264">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="01c1f-264">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="01c1f-265">範例應用程式中的這段程式碼可與下列模型搭配運作 `Customer` ：</span><span class="sxs-lookup"><span data-stu-id="01c1f-265">This code from the sample app works with the following `Customer` model:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="01c1f-266">範例動作方法：</span><span class="sxs-lookup"><span data-stu-id="01c1f-266">The sample action method:</span></span>

* <span data-ttu-id="01c1f-267">建構 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-267">Constructs a `Customer`.</span></span>
* <span data-ttu-id="01c1f-268">套用修補檔案。</span><span class="sxs-lookup"><span data-stu-id="01c1f-268">Applies the patch.</span></span>
* <span data-ttu-id="01c1f-269">在回應本文中傳回結果。</span><span class="sxs-lookup"><span data-stu-id="01c1f-269">Returns the result in the body of the response.</span></span>

<span data-ttu-id="01c1f-270">在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="01c1f-270">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="01c1f-271">模型狀態</span><span class="sxs-lookup"><span data-stu-id="01c1f-271">Model state</span></span>

<span data-ttu-id="01c1f-272">上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。</span><span class="sxs-lookup"><span data-stu-id="01c1f-272">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="01c1f-273">使用此選項，您就能在回應中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="01c1f-273">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="01c1f-274">下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：</span><span class="sxs-lookup"><span data-stu-id="01c1f-274">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="01c1f-275">動態物件</span><span class="sxs-lookup"><span data-stu-id="01c1f-275">Dynamic objects</span></span>

<span data-ttu-id="01c1f-276">下列動作方法範例顯示如何將修補程式套用至動態物件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-276">The following action method example shows how to apply a patch to a dynamic object:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="01c1f-277">新增作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-277">The add operation</span></span>

* <span data-ttu-id="01c1f-278">如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。</span><span class="sxs-lookup"><span data-stu-id="01c1f-278">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="01c1f-279">如果 `path` 指向屬性：設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-279">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="01c1f-280">如果 `path` 指向不存在的位置：</span><span class="sxs-lookup"><span data-stu-id="01c1f-280">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="01c1f-281">如果要修補的資源是動態物件：加入屬性。</span><span class="sxs-lookup"><span data-stu-id="01c1f-281">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="01c1f-282">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-282">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="01c1f-283">下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。</span><span class="sxs-lookup"><span data-stu-id="01c1f-283">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="01c1f-284">移除作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-284">The remove operation</span></span>

* <span data-ttu-id="01c1f-285">如果 `path` 指向陣列元素：移除該元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-285">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="01c1f-286">如果 `path` 指向屬性：</span><span class="sxs-lookup"><span data-stu-id="01c1f-286">If `path` points to a property:</span></span>
  * <span data-ttu-id="01c1f-287">如果要修補的資源是動態物件：移除屬性。</span><span class="sxs-lookup"><span data-stu-id="01c1f-287">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="01c1f-288">如果要修補的資源是靜態物件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-288">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="01c1f-289">如果屬性可為 Null：將它設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="01c1f-289">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="01c1f-290">如果屬性不可為 Null，則將它設定為 `default<T>`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-290">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="01c1f-291">下列範例修補檔會將設定 `CustomerName` 為 null 並刪除 `Orders[0]` ：</span><span class="sxs-lookup"><span data-stu-id="01c1f-291">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="01c1f-292">取代作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-292">The replace operation</span></span>

<span data-ttu-id="01c1f-293">此作業在功能上與 `remove` 之後接著 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-293">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="01c1f-294">下列範例修補檔會設定的值 `CustomerName` ，並將取代 `Orders[0]` 為新的 `Order` 物件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-294">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="01c1f-295">移動作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-295">The move operation</span></span>

* <span data-ttu-id="01c1f-296">如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `from` 元素上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-296">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="01c1f-297">如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-297">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="01c1f-298">如果 `path` 指向不存在的屬性：</span><span class="sxs-lookup"><span data-stu-id="01c1f-298">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="01c1f-299">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-299">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="01c1f-300">如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-300">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="01c1f-301">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-301">The following sample patch document:</span></span>

* <span data-ttu-id="01c1f-302">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-302">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="01c1f-303">將 `Orders[0].OrderName` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="01c1f-303">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="01c1f-304">將 `Orders[1]` 移到 `Orders[0]` 前面。</span><span class="sxs-lookup"><span data-stu-id="01c1f-304">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="01c1f-305">複製作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-305">The copy operation</span></span>

<span data-ttu-id="01c1f-306">此作業在功能上與不含最後 `remove` 步驟的 `move` 作業相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-306">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="01c1f-307">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-307">The following sample patch document:</span></span>

* <span data-ttu-id="01c1f-308">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-308">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="01c1f-309">在 `Orders[0]` 前面插入 `Orders[1]` 的複本。</span><span class="sxs-lookup"><span data-stu-id="01c1f-309">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="01c1f-310">測試作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-310">The test operation</span></span>

<span data-ttu-id="01c1f-311">如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-311">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="01c1f-312">在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。</span><span class="sxs-lookup"><span data-stu-id="01c1f-312">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="01c1f-313">`test` 作業通常會用來防止在發生並行衝突時進行更新。</span><span class="sxs-lookup"><span data-stu-id="01c1f-313">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="01c1f-314">如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：</span><span class="sxs-lookup"><span data-stu-id="01c1f-314">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="01c1f-315">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-315">Get the code</span></span>

<span data-ttu-id="01c1f-316">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples)。</span><span class="sxs-lookup"><span data-stu-id="01c1f-316">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples).</span></span> <span data-ttu-id="01c1f-317">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="01c1f-317">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="01c1f-318">若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="01c1f-318">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="01c1f-319">URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="01c1f-319">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="01c1f-320">HTTP 方法：`PATCH`</span><span class="sxs-lookup"><span data-stu-id="01c1f-320">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="01c1f-321">標題：`Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="01c1f-321">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="01c1f-322">主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。</span><span class="sxs-lookup"><span data-stu-id="01c1f-322">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01c1f-323">其他資源</span><span class="sxs-lookup"><span data-stu-id="01c1f-323">Additional resources</span></span>

* <span data-ttu-id="01c1f-324">[IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-324">[IETF RFC 5789 PATCH method specification](https://tools.ietf.org/html/rfc5789)</span></span>
* <span data-ttu-id="01c1f-325">[IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-325">[IETF RFC 6902 JSON Patch specification](https://tools.ietf.org/html/rfc6902)</span></span>
* <span data-ttu-id="01c1f-326">[IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-326">[IETF RFC 6901 JSON Patch path format spec](https://tools.ietf.org/html/rfc6901)</span></span>
* <span data-ttu-id="01c1f-327">[JSON Patch 文件](https://jsonpatch.com/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="01c1f-327">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="01c1f-328">包含用於建立 JSON Patch 文件的資源連結。</span><span class="sxs-lookup"><span data-stu-id="01c1f-328">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="01c1f-329">ASP.NET Core JSON Patch 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-329">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end

::: moniker range="< aspnetcore-3.0"

<span data-ttu-id="01c1f-330">本文說明如何處理 ASP.NET Core Web API 中的 JSON Patch 要求。</span><span class="sxs-lookup"><span data-stu-id="01c1f-330">This article explains how to handle JSON Patch requests in an ASP.NET Core web API.</span></span>

## <a name="patch-http-request-method"></a><span data-ttu-id="01c1f-331">PATCH HTTP 要求方法</span><span class="sxs-lookup"><span data-stu-id="01c1f-331">PATCH HTTP request method</span></span>

<span data-ttu-id="01c1f-332">PUT 和 [PATCH](https://tools.ietf.org/html/rfc5789) \(英文\) 方法均用來更新現有的資源。</span><span class="sxs-lookup"><span data-stu-id="01c1f-332">The PUT and [PATCH](https://tools.ietf.org/html/rfc5789) methods are used to update an existing resource.</span></span> <span data-ttu-id="01c1f-333">它們之間的差異是 PUT 會取代整個資源，而 PATCH 只會指定變更。</span><span class="sxs-lookup"><span data-stu-id="01c1f-333">The difference between them is that PUT replaces the entire resource, while PATCH specifies only the changes.</span></span>

## <a name="json-patch"></a><span data-ttu-id="01c1f-334">JSON Patch</span><span class="sxs-lookup"><span data-stu-id="01c1f-334">JSON Patch</span></span>

<span data-ttu-id="01c1f-335">[JSON Patch](https://tools.ietf.org/html/rfc6902) \(英文\) 是一種格式，可用來指定要套用至資源的更新。</span><span class="sxs-lookup"><span data-stu-id="01c1f-335">[JSON Patch](https://tools.ietf.org/html/rfc6902) is a format for specifying updates to be applied to a resource.</span></span> <span data-ttu-id="01c1f-336">JSON Patch 文件具有一個「作業」\*\* 陣列。</span><span class="sxs-lookup"><span data-stu-id="01c1f-336">A JSON Patch document has an array of *operations*.</span></span> <span data-ttu-id="01c1f-337">每個作業都會識別特定類型的變更，例如，加入陣列元素或取代屬性值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-337">Each operation identifies a particular type of change, such as add an array element or replace a property value.</span></span>

<span data-ttu-id="01c1f-338">例如，下列 JSON 文件代表一個資源、一份適用於該資源的 JSON 修補文件，以及套用修補作業的結果。</span><span class="sxs-lookup"><span data-stu-id="01c1f-338">For example, the following JSON documents represent a resource, a JSON patch document for the resource, and the result of applying the patch operations.</span></span>

### <a name="resource-example"></a><span data-ttu-id="01c1f-339">資源範例</span><span class="sxs-lookup"><span data-stu-id="01c1f-339">Resource example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/customer.json)]

### <a name="json-patch-example"></a><span data-ttu-id="01c1f-340">JSON 修補範例</span><span class="sxs-lookup"><span data-stu-id="01c1f-340">JSON patch example</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

<span data-ttu-id="01c1f-341">在上述 JSON 中：</span><span class="sxs-lookup"><span data-stu-id="01c1f-341">In the preceding JSON:</span></span>

* <span data-ttu-id="01c1f-342">`op` 屬性會指出作業的類型。</span><span class="sxs-lookup"><span data-stu-id="01c1f-342">The `op` property indicates the type of operation.</span></span>
* <span data-ttu-id="01c1f-343">`path` 屬性會指出要更新的元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-343">The `path` property indicates the element to update.</span></span>
* <span data-ttu-id="01c1f-344">`value` 屬性會提供新值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-344">The `value` property provides the new value.</span></span>

### <a name="resource-after-patch"></a><span data-ttu-id="01c1f-345">修補之後的資源</span><span class="sxs-lookup"><span data-stu-id="01c1f-345">Resource after patch</span></span>

<span data-ttu-id="01c1f-346">以下是套用上述 JSON Patch 文件之後的資源：</span><span class="sxs-lookup"><span data-stu-id="01c1f-346">Here's the resource after applying the preceding JSON Patch document:</span></span>

```json
{
  "customerName": "Barry",
  "orders": [
    {
      "orderName": "Order0",
      "orderType": null
    },
    {
      "orderName": "Order1",
      "orderType": null
    },
    {
      "orderName": "Order2",
      "orderType": null
    }
  ]
}
```

<span data-ttu-id="01c1f-347">藉由將 JSON Patch 文件套用至資源所做的變更是不可部分完成的：如果清單中有任何作業失敗，就不會套用清單中的任何作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-347">The changes made by applying a JSON Patch document to a resource are atomic: if any operation in the list fails, no operation in the list is applied.</span></span>

## <a name="path-syntax"></a><span data-ttu-id="01c1f-348">路徑語法</span><span class="sxs-lookup"><span data-stu-id="01c1f-348">Path syntax</span></span>

<span data-ttu-id="01c1f-349">作業物件的 [path](https://tools.ietf.org/html/rfc6901) \(英文\) 屬性在層級之間有斜線。</span><span class="sxs-lookup"><span data-stu-id="01c1f-349">The [path](https://tools.ietf.org/html/rfc6901) property of an operation object has slashes between levels.</span></span> <span data-ttu-id="01c1f-350">例如：`"/address/zipCode"`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-350">For example, `"/address/zipCode"`.</span></span>

<span data-ttu-id="01c1f-351">以零為起始的索引可用來指定陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-351">Zero-based indexes are used to specify array elements.</span></span> <span data-ttu-id="01c1f-352">`addresses` 陣列的第一個元素會在 `/addresses/0` 上。</span><span class="sxs-lookup"><span data-stu-id="01c1f-352">The first element of the `addresses` array would be at `/addresses/0`.</span></span> <span data-ttu-id="01c1f-353">若要 `add` 到陣列結尾處，請使用連字號 (-) 而不是索引號碼：`/addresses/-`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-353">To `add` to the end of an array, use a hyphen (-) rather than an index number: `/addresses/-`.</span></span>

### <a name="operations"></a><span data-ttu-id="01c1f-354">作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-354">Operations</span></span>

<span data-ttu-id="01c1f-355">下表顯示支援的作業，如 [JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\) 中所定義：</span><span class="sxs-lookup"><span data-stu-id="01c1f-355">The following table shows supported operations as defined in the [JSON Patch specification](https://tools.ietf.org/html/rfc6902):</span></span>

|<span data-ttu-id="01c1f-356">作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-356">Operation</span></span>  | <span data-ttu-id="01c1f-357">備忘稿</span><span class="sxs-lookup"><span data-stu-id="01c1f-357">Notes</span></span> |
|---
<span data-ttu-id="01c1f-358">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-358">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-359">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-359">'Blazor'</span></span>
- <span data-ttu-id="01c1f-360">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-360">'Identity'</span></span>
- <span data-ttu-id="01c1f-361">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-361">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-362">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-362">'Razor'</span></span>
- <span data-ttu-id="01c1f-363">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-363">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-364">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-364">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-365">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-365">'Blazor'</span></span>
- <span data-ttu-id="01c1f-366">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-366">'Identity'</span></span>
- <span data-ttu-id="01c1f-367">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-367">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-368">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-368">'Razor'</span></span>
- <span data-ttu-id="01c1f-369">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-369">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-370">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-370">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-371">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-371">'Blazor'</span></span>
- <span data-ttu-id="01c1f-372">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-372">'Identity'</span></span>
- <span data-ttu-id="01c1f-373">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-373">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-374">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-374">'Razor'</span></span>
- <span data-ttu-id="01c1f-375">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-375">'SignalR' uid:</span></span> 

<span data-ttu-id="01c1f-376">------|---
標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-376">------|---
title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-377">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-377">'Blazor'</span></span>
- <span data-ttu-id="01c1f-378">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-378">'Identity'</span></span>
- <span data-ttu-id="01c1f-379">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-379">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-380">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-380">'Razor'</span></span>
- <span data-ttu-id="01c1f-381">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-381">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-382">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-382">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-383">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-383">'Blazor'</span></span>
- <span data-ttu-id="01c1f-384">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-384">'Identity'</span></span>
- <span data-ttu-id="01c1f-385">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-385">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-386">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-386">'Razor'</span></span>
- <span data-ttu-id="01c1f-387">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-387">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-388">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-388">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-389">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-389">'Blazor'</span></span>
- <span data-ttu-id="01c1f-390">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-390">'Identity'</span></span>
- <span data-ttu-id="01c1f-391">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-391">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-392">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-392">'Razor'</span></span>
- <span data-ttu-id="01c1f-393">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-393">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-394">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-394">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-395">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-395">'Blazor'</span></span>
- <span data-ttu-id="01c1f-396">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-396">'Identity'</span></span>
- <span data-ttu-id="01c1f-397">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-397">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-398">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-398">'Razor'</span></span>
- <span data-ttu-id="01c1f-399">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-399">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-400">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-400">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-401">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-401">'Blazor'</span></span>
- <span data-ttu-id="01c1f-402">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-402">'Identity'</span></span>
- <span data-ttu-id="01c1f-403">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-403">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-404">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-404">'Razor'</span></span>
- <span data-ttu-id="01c1f-405">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-405">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-406">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-406">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-407">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-407">'Blazor'</span></span>
- <span data-ttu-id="01c1f-408">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-408">'Identity'</span></span>
- <span data-ttu-id="01c1f-409">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-409">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-410">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-410">'Razor'</span></span>
- <span data-ttu-id="01c1f-411">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-411">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-412">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-412">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-413">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-413">'Blazor'</span></span>
- <span data-ttu-id="01c1f-414">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-414">'Identity'</span></span>
- <span data-ttu-id="01c1f-415">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-415">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-416">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-416">'Razor'</span></span>
- <span data-ttu-id="01c1f-417">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-417">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-418">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-418">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-419">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-419">'Blazor'</span></span>
- <span data-ttu-id="01c1f-420">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-420">'Identity'</span></span>
- <span data-ttu-id="01c1f-421">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-421">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-422">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-422">'Razor'</span></span>
- <span data-ttu-id="01c1f-423">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-423">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-424">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-424">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-425">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-425">'Blazor'</span></span>
- <span data-ttu-id="01c1f-426">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-426">'Identity'</span></span>
- <span data-ttu-id="01c1f-427">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-427">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-428">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-428">'Razor'</span></span>
- <span data-ttu-id="01c1f-429">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-429">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-430">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-430">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-431">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-431">'Blazor'</span></span>
- <span data-ttu-id="01c1f-432">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-432">'Identity'</span></span>
- <span data-ttu-id="01c1f-433">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-433">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-434">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-434">'Razor'</span></span>
- <span data-ttu-id="01c1f-435">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-435">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-436">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-436">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-437">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-437">'Blazor'</span></span>
- <span data-ttu-id="01c1f-438">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-438">'Identity'</span></span>
- <span data-ttu-id="01c1f-439">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-439">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-440">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-440">'Razor'</span></span>
- <span data-ttu-id="01c1f-441">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-441">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-442">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-442">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-443">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-443">'Blazor'</span></span>
- <span data-ttu-id="01c1f-444">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-444">'Identity'</span></span>
- <span data-ttu-id="01c1f-445">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-445">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-446">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-446">'Razor'</span></span>
- <span data-ttu-id="01c1f-447">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-447">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-448">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-448">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-449">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-449">'Blazor'</span></span>
- <span data-ttu-id="01c1f-450">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-450">'Identity'</span></span>
- <span data-ttu-id="01c1f-451">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-451">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-452">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-452">'Razor'</span></span>
- <span data-ttu-id="01c1f-453">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-453">'SignalR' uid:</span></span> 

-
<span data-ttu-id="01c1f-454">標題： author： description： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="01c1f-454">title: author: description: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="01c1f-455">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-455">'Blazor'</span></span>
- <span data-ttu-id="01c1f-456">'Identity'</span><span class="sxs-lookup"><span data-stu-id="01c1f-456">'Identity'</span></span>
- <span data-ttu-id="01c1f-457">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="01c1f-457">'Let's Encrypt'</span></span>
- <span data-ttu-id="01c1f-458">'Razor'</span><span class="sxs-lookup"><span data-stu-id="01c1f-458">'Razor'</span></span>
- <span data-ttu-id="01c1f-459">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="01c1f-459">'SignalR' uid:</span></span> 

<span data-ttu-id="01c1f-460">----------------| |`add`     |新增屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-460">----------------| | `add`     | Add a property or array element.</span></span> <span data-ttu-id="01c1f-461">針對現有的屬性： set 值。 ||`remove`  |移除屬性或陣列元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-461">For existing property: set value.| | `remove`  | Remove a property or array element.</span></span> <span data-ttu-id="01c1f-462">| |`replace` |相同于， `remove` 後面接著 `add` 相同的位置。</span><span class="sxs-lookup"><span data-stu-id="01c1f-462">| | `replace` | Same as `remove` followed by `add` at same location.</span></span> <span data-ttu-id="01c1f-463">| |`move`    |與 `remove` 從來源開始，然後 `add` 使用來源的值，後面接著至目的地。</span><span class="sxs-lookup"><span data-stu-id="01c1f-463">| | `move`    | Same as `remove` from source followed by `add` to destination using value from source.</span></span> <span data-ttu-id="01c1f-464">| |`copy`    |`add`使用來源的值，與目的地相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-464">| | `copy`    | Same as `add` to destination using value from source.</span></span> <span data-ttu-id="01c1f-465">| |`test`    |如果提供的值為，則傳回成功狀態碼 `path` `value` 。 |</span><span class="sxs-lookup"><span data-stu-id="01c1f-465">| | `test`    | Return success status code if value at `path` = provided `value`.|</span></span>

## <a name="jsonpatch-in-aspnet-core"></a><span data-ttu-id="01c1f-466">ASP.NET Core 中的 JsonPatch</span><span class="sxs-lookup"><span data-stu-id="01c1f-466">JsonPatch in ASP.NET Core</span></span>

<span data-ttu-id="01c1f-467">[Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) \(英文\) NuGet 套件中會提供 JSON Patch 的 ASP.NET Core 實作。</span><span class="sxs-lookup"><span data-stu-id="01c1f-467">The ASP.NET Core implementation of JSON Patch is provided in the [Microsoft.AspNetCore.JsonPatch](https://www.nuget.org/packages/microsoft.aspnetcore.jsonpatch/) NuGet package.</span></span> <span data-ttu-id="01c1f-468">此套件隨附於 [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) 中繼套件中。</span><span class="sxs-lookup"><span data-stu-id="01c1f-468">The package is included in the [Microsoft.AspnetCore.App](xref:fundamentals/metapackage-app) metapackage.</span></span>

## <a name="action-method-code"></a><span data-ttu-id="01c1f-469">動作方法程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-469">Action method code</span></span>

<span data-ttu-id="01c1f-470">在 API 控制器中，JSON Patch 的動作方法：</span><span class="sxs-lookup"><span data-stu-id="01c1f-470">In an API controller, an action method for JSON Patch:</span></span>

* <span data-ttu-id="01c1f-471">使用 `HttpPatch` 屬性來標註。</span><span class="sxs-lookup"><span data-stu-id="01c1f-471">Is annotated with the `HttpPatch` attribute.</span></span>
* <span data-ttu-id="01c1f-472">接受 `JsonPatchDocument<T>` ，通常使用 `[FromBody]` 。</span><span class="sxs-lookup"><span data-stu-id="01c1f-472">Accepts a `JsonPatchDocument<T>`, typically with `[FromBody]`.</span></span>
* <span data-ttu-id="01c1f-473">呼叫修補文件上的 `ApplyTo` 以套用變更。</span><span class="sxs-lookup"><span data-stu-id="01c1f-473">Calls `ApplyTo` on the patch document to apply the changes.</span></span>

<span data-ttu-id="01c1f-474">以下為範例：</span><span class="sxs-lookup"><span data-stu-id="01c1f-474">Here's an example:</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_PatchAction&highlight=1,3,9)]

<span data-ttu-id="01c1f-475">這段來自範例應用程式的程式碼會使用下列 `Customer` 模型。</span><span class="sxs-lookup"><span data-stu-id="01c1f-475">This code from the sample app works with the following `Customer` model.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Models/Customer.cs?name=snippet_Customer)]

[!code-csharp[](jsonpatch/samples/2.2/Models/Order.cs?name=snippet_Order)]

<span data-ttu-id="01c1f-476">範例動作方法：</span><span class="sxs-lookup"><span data-stu-id="01c1f-476">The sample action method:</span></span>

* <span data-ttu-id="01c1f-477">建構 `Customer`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-477">Constructs a `Customer`.</span></span>
* <span data-ttu-id="01c1f-478">套用修補檔案。</span><span class="sxs-lookup"><span data-stu-id="01c1f-478">Applies the patch.</span></span>
* <span data-ttu-id="01c1f-479">在回應本文中傳回結果。</span><span class="sxs-lookup"><span data-stu-id="01c1f-479">Returns the result in the body of the response.</span></span>

 <span data-ttu-id="01c1f-480">在實際的應用程式中，程式碼會從資料庫之類的存放區擷取資料，並在套用修補檔案之後更新資料庫。</span><span class="sxs-lookup"><span data-stu-id="01c1f-480">In a real app, the code would retrieve the data from a store such as a database and update the database after applying the patch.</span></span>

### <a name="model-state"></a><span data-ttu-id="01c1f-481">模型狀態</span><span class="sxs-lookup"><span data-stu-id="01c1f-481">Model state</span></span>

<span data-ttu-id="01c1f-482">上述動作方法範例會呼叫 `ApplyTo` 的多載，以取得模型狀態作為它的其中一個參數。</span><span class="sxs-lookup"><span data-stu-id="01c1f-482">The preceding action method example calls an overload of `ApplyTo` that takes model state as one of its parameters.</span></span> <span data-ttu-id="01c1f-483">使用此選項，您就能在回應中收到錯誤訊息。</span><span class="sxs-lookup"><span data-stu-id="01c1f-483">With this option, you can get error messages in responses.</span></span> <span data-ttu-id="01c1f-484">下列範例會針對 `test` 作業顯示「400 不正確的要求」回應的本文：</span><span class="sxs-lookup"><span data-stu-id="01c1f-484">The following example shows the body of a 400 Bad Request response for a `test` operation:</span></span>

```json
{
    "Customer": [
        "The current value 'John' at path 'customerName' is not equal to the test value 'Nancy'."
    ]
}
```

### <a name="dynamic-objects"></a><span data-ttu-id="01c1f-485">動態物件</span><span class="sxs-lookup"><span data-stu-id="01c1f-485">Dynamic objects</span></span>

<span data-ttu-id="01c1f-486">下列動作方法範例示範如何將修補檔案套用至動態物件。</span><span class="sxs-lookup"><span data-stu-id="01c1f-486">The following action method example shows how to apply a patch to a dynamic object.</span></span>

[!code-csharp[](jsonpatch/samples/2.2/Controllers/HomeController.cs?name=snippet_Dynamic)]

## <a name="the-add-operation"></a><span data-ttu-id="01c1f-487">新增作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-487">The add operation</span></span>

* <span data-ttu-id="01c1f-488">如果 `path` 指向陣列元素：將新元素插入至 `path` 所指定的元素之前。</span><span class="sxs-lookup"><span data-stu-id="01c1f-488">If `path` points to an array element: inserts new element before the one specified by `path`.</span></span>
* <span data-ttu-id="01c1f-489">如果 `path` 指向屬性：設定屬性值。</span><span class="sxs-lookup"><span data-stu-id="01c1f-489">If `path` points to a property: sets the property value.</span></span>
* <span data-ttu-id="01c1f-490">如果 `path` 指向不存在的位置：</span><span class="sxs-lookup"><span data-stu-id="01c1f-490">If `path` points to a nonexistent location:</span></span>
  * <span data-ttu-id="01c1f-491">如果要修補的資源是動態物件：加入屬性。</span><span class="sxs-lookup"><span data-stu-id="01c1f-491">If the resource to patch is a dynamic object: adds a property.</span></span>
  * <span data-ttu-id="01c1f-492">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-492">If the resource to patch is a static object: the request fails.</span></span>

<span data-ttu-id="01c1f-493">下列範例修補文件會設定 `CustomerName` 的值，並將 `Order` 物件加入至 `Orders` 陣列的結尾處。</span><span class="sxs-lookup"><span data-stu-id="01c1f-493">The following sample patch document sets the value of `CustomerName` and adds an `Order` object to the end of the `Orders` array.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/add.json)]

## <a name="the-remove-operation"></a><span data-ttu-id="01c1f-494">移除作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-494">The remove operation</span></span>

* <span data-ttu-id="01c1f-495">如果 `path` 指向陣列元素：移除該元素。</span><span class="sxs-lookup"><span data-stu-id="01c1f-495">If `path` points to an array element: removes the element.</span></span>
* <span data-ttu-id="01c1f-496">如果 `path` 指向屬性：</span><span class="sxs-lookup"><span data-stu-id="01c1f-496">If `path` points to a property:</span></span>
  * <span data-ttu-id="01c1f-497">如果要修補的資源是動態物件：移除屬性。</span><span class="sxs-lookup"><span data-stu-id="01c1f-497">If resource to patch is a dynamic object: removes the property.</span></span>
  * <span data-ttu-id="01c1f-498">如果要修補的資源是靜態物件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-498">If resource to patch is a static object:</span></span>
    * <span data-ttu-id="01c1f-499">如果屬性可為 Null：將它設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="01c1f-499">If the property is nullable: sets it to null.</span></span>
    * <span data-ttu-id="01c1f-500">如果屬性不可為 Null，則將它設定為 `default<T>`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-500">If the property is non-nullable, sets it to `default<T>`.</span></span>

<span data-ttu-id="01c1f-501">下列範例修補文件會將 `CustomerName` 設定為 Null 並刪除 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-501">The following sample patch document sets `CustomerName` to null and deletes `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/remove.json)]

## <a name="the-replace-operation"></a><span data-ttu-id="01c1f-502">取代作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-502">The replace operation</span></span>

<span data-ttu-id="01c1f-503">此作業在功能上與 `remove` 之後接著 `add` 相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-503">This operation is functionally the same as a `remove` followed by an `add`.</span></span>

<span data-ttu-id="01c1f-504">下列範例修補文件會設定 `CustomerName` 的值，並使用新的 `Order` 物件來取代 `Orders[0]`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-504">The following sample patch document sets the value of `CustomerName` and replaces `Orders[0]`with a new `Order` object.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/replace.json)]

## <a name="the-move-operation"></a><span data-ttu-id="01c1f-505">移動作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-505">The move operation</span></span>

* <span data-ttu-id="01c1f-506">如果 `path` 指向陣列元素：將 `from` 元素複製到 `path` 元素的位置，然後在 `from` 元素上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-506">If `path` points to an array element: copies `from` element to location of `path` element, then runs a `remove` operation on the `from` element.</span></span>
* <span data-ttu-id="01c1f-507">如果 `path` 指向屬性：將 `from` 屬性的值複製到 `path` 屬性，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-507">If `path` points to a property: copies value of `from` property to `path` property, then runs a `remove` operation on the `from` property.</span></span>
* <span data-ttu-id="01c1f-508">如果 `path` 指向不存在的屬性：</span><span class="sxs-lookup"><span data-stu-id="01c1f-508">If `path` points to a nonexistent property:</span></span>
  * <span data-ttu-id="01c1f-509">如果要修補的資源是靜態物件：要求失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-509">If the resource to patch is a static object: the request fails.</span></span>
  * <span data-ttu-id="01c1f-510">如果要修補的資源是動態物件：將 `from` 屬性複製到 `path` 所指出的位置，然後在 `from` 屬性上執行 `remove` 作業。</span><span class="sxs-lookup"><span data-stu-id="01c1f-510">If the resource to patch is a dynamic object: copies `from` property to location indicated by `path`, then runs a `remove` operation on the `from` property.</span></span>

<span data-ttu-id="01c1f-511">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-511">The following sample patch document:</span></span>

* <span data-ttu-id="01c1f-512">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-512">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="01c1f-513">將 `Orders[0].OrderName` 設定為 Null。</span><span class="sxs-lookup"><span data-stu-id="01c1f-513">Sets `Orders[0].OrderName` to null.</span></span>
* <span data-ttu-id="01c1f-514">將 `Orders[1]` 移到 `Orders[0]` 前面。</span><span class="sxs-lookup"><span data-stu-id="01c1f-514">Moves `Orders[1]` to before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/move.json)]

## <a name="the-copy-operation"></a><span data-ttu-id="01c1f-515">複製作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-515">The copy operation</span></span>

<span data-ttu-id="01c1f-516">此作業在功能上與不含最後 `remove` 步驟的 `move` 作業相同。</span><span class="sxs-lookup"><span data-stu-id="01c1f-516">This operation is functionally the same as a `move` operation without the final `remove` step.</span></span>

<span data-ttu-id="01c1f-517">下列範例修補文件：</span><span class="sxs-lookup"><span data-stu-id="01c1f-517">The following sample patch document:</span></span>

* <span data-ttu-id="01c1f-518">將 `Orders[0].OrderName` 的值複製到 `CustomerName`。</span><span class="sxs-lookup"><span data-stu-id="01c1f-518">Copies the value of `Orders[0].OrderName` to `CustomerName`.</span></span>
* <span data-ttu-id="01c1f-519">在 `Orders[0]` 前面插入 `Orders[1]` 的複本。</span><span class="sxs-lookup"><span data-stu-id="01c1f-519">Inserts a copy of `Orders[1]` before `Orders[0]`.</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/copy.json)]

## <a name="the-test-operation"></a><span data-ttu-id="01c1f-520">測試作業</span><span class="sxs-lookup"><span data-stu-id="01c1f-520">The test operation</span></span>

<span data-ttu-id="01c1f-521">如果 `path` 所指出位置上的值與 `value` 中所提供的值不同，則要求會失敗。</span><span class="sxs-lookup"><span data-stu-id="01c1f-521">If the value at the location indicated by `path` is different from the value provided in `value`, the request fails.</span></span> <span data-ttu-id="01c1f-522">在該情況下，整個 PATCH 要求會失敗，即使修補文件中的所有其他作業都成功也一樣。</span><span class="sxs-lookup"><span data-stu-id="01c1f-522">In that case, the whole PATCH request fails even if all other operations in the patch document would otherwise succeed.</span></span>

<span data-ttu-id="01c1f-523">`test` 作業通常會用來防止在發生並行衝突時進行更新。</span><span class="sxs-lookup"><span data-stu-id="01c1f-523">The `test` operation is commonly used to prevent an update when there's a concurrency conflict.</span></span>

<span data-ttu-id="01c1f-524">如果 `CustomerName` 的初始值是 "John"，則下列範例修補文件不會有任何作用，因為測試失敗：</span><span class="sxs-lookup"><span data-stu-id="01c1f-524">The following sample patch document has no effect if the initial value of `CustomerName` is "John", because the test fails:</span></span>

[!code-json[](jsonpatch/samples/2.2/JSON/test-fail.json)]

## <a name="get-the-code"></a><span data-ttu-id="01c1f-525">取得程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-525">Get the code</span></span>

<span data-ttu-id="01c1f-526">[查看或下載範例程式碼](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2)。</span><span class="sxs-lookup"><span data-stu-id="01c1f-526">[View or download sample code](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/web-api/jsonpatch/samples/2.2).</span></span> <span data-ttu-id="01c1f-527">([如何下載](xref:index#how-to-download-a-sample))。</span><span class="sxs-lookup"><span data-stu-id="01c1f-527">([How to download](xref:index#how-to-download-a-sample)).</span></span>

<span data-ttu-id="01c1f-528">若要測試範例，請執行應用程式，並使用下列設定來傳送 HTTP 要求：</span><span class="sxs-lookup"><span data-stu-id="01c1f-528">To test the sample, run the app and send HTTP requests with the following settings:</span></span>

* <span data-ttu-id="01c1f-529">URL： `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span><span class="sxs-lookup"><span data-stu-id="01c1f-529">URL: `http://localhost:{port}/jsonpatch/jsonpatchwithmodelstate`</span></span>
* <span data-ttu-id="01c1f-530">HTTP 方法：`PATCH`</span><span class="sxs-lookup"><span data-stu-id="01c1f-530">HTTP method: `PATCH`</span></span>
* <span data-ttu-id="01c1f-531">標題：`Content-Type: application/json-patch+json`</span><span class="sxs-lookup"><span data-stu-id="01c1f-531">Header: `Content-Type: application/json-patch+json`</span></span>
* <span data-ttu-id="01c1f-532">主體：從*json*專案資料夾複製並貼上其中一個 json 修補程式檔範例。</span><span class="sxs-lookup"><span data-stu-id="01c1f-532">Body: Copy and paste one of the JSON patch document samples from the *JSON* project folder.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="01c1f-533">其他資源</span><span class="sxs-lookup"><span data-stu-id="01c1f-533">Additional resources</span></span>

* <span data-ttu-id="01c1f-534">[IETF RFC 5789 PATCH 方法規格](https://tools.ietf.org/html/rfc5789) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-534">[IETF RFC 5789 PATCH method specification](https://tools.ietf.org/html/rfc5789)</span></span>
* <span data-ttu-id="01c1f-535">[IETF RFC 6902 JSON Patch 規格](https://tools.ietf.org/html/rfc6902) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-535">[IETF RFC 6902 JSON Patch specification](https://tools.ietf.org/html/rfc6902)</span></span>
* <span data-ttu-id="01c1f-536">[IETF RFC 6901 JSON Patch 路徑格式規格](https://tools.ietf.org/html/rfc6901) \(英文\)</span><span class="sxs-lookup"><span data-stu-id="01c1f-536">[IETF RFC 6901 JSON Patch path format spec](https://tools.ietf.org/html/rfc6901)</span></span>
* <span data-ttu-id="01c1f-537">[JSON Patch 文件](https://jsonpatch.com/) \(英文\)。</span><span class="sxs-lookup"><span data-stu-id="01c1f-537">[JSON Patch documentation](https://jsonpatch.com/).</span></span> <span data-ttu-id="01c1f-538">包含用於建立 JSON Patch 文件的資源連結。</span><span class="sxs-lookup"><span data-stu-id="01c1f-538">Includes links to resources for creating JSON Patch documents.</span></span>
* [<span data-ttu-id="01c1f-539">ASP.NET Core JSON Patch 原始程式碼</span><span class="sxs-lookup"><span data-stu-id="01c1f-539">ASP.NET Core JSON Patch source code</span></span>](https://github.com/dotnet/AspNetCore/tree/master/src/Features/JsonPatch/src)

::: moniker-end
