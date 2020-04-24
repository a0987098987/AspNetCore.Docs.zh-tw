`Authentication`元件（*Pages/Authentication. razor*）所產生的頁面會定義處理不同驗證階段所需的路由。

`RemoteAuthenticatorView`元件：

* 是由 AspNetCore 所提供。 [WebAssembly. 驗證](https://www.nuget.org/packages/Microsoft.AspNetCore.Components.WebAssembly.Authentication/)套件。
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
