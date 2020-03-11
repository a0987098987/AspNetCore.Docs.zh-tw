---
title: 防止 ASP.NET Core 中的開啟重新導向攻擊
author: ardalis
description: 示範如何防止對 ASP.NET Core 應用程式的開啟重新導向攻擊
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 9d8cac8708fe9aeadba5af1287362a20df7f6bfe
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78660521"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>防止 ASP.NET Core 中的開啟重新導向攻擊

重新導向至透過要求（例如 querystring 或表單資料）所指定 URL 的 web 應用程式可能會遭到篡改，以將使用者重新導向至外部的惡意 URL。 這種篡改稱為開放式重新導向攻擊。

每當您的應用程式邏輯重新導向至指定的 URL 時，您都必須確認重新導向 URL 未遭到篡改。 ASP.NET Core 具有內建功能，可協助保護應用程式不受開啟的重新導向（也稱為開放式重新導向）攻擊。

## <a name="what-is-an-open-redirect-attack"></a>什麼是開啟的重新導向攻擊？

Web 應用程式通常會在使用者存取需要驗證的資源時，將他們重新導向至登入頁面。 重新導向通常會包含 `returnUrl` querystring 參數，讓使用者可以在成功登入之後，傳回給原始要求的 URL。 使用者驗證之後，系統會將他們重新導向至原先要求的 URL。

因為在要求的 querystring 中指定了目的地 URL，所以惡意使用者可能會篡改 querystring。 遭篡改的 querystring 可能會允許網站將使用者重新導向至外部的惡意網站。 這項技術稱為「開啟重新導向」（或「重新導向」）攻擊。

### <a name="an-example-attack"></a>範例攻擊

惡意使用者可能會開發攻擊，以允許惡意使用者存取使用者的認證或機密資訊。 若要開始攻擊，惡意使用者說服使用者按一下您網站登入頁面的連結，並將 `returnUrl` querystring 值新增至 URL。 例如，請考慮在 `contoso.com` 的應用程式，其中包含 `http://contoso.com/Account/LogOn?returnUrl=/Home/About`的登入頁面。 攻擊會遵循下列步驟：

1. 使用者按一下 `http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn` 的惡意連結（第二個 URL 是 "contoso**1**.com"，而不是 "contoso.com"）。
2. 使用者登入成功。
3. 使用者會被重新導向（由網站）以 `http://contoso1.com/Account/LogOn` （看起來與實際網站完全類似的惡意網站）。
4. 使用者再次登入（提供惡意網站的認證），然後重新導向至實際網站。

使用者可能認為第一次嘗試登入失敗，而第二次嘗試成功。 使用者很可能會不知道他們的認證會受到危害。

![開啟重新導向攻擊程式](preventing-open-redirects/_static/open-redirection-attack-process.png)

除了登入頁面以外，有些網站會提供重新導向頁面或端點。 假設您的應用程式有一個頁面，其中含有開啟的重新導向，`/Home/Redirect`。 比方說，攻擊者可以建立電子郵件中的連結，`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`。 一般使用者會查看 URL，並從您的網站名稱開始查看。 信任它，他們會按一下連結。 然後，開啟的重新導向會將使用者傳送至網路釣魚網站，看起來與您的內容相同，而且使用者可能會登入他們認為您的網站。

## <a name="protecting-against-open-redirect-attacks"></a>防止開啟重新導向攻擊

開發 web 應用程式時，將所有使用者提供的資料視為不受信任。 如果您的應用程式具有可根據 URL 內容重新導向使用者的功能，請確定這類重新導向只會在您的應用程式中本機完成（或至已知的 URL，而不是查詢字串中可能提供的任何 URL）。

### <a name="localredirect"></a>LocalRedirect

使用基底 `Controller` 類別中的 `LocalRedirect` helper 方法：

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

如果指定非本機 URL，`LocalRedirect` 將會擲回例外狀況。 否則，它的行為就如同 `Redirect` 方法。

### <a name="islocalurl"></a>IsLocalUrl

重新導向之前，請使用[IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper.islocalurl#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)方法來測試 url：

下列範例顯示如何在重新導向之前檢查 URL 是否為本機。

```csharp
private IActionResult RedirectToLocal(string returnUrl)
{
    if (Url.IsLocalUrl(returnUrl))
    {
        return Redirect(returnUrl);
    }
    else
    {
        return RedirectToAction(nameof(HomeController.Index), "Home");
    }
}
```

`IsLocalUrl` 方法可保護使用者不小心被意外重新導向至惡意網站。 當您需要本機 URL 的情況下提供非本機 URL 時，您可以記錄所提供的 URL 詳細資料。 記錄重新導向 Url 可能有助於診斷重新導向攻擊。
