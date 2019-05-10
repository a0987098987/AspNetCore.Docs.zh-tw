---
title: ASP.NET Core 的影像標籤協助程式
author: pkellner
description: 示範如何使用影像標籤協助程式。
ms.author: riande
ms.custom: mvc
ms.date: 04/06/2019
uid: mvc/views/tag-helpers/builtin-th/image-tag-helper
ms.openlocfilehash: 916a68c187cbf516a59d3c5d7578cdb6ada01b86
ms.sourcegitcommit: 5b0eca8c21550f95de3bb21096bd4fd4d9098026
ms.translationtype: HT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/27/2019
ms.locfileid: "64887663"
---
# <a name="image-tag-helper-in-aspnet-core"></a><span data-ttu-id="31321-103">ASP.NET Core 的影像標籤協助程式</span><span class="sxs-lookup"><span data-stu-id="31321-103">Image Tag Helper in ASP.NET Core</span></span>

<span data-ttu-id="31321-104">由 [Peter Kellner](http://peterkellner.net) 提供</span><span class="sxs-lookup"><span data-stu-id="31321-104">By [Peter Kellner](http://peterkellner.net)</span></span>

<span data-ttu-id="31321-105">影像標籤協助程式會強化 `<img>` 標記，為靜態影像檔案提供快取破壞行為。</span><span class="sxs-lookup"><span data-stu-id="31321-105">The Image Tag Helper enhances the `<img>` tag to provide cache-busting behavior for static image files.</span></span>

<span data-ttu-id="31321-106">快取破壞 (cache busting) 字串是唯一值，代表附加至資產的 URL 靜態影像檔案雜湊。</span><span class="sxs-lookup"><span data-stu-id="31321-106">A cache-busting string is a unique value representing the hash of the static image file appended to the asset's URL.</span></span> <span data-ttu-id="31321-107">唯一字串會提示用戶端 (和某些 Proxy) 從主機 Web 伺服器重新載入影像，而不是從用戶端的快取重新載入。</span><span class="sxs-lookup"><span data-stu-id="31321-107">The unique string prompts clients (and some proxies) to reload the image from the host web server and not from the client's cache.</span></span>

<span data-ttu-id="31321-108">如果影像來源 (`src`) 是主機 Web 伺服器上的靜態檔案：</span><span class="sxs-lookup"><span data-stu-id="31321-108">If the image source (`src`) is a static file on the host web server:</span></span>

* <span data-ttu-id="31321-109">唯一的快取破壞字串會作為查詢參數附加至影像來源。</span><span class="sxs-lookup"><span data-stu-id="31321-109">A unique cache-busting string is appended as a query parameter to the image source.</span></span>
* <span data-ttu-id="31321-110">如果主機 Web 伺服器上的檔案變更，則會產生唯一的要求 URL，以包含更新的要求參數。</span><span class="sxs-lookup"><span data-stu-id="31321-110">If the file on the host web server changes, a unique request URL is generated that includes the updated request parameter.</span></span>

<span data-ttu-id="31321-111">如需標籤協助程式的概觀，請參閱 <xref:mvc/views/tag-helpers/intro>。</span><span class="sxs-lookup"><span data-stu-id="31321-111">For an overview of Tag Helpers, see <xref:mvc/views/tag-helpers/intro>.</span></span>

## <a name="image-tag-helper-attributes"></a><span data-ttu-id="31321-112">影像標籤協助程式屬性</span><span class="sxs-lookup"><span data-stu-id="31321-112">Image Tag Helper Attributes</span></span>

### <a name="src"></a><span data-ttu-id="31321-113">src</span><span class="sxs-lookup"><span data-stu-id="31321-113">src</span></span>

<span data-ttu-id="31321-114">若要啟用影像標籤協助程式，`src` 項目需要 `<img>` 元素。</span><span class="sxs-lookup"><span data-stu-id="31321-114">To activate the Image Tag Helper, the `src` attribute is required on the `<img>` element.</span></span>

<span data-ttu-id="31321-115">影像來源 (`src`) 必須指向伺服器上的實體靜態檔案。</span><span class="sxs-lookup"><span data-stu-id="31321-115">The image source (`src`) must point to a physical static file on the server.</span></span> <span data-ttu-id="31321-116">如果 `src` 是遠端 URI，將不會產生快取破壞查詢字串參數。</span><span class="sxs-lookup"><span data-stu-id="31321-116">If the `src` is a remote URI, the cache-busting query string parameter isn't generated.</span></span>

### <a name="asp-append-version"></a><span data-ttu-id="31321-117">asp-append-version</span><span class="sxs-lookup"><span data-stu-id="31321-117">asp-append-version</span></span>

<span data-ttu-id="31321-118">使用 `true` 值與 `src` 屬性指定 `asp-append-version` 時，即會叫用影像標籤協助程式。</span><span class="sxs-lookup"><span data-stu-id="31321-118">When `asp-append-version` is specified with a `true` value along with a `src` attribute, the Image Tag Helper is invoked.</span></span>

<span data-ttu-id="31321-119">下列範例使用影像標籤協助程式：</span><span class="sxs-lookup"><span data-stu-id="31321-119">The following example uses an Image Tag Helper:</span></span>

```cshtml
<img src="~/images/asplogo.png" asp-append-version="true">
```

<span data-ttu-id="31321-120">如果靜態檔案存在於 */wwwroot/images/* 目錄中，產生的 HTML 類似如下 (雜湊會不同)：</span><span class="sxs-lookup"><span data-stu-id="31321-120">If the static file exists in the directory */wwwroot/images/*, the generated HTML is similar to the following (the hash will be different):</span></span>

```html
<img src="/images/asplogo.png?v=Kl_dqr9NVtnMdsM2MUg4qthUnWZm5T1fCEimBPWDNgM">
```

<span data-ttu-id="31321-121">指派給 `v` 參數的值是磁碟上 *asplogo.png* 檔案的雜湊值。</span><span class="sxs-lookup"><span data-stu-id="31321-121">The value assigned to the parameter `v` is the hash value of the *asplogo.png* file on disk.</span></span> <span data-ttu-id="31321-122">如果網頁伺服器無法取得靜態檔案讀取權限，就不會在轉譯標記中將 `v` 參數新增至 `src` 屬性。</span><span class="sxs-lookup"><span data-stu-id="31321-122">If the web server is unable to obtain read access to the static file, no `v` parameter is added to the `src` attribute in the rendered markup.</span></span>

## <a name="hash-caching-behavior"></a><span data-ttu-id="31321-123">雜湊快取行為</span><span class="sxs-lookup"><span data-stu-id="31321-123">Hash caching behavior</span></span>

<span data-ttu-id="31321-124">影像標籤協助程式會使用本機網頁伺服器上的快取提供者，來儲存指定檔案計算出的 `Sha512` 雜湊。</span><span class="sxs-lookup"><span data-stu-id="31321-124">The Image Tag Helper uses the cache provider on the local web server to store the calculated `Sha512` hash of a given file.</span></span> <span data-ttu-id="31321-125">如果多次要求檔案，則不會重新計算雜湊。</span><span class="sxs-lookup"><span data-stu-id="31321-125">If the file is requested multiple times, the hash isn't recalculated.</span></span> <span data-ttu-id="31321-126">當檔案的 `Sha512` 雜湊計算得出時，附加至檔案的檔案監看員會讓快取失效。</span><span class="sxs-lookup"><span data-stu-id="31321-126">The cache is invalidated by a file watcher that's attached to the file when the file's `Sha512` hash is calculated.</span></span> <span data-ttu-id="31321-127">當磁碟上的檔案變更時，會計算並快取新的雜湊。</span><span class="sxs-lookup"><span data-stu-id="31321-127">When the file changes on disk, a new hash is calculated and cached.</span></span>

## <a name="additional-resources"></a><span data-ttu-id="31321-128">其他資源</span><span class="sxs-lookup"><span data-stu-id="31321-128">Additional resources</span></span>

* <xref:performance/caching/memory>
