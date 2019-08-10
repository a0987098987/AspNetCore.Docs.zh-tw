---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor 的支援平臺。
monikerRange: '>= aspnetcore-3.0'
ms.author: riande
ms.custom: mvc
ms.date: 07/01/2019
uid: blazor/supported-platforms
ms.openlocfilehash: 01f3a55a8536feedf713e07ea3724a0bc51e7c63
ms.sourcegitcommit: 7a40c56bf6a6aaa63a7ee83a2cac9b3a1d77555e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/12/2019
ms.locfileid: "68948248"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor 支援的平臺

作者：[Luke Latham](https://github.com/guardrex)

## <a name="browser-requirements"></a>瀏覽器需求

### <a name="blazor-client-side"></a>Blazor 用戶端

| 瀏覽器                          | 版本               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | 目前               |
| Mozilla Firefox                  | 目前               |
| Google Chrome, 包括 Android | 目前               |
| Safari, 包括 iOS            | 目前               |
| Microsoft Internet Explorer      | 不受支援&dagger; |

&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。

### <a name="blazor-server-side"></a>Blazor 伺服器端

| 瀏覽器                          | 版本    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | 目前    |
| Mozilla Firefox                  | 目前    |
| Google Chrome, 包括 Android | 目前    |
| Safari, 包括 iOS            | 目前    |
| Microsoft Internet Explorer      | 英寸&dagger; |

&dagger;需要額外的 polyfills (例如, 可透過[Polyfill.io](https://polyfill.io/v3/)配套新增承諾)。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/hosting-models>
