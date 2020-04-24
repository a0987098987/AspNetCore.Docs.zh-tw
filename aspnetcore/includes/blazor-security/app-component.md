元件（razor）類似于 Blazor 伺服器應用程式中`App`找到的元件：* * `App`

* `CascadingAuthenticationState`元件會管理將公開`AuthenticationState`至應用程式的其餘部分。
* 此`AuthorizeRouteView`元件可確保目前的使用者已獲授權存取給定的頁面，或以其他方式`RedirectToLogin`呈現元件。
* `RedirectToLogin`元件會管理將未經授權的使用者重新導向至登入頁面。

```razor
<CascadingAuthenticationState>
    <Router AppAssembly="@typeof(Program).Assembly">
        <Found Context="routeData">
            <AuthorizeRouteView RouteData="@routeData" 
                DefaultLayout="@typeof(MainLayout)">
                <NotAuthorized>
                    @if (!context.User.Identity.IsAuthenticated)
                    {
                        <RedirectToLogin />
                    }
                    else
                    {
                        <p>
                            You are not authorized to access 
                            this resource.
                        </p>
                    }
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
