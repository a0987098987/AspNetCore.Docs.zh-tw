<span data-ttu-id="cbfe3-101">`Authentication` 元件（*Pages/Authentication. razor*）所產生的頁面會定義處理不同驗證階段所需的路由。</span><span class="sxs-lookup"><span data-stu-id="cbfe3-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="cbfe3-102">`RemoteAuthenticatorView` 元件：</span><span class="sxs-lookup"><span data-stu-id="cbfe3-102">The `RemoteAuthenticatorView` component:</span></span>

* <span data-ttu-id="cbfe3-103">由 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 封裝所提供。</span><span class="sxs-lookup"><span data-stu-id="cbfe3-103">Is provided by the `Microsoft.AspNetCore.Components.WebAssembly.Authentication` package.</span></span>
* <span data-ttu-id="cbfe3-104">管理在每個驗證階段執行適當的動作。</span><span class="sxs-lookup"><span data-stu-id="cbfe3-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
