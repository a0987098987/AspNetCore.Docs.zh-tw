---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: '[How Do i:] 快取 ASP.NET 網頁會根據 HTTP 標頭中的資訊 |Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何根據頁面的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。 可能的 HTTP 頁首的第一個...
ms.author: riande
ms.date: 02/26/2009
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: c90a3db1357df062909ad0e3b73fdeeb3dc16329
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41834233"
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="60f85-104">[How Do i:] 快取 ASP.NET 網頁會根據 HTTP 標頭中的資訊</span><span class="sxs-lookup"><span data-stu-id="60f85-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="60f85-105">藉由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="60f85-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="60f85-106">在此影片的 Chris Pels 示範如何根據頁面的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。</span><span class="sxs-lookup"><span data-stu-id="60f85-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="60f85-107">首先，檢閱可能的 HTTP 標頭值。</span><span class="sxs-lookup"><span data-stu-id="60f85-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="60f85-108">然後，建立範例頁面，然後 OutputCache 指示詞搭配 VaryByHeader 屬性包含值 「 接受語言 」，HTTP 標頭，來控制快取根據使用者的瀏覽器的語言。</span><span class="sxs-lookup"><span data-stu-id="60f85-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="60f85-109">在 IE 中是設定為英文，然後它會設定為使用法文的 FireFox 中檢視範例網頁。</span><span class="sxs-lookup"><span data-stu-id="60f85-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="60f85-110">最後，將會討論將快取的定義移至 CacheProfile web.config 檔案中的選項。</span><span class="sxs-lookup"><span data-stu-id="60f85-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="60f85-111">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="60f85-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
