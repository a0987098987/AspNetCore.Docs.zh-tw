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
# <a name="aspnet-core-21-mvc-samesite-cookie-sample"></a><span data-ttu-id="877a1-103">ASP.NET Core 2.1 MVC SameSite cookie 範例</span><span class="sxs-lookup"><span data-stu-id="877a1-103">ASP.NET Core 2.1 MVC SameSite cookie sample</span></span>

<span data-ttu-id="877a1-104">ASP.NET Core 2.1 具有[SameSite](https://www.owasp.org/index.php/SameSite)屬性的內建支援，但它已寫入原始標準。</span><span class="sxs-lookup"><span data-stu-id="877a1-104">ASP.NET Core 2.1 has built-in support for the [SameSite](https://www.owasp.org/index.php/SameSite) attribute, but it was written to the original standard.</span></span> <span data-ttu-id="877a1-105">已[修補的行為](https://github.com/dotnet/aspnetcore/issues/8212)已變更 `SameSite.None` 的意義，以 `None`的值發出 sameSite 屬性，而不是完全發出值。</span><span class="sxs-lookup"><span data-stu-id="877a1-105">The [patched behavior](https://github.com/dotnet/aspnetcore/issues/8212) changed the meaning of `SameSite.None` to emit the sameSite attribute with a value of `None`, rather than not emit the value at all.</span></span> <span data-ttu-id="877a1-106">如果您不想要發出值，可以將 cookie 上的 `SameSite` 屬性設定為-1。</span><span class="sxs-lookup"><span data-stu-id="877a1-106">If you want to not emit the value you can set the `SameSite` property on a cookie to -1.</span></span>

## <a name="sampleCode"></a><span data-ttu-id="877a1-107">撰寫 SameSite 屬性</span><span class="sxs-lookup"><span data-stu-id="877a1-107">Writing the SameSite attribute</span></span>

<span data-ttu-id="877a1-108">以下是如何在 cookie 上撰寫 SameSite 屬性的範例：</span><span class="sxs-lookup"><span data-stu-id="877a1-108">Following is an example of how to write a SameSite attribute on a cookie:</span></span>

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

## <a name="setting-cookie-authentication-and-session-state-cookies"></a><span data-ttu-id="877a1-109">設定 Cookie 驗證和會話狀態 cookie</span><span class="sxs-lookup"><span data-stu-id="877a1-109">Setting Cookie Authentication and Session State cookies</span></span>

<span data-ttu-id="877a1-110">Cookie 驗證、會話狀態和[各種其他元件](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)都會透過 cookie 選項設定其 sameSite 選項，例如</span><span class="sxs-lookup"><span data-stu-id="877a1-110">Cookie authentication, session state and [various other components](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1) set their sameSite options via Cookie options, for example</span></span>

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

<span data-ttu-id="877a1-111">在上述程式碼中，cookie 驗證和會話狀態都會將其 sameSite 屬性設定為 `None`、發出具有 `None` 值的屬性，以及將 Secure 屬性設定為 true。</span><span class="sxs-lookup"><span data-stu-id="877a1-111">In the preceding code, both cookie authentication and session state set their sameSite attribute to `None`, emitting the attribute with a `None` value, and also set the Secure attribute to true.</span></span>

### <a name="run-the-sample"></a><span data-ttu-id="877a1-112">執行範例</span><span class="sxs-lookup"><span data-stu-id="877a1-112">Run the sample</span></span>

<span data-ttu-id="877a1-113">如果您執行[範例專案](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)，請在初始頁面上載入瀏覽器偵錯工具，並用它來查看網站的 cookie 集合。</span><span class="sxs-lookup"><span data-stu-id="877a1-113">If you run the [sample project](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC), load your browser debugger on the initial page and use it to view the cookie collection for the site.</span></span> <span data-ttu-id="877a1-114">若要在 Edge 和 Chrome 中執行此操作，請按 `F12` 然後選取 [`Application`] 索引標籤，然後按一下 [`Storage`] 區段中 [`Cookies`] 選項底下的網站 URL。</span><span class="sxs-lookup"><span data-stu-id="877a1-114">To do so in Edge and Chrome press `F12` then select the `Application` tab and click the site URL under the `Cookies` option in the `Storage` section.</span></span>

![瀏覽器偵錯工具 Cookie 清單](BrowserDebugger.png)

<span data-ttu-id="877a1-116">當您按一下 [建立 SameSite Cookie] 按鈕的 SameSite 屬性值為 `Lax`，且符合[範例程式碼](#sampleCode)中所設定的值時，您可以從上圖中看到範例所建立的 cookie。</span><span class="sxs-lookup"><span data-stu-id="877a1-116">You can see from the image above that the cookie created by the sample when you click the "Create SameSite Cookie" button has a SameSite attribute value of `Lax`, matching the value set in the [sample code](#sampleCode).</span></span>

## <a name="interception"></a><span data-ttu-id="877a1-117">攔截 cookie</span><span class="sxs-lookup"><span data-stu-id="877a1-117">Intercepting cookies</span></span>

<span data-ttu-id="877a1-118">為了攔截 cookie，若要根據使用者的瀏覽器代理程式中的支援來調整 none 值，您必須使用 `CookiePolicy` 中介軟體。</span><span class="sxs-lookup"><span data-stu-id="877a1-118">In order to intercept cookies, to adjust the none value according to its support in the user's browser agent you must use the `CookiePolicy` middleware.</span></span> <span data-ttu-id="877a1-119">這必須放入 HTTP 要求管線中，**才能**在 `ConfigureServices()`中撰寫 cookie 和設定的任何元件之前。</span><span class="sxs-lookup"><span data-stu-id="877a1-119">This must be placed into the http request pipeline **before** any components that write cookies and configured within `ConfigureServices()`.</span></span>

<span data-ttu-id="877a1-120">若要將其插入管線中，請使用[Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs)中 `Configure(IApplicationBuilder, IHostingEnvironment)` 方法的 `app.UseCookiePolicy()`。</span><span class="sxs-lookup"><span data-stu-id="877a1-120">To insert it into the pipeline use `app.UseCookiePolicy()` in the `Configure(IApplicationBuilder, IHostingEnvironment)` method in [Startup.cs](https://github.com/blowdart/AspNetSameSiteSamples/blob/master/AspNetCore21MVC/Startup.cs).</span></span> <span data-ttu-id="877a1-121">例如：</span><span class="sxs-lookup"><span data-stu-id="877a1-121">For example:</span></span>

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

<span data-ttu-id="877a1-122">然後在 `ConfigureServices(IServiceCollection services)` 設定 cookie 原則，以在附加或刪除 cookie 時呼叫 helper 類別。</span><span class="sxs-lookup"><span data-stu-id="877a1-122">Then in the `ConfigureServices(IServiceCollection services)` configure the cookie policy to call out to a helper class when cookies are appended or deleted.</span></span> <span data-ttu-id="877a1-123">例如：</span><span class="sxs-lookup"><span data-stu-id="877a1-123">For example:</span></span>

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

<span data-ttu-id="877a1-124">Helper 函數 `CheckSameSite(HttpContext, CookieOptions)`：</span><span class="sxs-lookup"><span data-stu-id="877a1-124">The helper function `CheckSameSite(HttpContext, CookieOptions)`:</span></span>

* <span data-ttu-id="877a1-125">當 cookie 附加至要求或從要求中刪除時，會呼叫。</span><span class="sxs-lookup"><span data-stu-id="877a1-125">Is called when cookies are appended to the request or deleted from the request.</span></span>
* <span data-ttu-id="877a1-126">檢查 [`SameSite`] 屬性是否設定為 [`None`]。</span><span class="sxs-lookup"><span data-stu-id="877a1-126">Checks to see if the `SameSite` property is set to `None`.</span></span>
* <span data-ttu-id="877a1-127">如果 `SameSite` 設定為 `None` 而且目前的使用者代理程式已知不支援 none 屬性值。</span><span class="sxs-lookup"><span data-stu-id="877a1-127">If `SameSite` is set to `None` and the current user agent is known to not support the none attribute value.</span></span> <span data-ttu-id="877a1-128">檢查是使用[SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs)類別來完成：</span><span class="sxs-lookup"><span data-stu-id="877a1-128">The check is done using the [SameSiteSupport](https://github.com/dotnet/AspNetCore.Docs/tree/master/aspnetcore/security/samesite/sample/snippets/SameSiteSupport.cs) class:</span></span>
  * <span data-ttu-id="877a1-129">將屬性設定為，將 `SameSite` 設定為不發出值 `(SameSiteMode)(-1)`</span><span class="sxs-lookup"><span data-stu-id="877a1-129">Sets `SameSite` to not emit the value by setting the property to `(SameSiteMode)(-1)`</span></span>

## <a name="targeting-net-framework"></a><span data-ttu-id="877a1-130">目標 .NET Framework</span><span class="sxs-lookup"><span data-stu-id="877a1-130">Targeting .NET Framework</span></span>

<span data-ttu-id="877a1-131">ASP.NET Core 和 System.web （ASP.NET 傳統）具有 SameSite 的獨立實現。</span><span class="sxs-lookup"><span data-stu-id="877a1-131">ASP.NET Core and System.Web (ASP.NET Classic) have independent implementations of SameSite.</span></span> <span data-ttu-id="877a1-132">如果使用 ASP.NET Core，也不需要 .NET Framework 的 SameSite KB 修補程式，適用于 ASP.NET Core 的 SameSite 最低架構版本需求（.NET 4.7.2）。</span><span class="sxs-lookup"><span data-stu-id="877a1-132">The SameSite KB patches for .NET Framework are not required if using ASP.NET Core nor does the System.Web SameSite minimum framework version requirement (.NET 4.7.2) apply to ASP.NET Core.</span></span>

<span data-ttu-id="877a1-133">在 .NET 上 ASP.NET Core 需要更新 nuget 套件相依性，才能取得適當的修正程式。</span><span class="sxs-lookup"><span data-stu-id="877a1-133">ASP.NET Core on .NET requires updating nuget package dependencies to get the appropriate fixes.</span></span>

<span data-ttu-id="877a1-134">若要取得 .NET Framework 的 ASP.NET Core 變更，請確定您已修補套件和版本（2.1.14 或更新版本2.1 版）的直接參考。</span><span class="sxs-lookup"><span data-stu-id="877a1-134">To get the ASP.NET Core changes for .NET Framework ensure that you have a direct reference to the patched packages and versions (2.1.14 or later 2.1 versions).</span></span>

```xml
<PackageReference Include="Microsoft.Net.Http.Headers" Version="2.1.14" />
<PackageReference Include="Microsoft.AspNetCore.CookiePolicy" Version="2.1.14" />
```

### <a name="more-information"></a><span data-ttu-id="877a1-135">相關資訊</span><span class="sxs-lookup"><span data-stu-id="877a1-135">More Information</span></span>
 
<span data-ttu-id="877a1-136">[Chrome 更新](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite 檔](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite 變更公告](https://github.com/dotnet/aspnetcore/issues/8212)</span><span class="sxs-lookup"><span data-stu-id="877a1-136">[Chrome Updates](https://www.chromium.org/updates/same-site)
[ASP.NET Core SameSite Documentation](https://docs.microsoft.com/aspnet/core/security/samesite?view=aspnetcore-2.1)
[ASP.NET Core 2.1 SameSite Change Announcement](https://github.com/dotnet/aspnetcore/issues/8212)</span></span>