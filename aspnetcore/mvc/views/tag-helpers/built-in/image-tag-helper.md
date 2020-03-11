---
title: ASP.NET Core 的影像標籤協助程式
author: pkellner
description: 示範如何使用影像標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 964072ad276f7e3e411ee41cb03a2efb9d05c585
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78663993"
---
# <a name="image-tag-helper-in-aspnet-core"></a>ASP.NET Core 的影像標籤協助程式

由 [Peter Kellner](https://peterkellner.net) 提供

影像標籤協助程式會強化 `<img>` 標記，為靜態影像檔案提供快取破壞行為。

快取破壞 (cache busting) 字串是唯一值，代表附加至資產的 URL 靜態影像檔案雜湊。 唯一字串會提示用戶端 (和某些 Proxy) 從主機 Web 伺服器重新載入影像，而不是從用戶端的快取重新載入。

如果影像來源 (`src`) 是主機 Web 伺服器上的靜態檔案：

* 唯一的快取破壞字串會作為查詢參數附加至影像來源。
* 如果主機 Web 伺服器上的檔案變更，則會產生唯一的要求 URL，以包含更新的要求參數。

如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。

## <a name="image-tag-helper-attributes"></a>影像標籤協助程式屬性

### <a name="src"></a>src

若要啟用影像標籤協助程式，`src` 項目需要 `<img>` 元素。

影像來源 (`src`) 必須指向伺服器上的實體靜態檔案。 如果 `src` 是遠端 URI，將不會產生快取破壞查詢字串參數。

### <a name="asp-append-version"></a>asp-append-version

使用 `asp-append-version` 值與 `true` 屬性指定 `src` 時，即會叫用影像標籤協助程式。

下列範例使用影像標籤協助程式：

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

如果靜態檔案存在於 */wwwroot/images/* 目錄中，產生的 HTML 類似如下 (雜湊會不同)：

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

指派給 `v` 參數的值是磁碟上 *asplogo.png* 檔案的雜湊值。 如果網頁伺服器無法取得靜態檔案讀取權限，就不會在轉譯標記中將 `v` 參數新增至 `src` 屬性。

## <a name="hash-caching-behavior"></a>雜湊快取行為

影像標籤協助程式會使用本機網頁伺服器上的快取提供者，來儲存指定檔案計算出的 `Sha512` 雜湊。 如果多次要求檔案，則不會重新計算雜湊。 當檔案的 `Sha512` 雜湊計算得出時，附加至檔案的檔案監看員會讓快取失效。 當磁碟上的檔案變更時，會計算並快取新的雜湊。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
