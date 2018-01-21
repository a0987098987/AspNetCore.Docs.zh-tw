---
title: "影像標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用影像標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 438c5afb96dce6d8978d26159a3b460614111988
ms.sourcegitcommit: 3e303620a125325bb9abd4b2d315c106fb8c47fd
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/19/2018
---
# <a name="imagetaghelper"></a>ImageTagHelper

由 [Peter Kellner](http://peterkellner.net) 提供 

影像標記協助程式增強`img`(`<img>`) 標記。 它需要`src`標記以及`boolean`屬性`asp-append-version`。

如果影像來源 (`src`) 是靜態檔案主機 web 伺服器上唯一快取 busting 字串會附加當做映像來源的查詢參數。 這可確保，如果主機 web 伺服器上的檔案變更時，唯一的要求 URL 會產生包含已更新的要求參數。 快取 busting 字串是代表靜態影像檔案的雜湊的唯一值。

如果影像來源 (`src`) 不是靜態的檔案 （例如遠端 URL 或檔案伺服器不存在），`<img>`標記之`src`busting 查詢字串參數無快取的產生屬性。

## <a name="image-tag-helper-attributes"></a>映像標記協助程式屬性


### <a name="asp-append-version"></a>asp-append-version

當指定連同`src`影像標記協助程式的屬性，會叫用。

有效範例`img`標記協助程式：

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

如果靜態檔案存在於目錄*...wwwroot/images/asplogo.png*產生的 html 是類似下列 （雜湊會不同）：

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

指派給參數的值`v`是磁碟上檔案的雜湊值。 如果 web 伺服器無法取得讀取權限靜態參考的檔案，沒有`v`將參數加入到`src`屬性。

- - -

### <a name="src"></a>src

若要啟用影像標記協助程式，需要的 src 屬性`<img>`項目。 

> [!NOTE]
> 使用影像標記協助程式`Cache`本機 web 伺服器上的提供者儲存導出`Sha512`的指定的檔案。 如果檔案一次要求`Sha512`不需要重新計算。 快取都無效的檔案監看員，附加至檔案時檔案的`Sha512`計算。

## <a name="additional-resources"></a>其他資源

* <xref:performance/caching/memory>
