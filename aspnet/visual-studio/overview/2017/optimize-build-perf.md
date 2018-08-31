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
# <a name="optimize-build-performance-for-solution"></a>將方案的建置效能最佳化

Visual Studio 2017 15.8年或更新版本包含功能表項目：**建置** > **ASP.NET 編譯** > **最佳化的方案建置的效能**。

![新的功能表項目的螢幕擷取畫面](optimize-build-perf/_static/optimize-build-performance-for-solution.png)

ASP.NET 會編譯它在執行階段，這表示，ASP.NET 專案會有一份編譯器的檢視。 但是開發人員電腦上時，編譯器的複本不符合 Visual Studio 的複本，組建會影響效能大約 1-3 秒累加建置。 這項功能會更新以符合 Visual Studio 的編譯器，通常會加快累加建置的專案的複本。

**這是適用於 ASP.NET Framework 4.7.1 或更新版本僅限專案，它不會套用至 ASP.NET Core。**
