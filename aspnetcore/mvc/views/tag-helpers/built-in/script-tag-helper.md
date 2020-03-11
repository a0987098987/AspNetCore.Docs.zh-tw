---
title: ASP.NET Core 中的腳本標記協助程式
author: rick-anderson
ms.author: riande
description: 探索 ASP.NET Core 腳本標記協助程式屬性，以及每個屬性在 HTML 腳本標記的擴充行為中所扮演的角色。
ms.custom: mvc
ms.date: 12/02/2019
uid: mvc/views/tag-helpers/builtin-th/script-tag-helper
ms.openlocfilehash: a037abb6a454e6d06305e7d7f6ecad0c2a0ca717
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78659835"
---
# <a name="script-tag-helper-in-aspnet-core"></a>ASP.NET Core 中的腳本標記協助程式

由 [Rick Anderson](https://twitter.com/RickAndMSFT) 提供

[腳本標記](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper)協助程式會產生主要或切換回腳本檔案的連結。 主要腳本檔案通常位於[內容傳遞網路](/office365/enterprise/content-delivery-networks#what-exactly-is-a-cdn)（CDN）上。

[!INCLUDE[](~/includes/cdn.md)]

當 CDN 無法使用時，腳本標籤協助程式可讓您指定腳本檔案的 CDN 和回退。 腳本標記協助程式可提供 CDN 的效能優勢，以及本機裝載的穩定性。

下列 Razor 標記顯示具有 fallback 的 `script` 元素：

```html
<script src="https://ajax.aspnetcdn.com/ajax/jquery/jquery-3.3.1.min.js"
        asp-fallback-src="~/lib/jquery/dist/jquery.min.js"
        asp-fallback-test="window.jQuery"
        crossorigin="anonymous"
        integrity="sha384-tsQFqpEReu7ZLhBV2VZlAu7zcOV+rXbYlF2cqB8txI/8aZajjp4Bqd+V6D5IgvKT">
</script>
```

請勿使用 `<script>` 元素的[defer](https://developer.mozilla.org/docs/Web/HTML/Element/script)屬性來延遲載入 CDN 腳本。 腳本標記協助程式會轉譯 JavaScript，以立即執行[asp-fallback-測試](#asp-fallback-test)運算式。 如果載入 CDN 腳本已延遲，運算式就會失敗。

## <a name="commonly-used-script-tag-helper-attributes"></a>常用的腳本標記協助程式屬性

如需所有腳本標記協助程式屬性、屬性和方法，請參閱[腳本標記 helper](xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper) 。

### <a name="asp-fallback-test"></a>asp-回溯-測試

要用於回溯測試之主要腳本中定義的腳本方法。 如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackTestExpression>。

### <a name="asp-fallback-src"></a>asp-fallback-src

當主要複本失敗時，要回復之腳本標記的 URL。 如需詳細資訊，請參閱 <xref:Microsoft.AspNetCore.Mvc.TagHelpers.ScriptTagHelper.FallbackSrc>。

## <a name="additional-resources"></a>其他資源

* <xref:mvc/views/tag-helpers/intro>
* <xref:mvc/controllers/areas>
* <xref:razor-pages/index>
* <xref:mvc/compatibility-version>
