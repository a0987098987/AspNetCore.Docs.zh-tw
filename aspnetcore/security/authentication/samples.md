---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中的驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: 7b3c911d60ad4737ebd12ce6f7628ad624b11658
ms.sourcegitcommit: c47d7c131eebbcd8811e31edda210d64cf4b9d6b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2019
ms.locfileid: "55236670"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core 的驗證範例

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

[ASP.NET Core 儲存機制](https://github.com/aspnet/AspNetCore)包含中的下列驗證範例*AspNetCore/src/Security/samples*資料夾：

* [宣告轉型](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [自訂原則提供者-IAuthorizationPolicyProvider](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [動態驗證配置和選項](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [外部宣告](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [選取 cookie 與另一種根據要求的驗證配置](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [限制存取靜態檔案](https://github.com/aspnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選取 [分支](https://github.com/aspnet/AspNetCore)。 例如： `release/2.2` 
* 複製或下載[ASP.NET Core 存放庫](https://github.com/aspnet/AspNetCore)。
* 確認您已安裝[.NET Core SDK](https://www.microsoft.com/net/download/all)比對的 ASP.NET Core 存放庫複製的版本。
* 瀏覽至中的範例*AspNetCore/src/Security/samples*並執行範例，其中含有`dotnet run`。
