---
uid: web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
title: '[如何:]ASP.NET 網頁的快取為基礎的自訂資訊的控制 |Microsoft 文件'
author: rick-anderson
description: 在這個視訊 Chris Pels 示範如何控制快取 ASP.NET 網頁的自訂資訊為基礎的準則。 建立範例頁面並再 O...
ms.author: aspnetcontent
manager: wpickett
ms.date: 02/19/2009
ms.topic: article
ms.assetid: f230c316-1313-4b8f-967c-62f9684fe378
ms.technology: dotnet-webforms
ms.prod: .net-framework
msc.legacyurl: /web-forms/videos/how-do-i/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information
msc.type: video
ms.openlocfilehash: b785de4e1161ae82ee458148aee4b30820801147
ms.sourcegitcommit: 9a9483aceb34591c97451997036a9120c3fe2baf
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/10/2017
ms.locfileid: "26528127"
---
<a name="how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information"></a><span data-ttu-id="efa31-104">[如何:]ASP.NET 網頁的快取為基礎的自訂資訊的控制項</span><span class="sxs-lookup"><span data-stu-id="efa31-104">[How Do I:] Control the Caching of an ASP.NET Page Based Upon Custom Information</span></span>
====================
<span data-ttu-id="efa31-105">由[Chris Pels](https://twitter.com/chrispels)</span><span class="sxs-lookup"><span data-stu-id="efa31-105">by [Chris Pels](https://twitter.com/chrispels)</span></span>

<span data-ttu-id="efa31-106">在這個視訊 Chris Pels 示範如何控制快取 ASP.NET 網頁的自訂資訊為基礎的準則。</span><span class="sxs-lookup"><span data-stu-id="efa31-106">In this video Chris Pels shows how to control the criteria for caching an ASP.NET page based upon custom information.</span></span> <span data-ttu-id="efa31-107">建立範例頁面，然後 OutputCache 指示詞搭配 VaryByCustom 屬性，其中包含自訂值。</span><span class="sxs-lookup"><span data-stu-id="efa31-107">A sample page is created and then the OutputCache directive is used with the VaryByCustom attribute which contains a custom value.</span></span> <span data-ttu-id="efa31-108">接下來，GetVaryCustomByString() 方法會覆寫在 global.asax 模組可提供自訂屬性的處理。</span><span class="sxs-lookup"><span data-stu-id="efa31-108">Next, the GetVaryCustomByString() method is overridden in the global.asax module which provides the handling of the custom attribute.</span></span> <span data-ttu-id="efa31-109">該方法會傳回字串，可唯一識別頁面的快取的版本。</span><span class="sxs-lookup"><span data-stu-id="efa31-109">In that method a string is returned that uniquely identifies the cached version of the page.</span></span> <span data-ttu-id="efa31-110">最後，是有關如何快取使用的自訂值可以使用數種方式的網站。</span><span class="sxs-lookup"><span data-stu-id="efa31-110">Finally, there is a discussion about how caching using a custom value can be used in several ways for a web site.</span></span>

[<span data-ttu-id="efa31-111">&#9654;觀看影片 （12 分鐘）</span><span class="sxs-lookup"><span data-stu-id="efa31-111">&#9654; Watch video (12 minutes)</span></span>](https://channel9.msdn.com/Blogs/ASP-NET-Site-Videos/how-do-i-control-the-caching-of-an-aspnet-page-based-upon-custom-information)
