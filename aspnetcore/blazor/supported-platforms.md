---
title: ASP.NET Core Blazor 支援的平臺
author: guardrex
description: 瞭解 ASP.NET Core Blazor的支援平臺。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: 9a129f5f3e31cc449742b164d5004894bfca90aa
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 03/06/2020
ms.locfileid: "78658855"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET Core Blazor 支援的平臺

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>瀏覽器需求

### <a name="blazor-webassembly"></a>Blazor WebAssembly

| 瀏覽器                          | 版本               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | 目前               |
| Mozilla Firefox                  | 目前               |
| Google Chrome，包括 Android | 目前               |
| Safari，包括 iOS            | 目前               |
| Microsoft Internet Explorer      | 不支援&dagger; |

&dagger;Microsoft Internet Explorer 不支援[WebAssembly](https://webassembly.org)。

### <a name="blazor-server"></a>Blazor 伺服器

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
