---
<span data-ttu-id="77289-101">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-101">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-102">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-102">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-103">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-103">'Blazor'</span></span>
- <span data-ttu-id="77289-104">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-104">'Identity'</span></span>
- <span data-ttu-id="77289-105">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-105">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-106">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-106">'Razor'</span></span>
- <span data-ttu-id="77289-107">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-107">'SignalR' uid:</span></span> 

---
# <a name="aspnet-core-blazor-event-handling"></a><span data-ttu-id="77289-108">ASP.NET Core Blazor 事件處理</span><span class="sxs-lookup"><span data-stu-id="77289-108">ASP.NET Core Blazor event handling</span></span>

<span data-ttu-id="77289-109">By [Luke Latham](https://github.com/guardrex)和[Daniel Roth](https://github.com/danroth27)</span><span class="sxs-lookup"><span data-stu-id="77289-109">By [Luke Latham](https://github.com/guardrex) and [Daniel Roth](https://github.com/danroth27)</span></span>

Razor<span data-ttu-id="77289-110">元件提供事件處理功能。</span><span class="sxs-lookup"><span data-stu-id="77289-110"> components provide event handling features.</span></span> <span data-ttu-id="77289-111">對於名為的 HTML 專案屬性 [`@on{EVENT}`](xref:mvc/views/razor#onevent) （例如， `@onclick` ）與委派類型的值，元件會將 Razor 屬性的值視為事件處理常式。</span><span class="sxs-lookup"><span data-stu-id="77289-111">For an HTML element attribute named [`@on{EVENT}`](xref:mvc/views/razor#onevent) (for example, `@onclick`) with a delegate-typed value, a Razor component treats the attribute's value as an event handler.</span></span>

<span data-ttu-id="77289-112">下列程式碼會在 `UpdateHeading` UI 中選取按鈕時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="77289-112">The following code calls the `UpdateHeading` method when the button is selected in the UI:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private void UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

<span data-ttu-id="77289-113">下列程式碼會在 `CheckChanged` UI 中變更核取方塊時呼叫方法：</span><span class="sxs-lookup"><span data-stu-id="77289-113">The following code calls the `CheckChanged` method when the check box is changed in the UI:</span></span>

```razor
<input type="checkbox" class="form-check-input" @onchange="CheckChanged" />

@code {
    private void CheckChanged()
    {
        ...
    }
}
```

<span data-ttu-id="77289-114">事件處理常式也可以是非同步，並且會傳回 <xref:System.Threading.Tasks.Task> 。</span><span class="sxs-lookup"><span data-stu-id="77289-114">Event handlers can also be asynchronous and return a <xref:System.Threading.Tasks.Task>.</span></span> <span data-ttu-id="77289-115">不需要手動呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes)。</span><span class="sxs-lookup"><span data-stu-id="77289-115">There's no need to manually call [StateHasChanged](xref:blazor/lifecycle#state-changes).</span></span> <span data-ttu-id="77289-116">例外狀況會在發生時記錄。</span><span class="sxs-lookup"><span data-stu-id="77289-116">Exceptions are logged when they occur.</span></span>

<span data-ttu-id="77289-117">在下列範例中， `UpdateHeading` 當選取按鈕時，會以非同步方式呼叫：</span><span class="sxs-lookup"><span data-stu-id="77289-117">In the following example, `UpdateHeading` is called asynchronously when the button is selected:</span></span>

```razor
<button class="btn btn-primary" @onclick="UpdateHeading">
    Update heading
</button>

@code {
    private async Task UpdateHeading(MouseEventArgs e)
    {
        ...
    }
}
```

## <a name="event-argument-types"></a><span data-ttu-id="77289-118">事件引數類型</span><span class="sxs-lookup"><span data-stu-id="77289-118">Event argument types</span></span>

<span data-ttu-id="77289-119">對於某些事件，則允許事件引數類型。</span><span class="sxs-lookup"><span data-stu-id="77289-119">For some events, event argument types are permitted.</span></span> <span data-ttu-id="77289-120">只有在方法中使用事件種類時，才需要在方法呼叫中指定事件種類。</span><span class="sxs-lookup"><span data-stu-id="77289-120">Specifying an event type in the method call is only necessary if the event type is used in the method.</span></span>

<span data-ttu-id="77289-121"><xref:System.EventArgs>下表顯示支援的。</span><span class="sxs-lookup"><span data-stu-id="77289-121">Supported <xref:System.EventArgs> are shown in the following table.</span></span>

| <span data-ttu-id="77289-122">事件</span><span class="sxs-lookup"><span data-stu-id="77289-122">Event</span></span>            | <span data-ttu-id="77289-123">類別</span><span class="sxs-lookup"><span data-stu-id="77289-123">Class</span></span>                | <span data-ttu-id="77289-124">DOM 事件和注意事項</span><span class="sxs-lookup"><span data-stu-id="77289-124">DOM events and notes</span></span> |
| ---
<span data-ttu-id="77289-125">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-125">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-126">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-126">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-127">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-127">'Blazor'</span></span>
- <span data-ttu-id="77289-128">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-128">'Identity'</span></span>
- <span data-ttu-id="77289-129">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-129">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-130">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-130">'Razor'</span></span>
- <span data-ttu-id="77289-131">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-131">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-132">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-132">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-133">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-133">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-134">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-134">'Blazor'</span></span>
- <span data-ttu-id="77289-135">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-135">'Identity'</span></span>
- <span data-ttu-id="77289-136">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-136">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-137">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-137">'Razor'</span></span>
- <span data-ttu-id="77289-138">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-138">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-139">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-139">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-140">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-140">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-141">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-141">'Blazor'</span></span>
- <span data-ttu-id="77289-142">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-142">'Identity'</span></span>
- <span data-ttu-id="77289-143">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-143">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-144">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-144">'Razor'</span></span>
- <span data-ttu-id="77289-145">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-145">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-146">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-146">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-147">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-147">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-148">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-148">'Blazor'</span></span>
- <span data-ttu-id="77289-149">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-149">'Identity'</span></span>
- <span data-ttu-id="77289-150">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-150">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-151">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-151">'Razor'</span></span>
- <span data-ttu-id="77289-152">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-152">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-153">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-153">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-154">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-154">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-155">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-155">'Blazor'</span></span>
- <span data-ttu-id="77289-156">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-156">'Identity'</span></span>
- <span data-ttu-id="77289-157">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-157">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-158">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-158">'Razor'</span></span>
- <span data-ttu-id="77289-159">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-159">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-160">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-160">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-161">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-161">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-162">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-162">'Blazor'</span></span>
- <span data-ttu-id="77289-163">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-163">'Identity'</span></span>
- <span data-ttu-id="77289-164">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-164">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-165">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-165">'Razor'</span></span>
- <span data-ttu-id="77289-166">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-166">'SignalR' uid:</span></span> 

<span data-ttu-id="77289-167">-------- |---標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-167">-------- | --- title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-168">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-168">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-169">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-169">'Blazor'</span></span>
- <span data-ttu-id="77289-170">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-170">'Identity'</span></span>
- <span data-ttu-id="77289-171">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-171">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-172">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-172">'Razor'</span></span>
- <span data-ttu-id="77289-173">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-173">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-174">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-174">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-175">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-175">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-176">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-176">'Blazor'</span></span>
- <span data-ttu-id="77289-177">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-177">'Identity'</span></span>
- <span data-ttu-id="77289-178">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-178">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-179">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-179">'Razor'</span></span>
- <span data-ttu-id="77289-180">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-180">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-181">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-181">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-182">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-182">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-183">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-183">'Blazor'</span></span>
- <span data-ttu-id="77289-184">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-184">'Identity'</span></span>
- <span data-ttu-id="77289-185">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-185">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-186">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-186">'Razor'</span></span>
- <span data-ttu-id="77289-187">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-187">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-188">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-188">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-189">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-189">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-190">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-190">'Blazor'</span></span>
- <span data-ttu-id="77289-191">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-191">'Identity'</span></span>
- <span data-ttu-id="77289-192">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-192">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-193">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-193">'Razor'</span></span>
- <span data-ttu-id="77289-194">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-194">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-195">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-195">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-196">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-196">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-197">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-197">'Blazor'</span></span>
- <span data-ttu-id="77289-198">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-198">'Identity'</span></span>
- <span data-ttu-id="77289-199">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-199">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-200">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-200">'Razor'</span></span>
- <span data-ttu-id="77289-201">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-201">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-202">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-202">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-203">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-203">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-204">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-204">'Blazor'</span></span>
- <span data-ttu-id="77289-205">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-205">'Identity'</span></span>
- <span data-ttu-id="77289-206">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-206">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-207">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-207">'Razor'</span></span>
- <span data-ttu-id="77289-208">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-208">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-209">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-209">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-210">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-210">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-211">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-211">'Blazor'</span></span>
- <span data-ttu-id="77289-212">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-212">'Identity'</span></span>
- <span data-ttu-id="77289-213">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-213">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-214">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-214">'Razor'</span></span>
- <span data-ttu-id="77289-215">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-215">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-216">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-216">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-217">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-217">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-218">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-218">'Blazor'</span></span>
- <span data-ttu-id="77289-219">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-219">'Identity'</span></span>
- <span data-ttu-id="77289-220">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-220">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-221">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-221">'Razor'</span></span>
- <span data-ttu-id="77289-222">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-222">'SignalR' uid:</span></span> 

<span data-ttu-id="77289-223">---------- |---標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-223">---------- | --- title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-224">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-224">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-225">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-225">'Blazor'</span></span>
- <span data-ttu-id="77289-226">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-226">'Identity'</span></span>
- <span data-ttu-id="77289-227">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-227">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-228">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-228">'Razor'</span></span>
- <span data-ttu-id="77289-229">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-229">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-230">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-230">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-231">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-231">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-232">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-232">'Blazor'</span></span>
- <span data-ttu-id="77289-233">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-233">'Identity'</span></span>
- <span data-ttu-id="77289-234">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-234">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-235">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-235">'Razor'</span></span>
- <span data-ttu-id="77289-236">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-236">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-237">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-237">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-238">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-238">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-239">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-239">'Blazor'</span></span>
- <span data-ttu-id="77289-240">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-240">'Identity'</span></span>
- <span data-ttu-id="77289-241">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-241">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-242">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-242">'Razor'</span></span>
- <span data-ttu-id="77289-243">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-243">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-244">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-244">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-245">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-245">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-246">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-246">'Blazor'</span></span>
- <span data-ttu-id="77289-247">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-247">'Identity'</span></span>
- <span data-ttu-id="77289-248">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-248">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-249">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-249">'Razor'</span></span>
- <span data-ttu-id="77289-250">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-250">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-251">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-251">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-252">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-252">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-253">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-253">'Blazor'</span></span>
- <span data-ttu-id="77289-254">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-254">'Identity'</span></span>
- <span data-ttu-id="77289-255">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-255">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-256">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-256">'Razor'</span></span>
- <span data-ttu-id="77289-257">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-257">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-258">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-258">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-259">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-259">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-260">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-260">'Blazor'</span></span>
- <span data-ttu-id="77289-261">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-261">'Identity'</span></span>
- <span data-ttu-id="77289-262">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-262">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-263">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-263">'Razor'</span></span>
- <span data-ttu-id="77289-264">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-264">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-265">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-265">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-266">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-266">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-267">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-267">'Blazor'</span></span>
- <span data-ttu-id="77289-268">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-268">'Identity'</span></span>
- <span data-ttu-id="77289-269">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-269">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-270">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-270">'Razor'</span></span>
- <span data-ttu-id="77289-271">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-271">'SignalR' uid:</span></span> 

-
<span data-ttu-id="77289-272">標題： ' ASP.NET Core Blazor 事件處理 ' 作者：描述： ' 瞭解 Blazor 的事件處理功能，包括事件引數類型、事件回呼和管理預設瀏覽器事件。 '</span><span class="sxs-lookup"><span data-stu-id="77289-272">title: 'ASP.NET Core Blazor event handling' author: description: 'Learn about Blazor's event handling features, including event argument types, event callbacks, and managing default browser events.'</span></span>
<span data-ttu-id="77289-273">monikerRange： ms-chap： ms. custom： ms. date： no-loc：</span><span class="sxs-lookup"><span data-stu-id="77289-273">monikerRange: ms.author: ms.custom: ms.date: no-loc:</span></span>
- <span data-ttu-id="77289-274">'Blazor'</span><span class="sxs-lookup"><span data-stu-id="77289-274">'Blazor'</span></span>
- <span data-ttu-id="77289-275">'Identity'</span><span class="sxs-lookup"><span data-stu-id="77289-275">'Identity'</span></span>
- <span data-ttu-id="77289-276">'Let's Encrypt'</span><span class="sxs-lookup"><span data-stu-id="77289-276">'Let's Encrypt'</span></span>
- <span data-ttu-id="77289-277">'Razor'</span><span class="sxs-lookup"><span data-stu-id="77289-277">'Razor'</span></span>
- <span data-ttu-id="77289-278">' SignalR ' uid：</span><span class="sxs-lookup"><span data-stu-id="77289-278">'SignalR' uid:</span></span> 

<span data-ttu-id="77289-279">---------- | |剪貼簿 |<xref:Microsoft.AspNetCore.Components.Web.ClipboardEventArgs> | `oncut`, `oncopy`, `onpaste` | |拖曳 |<xref:Microsoft.AspNetCore.Components.Web.DragEventArgs> | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`,`ondragend`</span><span class="sxs-lookup"><span data-stu-id="77289-279">---------- | | Clipboard        | <xref:Microsoft.AspNetCore.Components.Web.ClipboardEventArgs> | `oncut`, `oncopy`, `onpaste` | | Drag             | <xref:Microsoft.AspNetCore.Components.Web.DragEventArgs> | `ondrag`, `ondragstart`, `ondragenter`, `ondragleave`, `ondragover`, `ondrop`, `ondragend`</span></span><br><br><span data-ttu-id="77289-280"><xref:Microsoft.AspNetCore.Components.Web.DataTransfer>並 <xref:Microsoft.AspNetCore.Components.Web.DataTransferItem> 按住拖曳的專案資料。</span><span class="sxs-lookup"><span data-stu-id="77289-280"><xref:Microsoft.AspNetCore.Components.Web.DataTransfer> and <xref:Microsoft.AspNetCore.Components.Web.DataTransferItem> hold dragged item data.</span></span> <span data-ttu-id="77289-281">| |錯誤 |<xref:Microsoft.AspNetCore.Components.Web.ErrorEventArgs> | `onerror`| |事件 |<xref:System.EventArgs>  |  *一般*</span><span class="sxs-lookup"><span data-stu-id="77289-281">| | Error            | <xref:Microsoft.AspNetCore.Components.Web.ErrorEventArgs> | `onerror` | | Event            | <xref:System.EventArgs> | *General*</span></span><br><span data-ttu-id="77289-282">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span><span class="sxs-lookup"><span data-stu-id="77289-282">`onactivate`, `onbeforeactivate`, `onbeforedeactivate`, `ondeactivate`, `onended`, `onfullscreenchange`, `onfullscreenerror`, `onloadeddata`, `onloadedmetadata`, `onpointerlockchange`, `onpointerlockerror`, `onreadystatechange`, `onscroll`</span></span><br><br><span data-ttu-id="77289-283">*剪貼簿*</span><span class="sxs-lookup"><span data-stu-id="77289-283">*Clipboard*</span></span><br><span data-ttu-id="77289-284">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span><span class="sxs-lookup"><span data-stu-id="77289-284">`onbeforecut`, `onbeforecopy`, `onbeforepaste`</span></span><br><br><span data-ttu-id="77289-285">*輸入*</span><span class="sxs-lookup"><span data-stu-id="77289-285">*Input*</span></span><br><span data-ttu-id="77289-286">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit></span><span class="sxs-lookup"><span data-stu-id="77289-286">`oninvalid`, `onreset`, `onselect`, `onselectionchange`, `onselectstart`, <xref:Microsoft.AspNetCore.Components.Forms.EditForm.OnSubmit></span></span><br><br><span data-ttu-id="77289-287">*媒體*</span><span class="sxs-lookup"><span data-stu-id="77289-287">*Media*</span></span><br><span data-ttu-id="77289-288">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span><span class="sxs-lookup"><span data-stu-id="77289-288">`oncanplay`, `oncanplaythrough`, `oncuechange`, `ondurationchange`, `onemptied`, `onpause`, `onplay`, `onplaying`, `onratechange`, `onseeked`, `onseeking`, `onstalled`, `onstop`, `onsuspend`, `ontimeupdate`, `onvolumechange`, `onwaiting`</span></span><br><br><span data-ttu-id="77289-289"><xref:Microsoft.AspNetCore.Components.Web.EventHandlers>保存屬性，以設定事件名稱和事件引數類型之間的對應。</span><span class="sxs-lookup"><span data-stu-id="77289-289"><xref:Microsoft.AspNetCore.Components.Web.EventHandlers> holds attributes to configure the mappings between event names and event argument types.</span></span> <span data-ttu-id="77289-290">| |焦點 |<xref:Microsoft.AspNetCore.Components.Web.FocusEventArgs> | `onfocus`, `onblur`, `onfocusin`,`onfocusout`</span><span class="sxs-lookup"><span data-stu-id="77289-290">| | Focus            | <xref:Microsoft.AspNetCore.Components.Web.FocusEventArgs> | `onfocus`, `onblur`, `onfocusin`, `onfocusout`</span></span><br><br><span data-ttu-id="77289-291">不包含的支援 `relatedTarget` 。</span><span class="sxs-lookup"><span data-stu-id="77289-291">Doesn't include support for `relatedTarget`.</span></span> <span data-ttu-id="77289-292">| |輸入 |<xref:Microsoft.AspNetCore.Components.ChangeEventArgs> | `onchange`, `oninput` | |鍵盤 |<xref:Microsoft.AspNetCore.Components.Web.KeyboardEventArgs> | `onkeydown`, `onkeypress`, `onkeyup` | |滑鼠 |<xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` | |滑鼠指標 |<xref:Microsoft.AspNetCore.Components.Web.PointerEventArgs> | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` | |滑鼠滾輪 |<xref:Microsoft.AspNetCore.Components.Web.WheelEventArgs> | `onwheel`, `onmousewheel` | |進度 |<xref:Microsoft.AspNetCore.Components.Web.ProgressEventArgs> | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` | |觸控 |<xref:Microsoft.AspNetCore.Components.Web.TouchEventArgs> | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`,`ontouchcancel`</span><span class="sxs-lookup"><span data-stu-id="77289-292">| | Input            | <xref:Microsoft.AspNetCore.Components.ChangeEventArgs> | `onchange`, `oninput` | | Keyboard         | <xref:Microsoft.AspNetCore.Components.Web.KeyboardEventArgs> | `onkeydown`, `onkeypress`, `onkeyup` | | Mouse            | <xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> | `onclick`, `oncontextmenu`, `ondblclick`, `onmousedown`, `onmouseup`, `onmouseover`, `onmousemove`, `onmouseout` | | Mouse pointer    | <xref:Microsoft.AspNetCore.Components.Web.PointerEventArgs> | `onpointerdown`, `onpointerup`, `onpointercancel`, `onpointermove`, `onpointerover`, `onpointerout`, `onpointerenter`, `onpointerleave`, `ongotpointercapture`, `onlostpointercapture` | | Mouse wheel      | <xref:Microsoft.AspNetCore.Components.Web.WheelEventArgs> | `onwheel`, `onmousewheel` | | Progress         | <xref:Microsoft.AspNetCore.Components.Web.ProgressEventArgs> | `onabort`, `onload`, `onloadend`, `onloadstart`, `onprogress`, `ontimeout` | | Touch            | <xref:Microsoft.AspNetCore.Components.Web.TouchEventArgs> | `ontouchstart`, `ontouchend`, `ontouchmove`, `ontouchenter`, `ontouchleave`, `ontouchcancel`</span></span><br><br><span data-ttu-id="77289-293"><xref:Microsoft.AspNetCore.Components.Web.TouchPoint>代表觸控裝置上的單一連絡人點。</span><span class="sxs-lookup"><span data-stu-id="77289-293"><xref:Microsoft.AspNetCore.Components.Web.TouchPoint> represents a single contact point on a touch-sensitive device.</span></span> |

<span data-ttu-id="77289-294">如需詳細資訊，請參閱下列資源：</span><span class="sxs-lookup"><span data-stu-id="77289-294">For more information, see the following resources:</span></span>

* <span data-ttu-id="77289-295">[ASP.NET Core 參考來源中的 EventArgs 類別（dotnet/aspnetcore release/3.1 分支）](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web)。</span><span class="sxs-lookup"><span data-stu-id="77289-295">[EventArgs classes in the ASP.NET Core reference source (dotnet/aspnetcore release/3.1 branch)](https://github.com/dotnet/aspnetcore/tree/release/3.1/src/Components/Web/src/Web).</span></span>
* <span data-ttu-id="77289-296">[MDN web 檔： GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers)：包含哪些 HTML 元素支援每個 DOM 事件的資訊。</span><span class="sxs-lookup"><span data-stu-id="77289-296">[MDN web docs: GlobalEventHandlers](https://developer.mozilla.org/docs/Web/API/GlobalEventHandlers): Includes information on which HTML elements support each DOM event.</span></span>

## <a name="lambda-expressions"></a><span data-ttu-id="77289-297">Lambda 運算式</span><span class="sxs-lookup"><span data-stu-id="77289-297">Lambda expressions</span></span>

<span data-ttu-id="77289-298">您也可以使用[Lambda 運算式](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions)：</span><span class="sxs-lookup"><span data-stu-id="77289-298">[Lambda expressions](/dotnet/csharp/programming-guide/statements-expressions-operators/lambda-expressions) can also be used:</span></span>

```razor
<button @onclick="@(e => Console.WriteLine("Hello, world!"))">Say hello</button>
```

<span data-ttu-id="77289-299">關閉其他值（例如反覆運算一組專案時）通常會很方便。</span><span class="sxs-lookup"><span data-stu-id="77289-299">It's often convenient to close over additional values, such as when iterating over a set of elements.</span></span> <span data-ttu-id="77289-300">下列範例會建立三個按鈕， `UpdateHeading` 在 UI 中選取時，每個都會呼叫傳遞事件引數（ <xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs> ）和其按鈕編號（ `buttonNumber` ）：</span><span class="sxs-lookup"><span data-stu-id="77289-300">The following example creates three buttons, each of which calls `UpdateHeading` passing an event argument (<xref:Microsoft.AspNetCore.Components.Web.MouseEventArgs>) and its button number (`buttonNumber`) when selected in the UI:</span></span>

```razor
<h2>@message</h2>

@for (var i = 1; i < 4; i++)
{
    var buttonNumber = i;

    <button class="btn btn-primary"
            @onclick="@(e => UpdateHeading(e, buttonNumber))">
        Button #@i
    </button>
}

@code {
    private string message = "Select a button to learn its position.";

    private void UpdateHeading(MouseEventArgs e, int buttonNumber)
    {
        message = $"You selected Button #{buttonNumber} at " +
            $"mouse position: {e.ClientX} X {e.ClientY}.";
    }
}
```

> [!NOTE]
> <span data-ttu-id="77289-301">請**不要在** `i` `for` lambda 運算式中直接使用迴圈中的迴圈變數（）。</span><span class="sxs-lookup"><span data-stu-id="77289-301">Do **not** use the loop variable (`i`) in a `for` loop directly in a lambda expression.</span></span> <span data-ttu-id="77289-302">否則，所有 lambda 運算式都會使用相同的變數，使 `i` 的值在所有 lambda 中都相同。</span><span class="sxs-lookup"><span data-stu-id="77289-302">Otherwise the same variable is used by all lambda expressions causing `i`'s value to be the same in all lambdas.</span></span> <span data-ttu-id="77289-303">請一律在本機變數中捕捉其值（ `buttonNumber` 在上述範例中為），然後使用它。</span><span class="sxs-lookup"><span data-stu-id="77289-303">Always capture its value in a local variable (`buttonNumber` in the preceding example) and then use it.</span></span>

## <a name="eventcallback"></a><span data-ttu-id="77289-304">App eventcallback</span><span class="sxs-lookup"><span data-stu-id="77289-304">EventCallback</span></span>

<span data-ttu-id="77289-305">有一個常見的嵌套元件案例，就是當子元件事件發生時，想要執行父元件的方法。</span><span class="sxs-lookup"><span data-stu-id="77289-305">A common scenario with nested components is the desire to run a parent component's method when a child component event occurs.</span></span> <span data-ttu-id="77289-306">`onclick`子元件中發生的事件是常見的使用案例。</span><span class="sxs-lookup"><span data-stu-id="77289-306">An `onclick` event occurring in the child component is a common use case.</span></span> <span data-ttu-id="77289-307">若要在元件之間公開事件，請使用 <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="77289-307">To expose events across components, use an <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span> <span data-ttu-id="77289-308">父元件可以將回呼方法指派給子元件的 <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="77289-308">A parent component can assign a callback method to a child component's <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span>

<span data-ttu-id="77289-309">`ChildComponent`範例應用程式（*Components/ChildComponent*）中的會示範如何設定按鈕的 `onclick` 處理常式，以接收 <xref:Microsoft.AspNetCore.Components.EventCallback> 來自範例的委派 `ParentComponent` 。</span><span class="sxs-lookup"><span data-stu-id="77289-309">The `ChildComponent` in the sample app (*Components/ChildComponent.razor*) demonstrates how a button's `onclick` handler is set up to receive an <xref:Microsoft.AspNetCore.Components.EventCallback> delegate from the sample's `ParentComponent`.</span></span> <span data-ttu-id="77289-310"><xref:Microsoft.AspNetCore.Components.EventCallback>會使用輸入 `MouseEventArgs` ，這適用于 `onclick` 來自週邊裝置的事件：</span><span class="sxs-lookup"><span data-stu-id="77289-310">The <xref:Microsoft.AspNetCore.Components.EventCallback> is typed with `MouseEventArgs`, which is appropriate for an `onclick` event from a peripheral device:</span></span>

[!code-razor[](common/samples/3.x/BlazorWebAssemblySample/Components/ChildComponent.razor?highlight=5-7,17-18)]

<span data-ttu-id="77289-311">會 `ParentComponent` 將子系的 <xref:Microsoft.AspNetCore.Components.EventCallback%601> （ `OnClickCallback` ）設定為其 `ShowMessage` 方法。</span><span class="sxs-lookup"><span data-stu-id="77289-311">The `ParentComponent` sets the child's <xref:Microsoft.AspNetCore.Components.EventCallback%601> (`OnClickCallback`) to its `ShowMessage` method.</span></span>

<span data-ttu-id="77289-312">*Pages/ParentComponent. razor*：</span><span class="sxs-lookup"><span data-stu-id="77289-312">*Pages/ParentComponent.razor*:</span></span>

```razor
@page "/ParentComponent"

<h1>Parent-child example</h1>

<ChildComponent Title="Panel Title from Parent"
                OnClickCallback="@ShowMessage">
    Content of the child component is supplied
    by the parent component.
</ChildComponent>

<p><b>@messageText</b></p>

@code {
    private string messageText;

    private void ShowMessage(MouseEventArgs e)
    {
        messageText = $"Blaze a new trail with Blazor! ({e.ScreenX}, {e.ScreenY})";
    }
}
```

<span data-ttu-id="77289-313">當您在中選取按鈕時 `ChildComponent` ：</span><span class="sxs-lookup"><span data-stu-id="77289-313">When the button is selected in the `ChildComponent`:</span></span>

* <span data-ttu-id="77289-314">`ParentComponent` `ShowMessage` 會呼叫的方法。</span><span class="sxs-lookup"><span data-stu-id="77289-314">The `ParentComponent`'s `ShowMessage` method is called.</span></span> <span data-ttu-id="77289-315">`messageText`會更新並顯示在中 `ParentComponent` 。</span><span class="sxs-lookup"><span data-stu-id="77289-315">`messageText` is updated and displayed in the `ParentComponent`.</span></span>
* <span data-ttu-id="77289-316">回呼的方法（）中不需要呼叫[StateHasChanged](xref:blazor/lifecycle#state-changes) `ShowMessage` 。</span><span class="sxs-lookup"><span data-stu-id="77289-316">A call to [StateHasChanged](xref:blazor/lifecycle#state-changes) isn't required in the callback's method (`ShowMessage`).</span></span> <span data-ttu-id="77289-317"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A>會自動呼叫來 rerender `ParentComponent` ，就像子事件會觸發元件 rerendering 在子系內執行的事件處理常式一樣。</span><span class="sxs-lookup"><span data-stu-id="77289-317"><xref:Microsoft.AspNetCore.Components.ComponentBase.StateHasChanged%2A> is called automatically to rerender the `ParentComponent`, just as child events trigger component rerendering in event handlers that execute within the child.</span></span>

<span data-ttu-id="77289-318"><xref:Microsoft.AspNetCore.Components.EventCallback>和 <xref:Microsoft.AspNetCore.Components.EventCallback%601> 允許非同步委派。</span><span class="sxs-lookup"><span data-stu-id="77289-318"><xref:Microsoft.AspNetCore.Components.EventCallback> and <xref:Microsoft.AspNetCore.Components.EventCallback%601> permit asynchronous delegates.</span></span> <span data-ttu-id="77289-319"><xref:Microsoft.AspNetCore.Components.EventCallback%601>是強型別，而且需要特定的引數類型。</span><span class="sxs-lookup"><span data-stu-id="77289-319"><xref:Microsoft.AspNetCore.Components.EventCallback%601> is strongly typed and requires a specific argument type.</span></span> <span data-ttu-id="77289-320"><xref:Microsoft.AspNetCore.Components.EventCallback>是弱型別，並允許任何引數類型。</span><span class="sxs-lookup"><span data-stu-id="77289-320"><xref:Microsoft.AspNetCore.Components.EventCallback> is weakly typed and allows any argument type.</span></span>

```razor
<ChildComponent 
    OnClickCallback="@(async () => { await Task.Yield(); messageText = "Blaze It!"; })" />
```

<span data-ttu-id="77289-321">使用叫 <xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 用或 <xref:Microsoft.AspNetCore.Components.EventCallback.InvokeAsync%2A> ，並等待 <xref:System.Threading.Tasks.Task> ：</span><span class="sxs-lookup"><span data-stu-id="77289-321">Invoke an <xref:Microsoft.AspNetCore.Components.EventCallback> or <xref:Microsoft.AspNetCore.Components.EventCallback%601> with <xref:Microsoft.AspNetCore.Components.EventCallback.InvokeAsync%2A> and await the <xref:System.Threading.Tasks.Task>:</span></span>

```csharp
await callback.InvokeAsync(arg);
```

<span data-ttu-id="77289-322"><xref:Microsoft.AspNetCore.Components.EventCallback> <xref:Microsoft.AspNetCore.Components.EventCallback%601> 針對事件處理和系結元件參數使用和。</span><span class="sxs-lookup"><span data-stu-id="77289-322">Use <xref:Microsoft.AspNetCore.Components.EventCallback> and <xref:Microsoft.AspNetCore.Components.EventCallback%601> for event handling and binding component parameters.</span></span>

<span data-ttu-id="77289-323">慣用強型別 <xref:Microsoft.AspNetCore.Components.EventCallback%601> <xref:Microsoft.AspNetCore.Components.EventCallback> 。</span><span class="sxs-lookup"><span data-stu-id="77289-323">Prefer the strongly typed <xref:Microsoft.AspNetCore.Components.EventCallback%601> over <xref:Microsoft.AspNetCore.Components.EventCallback>.</span></span> <span data-ttu-id="77289-324"><xref:Microsoft.AspNetCore.Components.EventCallback%601>為元件的使用者提供更好的錯誤意見反應。</span><span class="sxs-lookup"><span data-stu-id="77289-324"><xref:Microsoft.AspNetCore.Components.EventCallback%601> provides better error feedback to users of the component.</span></span> <span data-ttu-id="77289-325">與其他 UI 事件處理常式類似，指定事件參數是選擇性的。</span><span class="sxs-lookup"><span data-stu-id="77289-325">Similar to other UI event handlers, specifying the event parameter is optional.</span></span> <span data-ttu-id="77289-326"><xref:Microsoft.AspNetCore.Components.EventCallback>當沒有任何值傳遞給回呼時，請使用。</span><span class="sxs-lookup"><span data-stu-id="77289-326">Use <xref:Microsoft.AspNetCore.Components.EventCallback> when there's no value passed to the callback.</span></span>

## <a name="prevent-default-actions"></a><span data-ttu-id="77289-327">防止預設動作</span><span class="sxs-lookup"><span data-stu-id="77289-327">Prevent default actions</span></span>

<span data-ttu-id="77289-328">使用指示詞 [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) 屬性可防止事件的預設動作。</span><span class="sxs-lookup"><span data-stu-id="77289-328">Use the [`@on{EVENT}:preventDefault`](xref:mvc/views/razor#oneventpreventdefault) directive attribute to prevent the default action for an event.</span></span>

<span data-ttu-id="77289-329">在輸入裝置上選取索引鍵，且元素焦點位於文字方塊上時，瀏覽器通常會在文字方塊中顯示索引鍵的字元。</span><span class="sxs-lookup"><span data-stu-id="77289-329">When a key is selected on an input device and the element focus is on a text box, a browser normally displays the key's character in the text box.</span></span> <span data-ttu-id="77289-330">在下列範例中，會藉由指定指示詞屬性來防止預設行為 `@onkeypress:preventDefault` 。</span><span class="sxs-lookup"><span data-stu-id="77289-330">In the following example, the default behavior is prevented by specifying the `@onkeypress:preventDefault` directive attribute.</span></span> <span data-ttu-id="77289-331">計數器會遞增，而且索引 **+** 鍵不會被捕捉到 `<input>` 元素的值中：</span><span class="sxs-lookup"><span data-stu-id="77289-331">The counter increments, and the **+** key isn't captured into the `<input>` element's value:</span></span>

```razor
<input value="@count" @onkeypress="KeyHandler" @onkeypress:preventDefault />

@code {
    private int count = 0;

    private void KeyHandler(KeyboardEventArgs e)
    {
        if (e.Key == "+")
        {
            count++;
        }
    }
}
```

<span data-ttu-id="77289-332">指定 `@on{EVENT}:preventDefault` 不含值的屬性，相當於 `@on{EVENT}:preventDefault="true"` 。</span><span class="sxs-lookup"><span data-stu-id="77289-332">Specifying the `@on{EVENT}:preventDefault` attribute without a value is equivalent to `@on{EVENT}:preventDefault="true"`.</span></span>

<span data-ttu-id="77289-333">屬性的值也可以是運算式。</span><span class="sxs-lookup"><span data-stu-id="77289-333">The value of the attribute can also be an expression.</span></span> <span data-ttu-id="77289-334">在下列範例中， `shouldPreventDefault` 是 `bool` 設定為或的欄位 `true` `false` ：</span><span class="sxs-lookup"><span data-stu-id="77289-334">In the following example, `shouldPreventDefault` is a `bool` field set to either `true` or `false`:</span></span>

```razor
<input @onkeypress:preventDefault="shouldPreventDefault" />
```

<span data-ttu-id="77289-335">不需要事件處理常式來防止預設動作。</span><span class="sxs-lookup"><span data-stu-id="77289-335">An event handler isn't required to prevent the default action.</span></span> <span data-ttu-id="77289-336">事件處理常式和防止預設動作案例可以獨立使用。</span><span class="sxs-lookup"><span data-stu-id="77289-336">The event handler and prevent default action scenarios can be used independently.</span></span>

## <a name="stop-event-propagation"></a><span data-ttu-id="77289-337">停止事件傳播</span><span class="sxs-lookup"><span data-stu-id="77289-337">Stop event propagation</span></span>

<span data-ttu-id="77289-338">使用指示詞 [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) 屬性來停止事件傳播。</span><span class="sxs-lookup"><span data-stu-id="77289-338">Use the [`@on{EVENT}:stopPropagation`](xref:mvc/views/razor#oneventstoppropagation) directive attribute to stop event propagation.</span></span>

<span data-ttu-id="77289-339">在下列範例中，選取核取方塊可防止從第二個子系的 click 事件 `<div>` 傳播到父系 `<div>` ：</span><span class="sxs-lookup"><span data-stu-id="77289-339">In the following example, selecting the check box prevents click events from the second child `<div>` from propagating to the parent `<div>`:</span></span>

```razor
<label>
    <input @bind="stopPropagation" type="checkbox" />
    Stop Propagation
</label>

<div @onclick="OnSelectParentDiv">
    <h3>Parent div</h3>

    <div @onclick="OnSelectChildDiv">
        Child div that doesn't stop propagation when selected.
    </div>

    <div @onclick="OnSelectChildDiv" @onclick:stopPropagation="stopPropagation">
        Child div that stops propagation when selected.
    </div>
</div>

@code {
    private bool stopPropagation = false;

    private void OnSelectParentDiv() => 
        Console.WriteLine($"The parent div was selected. {DateTime.Now}");
    private void OnSelectChildDiv() => 
        Console.WriteLine($"A child div was selected. {DateTime.Now}");
}
```
