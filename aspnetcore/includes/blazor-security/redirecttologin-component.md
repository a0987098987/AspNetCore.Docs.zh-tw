<span data-ttu-id="e9a9f-101">`RedirectToLogin`元件（ `Shared/RedirectToLogin.razor` ）：</span><span class="sxs-lookup"><span data-stu-id="e9a9f-101">The `RedirectToLogin` component (`Shared/RedirectToLogin.razor`):</span></span>

* <span data-ttu-id="e9a9f-102">管理將未經授權的使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="e9a9f-102">Manages redirecting unauthorized users to the login page.</span></span>
* <span data-ttu-id="e9a9f-103">會保留使用者嘗試存取的目前 URL，以便在驗證成功時，將其傳回至該頁面。</span><span class="sxs-lookup"><span data-stu-id="e9a9f-103">Preserves the current URL that the user is attempting to access so that they can be returned to that page if authentication is successful.</span></span>

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
