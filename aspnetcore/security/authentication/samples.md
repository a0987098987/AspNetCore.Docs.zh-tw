---
title: ASP.NET核心的身份驗證範例
author: rick-anderson
description: 提供指向ASP.NET核心存儲庫中的身份驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
uid: security/authentication/samples
ms.openlocfilehash: b33eaa5c1bda9e23b2815cd1663e02ae06fec856
ms.sourcegitcommit: 5af16166977da598953f82da3ed3b7712d38f6cb
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 04/14/2020
ms.locfileid: "81308211"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET核心的身份驗證範例

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[ASP.NET核心儲存函式庫](https://github.com/dotnet/AspNetCore)在*AspNetCore/src/安全/範例*資料夾中包含以下身份驗證範例:

* [宣告轉換](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)
* [自訂政策提供者 - 授權原則提供者](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [動態認證方案和選項](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* [外部索賠](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 Cookie 和另一個身份驗證方案之間進行選擇](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [限制對靜態檔案的存取](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選擇[分支](https://github.com/dotnet/AspNetCore)。 例如， `release/3.1`
* KSP.[NET 核心儲存函式庫](https://github.com/dotnet/AspNetCore)。
* 驗證已安裝與ASP.NET核心儲存庫的克隆相匹配的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。
* 導航到*AspNetCore/src/Security/範例*中的範例,`dotnet run`然後使用 執行範例。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[ASP.NET核心儲存函式庫](https://github.com/dotnet/AspNetCore)在*AspNetCore/src/安全/範例*資料夾中包含以下身份驗證範例:

* [宣告轉換](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Cookies)
* [自訂政策提供者 - 授權原則提供者](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/CustomPolicyProvider)
* [動態認證方案和選項](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/DynamicSchemes)
* [外部索賠](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 Cookie 和另一個身份驗證方案之間進行選擇](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/PathSchemeSelection)
* [限制對靜態檔案的存取](https://github.com/dotnet/AspNetCore/tree/release/2.2/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選擇[分支](https://github.com/dotnet/AspNetCore)。 例如， `release/2.2`
* KSP.[NET 核心儲存函式庫](https://github.com/dotnet/AspNetCore)。
* 驗證已安裝與ASP.NET核心儲存庫的克隆相匹配的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。
* 導航到*AspNetCore/src/Security/範例*中的範例,`dotnet run`然後使用 執行範例。

::: moniker-end
