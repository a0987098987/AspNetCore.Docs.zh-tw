---
title: ASP.NET Core 中的雜湊密碼
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來雜湊密碼。
ms.author: riande
ms.date: 10/14/2016
no-loc:
- Blazor
- Identity
- Let's Encrypt
- Razor
- SignalR
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 6a5e0e4378241671905f2a759aad88372901e7d2
ms.sourcegitcommit: 70e5f982c218db82aa54aa8b8d96b377cfc7283f
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 05/04/2020
ms.locfileid: "82769776"
---
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core 中的雜湊密碼

資料保護程式碼基底包含 AspNetCore 的套件*KeyDerivation* ，其中包含密碼編譯金鑰衍生功能。 此套件是獨立元件，不會相依于資料保護系統的其餘部分。 您可以完全獨立地使用它。 來源會與資料保護程式碼基底並存，以方便使用。

封裝目前提供的方法`KeyDerivation.Pbkdf2`可讓您使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)來對密碼進行雜湊處理。 此 API 與 .NET Framework 的現有[Rfc2898DeriveBytes 類型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)非常類似，但有三個重要的差異：

1. `KeyDerivation.Pbkdf2`方法支援使用多個 PRFs （目前`HMACSHA1`為`HMACSHA256`、和`HMACSHA512`），而`Rfc2898DeriveBytes`類型僅支援`HMACSHA1`。

2. `KeyDerivation.Pbkdf2`方法會偵測目前的作業系統，並嘗試選擇最優化的常式執行，在某些情況下提供更好的效能。 （在 Windows 8 上，它提供的輸送量大約 10 `Rfc2898DeriveBytes`倍）。

3. `KeyDerivation.Pbkdf2`方法會要求呼叫者指定所有參數（SALT、PRF 和反復專案計數）。 `Rfc2898DeriveBytes`型別會提供這些的預設值。

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

請參閱[source code](https://github.com/dotnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs)適用于真實世界Identity使用`PasswordHasher`案例 ASP.NET Core 類型的原始程式碼。
