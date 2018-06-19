---
title: ASP.NET Core 的影像標籤協助程式
author: pkellner
description: 示範如何使用影像標籤協助程式
manager: wpickett
ms.author: riande
ms.date: 02/14/2017
ms.prod: aspnet-core
ms.technology: aspnet
ms.topic: article
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 6aa9175f873c4ea62e0319c812e5312cd3331141
ms.sourcegitcommit: 48beecfe749ddac52bc79aa3eb246a2dcdaa1862
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/22/2018
ms.locfileid: "30072258"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="4bb0b-103">ASP.NET Core 的影像標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="4bb0b-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="4bb0b-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="4bb0b-104">By [Peter Kellner](http://peterkellner.net)</span></span> 

<span data-ttu-id="4bb0b-105">影像標籤協助程式強化了 `img` (`<img>`) 標籤。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-105">The Image Tag Helper enhances the `img` (`<img>`) tag.</span></span> <span data-ttu-id="4bb0b-106">它需要使用 `src` 標籤以及 `boolean` 屬性 `asp-append-version`。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-106">It requires a `src` tag as well as the `boolean` attribute `asp-append-version`.</span></span>

<span data-ttu-id="4bb0b-107">如果影像來源 (`src`) 是主網頁伺服器上的靜態檔案，則會以查詢參數形式，將唯一的快取破壞 (cache busting) 字串附加至影像來源。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-107">If the image source (`src`) is a static file on the host web server, a unique cache busting string is appended as a query parameter to the image source.</span></span> <span data-ttu-id="4bb0b-108">這樣即可確保，當主網頁 伺服器上的檔案變更時，會產生唯一的要求 URL，以包含更新的要求參數。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-108">This ensures that if the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span> <span data-ttu-id="4bb0b-109">快取破壞 (cache busting) 字串是代表靜態影像檔案的雜湊的唯一值。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-109">The cache busting string is a unique value representing the hash of the static image file.</span></span>

<span data-ttu-id="4bb0b-110">如果影像來源 (`src`) 不是靜態的檔案 (例如遠端 URL 或不存在於伺服器的檔案)，產生的 `<img>` 標籤 `src` 屬性即不含快取破壞 (cache busting) 查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-110">If the image source (`src`) isn't a static file (for example a remote URL or the file doesn't exist on the server), the `<img>` tag's `src` attribute is generated with no cache busting query string parameter.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="4bb0b-111">影像標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="4bb0b-111">Image Tag Helper Attributes</span></span>


### <a name="asp-append-version"></a><span data-ttu-id="4bb0b-112">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="4bb0b-112">asp-append-version</span></span>

<span data-ttu-id="4bb0b-113">使用 `src` 屬性加以指定時，即會叫用影像標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-113">When specified along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="4bb0b-114">有效的 `img` 標籤協助程式範例為：</span><span class="sxs-lookup"><span data-stu-id="4bb0b-114">An example of a valid `img` tag helper is:</span></span>

```cshtml
<img src="~/images/asplogo.png" 
    asp-append-version="true"  />
```

<span data-ttu-id="4bb0b-115">如果靜態檔案存在於 *...wwwroot/images/asplogo.png* 目錄中，產生的 HTML 類似如下 (雜湊則不同)：</span><span class="sxs-lookup"><span data-stu-id="4bb0b-115">If the static file exists in the directory *..wwwroot/images/asplogo.png* the generated html is similar to the following (the hash will be different):</span></span>

```html
<img 
    src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM"/>
```

<span data-ttu-id="4bb0b-116">指派給 `v` 參數的值是磁碟上檔案的雜湊值。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-116">The value assigned to the parameter `v` is the hash value of the file on disk.</span></span> <span data-ttu-id="4bb0b-117">如果網頁伺服器無法取得所參考的靜態檔案讀取權限，就不會將 `v` 參數新增至 `src` 屬性。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-117">If the web server is unable to obtain read access to the static file referenced,  no `v` parameters is added to the `src` attribute.</span></span>

- - -

### <a name="src"></a><span data-ttu-id="4bb0b-118">src</span><span class="sxs-lookup"><span data-stu-id="4bb0b-118">src</span></span>

<span data-ttu-id="4bb0b-119">若要啟用影像標籤協助程式，`<img>` 項目上需要 src 屬性。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-119">To activate the Image Tag Helper, the src attribute is required on the `<img>` element.</span></span> 

> [!NOTE]
> <span data-ttu-id="4bb0b-120">影像標籤協助程式會使用本機網頁伺服器上的 `Cache` 提供者，來儲存指定檔案計算出的 `Sha512`。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-120">The Image Tag Helper uses the `Cache` provider on the local web server to store the calculated `Sha512` of a given file.</span></span> <span data-ttu-id="4bb0b-121">如果再次要求檔案，即不需要重新計算 `Sha512`。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-121">If the file is requested again the `Sha512` doesn't need to be recalculated.</span></span> <span data-ttu-id="4bb0b-122">當檔案的 `Sha512` 計算得出時，附加至檔案的檔案監看員會讓快取失效。</span><span class="sxs-lookup"><span data-stu-id="4bb0b-122">The Cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` is calculated.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="4bb0b-123">其他資源</span><span class="sxs-lookup"><span data-stu-id="4bb0b-123">Additional resources</span></span>

* <xref:performance/caching/memory>
