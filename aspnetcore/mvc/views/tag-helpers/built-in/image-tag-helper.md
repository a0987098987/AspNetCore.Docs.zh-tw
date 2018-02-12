---
title: "ASP.NET Core 的影像標籤協助程式"
author: pkellner
description: "示範如何使用影像標籤協助程式"
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 75bddd01a95f3ae0b1ea19de0eb64ad3b9066319
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

由 [Peter Kellner](http://peterkellner.net) 提供 

影像標籤協助程式強化了 `img` (`<img>`) 標籤。 它需要使用 `src` 標籤以及 `boolean` 屬性 `asp-append-version`。

如果影像來源 (`src`) 是主網頁伺服器上的靜態檔案，則會以查詢參數形式，將唯一的快取破壞 (cache busting) 字串附加至影像來源。 這樣即可確保，當主網頁 伺服器上的檔案變更時，會產生唯一的要求 URL，以包含更新的要求參數。 快取破壞 (cache busting) 字串是代表靜態影像檔案的雜湊的唯一值。

如果影像來源 (`src`) 不是靜態的檔案 (例如遠端 URL 或不存在於伺服器的檔案)，產生的 `<img>` 標籤 `src` 屬性即不含快取破壞 (cache busting) 查詢字串參數。

## <a name="image-tag-helper-attributes"></a>影像標籤協助程式屬性


### <a name="asp-append-version"></a>asp-append-version

使用 `src` 屬性加以指定時，即會叫用影像標籤協助程式。

有效的 `img` 標籤協助程式範例為：

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

如果靜態檔案存在於 *...wwwroot/images/asplogo.png* 目錄中，產生的 HTML 類似如下 (雜湊則不同)：

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

指派給 `v` 參數的值是磁碟上檔案的雜湊值。 如果網頁伺服器無法取得所參考的靜態檔案讀取權限，就不會將 `v` 參數新增至 `src` 屬性。

- - -

### <a name="src"></a>src

若要啟用影像標籤協助程式，`<img>` 項目上需要 src 屬性。 

> [!NOTE]
> 影像標籤協助程式會使用本機網頁伺服器上的 `Cache` 提供者，來儲存指定檔案計算出的 `Sha512`。 如果再次要求檔案，即不需要重新計算 `Sha512`。 當檔案的 `Sha512` 計算得出時，附加至檔案的檔案監看員會讓快取失效。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
