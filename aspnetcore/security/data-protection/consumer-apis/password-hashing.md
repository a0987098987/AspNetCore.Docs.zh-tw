---
title: ASP.NET Core 中的雜湊密碼
author: rick-anderson
description: 了解如何使用 ASP.NET Core 資料保護 Api 的密碼的雜湊。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 882ac9b256b0cdf5fd19dc4bd2757cac7e8ecad3
ms.sourcegitcommit: a742b55e4b8276a48b8b4394784554fecd883c84
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/13/2018
ms.locfileid: "45538371"
---
# <a name="hash-passwords-in-aspnet-core"></a>ASP.NET Core 中的雜湊密碼

資料保護程式碼基底包含封裝*Microsoft.AspNetCore.Cryptography.KeyDerivation*其中包含密碼編譯金鑰衍生函式。 此套件是獨立的元件，且沒有任何相依性資料保護系統的其餘部分。 它可以完全獨立使用。 來源存在與資料保護程式碼基底，為了方便起見。

封裝目前提供一種方法`KeyDerivation.Pbkdf2`可讓雜湊密碼，使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)。 此 API 是非常類似於現有的.NET Framework [Rfc2898DeriveBytes 類型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)，但是有三個重要的區別：

1. `KeyDerivation.Pbkdf2`方法可讓您支援使用多個 PRFs (目前`HMACSHA1`， `HMACSHA256`，以及`HMACSHA512`)，而`Rfc2898DeriveBytes`類型只支援`HMACSHA1`。

2. `KeyDerivation.Pbkdf2`方法會偵測目前的作業系統，並嘗試選擇常式中，提供更好的效能，在某些情況下的最佳化的實作。 (在 Windows 8 中，它提供約 10 倍的輸送量`Rfc2898DeriveBytes`。)

3. `KeyDerivation.Pbkdf2`方法需要呼叫端指定所有參數 (salt，PRF 和反覆項目計數)。 `Rfc2898DeriveBytes`針對這些類型提供的預設值。

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

請參閱 [原始程式碼] (https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) ASP.NET Core 身分識別的`PasswordHasher`真實世界的型別使用案例。
