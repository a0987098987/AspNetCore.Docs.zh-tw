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
# <a name="optimize-build-performance-for-solution"></a>將方案的建置效能最佳化
Visual Studio 2017 15.8年和稍後加入新的功能表項目底下**建置 > ASP.NET 編譯 > 最佳化的方案建置的效能**。

![新的功能表項目的螢幕擷取畫面](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET 會編譯它在執行階段，這表示您的 ASP.NET 專案會有一份編譯器的檢視。 不過，開發人員電腦上時，編譯器的複本不符合 Visual Studio 的複本，建置效能會受到影響大約 1-3 秒累加建置。 這項功能會更新以符合 Visual Studio 的累加建置應該加速編譯器專案的複本。

這是適用於僅限 ASP.NET Framework 的專案，它不會套用至 ASP.NET Core。
