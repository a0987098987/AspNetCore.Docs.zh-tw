<span data-ttu-id="84a59-101">`App` 元件（*razor*）類似于在 Blazor 伺服器應用程式中找到的 `App` 元件：</span><span class="sxs-lookup"><span data-stu-id="84a59-101">The `App` component (*App.razor*) is similar to the `App` component found in Blazor Server apps:</span></span>

* <span data-ttu-id="84a59-102">`CascadingAuthenticationState` 元件會管理將 `AuthenticationState` 公開給應用程式的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="84a59-102">The `CascadingAuthenticationState` component manages exposing the `AuthenticationState` to the rest of the app.</span></span>
* <span data-ttu-id="84a59-103">`AuthorizeRouteView` 元件可確保目前的使用者已獲授權存取特定頁面，或轉譯 `RedirectToLogin` 元件。</span><span class="sxs-lookup"><span data-stu-id="84a59-103">The `AuthorizeRouteView` component makes sure that the current user is authorized to access a given page or otherwise renders the `RedirectToLogin` component.</span></span>
* <span data-ttu-id="84a59-104">`RedirectToLogin` 元件會管理將未經授權的使用者重新導向至登入頁面。</span><span class="sxs-lookup"><span data-stu-id="84a59-104">The `RedirectToLogin` component manages redirecting unauthorized users to the login page.</span></span>

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    <RedirectToLogin />
                </NotAuthorized>
            </AuthorizeRouteView>
        </Found>
        <NotFound>
            <LayoutView Layout="@typeof(MainLayout)">
                <p>Sorry, there's nothing at this address.</p>
            </LayoutView>
        </NotFound>
    </Router>
</CascadingAuthenticationState>
```
