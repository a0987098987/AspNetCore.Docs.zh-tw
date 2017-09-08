---
title: "防止 ASP.NET Core 應用程式中開啟的重新導向攻擊 |Microsoft 文件"
author: ardalis
description: "示範如何防止對 ASP.NET Core 應用程式的開啟重新導向攻擊"
keywords: "ASP.NET Core，安全性，開啟重新導向攻擊"
ms.author: riande
manager: wpickett
ms.date: 07/7/2017
ms.topic: article
ms.assetid: 4604e563-e91a-4ecd-b7ed-00b3f1eee2b5
ms.technology: aspnet
ms.prod: asp.net-core
uid: security/preventing-open-redirects
ms.openlocfilehash: 7a62b08d641de5a9df5c2f7d89bf6bf97ed8e39d
ms.sourcegitcommit: 0b6c8e6d81d2b3c161cd375036eecbace46a9707
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/11/2017
---
# <a name="preventing-open-redirect-attacks-in-an-aspnet-core-app"></a>防止 ASP.NET Core 應用程式中開啟的重新導向攻擊

重新導向至指定的要求，例如查詢字串或表單資料透過 URL 的 web 應用程式將使用者重新導向至外部、 惡意 URL 可能遭竄改。 這種竄改稱為開啟重新導向攻擊。

每當應用程式邏輯重新導向至指定的 URL，您必須確認重新導向 URL，沒有遭到竄改。 ASP.NET Core 具有內建的功能，可協助保護應用程式，從開啟的重新導向 （也就是開啟的重新導向） 攻擊。

## <a name="what-is-an-open-redirect-attack"></a>什麼是開啟的重新導向攻擊？

Web 應用程式經常將使用者重新導向至登入頁面存取需要驗證的資源時。 包含重新導向 typlically `returnUrl` querystring 參數，以便使用者可以傳回至原本要求的 URL 之後使用者成功登入。 使用者驗證之後，它們會重新導向至其原先要求的 URL。

因為要求的 querystring 中指定的目的地 URL，惡意的使用者無法修改查詢字串。 遭竄改的查詢字串可能會允許將使用者重新導向至外部，惡意的站台的站台。 這項技術稱為開啟重新導向 （或重新導向） 攻擊。

### <a name="an-example-attack"></a>範例攻擊

惡意使用者可以開發目的是讓使用者的認證或在您的應用程式上的機密資訊的惡意使用者存取的攻擊。 若要開始攻擊，它們說服使用者連結至網站的登入頁面上，按一下與`returnUrl`加入 URL querystring 值。 例如， [NerdDinner.com](http://nerddinner.com)範例應用程式 （寫入 ASP.NET mvc） 包含這類登入頁面。 這裡： ``http://nerddinner.com/Account/LogOn?returnUrl=/Home/About``。 攻擊，然後遵循下列步驟：

1. 使用者按下連結``http://nerddinner.com/Account/LogOn?returnUrl=http://nerddiner.com/Account/LogOn``(請注意，第二個 URL 是 nerddi**n**呃，不 nerddi**nn**er)。
2. 使用者成功登入。
3. 使用者重新導向 （由站台） 至``http://nerddiner.com/Account/LogOn``（看起來像真正的站台的惡意網站）。
4. 使用者再次登入 （提供惡意網站他們的認證） 會重新導向至實際站台。

使用者將可能認為其第一次嘗試登入失敗，且其第二個是成功。 將最有可能仍不知道他們的認證遭洩漏。

![開啟的重新導向攻擊程序](preventing-open-redirects/_static/open-redirection-attack-process.png)

除了登入頁面中，有些網站會提供重新導向頁面或端點。 假設您的應用程式已開啟的重新導向的分頁``/Home/Redirect``。 攻擊者無法建立，比方說，連結會移至以電子郵件``[yoursite]/Home/Redirect?url=http://phishingsite.com/Home/Login``。 一般使用者會查看 URL，並請參閱您的網站名稱開頭。 信任的會讓他們按一下連結。 開啟的重新導向至網路釣魚網站，看起來與您相同，然後就會傳送使用者和使用者便可能它們所認為的登入是您的網站。

## <a name="protecting-against-open-redirect-attacks"></a>保護開啟的重新導向攻擊

在開發 web 應用程式時，所有使用者提供資料都視為不受信任。 如果您的應用程式已重新導向 URL 的內容會根據使用者的功能，請確定的這類的重新導向只進行本機應用程式中 （或已知的 URL，可能需要提供於 querystring 中不含任何 URL）。

### <a name="localredirect"></a>LocalRedirect

使用``LocalRedirect``helper 方法的基底`Controller`類別：

```
public IActionResult SomeAction(string redirectUrl)
{
    return LocalRedirect(redirectUrl);
}
```

``LocalRedirect``如果指定非本機 URL，則會擲回例外狀況。 否則，它的行為就像是``Redirect``方法。

### <a name="islocalurl"></a>IsLocalUrl

使用[IsLocalUrl](https://docs.microsoft.com/aspnet/core/api/microsoft.aspnetcore.mvc.iurlhelper#Microsoft_AspNetCore_Mvc_IUrlHelper_IsLocalUrl_System_String_)方法來測試之前將重新導向的 Url:

下列範例會示範如何檢查 URL 是否本機，然後才將重新導向。

```
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

`IsLocalUrl`方法可防止使用者不小心重新導向至惡意網站。 您可以記錄的情況下，您必須是本機 URL 提供給非本機 URL 時所提供的 URL 的詳細資料。 記錄重新導向 Url 可能有助於診斷重新導向攻擊。
