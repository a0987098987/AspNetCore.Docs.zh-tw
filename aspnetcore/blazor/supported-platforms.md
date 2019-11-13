---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor的支援平臺。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 10/15/2019
no-loc:
- Blazor
uid: blazor/supported-platforms
ms.openlocfilehash: de51296cc8785474e1c1406cfd5d4e5bd4050172
ms.sourcegitcommit: 3fc3020961e1289ee5bf5f3c365ce8304d8ebf19
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 11/12/2019
ms.locfileid: "73962735"
---
# <a name="aspnet-core-opno-locblazor-supported-platforms"></a>ASP.NET Core Blazor 支援的平臺

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>瀏覽器需求

### <a name="opno-locblazor-webassembly"></a>Blazor WebAssembly

| 瀏覽器                          | 版本               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | 目前               |
| Mozilla Firefox                  | 目前               |
| Google Chrome，包括 Android | 目前               |
| Safari，包括 iOS            | 目前               |
| Microsoft Internet Explorer      | 不支援&dagger; |

&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。

### <a name="opno-locblazor-server"></a>Blazor 伺服器

| 瀏覽器                          | 版本    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | 目前    |
| Mozilla Firefox                  | 目前    |
| Google Chrome，包括 Android | 目前    |
| Safari，包括 iOS            | 目前    |
| Microsoft Internet Explorer      | 11&dagger; |

需要 &dagger;其他 polyfills （例如，可透過[Polyfill.io](https://polyfill.io/v3/)配套新增承諾）。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/hosting-models>
