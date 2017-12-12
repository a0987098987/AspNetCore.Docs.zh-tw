---
uid: web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
title: "[如何:] ASP.NET 網頁根據 HTTP 標頭中資訊的快取 |Microsoft 文件"
author: rick-anderson
description: "在這個視訊 Chris Pels 示範如何根據網頁的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。 第一個可能的 HTTP hea..."
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/26/2009
ms.topic: article
ms.assetid: 0f8df1bd-080a-4eeb-980c-c2fbb05d30c2
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header
msc.type: video
ms.openlocfilehash: fcb4b5e3b60bbcf3a0e5a95ff294bf41c7553a9d
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
---
<a name="how-do-i--cache-an-aspnet-page-based-upon-information-in-the-http-header"></a><span data-ttu-id="4d712-104">[如何:] 快取的 ASP.NET 網頁根據 HTTP 標頭中的資訊</span><span class="sxs-lookup"><span data-stu-id="4d712-104">[How Do I:]  Cache an ASP.NET Page Based Upon Information in the HTTP Header</span></span>
====================
<span data-ttu-id="4d712-105">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="4d712-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="4d712-106">在這個視訊 Chris Pels 示範如何根據網頁的 HTTP 標頭中的資訊在 ASP.NET 輸出快取中保留的頁面。</span><span class="sxs-lookup"><span data-stu-id="4d712-106">In this video Chris Pels shows how to keep a page in the ASP.NET output cache based upon information in the page's HTTP header.</span></span> <span data-ttu-id="4d712-107">首先，檢閱可能的 HTTP 標頭值。</span><span class="sxs-lookup"><span data-stu-id="4d712-107">First, the potential HTTP header values are reviewed.</span></span> <span data-ttu-id="4d712-108">然後會建立範例頁面在 OutputCache 指示詞再搭配 VaryByHeader 屬性包含值 「 接受語言 」，HTTP 標頭，來控制快取使用者的瀏覽器的語言為基礎。</span><span class="sxs-lookup"><span data-stu-id="4d712-108">Then, a sample page is created and then the OutputCache directive is used with the VaryByHeader attribute which contains a value "accept-language", an HTTP header, to control caching based upon the language of the user's browser.</span></span> <span data-ttu-id="4d712-109">Ie 是設定為英文，然後它會設定為使用法文 FireFox 中檢視範例網頁。</span><span class="sxs-lookup"><span data-stu-id="4d712-109">The sample page is viewed in IE which is set to English and then in FireFox which is set to use French.</span></span> <span data-ttu-id="4d712-110">最後，會討論將快取定義移至 CacheProfile web.config 檔案中的選項。</span><span class="sxs-lookup"><span data-stu-id="4d712-110">Finally, the option to move the cache definition to a CacheProfile in the web.config file is discussed.</span></span>

[<span data-ttu-id="4d712-111">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="4d712-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-cache-an-aspnet-page-based-upon-information-in-the-http-header)
