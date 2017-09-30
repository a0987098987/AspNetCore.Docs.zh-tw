---
title: "影像標記協助程式 |Microsoft 文件"
author: pkellner
description: "示範如何使用影像標記協助程式"
keywords: "ASP.NET Core,標記協助程式"
ms.author: riande
manager: wpickett
ms.date: 02/14/2017
ms.topic: article
ms.assetid: c045d485-d1dc-4cea-a675-46be83b7a013
ms.technology: aspnet
ms.prod: aspnet-core
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 0d55514508b963ce05031f89a20af696f5d4a670
ms.sourcegitcommit: 6e83c55eb0450a3073ef2b95fa5f5bcb20dbbf89
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/28/2017
---
# <a name="imagetaghelper"></a><span data-ttu-id="27a10-104">ImageTagHelper</span><span class="sxs-lookup"><span data-stu-id="27a10-104">ImageTagHelper</span></span>

<span data-ttu-id="27a10-105">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="27a10-105">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="27a10-106">影像標記協助程式增強`img`(`<img>`) 標記。</span><span class="sxs-lookup"><span data-stu-id="27a10-106">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="27a10-107">它需要`src`標記以及`boolean`屬性`asp-append-version`。</span><span class="sxs-lookup"><span data-stu-id="27a10-107">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="27a10-108">如果影像來源 (`src`) 是靜態檔案主機 web 伺服器上唯一快取 busting 字串會附加當做映像來源的查詢參數。</span><span class="sxs-lookup"><span data-stu-id="27a10-108">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="27a10-109">這可確保，如果主機 web 伺服器上的檔案變更時，唯一的要求 URL 會產生包含已更新的要求參數。</span><span class="sxs-lookup"><span data-stu-id="27a10-109">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="27a10-110">快取 busting 字串是代表靜態影像檔案的雜湊的唯一值。</span><span class="sxs-lookup"><span data-stu-id="27a10-110">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="27a10-111">如果影像來源 (`src`) 不是靜態的檔案 （例如遠端 URL 或檔案伺服器不存在），`<img>`標記之`src`busting 查詢字串參數無快取的產生屬性。</span><span class="sxs-lookup"><span data-stu-id="27a10-111">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="27a10-112">映像標記協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="27a10-112">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="27a10-113">asp 附加版本</span><span class="sxs-lookup"><span data-stu-id="27a10-113">asp-append-version</span></span>

<span data-ttu-id="27a10-114">當指定連同`src`影像標記協助程式的屬性，會叫用。</span><span class="sxs-lookup"><span data-stu-id="27a10-114">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="27a10-115">有效範例`img`標記協助程式：</span><span class="sxs-lookup"><span data-stu-id="27a10-115">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="27a10-116">如果靜態檔案存在於目錄*...wwwroot/images/asplogo.png*產生的 html 是類似下列 （雜湊會不同）：</span><span class="sxs-lookup"><span data-stu-id="27a10-116">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="27a10-117">指派給參數的值`v`是磁碟上檔案的雜湊值。</span><span class="sxs-lookup"><span data-stu-id="27a10-117">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="27a10-118">如果 web 伺服器無法取得讀取權限靜態參考的檔案，沒有`v`將參數加入到`src`屬性。</span><span class="sxs-lookup"><span data-stu-id="27a10-118">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="27a10-119">src</span><span class="sxs-lookup"><span data-stu-id="27a10-119">src</span></span>

<span data-ttu-id="27a10-120">若要啟用影像標記協助程式，需要的 src 屬性`<img>`項目。</span><span class="sxs-lookup"><span data-stu-id="27a10-120">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="27a10-121">使用影像標記協助程式`Cache`本機 web 伺服器上的提供者儲存導出`Sha512`的指定的檔案。</span><span class="sxs-lookup"><span data-stu-id="27a10-121">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="27a10-122">如果檔案一次要求`Sha512`不需要重新計算。</span><span class="sxs-lookup"><span data-stu-id="27a10-122">If the file is requested again the `Sha512` does not need to be recalculated.</span></span> <span data-ttu-id="27a10-123">快取都無效的檔案監看員，附加至檔案時檔案的`Sha512`計算。</span><span class="sxs-lookup"><span data-stu-id="27a10-123">The Cache is invalidated by a file watcher that is attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="27a10-124">其他資源</span><span class="sxs-lookup"><span data-stu-id="27a10-124">Additional resources</span></span>

* <xref:performance/caching/memory>
