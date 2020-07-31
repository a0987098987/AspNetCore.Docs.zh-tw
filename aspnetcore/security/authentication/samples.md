---
title: ASP.NET Core 的驗證範例
author: rick-anderson
description: 提供 ASP.NET Core 存放庫中驗證範例的連結。
ms.author: riande
ms.date: 01/31/2019
no-loc:
- Blazor
- Blazor Server
- Blazor WebAssembly
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/authentication/samples
ms.openlocfilehash: 3e5e487adafc09d38400ea58936c5c2e8385e84f
ms.sourcegitcommit: 5a36758cca2861aeb10840093e46d273a6e6e91d
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/28/2020
ms.locfileid: "87303595"
---
# <a name="authentication-samples-for-aspnet-core"></a>ASP.NET Core 的驗證範例

作者：[Rick Anderson](https://twitter.com/RickAndMSFT)

::: moniker range=">= aspnetcore-3.0"

[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：

* [宣告轉換](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Cookies)
* [自訂原則提供者-IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/CustomPolicyProvider)
* [動態驗證配置和選項](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/DynamicSchemes)
* [外部宣告](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 cookie 和另一個驗證配置之間進行選取](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/PathSchemeSelection)
* [限制靜態檔案的存取](https://github.com/dotnet/AspNetCore/tree/release/3.1/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選取[分支](https://github.com/dotnet/AspNetCore)。 例如， `release/3.1`
* 複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。
* 確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。
* 流覽至*AspNetCore/src/Security/samples*中的範例，並使用來執行範例 `dotnet run` 。

::: moniker-end

::: moniker range="< aspnetcore-3.0"

[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)包含下列*AspNetCore/src/Security/samples*資料夾中的驗證範例：

* [宣告轉換](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/ClaimsTransformation)
* [Cookie 驗證](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Cookies)
* [自訂原則提供者-IAuthorizationPolicyProvider](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/CustomPolicyProvider)
* [動態驗證配置和選項](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/DynamicSchemes)
* [外部宣告](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/Identity.ExternalClaims)
* [根據要求在 cookie 和另一個驗證配置之間進行選取](https://github.com/dotnet/AspNetCore/tree/release/2.1/src/Security/samples/PathSchemeSelection)
* [限制靜態檔案的存取](https://github.com/dotnet/AspNetCore/tree/2.1.3/src/Security/samples/StaticFilesAuth)

## <a name="run-the-samples"></a>執行範例

* 選取[分支](https://github.com/dotnet/AspNetCore)。 例如， `release/2.1`
* 複製或下載[ASP.NET Core 存放庫](https://github.com/dotnet/AspNetCore)。
* 確認您已安裝符合 ASP.NET Core 存放庫複製的[.NET Core SDK](https://dotnet.microsoft.com/download/dotnet-core)版本。
* 流覽至*AspNetCore/src/Security/samples*中的範例，並使用來執行範例 `dotnet run` 。

::: moniker-end
