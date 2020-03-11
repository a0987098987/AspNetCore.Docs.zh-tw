---
title: ASP.NET Core 2.1 MVC SameSite cookie 範例
author: rick-anderson
description: ASP.NET Core 2.1 MVC SameSite cookie 範例
monikerRange: '>= aspnetcore-2.1 < aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
uid: security/samesite/mvc21
ms.openlocfilehash: 14f3d3d27d5740519e1ba57529d5694b4cdeb9ec
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78667745"
---
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a>ASP.NET Core 2.1 MVC SameSite cookie 範例

ASP.NET Core 2.1 具有[SameSite](https://www.owasp.org/index.php/SameSite)屬性的內建支援，但它已寫入原始標準。 已[修補的行為](https://github.com/dotnet/aspnetcore/issues/8212)已變更 `SameSite.None` 的意義，以 `None`的值發出 sameSite 屬性，而不是完全發出值。 如果您不想要發出值，可以將 cookie 上的 `SameSite` 屬性設定為-1。

## <a name="sampleCode"></a>撰寫 SameSite 屬性

以下是如何在 cookie 上撰寫 SameSite 屬性的範例：

```c#
var cookieOptions = new CookieOptions
{
    // Set the secure flag, which Chrome's changes will require for SameSite none.
    // Note this will also require you to be running on HTTPS
    Secure = true,

    // Set the cookie to HTTP only which is good practice unless you really do need
    // to access it client side in scripts.
    HttpOnly = true,

    // Add the SameSite attribute, this will emit the attribute with a value of none.
    // To not emit the attribute at all set the SameSite property to (SameSiteMode)(-1).
    SameSite = SameSiteMode.None
};

// Add the cookie to the response cookie collection
Response.Cookies.Append(CookieName, "cookieValue", cookieOptions);
```

## <a name="setting-cookie-authentication-and-session-state-cookies"></a>設定 Cookie 驗證和會話狀態 cookie

Cookie 驗證、會話狀態和[各種其他元件](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)都會透過 cookie 選項設定其 sameSite 選項，例如

```c#
services.AddAuthentication(CookieAuthenticationDefaults.AuthenticationScheme)
    .AddCookie(options =>
    {
        options.Cookie.SameSite = SameSiteMode.None;
        options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
        options.Cookie.IsEssential = true;
    });

services.AddSession(options =>
{
    options.Cookie.SameSite = SameSiteMode.None;
    options.Cookie.SecurePolicy = CookieSecurePolicy.Always;
    options.Cookie.IsEssential = true;
});
```

在上述程式碼中，cookie 驗證和會話狀態都會將其 sameSite 屬性設定為 `None`、發出具有 `None` 值的屬性，以及將 Secure 屬性設定為 true。

### <a name="run-the-sample"></a>執行範例

如果您執行[範例專案](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)，請在初始頁面上載入瀏覽器偵錯工具，並用它來查看網站的 cookie 集合。 若要在 Edge 和 Chrome 中執行此操作，請按 `F12` 然後選取 [`Application`] 索引標籤，然後按一下 [`Storage`] 區段中 [`Cookies`] 選項底下的網站 URL。

![瀏覽器偵錯工具 Cookie 清單](BrowserDebugger.png)

當您按一下 [建立 SameSite Cookie] 按鈕的 SameSite 屬性值為 `Lax`，且符合[範例程式碼](#sampleCode)中所設定的值時，您可以從上圖中看到範例所建立的 cookie。

## <a name="interception"></a>攔截 cookie

為了攔截 cookie，若要根據使用者的瀏覽器代理程式中的支援來調整 none 值，您必須使用 `CookiePolicy` 中介軟體。 這必須放入 HTTP 要求管線中，**才能**在 `ConfigureServices()`中撰寫 cookie 和設定的任何元件之前。

若要將其插入管線中，請使用[Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs)中 `Configure(IApplicationBuilder, IHostingEnvironment)` 方法的 `app.UseCookiePolicy()`。 例如：

```c#
public void Configure(IApplicationBuilder app, IHostingEnvironment env)
{
    if (env.IsDevelopment())
    {
       app.UseDeveloperExceptionPage();
    }
    else
    {
        app.UseExceptionHandler("/Home/Error");
        app.UseHsts();
    }

    app.UseHttpsRedirection();
    app.UseStaticFiles();
    app.UseCookiePolicy();
    app.UseAuthentication();
    app.UseSession();

    app.UseMvc(routes =>
    {
        routes.MapRoute(
            name: "default",
            template: "{controller=Home}/{action=Index}/{id?}");
    });
}
```

然後在 `ConfigureServices(IServiceCollection services)` 設定 cookie 原則，以在附加或刪除 cookie 時呼叫 helper 類別。 例如：

```c#
public void ConfigureServices(IServiceCollection services)
{
    services.Configure<CookiePolicyOptions>(options =>
    {
        options.CheckConsentNeeded = context => true;
        options.MinimumSameSitePolicy = SameSiteMode.None;
        options.OnAppendCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
        options.OnDeleteCookie = cookieContext =>
            CheckSameSite(cookieContext.Context, cookieContext.CookieOptions);
    });
}

private void CheckSameSite(HttpContext httpContext, CookieOptions options)
{
    if (options.SameSite == SameSiteMode.None)
    {
        var userAgent = httpContext.Request.Headers["User-Agent"].ToString();
        if (SameSite.BrowserDetection.DisallowsSameSiteNone(userAgent))
        {
            options.SameSite = (SameSiteMode)(-1);
        }
    }
}
```

Helper 函數 `CheckSameSite(HttpContext, CookieOptions)`：

* 當 cookie 附加至要求或從要求中刪除時，會呼叫。
* 檢查 [`SameSite`] 屬性是否設定為 [`None`]。
* 如果 `SameSite` 設定為 `None` 而且目前的使用者代理程式已知不支援 none 屬性值。 檢查是使用[SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs)類別來完成：
  * 將屬性設定為，將 `SameSite` 設定為不發出值 `(SameSiteMode)(-1)`

## <a name="targeting-net-framework"></a>目標 .NET Framework

ASP.NET Core 和 System.web （ASP.NET 傳統）具有 SameSite 的獨立實現。 如果使用 ASP.NET Core，也不需要 .NET Framework 的 SameSite KB 修補程式，適用于 ASP.NET Core 的 SameSite 最低架構版本需求（.NET 4.7.2）。

在 .NET 上 ASP.NET Core 需要更新 nuget 套件相依性，才能取得適當的修正程式。

若要取得 .NET Framework 的 ASP.NET Core 變更，請確定您已修補套件和版本（2.1.14 或更新版本2.1 版）的直接參考。

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a>相關資訊
 
[Chrome 更新](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite 檔](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite 變更公告](https://github.com/dotnet/aspnetcore/issues/8212)