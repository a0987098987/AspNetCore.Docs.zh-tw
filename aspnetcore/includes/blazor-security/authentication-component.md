<span data-ttu-id="925cd-101">`Authentication`元件（*Pages/Authentication. razor*）所產生的頁面會定義處理不同驗證階段所需的路由。</span><span class="sxs-lookup"><span data-stu-id="925cd-101">The page produced by the `Authentication` component (*Pages/Authentication.razor*) defines the routes required for handling different authentication stages.</span></span>

<span data-ttu-id="925cd-102"><xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView>元件：</span><span class="sxs-lookup"><span data-stu-id="925cd-102">The <xref:Microsoft.AspNetCore.Components.WebAssembly.Authentication.RemoteAuthenticatorView> component:</span></span>

* <span data-ttu-id="925cd-103">是由 AspNetCore 所提供。 [WebAssembly. 驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)套件。</span><span class="sxs-lookup"><span data-stu-id="925cd-103">Is provided by the [Microsoft.AspNetCore.Components.WebAssembly.Authentication](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/) package.</span></span>
* <span data-ttu-id="925cd-104">管理在每個驗證階段執行適當的動作。</span><span class="sxs-lookup"><span data-stu-id="925cd-104">Manages performing the appropriate actions at each stage of authentication.</span></span>

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
