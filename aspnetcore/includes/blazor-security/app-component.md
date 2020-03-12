`App` 元件（*razor*）類似于在 Blazor 伺服器應用程式中找到的 `App` 元件：

* `CascadingAuthenticationState` 元件會管理將 `AuthenticationState` 公開給應用程式的其餘部分。
* `AuthorizeRouteView` 元件可確保目前的使用者已獲授權存取特定頁面，或轉譯 `RedirectToLogin` 元件。
* `RedirectToLogin` 元件會管理將未經授權的使用者重新導向至登入頁面。

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
