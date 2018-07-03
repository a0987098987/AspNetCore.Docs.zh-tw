---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[How Do i:]ASP.NET 網頁的快取根據自訂資訊的控制項 |Microsoft Docs'
author: rick-anderson
description: 在此影片的 Chris Pels 示範如何控制快取 ASP.NET 網頁，根據自訂資訊的準則。 建立範例頁面並再 O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: d2c8e2384d39255f66c11f1cc303398750229779
ms.sourcegitcommit: 953ff9ea4369f154d6fd0239599279ddd3280009
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/03/2018
ms.locfileid: "37376016"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="2e238-104">[How Do i:]ASP.NET 網頁的快取根據自訂資訊的控制項</span><span class="sxs-lookup"><span data-stu-id="2e238-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="2e238-105">藉由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="2e238-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="2e238-106">在此影片的 Chris Pels 示範如何控制快取 ASP.NET 網頁，根據自訂資訊的準則。</span><span class="sxs-lookup"><span data-stu-id="2e238-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="2e238-107">建立範例頁面，然後將 OutputCache 指示詞搭配 VaryByCustom 屬性，其中包含自訂的值。</span><span class="sxs-lookup"><span data-stu-id="2e238-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="2e238-108">接下來，GetVaryCustomByString() 方法會覆寫在 global.asax 模組可讓您自訂屬性的處理。</span><span class="sxs-lookup"><span data-stu-id="2e238-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="2e238-109">在該方法會傳回字串可唯一識別網頁的快取的版本。</span><span class="sxs-lookup"><span data-stu-id="2e238-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="2e238-110">最後，還有如何快取 使用自訂的值可以用於數種方式的網站進行的討論。</span><span class="sxs-lookup"><span data-stu-id="2e238-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="2e238-111">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="2e238-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
