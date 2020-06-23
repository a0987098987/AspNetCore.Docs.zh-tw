<span data-ttu-id="ede93-101">`LoginDisplay`元件（ `Shared/LoginDisplay.razor` ）會在 `MainLayout` 元件（）中轉譯 `Shared/MainLayout.razor` ，並管理下列行為：</span><span class="sxs-lookup"><span data-stu-id="ede93-101">The `LoginDisplay` component (`Shared/LoginDisplay.razor`) is rendered in the `MainLayout` component (`Shared/MainLayout.razor`) and manages the following behaviors:</span></span>

* <span data-ttu-id="ede93-102">針對已驗證的使用者：</span><span class="sxs-lookup"><span data-stu-id="ede93-102">For authenticated users:</span></span>
  * <span data-ttu-id="ede93-103">顯示目前的使用者名稱。</span><span class="sxs-lookup"><span data-stu-id="ede93-103">Displays the current username.</span></span>
  * <span data-ttu-id="ede93-104">提供用來登出應用程式的按鈕。</span><span class="sxs-lookup"><span data-stu-id="ede93-104">Offers a button to log out of the app.</span></span>
* <span data-ttu-id="ede93-105">若為匿名使用者，則提供登入的選項。</span><span class="sxs-lookup"><span data-stu-id="ede93-105">For anonymous users, offers the option to log in.</span></span>

```razor
@using Microsoft.AspNetCore.Components.Authorization
@using Microsoft.AspNetCore.Components.WebAssembly.Authentication
@inject NavigationManager Navigation
@inject SignOutSessionStateManager SignOutManager

<AuthorizeView>
    <Authorized>
        Hello, @context.User.Identity.Name!
        <button class="nav-link btn btn-link" @onclick="BeginLogout">
            Log out
        </button>
    </Authorized>
    <NotAuthorized>
        <a href="authentication/login">Log in</a>
    </NotAuthorized>
</AuthorizeView>

@code {
    private async Task BeginLogout(MouseEventArgs args)
    {
        await SignOutManager.SetSignOutState();
        Navigation.NavigateTo("authentication/logout");
    }
}
```
