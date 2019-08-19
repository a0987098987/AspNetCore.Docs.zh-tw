---
title: ASP.NET Core 中的雜湊密碼
author: rick-anderson
description: 瞭解如何使用 ASP.NET Core 的資料保護 Api 來雜湊密碼。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: bd4b8fcf6a5a16a86ada97bbd3519f872d1417b7
ms.sourcegitcommit: 0efb9e219fef481dee35f7b763165e488aa6cf9c
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 07/29/2019
ms.locfileid: "68602448"
---
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core 中的雜湊密碼

資料保護程式碼基底包含 AspNetCore 的套件*KeyDerivation* , 其中包含密碼編譯金鑰衍生功能。 此套件是獨立元件, 不會相依于資料保護系統的其餘部分。 您可以完全獨立地使用它。 來源會與資料保護程式碼基底並存, 以方便使用。

封裝目前提供的方法`KeyDerivation.Pbkdf2`可讓您使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)來對密碼進行雜湊處理。 此 API 與 .NET Framework 的現有[Rfc2898DeriveBytes 類型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)非常類似, 但有三個重要的差異:

1. `HMACSHA512` `HMACSHA256` `HMACSHA1` `Rfc2898DeriveBytes` `HMACSHA1`方法支援使用多個 PRFs (目前為、和), 而類型僅支援。 `KeyDerivation.Pbkdf2`

2. `KeyDerivation.Pbkdf2`方法會偵測目前的作業系統, 並嘗試選擇最優化的常式執行, 在某些情況下提供更好的效能。 (在 Windows 8 上, 它提供的輸送量`Rfc2898DeriveBytes`大約10倍)。

3. `KeyDerivation.Pbkdf2`方法會要求呼叫者指定所有參數 (salt、PRF 和反復專案計數)。 `Rfc2898DeriveBytes`型別會提供這些的預設值。

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

如需實際使用案例, 請參閱 ASP.NET Core 身分識別`PasswordHasher`類型的[原始程式碼](https://github.com/aspnet/AspNetCore/blob/master/src/Identity/Extensions.Core/src/PasswordHasher.cs)。
