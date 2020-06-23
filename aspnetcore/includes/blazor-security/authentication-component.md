<span data-ttu-id="3e7d1-101">元件（）所產生的頁面會 `Authentication` `Pages/Authentication.razor` 定義處理不同驗證階段所需的路由。</span><span class="sxs-lookup"><span data-stu-id="3e7d1-101">The page produced by the `Authentication` component (`Pages/Authentication.razor`) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="3e7d1-102"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>元件：</span><span class="sxs-lookup"><span data-stu-id="3e7d1-102">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> component:</span></span>

* <span data-ttu-id="3e7d1-103">由 [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) 封裝提供。</span><span class="sxs-lookup"><span data-stu-id="3e7d1-103">Is provided by the [`Microsoft.AspNetCore.Components.WebAssembly.Authentication`](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package.</span></span>
* <span data-ttu-id="3e7d1-104">管理在每個驗證階段執行適當的動作。</span><span class="sxs-lookup"><span data-stu-id="3e7d1-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
