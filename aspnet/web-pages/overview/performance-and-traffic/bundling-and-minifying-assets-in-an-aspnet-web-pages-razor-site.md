---
uid: web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
title: 統合及縮小資產在 ASP.NET Web Pages (Razor) 網站 |Microsoft Docs
author: microsoft
description: 統合和縮製是讓您的網站更快的方式。 統合可讓您結合多個 JavaScript (.js) 檔案或多個階層式樣式表 （...
ms.author: aspnetcontent
manager: wpickett
ms.date: 06/21/2012
ms.topic: article
ms.assetid: 8906f1e9-4b66-4a03-8e8a-9e9debf8ed91
ms.technology: dotnet-webpages
msc.legacyurl: /web-pages/overview/performance-and-traffic/bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site
msc.type: authoredcontent
ms.openlocfilehash: dd1b184846a7ac9c08df0212a69b154279791d1a
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37393793"
---
<a name="bundling-and-minifying-assets-in-an-aspnet-web-pages-razor-site"></a><span data-ttu-id="43ee7-104">統合及縮小 ASP.NET Web Pages (Razor) 網站中的資產</span><span class="sxs-lookup"><span data-stu-id="43ee7-104">Bundling and Minifying Assets in an ASP.NET Web Pages (Razor) Site</span></span>
====================
<span data-ttu-id="43ee7-105">by [Microsoft](https://github.com/microsoft)</span><span class="sxs-lookup"><span data-stu-id="43ee7-105">by [Microsoft](https://github.com/microsoft)</span></span>

> <span data-ttu-id="43ee7-106">統合和縮製是讓您的網站更快的方式。</span><span class="sxs-lookup"><span data-stu-id="43ee7-106">Bundling and minification are ways to make your site faster.</span></span> <span data-ttu-id="43ee7-107">統合可讓您結合多個 JavaScript (*.js*) 檔案或多個階層式樣式表 (*.css*) 檔案，讓他們可以下載為一個單位，而非一次。</span><span class="sxs-lookup"><span data-stu-id="43ee7-107">Bundling lets you combine multiple JavaScript (*.js*) files or multiple cascading style sheet (*.css*) files so that they can be downloaded as a unit, rather than one at a time.</span></span> <span data-ttu-id="43ee7-108">縮製壓榨出的空白字元，並執行其他類型的壓縮功能以進行下載的檔案較小的可能。</span><span class="sxs-lookup"><span data-stu-id="43ee7-108">Minification squeezes out whitespace and performs other types of compression to make the downloaded files as small a possible.</span></span>
> 
> > [!NOTE]
> > <span data-ttu-id="43ee7-109">ASP.NET Web Pages 2 RC 版本不支援統合和縮製，因為封裝包含必要的項目尚無法使用 Microsoft WebMatrix 中。</span><span class="sxs-lookup"><span data-stu-id="43ee7-109">The RC release of ASP.NET Web Pages 2 does not support bundling and minification because the package that contains the required elements is not yet available in Microsoft WebMatrix.</span></span> <span data-ttu-id="43ee7-110">此造成的不便，敬請見諒。</span><span class="sxs-lookup"><span data-stu-id="43ee7-110">We apologize for this inconvenience.</span></span> <span data-ttu-id="43ee7-111">封裝應該可在 ASP.NET Web Pages 2 和 WebMatrix 2 的最終發行版本。</span><span class="sxs-lookup"><span data-stu-id="43ee7-111">The package is expected to be available in the final release of ASP.NET Web Pages 2 and WebMatrix 2.</span></span>
