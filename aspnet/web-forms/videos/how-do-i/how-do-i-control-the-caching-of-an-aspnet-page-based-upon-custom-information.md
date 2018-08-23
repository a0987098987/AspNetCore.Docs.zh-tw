---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[How Do i:]ASP.NET 網頁的快取根據自訂資訊的控制項 |Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何控制快取 ASP.NET 網頁，根據自訂資訊的準則。 建立範例頁面並再 O...
ms.author: riande
ms.date: 02/19/2009
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: a9ed2baad3460441bc57d97bf74f6de5977db0c9
ms.sourcegitcommit: 45ac74e400f9f2b7dbded66297730f6f14a4eb25
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/16/2018
ms.locfileid: "41833184"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="b0d4b-104">[How Do i:]ASP.NET 網頁的快取根據自訂資訊的控制項</span><span class="sxs-lookup"><span data-stu-id="b0d4b-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="b0d4b-105">藉由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="b0d4b-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="b0d4b-106">在此影片的 Chris Pels 示範如何控制快取 ASP.NET 網頁，根據自訂資訊的準則。</span><span class="sxs-lookup"><span data-stu-id="b0d4b-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="b0d4b-107">建立範例頁面，然後將 OutputCache 指示詞搭配 VaryByCustom 屬性，其中包含自訂的值。</span><span class="sxs-lookup"><span data-stu-id="b0d4b-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="b0d4b-108">接下來，GetVaryCustomByString() 方法會覆寫在 global.asax 模組可讓您自訂屬性的處理。</span><span class="sxs-lookup"><span data-stu-id="b0d4b-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="b0d4b-109">在該方法會傳回字串可唯一識別網頁的快取的版本。</span><span class="sxs-lookup"><span data-stu-id="b0d4b-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="b0d4b-110">最後，還有如何快取 使用自訂的值可以用於數種方式的網站進行的討論。</span><span class="sxs-lookup"><span data-stu-id="b0d4b-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="b0d4b-111">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="b0d4b-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
