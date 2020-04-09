---
title: ASP.NET核心Blazor支援的平臺
author: guardrex
description: 瞭解受支援的平台,用於ASP.NET核心Blazor。
monikerRange: '>= aspnetcore-3.1'
ms.author: riande
ms.custom: mvc
ms.date: 12/18/2019
no-loc:
- Blazor
- SignalR
uid: blazor/supported-platforms
ms.openlocfilehash: 505974280b5c96ec2bcae42c6e076ab67a15bb07
ms.sourcegitcommit: f7886fd2e219db9d7ce27b16c0dc5901e658d64e
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/06/2020
ms.locfileid: "78658855"
---
# <a name="aspnet-core-blazor-supported-platforms"></a>ASP.NET核心布拉佐爾支援的平臺

作者：[Luke Latham](https://github.com/guardrex)

[!INCLUDE[](~/includes/blazorwasm-preview-notice.md)]

## <a name="browser-requirements"></a>瀏覽器需求

### <a name="blazor-webassembly"></a>Blazor WebAssembly

| 瀏覽器                          | 版本               |
| -------------------------------- | :-------------------: |
| Microsoft Edge                   | 目前               |
| Mozilla Firefox                  | 目前               |
| 谷歌瀏覽器,包括安卓 | 目前               |
| 野生動物園,包括 iOS            | 目前               |
| Microsoft Internet Explorer      | 不支援&dagger; |

&dagger;微軟網際網路瀏覽器不支援[網路組裝](https://webassembly.org)。

### <a name="blazor-server"></a>Blazor 伺服器

| 瀏覽器                          | 版本    |
| -------------------------------- | :--------: |
| Microsoft Edge                   | 目前    |
| Mozilla Firefox                  | 目前    |
| 谷歌瀏覽器,包括安卓 | 目前    |
| 野生動物園,包括 iOS            | 目前    |
| Microsoft Internet Explorer      | 11&dagger; |

&dagger;需要額外的多填充(例如,可以通過[Polyfill.io](https://polyfill.io/v3/)捆綁包添加承諾)。

## <a name="additional-resources"></a>其他資源

* <xref:blazor/hosting-models>
