---
title: ASP.NET Core 中的雜湊密碼
author: rick-anderson
description: 了解如何使用 ASP.NET Core 資料保護 Api 的密碼的雜湊。
ms.author: riande
ms.date: 10/14/2016
uid: security/data-protection/consumer-apis/password-hashing
ms.openlocfilehash: 70301ffffbaaf3c5ff0642b19b80e40be83aa438
ms.sourcegitcommit: b2723654af4969a24545f09ebe32004cb5e84a96
ms.translationtype: MT
ms.contentlocale: zh-TW
ms.lasthandoff: 09/18/2018
ms.locfileid: "46010958"
---
# <a name="hash-passwords-in-aspnet-core"></a><span data-ttu-id="70af3-103">ASP.NET Core 中的雜湊密碼</span><span class="sxs-lookup"><span data-stu-id="70af3-103">Hash passwords in ASP.NET Core</span></span>

<span data-ttu-id="70af3-104">資料保護程式碼基底包含封裝*Microsoft.AspNetCore.Cryptography.KeyDerivation*其中包含密碼編譯金鑰衍生函式。</span><span class="sxs-lookup"><span data-stu-id="70af3-104">The data protection code base includes a package *Microsoft.AspNetCore.Cryptography.KeyDerivation* which contains cryptographic key derivation functions.</span></span> <span data-ttu-id="70af3-105">此套件是獨立的元件，且沒有任何相依性資料保護系統的其餘部分。</span><span class="sxs-lookup"><span data-stu-id="70af3-105">This package is a standalone component and has no dependencies on the rest of the data protection system.</span></span> <span data-ttu-id="70af3-106">它可以完全獨立使用。</span><span class="sxs-lookup"><span data-stu-id="70af3-106">It can be used completely independently.</span></span> <span data-ttu-id="70af3-107">來源存在與資料保護程式碼基底，為了方便起見。</span><span class="sxs-lookup"><span data-stu-id="70af3-107">The source exists alongside the data protection code base as a convenience.</span></span>

<span data-ttu-id="70af3-108">封裝目前提供一種方法`KeyDerivation.Pbkdf2`可讓雜湊密碼，使用[PBKDF2 演算法](https://tools.ietf.org/html/rfc2898#section-5.2)。</span><span class="sxs-lookup"><span data-stu-id="70af3-108">The package currently offers a method `KeyDerivation.Pbkdf2` which allows hashing a password using the [PBKDF2 algorithm](https://tools.ietf.org/html/rfc2898#section-5.2).</span></span> <span data-ttu-id="70af3-109">此 API 是非常類似於現有的.NET Framework [Rfc2898DeriveBytes 類型](/dotnet/api/system.security.cryptography.rfc2898derivebytes)，但是有三個重要的區別：</span><span class="sxs-lookup"><span data-stu-id="70af3-109">This API is very similar to the .NET Framework's existing [Rfc2898DeriveBytes type](/dotnet/api/system.security.cryptography.rfc2898derivebytes), but there are three important distinctions:</span></span>

1. <span data-ttu-id="70af3-110">`KeyDerivation.Pbkdf2`方法可讓您支援使用多個 PRFs (目前`HMACSHA1`， `HMACSHA256`，以及`HMACSHA512`)，而`Rfc2898DeriveBytes`類型只支援`HMACSHA1`。</span><span class="sxs-lookup"><span data-stu-id="70af3-110">The `KeyDerivation.Pbkdf2` method supports consuming multiple PRFs (currently `HMACSHA1`, `HMACSHA256`, and `HMACSHA512`), whereas the `Rfc2898DeriveBytes` type only supports `HMACSHA1`.</span></span>

2. <span data-ttu-id="70af3-111">`KeyDerivation.Pbkdf2`方法會偵測目前的作業系統，並嘗試選擇常式中，提供更好的效能，在某些情況下的最佳化的實作。</span><span class="sxs-lookup"><span data-stu-id="70af3-111">The `KeyDerivation.Pbkdf2` method detects the current operating system and attempts to choose the most optimized implementation of the routine, providing much better performance in certain cases.</span></span> <span data-ttu-id="70af3-112">(在 Windows 8 中，它提供約 10 倍的輸送量`Rfc2898DeriveBytes`。)</span><span class="sxs-lookup"><span data-stu-id="70af3-112">(On Windows 8, it offers around 10x the throughput of `Rfc2898DeriveBytes`.)</span></span>

3. <span data-ttu-id="70af3-113">`KeyDerivation.Pbkdf2`方法需要呼叫端指定所有參數 (salt，PRF 和反覆項目計數)。</span><span class="sxs-lookup"><span data-stu-id="70af3-113">The `KeyDerivation.Pbkdf2` method requires the caller to specify all parameters (salt, PRF, and iteration count).</span></span> <span data-ttu-id="70af3-114">`Rfc2898DeriveBytes`針對這些類型提供的預設值。</span><span class="sxs-lookup"><span data-stu-id="70af3-114">The `Rfc2898DeriveBytes` type provides default values for these.</span></span>

[!code-csharp[](password-hashing/samples/passwordhasher.cs)]

<span data-ttu-id="70af3-115">請參閱[原始程式碼](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs)ASP.NET Core 身分識別的`PasswordHasher`真實世界的型別使用案例。</span><span class="sxs-lookup"><span data-stu-id="70af3-115">See the [source code](https://github.com/aspnet/Identity/blob/master/src/Core/PasswordHasher.cs) for ASP.NET Core Identity's `PasswordHasher` type for a real-world use case.</span></span>
