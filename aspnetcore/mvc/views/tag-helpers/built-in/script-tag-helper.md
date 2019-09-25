---
title: ASP.NET Core 中的腳本標記協助程式
author: rick-anderson
ms.author: riande
description: 探索 ASP.NET Core 腳本標記協助程式屬性，以及每個屬性在 HTML 腳本標記的擴充行為中所扮演的角色。
ms.custom: mvc
ms.date: 12/18/2018
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: 5f2fb8a45048804afa8aff2989cd53489e45a33b
ms.sourcegitcommit: fae6f0e253f9d62d8f39de5884d2ba2b4b2a6050
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/25/2019
ms.locfileid: "71256495"
---
# <a name="script-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的腳本標記協助程式

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[腳本標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper)協助程式會產生主要或切換回腳本檔案的連結。 主要腳本檔案通常位於[內容傳遞網路](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn)（CDN）上。

[!INCLUDE[](~/includes/cdn.md)]

當 CDN 無法使用時，腳本標籤協助程式可讓您指定腳本檔案的 CDN 和回退。 腳本標記協助程式可提供 CDN 的效能優勢，以及本機裝載的穩定性。

下列 Razor 標記會顯示使用`script` ASP.NET Core web 應用程式範本所建立之配置檔案的元素：

[!code-html[](link-tag-helper/sample/_Layout.cshtml?name=snippet2)]

下列類似于上述程式碼所呈現的 HTML （在非開發環境中）：

[!code-csharp[](link-tag-helper/sample/HtmlPage2.html)]

在上述程式碼中，腳本標記協助程式`<script>  (window.jQuery || document.write(` `window.jQuery`產生了第二個 script （）專案，它會測試。 如果`window.jQuery`找不到， `document.write(`則會執行並建立腳本 

## <a name="commonly-used-script-tag-helper-attributes"></a>常用的腳本標記協助程式屬性

如需所有腳本標記協助程式屬性、屬性和方法，請參閱[腳本標記 helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) 。

### <a name="href"></a>href

連結資源的慣用位址。 在所有情況下，會將位址視為產生的 HTML。

### <a name="asp-fallback-href"></a>asp-fallback-href

當主要 URL 失敗時，要回復的 CSS 樣式表單 URL。

### <a name="asp-fallback-test-class"></a>asp-fallback-測試類別

在樣式表單中定義用於回溯測試的類別名稱。 如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestClass>。

### <a name="asp-fallback-test-property"></a>asp-fallback-測試-屬性

用於回退測試的 CSS 屬性名稱。 如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestProperty>。

### <a name="asp-fallback-test-value"></a>asp-fallback-測試-值

要用於 fallback 測試的 CSS 屬性值。 如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>。

### <a name="asp-fallback-test-value"></a>asp-fallback-測試-值

要用於 fallback 測試的 CSS 屬性值。 如需詳細資訊，請參閱<xref:Microsoft.AspNetCore.Mvc.TagHelpers.LinkTagHelper.FallbackTestValue>。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
