---
uid: mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
title: 在 ASP.NET MVC 和網頁的 XSRF/CSRF 防護 |Microsoft 文件
author: Rick-Anderson
description: 跨站台要求偽造 （也稱為 XSRF 或 CSRF） 是 web 裝載的應用程式讓惡意網站可能會影響 interacti 攻擊...
ms.author: aspnetcontent
manager: wpickett
ms.date: 03/14/2013
ms.topic: article
ms.assetid: aadc5fa4-8215-4fc7-afd5-bcd2ef879728
ms.technology: dotnet-mvc
ms.prod: .net-framework
msc.legacyurl: /mvc/overview/security/xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages
msc.type: authoredcontent
ms.openlocfilehash: 6cf30daa7ed966b11405cec715c5bc803b567249
ms.sourcegitcommit: f8852267f463b62d7f975e56bea9aa3f68fbbdeb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/10/2018
ms.locfileid: "28033992"
---
<a name="xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages"></a>在 ASP.NET MVC 和網頁的 XSRF/CSRF 防護
====================
由[Rick Anderson](https://github.com/Rick-Anderson)

> 跨網站要求偽造 （也稱為 XSRF 或 CSRF） 是對 web 裝載的應用程式讓惡意網站可能會影響用戶端瀏覽器與該瀏覽器的受信任的網站之間的互動的攻擊。 這些攻擊都可能因為網頁瀏覽器將會傳送給網站的自動與每個要求的驗證權杖。 標準範例是驗證 cookie，例如 ASP。網路的表單驗證票證。 不過，這些攻擊可以目標網站使用任何持續性驗證機制 （例如 Windows 驗證、 基本和其他等等）。
> 
> XSRF 攻擊是不同的網路釣魚攻擊。 網路釣魚攻擊需要犧牲者。 在詐騙攻擊中，惡意網站會模仿目標網站，和受害者騙到提供給攻擊者的機密資訊。 在 XSRF 攻擊中，通常會沒有互動的必要。 相反地，攻擊者信賴憑證者會自動將所有相關的 cookie 傳送至目的地網站的瀏覽器上。
> 
> 如需詳細資訊，請參閱[開啟 Web 應用程式安全性專案](https://www.owasp.org/index.php/Main_Page)(OWASP) [XSRF](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF))。


## <a name="anatomy-of-an-attack"></a>攻擊的剖析

若要逐步解說 XSRF 攻擊，請考慮使用者想要執行某些線上銀行交易。 此使用者第一次造訪 WoodgroveBank.com 和記錄檔中，此時回應標頭將包含驗證 cookie:

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample1.cmd)]

驗證 cookie 會以工作階段 cookie，因為它將會自動清除瀏覽器瀏覽器處理序結束時。 不過，這段時間，直到瀏覽器會自動包含與每個要求 WoodgroveBank.com cookie。使用者現在想要傳送到另一個帳戶，$1000，因此她填寫表單，銀行網站上，並在瀏覽器對伺服器提出此要求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample2.cmd)]

這項作業具有副作用 （啟始貨幣交易），因為已選擇銀行網站，若要起始這項作業需要 HTTP POST。 伺服器會讀取要求的驗證權杖、 查閱目前使用者的帳戶號碼、 驗證足夠資金存在，而且就會起始到目的地帳戶交易。

她的線上銀行完成，使用者移動離開銀行網站，造訪網站上的其他位置。 其中一個這些站台 – fabrikam.com – 包括內嵌在頁面上的下列標記&lt;iframe&gt;:

[!code-html[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample3.html)]

這會將會導致瀏覽器以進行這項要求：

[!code-console[Main](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/samples/sample4.cmd)]

攻擊者會利用的事實以及使用者可能仍然有有效的驗證權杖的目標網站，她會導致瀏覽器將 HTTP POST 到目標站台會自動使用 Javascript 的小型程式碼片段。 如果仍然有效驗證權杖，銀行網站將會起始將 $250 傳入攻擊者所選擇的帳戶。

### <a name="ineffective-mitigations"></a>無效的補救措施

值得注意在上述案例中，WoodgroveBank.com 已透過 SSL 存取，而且有 SSL 驗證 cookie 已不足以防止攻擊。 攻擊者就可以指定[URI 配置](http://en.wikipedia.org/wiki/URI_scheme)(https) 在她&lt;表單&gt;項目，並在瀏覽器會繼續，只要這些 cookie 是一致的 uri，將會過期的 cookie 傳送到目標站台預期的目標的配置。

其中一個可以說，使用者應該不請瀏覽受信任的網站，為只有受信任的網站是協助仍然可以保持安全線上瀏覽。 某些真資料，但不幸的是這項建議不一定實用。 可能是使用者 「 信任 」 當地新聞網站 ConsolidatedMessenger。 ConsolidatedMessenger.com 和會移至網站，請瀏覽但該站台都有 XSS 弱點可能會讓攻擊者將 fabrikam.com 執行的程式碼中的相同程式碼片段。

您可以確認傳入要求可以[推薦者標頭](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)參考您的網域。 這樣會停止從協力廠商網域不知情的狀況下提交要求。 不過，有些人停用其瀏覽器推薦者標頭，基於隱私原因，而且如果犧牲者已安裝特定安全軟體攻擊者可以有時假冒該標頭。 正在驗證[推薦者標頭](http://www.w3.org/Protocols/HTTP/HTRQ_Headers.html#z14)不會被視為安全的方法，以防止 XSRF 攻擊。

## <a name="web-stack-runtime-xsrf-mitigations"></a>Web 堆疊執行階段 XSRF 補救措施

ASP.NET Web 堆疊執行階段使用的變數[同步器 token 模式](https://www.owasp.org/index.php/Cross-Site_Request_Forgery_(CSRF)_Prevention_Cheat_Sheet#General_Recommendation:_Synchronizer_Token_Pattern)防禦 XSRF 攻擊。 一般表單的 同步器 token 模式是伺服器 （除了驗證語彙基元） 的每個 HTTP POST 送出兩個防 XSRF 權杖： 當做 cookie，而表單值為另一個權杖。 ASP.NET 執行階段所產生的語彙基元值不具決定性或可預測的攻擊。 當提交語彙基元時，伺服器會允許兩個語彙基元傳遞比較核取時，才進行處理的要求。

XSRF 要求驗證*工作階段權杖*會儲存為 HTTP cookie，而且目前包含其裝載中的下列資訊：

- 安全性權杖，隨機的 128 位元識別碼所組成。   
 下圖顯示使用 Internet Explorer F12 開發人員工具所顯示的 XSRF 要求驗證工作階段權杖: (注意，這是目前的實作，而且主體，甚至可能，變更。)

![](xsrfcsrf-prevention-in-aspnet-mvc-and-web-pages/_static/image1.png)

*欄位語彙基元*儲存為`<input type="hidden" />`且包含其裝載中的下列資訊：

- 登入之使用者的使用者名稱 （如果通過驗證）。
- 所提供的任何其他資料[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)。

防 XSRF 權杖的承載都會經過加密並簽署，所以使用工具來檢查權杖時，您無法檢視的使用者名稱。 當 web 應用程式以 ASP.NET 4.0 為目標時，密碼編譯服務提供的[MachineKey.Encode](https://msdn.microsoft.com/library/system.web.security.machinekey.encode.aspx)常式。 當 web 應用程式的目標 ASP.NET 4.5 或更高版本、 密碼編譯服務所提供的[MachineKey.Protect](https://msdn.microsoft.com/library/system.web.security.machinekey.protect(v=vs.110))常式，其可提供較佳的效能、 擴充性和安全性。 請參閱下列部落格文章以取得詳細資料：

- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。1](https://blogs.msdn.com/b/webdev/archive/2012/10/22/cryptographic-improvements-in-asp-net-4-5-pt-1.aspx)
- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。2](https://blogs.msdn.com/b/webdev/archive/2012/10/23/cryptographic-improvements-in-asp-net-4-5-pt-2.aspx)
- [ASP.NET 4.5 中的密碼編譯增強功能、 pt。3](https://blogs.msdn.com/b/webdev/archive/2012/10/24/cryptographic-improvements-in-asp-net-4-5-pt-3.aspx)

## <a name="generating-the-tokens"></a>產生權杖

若要產生的防 XSRF 權杖，請呼叫[ @Html.AntiForgeryToken ](https://msdn.microsoft.com/library/dd470175.aspx) MVC 檢視中的方法或@AntiForgery.GetHtml從 Razor 頁面 （)。 然後，執行階段會執行下列步驟：

1. 如果目前的 HTTP 要求中已包含的防 XSRF 工作階段權杖 (防 XSRF cookie \_ \_RequestVerificationToken)，從其擷取安全性權杖。 如果 HTTP 要求不包含防 XSRF 工作階段權杖或安全性權杖的擷取失敗，就會產生新的隨機的防 XSRF 權杖。
2. 防 XSRF 欄位語彙基元會產生使用從上個步驟 (1) 和目前的登入之使用者的身分識別的安全性 token。 (如需判斷使用者的身分識別的詳細資訊，請參閱**[具有特殊的支援案例](#_Scenarios_with_special)** 下一節。)此外，如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/jj158328(v=vs.111).aspx)是設定，執行階段會呼叫其[GetAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.getadditionaldata(v=vs.111).aspx)方法並將傳回的字串包含在欄位語彙基元。 (請參閱**[組態和擴充性](#_Configuration_and_extensibility)** 節的詳細資訊。)
3. 如果步驟 (1) 中產生新的 ANTI-XSRF 權杖，新的工作階段語彙基元會建立包含該，並將加入至傳出 HTTP cookie 集合。 步驟 (2) 中的欄位語彙基元會包裝在`<input type="hidden" />`項目，然後這個 HTML 標記將會傳回值`Html.AntiForgeryToken()`或`AntiForgery.GetHtml()`。

## <a name="validating-the-tokens"></a>驗證權杖

若要驗證連入的防 XSRF 權杖，包括開發人員[ValidateAntiForgeryToken](https://msdn.microsoft.com/library/system.web.mvc.validateantiforgerytokenattribute(VS.108).aspx)屬性她 MVC 動作或控制器，或她呼叫`@AntiForgery.Validate()`從她 Razor 頁面。 執行階段會執行下列步驟：

1. 會讀取內送工作階段權杖和欄位語彙基元，並從每個擷取的防 XSRF 權杖。 防 XSRF 權杖必須是每個步驟 (2) 產生常式中完全相同。
2. 如果目前的使用者驗證時，她的使用者名稱進行比較與儲存在欄位語彙基元中的使用者名稱。 使用者名稱必須相符。
3. 如果[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)設定時，執行階段呼叫其*ValidateAdditionalData*方法。 此方法必須傳回布林值*true*。

如果驗證成功，允許要求繼續執行。 如果驗證失敗，將會擲回架構*HttpAntiForgeryException*。

## <a name="failure-conditions"></a>失敗狀況

任何從 ASP.NET Web 堆疊執行階段 v2 *HttpAntiForgeryException*期間擲回驗證將會包含有關錯誤的詳細的資訊。 目前已定義的失敗狀況如下：

- 表單語彙基元的工作階段權杖要求中沒有。
- 工作階段權杖或表單語彙基元無法讀取。 最可能的原因是伺服器陣列執行 ASP.NET Web 堆疊執行階段或伺服陣列的版本不相符， &lt;machineKey&gt; Web.config 中的項目各有不同的電腦。 您可以使用 Fiddler 之類的工具來強制執行此例外狀況任一防 XSRF 權杖遭到竄改。
- 已交換的工作階段權杖和欄位語彙基元。
- 工作階段權杖和欄位語彙基元包含不相符的安全性權杖。
- 欄位語彙基元中內嵌使用者名稱不符合目前已登入之使用者的使用者名稱。
- *[IAntiForgeryAdditionalDataProvider.ValidateAdditionalData](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider.validateadditionaldata(v=vs.111).aspx)* 方法會傳回*false*。

防 XSRF 設備可能也會執行其他檢查權杖產生或驗證期間，這些檢查期間發生的失敗可能會導致擲回例外狀況。 請參閱[WIF / ACS 宣告型驗證](#_WIF_ACS)和**[組態和擴充性](#_Configuration_and_extensibility)** 區段，如需詳細資訊。

<a id="_Scenarios_with_special"></a>

## <a name="scenarios-with-special-support"></a>使用特殊的支援案例

### <a name="anonymous-authentication"></a>匿名驗證

防 XSRF 系統包含 「 匿名 」 定義是以使用者的匿名使用者的特殊支援其中*IIdentity.IsAuthenticated*屬性會傳回*false*。 案例包括提供登入頁面 （之前驗證使用者），其中應用程式使用一種機制以外的自訂驗證配置來當做 XSRF 保護*IIdentity*來識別使用者。

若要支援這些案例，請記得安全性權杖，也就是 128 位元隨機產生不透明識別項聯結的工作階段和欄位 token。 這個安全性權杖用來追蹤她巡覽網站，讓它實際上可以做的匿名識別項目的個別使用者的工作階段。 取代使用者名稱會使用空的字串，如上面所述的產生和驗證常式。

<a id="_WIF_ACS"></a>

### <a name="wif--acs--claims-based-authentication"></a>WIF / ACS 宣告型驗證

一般來說， *IIdentity*內建於.NET Framework 類別都具有屬性的*IIdentity.Name*足以唯一識別特定的應用程式內的特定使用者。 例如， *FormsIdentity.Name*傳回使用者名稱儲存在成員資格資料庫中 （這是唯一的所有應用程式視資料庫而定）， *WindowsIdentity.Name*傳回網域限定識別使用者，等等。 這些系統提供的不只驗證，它們也*識別*應用程式的使用者。

宣告式驗證，相反地，不一定需要識別特定的使用者。 相反地， *ClaimsPrincipal*和*ClaimsIdentity*類型相關聯的一組*宣告*情況下，個別的宣告可能是"is 18 + 歲"或"是系統管理員 」 以任何其他項目。 由於尚未一定識別使用者，無法使用執行階段*ClaimsIdentity.Name*屬性做為此特定使用者的唯一識別碼。 小組已經看到真實世界範例其中*ClaimsIdentity.Name*傳回*null*、 傳回 （顯示） 的好記的名稱，或否則會傳回不適合做為唯一的識別項使用的字串使用者。

許多使用宣告式驗證的部署使用[Azure 存取控制服務](https://msdn.microsoft.com/library/windowsazure/gg429786.aspx)(ACS) 特別。 ACS 可讓開發人員設定個別*身分識別提供者*（例如 Microsoft 帳戶提供者的 ADFS，OpenID 提供者類似 yahoo ！ 等），和身分識別提供者傳回*命名識別項*. 這些名稱識別項可能包含個人識別資訊 (PII)，像是電子郵件地址，或它們可以匿名像私有的個人識別碼 (PPID)。 不論如何，tuple （身分識別提供者，名稱識別項） 不夠作為特定的使用者適當的追蹤 token，而她瀏覽網站，因此 ASP.NET Web 堆疊執行階段產生時，可以使用 tuple 取代使用者名稱和驗證防 XSRF 欄位語彙基元。 身分識別提供者 」 和 「 名稱識別碼的特定 Uri 是：

- `http://schemas.microsoft.com/accesscontrolservice/2010/07/claims/identityprovider`
- `http://schemas.xmlsoap.org/ws/2005/05/identity/claims/nameidentifier`

(請參閱此[ACS 文件頁面](https://msdn.microsoft.com/library/windowsazure/gg185971.aspx)如需詳細資訊。)

當產生或驗證權杖時，ASP.NET Web 堆疊執行階段會在執行階段嘗試繫結的型別：

- `Microsoft.IdentityModel.Claims.IClaimsIdentity, Microsoft.IdentityModel, Version=3.5.0.0, Culture=neutral, PublicKeyToken=31bf3856ad364e35` （適用於 WIF SDK。)
- `System.Security.Claims.ClaimsIdentity` （適用於.NET 4.5)。

如果這些型別存在，且目前使用者的*IIIIdentity*下列其中一種實作或子類型、 身分識別提供者 （名稱識別項），將會使用防 XSRF 設備 tuple 取代產生時，使用者名稱和驗證權杖。 如果沒有這類 tuple 存在時，要求將會失敗，開發人員說明如何設定來了解使用中的特定宣告為基礎的驗證機制的防 XSRF 系統發生錯誤。 請參閱**[組態和擴充性](#_Configuration_and_extensibility)** 節的詳細資訊。

### <a name="oauth--openid-authentication"></a>OAuth / OpenID 驗證

最後，防 XSRF 設備有特殊的應用程式使用 OAuth 或 OpenID 驗證的支援。 這項支援是啟發學習法為基礎： 如果目前*IIdentity.Name*開頭為 http:// 或 https:// ，然後將完成的使用者名稱比較使用序數比較子，而不是預設 OrdinalIgnoreCase 比較子。

<a id="_Configuration_and_extensibility"></a>

## <a name="configuration-and-extensibility"></a>組態和擴充性

有時候，開發人員可能想更加嚴格控制的防 XSRF 產生和驗證行為。 比方說，或許自動新增至回應的 HTTP cookie 將 MVC 和網頁的協助程式的預設行為是讓人困擾，並開發人員可能想要保存其他地方的語彙基元。 有兩個 Api，來協助進行此設定：

`AntiForgery.GetTokens(string oldCookieToken, out string newCookieToken, out string formToken);`  
`AntiForgery.Validate(string cookieToken, string formToken);`

*GetTokens*方法會採用做為輸入現有 XSRF 要求驗證工作階段語彙基元 （這可能是 null） 和產生做為輸出的新 XSRF 要求驗證工作階段語彙基元和欄位語彙基元。 語彙基元是無裝飾; 只是不透明的字串*formToken*值將會執行個體不包裝在&lt;輸入&gt;標記。 *NewCookieToken*值可能為 null; 如果發生這種情況，則*oldCookieToken*值仍然有效，而且需要設定任何新的回應 cookie。 呼叫端*GetTokens*負責保存任何必要的回應 cookie 或產生任何必要的標記; *GetTokens*方法本身並不會更改副作用的回應。 *驗證*方法會採用內送工作階段和欄位語彙基元，而且其上執行上述的驗證邏輯。

### <a name="antiforgeryconfig"></a>AntiForgeryConfig

開發人員可能會設定從應用程式的防 XSRF 系統\_開始。 以程式設計方式設定。 靜態屬性*AntiForgeryConfig*類型說明如下。 大部分使用宣告的使用者會想要設定 UniqueClaimTypeIdentifier 屬性。

| **Property** | **描述** |
| --- | --- |
| **AdditionalDataProvider** | [IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) ，在語彙基元產生期間提供額外的資料和權杖驗證期間會耗用額外的資料。 預設值是*null*。 如需詳細資訊，請參閱[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx) > 一節。 |
| **CookieName** | 提供用來儲存防 XSRF 工作階段權杖的 HTTP cookie 名稱的字串。 如果未設定此值，產生的名稱將會自動根據應用程式的已部署的虛擬路徑。 預設值是*null*。 |
| **RequireSsl** | 布林值，指出是否需要 SSL 安全保護的通道上送出的防 XSRF 權杖。 如果此值為*true*，任何自動產生的 cookie 會有 「 安全 」 的旗標設定，如果從呼叫中不會透過 SSL 提交的要求，將會擲回的防 XSRF Api。 預設值為 *false*。 |
| **SuppressIdentityHeuristicChecks** | 布林值，指出是否防 XSRF 系統應該停用其支援的宣告式身分識別。 如果此值為*true*，系統會假設*IIdentity.Name*很適合做為每個使用者的唯一識別項，將不會嘗試特殊案例*IClaimsIdentity*或*ClClaimsIdentity*中所述[WIF / ACS 宣告型驗證](#_WIF_ACS)> 一節。 預設值是 `false`。 |
| **UniqueClaimTypeIdentifier** | 字串，表示哪些宣告型別很適合做為每個使用者的唯一識別項。 如果這個值是設定和目前*IIdentity*宣告為基礎，系統會嘗試擷取之型別的宣告所指定*UniqueClaimTypeIdentifier*，將會使用對應的值取代產生的欄位語彙基元時，使用者的使用者名稱。 如果找不到宣告類型，系統會讓要求失敗。 預設值是*null*，表示系統應該使用身分識別提供者 （名稱識別項） 來取代使用者的使用者名稱如先前所述的 tuple。 |

<a id="_IAntiForgeryAdditionalDataProvider"></a>

### <a name="iantiforgeryadditionaldataprovider"></a>IAntiForgeryAdditionalDataProvider

*[IAntiForgeryAdditionalDataProvider](https://msdn.microsoft.com/library/system.web.helpers.iantiforgeryadditionaldataprovider(v=vs.111).aspx)* 類型可讓開發人員擴充以便在往返過程中每個語彙基元的其他資料的防 XSRF 系統行為。 *GetAdditionalData*每次呼叫方法會產生欄位語彙基元，並傳回值內嵌在產生的語彙基元。 實作者可能從這個方法會傳回時間戳記、 nonce 或任何其他值，她希望。

同樣地， *ValidateAdditionalData*每次呼叫方法來驗證欄位語彙基元時，並已內嵌在權杖中的 [詳細資料] 字串傳遞給方法。 驗證常式無法實作的逾時 （藉由檢查目前的時間針對建立語彙基元時，已儲存的時間）、 nonce 檢查常式，或任何其他所需的邏輯。

## <a name="design-decisions-and-security-considerations"></a>設計決策和安全性考量

安全性權杖所連結的工作階段和欄位 token，技術上時才需要嘗試保護 XSRF 攻擊匿名 / 未經驗證的使用者。 當使用者進行驗證時，可用來驗證權杖本身 （假定提交 cookie 的形式） 做為其中一個一半的同步處理程式 token 組。 不過，有效的情況下保護未驗證的使用者所叫用的登入頁面和防 XSRF 邏輯一律產生及驗證安全性權杖，即使已驗證的使用者所做更簡單。 它也提供一些額外的保護，攻擊者，以設定或猜測的工作階段權杖為了克服攻擊者的另一個障礙曾經危害欄位語彙基元。

單一定義域中裝載多個應用程式時，開發人員應謹慎小心。 比方說，即使*example1.cloudapp.net*和*example2.cloudapp.net*是不同的主控件下的所有主機之間沒有隱含的信任關係 *\*.cloudapp.net*網域。 這個隱含的信任關係[讓可能不受信任的主機會影響彼此的 cookie](http://stackoverflow.com/questions/9636857/how-can-asp-net-or-asp-net-mvc-be-protected-from-related-domain-cookie-attacks) （控管 AJAX 要求的相同原始原則不一定會套用至 HTTP cookie）。 ASP.NET Web 堆疊執行階段提供某些風險降低，因為即使惡意的子網域可將覆寫工作階段語彙基元會無法產生使用者的有效的欄位語彙基元欄位語彙基元中，內嵌使用者名稱。 不過，這類環境中裝載時的內建的防 XSRF 常式仍無法防禦劫持或登入 XSRF。

防 XSRF 常式目前不要不防範[clickjacking](https://www.owasp.org/index.php/Clickjacking)。 應用程式想要自行防禦 clickjacking 輕鬆這樣傳送 X 框架選項： sameorigin 所使用的每個回應的標頭。 此標頭支援所有新的瀏覽器。 如需詳細資訊，請參閱[IE 部落格](https://blogs.msdn.com/b/ieinternals/archive/2010/03/30/combating-clickjacking-with-x-frame-options.aspx)、 [SDL 部落格](https://blogs.msdn.com/b/sdl/archive/2009/02/05/clickjacking-defense-in-ie8.aspx)，和[OWASP](https://www.owasp.org/index.php/Clickjacking)。 ASP.NET Web 堆疊執行階段可能在某些未來的版本建立的 MVC 和網頁防 XSRF helper 會自動將此標頭，讓應用程式會自動施以保護，對這種攻擊。

Web 開發人員應該繼續以確保其站台不會受到影響 XSS 攻擊。 XSS 攻擊非常強大，而且成功利用也會中斷 ASP.NET Web 堆疊執行階段防禦 XSRF 攻擊。

## <a name="acknowledgment"></a>通知

[@LeviBroderick](https://twitter.com/LeviBroderick)撰寫者 ASP.NET 安全性驗證碼的大部分的這項資訊。
