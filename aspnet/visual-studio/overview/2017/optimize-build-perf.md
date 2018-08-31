---
uid: visual-studio/overview/2017/optimize-build-perf
title: 將方案的建置效能最佳化
author: AngelosP
description: 將方案的建置效能最佳化
ms.author: riande
ms.date: 08/29/2018
msc.type: authoredcontent
ms.openlocfilehash: c1a5cf5e59374b4c0dd7150c5dd62fbde42af555
ms.sourcegitcommit: ecf2cd4e0613569025b28e12de3baa21d86d4258
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/30/2018
ms.locfileid: "43312137"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="32998-103">將方案的建置效能最佳化</span><span class="sxs-lookup"><span data-stu-id="32998-103">Optimize build performance for solution</span></span>

<span data-ttu-id="32998-104">Visual Studio 2017 15.8年或更新版本包含功能表項目：**建置** > **ASP.NET 編譯** > **最佳化的方案建置的效能**。</span><span class="sxs-lookup"><span data-stu-id="32998-104">Visual Studio 2017 15.8 or later include a menu item: **Build** > **ASP.NET Compilation** > **Optimize Build Performance for Solution**.</span></span>

![新的功能表項目的螢幕擷取畫面](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="32998-106">ASP.NET 會編譯它在執行階段，這表示，ASP.NET 專案會有一份編譯器的檢視。</span><span class="sxs-lookup"><span data-stu-id="32998-106">ASP.NET compiles its views at runtime, which means that an ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="32998-107">但是開發人員電腦上時，編譯器的複本不符合 Visual Studio 的複本，組建會影響效能大約 1-3 秒累加建置。</span><span class="sxs-lookup"><span data-stu-id="32998-107">However on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="32998-108">這項功能會更新以符合 Visual Studio 的編譯器，通常會加快累加建置的專案的複本。</span><span class="sxs-lookup"><span data-stu-id="32998-108">This feature updates your project's copy of the compiler to match Visual Studio's, which usually speeds up incremental builds.</span></span>

<span data-ttu-id="32998-109">**這是適用於 ASP.NET Framework 4.7.1 或更新版本僅限專案，它不會套用至 ASP.NET Core。**</span><span class="sxs-lookup"><span data-stu-id="32998-109">**This is applicable to ASP.NET Framework 4.7.1 or later projects only, it does not apply to ASP.NET Core.**</span></span>
