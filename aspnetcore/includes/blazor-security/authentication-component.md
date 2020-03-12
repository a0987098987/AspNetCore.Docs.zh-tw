`Authentication` 元件（*Pages/Authentication. razor*）所產生的頁面會定義處理不同驗證階段所需的路由。

`RemoteAuthenticatorView` 元件：

* 由 `Microsoft.AspNetCore.Components.WebAssembly.Authentication` 封裝所提供。
* 管理在每個驗證階段執行適當的動作。

```razor
@page "/authentication/{action}"
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication

<RemoteAuthenticatorView Action="@Action" />

@code {
    [Parameter]
    public string Action { get; set; }
}
```
