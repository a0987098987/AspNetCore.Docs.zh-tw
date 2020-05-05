---
title: 在 ASP.NET Core 中使用 SameSite cookie
author: rick-anderson
description: 瞭解如何在 ASP.NET Core 中使用 SameSite cookie
ms.author: riande
ms.custom: mvc
ms.date: 12/03/2019
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
- Electron
uid: security/samesite
ms.openlocfilehash: 43d5a3dbc5e202688e006355e0b105a86d721460
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775098"
---
# <a name="work-with-samesite-cookies-in-aspnet-core"></a>在 ASP.NET Core 中使用 SameSite cookie

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

SameSite 是一種[IETF](https://ietf.org/about/)草稿標準，旨在針對跨網站偽造要求（CSRF）攻擊提供一些保護。 初稿標準已于[2016](https://tools.ietf.org/html/draft-west-first-party-cookies-07)開始繪製，已于[2019](https://tools.ietf.org/html/draft-west-cookie-incrementalism-00)更新。 更新的標準不會與前一個標準回溯相容，下列是最顯著的差異：

* 預設會將不含 SameSite 標`SameSite=Lax`頭的 cookie 視為。
* `SameSite=None`必須用來允許跨網站 cookie 的使用。
* 判斷提示的`SameSite=None` cookie 也必須標記為`Secure`。
* 使用[`<iframe>`](https://developer.mozilla.org/docs/Web/HTML/Element/iframe)的應用程式可能會遇到`sameSite=Lax`或`sameSite=Strict` cookie 的`<iframe>`問題，因為會被視為跨網站案例。
* [2016 標準](https://tools.ietf.org/html/draft-west-first-party-cookies-07)不允許此值`SameSite=Strict` `SameSite=None` ，而會導致某些實作為將這類 cookie 視為。 請參閱本檔中的[支援舊版瀏覽器](#sob)。

此`SameSite=Lax`設定適用于大部分的應用程式 cookie。 某些形式的驗證，例如[OpenID connect](https://openid.net/connect/) （OIDC）和[WS-同盟](https://auth0.com/docs/protocols/ws-fed)，預設為以 POST 為基礎的重新導向。 以 POST 為基礎的重新導向會觸發 SameSite 瀏覽器保護，因此這些元件已停用 SameSite。 大部分的[OAuth](https://oauth.net/)登入都不會受到影響，因為要求的流動方式不同。

每個發出 cookie 的 ASP.NET Core 元件都必須決定是否適合 SameSite。

## <a name="samesite-test-sample-code"></a>SameSite 測試範例程式碼

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

您可以下載並測試下列範例：

| 範例               | Document |
| ----------------- | ------------ |
| [.NET Core MVC](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)  | <xref:security/samesite/rp21> |

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

您可以下載並測試下列範例：


| 範例               | Document |
| ----------------- | ------------ |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)  | <xref:security/samesite/rp31> |

::: moniker-end

::: moniker range=">= aspnetcore-2.2"

## <a name="net-core-support-for-the-samesite-attribute"></a>SameSite 屬性的 .NET Core 支援

.NET Core 2.2 支援自12月2019的更新發行以來，適用于 SameSite 的 2019 draft standard。 開發人員可以使用`HttpCookie.SameSite`屬性，以程式設計方式控制 sameSite 屬性的值。 將屬性`SameSite`設定為 [Strict]、[寬鬆] 或 [無] 會導致這些值在網路上使用 cookie 寫入。 將其設定為等於（SameSiteMode）（-1）表示不應在具有 cookie 的網路上包含任何 sameSite 屬性

[!code-csharp[](samesite/snippets/Privacy.cshtml.cs?name=snippet)]

.NET Core 3.0 支援已更新的 SameSite 值，並將額外的列舉`SameSiteMode.Unspecified`值新增`SameSiteMode`至列舉。
這個新的值表示不應使用 cookie 傳送 sameSite。

::: moniker-end

::: moniker range="= aspnetcore-2.1"

## <a name="december-patch-behavior-changes"></a>12月修補程式列為變更

.NET Framework 和 .NET Core 2.1 的特定行為變更是`SameSite`屬性解讀`None`值的方式。 在修補程式之前，「 `None`不要發出屬性」的值，在修補程式之後，表示「發出屬性，其值`None`為」。 在修補程式之後`SameSite` ，的`(SameSiteMode)(-1)`值會導致不發出屬性。

表單驗證和會話狀態 cookie 的預設 SameSite 值已從`None`變更為`Lax`。

::: moniker-end

## <a name="api-usage-with-samesite"></a>使用 SameSite 的 API 使用方式

[HttpCoNtext](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)會將預設值附加至`Unspecified`，這表示 cookie 不會加入任何 SameSite 屬性，而且用戶端將會使用其預設行為（較寬鬆的新瀏覽器，也就是不需要）。 下列程式碼顯示如何將 cookie SameSite 值變更為`SameSiteMode.Lax`：

[!code-csharp[](samesite/sample/Pages/Index.cshtml.cs?name=snippet)]

發出 cookie 的所有 ASP.NET Core 元件都會使用適用于其案例的設定來覆寫先前的預設值。 先前已覆寫的預設值尚未變更。

| 元件 | 去 | 預設 |
| ------------- | ------------- |
| <xref:Microsoft.AspNetCore.Http.CookieBuilder> | <xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite> | `Unspecified` |
| <xref:Microsoft.AspNetCore.Http.HttpContext.Session>  | [SessionOptions. Cookie](xref:Microsoft.AspNetCore.Builder.SessionOptions.Cookie) |`Lax` |
| <xref:Microsoft.AspNetCore.Mvc.ViewFeatures.CookieTempDataProvider>  | [CookieTempDataProviderOptions. Cookie](xref:Microsoft.AspNetCore.Mvc.CookieTempDataProviderOptions.Cookie) | `Lax` |
| <xref:Microsoft.AspNetCore.Antiforgery.IAntiforgery> | [AntiforgeryOptions. Cookie](xref:Microsoft.AspNetCore.Antiforgery.AntiforgeryOptions.Cookie)| `Strict` |
| [Cookie 驗證](xref:Microsoft.Extensions.DependencyInjection.CookieExtensions.AddCookie*) | [Cookieauthenticationoptions.authenticationtype. Cookie](xref:Microsoft.AspNetCore.Builder.CookieAuthenticationOptions.CookieName) | `Lax` |
| <xref:Microsoft.Extensions.DependencyInjection.TwitterExtensions.AddTwitter*> | [TwitterOptions.StateCookie](xref:Microsoft.AspNetCore.Authentication.Twitter.TwitterOptions.StateCookie) | `Lax`  |
| <xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationHandler`1> | [RemoteAuthenticationOptions.CorrelationCookie](xref:Microsoft.AspNetCore.Authentication.RemoteAuthenticationOptions.CorrelationCookie)  | `None` |
| <xref:Microsoft.Extensions.DependencyInjection.OpenIdConnectExtensions.AddOpenIdConnect*> | [OpenIdConnectOptions.NonceCookie](xref:Microsoft.AspNetCore.Authentication.OpenIdConnect.OpenIdConnectOptions.NonceCookie)| `None` |
| [HttpCoNtext. Cookie. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*) | <xref:Microsoft.AspNetCore.Http.CookieOptions> | `Unspecified` |

::: moniker range=">= aspnetcore-3.1"

ASP.NET Core 3.1 和更新版本提供下列 SameSite 支援：

* 重新定義發出的`SameSiteMode.None`行為`SameSite=None`
* 加入新的值`SameSiteMode.Unspecified` ，以省略 SameSite 屬性。
* 所有 cookie Api 都會預設`Unspecified`為。 某些使用 cookie 的元件會針對其案例設定更具體的值。 如需範例，請參閱上表。

::: moniker-end

::: moniker range=">= aspnetcore-3.0"

在 ASP.NET Core 3.0 和更新版本中，SameSite 預設值已變更，以避免與不一致的用戶端預設值發生衝突。 下列 Api 已將預設值從`SameSiteMode.Lax `變更為`-1` ，以避免發出這些 cookie 的 SameSite 屬性：

* <xref:Microsoft.AspNetCore.Http.CookieOptions>用於[HttpCoNtext. Response. Append](xref:Microsoft.AspNetCore.Http.IResponseCookies.Append*)
* <xref:Microsoft.AspNetCore.Http.CookieBuilder>當做的 factory 使用`CookieOptions`
* [CookiePolicyOptions.MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)

::: moniker-end

## <a name="history-and-changes"></a>歷程記錄和變更

SameSite 支援首次使用[2016 draft 標準](https://tools.ietf.org/html/draft-west-first-party-cookies-07#section-4.1)在2.0 的 ASP.NET Core 中執行。 2016標準是加入宣告。 預設會將數個 cookie 設定為， `Lax`以 ASP.NET Core 加入宣告。 在驗證發生數個[問題](https://github.com/aspnet/Announcements/issues/318)之後，大部分 SameSite 的使用量都會[停用](https://github.com/aspnet/Announcements/issues/348)。

[修補程式](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)于2019年11月發行，從2016標準更新為2019標準。 [SameSite 規格的2019草稿](https://github.com/aspnet/Announcements/issues/390)：

* 與2016草稿**不**相容。 如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。
* 指定 cookie 預設會視為`SameSite=Lax` 。
* 指定明確`SameSite=None`判斷提示以啟用跨網站傳遞的 cookie 應該標記為`Secure`。 `None`是要退出的新專案。
* 發行給 ASP.NET Core 2.1、2.2 和3.0 的修補程式支援。 ASP.NET Core 3.1 有額外的 SameSite 支援。
* 排定預設會在[2020 年2月](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)啟用。 [Chrome](https://chromestatus.com/feature/5088147346030592) 瀏覽器已開始移至2019中的此標準。

## <a name="apis-impacted-by-the-change-from-the-2016-samesite-draft-standard-to-the-2019-draft-standard"></a>受 2016 SameSite draft standard 變更為 2019 draft standard 的 Api

* [SameSiteMode](xref:Microsoft.AspNetCore.Http.SameSiteMode)
* [Cookieoptions.secure. SameSite](xref:Microsoft.AspNetCore.Http.CookieOptions.SameSite)
* [CookieBuilder. SameSite](xref:Microsoft.AspNetCore.Http.CookieBuilder.SameSite)
* [CookiePolicyOptions.MinimumSameSitePolicy](xref:Microsoft.AspNetCore.Builder.CookiePolicyOptions.MinimumSameSitePolicy)
* <xref:Microsoft.Net.Http.Headers.SameSiteMode?displayProperty=fullName>
* <xref:Microsoft.Net.Http.Headers.SetCookieHeaderValue.SameSite?displayProperty=fullName>

<a name="sob"></a>

## <a name="supporting-older-browsers"></a>支援舊版瀏覽器

2016 SameSite 標準規定必須將未知的值視為`SameSite=Strict`值。 從支援 2016 SameSite standard 的舊版瀏覽器存取的應用程式，可能會在取得值為的`None`SameSite 屬性時中斷。 如果 Web 應用程式想要支援較舊的瀏覽器，則必須執行瀏覽器偵測。 ASP.NET Core 不會實作為瀏覽器偵測，因為使用者代理程式的值會高度變動並經常變更。 中<xref:Microsoft.AspNetCore.CookiePolicy>的擴充點允許插入使用者代理程式特定的邏輯。

在`Startup.Configure`中，新增在呼叫<xref:Microsoft.AspNetCore.Builder.CookiePolicyAppBuilderExtensions.UseCookiePolicy*> <xref:Microsoft.AspNetCore.Builder.AuthAppBuilderExtensions.UseAuthentication*>之前呼叫的程式碼，或*任何*寫入 cookie 的方法：

[!code-csharp[](samesite/sample/Startup.cs?name=snippet5&highlight=18-19)]

在`Startup.ConfigureServices`中，新增類似下列的程式碼：

::: moniker range="= aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet)]

::: moniker-end

::: moniker range="< aspnetcore-3.1"

[!code-csharp[](samesite/sample/Startup.cs?name=snippet)]

::: moniker-end

在上述範例中， `MyUserAgentDetectionLib.DisallowsSameSiteNone`是使用者提供的程式庫，可偵測使用者代理程式是否不`None`支援 SameSite：

[!code-csharp[](samesite/sample/Startup31.cs?name=snippet2)]

下列程式碼顯示範例`DisallowsSameSiteNone`方法：

> [!WARNING]
> 下列程式碼僅供示範之用：
> * 不應該將它視為已完成。
> * 不會維護或支援。

[!code-csharp[](samesite/sample/Startup31.cs?name=snippetX)]

## <a name="test-apps-for-samesite-problems"></a>測試應用程式的 SameSite 問題

與遠端網站（例如透過協力廠商登入）互動的應用程式需要：

* 測試多個瀏覽器的互動。
* 套用本檔中討論的[CookiePolicy 瀏覽器偵測和緩和措施](#sob)。

使用可選擇新 SameSite 行為的用戶端版本來測試 web 應用程式。 Chrome、Firefox 和 Chromium Edge 全都具有可用於測試的新加入宣告功能旗標。 在您的應用程式套用 SameSite 修補程式之後，請使用較舊的用戶端版本（尤其是 Safari）進行測試。 如需詳細資訊，請參閱本檔中的[支援舊版瀏覽器](#sob)。

### <a name="test-with-chrome"></a>使用 Chrome 進行測試

Chrome 78 + 會提供誤導的結果，因為它有暫時的緩和措施。 Chrome 78 + 暫時緩和可讓 cookie 少於兩分鐘。 已啟用適當測試旗標的 Chrome 76 或77提供更精確的結果。 若要測試新的 SameSite 行為`chrome://flags/#same-site-by-default-cookies` ，請切換為 [**已啟用**]。 舊版 Chrome （75和更低版本）會回報為失敗，並出現`None`新的設定。 請參閱本檔中的[支援舊版瀏覽器](#sob)。

Google 不會提供舊版的 chrome 版本。 請遵循[下載 Chromium](https://www.chromium.org/getting-involved/download-chromium)的指示來測試舊版 Chrome。 請勿從搜尋舊版 chrome 所提供的**連結下載 Chrome** 。

* [Chromium 76 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/664998/)
* [Chromium 74 Win64](https://commondatastorage.googleapis.com/chromium-browser-snapshots/index.html?prefix=Win_x64/638880/)

從抑制的版本`80.0.3975.0`開始，您可以使用新的旗`--enable-features=SameSiteDefaultChecksMethodRigorously`標來停用最寬鬆的 + POST 暫時緩和，以允許在已移除緩和功能的最終狀態中測試網站和服務。 如需詳細資訊，請參閱 Chromium Projects [SameSite Updates](https://www.chromium.org/updates/same-site)

### <a name="test-with-safari"></a>使用 Safari 進行測試

Safari 12 已嚴格實作為先前的草稿，當新`None`的值在 cookie 中時就會失敗。 `None`透過支援本檔中[較舊流覽](#sob)器的瀏覽器偵測程式碼來避免。 使用 MSAL、ADAL 或您使用的任何程式庫，測試 Safari 12、Safari 13 和以 WebKit 為基礎的 OS 樣式登入。 問題取決於基礎作業系統版本。 OSX Mojave （10.14）和 iOS 12 已知具有新 SameSite 行為的相容性問題。 將 OS 升級至 OSX Catalina （10.15）或 iOS 13 會修正問題。 Safari 目前沒有加入宣告旗標來測試新的規格行為。

### <a name="test-with-firefox"></a>使用 Firefox 進行測試

您可以使用功能旗`about:config` `network.cookie.sameSite.laxByDefault`標在頁面上進行選擇，以在第68版上測試新標準的 Firefox 支援。 尚未報告舊版 Firefox 的相容性問題。

### <a name="test-with-edge-browser"></a>使用 Edge 瀏覽器進行測試

Edge 支援舊的 SameSite 標準。 Edge 版本44沒有任何已知的新標準相容性問題。

### <a name="test-with-edge-chromium"></a>使用 Edge 進行測試（Chromium）

SameSite 旗標是在`edge://flags/#same-site-by-default-cookies`頁面上設定的。 Edge Chromium 未發現相容性問題。

### <a name="test-with-electron"></a>使用 Electron 進行測試

Electron 的版本包含舊版的 Chromium。 例如，小組所使用的 Electron 版本是 Chromium 66，這會展現較舊的行為。 您必須使用您的產品所使用的 Electron 版本來執行自己的相容性測試。 請參閱下一節中的[支援舊版瀏覽器](#sob)。

## <a name="additional-resources"></a>其他資源

* [Chromium Blog：開發人員：準備開始新的 SameSite = None;安全 Cookie 設定](https://blog.chromium.org/2019/10/developers-get-ready-for-new.html)
* [SameSite cookie 說明](https://web.dev/samesite-cookies-explained/)
* [2019年11月修補程式](https://devblogs.microsoft.com/dotnet/net-core-November-2019/)

 ::: moniker range=">= aspnetcore-2.1 < aspnetcore-3.0"

| 範例               | Document |
| ----------------- | ------------ |
| [.NET Core MVC](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21MVC)  | <xref:security/samesite/mvc21> |
| [.NET Core Razor Pages](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore21RazorPages)  | <xref:security/samesite/rp21> |

::: moniker-end

 ::: moniker range=">= aspnetcore-3.0"

| 範例               | Document |
| ----------------- | ------------ |
| [.NET Core Razor頁面](https://github.com/blowdart/AspNetSameSiteSamples/tree/master/AspNetCore31RazorPages)  | <xref:security/samesite/rp31> |

::: moniker-end
