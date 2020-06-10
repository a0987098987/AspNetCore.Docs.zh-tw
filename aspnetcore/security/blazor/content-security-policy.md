---
title: 針對 ASP.NET Core 強制執行內容安全性原則Blazor
author: guardrex
description: 瞭解如何搭配 ASP.NET Core apps 使用內容安全性原則（CSP） Blazor ，協助防止跨網站腳本（XSS）攻擊。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 8615b199373ca856c252b9f843e3635770367e4a
ms.sourcegitcommit: 6a71b560d897e13ad5b61d07afe4fcb57f8ef6dc
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/09/2020
ms.locfileid: "84106386"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-blazor"></a>針對 ASP.NET Core 強制執行內容安全性原則Blazor

By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)

[跨網站腳本（XSS）](xref:security/cross-site-scripting)是一種安全性弱點，攻擊者會將一或多個惡意的用戶端腳本放入應用程式轉譯的內容中。 內容安全性原則（CSP）會通知瀏覽器有效，以協助防止 XSS 攻擊：

* 載入內容的來源，包括腳本、樣式表單和影像。
* 頁面所採取的動作，指定表單的允許 URL 目標。
* 可以載入的外掛程式。

若要將 CSP 套用至應用程式，開發人員會在一或*directives*多個 `Content-Security-Policy` 標頭或標記中指定數個 csp 內容安全性指示詞 `<meta>` 。

載入頁面時，瀏覽器會評估原則。 瀏覽器會檢查頁面的來源，並判斷它們是否符合內容安全性指示詞的需求。 當資源不符合原則指示詞時，瀏覽器不會載入資源。 例如，假設有一個不允許協力廠商腳本的原則。 當頁面包含 `<script>` 屬性中具有協力廠商來源的標記時 `src` ，瀏覽器會防止載入腳本。

大部分的新式桌上型電腦和行動瀏覽器都支援 CSP，包括 Chrome、Edge、Firefox、Opera 和 Safari。 建議將 CSP 用於 Blazor 應用程式。

## <a name="policy-directives"></a>原則指示詞

請至少指定下列指示詞和 Blazor 應用程式的來源。 視需要新增其他指示詞和來源。 下列指示詞用於本文的套用[原則](#apply-the-policy)一節，其中 Blazor 提供 WebAssembly 和伺服器的範例安全性原則 Blazor ：

* [base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri)：限制頁面標記的 url `<base>` 。 指定 `self` 以指出應用程式的來源（包括配置和埠號碼）是有效的來源。
* [全部封鎖-混合內容](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content)：防止載入混合 HTTP 和 HTTPS 內容。
* [預設值-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src)：表示原則未明確指定之來源指示詞的 fallback。 指定 `self` 以指出應用程式的來源（包括配置和埠號碼）是有效的來源。
* [img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src)：表示影像的有效來源。
  * 指定 `data:` 允許從 url 載入影像 `data:` 。
  * 指定 `https:` 以允許從 HTTPS 端點載入影像。
* [物件-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src)：表示 `<object>` 、 `<embed>` 和標記的有效來源 `<applet>` 。 指定 `none` 以防止所有 URL 來源。
* [腳本-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src)：表示腳本的有效來源。
  * 指定 `https://stackpath.bootstrapcdn.com/` 啟動程式腳本的主機來源。
  * 指定 `self` 以指出應用程式的來源（包括配置和埠號碼）是有效的來源。
  * 在 Blazor WebAssembly 應用程式中：
    * 指定下列雜湊以允許載入必要的 Blazor WebAssembly 內嵌腳本：
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * 指定 `unsafe-eval` 使用 `eval()` 和方法，從字串建立程式碼。
  * 在 Blazor 伺服器應用程式中，指定為 `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` 樣式表單執行回溯偵測的內嵌腳本雜湊。
* [樣式-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src)：表示樣式表單的有效來源。
  * 指定啟動載入器樣式表單的 `https://stackpath.bootstrapcdn.com/` 主機來源。
  * 指定 `self` 以指出應用程式的來源（包括配置和埠號碼）是有效的來源。
  * 指定 `unsafe-inline` 允許使用內嵌樣式。 伺服器應用程式中的 UI 需要內嵌宣告，才能在 Blazor 初始要求之後重新連接用戶端和伺服器。 在未來的版本中，可能會移除內嵌樣式，因此不再 `unsafe-inline` 需要。
* [升級-不安全-要求](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests)：表示來自不安全（HTTP）來源的內容 url 應透過 HTTPS 安全地取得。

所有瀏覽器都支援上述指示詞，但 Microsoft Internet Explorer 除外。

若要取得其他內嵌腳本的 SHA 雜湊：

* 套用 [套用[原則](#apply-the-policy)] 區段中所示的 CSP。
* 在本機執行應用程式時，存取瀏覽器的開發人員工具主控台。 當 CSP 標頭或標記存在時，瀏覽器會計算並顯示已封鎖腳本的雜湊 `meta` 。
* 將瀏覽器提供的雜湊複製到 `script-src` 來源。 請在每個雜湊前後加上單引號。

如需內容安全性原則層級2瀏覽器支援矩陣，請參閱[我可以使用：內容安全性原則層級 2](https://www.caniuse.com/#feat=contentsecuritypolicy2)。

## <a name="apply-the-policy"></a>套用原則

使用 `<meta>` 標記來套用原則：

* 將屬性的值設定 `http-equiv` 為 `Content-Security-Policy` 。
* 將指示詞放在 `content` 屬性值中。 以分號（）分隔指示詞 `;` 。
* 請一律將 `meta` 標記放在 `<head>` 內容中。

下列各節顯示 Blazor WebAssembly 和伺服器的範例原則 Blazor 。 這些範例會針對的每個版本，使用此文章進行版本設定 Blazor 。 若要使用適用于您版本的版本，請選取此網頁上具有 [**版本**] 下拉式選取器的檔版本。

### <a name="blazor-webassembly"></a>BlazorWebAssembly

在 [ `<head>` *wwwroot/index.html*主機的內容] 頁面上，套用[原則](#policy-directives)指示詞一節中所述的指示詞：

```html
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=' 
                          'sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=' 
                          'sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=' 
                          'unsafe-eval';
               style-src https://stackpath.bootstrapcdn.com/
                         'self'
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

### <a name="blazor-server"></a>Blazor伺服器

在 `<head>` *Pages/_Host*的 [內容] 頁面中，套用[原則](#policy-directives)指示詞一節中所述的指示詞：

```cshtml
<meta http-equiv="Content-Security-Policy" 
      content="base-uri 'self';
               block-all-mixed-content;
               default-src 'self';
               img-src data: https:;
               object-src 'none';
               script-src https://stackpath.bootstrapcdn.com/ 
                          'self' 
                          'sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=';
               style-src https://stackpath.bootstrapcdn.com/
                         'self' 
                         'unsafe-inline';
               upgrade-insecure-requests;">
```

## <a name="meta-tag-limitations"></a>中繼標記限制

`<meta>`標記原則不支援下列指示詞：

* [框架-祖系](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [報告至](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [報告-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [沙箱](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

若要支援上述指示詞，請使用名為的標頭 `Content-Security-Policy` 。 指示詞字串是標頭的值。

## <a name="test-a-policy-and-receive-violation-reports"></a>測試原則並接收違規報告

測試可協助確認在建立初始原則時，不會不慎封鎖協力廠商腳本。

若要在一段時間內測試原則，而不強制執行原則指示詞，請將 `<meta>` `http-equiv` 標頭型原則的標記屬性或標頭名稱設定為 `Content-Security-Policy-Report-Only` 。 失敗報告會以 JSON 檔的形式傳送到指定的 URL。 如需詳細資訊，請參閱[MDN web 檔：內容-安全性-僅限報告](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only)。

如需原則作用時的違規報告，請參閱下列文章：

* [報告至](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [報告-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

雖然不再 `report-uri` 建議使用，但必須使用這兩個指示詞，直到 `report-to` 所有主要瀏覽器都支援為止。 請勿獨佔使用 `report-uri` ，因為的支援 `report-uri` 可能會隨時從*at any time*瀏覽器中卸載。 `report-uri`當受到完整支援時，請移除原則中的支援 `report-to` 。 若要追蹤的採用 `report-to` ，請參閱[我可以使用：報告至](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to)。

測試和更新每個版本的應用程式原則。

## <a name="troubleshoot"></a>疑難排解

* 錯誤會出現在瀏覽器的開發人員工具主控台中。 瀏覽器會提供下列資訊：
  * 不符合原則的元素。
  * 如何修改原則以允許封鎖的專案。
* 只有當用戶端的瀏覽器支援所有包含的指示詞時，原則才會完全有效。 如需目前的瀏覽器支援矩陣，請參閱[我可以使用：內容-安全性-原則](https://caniuse.com/#search=Content-Security-Policy)。

## <a name="additional-resources"></a>其他資源

* [MDN web 檔：內容-安全性-原則](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [內容安全性原則層級2](https://www.w3.org/TR/CSP2/)
* [Google CSP 評估工具](https://csp-evaluator.withgoogle.com/)
