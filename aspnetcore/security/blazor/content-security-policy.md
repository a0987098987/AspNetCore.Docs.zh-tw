---
title: 針對 ASP.NET Core 強制執行內容安全性原則Blazor
author: guardrex
description: 瞭解如何搭配 ASP.NET Core Blazor Apps 使用內容安全性原則（CSP），協助防止跨網站腳本（XSS）攻擊。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 03/02/2020
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/blazor/content-security-policy
ms.openlocfilehash: 8c5e1c5dd2d41efade91a612bea2855569a61fee
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82775579"
---
# <a name="enforce-a-content-security-policy-for-aspnet-core-blazor"></a><span data-ttu-id="e5da0-103">針對 ASP.NET Core 強制執行內容安全性原則Blazor</span><span class="sxs-lookup"><span data-stu-id="e5da0-103">Enforce a Content Security Policy for ASP.NET Core Blazor</span></span>

<span data-ttu-id="e5da0-104">By [Javier Calvarro Nelson](https://github.com/javiercn)和[Luke Latham](https://github.com/guardrex)</span><span class="sxs-lookup"><span data-stu-id="e5da0-104">By [Javier Calvarro Nelson](https://github.com/javiercn) and [Luke Latham](https://github.com/guardrex)</span></span>

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

<span data-ttu-id="e5da0-105">[跨網站腳本（XSS）](xref:security/cross-site-scripting)是一種安全性弱點，攻擊者會將一或多個惡意的用戶端腳本放入應用程式轉譯的內容中。</span><span class="sxs-lookup"><span data-stu-id="e5da0-105">[Cross-Site Scripting (XSS)](xref:security/cross-site-scripting) is a security vulnerability where an attacker places one or more malicious client-side scripts into an app's rendered content.</span></span> <span data-ttu-id="e5da0-106">內容安全性原則（CSP）會通知瀏覽器有效，以協助防止 XSS 攻擊：</span><span class="sxs-lookup"><span data-stu-id="e5da0-106">A Content Security Policy (CSP) helps protect against XSS attacks by informing the browser of valid:</span></span>

* <span data-ttu-id="e5da0-107">載入內容的來源，包括腳本、樣式表單和影像。</span><span class="sxs-lookup"><span data-stu-id="e5da0-107">Sources for loaded content, including scripts, stylesheets, and images.</span></span>
* <span data-ttu-id="e5da0-108">頁面所採取的動作，指定表單的允許 URL 目標。</span><span class="sxs-lookup"><span data-stu-id="e5da0-108">Actions taken by a page, specifying permitted URL targets of forms.</span></span>
* <span data-ttu-id="e5da0-109">可以載入的外掛程式。</span><span class="sxs-lookup"><span data-stu-id="e5da0-109">Plugins that can be loaded.</span></span>

<span data-ttu-id="e5da0-110">若要將 CSP 套用至應用程式，開發人員會在一或多個`Content-Security-Policy`標頭或`<meta>` *標記中指定*數個 csp 內容安全性指示詞。</span><span class="sxs-lookup"><span data-stu-id="e5da0-110">To apply a CSP to an app, the developer specifies several CSP content security *directives* in one or more `Content-Security-Policy` headers or `<meta>` tags.</span></span>

<span data-ttu-id="e5da0-111">載入頁面時，瀏覽器會評估原則。</span><span class="sxs-lookup"><span data-stu-id="e5da0-111">Policies are evaluated by the browser while a page is loading.</span></span> <span data-ttu-id="e5da0-112">瀏覽器會檢查頁面的來源，並判斷它們是否符合內容安全性指示詞的需求。</span><span class="sxs-lookup"><span data-stu-id="e5da0-112">The browser inspects the page's sources and determines if they meet the requirements of the content security directives.</span></span> <span data-ttu-id="e5da0-113">當資源不符合原則指示詞時，瀏覽器不會載入資源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-113">When policy directives aren't met for a resource, the browser doesn't load the resource.</span></span> <span data-ttu-id="e5da0-114">例如，假設有一個不允許協力廠商腳本的原則。</span><span class="sxs-lookup"><span data-stu-id="e5da0-114">For example, consider a policy that doesn't allow third-party scripts.</span></span> <span data-ttu-id="e5da0-115">當頁面包含`src`屬性中`<script>`具有協力廠商來源的標記時，瀏覽器會防止載入腳本。</span><span class="sxs-lookup"><span data-stu-id="e5da0-115">When a page contains a `<script>` tag with a third-party origin in the `src` attribute, the browser prevents the script from loading.</span></span>

<span data-ttu-id="e5da0-116">大部分的新式桌上型電腦和行動瀏覽器都支援 CSP，包括 Chrome、Edge、Firefox、Opera 和 Safari。</span><span class="sxs-lookup"><span data-stu-id="e5da0-116">CSP is supported in most modern desktop and mobile browsers, including Chrome, Edge, Firefox, Opera, and Safari.</span></span> <span data-ttu-id="e5da0-117">建議將 CSP 用於Blazor應用程式。</span><span class="sxs-lookup"><span data-stu-id="e5da0-117">CSP is recommended for Blazor apps.</span></span>

## <a name="policy-directives"></a><span data-ttu-id="e5da0-118">原則指示詞</span><span class="sxs-lookup"><span data-stu-id="e5da0-118">Policy directives</span></span>

<span data-ttu-id="e5da0-119">請至少指定下列指示詞和Blazor應用程式的來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-119">Minimally, specify the following directives and sources for Blazor apps.</span></span> <span data-ttu-id="e5da0-120">視需要新增其他指示詞和來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-120">Add additional directives and sources as needed.</span></span> <span data-ttu-id="e5da0-121">下列指示詞用於本文的套用[原則](#apply-the-policy)一節，其中提供Blazor WebAssembly 和Blazor伺服器的範例安全性原則：</span><span class="sxs-lookup"><span data-stu-id="e5da0-121">The following directives are used in the [Apply the policy](#apply-the-policy) section of this article, where example security policies for Blazor WebAssembly and Blazor Server are provided:</span></span>

* <span data-ttu-id="e5da0-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash;會限制頁面`<base>`標記的 url。</span><span class="sxs-lookup"><span data-stu-id="e5da0-122">[base-uri](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/base-uri) &ndash; Restricts the URLs for a page's `<base>` tag.</span></span> <span data-ttu-id="e5da0-123">指定`self`以指出應用程式的來源（包括配置和埠號碼）是有效的來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-123">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="e5da0-124">[全部封鎖-混合內容](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash;可防止載入混合 HTTP 和 HTTPS 內容。</span><span class="sxs-lookup"><span data-stu-id="e5da0-124">[block-all-mixed-content](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/block-all-mixed-content) &ndash; Prevents loading mixed HTTP and HTTPS content.</span></span>
* <span data-ttu-id="e5da0-125">[預設值-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash;表示原則未明確指定之來源指示詞的 fallback。</span><span class="sxs-lookup"><span data-stu-id="e5da0-125">[default-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/default-src) &ndash; Indicates a fallback for source directives that aren't explicitly specified by the policy.</span></span> <span data-ttu-id="e5da0-126">指定`self`以指出應用程式的來源（包括配置和埠號碼）是有效的來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-126">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
* <span data-ttu-id="e5da0-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash;表示影像的有效來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-127">[img-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/img-src) &ndash; Indicates valid sources for images.</span></span>
  * <span data-ttu-id="e5da0-128">指定`data:`允許從`data:` url 載入影像。</span><span class="sxs-lookup"><span data-stu-id="e5da0-128">Specify `data:` to permit loading images from `data:` URLs.</span></span>
  * <span data-ttu-id="e5da0-129">指定`https:`以允許從 HTTPS 端點載入影像。</span><span class="sxs-lookup"><span data-stu-id="e5da0-129">Specify `https:` to permit loading images from HTTPS endpoints.</span></span>
* <span data-ttu-id="e5da0-130">[物件-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash;表示、 `<embed>`和`<applet>`標記的`<object>`有效來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-130">[object-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/object-src) &ndash; Indicates valid sources for the `<object>`, `<embed>`, and `<applet>` tags.</span></span> <span data-ttu-id="e5da0-131">指定`none`以防止所有 URL 來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-131">Specify `none` to prevent all URL sources.</span></span>
* <span data-ttu-id="e5da0-132">[腳本-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash;表示腳本的有效來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-132">[script-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/script-src) &ndash; Indicates valid sources for scripts.</span></span>
  * <span data-ttu-id="e5da0-133">指定啟動`https://stackpath.bootstrapcdn.com/`程式腳本的主機來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-133">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap scripts.</span></span>
  * <span data-ttu-id="e5da0-134">指定`self`以指出應用程式的來源（包括配置和埠號碼）是有效的來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-134">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="e5da0-135">在Blazor WebAssembly 應用程式中：</span><span class="sxs-lookup"><span data-stu-id="e5da0-135">In a Blazor WebAssembly app:</span></span>
    * <span data-ttu-id="e5da0-136">指定下列雜湊以允許載入必要Blazor的 WebAssembly 內嵌腳本：</span><span class="sxs-lookup"><span data-stu-id="e5da0-136">Specify the following hashes to permit the required Blazor WebAssembly inline scripts to load:</span></span>
      * `sha256-v8ZC9OgMhcnEQ/Me77/R9TlJfzOBqrMTW8e1KuqLaqc=`
      * `sha256-If//FtbPc03afjLezvWHnC3Nbu4fDM04IIzkPaf3pH0=`
      * `sha256-v8v3RKRPmN4odZ1CWM5gw80QKPCCWMcpNeOmimNL2AA=`
    * <span data-ttu-id="e5da0-137">指定`unsafe-eval`使用`eval()`和方法，從字串建立程式碼。</span><span class="sxs-lookup"><span data-stu-id="e5da0-137">Specify `unsafe-eval` to use `eval()` and methods for creating code from strings.</span></span>
  * <span data-ttu-id="e5da0-138">在Blazor伺服器應用程式中，指定`sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=`為樣式表單執行回溯偵測的內嵌腳本雜湊。</span><span class="sxs-lookup"><span data-stu-id="e5da0-138">In a Blazor Server app, specify the `sha256-34WLX60Tw3aG6hylk0plKbZZFXCuepeQ6Hu7OqRf8PI=` hash for the inline script that performs fallback detection for stylesheets.</span></span>
* <span data-ttu-id="e5da0-139">[樣式-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash;表示樣式表單的有效來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-139">[style-src](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/style-src) &ndash; Indicates valid sources for stylesheets.</span></span>
  * <span data-ttu-id="e5da0-140">指定啟動`https://stackpath.bootstrapcdn.com/`載入器樣式表單的主機來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-140">Specify the `https://stackpath.bootstrapcdn.com/` host source for Bootstrap stylesheets.</span></span>
  * <span data-ttu-id="e5da0-141">指定`self`以指出應用程式的來源（包括配置和埠號碼）是有效的來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-141">Specify `self` to indicate that the app's origin, including the scheme and port number, is a valid source.</span></span>
  * <span data-ttu-id="e5da0-142">指定`unsafe-inline`允許使用內嵌樣式。</span><span class="sxs-lookup"><span data-stu-id="e5da0-142">Specify `unsafe-inline` to allow the use of inline styles.</span></span> <span data-ttu-id="e5da0-143">伺服器應用程式中Blazor的 UI 需要內嵌宣告，才能在初始要求之後重新連接用戶端和伺服器。</span><span class="sxs-lookup"><span data-stu-id="e5da0-143">The inline declaration is required for the UI in Blazor Server apps for reconnecting the client and server after the initial request.</span></span> <span data-ttu-id="e5da0-144">在未來的版本中，可能會移除內嵌樣式， `unsafe-inline`因此不再需要。</span><span class="sxs-lookup"><span data-stu-id="e5da0-144">In a future release, inline styling might be removed so that `unsafe-inline` is no longer required.</span></span>
* <span data-ttu-id="e5da0-145">[升級-不安全-要求](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash;表示來自不安全（HTTP）來源的內容 url 應透過 HTTPS 安全地取得。</span><span class="sxs-lookup"><span data-stu-id="e5da0-145">[upgrade-insecure-requests](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/upgrade-insecure-requests) &ndash; Indicates that content URLs from insecure (HTTP) sources should be acquired securely over HTTPS.</span></span>

<span data-ttu-id="e5da0-146">所有瀏覽器都支援上述指示詞，但 Microsoft Internet Explorer 除外。</span><span class="sxs-lookup"><span data-stu-id="e5da0-146">The preceding directives are supported by all browsers except Microsoft Internet Explorer.</span></span>

<span data-ttu-id="e5da0-147">若要取得其他內嵌腳本的 SHA 雜湊：</span><span class="sxs-lookup"><span data-stu-id="e5da0-147">To obtain SHA hashes for additional inline scripts:</span></span>

* <span data-ttu-id="e5da0-148">套用 [套用[原則](#apply-the-policy)] 區段中所示的 CSP。</span><span class="sxs-lookup"><span data-stu-id="e5da0-148">Apply the CSP shown in the [Apply the policy](#apply-the-policy) section.</span></span>
* <span data-ttu-id="e5da0-149">在本機執行應用程式時，存取瀏覽器的開發人員工具主控台。</span><span class="sxs-lookup"><span data-stu-id="e5da0-149">Access the browser's developer tools console while running the app locally.</span></span> <span data-ttu-id="e5da0-150">當 CSP 標頭或`meta`標記存在時，瀏覽器會計算並顯示已封鎖腳本的雜湊。</span><span class="sxs-lookup"><span data-stu-id="e5da0-150">The browser calculates and displays hashes for blocked scripts when a CSP header or `meta` tag is present.</span></span>
* <span data-ttu-id="e5da0-151">將瀏覽器提供的雜湊複製到`script-src`來源。</span><span class="sxs-lookup"><span data-stu-id="e5da0-151">Copy the hashes provided by the browser to the `script-src` sources.</span></span> <span data-ttu-id="e5da0-152">請在每個雜湊前後加上單引號。</span><span class="sxs-lookup"><span data-stu-id="e5da0-152">Use single quotes around each hash.</span></span>

<span data-ttu-id="e5da0-153">如需內容安全性原則層級2瀏覽器支援矩陣，請參閱[我可以使用：內容安全性原則層級 2](https://www.caniuse.com/#feat=contentsecuritypolicy2)。</span><span class="sxs-lookup"><span data-stu-id="e5da0-153">For a Content Security Policy Level 2 browser support matrix, see [Can I use: Content Security Policy Level 2](https://www.caniuse.com/#feat=contentsecuritypolicy2).</span></span>

## <a name="apply-the-policy"></a><span data-ttu-id="e5da0-154">套用原則</span><span class="sxs-lookup"><span data-stu-id="e5da0-154">Apply the policy</span></span>

<span data-ttu-id="e5da0-155">使用`<meta>`標記來套用原則：</span><span class="sxs-lookup"><span data-stu-id="e5da0-155">Use a `<meta>` tag to apply the policy:</span></span>

* <span data-ttu-id="e5da0-156">將`http-equiv`屬性的值設定為`Content-Security-Policy`。</span><span class="sxs-lookup"><span data-stu-id="e5da0-156">Set the value of the `http-equiv` attribute to `Content-Security-Policy`.</span></span>
* <span data-ttu-id="e5da0-157">將指示詞放在`content`屬性值中。</span><span class="sxs-lookup"><span data-stu-id="e5da0-157">Place the directives in the `content` attribute value.</span></span> <span data-ttu-id="e5da0-158">以分號（`;`）分隔指示詞。</span><span class="sxs-lookup"><span data-stu-id="e5da0-158">Separate directives with a semicolon (`;`).</span></span>
* <span data-ttu-id="e5da0-159">請一律將`meta`標記放在`<head>`內容中。</span><span class="sxs-lookup"><span data-stu-id="e5da0-159">Always place the `meta` tag in the `<head>` content.</span></span>

<span data-ttu-id="e5da0-160">下列各節顯示Blazor WebAssembly 和Blazor伺服器的範例原則。</span><span class="sxs-lookup"><span data-stu-id="e5da0-160">The following sections show example policies for Blazor WebAssembly and Blazor Server.</span></span> <span data-ttu-id="e5da0-161">這些範例會針對的每個版本，使用此Blazor文章進行版本設定。</span><span class="sxs-lookup"><span data-stu-id="e5da0-161">These examples are versioned with this article for each release of Blazor.</span></span> <span data-ttu-id="e5da0-162">若要使用適用于您版本的版本，請選取此網頁上具有 [**版本**] 下拉式選取器的檔版本。</span><span class="sxs-lookup"><span data-stu-id="e5da0-162">To use a version appropriate for your release, select the document version with the **Version** drop down selector on this webpage.</span></span>

### <a name="blazor-webassembly"></a>Blazor<span data-ttu-id="e5da0-163">WebAssembly</span><span class="sxs-lookup"><span data-stu-id="e5da0-163"> WebAssembly</span></span>

<span data-ttu-id="e5da0-164">在 [ `<head>` *wwwroot/index.html*主機的內容] 頁面上，套用[原則](#policy-directives)指示詞一節中所述的指示詞：</span><span class="sxs-lookup"><span data-stu-id="e5da0-164">In the `<head>` content of the *wwwroot/index.html* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

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

### <a name="blazor-server"></a>Blazor<span data-ttu-id="e5da0-165">伺服器</span><span class="sxs-lookup"><span data-stu-id="e5da0-165"> Server</span></span>

<span data-ttu-id="e5da0-166">在`<head>` *Pages/_Host*的 [內容] 頁面中，套用[原則](#policy-directives)指示詞一節中所述的指示詞：</span><span class="sxs-lookup"><span data-stu-id="e5da0-166">In the `<head>` content of the *Pages/_Host.cshtml* host page, apply the directives described in the [Policy directives](#policy-directives) section:</span></span>

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

## <a name="meta-tag-limitations"></a><span data-ttu-id="e5da0-167">中繼標記限制</span><span class="sxs-lookup"><span data-stu-id="e5da0-167">Meta tag limitations</span></span>

<span data-ttu-id="e5da0-168">`<meta>`標記原則不支援下列指示詞：</span><span class="sxs-lookup"><span data-stu-id="e5da0-168">A `<meta>` tag policy doesn't support the following directives:</span></span>

* [<span data-ttu-id="e5da0-169">框架-祖系</span><span class="sxs-lookup"><span data-stu-id="e5da0-169">frame-ancestors</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/frame-ancestors)
* [<span data-ttu-id="e5da0-170">報告至</span><span class="sxs-lookup"><span data-stu-id="e5da0-170">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="e5da0-171">報告-uri</span><span class="sxs-lookup"><span data-stu-id="e5da0-171">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)
* [<span data-ttu-id="e5da0-172">沙箱</span><span class="sxs-lookup"><span data-stu-id="e5da0-172">sandbox</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/sandbox)

<span data-ttu-id="e5da0-173">若要支援上述指示詞，請使用名`Content-Security-Policy`為的標頭。</span><span class="sxs-lookup"><span data-stu-id="e5da0-173">To support the preceding directives, use a header named `Content-Security-Policy`.</span></span> <span data-ttu-id="e5da0-174">指示詞字串是標頭的值。</span><span class="sxs-lookup"><span data-stu-id="e5da0-174">The directive string is the header's value.</span></span>

## <a name="test-a-policy-and-receive-violation-reports"></a><span data-ttu-id="e5da0-175">測試原則並接收違規報告</span><span class="sxs-lookup"><span data-stu-id="e5da0-175">Test a policy and receive violation reports</span></span>

<span data-ttu-id="e5da0-176">測試可協助確認在建立初始原則時，不會不慎封鎖協力廠商腳本。</span><span class="sxs-lookup"><span data-stu-id="e5da0-176">Testing helps confirm that third-party scripts aren't inadvertently blocked when building an initial policy.</span></span>

<span data-ttu-id="e5da0-177">若要在一段時間內測試原則，而不強制執行原則指示詞`<meta>` ，請`http-equiv`將標頭型原則的標記屬性或標頭`Content-Security-Policy-Report-Only`名稱設定為。</span><span class="sxs-lookup"><span data-stu-id="e5da0-177">To test a policy over a period of time without enforcing the policy directives, set the `<meta>` tag's `http-equiv` attribute or header name of a header-based policy to `Content-Security-Policy-Report-Only`.</span></span> <span data-ttu-id="e5da0-178">失敗報告會以 JSON 檔的形式傳送到指定的 URL。</span><span class="sxs-lookup"><span data-stu-id="e5da0-178">Failure reports are sent as JSON documents to a specified URL.</span></span> <span data-ttu-id="e5da0-179">如需詳細資訊，請參閱[MDN web 檔：內容-安全性-僅限報告](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only)。</span><span class="sxs-lookup"><span data-stu-id="e5da0-179">For more information, see [MDN web docs: Content-Security-Policy-Report-Only](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy-Report-Only).</span></span>

<span data-ttu-id="e5da0-180">如需原則作用時的違規報告，請參閱下列文章：</span><span class="sxs-lookup"><span data-stu-id="e5da0-180">For reporting on violations while a policy is active, see the following articles:</span></span>

* [<span data-ttu-id="e5da0-181">報告至</span><span class="sxs-lookup"><span data-stu-id="e5da0-181">report-to</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-to)
* [<span data-ttu-id="e5da0-182">報告-uri</span><span class="sxs-lookup"><span data-stu-id="e5da0-182">report-uri</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy/report-uri)

<span data-ttu-id="e5da0-183">雖然`report-uri`不再建議使用，但必須使用這兩個指示詞， `report-to`直到所有主要瀏覽器都支援為止。</span><span class="sxs-lookup"><span data-stu-id="e5da0-183">Although `report-uri` is no longer recommended for use, both directives should be used until `report-to` is supported by all of the major browsers.</span></span> <span data-ttu-id="e5da0-184">請勿獨佔使用`report-uri` `report-uri` ，因為的支援可能會*隨時從流覽*器中卸載。</span><span class="sxs-lookup"><span data-stu-id="e5da0-184">Don't exclusively use `report-uri` because support for `report-uri` is subject to being dropped *at any time* from browsers.</span></span> <span data-ttu-id="e5da0-185">當受到完整`report-uri`支援時`report-to` ，請移除原則中的支援。</span><span class="sxs-lookup"><span data-stu-id="e5da0-185">Remove support for `report-uri` in your policies when `report-to` is fully supported.</span></span> <span data-ttu-id="e5da0-186">若要追蹤的`report-to`採用，請參閱[我可以使用：報告至](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to)。</span><span class="sxs-lookup"><span data-stu-id="e5da0-186">To track adoption of `report-to`, see [Can I use: report-to](https://caniuse.com/#feat=mdn-http_headers_csp_content-security-policy_report-to).</span></span>

<span data-ttu-id="e5da0-187">測試和更新每個版本的應用程式原則。</span><span class="sxs-lookup"><span data-stu-id="e5da0-187">Test and update an app's policy every release.</span></span>

## <a name="troubleshoot"></a><span data-ttu-id="e5da0-188">疑難排解</span><span class="sxs-lookup"><span data-stu-id="e5da0-188">Troubleshoot</span></span>

* <span data-ttu-id="e5da0-189">錯誤會出現在瀏覽器的開發人員工具主控台中。</span><span class="sxs-lookup"><span data-stu-id="e5da0-189">Errors appear in the browser's developer tools console.</span></span> <span data-ttu-id="e5da0-190">瀏覽器會提供下列資訊：</span><span class="sxs-lookup"><span data-stu-id="e5da0-190">Browsers provide information about:</span></span>
  * <span data-ttu-id="e5da0-191">不符合原則的元素。</span><span class="sxs-lookup"><span data-stu-id="e5da0-191">Elements that don't comply with the policy.</span></span>
  * <span data-ttu-id="e5da0-192">如何修改原則以允許封鎖的專案。</span><span class="sxs-lookup"><span data-stu-id="e5da0-192">How to modify the policy to allow for a blocked item.</span></span>
* <span data-ttu-id="e5da0-193">只有當用戶端的瀏覽器支援所有包含的指示詞時，原則才會完全有效。</span><span class="sxs-lookup"><span data-stu-id="e5da0-193">A policy is only completely effective when the client's browser supports all of the included directives.</span></span> <span data-ttu-id="e5da0-194">如需目前的瀏覽器支援矩陣，請參閱[我可以使用：內容-安全性-原則](https://caniuse.com/#search=Content-Security-Policy)。</span><span class="sxs-lookup"><span data-stu-id="e5da0-194">For a current browser support matrix, see [Can I use: Content-Security-Policy](https://caniuse.com/#search=Content-Security-Policy).</span></span>

## <a name="additional-resources"></a><span data-ttu-id="e5da0-195">其他資源</span><span class="sxs-lookup"><span data-stu-id="e5da0-195">Additional resources</span></span>

* [<span data-ttu-id="e5da0-196">MDN web 檔：內容-安全性-原則</span><span class="sxs-lookup"><span data-stu-id="e5da0-196">MDN web docs: Content-Security-Policy</span></span>](https://developer.mozilla.org/docs/Web/HTTP/Headers/Content-Security-Policy)
* [<span data-ttu-id="e5da0-197">內容安全性原則層級2</span><span class="sxs-lookup"><span data-stu-id="e5da0-197">Content Security Policy Level 2</span></span>](https://www.w3.org/TR/CSP2/)
* [<span data-ttu-id="e5da0-198">Google CSP 評估工具</span><span class="sxs-lookup"><span data-stu-id="e5da0-198">Google CSP Evaluator</span></span>](https://csp-evaluator.withgoogle.com/)
