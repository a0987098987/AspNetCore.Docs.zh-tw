---
uid: visual-studio/overview/2017/optimize-build-perf
title: 將方案的建置效能最佳化
author: tfitzmac
description: 將方案的建置效能最佳化
ms.author: riande
ms.date: 08/22/2018
msc.type: authoredcontent
ms.openlocfilehash: 19f190835e7477e69db470b74edac9e211fd9158
ms.sourcegitcommit: 5a2456cbf429069dc48aaa2823cde14100e4c438
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 08/22/2018
ms.locfileid: "41909981"
---
# <a name="optimize-build-performance-for-solution"></a><span data-ttu-id="ed9f1-103">將方案的建置效能最佳化</span><span class="sxs-lookup"><span data-stu-id="ed9f1-103">Optimize build performance for solution</span></span>
<span data-ttu-id="ed9f1-104">Visual Studio 2017 15.8年和稍後加入新的功能表項目底下**建置 > ASP.NET 編譯 > 最佳化的方案建置的效能**。</span><span class="sxs-lookup"><span data-stu-id="ed9f1-104">Visual Studio 2017 15.8 and later added a new menu item under **Build > ASP.NET Compilation > Optimize Build Performance for Solution**.</span></span>

![新的功能表項目的螢幕擷取畫面](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

<span data-ttu-id="ed9f1-106">ASP.NET 會編譯它在執行階段，這表示您的 ASP.NET 專案會有一份編譯器的檢視。</span><span class="sxs-lookup"><span data-stu-id="ed9f1-106">ASP.NET compiles its views at runtime, which means your ASP.NET project carries with it a copy of the compiler.</span></span> <span data-ttu-id="ed9f1-107">不過，開發人員電腦上時，編譯器的複本不符合 Visual Studio 的複本，建置效能會受到影響大約 1-3 秒累加建置。</span><span class="sxs-lookup"><span data-stu-id="ed9f1-107">However, on a developer machine when the copy of the compiler doesn't match Visual Studio's copy, your build performance is impacted on the order of 1-3 seconds per incremental build.</span></span> <span data-ttu-id="ed9f1-108">這項功能會更新以符合 Visual Studio 的累加建置應該加速編譯器專案的複本。</span><span class="sxs-lookup"><span data-stu-id="ed9f1-108">This feature will update your project's copy of the compiler to match Visual Studio's which should speed up incremental builds.</span></span>

<span data-ttu-id="ed9f1-109">這是適用於僅限 ASP.NET Framework 的專案，它不會套用至 ASP.NET Core。</span><span class="sxs-lookup"><span data-stu-id="ed9f1-109">This is applicable to ASP.NET Framework projects only, it does not apply to ASP.NET Core.</span></span>
