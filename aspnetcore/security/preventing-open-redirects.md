---
title: 防止 ASP.NET Core 中的開啟重新導向攻擊
author: ardalis
description: 示範如何防止開啟重新導向攻擊將 ASP.NET Core 應用程式
ms.author: riande
ms.date: 07/07/2017
uid: security/preventing-open-redirects
ms.openlocfilehash: 0896189d2caaccb19647eb7c6d57f29dfc0290dd
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64891615"
---
# <a name="prevent-open-redirect-attacks-in-aspnet-core"></a>防止 ASP.NET Core 中的開啟重新導向攻擊

重新導向至指定透過例如查詢字串或表單資料要求 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。 這種竄改稱為開啟重新導向攻擊。

每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL 未遭竄改。 ASP.NET Core 具有內建的功能，以協助防止開啟重新導向 （也就是開啟重新導向） 攻擊的應用程式。

## <a name="what-is-an-open-redirect-attack"></a>什麼是開啟重新導向攻擊？

Web 應用程式經常將使用者重新導向至登入頁面存取需要驗證的資源時。 重新導向通常包含`returnUrl`querystring 參數，讓使用者可以傳回給原始要求的 URL 之後使用者已成功登入。 使用者驗證之後，它們會被重新導向至其原先要求的 URL。

因為要求的 querystring 中指定的目的地 URL，則惡意使用者無法修改查詢字串。 遭竄改的查詢字串可能會允許將使用者重新導向至外部，惡意網站的站台。 這項技術稱為開啟重新導向 （或重新導向） 攻擊。

### <a name="an-example-attack"></a>範例攻擊

惡意使用者可以開發目的是讓惡意使用者的存取權的使用者認證或機密資訊的攻擊。 若要開始的攻擊，惡意使用者說服使用者按一下與您的網站登入頁面的連結`returnUrl`新增至 URL 的查詢字串值。 例如，請考慮在應用程式`contoso.com`，其中包含登入頁面`http://contoso.com/Account/LogOn?returnUrl=/Home/About`。 攻擊會遵循下列步驟：

1. 使用者按下惡意連結`http://contoso.com/Account/LogOn?returnUrl=http://contoso1.com/Account/LogOn`(第二個 URL 是"contoso**1**.com"，不是"contoso.com")。
2. 使用者成功登入。
3. 將使用者重新導向 （由站台） `http://contoso1.com/Account/LogOn` （看起來像真正的站台完全惡意網站）。
4. 在使用者登入一次 （讓惡意網站他們的認證） 並被重新導向到真網站。

使用者可能會認為他們第一次嘗試登入失敗，且其第二次嘗試成功。 使用者很可能並不會知道他們的認證遭到入侵。

![開啟重新導向攻擊程序](preventing-open-redirects/_static/open-redirection-attack-process.png)

除了登入頁面中，有些網站會提供重新導向頁面或端點。 假設您的應用程式有開啟的重新導向的頁面`/Home/Redirect`。 攻擊者建立，比方說，連至電子郵件中的連結`[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login`。 一般使用者將看看 URL，並看看它開始與您的網站名稱。 信任的會讓他們按一下連結。 開啟重新導向至網路釣魚網站，看起來與您相同，然後會傳送使用者和使用者可能登入他們認為是您的網站。

## <a name="protecting-against-open-redirect-attacks"></a>防止開啟重新導向攻擊

在開發 web 應用程式時，將使用者提供的所有資料視為不受信任。 如果您的應用程式 URL 的內容為基礎的使用者重新導向的功能，請確定，這類的重新導向是只在本機完成應用程式內 （或已知的 URL，不含任何可能會在查詢字串中提供的 URL）。

### <a name="localredirect"></a>LocalRedirect

使用`LocalRedirect`自基底的 helper 方法`Controller`類別：

```csharp
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

`LocalRedirect` 如果指定非本機 URL，則會擲回例外狀況。 否則，它的行為就像`Redirect`方法。

### <a name="islocalurl"></a>IsLocalUrl

使用[IsLocalUrl](/dotnet/api/Microsoft.AspNetCore.Mvc.IUrlHelper?view=aspnetcore-2.0#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)之前，先測試 Url 重新導向的方法：

下列範例示範如何檢查 URL 是否本機，再重新導向。

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

`IsLocalUrl`方法可防止使用者被不小心重新導向至惡意網站。 您可以記錄的情況下，您必須是本機的 URL 提供給非本機 URL 時所提供的 URL 的詳細資料。 記錄重新導向 Url 可能有助於診斷重新導向攻擊。
