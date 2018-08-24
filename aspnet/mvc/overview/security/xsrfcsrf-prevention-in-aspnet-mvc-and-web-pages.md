---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: 在 ASP.NET MVC 和 Web Pages 中的 XSRF/CSRF 防護 |Microsoft Docs
author: Rick-Anderson
description: 跨網站偽造要求 （也稱為 XSRF 或 CSRF） 攻擊會將對 web 裝載的應用程式讓惡意網站可能會影響 interacti...
ms.author: riande
ms.date: 03/14/2013
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: cd1b8de51c180471ab273c4541959368ffbd48a3
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833440"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>在 ASP.NET MVC 和 Web Pages 中的 XSRF/CSRF 防護
====================
藉由[Rick Anderson](https://github.com/Rick-Anderson)

> 跨網站要求偽造 （也稱為 XSRF 或 CSRF） 攻擊會將對 web 裝載的應用程式讓惡意網站可能會影響用戶端瀏覽器和該瀏覽器信任的網站之間的互動。 這些攻擊會進行可能的因為網頁瀏覽器會將驗證權杖，會自動隨著每個要求傳送至網站。 標準範例是驗證 cookie，例如 ASP。NET 的表單驗證票證。 不過，這些攻擊可以被目標網站使用 （例如 Windows 驗證、 基本及其他等等） 的任何持續驗證機制。
> 
> XSRF 攻擊是網路釣魚攻擊不同。 網路釣魚攻擊需要與受害者互動。 在網路釣魚攻擊，惡意網站會模擬目標網站，然後受害者因受騙而到機密資訊提供給攻擊者。 XSRF 攻擊時，則通常不需要互動需要與受害者。 相反地，攻擊者就信賴憑證者會自動將所有相關的 cookie 傳送至目的地網站的瀏覽器上。
> 
> 如需詳細資訊，請參閱 < [Open Web Application Security Project](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))。


## <a name="anatomy-of-an-attack"></a>攻擊的剖析

若要逐步解說 XSRF 攻擊，請考慮使用者想要執行的某些線上銀行交易。 此使用者第一次造訪 WoodgroveBank.com 並登入，屆時，回應標頭將包含驗證 cookie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

驗證 cookie 是工作階段 cookie，因為它會自動清除瀏覽器的瀏覽器處理序結束時。 不過，這段時間，直到瀏覽器就會自動包含與 WoodgroveBank.com 的每個要求的 cookie。 使用者現在想要傳輸到另一個帳戶，$1000，因此她填寫銀行網站上的表單和瀏覽器對伺服器提出此要求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

這項作業有副作用 （它會起始金錢交易），因為已選擇銀行網站，才能初始化這項作業需要 HTTP POST。 伺服器會讀取要求的驗證權杖、 查閱目前使用者的帳戶號碼、 驗證足夠的款項存在，而且起始的異動插入目的地帳戶。

她線上銀行 完整使用者瀏覽離開銀行網站，並在網站上造訪其他位置。 其中一個這些站台 – fabrikam.com – 包括內嵌在頁面上的下列標記&lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

然後讓瀏覽器提出此要求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻擊者會利用使用者仍然可能會有目標網站，有效的驗證權杖，而且她使用 Javascript 的小型程式碼片段來導致瀏覽器會自動將 HTTP POST 對目標站台。 如果仍然有效的驗證權杖，銀行網站就會起始美金 250 元的傳輸到攻擊者所選擇的帳戶。

### <a name="ineffective-mitigations"></a>無效的緩和措施

值得注意，在上述案例中，WoodgroveBank.com 所透過 SSL 存取，而且有僅限 SSL 驗證 cookie 不足，無法阻止攻擊。 攻擊者就能夠指定[URI 配置](http://en.wikipedia.org/wiki/URI_scheme)(https) 在她&lt;表單&gt;項目，並在瀏覽器會繼續傳送到目標站台未過期的 cookie，只要這些 cookie 是一致的 uri預期的目標的配置。

我們可以說，使用者應該只要不瀏覽不受信任的網站，為只有信任的網站是可協助您仍然可以保持安全線上瀏覽。 某些說老實話，但不幸的是這項建議不一定總是可行。 可能是使用者 「 信任 」 的地方新聞網站 ConsolidatedMessenger。 ConsolidatedMessenger.com 和會移至相反地，網站，請瀏覽，但該網站已 XSS 的安全性弱點讓攻擊者將 fabrikam.com 執行的程式碼的相同程式碼片段。

您可以確認傳入的要求有[推薦者標頭](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)參考您的網域。 這會停止從第三方網域不自覺地提交的要求。 不過，有些人停用其瀏覽器的推薦者標頭，基於隱私原因，而且如果受害者有某些不安全的軟體安裝，攻擊者可以有時假冒該標頭。 正在驗證[推薦者標頭](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)不是安全的方法，以防止 XSRF 攻擊。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web 堆疊執行階段 XSRF 緩和措施

ASP.NET Web 堆疊執行階段會使用的變化[同步器 token 模式](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)防禦 XSRF 攻擊。 同步處理程式 token 模式的一般形式是兩個防 XSRF 權杖會提交到伺服器 （除了驗證權杖） 的每個 HTTP POST： 一個權杖當做 cookie 中，而另一個則做為表單值。 ASP.NET 執行階段所產生的語彙基元值不具決定性或攻擊者可預測。 當提交權杖時，伺服器會允許這兩個語彙基元傳遞比較核取時，才進行處理的要求。

XSRF 要求驗證*工作階段權杖*會儲存為 HTTP cookie 和目前包含其承載中的下列資訊：

- 安全性權杖，其中包含隨機的 128 位元識別碼。   
 下圖顯示使用 Internet Explorer F12 開發人員工具所顯示的 XSRF 要求驗證工作階段權杖: (請注意，這是目前的實作，而且，即使有可能變更。)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*欄位的語彙基元*會儲存為`<input type="hidden" />`且包含其承載中的下列資訊：

- 登入的使用者的使用者名稱 （如果已驗證）。
- 所提供的任何其他資料[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)。

防 XSRF 權杖的內容會經過加密及簽署，所以使用工具來檢查權杖時，您無法檢視的使用者名稱。 當 web 應用程式以 ASP.NET 4.0 為目標時，會提供密碼編譯服務[MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx)常式。 當 web 應用程式的目標 ASP.NET 4.5 或更高版本，密碼編譯服務提供所[MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110))常式，可提供更好的效能、 擴充性和安全性。 請參閱下列部落格文章以取得詳細資料：

- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>產生權杖

若要產生的防 XSRF 權杖，呼叫[ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) MVC 檢視的方法或@AntiForgery.GetHtml從 Razor 頁面 （)。 然後，執行階段會執行下列步驟：

1. 如果目前的 HTTP 要求已包含的防 XSRF 工作階段權杖 (防 XSRF cookie \_ \_RequestVerificationToken)，從它擷取安全性權杖。 如果 HTTP 要求未包含的防 XSRF 工作階段權杖或安全性權杖的擷取失敗，就會產生新的隨機的 ANTI-XSRF 權杖。
2. 使用從上個步驟 (1) 和目前的登入的使用者的身分識別的安全性權杖產生的防 XSRF 欄位語彙基元。 (如需有關如何判斷使用者的身分識別的詳細資訊，請參閱 < **[特殊支援的案例](#_Scenarios_with_special)** 下一節。)此外，如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx)是設定，執行階段會呼叫其[GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)方法並將傳回的字串包含在欄位的語彙基元。 (請參閱 **[組態和擴充性](#_Configuration_and_extensibility)** 節的詳細資訊。)
3. 如果在步驟 (1) 中產生新的防 XSRF 權杖，新的工作階段權杖將會建立包含該，並會新增至輸出的 HTTP cookie 集合。 從步驟 (2) 欄位的語彙基元會包裝在`<input type="hidden" />`項目及這個 HTML 標記會傳回值`Html.AntiForgeryToken()`或`AntiForgery.GetHtml()`。

## <a name="validating-the-tokens"></a>驗證權杖

若要驗證連入的防 XSRF 權杖，包括開發人員[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx)屬性，在她的 MVC 動作或控制器或她呼叫`@AntiForgery.Validate()`從她的 Razor 頁面。 執行階段會執行下列步驟：

1. 會讀取內送工作階段權杖和欄位的語彙基元，並從每個擷取的防 XSRF 權杖。 ANTI-XSRF 權杖必須每一層代常式中的步驟 (2) 相同。
2. 如果目前的使用者驗證時，她的使用者名稱進行比較與儲存在欄位的語彙基元中的使用者名稱。 使用者名稱必須相符。
3. 如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)設定，執行階段呼叫其*ValidateAdditionalData*方法。 此方法必須傳回布林值 *，則為 true*。

如果驗證成功，要求允許繼續執行。 如果驗證失敗，此架構將會擲回*HttpAntiForgeryException*。

## <a name="failure-conditions"></a>失敗狀況

任何從 ASP.NET Web 堆疊執行階段 v2 *HttpAntiForgeryException*期間擲回驗證將會包含有關錯誤的詳細的資訊。 目前定義的失敗的條件如下︰

- 表單語彙基元的工作階段權杖不存在的要求中。
- 表單語彙基元的工作階段權杖是無法讀取。 最可能的原因，這是執行 ASP.NET Web 堆疊執行階段或伺服陣列的版本不相符的伺服器陣列所在&lt;machineKey&gt; Web.config 中的項目與不同電腦之間。 您可以使用 Fiddler 等工具，以強制這個例外狀況，藉由篡改任一 ANTI-XSRF 權杖。
- 交換工作階段權杖和欄位的語彙基元。
- 工作階段權杖和欄位的語彙基元包含不相符的安全性權杖。
- 欄位的語彙基元中內嵌使用者名稱不符合目前已登入使用者的使用者名稱。
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* 方法會傳回*false*。

防 XSRF 設施可能也會執行額外的檢查，權杖產生或驗證期間，這些檢查期間發生的失敗可能會導致擲回例外狀況。 請參閱[WIF / ACS / 宣告式驗證](#_WIF_ACS) 並 **[組態和擴充性](#_Configuration_and_extensibility)** 區段，如需詳細資訊。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>使用特殊的支援案例

### <a name="anonymous-authentication"></a>匿名驗證

防 XSRF 系統包含 「 匿名 」 定義是以使用者的匿名使用者的特殊支援所在*IIdentity.IsAuthenticated*屬性會傳回*false*。 案例包括提供的登入頁面 （在之前驗證使用者） 和自訂驗證配置： 應用程式使用一種機制以外的 XSRF 保護*IIdentity*來識別使用者。

若要支援這些案例中，您應該記得工作階段和欄位的語彙基元會加入由安全性權杖，也就是 128 位元隨機產生不透明的識別碼。 這個安全性權杖用來追蹤個別使用者的工作階段，她瀏覽網站，讓它實際上有匿名識別項的用途。 取代使用者名稱會使用空的字串，如上面所述的產生和驗證常式。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS / 宣告式驗證

通常*IIdentity*內建於.NET Framework 的類別具有屬性， *IIdentity.Name*就足以唯一識別特定的應用程式內的特定使用者。 例如， *FormsIdentity.Name*會傳回成員資格資料庫 （這是唯一的根據該資料庫的所有應用程式） 中儲存之 username *WindowsIdentity.Name*傳回網域限定識別使用者，依此類推。 系統所提供的不只是驗證;它們也*識別*應用程式的使用者。

宣告式驗證，相反地，不一定需要識別特定的使用者。 相反地， *ClaimsPrincipal*並*ClaimsIdentity*型別是一組相關聯*宣告*執行個體，其中的個別宣告可能會是"is 18 + 歲"或"是系統管理員 」 以任何其他項目。 由於尚未一定已經識別使用者，不能使用執行階段*ClaimsIdentity.Name*屬性做為這個特定使用者的唯一識別碼。 小組從未見過真實世界範例所在*ClaimsIdentity.Name*會傳回*null*、 傳回 （顯示器） 的易記名稱，或否則會傳回不適合做為唯一的識別項的字串針對目前的使用者。

許多使用宣告式驗證的部署使用[Azure 存取控制服務](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx)(ACS) 特別。 ACS 可讓開發人員設定個別*身分識別提供者*（例如 ADFS，Microsoft 帳戶提供者中，OpenID 提供者如 yahoo ！ 等），和身分識別提供者傳回*命名識別項*. 這些名稱識別項可能包含個人識別資訊 (PII)，像是電子郵件地址，或它們可以匿名像私密個人識別碼 (PPID)。 不論如何，(名稱識別碼中的 身分識別提供者） 的 tuple 夠作為特定的使用者適當的追蹤 token，而她瀏覽網站，讓 ASP.NET Web 堆疊執行階段產生時，可以使用取代使用者名稱的元組和驗證 ANTI-XSRF 欄位的權杖。 身分識別提供者和名稱識別項的特定 Uri 是：

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(請參閱此[ACS 文件頁面](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx)如需詳細資訊。)

當產生或驗證權杖時，ASP.NET Web 堆疊執行階段會在執行階段嘗試繫結至類型：

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` （適用於 WIF SDK。)
- `System.Security.Claims.ClaimsIdentity` （適用於.NET 4.5)。

如果這些型別存在，而且目前的使用者*IIIIdentity*實作或子類別其中一種都已型別、 防 XSRF 設備將會使用身分識別提供者 （名稱識別項） 來取代產生時的使用者名稱的元組和驗證的權杖。 如果沒有這類 tuple 存在，則要求會失敗並說明開發人員如何設定來了解使用中的特定宣告為基礎的驗證機制的防 XSRF 系統發生錯誤。 請參閱 **[組態和擴充性](#_Configuration_and_extensibility)** 節的詳細資訊。

### <a name="oauth--openid-authentication"></a>OAuth / OpenID 驗證

最後，防 XSRF 設備已使用 OAuth 或 OpenID 驗證的應用程式的特殊支援。 這項支援是啟發學習法為基礎： 如果目前*IIdentity.Name*開頭為 http:// 或 https:// ，然後將完成的使用者名稱比較使用序數比較子，而不是預設 OrdinalIgnoreCase 比較子。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>組態和擴充性

有時候，開發人員可以更緊密地控制的防 XSRF 產生和驗證行為。 比方說，或許是將 MVC 和網頁的協助程式的預設行為，自動將 HTTP cookie 加入回應中的不適當，，和開發人員可能會想要保存在其他地方的權杖。 有兩個 Api，來協助進行此設定：

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*方法當成輸入現有 XSRF 要求驗證工作階段權杖 （這可能是 null） 和產生做為輸出的新 XSRF 要求驗證工作階段權杖和欄位的語彙基元。 權杖是無裝飾; 只是不透明的字串*formToken*值執行個體不會包裝在&lt;輸入&gt;標記。 *NewCookieToken*值可能為 null; 如果發生這種情況，則*oldCookieToken*值仍然有效，而且需要設定任何新的回應 cookie。 呼叫端*GetTokens*負責保存任何必要的回應 cookie 或產生任何必要的標記; *GetTokens*方法本身並不會更改副作用的回應。 *驗證*方法會採用傳入工作階段和欄位語彙基元，並透過它們執行上述的驗證邏輯。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

開發人員可以設定從應用程式的防 XSRF 系統\_開始。 以程式設計方式設定。 靜態屬性*AntiForgeryConfig*類型如下所述。 使用宣告的大部分使用者都想要設定 UniqueClaimTypeIdentifier 屬性。

| **Property** | **描述** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)權杖產生期間提供的其他資料，在 權杖驗證期間耗用額外的資料。 預設值是*null*。 如需詳細資訊，請參閱 < [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)一節。 |
| **CookieName** | 提供用來儲存的防 XSRF 工作階段權杖的 HTTP cookie 名稱的字串。 如果未設定此值，產生的名稱會自動根據應用程式的已部署的虛擬路徑。 預設值是*null*。 |
| **RequireSsl** | 布林值，指出 ANTI-XSRF 權杖是否需要透過 SSL 安全通道傳送。 如果這個值是 *，則為 true*、 任何自動產生的 cookie 會有 「 安全 」 的旗標設定，及防 XSRF Api 將會擲回，如果從呼叫中不會透過 SSL 提交的要求。 預設值為 *false*。 |
| **SuppressIdentityHeuristicChecks** | 布林值，指出防 XSRF 系統是否應該停用宣告式身分識別支援。 如果這個值是 *，則為 true*，系統會假設*IIdentity.Name*很適合作為每個使用者的唯一識別碼，並不會嘗試將特殊大小寫*IClaimsIdentity*或是*ClClaimsIdentity*中所述[WIF / ACS / 宣告式驗證](#_WIF_ACS)一節。 預設值是 `false`。 |
| **UniqueClaimTypeIdentifier** | 字串，表示此宣告類型很適合作為每個使用者的唯一識別碼。 如果這個值是設定和目前*IIdentity*宣告為基礎，系統會嘗試擷取之宣告的型別所指定*UniqueClaimTypeIdentifier*，便使用對應的值取代產生欄位的語彙基元時，使用者的使用者名稱。 如果找不到宣告類型，系統會讓要求失敗。 預設值是*null*，這表示系統應該使用身分識別提供者 （名稱識別項） 如先前所述來取代使用者的使用者名稱的元組。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 類型可讓開發人員來回行程中每個語彙基元的其他資料來擴充防 XSRF 系統的行為。 *GetAdditionalData*每次呼叫方法會產生欄位的語彙基元，並傳回值內嵌在產生的權杖。 實作者可以從這個方法傳回時間戳記、 nonce 或她希望的任何其他值。

同樣地， *ValidateAdditionalData*每次呼叫方法驗證欄位的權杖時，並已內嵌在權杖中的 [詳細資料] 字串傳遞至方法。 驗證常式可以實作逾時 （藉由檢查目前的時間，與建立權杖時所儲存的時間）、 nonce 檢查常式，或任何其他所需的邏輯。

## <a name="design-decisions-and-security-considerations"></a>設計決策和安全性考量

連結的工作階段和欄位的語彙基元的安全性權杖時，就技術上而言才需要時嘗試匿名 / 未經驗證的使用者，避免 XSRF 攻擊。 當驗證使用者時，可用來驗證權杖本身 （大概是形式送出的 cookie） 做為其中一個同步處理程式的下半部 k 組。 不過，有效的情況下保護未經授權的使用者，所叫用的登入頁面和防 XSRF 邏輯是一律產生和驗證安全性權杖，即使已驗證的使用者進行更簡單。 它也提供一些額外的保護，攻擊者，設定或是猜測的工作階段權杖會攻擊者克服的另一個障礙入侵欄位語彙基元。

在單一網域裝載多個應用程式時，開發人員應謹慎小心。 比方說，即使 *example1.cloudapp.net* 並 *example2.cloudapp.net* 是不同的主控件下的所有主機之間沒有隱含的信任關係 *\*.cloudapp.net* 網域。 這個隱含的信任關係[可讓可能不受信任的主機，以影響彼此的 cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。 ASP.NET Web 堆疊執行階段會提供一些移轉工作，因此即使惡意的子網域是可以覆寫工作階段權杖它將無法產生使用者的有效欄位 token，將會內嵌至欄位的語彙基元的使用者名稱。 不過，當裝載這類環境中的內建的防 XSRF 常式仍然無法防禦工作階段攔截或登入 XSRF。

防 XSRF 常式目前執行無法防禦[點擊劫持](https://www.owasp.org/index.php/Clickjacking)。 想要自行防禦點擊劫持的應用程式可能輕鬆地執行這項操作傳送 X 框架選項： sameorigin 所標頭，以及每個回應。 所有新的瀏覽器都支援此標頭。 如需詳細資訊，請參閱 < [IE 部落格](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)，則[SDL 的部落格](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)，並[OWASP](https://www.owasp.org/index.php/Clickjacking)。 ASP.NET Web 堆疊執行階段可能會在某些未來的版本進行 MVC 和 Web 網頁的防 XSRF 協助程式會自動將此標頭，以便應用程式會自動受到保護防止此攻擊。

Web 開發人員應該繼續以確保其站台不是很容易遭受 XSS 攻擊。 XSS 攻擊非常強大，而成功的攻擊也會中斷 XSRF 攻擊的防禦 ASP.NET Web 堆疊執行階段。

## <a name="acknowledgment"></a>通知

[@LeviBroderick](https://twitter.com/LeviBroderick)是誰撰寫大部分的 ASP.NET 安全性程式碼大部分的這項資訊。
