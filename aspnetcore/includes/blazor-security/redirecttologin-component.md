`RedirectToLogin`元件（*Shared/RedirectToLogin. razor*）：

* 管理將未經授權的使用者重新導向至登入頁面。
* 會保留使用者嘗試存取的目前 URL，以便在驗證成功時，將其傳回至該頁面。

```razor
@inject NavigationManager Navigation
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@code {
    protected override void OnInitialized()
    {
        Navigation.NavigateTo($"authentication/login?returnUrl=" +
            Uri.EscapeDataString(Navigation.Uri));
    }
}
```
