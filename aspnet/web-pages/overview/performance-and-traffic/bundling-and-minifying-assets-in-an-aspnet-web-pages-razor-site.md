---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: 統合及縮小資產的 ASP.NET Web Pages (Razor) 站台 |Microsoft 文件
author: microsoft
description: 組合和縮製是可讓您的站台更快的方法。 結合在一起可讓您結合多個 JavaScript (.js) 檔案或多個階層式樣式表 （...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
ms.prod: .net-framework
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: df00b5c9e741e429011143d04121df97c1930602
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528547"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="d6c1b-104">統合及縮小 ASP.NET Web Pages (Razor) 網站中的資產</span><span class="sxs-lookup"><span data-stu-id="d6c1b-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="d6c1b-105">由[Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="d6c1b-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="d6c1b-106">組合和縮製是可讓您的站台更快的方法。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="d6c1b-107">結合在一起，可讓您結合多個 JavaScript (*.js*) 檔案或多個階層式樣式表 (*.css*) 檔案，以便他們可以下載為一個單位，而非一次。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="d6c1b-108">縮製 squeezes 出的空白字元，並執行其他類型的壓縮，請下載的檔案較小的可能。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="d6c1b-109">ASP.NET Web Pages 2 的 RC 版本不支援統合及縮製，因為封裝包含必要的項目尚無法使用 Microsoft WebMatrix 中。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="d6c1b-110">造成的不便，敬請見諒。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="d6c1b-111">封裝必須是最後一個版本的 ASP.NET Web Pages 2 與 WebMatrix 2 中可用。</span><span class="sxs-lookup"><span data-stu-id="d6c1b-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
