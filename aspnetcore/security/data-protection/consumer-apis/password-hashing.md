---
title: "密碼雜湊"
author: rick-anderson
description: "本文件說明如何使用 ASP.NET Core 資料保護 Api 的密碼雜湊。"
manager: wpickett
ms.author: riande
ms.date: 10/14/2016
ms.prod: asp.net-core
ms.technology: aspnet
ms.topic: article
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 536499687865b4e4d5b1d9c4076623b5ac1ffdbe
ms.sourcegitcommit: a510f38930abc84c4b302029d019a34dfe76823b
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 01/30/2018
---
# <a name="password-hashing"></a>密碼雜湊

資料保護程式碼基底包含封裝*Microsoft.AspNetCore.Cryptography.KeyDerivation*包含密碼編譯金鑰衍生函式。 此套件是獨立的元件且沒有任何相依性的資料保護系統的其餘部分。 它可以完全獨立使用。 其中存在著來源與資料保護程式碼基底，為了方便起見。

封裝目前提供的方法，`KeyDerivation.Pbkdf2`讓雜湊密碼使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)。 這個 API 已非常類似現有的.NET Framework [Rfc2898DeriveBytes 類型](https://docs.microsoft.com/dotnet/api/system.security.cryptography.rfc2898derivebytes)，但是有三個重要的區別：

1. `KeyDerivation.Pbkdf2`方法支援使用多個 PRFs (目前`HMACSHA1`， `HMACSHA256`，和`HMACSHA512`)，而`Rfc2898DeriveBytes`類型只支援`HMACSHA1`。

2. `KeyDerivation.Pbkdf2`方法偵測到目前的作業系統，並嘗試選擇最最佳化的常式，並提供較佳的效能，在某些情況下實作。 (在 Windows 8 上，它提供了大約 10 倍的輸送量`Rfc2898DeriveBytes`。)

3. `KeyDerivation.Pbkdf2`方法要求呼叫端指定所有參數 (salt，PRF 和反覆項目計數)。 `Rfc2898DeriveBytes`針對這些類型提供的預設值。

[!code-csharp[Main](password-hashing/samples/passwordhasher.cs)]

ASP.NET Core 身分識別的請參閱原始程式碼`PasswordHasher`真實世界的類型使用的大小寫。
