---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core 支援的平臺 Blazor 。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 05/19/2020
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: adf27fa84acb3929a1639b561c728c2db29723f6
ms.sourcegitcommit: d65a027e78bf0b83727f975235a18863e685d902
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 06/26/2020
ms.locfileid: "85401931"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor 支援的平臺

作者：[Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>瀏覽器需求

### Blazor WebAssembly

| 瀏覽器                          | 版本               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | 目前               |
| Mozilla Firefox                  | 目前               |
| Google Chrome，包括 Android | 目前               |
| Safari，包括 iOS            | 目前               |
| Microsoft Internet Explorer      | 不受支援&dagger; |

&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。

### Blazor Server

| 瀏覽器                          | 版本    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | 目前    |
| Mozilla Firefox                  | 目前    |
| Google Chrome，包括 Android | 目前    |
| Safari，包括 iOS            | 目前    |
| Microsoft Internet Explorer      | 英寸&dagger; |

&dagger;需要額外的 polyfills （例如，可透過配套新增承諾 [`Polyfill.io`](https://polyfill.io/v3/) ）。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/hosting-models>
